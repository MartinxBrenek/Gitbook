---
description: L2
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

# Switching

### Network communication types

**Unicast** is a communication between a single source and single destination station

**Broadcast** is a communication between a single station to all other stations on a given network segment - used for DHCP, ARP or other discovery mechanisms. Broadcast is received and must be processed by all devices in the network and

**Multicast** is a communication sent from a single source to a group of devices that have specifically subscribed to receive the traffic destined to the multicast address. Multicast is often used for communication between multiple hosts that subscribed to receive multicast traffic from video or audio souce/server, but most importantly utilized by control plane mechanisms such as STP, CDP, DTP or L3 routing protocols that subscribes for message advertisements

{% hint style="info" %}
Switches and routers apart from sending data between stations also constantly communicating with each other to maintain the operational services and features enabled on them, such as CDP, LLDP, STP or any routing protocol
{% endhint %}

![](<../.gitbook/assets/Unknown image (1328)>)

### Media Access Control (MAC) address

**MAC address** is a unique 12-digit fixed physical address identifying the NIC of a host

Each hexadecimal digit can be represented by 4 bits, thus each digit takes 4 bits (12\*4-48bits)

The MAC address consists of two parts, the first 24bit part is called Organizationally Unique Identifier (OUI), which is managed and assigned by the IEEE to all hardware manufacturers, so the first part of the MAC address can be used to determine the manufacturer. For example, intel has assigned A0-E7-0B. Cisco has for example 0021A1

The second 24-bit part of the mac address is assigned strategically by the manufacturer, such as the serial number of each manufactured card

There are different forms of notation, either with a period (1234.5678.9ABC), a colon (12:34:56:78:9A:BC) or a dash (12-34-56-78-9A-BC)

The Layer 2 MAC address is essential for sending a datagram in the scope of the local network or between a direct physical links

Layer 2 defines how data is formatted for transmission and how access to the physical media is controlled. Layer 2 devices provide an interface with the physical media. Some common examples are network interface cards (NICs) installed in a host.

All devices that are connected to an Ethernet LAN have MAC-addressed interfaces. The NIC uses the MAC address in received frames to determine if a message should be passed to the upper layers for processing. The MAC address is permanently encoded into a ROM chip on a NIC

A source host can communicate directly (without a router) with a destination host only if the two hosts are on the same subnet. If the two hosts are on different subnets, the sending host must send the data to its default gateway, which will forward the data to the destination. The default gateway is an address on a router (or Layer 3 switch) connected to the same subnet that the source host is on. The router also continues to examine and rewrite the MAC address to get the datagram through each physical link in the path

![](<../.gitbook/assets/Unknown image (1329)>)

{% hint style="info" %}
In Ethernet, MAC address octets are transmitted most-significant (high-order) first, while bits within an octet are transmitted as least significant bit (LSb) (low-order) first
{% endhint %}

**I/G and U/L bits** are located in the most significant byte of each MAC address, where the IG bit is the least significant bit in this byte and the LG bit is the second least significant bit

**Individual/Group (I/G) bit** distinguishes whether the MAC address is an individual or group address - IG bit of 0 indicates that this is a unicast MAC address, an IG bit of 1 indicates a multicast or broadcast address

**Universal/Local (U/L) bit** indicates whether the MAC is vendor assigned (0) or administratively assigned (1) - manually adjusted in configuration

There are three types of communications: Unicast, Multicast, and Broadcast. Hence, there are also three types of MAC Addresses:

**Unicast MAC Address:** A MAC Address that identifies only one NIC on the network and it is the Layer 2 destination of a Unicast transmission. 00:00:0c:43:2e:08 is an example of Unicast MAC Address.

**Broadcast MAC Address:** A MAC address that identifies all the NICs in the Layer 2 domain. The Broadcast MAC Address format is FF:FF:FF:FF:FF:FF and it is the Layer 2 destination of a Broadcast transmission.

**Multicast MAC Address:** A MAC address that identifies a group of NICs in the Layer 2 domain, and it is the Layer 2 destination of a Multicast transmission. The Multicast MAC Address always begins with 01:00:5E and the last 23 bits are calculated from the Multicast Group IP Address.

### Ethernet frame formats

#### Ethernet II frame (DIX)

**Ethernet II frame (DIX)** is the most common type used for user traffic to the internet, due to its simplicity, low overhead, and direct cooperation with Internet protocol

