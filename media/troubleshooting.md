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

# Troubleshooting

In a typical troubleshooting process for complex problems, you will continually move between phases: gather information, analyze, eliminate possibilities, gather more information, analyze again, formulate hypotheses, test, reject, and so on.

A structured approach yields more predictable results, makes it easier to hand over work, and helps resume troubleshooting later.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### Troubleshooting approaches (overview)

* Top-down method\
  Work from the application layer down to physical (OSI Layers 7→1). If a high-layer test (for example, TCP on port 80) succeeds, you can often eliminate all underlying layers from your scope. This method is efficient when you suspect an application-layer problem. Drawback: you need access to application-layer software on the client/server.
* Bottom-up method\
  Work from physical up (OSI Layers 1→7). Verify each layer in turn (cable, link, VLAN, routes, services). Best when you can’t access endpoints. Drawback: in large networks it can be time-consuming.
* Divide-and-conquer method\
  Start in the middle (usually the network layer), run an end-to-end test (e.g., ping). If it succeeds, start bottom-up from the network layer; if it fails, start top-down from the network layer. Often faster overall.
* Follow-the-path method\
  Determine the forwarding path packets take, then validate/link-by-link to eliminate irrelevant devices/links.
* Swap components method\
  Physically swap cables, NICs, ports, or devices to see whether the fault moves with the component. Effective to isolate a single element, but assumes a single failing component and provides limited insight into root cause. Document all changes.
* Perform comparison method\
  Compare a working device/process to a non-working one (configs, versions, hardware). Can produce fast results but can lead to workarounds without root-cause understanding.
* Shoot-from-the-hip method\
  If you are experienced with the environment and issues recur in predictable ways, attempt the most likely fix first. If it fails, revert to a structured approach.

***

### Common IPv4 verification tools

* ping — basic IPv4 connectivity
* traceroute / tracert — path and hop reachability
* telnet / ssh — test TCP connectivity to a given port
* show ip arp / arp -a — ARP table, IPv4-to-MAC mapping
* show ip interface brief / ipconfig /all — interface addressing
* Other platform-specific show/debug commands as needed

***

### Troubleshooting common switch media issues

* Layer 1:
  * Damaged cables, wrong cable type, excessive cable length, EMI.
  * UTP category matters (Cat5 vs lower categories).
  * Poor cable management can stress RJ-45 connectors.
* Ethernet collisions and late collisions:
  * Late collisions should not occur on a properly designed Ethernet network; causes include wrong cabling, too many hubs, bad NICs (jabbering).
  * Use a protocol analyzer, verify cabling distances and physical layer limits.
* CRC errors:
  * Excessive CRC errors often indicate noise or cabling problems (if collisions are constant/unchanging, CRCs may be noise-related).
  * Cable testers and TDRs help find unterminated cabling or bending issues.
* Fiber-specific issues:
  * Microbend/macrobend losses, splice losses, dirty connectors, back reflections.
  * Splicing requires trained technicians (fusion splicing).
  * Clean connectors carefully (turn off lasers before inspecting). Small dust or contamination can cause large problems.
  * Ensure correct transceiver type and wavelength (SFP vs SFP+, MMF vs SMF; 850nm vs 1310nm).
  * When cleaning fiber connectors: inspect → clean → reinspect.

***

### Duplex and speed issues (ports)

* Duplex mismatch results in poor performance and late collisions.
* Speed mismatch also causes issues. IEEE 802.3ab (Gigabit) mandates autonegotiation.
* Recommended:
  * Point-to-point Ethernet links → full-duplex.
  * Autoneg on noncritical endpoints is acceptable.
  * Manually set speed/duplex for links between network devices and for critical endpoints.
  * If autoneg fails, Catalyst switches may fall back to half-duplex → possible mismatch.

Common interface states:

* administratively down, down → interface shutdown in config
* down, down → no physical link (cable missing/off)
* up, down → Layer 2 problem (mismatch, protocol)
* up, up → healthy

duplex command options:

* full
* half
* auto

Negotiation commands:

* \#negotiation auto — default (autoneg)
* \#speed nonegotiate — disables link-negotiation

Interface counters and meanings (high level):

* reliability x/255 — transmission reliability
* txload / rxload x/255 — buffer load; calculated periodically
* input/output queue size/max/drops/flushes — queue stats; output drops may indicate congestion
* runts (<64 bytes), giants, jumbo frames — frames outside expected sizes
* throttles, overruns, ignored — receiver/transmit resource issues
* collisions/late collisions — usually duplex mismatch
* CRC errors — frame check failures (cabling/noise/hardware)
* watchdog, babbles, deferred — other hardware/PHY anomalies

Useful show/test commands (examples)

