---
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
---

# Network Time Synchronization

Keeping device time consistent matters for certificates, logs, and troubleshooting. Use UTC everywhere. Use local timezone settings only for display.

### System clock fundamentals

Routers, switches, and firewalls track time with an internal system clock. It starts at boot and runs continuously. It tracks time internally in Coordinated Universal Time (UTC).

The system clock can be **authoritative** or **non-authoritative**. Non-authoritative time is for display only. It should not be redistributed.

### Network timing

Modern infrastructure needs stable and consistent timing. Network timing lowers cost versus end-site timing equipment.

Agile Metro components support Precision Time Protocol (PTP). They often support Class C timing accuracy.

Use G.8275.1 when possible for highest accuracy. IOS-XR supports G.8275.1 and G.8275.2 interworking. Use Synchronous Ethernet (SyncE) to stabilize timing to the PRC.

### Clocks on Cisco IOS

Most Cisco IOS devices have two clocks:

* **Software clock** (system clock)
* **Hardware clock** (calendar / RTC)

#### Software clock (system clock)

The software clock initializes at boot from the hardware clock. It tracks seconds and microseconds since boot.

To set the system clock manually, use `clock set` in privileged EXEC mode. Set date and time in UTC. Configure timezone and DST separately.

| Command                                                                | Notes                                                                       |
| ---------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| `clock timezone CET 1 0`                                               | Sets timezone to CET (UTC+1).                                               |
| `clock summer-time CEST recurring last Sun Mar 2:00 last Sun Oct 3:00` | Recurring DST for CET. Switches to CEST (UTC+2).                            |
| `show clock [detail]`                                                  | Shows current software clock time. NTP syncs the software clock by default. |
| `show calendar`                                                        | Shows current hardware clock time.                                          |

In logs and `show clock` output:

* `(*)` means NTP is not configured.
* `(.)` means NTP was synced before, but is currently unreachable.

#### Hardware clock (calendar / RTC)

The hardware clock is backed by a rechargeable battery. It retains date and time across reboots.

It is usually updated from the software clock. This happens after the software clock syncs to an authoritative source.

Avoid setting the hardware clock if you have a reliable external time source.

| Command                                          | Notes                                                                         |
| ------------------------------------------------ | ----------------------------------------------------------------------------- |
| `clock calendar-valid` / `clock update-calendar` | Trust RTC after reload. Also update RTC when software clock is authoritative. |

### Network Time Protocol (NTP)

NTP synchronizes clocks using a distributed client/server model. It uses UDP port `123`. The local clock IP is `127.127.1.1`.

NTP uses a hierarchy of **stratums**:

* Stratum 0: reference clocks (GPS, atomic clocks)
* Stratum 1: servers directly attached to stratum 0
* Stratum 2+: clients/servers downstream

NTP time sources can be:

* Local master clock
* Internet master clocks (for example `ntp.org`)
* GPS or atomic clocks (stratum 0)

NTP can traverse multiple hops. Accuracy improves slowly. Milliseconds of accuracy can take hours or days.

Configure timezone and initial clock settings first. They are used until NTP fully synchronizes.

![Konfigurace NTP serveru v Linuxu - Martinův život Linux](<../.gitbook/assets/Unknown image (1145)>)

#### NTP modes

* **Server**: provides time to clients.
* **Client**: synchronizes to an NTP server.
* **Peer**: exchanges time with other peers.
* **Broadcast/Multicast**: push mode from a server.

Private UTC-synced master clocks are the most secure option. Internet sources are easier but less secure.

NTP does not synchronize to an unsynchronized device. It also avoids sources whose time differs significantly from others.

![](<../.gitbook/assets/Unknown image (1146)>)

#### NTP and clock configuration (IOS-XE)

Configure clock settings and NTP together. This improves timekeeping during convergence.

Reference: [https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/syst-mgmt/b-system-management/m\_bsm-time-calendar-set.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/syst-mgmt/b-system-management/m_bsm-time-calendar-set.html)

**Common IOS-XE configuration snippets**

| Config                                                                    | Notes                                                        |
| ------------------------------------------------------------------------- | ------------------------------------------------------------ |
| `ntp server vrf Mgmt-vrf 162.159.200.1 prefer`                            | Sets preferred NTP server in management VRF.                 |
| `ntp source <interface>`                                                  | Source NTP from a stable interface (often a loopback).       |
| `ntp master [stratum]`                                                    | Makes the device an NTP master. Use with caution.            |
| `ntp authenticate` + `ntp authentication-key ...` + `ntp trusted-key ...` | Enables NTP authentication. Configure on server and clients. |
| `show ntp [associations\|status]`                                         | Shows NTP state and peer associations.                       |

#### IOS-XR notes