It's called DIX since Digital, Intel and Xerox worked together on this Ethernet format

Header Length is 26bytes (Preamble+DA+SA+Type+FCS)

Early Ethernet networks worked in shared link mode. To ensure the CSMA/CD mechanism, the minimum length of an Ethernet frame is 64 bytes and the maximum length is 1518 bytes.

64-byte minimum frame size was engineered to be long enough so that a transmitting station would still be sending the frame when a collision signal, originating from the most distant point on the network, returned. This ensured that the sending station would always detect the collision and retransmit the frame.

Early network hardware had very limited memory. A smaller maximum frame size meant that network interface cards (NICs) and other devices required less buffer memory to store frames, which kept costs down. A 1500-byte payload was considered a reasonable compromise.

Large frames are more efficient because they reduce the overhead of headers and trailers, but they also increase the time a single station occupies the network medium. This increased latency could be an issue for time-sensitive applications and could prevent other stations from transmitting

The preamble with SFD are used for synchronization and signaling purposes at the physical layer of the OSI model. Therefore, when calculating the maximum Ethernet frame length, the preamble is not included in the total header length

So the The maximum packet length for Ethernet is typically 1518 bytes, including 14 bytes of Ethernet header and 4 bytes of CRC, leaving 1500 bytes of payload

When a VLAN tag is involved the packet length is increased to 1522 bytes

![](<../.gitbook/assets/Unknown image (1330)>)

**Preamble (7 bytes)** consist of alternating 1s and 0s pattern allowing receivers to synchronize their clock at the bit-level with the transmitter

**Start Frame Delimiter (SFD) (1 byte)** ends with 1 to indicate start of the frame information, that the device should examine and process

**Destination MAC Address (6 bytes)** Identifies the MAC address of the destination device

**Source MAC Address (6 bytes)** Identifies the MAC address of the sender

**EtherType (2 bytes)** indicates an upper layer protocol carried/encapsulated within the payload such as IPv4,IPv6, ARP, determining how the routers and switches should process the data based on the encapsulated protocol.

| 0x0800 | IPv4  |
| ------ | ----- |
| 0x0806 | ARP   |
| 0x8100 | .1Q   |
| 0x86DD | IPv6  |
| 0x22F4 | IS-IS |

**Data or Padding** transmitted data may vary in size, so the padding ensures that the data carried meets the minimum frame size

**Frame Check Sequence (FCS) (4 bytes)** contains a checksum of the data frame, which enables the detection and correction of errors in data transmission on the physical medium, that can be caused by electrical interference, or a bad NIC

**Interpacket gap (IPG)** is idle time between packets. After a packet has been sent, transmitters are required to transmit a minimum of 96 bits (12 octets) of idle line state before transmitting the next packet

![](<../.gitbook/assets/Unknown image (1331)>)

#### IEEE 802.3 with LLC

LLC is an 802.2 standard that is used as an extension to Ethernet's 802.3's standard, providing sub-layer implemented in software, ensuring frame synchronization, flow control, error checking

Length field is used to indicate how many bytes of data are following this field before the FCS

It is used also to distinguish between DIX frame and 802.3 frame as for DIX the values in this field will be higher.

The EtherType values from 1500 to 1536 are intentionally excluded so that if the value in this field is less or equal to 1500, the frame is identified and processed as 802.3 frame, and if the value in this field is greater than 1536 (0x600 Hex) then it is a DIX frame and the value is an Ethertype value.

Source Service Access Point (SSAP) and Destination Service Access Point (DSAP) have a similar function as the Ethertype. IP (SSAP) to IP (DSAP) communication would be SSAP of 06 and DSAP of 06

One bit (LSB) in the DSAP is used to indicate if it is a group address or an individual address. If it is set to zero it refers to an individual address going to a Local SAP (LSAP). One bit in the SSAP (LSB) indicates if it is a command or response packet. That leaves us with 128 possible different SAPs for SSAP and DSAP.

CTRL (Control) field is used to select if communication should be connection-less or connection-oriented

#### IEEE 802.3 with LLC and SNAP

The IEEE had problems to address all the layer 3 processes due to the short DSAP and SSAP fields in the header. This is why they introduced a new frame format called

Subnetwork Access Protocol (SNAP)

Basically this header is using the type field found in the DIX header. If the SSAP and DSAP is set to 0xAA and the Control field is set to 0x03 then SNAP encapsulation will follow. SNAP has a five byte extension to the standard 802.2 LLC header and it consists of a 3 byte OUI and a two byte Type field.

