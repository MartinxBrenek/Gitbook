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
  actions:
    visible: true
---

# Hardware Management

### Switch and router installation

#### Physical installation

Physical installation and startup of a Catalyst switch require completion of these steps:

Before performing physical installation, verify the following:

Switch power requirements

Switch operating environment requirements (operational temperature and humidity)

Use the appropriate installation procedures for rack mounting, wall mounting, or table or shelf mounting.

Before starting the switch, verify the network cables that provide connectivity to end devices to the LAN.

Attach the power cable plug to the power supply socket of the switch. The switch will start. Some Catalyst switches do not have power buttons.

#### Boot sequence

Observe the boot sequence:

When the switch is on, the power on self-test (POST) begins. During POST, the switch LED indicators blink while a series of tests determines that the switch is functioning properly

Before you start the router, verify the power and cooling requirements, cabling, and console connection. Then, push the power switch to "On" and observe both the boot sequence and the Cisco IOS Software output on the console.

The Cisco IOS Software output text is displayed on the console.

When all startup procedures are finished, the switch is ready to configure.

![](<../.gitbook/assets/Unknown image (218)>)

#### Setup mode (router)

The setup mode is not intended to enter complex protocol features in the router but rather for a minimal configuration. You do not have to use the setup mode; you can use other configuration modes to configure the router.

```
Router# setup
....System Configuration Dialog....
Continue with configuration dialog? [yes/no]: no
```

The primary purpose of the setup mode is to rapidly bring up a minimal-feature configuration for any router that cannot find its configuration from some other source. In addition to being able to run the setup mode when the router boots, you may also initiate it by entering the setup privileged EXEC mode command.

#### Front-panel LEDs (Catalyst switches)

To help make sense of the LEDs, consider the example of the System LED for a moment. This LED provides a quick overall status of the switch with three simple states on most Cisco switches:

Off: The switch is not powered on.

On (green): The switch is powered on and operational. Cisco IOS Software has been loaded.

On (amber): The switch POST process failed, and the Cisco IOS Software did not load.

| Name                      | Description                                                                                                                                                                                                                                      |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Status                    | If on (green), each port LED represents the status of the port. If green, the link is present but there is no activity. If blinking green, the port is sending and receiving data. If amber, the port is blocked.                                |
| Duplex                    | If on (green), each port LED represents the duplex of this port (on is full-duplex; off means half-duplex).                                                                                                                                      |
| Speed                     | If on (green), each port LED represents the speed of this port, as follows: Off means 10 Mbps; Solid green means 100 Mbps; Single green flash means 1 Gbps. Blinking twice means >1Gbps.                                                         |
| Stack                     | The stack LED shows the sequence of member switches in a stack. For example, if the second port LED is lit on the switch, the switch is the second switch in the stack.                                                                          |
| Power over Ethernet (PoE) | Some switches have a PoE LED in the system status group of LEDs. If on (green), each port LED indicates if the port is supplying PoE.                                                                                                            |
| System                    | If green, the system is operating normally. If blinking green, the system is loading the software. If amber, the system is not functioning properly.                                                                                             |
| Active                    | This shows the stacking state of the switch. Green if the switch is active or standalone switch, slow blinking green if it is in the stack standby mode.                                                                                         |
| XPS                       | This indicates eXpandable Power System (XPS) status, used to provide backup power to connected devices that experience a power supply failure. It shows green if it is ready to provide back-up power to the connected device.                   |
| S-PWR                     | Shows the status of StackPower Cable. If green, each StackPower port is connected to a neighboring switch, consisting a ring topology. If blinking green, only one StackPower cable is connected to the switch, resulting in open ring topology. |
| Console                   | Shows the active USB Console port, this LED is off when the USB console is disabled.                                                                                                                                                             |
| Mode                      | A button that cycles the meaning of the port through six states (Status, Speed, Duplex, Active, Stack, and PoE).                                                                                                                                 |
| Port                      | Displays different meanings, depending on the port. The mode is toggled by using the mode button.                                                                                                                                                |

### Chassis-based routers and switches

In a chassis router, modular hardware components are housed within a chassis.

The hardware components connect to the chassis backplane.

The backplane is the circuitry within the chassis that allows the different hardware components to communicate with each other.

The connections between hardware components form the switch fabric (SF)

#### Hardware components

Line Cards (LC) provide network interfaces. ie. 10x400Gb SFP or 40x1Gig UTP interfaces

Route Switch Processor (RSP) cards manages control plane, management, and timing synchronization functions. It performs route processing and distributes forwarding tables to the line cards. It also controls fans, alarms, and power supplies using an I2C communication link to each fan tray and power supply

They contain management interfaces. RSP are also called supervisor cards

They can be deployed as 1+1 to provide redundancy

![](<../.gitbook/assets/Unknown image (219)>)

Route Processor (RP) cards also provide the control plane functions, packet switching, etc.

Fabric Control (BC) or Switch Cards (SC) connects the line cards between each other

Field Replaceable Unit (FRU) refers to a component or part of a device that is designed to be easily replaced or serviced by field technicians, for example Power supply, Fans, memory modules and linecards

![](<../.gitbook/assets/Unknown image (220)>)

### Console access and out-of-band management

#### Connecting to a console port

Unlike a computer host, Cisco switches do not have a keyboard, monitor, or mouse device to allow direct user interaction. Upon initial installation, you can configure the switch from a PC that is connected directly through the console port on the switch.

Cisco devices traditionally have an RJ-45 connector on their serial console port. On newer Cisco network devices, a USB serial console connection is also supported. An appropriate console cable is typically included with the Cisco device. Since modern computers and notebooks rarely include built-in serial ports, you may also need an adapter.

One way that you can access the CLI is through a direct, cabled connection that is called the console connection. To access the CLI directly using a console connection, you must be physically present at the location of the device. Accessing a device CLI through a console connection is also called out-of-band (OOB) access, emphasizing that no network bandwidth is consumed in the process.

Some devices, such as routers, may also support a legacy auxiliary port that was used to establish a CLI session remotely using a modem. Similar to a console connection, the auxiliary (AUX) port is OOB and does not require networking services to be configured or available.