| Config                             | Notes                            |
| ---------------------------------- | -------------------------------- |
| `clock timezone CET Europe/Prague` | Sets timezone.                   |
| `ntp server <ip>`                  | NTP sync can take a few minutes. |

### Precision Time Protocol (PTP)

NTP is typically accurate to under \~10 ms. PTP can achieve sub-microsecond accuracy. It is often measured in nanoseconds.

Use PTP when you need tight timing:

* Energy billing (peak and off-peak)
* High-frequency trading
* Industrial automation
* Audio/video synchronization

PTP runs over Ethernet and UDP. Profiles exist because industries need different behavior.

#### Profiles

* **Default profile**: standard IEEE 1588 behavior.
* **Telecom profile**: ITU-T G.8265.1, G.8275.1, G.8275.2.
* **Power profile**: IEEE C37.238 for power grids.
* **802.1AS**: AVB timing profile for audio/video over Ethernet.

#### Delays that matter

* **Propagation delay**: signal travel time in the medium.
* **Queueing delay**: buffering and congestion delay.
* **Processing delay**: parsing, lookups, and switching time.

NIC hardware timestamping improves accuracy. Software timestamping adds variable delay. Closer to the physical layer is better.

![](<../.gitbook/assets/Unknown image (1147)>)

#### Time scale: Unix epoch vs TAI vs UTC

PTP uses the Unix epoch starting at `00:00:00` on 1 January 1970. It synchronizes with International Atomic Time (TAI), not UTC.

UTC has leap seconds. TAI does not. That makes TAI more stable for precise timekeeping.

#### Message exchange: one-step vs two-step

**Hardware PTP** often uses one-step. The Sync message includes the T1 transmit timestamp.

**Software PTP** often uses two-step. The Sync omits T1. A Follow\_Up carries T1 immediately after.

The slave then exchanges Delay\_Req / Delay\_Resp to get T4.

To sync, the slave computes:

```
delay  = ((t2 - t1) + (t4 - t3)) / 2
offset = ((t2 - t1) - (t4 - t3)) / 2
```

The slave updates its clock using the computed offset. Network delay changes over time. The master keeps sending Sync messages.

![Ptp Master Slave Clock Synchronization Messages](<../.gitbook/assets/Unknown image (1148)>)

#### PTP messages

**Event messages (timestamped)**

* **Sync**: periodic timing from the master.
  * One-step: contains T1 in the Sync.
  * Two-step: Sync has no timestamp.
* **Follow\_Up**: sent in two-step mode with T1.
* **Delay\_Req**: slave requests path delay measurement.
* **Delay\_Resp**: master replies with T4.

**General messages (not timestamped)**

* **Announce**: clock quality and priority for BMCA decisions.
* **Management**: access management data (MIB).
* **Signaling**: negotiate intervals and other non-critical parameters.

#### Clock types and roles

PTP uses a master-slave hierarchy. Clocks sync by exchanging timestamped messages.

Each interface can take a **master (M)** or **slave (S)** role. A clock can be master on one interface and slave on another.

**Grandmaster clock (GMC)**

Primary source of time in the domain. It is typically locked to GPS or an atomic clock. It always acts as master on its interface(s).

**Ordinary clock (OC)**

Runs PTP on a single interface. It is usually an end device that needs synchronization.

**Boundary clock (BC)**

Runs PTP on two or more interfaces. Upstream interface is slave toward the grandmaster. Downstream interfaces are master toward other clocks.

BCs improve scale. They prevent all OCs from talking to the grandmaster directly.

![Ptp Boundary Clock Two Ordinary Clocks Vlans](<../.gitbook/assets/Unknown image (1149)>)

**Transparent clocks**

Transparent clocks forward PTP messages. They are not a time source. They typically operate within a VLAN.

They measure residence time and update the correction field.

**End-to-end transparent clock (E2E)**

Sits between the grandmaster and ordinary clock. It forwards PTP messages and accounts for residence time.

![Ptp Transparent Clock End To End Time Sync](<../.gitbook/assets/Unknown image (1150)>)

**Peer-to-peer transparent clock (P2P)**

Measures delay per link, per interface. This scales better than end-to-end delay measurement.

Peer delay messages stay on a single hop. Ordinary clocks do not send delay messages to the grandmaster.

![Ptp Transparent Clock Peer To Peer Time Sync](<../.gitbook/assets/Unknown image (1151)>)

#### Best Master Clock Algorithm (BMCA)

Clocks compare Announce messages to pick the best grandmaster. Comparison uses this order:

1. **Priority1** (0–255, manual)
2. **Class** (source type, for example GPS)
3. **Accuracy** (lower is better)
4. **Variance** (lower is better)
5. **Priority2** (0–255, manual)
6. **Identity** (unique clock identifier, often MAC-derived)

After selection, the master sends Sync at regular intervals. If a better master appears, roles switch.