From a vendor perspective this is good because then they can have an OUI and then create their own types to use. If we look at PVST+ BPDUs from a Cisco device we will see that they are SNAP encapsulated where the organization code is Cisco (0x00000c) and the PID is PVSTP+ (0x010b). CDP is also using SNAP and it has a PID of CDP (0x0200).

![Ethernet-Frame-Header-Types-ipcisco](<../.gitbook/assets/Unknown image (1332)>)

| **Frame type**  | **Ethertype or length** | **Payload start two bytes** |
| --------------- | ----------------------- | --------------------------- |
| Ethernet II     | ≥ 1536                  | Any                         |
| IEEE 802.2 LLC  | ≤ 1500                  | Other                       |
| IEEE 802.2 SNAP | ≤ 1500                  | 0xAAAA                      |

![](<../.gitbook/assets/Unknown image (1333)>)

### CAM table entry types

**STATIC** is manually entered unicast MAC address in the switch configuration or any control plane well-known multicast or broadcast MAC address used for protocol negotitation such as STP, LLP or even broadcast. Static entries don't age and they are retained in the table even when switch rebots

![](<../.gitbook/assets/Unknown image (1334)>)

| Switch(config)# mac address-table static c2f3.220a.12f4 vlan 4 interface gigabitethernet 1/0/1 |   |
| ---------------------------------------------------------------------------------------------- | - |

**DYNAMIC** are automatically learned MAC addresses by processing frames

If there are multiple MAC addresses learned on one port it indicates that this port is either trunk or server, that hosts virtual machines. Default aging-time for MAC address is 300s (5min)

| Switch(config)#mac address-table aging-time 500 vlan 2 | can be disabled by setting this to zero           |
| ------------------------------------------------------ | ------------------------------------------------- |
| no mac address-table learning vlan 10,12-14            | to disable mac address learning for specific vlan |
| clear mac address-table dynamic                        |                                                   |

![](<../.gitbook/assets/Unknown image (1335)>)

### Layer 2 forwarding (switching process)

Switch performs three main functions: Learning, Forwarding and participates in control plane mechanisms such as STP, CDP, LLDP

{% hint style="info" %}
Switch also drops the frame if the FCS is incorrect or he has special security feature to drop given frame
{% endhint %}

Learning

When a switch receives a frame it first examine the source MAC address in L2 header and verifies whether this MAC address is in his CAM table

If the source MAC is not in the table, it will add it to it's the CAM table together with the port on which this frame was received

Next he examines the destination MAC address, if the destination is his own MAC address he proceeds to process it in the CPU, since it can be meant for specific control protocol that he has been enabled for.

| Step | Action                                                                                                                                                                                                                                                                        |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | The switch receives a frame from PC A on port 1.                                                                                                                                                                                                                              |
| 2    | The switch enters the source MAC address (of PC A) and the switch port that received the frame into the MAC table.                                                                                                                                                            |
| 3    | The switch checks the table for the destination MAC address (of PC B). Because the destination address is not known, the switch floods the frame to all the ports except the port on which it received the frame. In this example, both PC B and PC C will receive the frame. |
| 4    | The destination device with the matching MAC address (PC B) replies with a unicast frame addressed to PC A.                                                                                                                                                                   |
| 5    | The switch enters the source MAC address of PC B and the port number of the switch port that received the frame into the MAC table. The destination address of the frame (PC A) and its associated port are found in the MAC table.                                           |
| 6    | The switch can now forward frames between the source and destination devices (PC A and PC B) without flooding because it has entries in the MAC table that identify the associated ports.                                                                                     |

![](<../.gitbook/assets/Unknown image (1336)>)

![](<../.gitbook/assets/Unknown image (1337)>)

![](<../.gitbook/assets/Unknown image (1338)>)

![](<../.gitbook/assets/Unknown image (1339)>)

![](<../.gitbook/assets/Unknown image (1340)>)

![](<../.gitbook/assets/Unknown image (1341)>)

#### Forwarding behavior

**Known Unicast** If the switch has the destination MAC address in its MAC address table, it Forwards the frame to the appropriate port where the device is connected

**Unknown Unicast** if the switch does not have the destination MAC address in its CAM table, it copies and Floods the frame unchaged to all ports (except the one where it was received) to send it to all stations. Once the frame is received by the destination station with corresponding MAC address, it responds to the source station directly with unicast and during the process, the switch learns and adds the MAC address to it's CAM and forwards it to the source station