* show ip interface brief
* show interface \[description | brief | status | counters errors]
* show run interface
* show interface capabilities
* show interface transceiver detail
* show ip traffic
* test cable-diagnostics tdr interface
* show cable-diagnostics tdr interface <>
* clear counters interface <>
* (config)# default interface

SFP health: check Tx/Rx power, transceiver type, and supported fiber (SMF/MMF).

***

### Troubleshooting physical connectivity issues

{% stepper %}
{% step %}
### From the host (basic host-side steps)

1. Verify the host IPv4 address and subnet mask.
2. Ping the loopback (127.0.0.1).
3. Ping the local interface IPv4 address.
4. Ping the default gateway.
5. Ping the remote server.
6. Gather ipconfig (Windows) or ifconfig/ ip addr (Linux) output to confirm interface configuration and Media State (e.g., "media disconnected").
7. In Windows, use route print to verify the default gateway is present.
{% endstep %}

{% step %}
### On the default gateway/router

1. Check interface status: show ip interface brief.
2. Confirm IPs/subnets: show running-config (or equivalent).
3. Check routing table: show ip route.
4. Inspect ARP/MAC tables: show ip arp ; show mac address-table (or equivalents).
{% endstep %}

{% step %}
### From network devices (per-hop verification)

1. Verify interface parameters and statistics (speed/duplex/errors/load/last input) using show interfaces.
2. Run ping/traceroute from source to destination; capture output and check for latency/packet loss.
3. If packet loss or latency is present, log into each hop on the path, verify next-hop reachability and route stability (look for flapping).
4. Check port/link integrity and device CPU utilization during problem windows.
5. If suspected provider/peer route issues, consider manipulating BGP neighbors (with caution) so traffic takes an alternate path.
6. Compare configuration with a similar, working edge device to spot differences.
{% endstep %}
{% endstepper %}

***

### Wireless troubleshooting (notes)

* Verify AP health and its access-switch connectivity (CDP/LLDP) and PoE status.
* Check for interfering devices on the same channel.
* Noise level / SNR target: as low as possible; SNR values around −90 to −100 dBm indicate weak signals (interpret per vendor guidance).
* Prefer 5 GHz where possible: set preferred band on client drivers if supported.

Useful commands:

* Windows:
  * netsh wlan show drivers
  * netsh wlan show all | clip

***

### Performance testing — iPerf

iPerf is a detailed speedtest tool (server/client). Resources:

* https://iperf.fr
* https://sourceforge.net/projects/iperf/files/jperf/jperf%202.0.0/
* Public servers: https://iperf.fr/iperf-servers.php#public-servers
* Guidance: https://www.sd-wan-experts.com/blog/iperf-bandwidth-testing/

***

### Troubleshooting questions (checklist)

<details>

<summary>Click to expand: Troubleshooting questions checklist</summary>

* What is the impact with full issue description?
* Source IP and MAC address: request ipconfig /all and nslookup output from the user; check proxy settings (browser or Windows network panel).
* Destination IP address or URL:
* Destination port:
* How many users are impacted?
* Since when did the issue start?
* Has this ever worked before?
* When was the last time it worked?
* Are affected users connected via wired or wireless?
* Are they connecting from home or office? (If from home, confirm whether personal devices are affected.)
* What type of device is used (laptop, phone, DaaS; corporate or personal)? If DaaS, confirm DaaS team has checked virtual desktop.
* Is the issue persistent or intermittent / only at specific times?
* Are they experiencing latency in a specific app/tool?
* Provide ping and traceroute outputs (preferably .txt) or screenshots.
* Request user to run ping/traceroute to the destination. Windows: use -t for continuous ping; -n for a count.

</details>

***

### Example checklist to request from user

* ipconfig /all (Windows) or ip addr / route (Linux)
* nslookup (DNS verification)
* ping -n 10 (or continuous -t if instructed)
* tracert/traceroute
* Screenshot of browser/proxy settings (if applicable)
* Is the user behind a proxy, VPN, or DaaS?

***

### Troubleshooting process (summary)

* Start with host verification (IPs, gateway).
* Validate local link and then upstream device interfaces.
* Use ping/traceroute to identify the failing hop.
* Inspect per-hop interface counters and CPU utilization.
* Narrow scope (device/link/config) and apply targeted fixes (speed/duplex, transceiver issues, cabling, routing).
* Re-test end-to-end after each change.
* Document all changes and be prepared to roll back if needed.

***

### Wireless / site-on-site specific

* Verify AP uplinks, CDP/LLDP, PoE, SNR, channel utilization.
* Run site/site: iPerf between an on-site test machine and a known server or another on-site host.
* For roaming issues, review controller/AP logs and client roaming thresholds.
