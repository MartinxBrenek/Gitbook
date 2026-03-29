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

# Hardware Management - Cisco

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

### Advanced equipment cooling

![](<../.gitbook/assets/Unknown image (241)>)