**Multicast**

If the switch receives Multicast for a control protocol that he was enabled for, he proceeds to send it to it's CPU, to process it and update it's control protocol (such as CDP, STP)

If the switch receives a frame with a well-known reserved multicast address of control protocols, it sends it to all ports except the one on which it received it. The reason for this is to send the frame to all devices, so that the devices that listen on that well-known reserved multicast address receive them. For example, OSPF, HSRP, VRRP or any IPv6 well-known multicast

Multicast for a specific group is handled by IGMP, which ensures that the multicast is send only out of ports, where the devices that subscribed for the multicast are connected to

**Broadcast** when a broadcast frame is received (e.g., ARP requests or DHCP requests), the switch forwards the frame out of all interfaces except the one where it was received on, ensuring it reaches all devices on the LAN segment.

{% hint style="info" %}
Broadcast differs from flooding as it is intentionally sent to all devices in the broadcast domain to the reserved MAC address FF:FF:FF:FF:FF:FF, while flooding is action that the switch perform when it receives frame with Broadcast or unknown unicast MAC address
{% endhint %}

Broadcast Radiation is accumulation of broadcast and multicast traffic on a computer network that may be cause by loop topology or excessive control plane traffic

### Switching modes

In order for the switch to forward the received frame to the correct outgoing port, it must read at least the first 6 bytes of the frame to determine the destination MAC address

**Store-and-Forward switching** upon reception switch store the entire frame in the buffer to perform FCS calculations to verify the integrity of the frame, before sending it to the destination, which slows down the switching performance

**Cut-Through Mode (Fast-Forward) switching** the switch starts forwarding the frame to outgoing port as soon as it examines the destination MAC address in the first 6bytes of the frame

This provides the lowest latency and fastest data transmission, however, it does not check the entire frame for errors, and invalid frames may still be forwarded to the next node

**Fragment-Free switching** the switch reads the first 64 bytes of the frame before making a forwarding decision, offering a balance between latency and integrity

The theory here is that frames that are damaged by collisions are often shorter than the minimum valid Ethernet frame size of 64 bytes. If the frame is less than 64 bytes, it is discarded

Frames that are smaller than 64 bytes are called runts; this is why fragment-free switching is sometimes called “runt-less” switching.

![](<../.gitbook/assets/Unknown image (1342)>)

The reason modern switches default to **Store and Forward** is that the increase in network speeds (10, 40, and 100 Gbps) has made the latency difference between these modes negligible. The millisecond or microsecond delay from storing the entire frame is insignificant compared to the reliability gained from validating every single frame. The priority has shifted from raw speed to network integrity and stability.

Adaptive switching dynamically selects between cut-through and store and forward behaviors based on current network conditions

**Adaptive switching** is a concept, not a widely implemented feature on modern switches. It's a theoretical mode designed to combine the low latency of **cut-through** with the error-checking reliability of **store and forward**. The idea is that the switch would operate in cut-through mode under normal, low-error conditions to minimize latency. If it detects a high rate of corrupted frames, it would then "adapt" and switch to store and forward mode to prevent the propagation of bad data

### Can devices communicate only via Layer 2?

Since most of current applications and operating systems support TCP/IP stack to send data between systems, also they expect that the hosts communicate over distinct network, thus they are not designed to support communication over layer 2

The exception is for example routing protocol IS-IS which is designed to send data between systems with only Layer 2 address

Moreover, if such application would be invented, it would be able to communicate between systems on the same segment

### Template interface

**Template interface** is a container of configurations or policies that can be applied to specific ports

| template interface (config-template)# switchport mode access (config-template)# switchport access vlan 10 (config-template)# switchport port-security ... (config-template)# dot1x.... (config-template)# spanning-tree portfast (config-template)# source template user-template1 |   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

### Slow protocols

**Slow protocols** are a set of Layer 2 protocols designed to operate at a slower, more deliberate pace than standard Ethernet traffic. They are essential for network management, operation, and troubleshooting. These protocols have a unique multicast MAC address as their destination, **01-80-C2-00-00-02**, which ensures they are handled specifically by network devices and are not forwarded like regular data frames.

The key point is that this destination address tells network equipment—like switches—to stop the frame and process it, rather than forwarding it to its destination. This prevents slow protocol frames from flooding the network.