#### Router auxiliary (AUX) port

Router Auxiliary (AUX) Port (#line aux 0) works like the console port, except that the Aux port is typically connected through a cable to an external analog modem, which in turn connects to a phone line for backup connectivity

#### Accessing the CLI via PuTTY (console)

[https://www.cisco.com/c/en/us/support/docs/smb/switches/cisco-small-business-300-series-managed-switches/smb4984-access-the-cli-via-putty-using-a-console-connection-on-300-a.html](https://www.cisco.com/c/en/us/support/docs/smb/switches/cisco-small-business-300-series-managed-switches/smb4984-access-the-cli-via-putty-using-a-console-connection-on-300-a.html)

For SDN device such as vEdge/cEdge use 115200 speed/baud rate

On devices with two console ports, only one console port can be active at a time. When a cable is plugged into the USB console port, the RJ-45 port becomes inactive. When the USB cable is removed from the USB port, the RJ-45 port becomes active.

When a console connection is established, you gain access to user EXEC mode by default.

![](<../.gitbook/assets/Unknown image (221)>)

#### In-band vs out-of-band management

In-band management uses the same network infrastructure as user data traffic. It uses SSH or Telnet.

Out-of-band management (OOBM) uses a separate physical connection. This can be a serial console, a dedicated management interface, or cellular.

#### Staging

**Staging** refers to the preparation and configuration of new devices (such as routers, switches, or firewalls) before they are deployed into a live network.

This process typically involves setting up the device's initial configuration, testing it in a controlled environment, loading necessary software or firmware, and ensuring the device meets all functional and security requirements

#### Terminal server

**Terminal server** is a router that serves as a central point for managing multiple networking devices

It has multiple asynchronous serial ports that connect to the console ports of these devices. These ports are used to establish console access connections to the managed devices.

Each asynchronous port on the terminal server acts as a virtual console connection to a network device

| ip host                   | Example ip host 2023 10.10.10.10                                                                                                                                                    |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show line                 | to check all availabke line properties and their respective number and their numbers                                                                                                |
| show sessions command     | asterisk (\*) indicates the current terminal session                                                                                                                                |
| telnet \[port] \[keyword] | Verification of successful telnet connection from terminal switch to the connected device port: Use the show line EXEC command to view the available lines; it's 2000+(line number) |
| Ctrl-Shift-6 then x.      | escape sequence                                                                                                                                                                     |

#### Connection to a switch via router AUX port

You can just ask customer to connect 2 devices using console cable for access

| telnet \<port 2000+AUX ID> | The port number is always 2000+ AUX ID that you can find under: ''show line'' |
| -------------------------- | ----------------------------------------------------------------------------- |

In this particular case the AUX have number 1. So port is 2001

![](<../.gitbook/assets/Unknown image (222)>)

| If telnet to AUX is disabled |
| ---------------------------- |
| line aux 0                   |
| transport input all          |

#### Change speed of an async port

To change speed of an async port:

```
conf t
line 33 40
rxspeed 9600
txspeed 9600
```

{% hint style="info" %}
Also try 38400 or 115200 for SDN devices.
{% endhint %}

### Rack and hardware installation

Some devices may need to be attached to both the front and back posts for stability and security, often enclosed in a locked cabinet for security and installed in Server racks

Server racks and chassis are physical structures used to house and organize server and network hardware. They provide a framework for securely mounting equipment in data centers or server rooms

The standard rack width is 19 inches (483 millimeters), and racks are divided into rack units (RU), with one RU being 1.75 inches/4,4cm in height

![](<../.gitbook/assets/Unknown image (223)>)

![](<../.gitbook/assets/Unknown image (224)>)

![](<../.gitbook/assets/Unknown image (225)>)

### Device factory reset

**Factory reset** erases all the customer-specific data stored in a device and restores the device to its original configuration at the time of shipping

[https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9200/software/release/17-18/configuration\_guide/sys\_mgmt/b\_1718\_sys\_mgmt\_9200\_cg/simplified\_factory\_reset.html](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9200/software/release/17-18/configuration_guide/sys_mgmt/b_1718_sys_mgmt_9200_cg/simplified_factory_reset.html)

| factory-reset {all\[ secure] \[ 3-pass] \| config \| boot-vars} | all : Erases all the content from the NVRAM, all the Cisco IOS images, including the current boot image, boot variables, startup and running configuration data, and user data. We recommend that you use this option. all secure : Performs data sanitization and securely resets the device. |
| --------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| pnpa service reset                                              | Removes all configuration from the switch including vlan.dat - this is better and safer version than factory reset command                                                                                                                                                                     |

### Cable management and termination

Vyvazovák (cable manager / cable organizer) is a passive rack accessory used to route and neatly organize patch cords or stack cables.

Purpose:

Keeps cables tidy, prevents tangling and bending.

Maintains proper airflow in the rack.

Reduces stress on switch ports and cables.

Size in RU:

The most common is 1 RU horizontal cable manager with rings or a cover.

Larger models exist (2 RU, 3 RU) if you have high cable density, but 1 RU is standard.

#### Patch panel

**Patch panel** provides a central point where multiple network cables can terminate their connections, allowing for the flexible, organized management and easier troubleshooting

It typically consists of a metal or plastic enclosure with multiple labeled ports or slots where cables can be inserted and connected

Fiber-optic Patch Panels are enclosures that act as a distribution hub for fiber cable. They make it easy to terminate fiber-optic cables and provide access to the cable's individual fibers for cross connection.

![](<../.gitbook/assets/Unknown image (226)>)

![](<../.gitbook/assets/Unknown image (227)>)

PC's in the office are usually connected to the wall plate, which leads through the walls to the patch panel

![](<../.gitbook/assets/Unknown image (228)>)

#### ODF (Optical Distribution Frame)

**ODF (Optical Distribution Frame)** is a physical frame or enclosure used for organizing, managing, and interconnecting fiber optic cables within a network infrastructure. It serves as a centralized point where fiber cables can be terminated, connected, and cross-connected

Types of ODFs:

Rack-Mounted ODF: Installed in a server rack, commonly used in data centers where space is optimized by stacking multiple ODFs.

Wall-Mounted ODF: Mounted on walls, typically for smaller installations where rack space is limited.

Stand-Alone ODF: Free-standing frames, often used in larger setups where more extensive cross-connecting is needed.

![USES OF OPTICAL DISTRIBUTION FRAME (ODF) - Linxcom UK](<../.gitbook/assets/Unknown image (229)>)

#### DWDM ODF / passive modules

These are simplex single mode optical patch cords terminating LC/PC connectors. These patch cords are used to connect individual modules to each other.

ONS-MPO16-2X8-x=

It is an optical patchcord that unravels from one MPO-24 with 16 used fibers to two MPO-12 with 8 used fibers, where x is the length of the optical patch cord. It is used for various connections together with the flex ROADM module, for example with the 20-port SMR module.

![](<../.gitbook/assets/Unknown image (230)>)

![](<../.gitbook/assets/Unknown image (231)>)

![](<../.gitbook/assets/Unknown image (232)>)

### Power: connecting devices to the power source

#### Alternating Current (AC) power

✅ Most Cisco devices (switches, routers, firewalls) use AC power.

✅ Typical connector:

Standard C13-C14 IEC cable (same as for PCs and servers).

Plugged into a 230V outlet (Europe) or 120V (US).

✅ How to recognize it?

The power supply has a standard three-pin C14 IEC input.

The cable goes directly from the power outlet to the device (no need for manual wiring).

The label on the device typically states 100–240V AC.

#### Direct Current (DC) power

✅ Used in telecommunications, data centers, and industrial environments.

✅ Typical voltages:

-48V DC (common in data centers and telco infrastructure).

+12V, +24V, or +48V DC (on some specialized Cisco products).

✅ How to recognize it?

Power inputs are screw terminal blocks, where DC wires are manually secured.

The power cable does not have a standard plug but instead connects directly to a DC generator or battery system.

The device label typically states -48V DC or another DC voltage.

#### Devices supporting both AC and DC power

Some Cisco devices support interchangeable power modules, allowing either AC or DC power:

Examples include Cisco ASR routers, some Catalyst switches (e.g., Catalyst 9500), and Nexus 9000 series.

Users can swap the power supply (PSU) depending on their power source.

You can check this on the PSU label – if it states AC/DC, the device supports both.

#### How to identify what your Cisco device requires

✅ Check the device label or power supply unit (PSU) – it will indicate AC 100-240V or DC -48V.

✅ Look at the power connector:

Standard IEC power cable (C13/C14) → AC

Screw terminal block → DC

✅ Review Cisco documentation – some devices allow modular power supply choices (AC or DC PSU).

DC kabel typicky není součástí dodávky DC zdroje, jelikož nemá konektory, ale připojují se přímo jednotlivé žíly kabelu.

Jen u velkých zařízení se dodávají i speciální DC kabely, ale to není případ IE switchů.

Pokud půjde do klasické zásuvky, tak „CAB-ACE=“, nebo „CAB-AC-EUR=“ nebo „PWR-CAB-AC-EU=“

![](<../.gitbook/assets/Unknown image (233)>)

Pokud to chtějí zapojit do PDUčka nebi UPSky tak třeba „CAB-C13-CBN=“ nebo „CAB-C13-C14-2M=“

![](<../.gitbook/assets/Unknown image (234)>)

### PDU (Power Distribution Unit)&#x20;

znamená rozvod napájení v racku, ne konkrétní typ kabelu.

Když někdo řekne **PDU napájecí kabel**, obvykle tím myslí kabel, který vede ze zařízení (router, switch, server) do rackového PDU (např. APC).

Existují dvě běžné varianty:

1. **IEC C13/C14** – nejběžnější.
   * Router: IEC C14 vstup.
   * Kabel: C13 → C14.
   * Druhý konec se zapojuje do PDU s IEC zásuvkami.
2. **Schuko (typ E/F)** – klasická "domácí" zástrčka.
   * Kabel: C13 → Schuko.
   * Může být zapojen přímo do zásuvky ve zdi nebo do PDU, které má Schuko zásuvky.

Takže odpověď je: **ano, může to být obojí**. Záleží pouze na tom, jaké výstupy má PDU.

Například:

* APC Rack PDU s IEC C13/C19 zásuvkami → používají se kabely C13↔C14.
* APC Rack PDU se Schuko zásuvkami → používají se klasické napájecí kabely se Schuko vidlicí.

Ve většině datacenter jsou standardem **IEC kabely (C13/C14 nebo C19/C20)**, protože se lépe zajišťují proti vytažení a šetří místo. Schuko kabely se častěji používají v kancelářích nebo menších serverovnách.

### Electricity overview

The energy that we call Electricity is essentially leveraging of flow of electrons from negatively charged end to positively charged end

Electricity in the grid is not about the direct movement of electrons but is transmitted through electromagnetic fields

![](<../.gitbook/assets/Unknown image (235)>)

![](<../.gitbook/assets/Unknown image (236)>)

That is why charger has two connectors: one for incoming electrons and one for outgoing

![](<../.gitbook/assets/Unknown image (237)>)

### Electricity primer

#### AC (Alternating current)

is an electrical current type in which the flow of electrical charge periodically reverses direction

It is the form in which electrical power is being generated by turbines or generators

We are able to maintain and transmit the AC over long distances thanks to Transformers.

Transformers work based on electromagnetic induction, which relies on changes in magnetic flux

Transformers can also step up or down the current capacity, resisting any power loss during the transmission

Once the electricity reaches substations near residential areas, it is stepped down to lower voltages for distribution to homes and businesses.

![](<../.gitbook/assets/Unknown image (238)>)

#### DC (Direct current)

is an electric current type that flows consistently in one single direction from negative to positive.

DC flows evenly throughout the cross-sectional area of the wire, reducing loss of power due to the 'skin effect' in AC

Converting DC voltage levels requires more complex and costly equipment, such as high-power converters, making it less efficient

Since Transformers work only for AC, there is big con of the DC and that is it can pose a power loss during the transmission

When the electricity enters your home or business, it typically comes in as AC.

However, most household appliances and electronic devices operate on DC

Therefore, the power adapters or converters you use with your devices convert the AC electricity from the wall outlet into DC electricity that your appliances can use

DC cannot be transmitted economically over long distances due to a drop in voltage.

![](<../.gitbook/assets/Unknown image (239)>)

![](<../.gitbook/assets/Unknown image (240)>)

QSFP-DD modules use a flat top design, allowing for a larger "riding" heatsink instead of a smaller, integrated one. This innovative approach enables significantly better cooling performance and facilitates ongoing design improvements for optimal thermal management.

### Power over Ethernet (PoE)

**Power over Ethernet (PoE)** is a technology that allows to transmit data signal and provide power source over a single ethernet cable, eliminating the need for external power source for devices like access point, another switch or IoT device

It is achieved by dedicating two of the four twisted pairs in an Ethernet cable (pins 1-2 and 3-6) for data transmission, while the other two pairs (pins 4-5 and 7-8) are dedicated for power transmission.

PoE is primarily designed for powering and connecting low-power network devices over Ethernet cables

PoE can transmit 100 meters from the switch or hub to the Network interface controller (NIC), regardless of where the power is injected. The limitation is not the power; the Ethernet cabling standards limit the total length of cabling to 100m

Power sourcing equipment (PSE) (switch) provides power to powered devices (PD)

PoE+ provides up to 30 watts of power per port, which is more than double the maximum power provided by PoE (which offers up to 15.4 watts).

uPOE (Ultra Power over Ethernet) provides up to 60 watts of power per port.

Perpetual POE provides uninterrupted power to connected powered device (PD) even when the power sourcing equipment (PSE) switch is booting.

| Device(config-if)# power inline port perpetual-poe-ha | Configures perpetual PoE. When you configure perpetual PoE on a port connected to a PD device, the PD device remains powered on during reload. |
| ----------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |

| show power inl   | POE |
| ---------------- | --- |
| show power       | PSU |
| show environment |     |

![](<../.gitbook/assets/Unknown image (1343)>)

![Mode A vs. Mode B PoE Pinout](<../.gitbook/assets/Unknown image (1344)>)

![PoE-Class-Types](<../.gitbook/assets/Unknown image (1345)>)

![IEEE Standards and Devices](<../.gitbook/assets/Unknown image (1346)>)

## Link Aggregation

**EtherChannel** is a link aggregation method that bundles several physical links into a single logical **Port-channel (Po)**

Etherchannel enables packets to be sent over several physical interfaces as if over a single interface

The logical Po interface is created automatically as soon as two or more physical links are assigned to the group. Each bundle has a single MAC, a single IP address, and a single configuration set such as ACLs or QoS. Port channel configuration is reflected to all member physical ports

EtherChannel always creates one-to-one logical links. You cannot send traffic to two different switches through the same EtherChannel logical link. One EtherChannel logical link always connects only two devices.

You can also configure multiple EtherChannel links between two devices. However, when several logical EtherChannel links exist between two switches, STP detects loops.

To avoid loops, STP will make only one logical link operational. When STP blocks the redundant links, it blocks one entire EtherChannel, thus blocking all the ports belonging to that EtherChannel link.

### Benefits

STP sees links in Ethechannel as a single link, as a result STP does not consider a single link to be loop and put all port channel interfaces in the forwarding state. The broadcast received on one of the etherchannel ports wouldn't cause broadcast storm, since all ports within the etherchannel acts as one port, thus the switch won't send the broadcast back from the port, where the broadcast was received from.

Ensures redundancy - If a link fails, EtherChannel redirects traffic from the failed link to the remaining links in the channel without downtime

Load balances network traffic across all links/ports. Frames belonging to the same flow always traverse the same physical link (per-flow). Flows cannot exceed BW of an individual link

Etherchanel is up as long as at least one physical link is active

It can bundle L3 routed ports (ports that do not run DTP,STP) - can be assigned an IP address to isolate broadcast domain

Can be used on L2/access or L2/trunk port as well as on L3 ports - also between switches and servers

![](<../.gitbook/assets/Unknown image (536)>)

### Bundle member requirements

The individual links must match on several parameters:

Interface types cannot be mixed, for instance FastEthernet or Gigabit Ethernet cannot be bundled into a single EtherChannel.

Speed and duplex settings must be the same on all the participating links.

Switchport mode and VLAN information must match. Access ports must be assigned to the same VLAN. Trunk ports must have the same allowed range of VLANs. The native VLAN

must be the same on all the participating links.

Routed L3 etherchannels must have matching duplex mode and bandwidth

![](<../.gitbook/assets/Unknown image (537)>)

![](<../.gitbook/assets/Unknown image (538)>)

### Static EtherChannel

Manual static configuration places the interface in an EtherChannel manually, without any negotiation. No negotiation between the two switches means that there is no checking to make

sure that all the ports have consistent settings.

With static configuration, you define a mode for a port. There is only one static mode, the on mode. When static on mode is configured, the interface does not negotiate—it does not exchange any control packets. It immediately becomes part of the aggregated logical link, even if the port on the other side is disabled

| (config-if)# channel-group mode on     | enables default etherchannel without negotiations - when a device does not support LACP or Pagp                   |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| port-channel channel-number persistent | Converts the auto created EtherChannel into a manual one and allows you to add configuration on the EtherChannel. |
| show etherchannel \[summary \| detail] |                                                                                                                   |
| show interface port-channel            |                                                                                                                   |

### LACP (Link Aggregation Control Protocol)

**LACP (Link Aggregation Control Protocol)** is an open (IEEE) EtherChannel protocol

With LACP, you can control link aggregation (for example the maximum number of bundled ports allowed). LACP is superior to static port channels with its automatic failover where

traffic from a failed link within EtherChannel is sent over remaining working links in the EtherChannel.

LACP controls the bundling of physical interfaces to form a single logical interface.

When you configure LACP, LACP packets are sent between LACP enabled ports to negotiate the forming of a channel. When LACP identifies matched Ethernet links, it groups the matching links into a logical EtherChannel link.

Supports up to 16 links in a channel, but only 8 can be in operation and all other as hot-standby

| (config-if-range)# channel-group 1 mode { active \| passive } | bundles physical ports into port channel. One end must be active and the second either active or passive. Passive and Passive does not form etherchannel between devices Active sets the port to actively negotiate etherchannel with LACP packets Passive sets the port to listen for PagP frames to form an etherchannel |
| ------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-if)# port-channel min-links <>                        | Specifies the minimum number of member ports that must be in the link-up state and bundled in the EtherChannel for the port channel interface to transition to the link-up state                                                                                                                                           |
| (config-if)# lacp port-priority 32000                         | to enforce active state for port in an etherchannel                                                                                                                                                                                                                                                                        |
| (config)#port-channel auto                                    | Auto-LAG is LACP feature that enables to create port channel autmatically                                                                                                                                                                                                                                                  |

LACP system priority identifies which switch is the master switch for a port channel (when there are more member interfaces in a port channel than the maximum number, master choose which member interfaces are active, and this can be configured with system priority)

LACP Fast when fail occurs a link can be identified and removed in 3 seconds compared to the 90 seconds specified in the original LACP standard

{% hint style="info" %}
The `channel-group` identifier does not need to match on both sides. Use the same number anyway. It makes operations easier.
{% endhint %}

### PAgP (Port Aggregation Protocol)

Cisco proprietary etherchannel protocol. Up to 8links in a channel

| (config-if-range)# channel-group 1 mode { desirable \| auto } | bundles physical ports into port channel. One end has to be desirable and the second either desirable or auto. Auto and Auto does not form etherchannel between devices Desirable sets the port to actively negotiate etherchannel Auto sets the port to listen for PagP frames to form an etherchannel |
| ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| interface Port-channel1 switchport mode trunk                 | then configure port channel as trunk You can configure IP address aswell, if you want this etherchannel to be L3                                                                                                                                                                                        |
| (config-if)#pagp port-priority <>                             | to configure which port is always selected for packet transmission                                                                                                                                                                                                                                      |

{% hint style="info" %}
The EtherChannel number does not need to match on both sides. Use matching numbers anyway. It reduces confusion later.
{% endhint %}

### Load-balancing options

EtherChannel balances the traffic load across the links in a channel by reducing part of the binary pattern formed from the addresses in the frame to a numerical value that selects one of the links in the channel. You can specify one of several different load-balancing modes, including load distribution based on MAC addresses, IP addresses, source addresses, destination addresses, or both source and destination addresses. The selected mode applies to all EtherChannels configured on the switch.

| (config)#port-channel load-balance { dst-ip \| dst-mac \| src-dst-ip \| src-dst-mac \| src-ip \| src-mac } | to set the load-distribution method based on the source/dest MAC/IP address show etherchannel load-balance                                                                                                                                                                                                      |
| ---------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| port-channel load-defer                                                                                    | allows ports to be bundled into port channels, but prevents the assignment of group mask values to these ports. This prevents the traffic from being forwarded to new stack members that would introduce data loss, since the data path is not fully established for new member. #show platform pm group-masks. |

### IOS XR: link bundles

The Link Bundling feature allows you to group multiple point-to-point links together into one logical link and provide higher bidirectional bandwidth, redundancy, and load balancing between two routers. A virtual interface is assigned to the bundled link. The component links can be dynamically added and deleted from the virtual interface.

The virtual interface is treated as a single interface on which one can configure an IP address and other software features used by the link bundle. Packets sent to the link bundle are forwarded to one of the links in the bundle.

A link bundle is simply a group of ports that are bundled together and act as a single link. The advantages of link bundles are as follows:

Multiple links can span several line cards to form a single interface. Thus, the failure of a single link does not cause a loss of connectivity.

Bundled interfaces increase bandwidth availability, because traffic is forwarded over all available members of the bundle. Therefore, traffic can flow on the available links if one of the links within a bundle fails. Bandwidth can be added without interrupting packet flow.

Cisco IOS XR software supports the following method of forming bundles of Ethernet interfaces:

IEEE 802.3ad—Standard technology that employs a Link Aggregation Control Protocol (LACP) to ensure that all the member links in a bundle are compatible. Links that are incompatible or have failed are automatically removed from a bundle.

Any type of Ethernet interfaces can be bundled, with or without the use of LACP (Link Aggregation Control Protocol).

Bundle membership can span across several line cards that are installed in a single router.

A single bundle supports maximum of 64 physical links.

Different link speeds are allowed within a single bundle, with a maximum of four times the speed difference between the members of the bundle.

Physical layer and link layer configuration are performed on individual member links of a bundle.

Configuration of network layer protocols and higher layer applications is performed on the bundle itself.

A bundle can be administratively enabled or disabled.

Each individual link within a bundle can be administratively enabled or disabled.

Ethernet link bundles are created in the same way as Ethernet channels, where the user enters the same configuration on both end systems.

The MAC address that is set on the bundle becomes the MAC address of the links within that bundle.

When LACP configured, each link within a bundle can be configured to allow different keepalive periods on different members.

Load balancing (the distribution of data between member links) is done by flow instead of by packet. Data is distributed to a link in proportion to the bandwidth of the link in relation to its bundle.

QoS is supported and is applied proportionally on each bundle member.

Link layer protocols, such as CDP and HDLC keepalives, work independently on each link within a bundle.

Upper layer protocols, such as routing updates and hellos, are sent over any member link of an interface bundle.

Bundled interfaces are point to point.

A link must be in the up state before it can be in distributing state in a bundle.

All links within a single bundle must be configured either to run LACP or EtherChannel (non-LACP). Mixed links within a single bundle are not supported.

A bundle interface can contain physical links and VLAN subinterfaces only.

Access Control List (ACL) configuration on link bundles is identical to ACL configuration on regular interfaces.

Multicast traffic is load balanced over the members of a bundle. For a given flow, internal processes select the member link and all traffic for that flow is sent over that member.

| RP/0/RSP0/CPU0:Router(config)# interface gig0/2/0/3 RP/0/RSP0/CPU0:Router(config-if)# bundle id 100 mode on\|active\|passive RP/0/RSP0/CPU0:Router(config)# interface gig0/2/0/4 RP/0/RSP0/CPU0:Router(config-if)# bundle id 100 mode on\|active\|passive | The default number of active links allowed in a single bundle is 8. To add interface members on the bundle If no mode is specified it falls into on mode - no LAC enabled                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RP/0/RSP0/CPU0:Router(config)# interface Bundle-Ether 100.1 l2transport RP/0/RSP0/CPU0:Router(config-subif)# encapsulation dot1q 11 RP/0/RSP0/CPU0:Router(config-subif)# commit                                                                           | To create subinterfaces on the bundle, use these commands:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| bundle maximum-active links 1                                                                                                                                                                                                                             | designates one active link and one link in standby mode that can take over immediately for a bundle if the active link fails (1:1 protection). Member interfaces that are in standby are displayed in the collecting state Election of active and standby The standby port is determined based on the Port ID and system ID. The system ID is based on system priority and MAC address. Whichever is lower is treated as higher priority. The lower system ID (or high-priority system) device will decide which port is in standby. The highest Port ID will be put into standby state. Both port ID and system priority are user-configurable |
| show bundle                                                                                                                                                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |

### Convergence notes (IOS XR)

A bundle member port switchover won't cause TE/FRR switchover. Link bundle convergence is around 20 msec for Layer 3 service and 3-4 msec for Layer 2 service. In 3.7.x release, the ASR 9000 Series supports only hot standby mode. In the 3.9 release, it supports both hot standby and warm standby modes. Warm standby is the default configuration.

With hot standby mode, the standby port is in the collecting state. Potentially, it will save a couple of msec moving to the forwarding state compared with warm standby

![](<../.gitbook/assets/Unknown image (539)>)

## Switch Stacking

**Switch stacking** combines multiple switches into one logical device.

**Switch stack** is a set of multiple switches interconnected with special cabling to form a single logical unit managed as one device. It simplifies management. It increases capacity (port count) and improves redundancy. If one member fails, the stack can keep operating (assuming endpoints are dual-homed). An Advantage license is required for switch stacking.

**Stack Master** is the switch with the highest priority (configured 1-15; 1 is default). It controls the whole stack. The stack is managed through a single IP address.

All stack members share control and management plane, and they keep synchronized system-level configuration files such as CAM,IP,STP,VLAN and BID of master switch.

The interface-specific configuration of each stack member is associated with the stack member number. All members use the same SDM template as the master

The standby switch takes over, if primary master fails. When a stack member leaves the switch stack, the remaining stack members age out or remove all addresses learned by the former stack member. All stack members must run the same Cisco IOS software image and feature set to ensure compatibility between stack members

stack number/module number/port number

**Multichassis EtherChannel (MEC/MLAG)** allows port-channels to be configured across different switch units within a stack

![](<../.gitbook/assets/Unknown image (528)>)

### Verification and troubleshooting commands

| show module                                                  |                                        |
| ------------------------------------------------------------ | -------------------------------------- |
| show switch \[detail]                                        |                                        |
| show diagnostic events slot <>show diagnostic result slot <> | to perform diagnostic test on a module |

### Stack protocol version and compatibility

Each software image includes a stack protocol version. The stack protocol version has a major version number and a minor version number (for example 1.4, where 1 is the major version number and 4 is the minor version number) that determine the level of compatibility among the stack members.

Switch with the same major version number but with a different minor version number are considered partially compatible. When connected to a switch stack, a partially compatible Switch enters version-mismatch (VM) mode and cannot join the stack as a fully functioning member and generates syslog message of the incompatibility detail

Auto-Copy copies software from stack members to upgrade a switch in VM mode.

Auto-Extract searches for the needed software across the stack if it's not found in the stack member.

| show platform stack-manager all                                                                      | Display all stack information, such as the stack protocol version.                                                                                                                                                                                                                                                                                                                              |
| ---------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| archive copy-sw /destination-system \[member-number] /allow-feature-upgrade /overwrite /force-reload | This command will copy the IOS image from the master switch to incompatible Member of the stack, allow feature upgrades, overwrite the existing image, and force a reload of Member to activate the new image. If you download your image by using the copy tftp: boot loader command instead of the archive download-sw privileged EXEC command, the proper directory structure is not created |
| archive download-sw /rolling-stack-upgrade                                                           | Rolling stack upgrade feature upgrades the members one at a time, to avoid losing network connectivity                                                                                                                                                                                                                                                                                          |
| show switch stack-ports summary                                                                      | display a summary of the stack ports                                                                                                                                                                                                                                                                                                                                                            |

![](<../.gitbook/assets/Unknown image (529)>)

### VSS (Virtual Switching System) (EOL)

support up to 2 physical switches that can establish a single logical network entity, they are connected by ethernet cable

It supports devices that are geographically separated, which ensures redundancy and disaster recovery in the event of a failure at one location

**Virtual Switch Link (VSL)** is the MLAG (EtherChannel) between the two switches. It carries VSS control traffic and data traffic destined for the other switch.

**VSLP (Virtual Switch Link Protocol)** is used to establish and maintain VSL/VSS. It has two component protocols: **LMP** and **RRP**.

**LMP (Link Management Protocol)** performs the following functions:

Verifies link integrity by establishing bidirectional traffic forwarding.

Unidirectional links are rejected.

Exchanges switch IDs.

Before setting up the VSS you should configure one switch's ID as '1' and the other as '2'.

Exchanges other required information.

**RRP (Role Resolution Protocol)** performs the following functions:

Determines if the hardware and software versions of the switches are compatible.

Determines the active switch and standby switch.

Depending on the compatibility checks, the stack will come up in one of two modes:

RPR (Route-Processor Redundancy) mode the standby switch cannot forward traffic, but is available as a backup if the active fails

NSF/SSO mode the standby switch is fully initiliazed and can forward traffic

![](<../.gitbook/assets/Unknown image (530)>)

![](<../.gitbook/assets/Unknown image (531)>)

### StackWise

**StackWise** supports up to 9 separate physical switches to be stacked into one stack "vertically". Switches are connected through StackWise Plus ports with proprietary cabling

When all switches in the stack are powered on, **SDP (Stack Discovery Protocol)** uses broadcast messages (via the stack connections) to discover the stack topology

After all switches in the stack have been discovered, switch numbers are determined. Switches must have a unique number (1 to 8).

After Discovery, an Active switch is elected. Switches that boot up within a 2-minute election window participate

The switch with the highest priority (default 1, highest 15) wins the election and assumes the Master role, if the priority is the same for all the switch with the lowest MAC wins

An election is not triggered when a new switch is added and they assume member role

#### Physical connection

Stacking Modules Required: Cisco 9200/9300 series switches do not come with stacking modules by default. You need to order them separately as a kit, which includes two modules and a cable for stacking.

Installation: Once you receive the stacking kit, remove the two panels at the back of the switch to install the modules. Each switch in the stack requires these modules.

Stacking Process: Connect the stacking cables between the modules of two switches, with the Cisco logo on the cable facing upwards. Stack additional switches by daisy-chaining the cables between them.

Complete the Stack Ring: On the final switch in the stack, connect the last cable from the bottom port of the last switch to the top port of the first switch to complete the stacking ring - as shown in the picutre below, the two switches are connectd with two cables to form a ring

Switch Configuration: Though not covered in the video, the stack allows configuration for setting the stack master and standby, and other management settings.

![](<../.gitbook/assets/Unknown image (532)>)

![](<../.gitbook/assets/Unknown image (533)>)

### StackPower

Cisco technology designed to share power across multiple switches in a stack. It allows connected switches to pool their power supplies, effectively creating a shared power system. If one switch in the stack loses its power supply, the other switches in the stack can compensate by supplying power to it. This enhances power redundancy and efficiency, ensuring that devices continue to operate even if there is a failure in one switch's power supply.

### StackWise Virtual

**StackWise Virtual** is a successor to VSS. It allows up to 8 separate physical switches to form a single virtual switch over Ethernet cables. It can connect switches "horizontally" over longer distances compared to physical stacking cables.

Supported on Catalyst 9400/9500/9600 series switches. They don't have dedicated stackwise modules, thus they are connected via usual ethernet ports.

3 links are needed - two links for etherchannel to create SVL and third is DAD link to prevent split brain issue

The switches in the stack are connected via the **SVL (StackWise Virtual Link)**

Link Management Protocol (LMP) is activated on each link of the StackWise Virtual link as soon as it is brought up online.

Periodic Hellos messages are exchanged to monitor and maintain the link health; integrity ensured by establishing bidirectional traffic forwarding, and rejects any unidirectional links

Control Traffic: Traffic used to establish and maintain the Stackwise Virtual domain.

Data Traffic: when traffic received on one member must be sent out of an interface on the other member

Control plane is active/standby, data plane is active/active

![](<../.gitbook/assets/Unknown image (534)>)

Frames sent across the SVL are encapsulated with a 64-byte StackWise Virtual Header.

![](<../.gitbook/assets/Unknown image (535)>)

### vPC (virtual Port Channel)

technology used for Cisco Nexus switches, but each switch is managed and configured independently (control and management planes are independent) with no preemption

It allows devices to connect to two separate Nexus switches as if they were connecting to a single logical switch, offering redundancy and improved network capacity

#### Switch cluster

is a set of Switch connected through their LAN ports, such as the 10/100/1000 ports.

### Stack membership and provisioning

A new, out-of-the-box Switch (one that has not joined a Switch stack or has not been manually assigned a unique stack member number) ships with a default stack member number of 1.

When it joins a Switch stack, its default stack member number changes to the lowest available member number in the stack.

Make sure that you power off the Switch that you add to or remove from the Switch stack

After adding or removing stack members, make sure that the Switch stack is operating at full bandwidth (64 Gb/s).

Press the Mode button on a stack member until the Stack mode LED is on. The last two right port LEDs (usually 10Ge SFP module ports) on all Switch in the stack should be green, if they are not, the stack is not operating at full bandwidth.

| #switch renumber                     | takes effect after reload and cannot be used when the switch is already provisioned                                                                                                                                                                                                         |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| #reload slot                         |                                                                                                                                                                                                                                                                                             |
| #switch priority                     | The new priority value takes effect immediately but does not affect the current active switchstack master. The new priority value helps determine which stack member is elected as the new active switchstack master when the current active switchstack master or the switch stack resets. |
| reload slot \<stack\_memebr\_number> | to reload specific member                                                                                                                                                                                                                                                                   |

As described in the hardware installation guide, you can use the Switch port LEDs in Stack mode to visually determine the stack member number of each stack member.

In the default mode Stack LED will blink in green color only on the stack master. However, when we scroll the Mode button to Stack option - Stack LED will glow green on all the stack members.

When mode button is scrolled to Stack option, the switch number of each stack member will be displayed as LEDs on the first five ports of that switch. The switch number is displayed in binary format for all stack members. On the switch, the amber LED indicates value 0 and green LED indicates value 1.

Example for switch number 5 (Binary - 00101):

First five LEDs will glow in below color combination on stack member with switch number 5.

Port-1 : Amber

Port-2 : Amber

Port-3 : Green

Port-4 : Amber

Port-5 : Green

Similarly first five LEDs will glow in amber or green, depending on the switch number on all stack members.

New switch member can be configured with offline configuration feature to be provisioned by the stack

You must change the stack-member-number on the provisioned switch before you add it to the stack, and it must match the stack member number that you created for the new Switch on the switch stack. The Switch type in the provisioned configuration must match the Switch type of the newly added Switch

| switch stack-member-number provision type                             |                                                                                               |
| --------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| session stack-member-number                                           | access specific members. The stack member number is appended to the system prompt (Switch-2#) |
| switch stack-member-number stack port port-number {enable \| disable} | Manually Enabling/Disabling a Stack Port                                                      |
| #pnpa service reset                                                   | removes the configuration files as well as the stack-related information                      |

### Potential issues

Adding powered-on Switch (merging) causes the stack masters of the merging Switch stacks to elect a stack master from among themselves. The re-elected stack master retains its role and configuration and so do its stack members. All remaining Switch, including the former stack masters, reload and join the Switch stack as stack members. They change their stack member numbers to the lowest available numbers and use the stack configuration of the re-elected stack master. The new stack master becomes available after a few seconds. In the meantime, the Switch stack uses the forwarding tables in memory to minimize network disruption. The physical interfaces on the other available stack members are not affected during a new stack master election and reset

Removing powered-on stack members causes the Switch stack to divide (partition) into two or more Switch stacks, each with the same configuration. This can cause an IP address

configuration conflict in your network. If you want the Switch stacks to remain separate, change the IP address or addresses of the newly created Switch stacks.

#### Split brain and dual-active detection (DAD)

If the VSL between two switches fails, one switch does not know the status of the other. Both switches could change to the active mode, causing a dual-active situation in the network with duplicate configurations (including duplicate IP addresses and bridge identifiers). The network might go down.

#### vPC split-brain scenario

Suppose you have two Cisco Nexus switches, switch A and switch B, configured with a vPC. The vPC is configured with two links, one from each switch, that are bundled together into a single logical port channel. The vPC is used to connect servers or other network devices to the data center network.

If there is a failure in the network infrastructure that connects the two switches, such as a cable or a switch failure, the two switches can become isolated from each other.

In this scenario, the vPC can become split-brain, where each switch believes it is the primary switch and continues to forward traffic to its connected devices.

If a server is connected to the vPC and is configured to use both links in the vPC, it may receive duplicate packets or may not receive packets at all, depending on which switch it is communicating with.

#### EtherChannel fix (dual-active prevention)

To prevent a dual-active situation, the core switches send PAgP protocol data units (PDUs) through the remote satellite links (RSLs) to the remote switches. The PAgP PDUs identify the active switch, and the remote switches forward the PDUs to core switches so that the core switches are in sync. If the active switch fails or resets, the standby switch takes over as the active switch. If the VSL goes down, one core switch knows the status of the other and does not change its state.

### StackWise Virtual staging

#### 48-port variant

**1.Přečíslování druhého switche na 2**

switch renumber 2

**2.Nastavení priority switche 1**

switch priority 15

**3.Nastavení domain – je třeba použít ID dle excelu per lokalita**

device(config)#stackwise-virtual

domain 15

Uložit a rebootovat oba switche.

**4.Nastavení StackWise Virtual Link na všech propojích a na obou switchích**

switch 1:

int twe1/0/47

stackwise-virtual link 1

des SWV SVL 1

int twe1/0/48

stackwise-virtual link 1

des SWV SVL 2

switch 2:

int twe2/0/47

stackwise-virtual link 1

des SWV SVL 1

int twe2/0/48

stackwise-virtual link 1

des SWV SVL 2

Uložit config a restartovat oba switche.

**5.Nastavení Dual-Active-Detection Link**

int twe 1/0/46

des SWV DAD

stackwise-virtual dual-active-detection

int twe 2/0/46

des SWV DAD

stackwise-virtual dual-active-detection

**6. Nastavení hostname -> dle tabulky -> hostname u C9500 je vždy s 01 nakonci. 02 slouží jen jako label na zařízení.**

hostname U-CV-RIEG-DSW01

#### 24-port variant

**1.Přečíslování druhého switche na 2**

switch renumber 2

**2.Nastavení priority switche 1**

switch priority 15

**3.Nastavení domain – je třeba použít ID dle excelu per lokalita**

device(config)#stackwise-virtual

domain 15

Uložit a rebootovat oba switche.

**4.Nastavení StackWise Virtual Link na všech propojích a na obou switchích**

switch 1:

int twe1/0/23

stackwise-virtual link 1

des SWV SVL 1

int twe1/0/24

stackwise-virtual link 1

des SWV SVL 2

switch 2:

int twe2/0/23

stackwise-virtual link 1

des SWV SVL 1

int twe2/0/24

stackwise-virtual link 1

des SWV SVL 2

Uložit config a restartovat oba switche.

**5.Nastavení Dual-Active-Detection Link**

int twe 1/0/22

des SWV DAD

stackwise-virtual dual-active-detection

int twe 2/0/22

des SWV DAD

stackwise-virtual dual-active-detection

**6. Nastavení hostname -> dle tabulky -> hostname u C9500 je vždy s 01 nakonci. 02 slouží jen jako label na zařízení.**

hostname U-CV-RIEG-DSW01

### StackWise RMA

1. Switch must be connected first outside of the stack, IOS needs to be updated to the same version as other switches (bundle mode, no install)
2. Renumber Switch to 1 if not done already.
3. Assign lesser priority to the new one and then turn it off, connect it to the rack, turn it on
4. New Switch should now sync the config from the stack master, you can change priority back to original state to make 1 master again.

Step 1: Power up the switch and wait for it to boot.

* DO NOT attach any stacking cable.

{% hint style="info" %}
Attaching a stack cable to a powered switch will cause the entire stack to reboot.
{% endhint %}

Step 2: Renumber the new switch accordingly with the old (faulty) member switch

Set the stack member number to the original stack numbering scheme (in configuration mode):

switch current-stack-member-number renumber new-stack-member-number

Verify the priority issuing the 'show switch detail' command.

If the priority is higher than one, change it to one:

switch stack-member-number priority priority-number

Step 3: Ensure that "software auto-upgrade enable" has been configured on the main stack so that if the new switch doesn't have the same IOS firmware as the master, it will be up/downgraded.

Step 4: Unplug the new (replacement) switch from power; make sure it's dead.

Step 5: Plug the stack wise cable in.

Step 6: The new (replacement) switch will boot and downloads its new firmware from the master if required and join the stack.

Step 7: The new (replacement) switch will upload the existing configuration from the master switch

Step 8: Check the new stack: #show switch #show interfaces status

### IOS mode and licensing alignment

When used in conjuction with StackWise:

all switches in the stack should be configured with the same mode of ios = install or bundle

all switches in the stack should have the same license as the active switch

when upgrading the IOS in install mode, simply upgrade the active switch, and all switches will be upgraded

when upgrading the IOS in bundle mode, the IOS file must be copied to each switch in the stack
