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

# Packet Forwarding

How routers/switches split data, control, and management plane processing.

In modern forwarding architectures data, control and management device functions are separated into individual layers or parts

This separation eliminates the need for individual device to perform complex control plane functions such as forwarding calculations, their advertisements, so that the device can perform and dedicate its computing power solely to its main function, which is to forward traffic

In [SDN](onenote:Architecture.one#Network%20Designs\&section-id={DB9CE639-F13F-4E99-AA12-1A96859BBE4A}\&page-id={E744E7CB-72B4-42D9-9C17-EDEFD9FA39BC}\&object-id={87611982-E8FE-09F4-1D89-B832A9DB50F3}\&F\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE) Control plane and Management plane functions are performed by dedicated device or software running on a server or in a cloud

### Data plane

physical layer encompassing individual forwarding components like ASICs and physical interfaces (Ethernet, Fiber optic, Serial), which are responsible for forwarding of the end-station, user-generated packets . The data plane components are programmed by the control plane

Data plane handles all tasks involved in packet forwarding, that is routing, switching, de/encapsulation, NAT, ACL policy enforcement (such as discarding unwanted packets)

### Control plane

layer responsible for managing and processing network control functions, enabling the creation, operation, and maintenance of the network such as:

Forwarding calculations and routing policy processing.

Communication with routing protocols like OSPF, BGP, and EIGRP to compute the Routing Information Base (RIB).

Managing CAM learning, ARP querying, and Spanning Tree Protocol (STP) control mechanisms.

Distributing routing policies and calculations to the Data Plane.

Control Plane packets are those generated or received by the network device for managing the network itself. These packets always have a receive destination IP address and are handled by the CPU in the route processor of the device

![Control vs Data Plane](<../.gitbook/assets/Unknown image (67)>)

### Management plane

consists of functions that achieve the management goals of the network. Such goals include interactive management sessions using Secure Shell (SSH), and statistics gathering with Simple Network Management Protocol (SNMP) or NetFlow

Packets that are generated or received by a network device, or generated or received packets by a management station that are used to manage the network. From the perspective of the network device, management plane packets always have a receive destination IP address and are handled by the CPU in the network device route processor.

![](<../.gitbook/assets/Unknown image (68)>)

![](<../.gitbook/assets/Unknown image (69)>)

## Address Resolution Protocol (ARP)

In modern networks, applications communicate using IP addresses (Layer 3), but actual data transmission occurs at Layer 2 (Ethernet, Wi-Fi, etc.). Since Ethernet frames require MAC addresses to be forwarded over a local network, there must be a mechanism to map an IP address to a MAC address before any communication can take place.

For this purpose an automatic mechanism called **ARP** is used to send a request message for the MAC address of the owner of the IP address

**ARP** is a L2 protocol that serves as a bridge between the L2 and L3 layers and dynamically translates IP address to MAC address and vice versa

All devices in the network maintain ARP cache/table, where they store all resolved all MAC-to-IP address mappings, so that they can construct layer 2 header for subsequent packets without initiating again the broadcast ARP process. Because ARP is a Layer 2 protocol, its scope is limited to the LAN.

Each entry, or row, of the ARP table, has a pair of values—an IPv4 address and a MAC address.

If no device responds to the ARP request, then the original packet is dropped because a frame to put the packet in cannot be created without the destination MAC address.

![](<../.gitbook/assets/Unknown image (709)>)

### ARP process

The sender first check their ARP cache/table. If the MAC address is not in the cache, they send an ARP request as a broadcast with destination IP 255.255.255.255 and MAC FFFF:FFFF:FFFF inquiring about the owner of the MAC address of a given IP in a local subnet. The device owning the IP responds with an unicast ARP reply, updating the requester's ARP cache.

Now the sender can construct the layer 2 header and send the data directly to the owner/destination IP.

If the IP is outside of the local network, the host must first resolve the MAC of it's default gateway, which will respond in the same manner.

[The default gateway/router then receives such packet and determines the next-hop router to reach to the destination. Once it determines the next-hop, it overwrites the destination MAC address with the next-hop router MAC and sends it. The process is repeated until the packet reaches the final destination IP.](onenote:#Routing\&section-id={6EC9C333-3E04-41C1-8C01-0E8D85C128A8}\&page-id={23330675-9420-47AE-8368-54C860A78BB9}\&object-id={52581BD7-7745-0862-3434-AF316314E545}&7C\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE/L3.one)

{% hint style="info" %}
ARP request is the reason to why first ping to specific IP address fails, because the device initially needs to perform the ARP to resolve what MAC address the destination IP owns
{% endhint %}

![](<../.gitbook/assets/Unknown image (710)>)

| arp -a                  | to check ARP table on Windows PC The device creates and maintains the ARP table dynamically, adding and changing address relationships as they are used on the local host. The entries in an ARP table expire after a while; the default expiry time for Cisco devices is 4 hours. Other operating systems (Windows, macOS) might have a different value; Windows uses a random value between 15 and 45 seconds. This timeout ensures that the table does not contain information for systems that may be switched off or moved. When the local host wants to transmit data again, the entry in the ARP table is regenerated through the ARP process. |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show arp \| show ip arp | to check ARP table on Cisco IOS device; ARP entries will age out after 4hours in Cisco IOS. Cisco IOS adds random jitter between 0 to 30minutes to the timeout counter of each ARP entry to prevent all ARP entries from expiring at the same time, causing ARP storm, that floods the network with ARP requests                                                                                                                                                                                                                                                                                                                                      |
| arp arpa                | to manually configure ARP entry                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| clear arp               | clears the arp table, the device will send a unicast ARP request to try to refresh each entry before removing it from the table                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

### ARP packet format

![](<../.gitbook/assets/Unknown image (711)>)

The host's MAC address appears twice. Once in the Ethernet frame as a source and once in the payload field (Info). It appears in the source field because the request message is a broadcast sourced from the host. However, the destination cannot learn the MAC address from the frame field because it is discarded during the decapsulation process. Therefore, the MAC address of the

host is also put in the ARP payload, so the ARP protocol on the destination device can retrieve the MAC address and store it in its ARP cache.

![](<../.gitbook/assets/Unknown image (712)>)

Hardware type (HTYPE)

This field specifies the network link protocol type. Example: Ethernet is 1.

Protocol type (PTYPE)

This field specifies the internetwork protocol for which the ARP request is intended. For IPv4, this has the value 0x0800. The permitted PTYPE values share a numbering space with those for EtherType.\[2]\[3]

Hardware length (HLEN)

Length (in octets) of a hardware address. Ethernet address length is 6.

Protocol length (PLEN)

Length (in octets) of internetwork addresses. The internetwork protocol is specified in PTYPE. Example: IPv4 address length is 4.

Operation

Specifies the operation that the sender is performing: 1 for request, 2 for reply.

Sender hardware address (SHA)

Media address of the sender. In an ARP request this field is used to indicate the address of the host sending the request. In an ARP reply this field is used to indicate the address of the host that the request was looking for.

Sender protocol address (SPA)

Internetwork address of the sender.

Target hardware address (THA)

Media address of the intended receiver. In an ARP request this field is ignored. In an ARP reply this field is used to indicate the address of the host that originated the ARP request.

Target protocol address (TPA)

Internetwork address of the intended receiver.

![](<../.gitbook/assets/Unknown image (713)>)

### Reverse Address Resolution Protocol (RARP)

**Reverse Address Resolution Protocol (RARP)** is requesting the IP for a known MAC address, which can happen in diskless booting scenarios where a computer needs to obtain its IP address and other network configuration information (such as subnet mask, default gateway, etc.) from a server. During the boot process, the computer sends out a RARP request containing its MAC address to request its IP address from a RARP server.

### Gratuitous ARP (GARP)

**Gratuitous ARP (GARP)** is an ARP reply message sent without being requested with an ARP request serving as a mechanism to inform the network about a change

It is sent to broadcast MAC address (unlike the regular ARP reply that is unicast) to update ARP table of all hosts

This can happen when router is announcing an interface MAC address state change, when failover between redundant devices in FHRP occurs

It updates the switches MAC address tables and host ARP tables.

![](<../.gitbook/assets/Unknown image (714)>)

### Proxy ARP

When a device wants to communicate with another device on a different network segment, it relies on its default gateway (usually a router) to forward the traffic

In such cases, proxy ARP allows a router to answer ARP requests where the target IP address that is in a different network, that the router can reach. It is enabled by default

IP local proxy ARP allows the router to respond to ARP requests on behalf of devices in the same subnet as the device that sent the ARP request.

So instead of the traffic going directly between hosts in the same subnet, traffic will be sent to the router first

![](<../.gitbook/assets/Unknown image (715)>)

## Maximum Transmission Unit (MTU)

defines the maximum amount of data that can be transmitted in a single frame, including the IP header and payload. It represents the largest packet size that can be sent over specific link without needing to be fragmented

Although the maximum length of an IPv4 datagram is 65535, most transmission links enforce a smaller maximum packet length limit such as 1500 for [Ethernet ll Frame Size](onenote:L2.one#L2%20-%20Data-Link\&section-id={EE338BC5-FE62-4714-B561-D35D9358E092}\&page-id={C107D1F8-6395-4825-A10A-0E6128CC4AA3}\&object-id={F5AD8E93-66EF-0E9D-3C6A-991FBC4A1D77}&3F\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE)

Destination-based routing results in IP packets being routed independently of each other - each router in the paths decides where to route the packet

As a result, we have different packets between the same end hosts that could take different routes with varying MTU sizes.

This is solved with fragmentation integrated within IPv4/6 headers

If a host wants to send a packet larger than the MTU for a network, the packet must be fragmented by Layer 3 (since Layer 2 doesn't offer any fragmentation capabilities)

#### **IP fragmentation**&#x20;

is the process of splitting an IP packet into smaller fragments when the packet size exceeds the MTU of a link along the path. In IPv4, fragmentation can occur either at the source host or at intermediate routers if the packet is larger than the outgoing interface MTU and the DF (Don’t Fragment) bit is not set. Each fragment is forwarded independently through the network and carries its own IP header, including the same Identification field, a Fragment Offset indicating its position within the original packet, and the MF (More Fragments) flag to indicate whether more fragments follow. Routers do not maintain any state about fragmented packets and simply forward fragments like any other IP packet. Reassembly is always performed only at the final destination host, which collects all fragments belonging to the same original packet using the Identification field, orders them using the Fragment Offset, and determines completion using the MF flag. Intermediate routers never reassemble fragments because this would require maintaining per-flow state and significantly reduce scalability and forwarding performance.

In IPv6, the behavior is different because routers are not allowed to fragment packets at all. Fragmentation can only be performed by the source host using a Fragment Extension Header, and if a packet exceeds the MTU of any link along the path, the router drops the packet and sends an ICMPv6 “Packet Too Big” message back to the sender. The sender then relies on Path MTU Discovery to adjust packet sizes dynamically. This design eliminates fragmentation overhead in the network core and improves forwarding efficiency. As a result, modern networks generally try to avoid fragmentation altogether by using PMTUD and techniques such as TCP MSS clamping, especially in environments with tunnels or encapsulation where effective MTU is reduced.

#### Path MTU

the smallest MTU on a device in the forwarding path determines the MTU on the entire forwarding path between the source and destination

This may be the same or smaller then the MTU of the interface the host sends the packet from.

![Path MTU](<../.gitbook/assets/Unknown image (70)>)

If a host sends a 2000-byte packet with a 1500-byte MTU, fragmentation occurs. The first fragment includes a 20-byte IP header and 1480 bytes of data

Subsequent fragments contain the remaining data

Fragments are reassembled at the destination using the More Fragments (MF) flag and fragment offset to restore the original packet accurately.

Ideally each interface of a network device along the entire path from the source to the destination should have the same MTU, MTU can be adjusted under interface config-level

{% hint style="info" %}
Upload/download speed is considerably affected by the MTU along the path.
{% endhint %}

**Maximum receive unit (MRU)** is the largest packet size that an interface can receive

| show system mtu | System MTU allows to configure system default MTUs. If no particular MTU value is configured on an interface, the System MTU applies to view all MTU settings on a switch |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### Larger MTU size effect on Network

More data can be put into fewer packets with a bigger MTU, allowing for faster and more efficient transmission with lower overhead, since less packets are required to be forwarded and processed. However larger packets take up more bandwidth, causing subsequent packets to be delayed

#### Smaller MTU size effect on Network

After decades of development, the Ethernet speed has increased from 10 Mbit/s to hundreds of Gbit/s. In such high-speed data transmission, if the maximum Ethernet frame length is still 1518 bytes, a large number of data packets are transmitted per second. Each data packet needs to be encapsulated and processed by each network device, introducing high extra overhead. The overhead becomes more noticeable as the network speed increases.

Therefore, some vendors put forward the concept of jumbo frame, which extends the maximum Ethernet frame length to 9 KB

**MTU Mismatch:** If one side of the connection (e.g., a router) has an MTU set to 1500 bytes, but the other side (e.g., a switch or host) has an MTU set to 1400 bytes, packets larger than 1400 bytes will have to be fragmented or dropped, depending on the configuration. This can lead to inefficiencies and even connection issues.

#### Types of MTU

![](<../.gitbook/assets/Unknown image (71)>)

**Layer 2 (L2) or Ethernet MTU** specifies the maximum transmittable size of the packet, measured from the ethernet header until the end of the packet. The default value of L2 MTU for a main interface is 1514 bytes. This value is configurable with the (config-if)#mtu command in the sub-config mode as well as other type of MTU's

**MPLS MTU** which specifies the maximum transmittable size of the packet measured from the MPLS labels until the end of the packet. This value is applicable only for labeled packets. The default value of MPLS MTU is L2 MTU subtracted by 14 bytes which is the size of the ethernet header of the main interface. You can configure the MPLS MTU with the mpls mtu command.

{% hint style="info" %}
mpls mtu parameter does not enforce a hard forwarding limit. The actual packet forwarding limit is determined by the interface MTU.

mpls mtu mainly affects control-plane signaling and internal MPLS calculations, not the physical packet transmission.&#x20;

mpls mtu is primarily used for:

MPLS control-plane signaling (for example LDP capability negotiation)

Internal MPLS calculations, such as determining maximum label stack depth

Ensuring the router does not attempt to impose a label stack that would exceed the expected MPLS payload size

It does not override the physical MTU enforcement in the data plane.
{% endhint %}

**IP MTU** is used to set the MTU size of an IP packet, excluding the Layer 2 header (which refer to the actual MTU of the interface).

When changing interface MTU, the IP and other protocol MTU is modified automatically to match the new MTU, the IP or other MTU

However, the reverse is not true; changing the IP MTU value has no effect on the value for the mtu command - it can be less than or equal to the Ethernet MTU, but not higher

Layer 4 usually considers the standard Ethernet MTU and reflects it to it's Maximum Segment size (MSS) and segments the data accordingly, however, devices along the path may have lower MTU than the default, so if that happens, the IP employ it's own mechanism called Fragmentation to break the data even further to be able to encapsulate and send them over the link with lower MTU.

**DF bit** is not set = packets larger than the IP MTU are fragmented

**DF bit** is set = packets larger than the IP MTU are dropped

An example of requirement for IP fragmentation is when a UDP application that is not aware of a path MTU sends large packets to a destination

Since a single packet is broke down into two smaller pieces, each one of them has to be encapsulated and processed, which increases overhead and put more processing load to transit routers, affecting performance adversely

TCP on Layer 4 notices the Fragmentation performed by the IP (from the ICMP message) and proceeds to adjust it's MSS to align with the lowest MTU size along the path

The IP and Layer 4 protocol verifies, whether the data received are complete, if not, the Layer 4 protocol ensures retransmission, since the IP protocol does not have any retransmission mechanism

Both MTU size and TCP MSS can be adjusted on the network gateway such as router in scenarios like when when GRE and IPsec technology is implemented

This is because both GRE and IPsec adds additional headers to the packet to achieve their purpose

| ping 8.8.8.8 -l 1500 -f                                                                       | -l size -f don’t fragment bit set                                                                      |
| --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| for /l %i in (1400,1,1500) do @ping -f -n 1 -l %i -f [www.google.com](http://www.google.com/) | ping test to determine the MTU value that the network is capable to send packets without fragmentation |

![](<../.gitbook/assets/Unknown image (72)>)

![](<../.gitbook/assets/Unknown image (73)>)

{% hint style="info" %}
If you generate a 9000 byte packet from an SVI (which has an MTU of 1500 because it is bound to a physical interface or bundle), fragmentation will occur on the output interface of the first box that tries to send the packet.
{% endhint %}

#### Fragmentation in IPv6

IPv6 requires that the link layer support a minimum MTU size of 1280 bytes

In IPv4, fragmentation is done whenever required, at the destination or routers

IPv6 takes a different approach. It assumes routers don't fragment packets so the source must perform PMTUD to determine the MTU for the entire path

When a packet hits an interface with a smaller MTU, the intermediary routers doesn't perform fragmentation, instead they send back an ICMPv6 type 2 error, known as Packet Too Big, to the sending host. The sending host receives the error message, reduces the size of the sending packet, and tries again

This strict IPv6 approach to fragmentation relieves the burden on intermediate devices, so that it does not have to waste resources for packet fragmentation

IPv6 and other extension headers are unfragmentable because every fragment has to go through nodes or routers, and at every router, information stored in these extension headers is required.

{% hint style="info" %}
There is no DF bit in IPv6. IPv6 devices drop oversized packets and reply with an ICMPv6 Packet Too Big message, so the source can adjust.
{% endhint %}

**IPv6 Virtual Fragmentation Reassembly (VFR)**

Non-initial fragments of a fragmented IPv6 packet is used to pass through IPsec and NAT64 without any examination due to the lack of the L4 header, which usually is only available on the initial fragment. The IPv6 VFR feature provides the ability to collect the fragments and provide L4 info for all fragments for IPsec and NAT64 features

#### Path MTU Discovery (PMTUD)

dynamically determines the path MTU according to the link with lowest MTU in the forwarding path between the source and destination

Whenever the Layer-4 session happens to send a data, the Layer 3 prepends to it's header DF bit and sends the datagram equal to the MTU of the it's outgoing interface

If this was an oversized datagram, a medium in the forwarding path that have lower MTU than the sender interface MTU will drop the packet since the DF bit is set, and sends ICMP Destination unreachable message back to the source, with a code "fragmentation needed" to report that the local egress MTU was exceeded and suggests the new MTU size

The MTU size reported by an intermediate router is cached as the new MTU for the destination host and all future outgoing datagrams will not exceed that MTU

The computed end-to-end MTU is a by-product of sending large datagrams and receiving ICMP replies informing the sender about the MTU that should be used

Since the end-to-end paths through the network might change with time, the hosts eventually age out the computed end-to-end MTU values (the timeout recommended in the RFC 1191 is ten minutes), resulting in renewed PMTUD process

PMTUD is enabled by default

![](<../.gitbook/assets/Unknown image (74)>)

{% hint style="info" %}
Currently, no effective method is available to discover the path MTU on an IPv4 network due to the following reasons:

ICMP traffic is usually blocked by the Internet firewalls, so the PMTUD cannot be determined properly

Some carriers or websites filter out ICMP probe packets for network security or other purposes

Path MTU detection requires cooperation between hosts and various network devices (such as switches, routers, and firewalls) on the Internet

Some network devices do not comply with RFC 1191
{% endhint %}

| permit icmp any any packet-too-big deny icmp any any fragments                                                                                                                | If you want PMTUD to work within your network                                                                                                                                                                                                              |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ip access-list extended CLEAR-DF-BIT permit udp any any                                                                                                                       | Traffic for TCP applications can be adjusted easily with MSS on the first hop router, however some UDP applications can set DF bit, which may result in packet drops. This can be adjusted with this ACL implemented in the route map for egress interface |
| route-map CLEAR-DF-BIT permit 10 description "Set the Don't Fragment (DF) bit to 0 to allow packet fragmentation" match ip address CLEAR-DF-BIT match ip address CLEAR-DF-BIT |                                                                                                                                                                                                                                                            |
| interface FastEthernet0/0 ip address 10.0.0.1 255.255.255.240 ip policy route-map ClearDF                                                                                     |                                                                                                                                                                                                                                                            |

## Layer 2 forwarding

#### Content Addressable Memory (CAM)

In the ordinary RAM, where a host or device explicitly specifies the memory location to access the content (such as double clicking on icon program or when a device wants to execute specific program while in operation), the CAM is the inverse. The device don't know the location where the content is located, but the device knows what to find (e.g it want to determine whether the exact content such as destination MAC address (binary value called key) is in the CAM table

The CAM table return binary response indicating whether the MAC address is present (1 - true) in the table or not (0 - false) with the port information (port id and vlan)

As frames arrive on switch ports, the source MAC addresses are learned and recorded in the CAM table as well as the port, where the frame has been received alongside to the VLAN and timestamp. If a MAC address learned on one switch port has moved to a different port, the MAC address and timestamp are recorded for the most recent arrival port

Then, the previous entry is deleted. If a MAC address is found already present in the table for the correct arrival port, only its timestamp is updated.

Whenever the switch receives an Ethernet frame, it will use a hashing algorithm to create a “key” for the destination MAC address + VLAN, and it will compare this hash to the already hashed information in the CAM table. This way, it is able to quickly look up information in the CAM table

The problem with CAM is that it can only do exact matches on ones and zeros (binary CAMs), and here comes TCAM.

CAM and TCAM are mainly used as an additional memory location for special applications to store a relatively small data set that needs to be searched very quickly, very often

### Forwarding hardware building blocks

#### ASICs

The general purpose CPU is optimal only for control and management plane operations, but not for data plane since data forwarding requires constant lookup into large memory tables (CAM, RIB, L4 ACLs for Security and QoS, etc.) and the processing in general purpose CPU decreases the forwarding performance

The ASICs are specialized version of CPU chips designed to perform specific tasks, such as maintaining and managing the forwarding of data in networking devices

The ASIC is programmed by the control plane in a binary format

![](<../.gitbook/assets/Unknown image (75)>)

#### FPGA

are integrated circuits are the combination of microprocessors, diodes, resistors, and transistors

FPGA is a chip that is similar to the to ASIC. Both ASIC and FPGAs are integrated on the line cards or forwarding modules within the device and ensures data plane processing tasks

The difference is that the internal circuitry in FPGA can be re-programmed to achieve certain function, thus they are not as custom and application-specific as the ASIC, but can reach almost the same performance as ASIC

Field-programmable devices (FPDs) describes any type of programmable hardware device, including FPGAs

SRAM-based FPGAs use static random-access memory (SRAM) cells to store the configuration of the programmable logic gates. These types of FPGAs are volatile, which means that the configuration is lost when power is removed.

Flash-based FPGAs use non-volatile flash memory cells to store the configuration. These types of FPGAs retain the configuration even when power is removed.

## Layer 3 forwarding

L3 forwarding decisions involve looking for most specific match in the routing table (eg. partial matches)

Layer 3 forwarding requires more processing than Layer 2, because the Router need to decrement the IP TTL, recompute the IP header checksum, change the source/destination MAC addresses and recompute the FCS before forwarding the packet

ASICs with TCAM allow to perform Layer 3 forwarding in a hardware

Software-based routers performs forwarding decisions in software, which is slow. The control and data planes are shared (not separate components of the device)

Hardware-based routers have purpose-made hardware components that handles the data plane forwarding, while the general-purpose CPU handles the control plane

CPU handles only the packets that the ASICs can't

Hybrid routers have specialized NP that performs packet forwarding (a balance between speed and programmability of the forwarding)

Network Processor (NP) is specialized microprocessors designed specifically for packet classification, forwarding, filtering, and traffic management

#### Ternary Content Addressable Memory (TCAM)

is a specialized type of high-speed memory that is designed to accelerate the process of searching for specific entries in tables to perform efficient and fast table lookups

It is used in networking to store higher layer information, where only longest/most specific matches are required instead of exact matches - those are for example RIB entries,ACL or QoS

TCAM stores data in fixed-length cells, each bit in a cell can have three states: 0, 1, or X ("don't care")

The ability to have "X" as a state enables TCAM to efficiently handle data with variable bits, to specify which bits in the data are essential for matching

Subnet masks (where some bits can be wildcards)

ACL entries (where specific fields like source/destination ports might be wildcards)

QoS entries (where specific traffic classes might be wildcards)

During a search, TCAM performs parallel bitwise comparisons between the search data and each stored cell to determine the match

Three components of TCAM Entry

**Value** refers to the actual data pattern being searched for such as IP addresses, protocol ports, DSCP values..

**Mask** is another set of bits, the same length as the Value. It defines which bits in the Value are crucial for matching the search data

0 in the mask indicates that the corresponding bit position in the data isn't considered for matching (it acts like a "wildcard")

1 in the mask indicates that the corresponding bit position must exactly match the search data

**Result** specifies the action to be taken if the search data matches the Value according to the Mask

Permit or deny access in an ACL

Route to a specific next hop in a routing table

Assign a specific QoS priority level

Example

ACL entry with a "Value" that matches a specific source IP address and a "Mask" that specifies a range of IP addresses e.g mask

The "Result" associated with this entry could be to permit or deny traffic that matches the specified IP range.

In routing tables, TCAM entries might have a "Value" that corresponds to a destination IP address and a "Mask" that defines subnet mask

The "Result" could indicate the next-hop router or interface to which the packet should be forwarded.

Entries in QoS configurations could have "Value" representing specific packet attributes (e.g., DSCP values) and a "Mask" that defines a range of values

The "Result" might determine the priority or treatment the packet should receive within the network

When a frame is received on a port, a copy of the first 200 bytes of the packet is copied to the forwarding controller, which is responsible for performing the actual lookups in the TCAM. The 200 bytes contains the necessary information required to perform forwarding decisions (VLANs, egress port(s) etc.) and determine the treatment the packet should receive in terms of applying QoS and ACL's

Feature Manager (FM) is a software component inside the switch/router that handles the compiling for example ACL's and programming them into the TCAM

Example

ip access-list extended FILTER

permit tcp 192.168.0.0 0.0.0.255 10.128.0.0 0.0.0.255 eq 80

Match 24 bits of source address AND 24 bits of destination address

First value

IP protocol: TCP

Source IP: 192.168.0.0

Destination IP: 10.128.0.0

Destination Port: 80

Associated result: Permit

Second value

IP protocol: TCP

Source IP: 10.128.0.0

Destination IP: 192.168.0.0

Destination Port: 443

Associated result: Permit

Third value

IP protocol: ANY

Source IP: 10.128.0.0

Destination IP: 192.168.0.0

Associated result: Deny

When a packet arrives, TCAM searches for a match in its entries. Each entry in TCAM corresponds to an IP access-list rule and contains the source IP address, source mask, destination IP address, destination mask, and other attributes.

TCAM performs a bitwise "AND" operation between the packet's source and destination IP addresses and their respective masks. This operation filters out irrelevant bits and leaves only the relevant network portions of the addresses.

TCAM then compares the filtered source and destination IP addresses with the corresponding fields in each entry. If both source and destination IP addresses match, TCAM checks additional attributes like the protocol type and port numbers to determine if the packet should be permitted or denied.

If a matching entry is found, TCAM returns the action associated with that entry (e.g., permit or deny). If no match is found, TCAM may return a default action defined by the network administrator.

| show platform tcam utilization                                |   |
| ------------------------------------------------------------- | - |
| show platform hardware qfp active tcam resource-manager usage |   |

TCAM is integrated in the ASIC which ensures a low internal signal path which in turn yields shorter handling times

![](<../.gitbook/assets/Unknown image (76)>)

**Restrictions**

CAM and TCAM do have some disadvantages. These memory cells require additional transistors to support the search feature. This makes it more expensive and less dense compared with traditional RAM. Each memory cell needs to be active on every cycle to perform the search, so it requires more power and produces additional heat.

Storing 1 bit in TCAM takes 10-12 transistors

If a router is unable to store all its routing entries in TCAM, it will fall back to slower memory, which will result in high CPU use and dropped packets

Due to its specialized use, the TCAM in a router is often much smaller than the general RAM. A smaller enterprise router might only have about 20 megabytes' worth of TCAM storage. Large internet routers in use at backbone internet service providers might need to store the entire Border Gateway Protocol table in TCAM, which is quickly approaching a million entries.

#### SDM (Switching Database Manager)

responsible for management of TCAM resources, allowing to allocate certain memory for L2 switching, L3 IPv4 or IPv6 routing or ACL,QoS...

Utilizes pre-defined templates which reserves certain processing portion for L2,L3,QoS.. operations. High-end switches and routers have separate TCAM tables for each function

Configuring the SDM template is just one single command followed by a switch reload

Enabling one feature such as ACL or QoS will usually result in a decreased capacity for other features or even disable them entirely

| show sdm prefer |                                                |
| --------------- | ---------------------------------------------- |
| sdm prefer      | changes are applied after reload of the switch |

#### MDB (IOS XR)

is an SDM of IOSXR systems used to modify router resources for the specific needs during the router boot up time

With L2MAX profiles we get more resources for applications mapped to L2 features like MAC scale, L2VPN etc.

While with L3MAX profiles we get higher resource carving for applications mapped to L3 features like routes, L3VPN etc.

| hw-module profile mdb |   |
| --------------------- | - |

### Switching / forwarding methods

#### Process switching (software)

the general-purpose CPU on a router is in charge of packet switching

ip\_input is process within IOS that runs on the general-purpose CPU and is responsible for doing the routing table and ARP table lookups to make making forwarding decisions

To find the best path for a packet's destination, process switching requires the router to scan its RIB for the longest prefix match

This involves multiple iterations if the RIB entries lack complete information, such as the outgoing interface, requiring recursive lookups until the packet can be forwarded

Routing table entries in a RIB are sorted sequentially according to the prefix lengths, so that all networks with the longest prefix length are always at the top up until to the least specific such as default route (/32 first /0 last)

Performing a lookup in this table meant traversing the table from the top, entry by entry, computing the binary AND between the packet’s destination address and the netmask in the RIB entry, and comparing the result with the network address in the entry, stopping at the first match, which came to be an very inefficient and CPU intensive operation

The routing table is a component made to build and store reachability information, not truly optimized for lookups

To optimize it, we need to change its linear structure to something more efficient to perform lookups faster, and to get rid of the recursion.

A structure meeting these requirements, truly optimized to perform fast lookups, is separate from the RIB although it is populated by its contents, and is called the FIB

![](<../.gitbook/assets/Unknown image (77)>)

#### Fast switching

uses the IP routing table for initial route lookups and stores the results in a cache, so that CPU processing is not involved for subsequent packets

#### Cisco Express Forwarding (CEF)

is a Cisco Layer 3 packet-switching technology with advanced IP forwarding algorithm

With CEF, the router builds forwarding and encapsulation information beforehand, so that it doesn't have to process each packet in the software by the CPU and instead all packets are switched by the ASIC hardware

Traditional methods of parsing the routing table involve scanning entries sequentially from the beginning longest prefix entries up until to the least specific entries, which is both time and resource intensive and can't keep up with the packet forwarding demand

CEF structure is designed more efficiently by building two additional tables, FIB and Adjacency table

{% hint style="info" %}
Anything that requires some level of "thinking" (crypto, NAT, PBR..) is processed in the software (CPU) and same applies for CEF
{% endhint %}

CEF sends the packet to the IP Input in CPU, if it can't handle complex packets such as:

Control traffic, such as BGP, OSPF, IS-IS, PIM, IGMP, ICMP

Management traffic, such as Telnet, SSH, SNMP

Layer 2 mechanisms, such as CDP, ARP (packet without arp entry), LACP PDU, BFD

Fragmentation, DF bit set, IP options set, TTL expired

| (config)# \[ip \| ipv6] cef                  | is enabled by default; for ipv6 is enabled with ipv6 unicast-routing already                    |
| -------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| (config)# \[ip \| ipv6] cef distributed      | enables distributed CEF                                                                         |
| (config-if)# ip route-cache cef              | Enables IPv4 CEF on the interface for software                                                  |
| (config-if)# ip load-sharing per-destination | Per-destination load balancing is enabled by default when you enable CEF                        |
| show ip cef                                  | show the entire FIB table                                                                       |
| show adjacency                               | To display Layer 2 information for adjacent device on an interface                              |
| show ipv6 cef summary                        | Verifies that the CEF is enabled for IPv6 and how many entries are present in the CEF table     |
| show \[ip \| ipv6] cef                       | To determine the egress port for a destination network in a FIB                                 |
| show \[ip \| ipv6] cef exact-route           | To determine the egress port for a destination network based on the specific source IP in a FIB |
| show cef linecard <> \[detail]               |                                                                                                 |
| show cef interface <>                        |                                                                                                 |

### CEF data structures

#### Forwarding Information Base (FIB)

is a database built based on the L3 information from the routing table with the next-hop IP address for each known destination network including predetermined next-hop IP of the recursive routing entries. This allows router to determine the ultimate forwarding information for a packet in a single lookup

When a routing or topology change occurs in the network, the IP routing table is updated, and these changes are reflected in the FIB

FIB is built and ordered in a way that allows it to optimize fast retrieval of information and longest prefix match lookup

Its structure is called trie, a tree-like searchable data structure comprised of nodes and leaves

The trie is composed of a root node, child nodes and keys. The root is the entry point where we start to traverse the tree, and it has pointers or links to other nodes called child nodes. As opposed to common trees in which a particular node stores the complete key, a trie stores a key (a word, or a number) by splitting it into letters or digits, and creating a node for each letter or digit on progressively deeper levels of the tree. The complete key is therefore looked up letter by letter, or digit by digit - prefix-oriented approach

In a trie data structure, each node can have a special indicator known as the terminal flag. This flag tells us if the sequence of characters from the root to this node forms a complete word. When the terminal flag is set, the node is called a terminal node.

With a trie, the idea is to store the prefix of every known destination network from the RIB in the FIB in the bit-by-bit fashion, one trie level for one bit of the prefix. Since an IPv4 prefix is at most 32 bits long, the depth of the trie will be at most 32 levels (33 if counting the root node as well). As a result, performing the longest prefix match lookup with a packet’s destination IP address will take at most 32 comparisons in the trie, no matter how many distinct prefixes are stored in it.

This is a vast improvement over a linear routing table where the number of comparisons was proportional to the number of prefixes stored.

FIB Lookup operation

1. Set the current node to the root node.
2. If the current node is a terminal node, remember it, forgetting any previously found terminal nodes.
3. Attempt to descend from the current node to its child node that matches the next letter or digit in the key we are looking up.
4. If there is no such child node, STOP. The most recently found terminal node represents the longest prefix match for the key

Otherwise, make the child node the new current node, and return to Step 2.

Example: 11100001 - 225 dec

![](<../.gitbook/assets/Unknown image (78)>)

**What happens if the network does not exist in the tree at all?**

The root can be a terminal node, too, in which case it would represent the default route. All those lookups for destination networks which we cannot find in the tree will start in the root first. If there is no other terminal node as we branch deeper into the tree, the most recently found terminal node would be the root - which is the case for a default route

#### Adjacency table

The router can start preparing the complete forwarding information - outgoing interface, frame rewrite - even before the packets start flowing.

All the necessary information is already available: The next hops are known from the RIB and their Layer2 addresses can be obtained through mechanisms that map Layer3 to Layer2 addresses (ARP and others similar tables).

The complete forwarding information compiled from this data would then be placed into a standalone database called the Adjacency table that built based on L2 information from the ARP table. The Adjacency table contain all directly connected neighbors with the corresponding outgoing port

A lookup in the FIB using the packet’s destination IP address as a key, produces a pointer into the adjacency table that contains pre-built frame headers for individual next hops, so that they can be instantly applied to the packet

Each adjacency is stored with preformed L2 information in a single-string sequence: destination MAC of the neighbor, the source MAC of the outgoing interface followed by the Ethertype

![](<../.gitbook/assets/Unknown image (79)>)

{% hint style="info" %}
If subinterface is used, the vlan tag is also prepended in the sequence:
{% endhint %}

CA022904003B - Destination MAC address - R2 Ethernet 2/3‘s MAC address

CA0320A4003B - Source MAC address - R3 Ethernet 2/3‘s MAC address

8100 - Ethertype of the 802.1Q header

0 - Class of Service (0) and Discard Eligibility Indicator (0)

064 - VLAN Tag - 100 (064 in Hex = 100 in decimal)

0800 - Ethertype of the original L2 header

#### CEF forwarding operation

Upon receipt of an IP packet, the FIB is checked for a valid entry.

If an entry is missing, the packet should go to the CPU because CEF is unable to handle it

Valid FIB entries continue processing by checking the adjacency table for each packet’s DIP

Missing adjacency entries invoke the ARP process. Once ARP is resolved, the complete CEF entry is created.

As part of the packet forwarding process, the packet’s headers are rewritten.

The router overwrites the destination MAC address of a packet with the next-hop router’s MAC address from the adjacency table, overwrites the source MAC add with the MAC add of the outgoing Layer 3 interface, decrements the IP TTL, recomputes the IP header checksum, and delivers the packet to the next-hop router

![](<../.gitbook/assets/Unknown image (80)>)

![Foundation Topics > CCNP Routing and Switching TSHOOT 300-135 Official Cert Guide: Troubleshooting Device Performance | Cisco Press](<../.gitbook/assets/Unknown image (81)>)

#### Software CEF

is used in a software-based routers, where the general-purpose CPU is in charge of packet forwarding

The FIB is processed and maintained in a RAM

It is also used as the initial processing engine in a hardware-based CEF routers

#### Hardware CEF

used in distributed forwarding architectures, where software CEF calculates and installs CEF data structures (FIB) to the TCAM of the ASIC and to ASICs on all line cards in case of modular distributed systems - called Distributed CEF (dCEF)

The RIB process is in charge of the calculation of best paths, alternative paths, and the redistribution from different protocols and all these details merge into the global RIB (gRIB), where the best path for a destination network is installed. This is further distributed into the software CEF tables of different line cards, which is further mirrored into hardware CEF

{% hint style="info" %}
When the number of routes surpasses the hardware CEF's capacity, the router switches to software-based forwarding mechanisms to ensure continued packet delivery.
{% endhint %}

Each entry in RIB will consume between approximately 200 and 280 bytes plus 44 bytes per extra path

Each LSA of OSPF database will consume a 100 byte overhead plus the size of the actual link state advertisement, possibly another 60 to 100 bytes

## Centralized vs distributed forwarding

#### Centralized forwarding

the forwarding decisions are made by the centralized RP

When a packet is received on the ingress line card, it is transmitted to the forwarding engine on the RP. The forwarding engine examines the packet’s headers and determines that the packet will be sent out a port on the egress line card, and forwards the packet to the egress line card to be forwarded

![](<../.gitbook/assets/Unknown image (82)>)

#### Distributed forwarding

the line cards are equipped with forwarding engines so that they can make packet switching decision without intervention of the RP

When a packet is received on the ingress line card, it is transmitted to the local forwarding engine. The forwarding engine performs a packet lookup, and if it determines that the outbound interface is local, it forwards the packet out a local interface. If the outbound interface is located on a different line card, the packet is sent across the switch fabric, also known as the backplane, directly to the egress line card, bypassing the RP

![](<../.gitbook/assets/Unknown image (83)>)

## Cisco FIB database design

Longest Prefix Match (LPM) database is an SRAM that stores IPv4 and IPv6 routes. Scale: variable from 128k to 400k entries. We can perform variable length prefix lookup in LPM

Large Exact Match (LEM) - in NCS Central Exact Match (CEM) database - in Cisco 8k

store specific IPv4 /32 and IPv6 routes /128, plus MAC addresses and MPLS labels. Scale: 786k entries. We perform exact match lookup in LEM

High Bandwidth Memory (HBM) - in cisco 8k

is special DRAM designed to be integrated closely with the processor or ASIC to provide deep buffering, which allows for the temporary storage of large amounts of data caused by high volumes of traffic during periods of network congestion

The close integration between ASIC and HBM ensures high data transfer rate with minimum processing delay, that can happen in other externally sourced memories

External TCAM (eTCAM) - implemented in SE "scale" NCS series

should not be confused with the 4GB external packet buffer which is present on the side of each FA, regardless the type of system or line card.

eTCAM is an externally implemented in the chip to increase the base TCAM forwarding database scale, offerring up to 2M IPv4 entries

The external packet buffer will be used in case of queue congestion only. It’s a very rapid graphical memory, specifically used for packets.

The eTCAM only handles prefixes and ACEs, not packets

Cisco 8000

![](<../.gitbook/assets/Unknown image (84)>)

| sh controllers npu voq-usage interface all instance all loc 0/rp0/cpu0 | display the assignments of physical ports to their corresponding NPU, slice, and IFG. (“IFG” stands for “interface group”, this is just another internal sub-block of the chip |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

NCS 5500

![](<../.gitbook/assets/Unknown image (85)>)

| RP/0/RP0/CPU0:NCS5500(config)#hw-module fib ipv4 scale ? host-optimized-disable Configure Host optimization by default internet-optimized Configure Internet optimized |   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| sh route sum                                                                                                                                                           |   |
| sh contr npu resources lem location 0/0/CPU0                                                                                                                           |   |
| sh contr npu resources lpm location 0/0/CPU0                                                                                                                           |   |
| show dpa resources iproute location 0/0/CPU0                                                                                                                           |   |

Host-optimized is the default option. Committing a change in the configuration will prompt you to reload the line-cards or chassis to enable the new profile.

first lookup is performed in the LEM searching for a IPv4/32 exact match

second lookup is accessing the LPM searching for a variable length match between IPv4/31 and IPv4/25

third lookup is done in the LEM again, searching for a IPv4/24 exact match

finally, the fourth lookup is checking the LPM a second time searching for a variable length match between IPv4/23 and /0

![](<../.gitbook/assets/Unknown image (86)>)

This mode is particularly useful with a large number of IPv4/32 and IPv4/24 in the routing table. It could be the case for hosting companies or data centers

![](<../.gitbook/assets/Unknown image (87)>)

Internet-optimized mode. This is a feature activated globally and not per line card. After reload, you will see a very different order of operation and prefix distribution in the various databases with base line cards and systems:

It moves the largest route population present on the Internet (IPv4/24, IPv4/23, IPv4/20) into the largest memory database: the LEM.

![](<../.gitbook/assets/Unknown image (88)>)

first lookup is in LPM searching for a match between IPv4/32 and IPv4/25

second lookup is performed in LEM for an exact match on IPv4/24 and IPv4/23.

third lookup is done in LEM too, and this time for also an exact match on IPv4/20

fourth and final step, a variable length lookup is executed in LPM for everything between IPv4/22 and /0

![](<../.gitbook/assets/Unknown image (89)>)

on scale line card, regardless of the profile enabled, the IPv4/32 are stored in LEM and not eTCAM

![](<../.gitbook/assets/Unknown image (90)>)

![](<../.gitbook/assets/Unknown image (91)>)

![](<../.gitbook/assets/Unknown image (92)>)

![](<../.gitbook/assets/Unknown image (93)>)

## Cisco IOS order of operations (ingress/egress)

Cisco IOS order of operations dictates the sequence for processing incoming and outgoing traffic, ensuring the most logical flow for security, routing, and quality of service

| **Order** | **Ingress features**                                       | **Egress features**                                |
| --------- | ---------------------------------------------------------- | -------------------------------------------------- |
| 1         | Virtual Reassembly                                         | Output IOS IPS Inspection                          |
| 2         | IP Traffic Export                                          | Output WCCP Redirect                               |
| 3         | QoS Policy Propagation through BGP (QPPB)                  | NIM-CIDS                                           |
| 4         | Ingress Flexible NetFlow (FNF)                             | NAT Inside-to-Outside or NAT Enable                |
| 5         | Network Based Application Recognition (NBAR)               | Network Based Application Recognition (NBAR)       |
| 6         | Input QoS Classification                                   | BGP Policy Accounting                              |
| 7         | Ingress NetFlow (TNF)                                      | Lawful Intercept                                   |
| 8         | Lawful Intercept                                           | Check crypto map ACL and mark for encryption       |
| 9         | IOS IPS Inspection (Inbound)                               | Output QoS Classification                          |
| 10        | Input Stateful Packet Inspection (IOS FW)                  | Output ACL check (if not marked for encryption)    |
| 11        | Check reverse crypto map ACL                               | Crypto output ACL check (if marked for encryption) |
| 12        | Input ACL (unless existing NetFlow record was found)       | Output Flexible Packet Matching (FPM)              |
| 13        | Input Flexible Packet Matching (FPM)                       | Denial of Service (DoS) Tracker                    |
| 14        | IPsec Decryption (if encrypted)                            | Output Stateful Packet Inspection (IOS FW)         |
| 15        | Crypto to inbound ACL check (if packet had been encrypted) | TCP Intercept                                      |
| 16        | Unicast RPF check                                          | Output QoS Marking                                 |
| 17        | Input QoS Marking                                          | Output Policing (CAR)                              |
| 18        | Input Policing (CAR)                                       | Output MAC/Precedence Accounting                   |
| 19        | Input MAC/Precedence Accounting                            | IPsec Encryption                                   |
| 20        | NAT Outside-to-Inside                                      | Output ACL check (if encrypted)                    |
| 21        | Policy Routing                                             | Egress NetFlow (TNF)                               |
| 22        | Input WCCP Redirect                                        | Egress Flexible NetFlow (FNF)                      |
| 23        | —                                                          | Egress RITE                                        |
| 24        | —                                                          | Output Queuing (CBWGQ, LLQ, WRED)                  |

![](<../.gitbook/assets/Unknown image (94)>)