Some of the common protocols that use this specific destination MAC address are:

* **Link Aggregation Control Protocol (LACP):** Used to bundle multiple physical links into a single logical link, improving bandwidth and providing redundancy.
* **Link Layer Discovery Protocol (LLDP):** Allows network devices to advertise their identity, capabilities, and neighbors on a local network.
* **Multiple Spanning Tree Protocol (MSTP):** An advanced version of the Spanning Tree Protocol (STP) used to prevent network loops.
* **Ethernet OAM (Operation, Administration, and Maintenance):** This is the one you mentioned. It is a set of tools for monitoring and troubleshooting Ethernet networks. The OAM PDUs (Protocol Data Units) use this multicast address for functions like link fault management and remote loopback

### Cisco Discovery Protocol (CDP)

**Cisco Discovery Protocol (CDP)** is a discovery protocol that provides information about directly connected Cisco devices such as IP, port, hostname, version. Enabled by default (no cdp run) - useful for troubleshooting

Information provided by the Cisco Discovery Protocol about each neighboring device:

Device identifiers: For example, the configured hostname of the device

Address list: Up to one network layer address for each protocol that is supported

Port identifier: The identifier of the local port (on the receiving device) and the connected remote port (on the sending device)

Capabilities list: Supported features—for example, the device acting as a source-route bridge and also as a router

Platform: The hardware platform of the device—for example, Cisco 4000 Series Routers

Includes rapid error tracking, for example indicates if switches have mismatched native VLAN

Also used for other communication like PoE negotiation. Can be enabled/disabled globally or per port

Each CDP-configured device sends periodic advertisements to CDP multicast MAC address 01:00:0c:cc:cc:cc. Advertisements are sent with no reply required

**CDP timer** (packet update frequency) 60 seconds

**CDP holdtime** (before discarding) 180 seconds

{% hint style="info" %}
For IOS-XR you must first install the cdp from the additional RPM package, the lldp is already included in the IOS-XR software
{% endhint %}

| cdp run                                                     | to enable cdp globally - enabled by default - current version is 2 |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| (cofig-if)#cdp enable                                       | to enable it under an interface                                    |
| show cdp \[ entry \| neighbors} \[ interface-id] \[ detail] | when IPv6 is used all tyes of addresses are displayed              |

### Link Layer Discovery Protocol (LLDP)

**Link Layer Discovery Protocol (LLDP)** is a standardized, vendor-independent discovery protocol that discovers neighboring devices from different vendors. The IEEE standardized this protocol as the 802.1AB standard. LLDP performs functions that are similar to Cisco Discovery Protocol.

This protocol runs over the data link layer, which allows two systems running different network-layer protocols to learn about each other.

LLDP supports a set of attributes that it uses to discover other devices. These attributes contain type, length, value (TLV) descriptions. TLVs are blocks of information embedded in LLDP advertisements, giving details about optional information elements such as IP address, Device ID, and Platform. LLDP devices can use TLVs to send and receive information to other devices on the network. Using this protocol, devices can advertise details such as configuration information, device capabilities, and device identity.

Some of the TLVs that are advertised by LLDP:

Management address: the IP address used to access the device for management (configuring and verifying the device)

System capabilities: different hardware and software specifications of the device

System name: the hostname that was configured on that device

LLDP has these configuration guidelines and limitations:

Must be enabled on the device before you can enable or disable it on any interface

Is supported only on physical interfaces

Can discover up to one device per port

Can discover Linux servers

| R1(config)# \[no] lldp run                                           | To enable or disable LLDP globally, use the following command:                                                                                                                                                                                                                                                                                            |
| -------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| R1(config-if)# \[no] lldp transmit R1(config-if)# \[no] lldp receive | To enable or disable LLDP on an interface, use the following commands: After you globally enable LLDP, it is enabled for transmit and receive on all supported interfaces by default. The lldp transmit command enables the transmission of LLDP packets on an interface. The lldp receive command enables the reception of LLDP packets on an interface. |
| show lldp neighbors                                                  |                                                                                                                                                                                                                                                                                                                                                           |

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

### HDLC (legacy)

**HDLC (legacy)** is a synchronous serial data link protocol commonly used for P2P or P2-Multipoint communication over Serial links

The flag sequence marks the beginning and end of the frame and helps synchronize the sender and receiver.

A major drawback of HDLC is that there isn’t a field specified to identify the layer three protocol which has been encapsulated. Therefore several other protocols have been defined, based on HDLC, thus the HDLC is usually implemented with PPP

Normal Response Mode (NRM) one device acts as the primary station and controls the communication with secondary stations

Asynchronous Balanced Mode (ABM) allows multiple devices to communicate in a peer-to-peer fashion without a designated primary station.

![](<../.gitbook/assets/Unknown image (1347)>)

Flag (8 bits) - used to mark the beginning of a frame.

Address (8+ bits) - specifies the destination address, even though HDLC is usually just used for point to point links with a single address.

Control (8 or 16 bits) – Not commonly used today.

Information (variable) – the data to be sent, usually in multiples of 8 bits.

FCS (16 or 32 bits) – A CRC (Cyclic Redundancy Check) frame check sequence to identify data errors.

![HDLC Protocol and Encapsulation method Explained](<../.gitbook/assets/Unknown image (1348)>)

#### MTU (serial links)

For serial links, the default MTU (Maximum Transmission Unit) is typically 1500 bytes. This value is common across many types of serial interfaces on Cisco routers, especially when using protocols like HDLC (High-Level Data Link Control) or PPP (Point-to-Point Protocol)

### PPP (legacy)

**PPP (legacy)** is a data link layer protocol commonly used to establish a direct connection between two network nodes, typically over a serial link

It provides a reliable and efficient means of transmitting data over point-to-point links.

Widely used for dial-up connections, leased lines, and other serial link connections.

It encapsulates various network layer protocols, such as IP (Internet Protocol), allowing them to be transmitted over the point-to-point link.

PPP frames consist of a header, payload, and trailer

The header includes control and addressing information, while the trailer includes a frame check sequence (FCS) for error detection.

It uses a Link control protocol (LCP) to establish a link, configuration, and maintenance.

PPP can use different authentication protocols, such as PAP (Password Authentication Protocol) and CHAP (Challenge Handshake Authentication Protocol), to verify the identity of the connecting devices.

![](<../.gitbook/assets/Unknown image (1349)>)

In Cisco IOS, a dialer interface is a logical interface typically used to support dynamic connection methods like PPPoE (Point-to-Point Protocol over Ethernet) or ISDN dial-up. It abstracts the physical interface and handles connection parameters, authentication (e.g., CHAP), IP assignment, and virtual encapsulation

The physical interface (e.g., GigabitEthernet0/0) is assigned to the dialer pool, and the actual IP address is assigned to the dialer interface after PPP negotiation.

When you ping your own public/WAN IP (the one assigned to your Dialer interface via PPPoE), the packet is sent outward to the provider’s PE (Provider Edge) router — it must exit your device, traverse the provider’s network, and then return back to you, just like if it were targeting any external IP. So when you ping your own public IP, you are not testing internal IP stack reachability

interface Dialer1

ip address negotiated

encapsulation ppp

dialer pool 1

ppp chap hostname user1

ppp chap password 0 secret

### PPPoE (PPP over Ethernet)

**PPPoE (PPP over Ethernet)** combines two widely accepted standards, Ethernet and PPP, to provide an authenticated method

of assigning IP addresses to client systems. PPPoE clients are typically personal computers connected

to an ISP over a remote broadband connection, such as DSL or cable service. ISPs deploy PPPoE because

it supports high-speed broadband access using their existing remote access infrastructure and because it

is easier for customers to use.

PPPoE provides a standard method of employing the authentication methods of the Point-to-Point

Protocol (PPP) over an Ethernet network. When used by ISPs, PPPoE allows authenticated assignment

of IP addresses. In this type of implementation, the PPPoE client and server are interconnected by Layer

2 bridging protocols running over a DSL or other broadband connection.

PPPoE is composed of two main phases:

• **Active Discovery Phase**—In this phase, the PPPoE client locates a PPPoE server, called an access

concentrator. During this phase, a Session ID is assigned and the PPPoE layer is established.

• **PPP Session Phase**—In this phase, PPP options are negotiated and authentication is performed.

Once the link setup is completed, PPPoE functions as a Layer 2 encapsulation method, allowing data

to be transferred over the PPP link within PPPoE headers.

At system initialization, the PPPoE client establishes a session with the access concentrator by

exchanging a series of packets. Once the session is established, a PPP link is set up, which includes

authentication using Password Authentication protocol (PAP). Once the PPP session is established, each

packet is encapsulated in the PPPoE and PPP headers.
