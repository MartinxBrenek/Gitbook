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

# MPLS L2VPN

### Overview

**MPLS Layer 2 VPNs (MPLS L2VPN)** enable service providers to offer point-to-point or multipoint Layer 2 connections between distant customer sites.

**L2VPN** is comprised of switched connections between subscriber endpoints over a shared network.

In L2 VPN the provider network appear to the customer as a switch, whereas in the L3 VPN it appear as another router in their network that connect their remote sites

The Choice of L2VPN over L3VPN Will Depend on How Much Control the Enterprise Wants to Retain. L2 VPN Services Are Complementary to L3 VPN Services

### Layer 2 VPN vs Layer 3 VPN

#### Layer 3 VPNs

SP devices forward customer packets based on Layer 3 information (e.g. IP addresses)

SP is involved in customer IP routing

Foundation for L4-7 Services

#### Layer 2 VPNs

An L2VPN is comprised of switched connections between subscriber endpoints over a shared network.

SP devices forward customer frames based on Layer 2 information (e.g. MAC)

The customer (enterprise) retains full control over Layer 3 policies, like routing, QoS (Quality of Service), and security, since the SP only transports Layer 2 frames.

Multiprotocol support, a company might have:

Site A connected to the SP via Frame Relay (FR)

Site B connected via ATM

Site C using Ethernet

This is achieved by configuring an pseudowire at both ends, which serves as an "virtual wire" between two customer locations. PE routers follow this configuration to know which traffic from which customer should be sent where and how it should be processed (encapsulated and decapsulated)

![](<../.gitbook/assets/Unknown image (1424)>)

### Metro Ethernet Forum (MEF) concepts

**Metro Ethernet Forum (MEF)** defines Carrier Ethernet (also called Metro Ethernet - MetroE) to ensure transport over WAN

The CE device is connected to the user-network interface (UNI) with typically a standard 10-/100-/1000-Mbps or 10-Gbps interface. UNI is the demarcation between the CE and the WAN. Services inside the WAN cloud can be supported by a wide range of technologies, including SONET/SDH, DWDM, Gigabit Ethernet, MPLS

**Ethernet Virtual Connections (EVCs)** are Ethernet service attributes that define an association of two or more UNIs enabling the transfer of Ethernet frames between them.

#### Terminology

**UNI**: User Network Interface, **UNI-C**: UNI-customer side, **UNI-N** network side

**NNI**: Network to Network Interface, **E-NNI**: External NNI; **I-NNI** Internal NNI

**CE**: Customer Equipment

The MEF defines three types of Carrier Ethernet services:

#### E-Line (Ethernet Line Service)

**E-Line (Ethernet Line Service)** is a P2P EVC link between CEs established over the provider's MetroE network.

Associate a point-to-point forwarding service to a Service Instance

Native Transport: Ethernet to Ethernet Local Switching (connect) - Local Connect is purely a local Layer 2 forwarding mechanism within a single PE router.

MAC learning is not possible because Local Connect simply forwards traffic between the two EFPs without building a MAC address table, making it a point-to-point connection rather than a bridged service.

MPLS Transport: EoMPLS (xconnect) - called pseudowire

Another common name for E-Line is VPWS (Virtual Private Wire Service) This name is used when the provider uses MPLS on their network, transporting Ethernet over the MPLS network.

Ethernet Internet Access (EIA) refers to providing Layer 2 Ethernet-based connectivity to the internet using an MPLS VPN infrastructure

The E-line is subdivided into:

**Ethernet Private Lines (EPL)** - single EVC per UNI - it is a port based

**Ethernet Virtual Private Lines (EVPL)** Service Multiplexed UNI (i.e. multiple EVCs per UNI)

![ethernet virtual circuit point to point](<../.gitbook/assets/Unknown image (1425)>)

#### E-LAN (Ethernet LAN Service)

forms a full mesh of multipoint-to-multipoint UNIs, providing service multiplexing

Associate a multipoint forwarding service (Bridge Domain) with EFPs

Native Transport: Ethernet multipoint bridging:The service provider's network uses Carrier Ethernet protocols, often Provider Bridging (IEEE 802.1ad) or Provider Backbone Bridging (PBB, IEEE 802.1ah), to extend the Layer 2 E-LAN transparently across the service provider's infrastructure.

MPLS Transport: VPLS

![ethernet lan service full mesh topology](<../.gitbook/assets/Unknown image (1426)>)

#### E-Tree (Ethernet Tree Service)

multipoint service connecting one or more roots and a set of leaves, preventing interleaf communication - hub and spoke topology

Associate a rooted-multipoint forwarding service (Bridge Domain with Split Horizon) with Service Instances

Native Transport: Service Instances - configuring filtering rules on the PE devices associated with the Bridge Domain.

MPLS Transport: VPLS Virtual Forwarding Instance with split horizon group

![metro ethernet tree service](<../.gitbook/assets/Unknown image (1427)>)

Bundling: More than one CE-VLAN on a UNI mapped to an EVC

All-to-one Bundling: All CE-VLANs on a UNI mapped to a single EVC

Service Multiplexing: Support multiple EVCs over a UNI; EVC selection is based on CE-VLAN value

### IETF L2VPN types

Service providers usually utilize MPLS-based core transport to provide Layer 2 VPN - encapsulating Layer 2 frames in MPLS labels

Two main steps are needed in the control plane:

Finding a neighbor (opposite end of VC)

Static configuration (AToM)

Dynamic Autodiscovery (VPLS) – MP-BGP based on Cisco

Label exchange – for each VC via LDP

Two labels are used:

Top label points to an egress router

Second label identifies VC

Forwarding equivalence class (FEC) is equal to one Virtual Circuit (VC)

#### VPWS (Virtual Private Wire Service)

IETF technology for point-to-point L2VPN, implementing MEF E-LINE services.

Ethernet over MPLS (EoMPLS), Frame Relay over MPLS, PPP over MPLS.

#### VPLS (Virtual Private LAN Service)

IETF technology for multipoint-to-multipoint L2VPN, implementing MEF E-LAN services (Legacy).

This technology supports multipoint Layer 2 connections by grouping a collection of PWs terminated on a PE router in a virtual forwarding instance (VFI). The VFI represents a virtual extension of the physical circuit that is attached to the PE system. The VFI resembles a switch that is capable of learning MAC addresses and forwards traffic based on its MAC address table. A VPLS can connect thousands of PEs into a single VLAN and is therefore subject to scalability constraints. To improve the scalability of the solution, hierarchical VPLS (H-VPLS) topologies enable a two-tier deployment of the PE devices.

Layer 2 VPNs are grouped into three main categories: local switching that serves directly connected links, VPWS that provides point-point connectivity, and VPLS that enables multipoint connection service.

VPWS can be implemented using Layer 2 Tunneling Protocol version 3 (L2TPv3) when run in an IP environment, or by using AToM when deployed in an MPLS core. VPWS is a point-to-point technology. VPWS supports connections between the same interface types (like-to-like), and between different interface types (any-to-any). The supported attached interface types include Ethernet, Frame Relay, and ATM, including ATM adaptation layer 5 (AAL5), PPP, and HDLC. VPLS offers point-to-multipoint and multipoint-to-multipoint connectivity. It uses MPLS as the transport infrastructure.

IP core transport uses the L2TPv3 and supports only point-to-point connections. The available encapsulations include Ethernet, Frame Relay, and ATM, including AAL5, PPP, and HDLC. They can be linked in any-to-any fashion.

![](<../.gitbook/assets/Unknown image (1428)>)

#### L2TPv3

**L2TPv3** is used to transport layer 2 frames over pure IP networks.

The entire L2TP packet, including payload and L2TP header, is sent within a UDP datagram. Traditionally, L2TP has been used to carry PPP sessions within an L2TP tunnel.

L2TPv3 provides additional security features, improved encapsulation, and the ability to carry data links other than just PPP over an IP network (for example, Frame Relay, Ethernet, ATM, and others). L2TP overhead includes the transport IP header (20 bytes) and an L2TP header of variable length. The only mandatory field in the L2TP header is the session ID (4 bytes). Optional fields are cookie (8 bytes) and control word. The payload of the L2TPv3 packet is the original Layer 2 protocol data unit (PDU).

![](<../.gitbook/assets/Unknown image (1429)>)

Old ios config example

<\<UC\_L2TP\_Praha.txt>>

<\<L2TP-RTR-CZ.txt>>

#### Any Transport over Multiprotocol Label Switching (AToM)

**Any Transport over Multiprotocol Label Switching (AToM)** is Cisco's implementation of Virtual Private Wire Service (VPWS) for IP/MPLS networks

Ethernet over MPLS (EoMPLS) is the most common example of AToM, AToM is a subset of VPWS that provides point-to-point virtual connections.

In EoMPLS, the preamble, the Start Frame Delimiter (SFD) and the Frame Check Sequence (FCS) are excluded from encapsulation. The preamble of an Ethernet frame consists of a 56-bit (7-byte) pattern of alternating 1 and 0 bits, which allows devices on the network to easily detect a new incoming frame. The SFD is designed to break this pattern, and signal the start of the actual frame. The SFD is the 8-bit (1-byte) value, which is 10101011. The SFD represents the end of the preamble of an Ethernet frame and is followed by the destination MAC address.

AToM enables the following types of Layer 2 frames and cells to be directed across an MPLS backbone:

Ethernet, Ethernet VLAN

ATM (AAL5 / cell relay)

Frame Relay

Point-to-Point Protocol (PPP)

High-Level Data Link Control (HDLC)

SDH/PDH - CEoP

EoMPLS Architecture

**EoMPLS architecture**

The Ingress PE encapsulates the customer frame into MPLS, while the Egress PE decapsulates it.

A label stack of two labels is used, similar to MPLS VPN operation:

Top label (“Tunnel Label”) – Identifies the LSP between PEs.

Second label (“VC Label”) – Identifies the outgoing interface on the egress PE.

A targeted (multihop) LDP session is established between PEs for VC signaling.

LDP (Label Distribution Protocol) is extended to carry Virtual Circuit Forwarding Equivalence Class (VC FEC) information.

![](<../.gitbook/assets/Unknown image (1430)>)

Virtual Circuit FEC Element

**Virtual Circuit FEC element**

The PW label which serves as the pseudowire demultiplexor can be assigned and distributed by LDP as specified in draft-ietf-pwe3-controlprotocol-07.txt.

A new LDP FEC element type 128 – Virtual Circuit FEC Element - has been defined to carry label mapping message and downstream unsolicited label distribution mode must be used.

[http://tools.ietf.org/html/draft-ietf-pwe3-control-protocol-07](http://tools.ietf.org/html/draft-ietf-pwe3-control-protocol-07)

[https://tools.ietf.org/html/rfc4906](https://tools.ietf.org/html/rfc4906)

C – Control word present

VC Type – ATM, FR, Ethernet, HDLC, PPP, etc …

VC Info Length – Length of VCID

Group ID – Group of VCs referenced by index (user configured)

VC ID – Identify PW

Interface Parameters – MTU, etc ….

![](<../.gitbook/assets/Unknown image (1431)>)

Pseudo Wire VC Type Field

**Pseudowire types (VC type field)**

| **PW type** | **Description**                               |
| ----------- | --------------------------------------------- |
| 0x0001      | Frame Relay DLCI                              |
| 0x0002      | ATM AAL5 SDU VCC transport                    |
| 0x0003      | ATM transparent cell transport                |
| **0x0004**  | **Ethernet Tagged Mode (VLAN)**               |
| **0x0005**  | **Ethernet**                                  |
| 0x0006      | HDLC                                          |
| 0x0007      | PPP                                           |
| 0x0008      | SONET/SDH Circuit Emulation Service Over MPLS |
| 0x0009      | ATM n-to-one VCC cell transport               |
| 0x000A      | ATM n-to-one VPC cell transport               |
| 0x000B      | IP Layer2 Transport                           |
| 0x000C      | ATM one-to-one VCC Cell Mode                  |
| 0x000D      | ATM one-to-one VPC Cell Mode                  |
| 0x000E      | ATM AAL5 PDU VCC transport                    |
| 0x000F      | Frame-Relay Port mode                         |
| 0x0010      | SONET/SDH Circuit Emulation over Packet (CEP) |

VCID (Virtual Circuit ID)

A unique identifier for each Layer 2 circuit configured on the PE.

Label TLVs (Type-Length-Value elements) are used by LDP to encode label information — e.g. for label advertising, requesting, releasing, or withdrawing.

Each TLV type defines the type of label being carried.

Generic Label TLV (0x200): Used for links where label values are independent of the link technology (e.g. Ethernet, PPP). This is the most common TLV type in MPLS networks.

**AToM operation**

The IGP and the LDP between directly connected LSRs establish one LSP in each direction.

Targeted LDP session (one LDP session can signal multiple VC)

No MAC learning, no fragmentation

The ingress and egress PE allocates VC label

The targeted LDP session between PE routers propagates the VC label.

The ingress PE receives a frame

The frame is encapsulated and forwarded along the LSP.

Egress PE matches the VC label in the packet to its local VCID, which is associated with the AC towards CE2

![](<../.gitbook/assets/Unknown image (1432)>)

**Attachment circuit (AC)**

Connection between CE and PE, it could be Ethernet physical or logical port. The attachment circuit is mapped to the emulated virtual circuit (VC) for transport through the service provider core.

AC Type

Ethernet Port Mode: is an AC configured to accept untagged frames

VLAN Mode: is an AC configured to accept single-tagged frames

QinQ Mode: is an AC configured to accept double-tagged frames

The PE removes an internal tag before sending traffic into the PW and adds a tag upon exit

**PW (Pseudo-Wire)** is a point-to-point Virtual Circuit (VC) connection that links attachment circuits that are attached to different PE routers

VC Type 5

is called Ethernet Port Mode e.g. Port-based PW

Transports the entire Ethernet port (all VLANs, or untagged traffic). Essentially a trunk.

Treats the port as a single entity — everything from that port goes into the pseudowire.

VC Type 4

is called VLAN Mode e.g. VLAN-based PW

Transports single VLAN over the pseudowire - Each VLAN is a separate L2VPN service

EoMPLS VC type 5 is the default configuration mode on the platforms supporting it. Meaning that the PEs will try to first negotiate and use VC 5, if one of them (or both) does not support it they will reverse to VC 4. EoMPLS type VLAN offers backward compatibility in case the remote peer does not support VC type Ethernet.

The VC type can be adjusted or forced under the pw-class configuration

**Label operations**

Imposition (packet entering a PW): PE adds the necessary headers for the PW (e.g. MPLS labels) and may remove or keep the original VLAN tags depending on the configuration and type of the PW.

Disposition (packet exiting a PW): PE removes the PW headers and may add or modify VLAN tags before sending the traffic to the destination network.

![](<../.gitbook/assets/Unknown image (1433)>)

![](<../.gitbook/assets/Unknown image (1434)>)

![](<../.gitbook/assets/Unknown image (1435)>)

**Pseudowire grouping**

When pseudowires (PW) are established, each PW is assigned a group ID that is common for all PWs created from the same physical port. Hence, when the physical port becomes non-functional or is deleted, L2VPN sends a single message to advertise the status change of all PWs belonging to the group. A single L2VPN signal thus avoids a lot of processing and loss in reactivity.

Pseudowire grouping is disabled by default.

### AToM configuration

AToM is configured using the following steps:

The PE routers must have a /32 address assigned to their loopbacks.

MPLS must be enabled in the core.

Make sure MTU is large enough in the core. CE MTU + L2 Header + AToM Control word + 2x MPLS header 1500 + 18 (14 ETH +4 VLAN) + 4 + 8 = 1530 B

Make sure MTU is same on both endpoint interfaces

#### IOS XE protocol-based CLI (command line interface)

is a modernization of Cisco's Layer 2 VPN configuration commands, designed to provide a consistent and scalable framework across Cisco platforms and operating systems. Introduced in Cisco IOS XE Release 3.7S and IOS 15.3(1)S, this feature replaces legacy commands with a structured, service-oriented approach, enhancing configuration clarity and operational flexibility.​

[https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/mpls/b-mpls/m\_l2vpn-prot-based.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/mpls/b-mpls/m_l2vpn-prot-based.html)

Key Features and Benefits

Unified Configuration Model: Utilizes l2vpn xconnect context for point-to-point services and l2vpn vfi context for multipoint services, standardizing configurations across platforms.​

Enhanced Pseudowire Management: Pseudowires are treated as virtual interfaces, allowing for direct application of features like Quality of Service (QoS), monitoring, and redundancy configurations.​

Improved Redundancy and High Availability: Supports independent configuration of redundant pseudowires, enabling seamless failover and maintenance without service disruption.​

Template-Based Configuration: Introduces interface templates for pseudowires, facilitating consistent and efficient deployment of services.​

The protocol-based CLI replaces several legacy commands to streamline configurations. For example:​

Legacy: xconnect

New: l2vpn xconnect context​

Legacy: l2 vfi

New: l2vpn vfi context​

On both systems, the PW class can used as a container (similar to template) for optional parameters, such as encapsulation type, the use of control word, transport mode (port or VLAN), sequencing, and others. If you do not configure a PW class, as above, the default encapsulation of MPLS is assumed.

In Cisco IOS XR Software, the attachment circuit must be enabled for Layer 2 transport using the l2transport keyword.

On Cisco IOS and IOS XE Software, the attachment circuit is configured with the service instance number ethernet \[name] interface configuration command. This command creates an EFP on a Layer 2 interface and enters the service instance configuration mode. You use service instance configuration mode to configure all management and control date plane attributes and parameters that apply to the service instance on a per-interface basis. In this example the service instance matches all frames with 802.1Q tag 11.

service instance ethernet

This defines the service instance. The number is arbitrary; it has nothing to do with the VLANs that will be processed by this particular Service Instance The "ethernet" keyword is always used.

Use service instance if:

You're working on MPLS L2VPN, EVC, MEF, or provider edge (PE) scenarios.

You need fine-grained VLAN manipulation, loop prevention, split-horizon, etc.

You're deploying on ASR, CSR, or newer IOS XE (17.x+) platforms.

Use int.x subinterfaces if:

You're on smaller/older platforms or doing basic L2VPN testing.

encapsulation dot1q

map an incoming tag to a service instance.

In Cisco IOS XR Software, the EoMPLS service is defined in the l2vpn xconnect group configuration mode. You define a point-to-point service using the p2p command. Its elements can include an attachment circuit and a PW. The neighbor command specifies the remote PE address and the VC ID.

In Cisco IOS/XE, the EoMPLS service is defined as a l2vpn xconnect context. Its two members are the attachment circuit and the PW. The attachment circuit is the Ethernet service instance 11 on the GigabitEthernet0/0/0 interface. The PW ID 11 connects to the peer (10.1.1.1), uses VC number 11 and encapsulation MPLS.

![](<../.gitbook/assets/Unknown image (1436)>)

### Monitoring and troubleshooting

Common issues:

Access circuit is down

Different MTU on AC ports

Different VC type

Problem in LDP (e.g. label propagation filtering)

| IOS-XE# sh l2vpn atom vc                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IOS-XE# show mpls l2transport vc                  | use vc detail to display details about specific pseudowire                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| IOS-XR# sh l2vpn xconnect \[ ]                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| PE1#ping mpls pseudowire                          | Virtual Circuit Connection Verification (VCCV) is an L2VPN Operations, Administration, and Maintenance (OAM) feature that allows network operators to run IP-based provider edge-to-provider edge (PE-to-PE) keepalive protocol (like BFD or LSP Ping) across a specified pseudowire to ensure that the pseudowire data path forwarding does not contain any faults. The disposition PE receives VCCV packets on a control channel, which is associated with the specified pseudowire. The control channel type and connectivity verification type, which are used for VCCV, are negotiated when the pseudowire is established between the PEs for each direction. The MPLS LSP Ping, Traceroute, and AToM VCCV feature uses MPLS echo request and reply packets to test LSPs. The Cisco implementation of MPLS echo request and echo reply are based on the Internet Engineering Task Force (IETF) Internet-Draft Detecting MPLS Data Plane Failures. More in [https://sites.google.com/site/amitsciscozone/mpls/mpls-wiki/bfd-for-pseudowire-vccv](https://sites.google.com/site/amitsciscozone/mpls/mpls-wiki/bfd-for-pseudowire-vccv) |
| pseudowire-class eompls encapsulation mpls status | Without Pseudowire Status Signaling - When AC (Access Circuit) is down, appropriate labels are withdrawn Status signaling enable preserving labels and only signal the status of AC. Both ends of pseudowire have to support this feature The pseudowire status messages are sent in label advertisement and label notification messages![](<../.gitbook/assets/Unknown image (1437)>)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

### Pseudowire redundancy

backup peer provides a redundant pseudowire (PW) connection in the case that the primary PW loses connection; if the primary PW goes down, the router diverts traffic to the backup PW. This feature provides the ability to recover from a failure of either the remote PE router or the link between the PE router and CE router.

Attachment circuit failure can be caused by interface condition (up/down/LOS) or integrated LMI notification

Pseudowire failure for AToM is discovered by established LDP targeted session

#### One-way EoMPLS redundancy

In a one-way redundancy method, the local PE has two PWs to two remote PEs serving the same destination site or to the same egress PE with redundant attachment circuits. One PW is declared as primary, the other as backup. A fault of the primary PW triggers a failover to the backup PW.

![](<../.gitbook/assets/Unknown image (1438)>)

#### Two-way EoMPLS redundancy

In a two-way redundancy method, four PWs are used to provide high availability service.

Only one PW is declared as primary. The three remaining PWs are intended for backup. In each site, one attachment circuit is primary, the other backup. To synchronize the redundancy information between the LAN and the PE devices, Multi-Chassis Link Aggregation Group (MC-LAG) must be enabled in the network. MC-LAG refers to the PE devices as points of attachment nodes. The points of attachment run Interchassis Communication Protocol (ICCP) to synchronize state and form a redundancy group.

IOS-XE [https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/mpls/b-mpls/m\_wan-l2vpn-pw-red-xe.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/mpls/b-mpls/m_wan-l2vpn-pw-red-xe.html)

![](<../.gitbook/assets/Unknown image (1439)>)

### Ethernet Virtual Circuit (EVC) framework

**Ethernet Virtual Circuit (EVC) framework** is the next generation cross-platform software architecture to address Carrier Ethernet Services requirements including service flexibility, scalability, network redundancy, HA, performance, OAM and QoS.

This new Ethernet infrastructure specifically addresses Carrier Ethernet and Layer 2 VPN services. Inspired by MEF terminology, it is given “EVC” as short name.

Traditional routers remove (pop) the VLAN tags configured under the subinterface from the frame before they are transported by the L2VPN feature. On a Cisco ASR 9000 Series Aggregation Services Router that uses the EVC model, the default action is to preserve the existing tags.

In the classic SVI model, the interface number is tied to the VLAN ID, so you’re limited to 4094 VLANs.

In the EVC model, the sub-interface number is independent of the VLAN tag — you still tag frames with 802.1Q IDs in the 1–4094 range, but you can create far more logical service interfaces, each with flexible classification rules (single VLAN, VLAN ranges, stacked VLANs, etc.).

#### EVC / sub-interface model (IOS XR, ME-Series, ASR9K, etc.)

You don’t use interface vlan. Instead, you use Layer 2 sub-interfaces (EFPs) under a physical port or bundle:

VLAN tag(s) are defined inside the encapsulation statement (encapsulation dot1q 112), which can still only be 1–4094 for 802.1Q.

Because the numbering of sub-interfaces is independent of the VLAN ID, you can have more than 4094 sub-interfaces (the limit is platform-dependent, not VLAN-ID-limited).

Multiple sub-interfaces can even reference the same VLAN ID since they fall into different bridge domain

| SW-C9300-24UX(config)#int vlan ? <1-4094> Vlan interface number |   |
| --------------------------------------------------------------- | - |

| RP/0/RP0/CPU0:N540X-16Z4G8Q2C-A(config)#int TenGigE0/0/0/11.200000? <0-2147483647> R/S/I/P/B or R/S/I/P |   |
| ------------------------------------------------------------------------------------------------------- | - |

| interface Gig0/0/0/0.5000 encapsulation dot1q 112 | The sub-interface number (.5000) is just a local identifier — it’s not required to match the VLAN ID. |
| ------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |

Cisco EVC supports flexible access VLAN to forwarding service mapping:

Frame matching based on one or more VLAN tags.

Optional VLAN tag manipulation.

1-to-1 access VLAN to a service

Same port, multiple access VLANs to a service

Multiple ports, multiple access VLANs to a service

Forwarding services include

L2 point-to-point local connect

L2 point-to-point xconnect

L2 multipoint bridging

L2 multipoint VPLS

L2 point-to-multipoint bridging

L3 termination

#### Components

**Ethernet flow point (EFP) service instance**

EFP is a substream partition of a main interface. On Cisco routers, EFP is implemented as a Layer 2 subinterface with an encapsulation statement.

They are logical entities that define how traffic is handled (e.g., classification, marking, or forwarding) within the EVC

Each service instance has a unique number per interface, but you can use the same number on different interfaces because service instances on different ports are not related.

EVC can be configured under bundle interface - load balancing can be adjusted&#x20;

![](<../.gitbook/assets/Unknown image (1440)>)

![](<../.gitbook/assets/Unknown image (1441)>)

**Ethernet Virtual Circuit (EVC)**

EVC is a logical Layer 2 connection that transports Ethernet frames between two or more endpoints (EFPs) across a provider's network

EVC is a virtual "pipe" carrying Ethernet traffic. It's a service-level concept, not a physical interface

**Bridge domain (BD)**

is a Layer 2 broadcast domain that behaves like a traditional VLAN but is not tied to a single VLAN ID.

VLAN bridge has 1:1 mapping between VLAN and internal Broadcast Domain - VLAN has global per-device significance

EVC bridge decouples VLAN from Broadcast Domain - VLAN treated as encapsulation on a wire

BD allows decoupling broadcast domain from VLAN

One-to-many mapping from BD to Service Instances

All devices in the same BD can communicate via broadcast, unicast, and multicast.

It acts like a software-based switch in the router.

In classic switching: 1 VLAN ID = 1 Broadcast Domain

In EVC: Bridge Domain ≠ VLAN ID

You can map multiple VLANs to the same BD

You can even use untagged traffic (no VLAN) in a BD

One-to-many mapping:

One BD can be linked to many EFPs (i.e., service instances)

Each EFP may match different VLANs, but all traffic ends up in the same BD

Example: VLAN 100 and VLAN 200 (on different interfaces) are both mapped to BD 200

Even though VLAN IDs are different, the bridge-domain creates one common broadcast domain

Inside the BD, MAC learning, flooding, and forwarding behave as if all traffic is on the same L2 segment

The original VLAN tags can still be preserved at ingress/egress for policy/QoS

An EVC broadcast domain is determined by a bridge domain and the EFPs that are connected to it. You can connect multiple EFPs to the same bridge domain on the same physical interface, and each EFP can have its own matching criteria and rewrite operation. An incoming frame is matched against EFP matching criteria on the interface, learned on the matching EFP, and forwarded to one or more EFPs in the bridge domain. If there are no matching EFPs, the frame is dropped.

You can use EFPs to configure VLAN translation. For example, if there are two EFPs egressing the same interface, each EFP can have a different VLAN rewrite operation, which is more flexible than the traditional switchport VLAN translation model.

QoS policies on EFPs are supported with ingress rewrite type as push. In the ingress direction with one VLAN tag is pushed and in the egress direction one VLAN tag is popped.

[https://www.cisco.com/c/en/us/td/docs/routers/ncs5xx/ncs520/configuration/guide/CE/17-1-1/b-ce-xe-17-1-1-ncs520/ethernet-virtual-connections-configuration.html](https://www.cisco.com/c/en/us/td/docs/routers/ncs5xx/ncs520/configuration/guide/CE/17-1-1/b-ce-xe-17-1-1-ncs520/ethernet-virtual-connections-configuration.html)

**Bridge Domain Interface (BDI) (optional)**

Is an Logical Layer 3 interface that can be associated with a BD to provide integrated routing and bridging for the bridge domain

#### Local and bridged forwarding services

Layer 2 P2P local services

No MAC learning

Two Service Instances (EFP) on same interface (hair-pin)

Two EFPs on different interfaces

Layer 2 MP bridged services

MAC based forwarding and learning

Bridge Domain (BD)—different access VLANs can be mapped to the same broadcast domain

Split Horizon → Prevents loops or to restrict communication between certain service instances.

#### Multiplexed forwarding services

Layer 2 P2P services using Ethernet over MPLS

EFP to EoMPLS Pseudowire

Layer 2 MP services using VPLS

Extends ethernet multipoint bridging over a full mesh of PWs

Split horizon support over attachment circuits and PWs

#### Rooted-multipoint forwarding services (E-Tree)

BD with Split Horizon Group can be used to implement rooted-multipoint forwarding service:

Place all Leaf EFPs in Split Horizon Group

Keep Root EFP outside the Split Horizon Group

Bidirectional connectivity between Root and all Leaf EFPs

Leaf EFPs cannot communicate to each other

#### Layer 3 services

Layer 3 termination through SVI/BDI interface

Layer 3 termination through Routed sub-interfaces

![](<../.gitbook/assets/Unknown image (1442)>)

![](<../.gitbook/assets/Unknown image (1443)>)

![](<../.gitbook/assets/Unknown image (1444)>)

#### Flexible service mapping

Flexible Ethernet mapping is the ability to process and classify various Ethernet frame types, each with various attributes (ethertypes, VLAN tags, class of service \[CoS] bits, and so on).

Single Tagged (S-VLAN) → Frames with a single VLAN tag.

Double Tagged (C-VLAN + S-VLAN) → Frames with two VLAN tags (Q-in-Q encapsulation)

Customer Network C-TAG Identifies customer VLAN

Service Provider S-TAG Identifies customer as a whole (tunnel)

Header/Payload Matching → Classification based on more than just VLAN tags (e.g., CoS or PPPoE)

**Loose match classification rule**

Unspecified fields are treated as wildcard / "any value matches"

| encap dot1q 10 | matches any frame with outer tag equal to 10 |
| -------------- | -------------------------------------------- |

![](<../.gitbook/assets/Unknown image (1445)>)

| encap dot1q 10 sec 50 | matches any frame with outer-most tag as 10 and second tag as 50 |
| --------------------- | ---------------------------------------------------------------- |

![](<../.gitbook/assets/Unknown image (1446)>)

**Longest match classification rule**

Frames are mapped to Service Instance with longest matching set of classification fields

the "longest match" rule means that if a packet has multiple VLAN tags, and there are different rules defined that match an outer tag, or an outer tag and a specific inner tag, the rule that matches the most specific combination of VLAN tags (e.g., outer + inner exact tag vs. just outer tag) will be applied.

![](<../.gitbook/assets/Unknown image (1447)>)

**Exact match classification rule**

![](<../.gitbook/assets/Unknown image (1448)>)

**Service instance with `default` encapsulation**

Matches all frames unmatched by any other EFP on a port

Note If default Service Instance is the only one configured on a port, it matches all traffic on the port (tagged and untagged)

Ethernet Flow Points (EFP)

also referred to as EVC service-instances provide classification of L2 flows on Ethernet interfaces

Support dot1q and Q-in-Q

Support VLAN lists

Support VLAN ranges

Support VLAN Lists and Ranges combined

Coexist with routed subinterfaces

![](<../.gitbook/assets/Unknown image (1449)>)

#### Forwarding, learning, and aging on EFPs

Layer 2 forwarding is based on the bridge domain ID and the destination MAC address.

The frame is forwarded to an EFP if the binding between the bridge domain, destination MAC address, and EFP is known;

MAC address learning is based on bridge domain ID, source MAC addresses, and logical port number.

If there is no matching entry in the Layer 2 forwarding table for the ingress frame, the frame is flooded to all the ports within the bridge domain.

#### VLAN tag manipulation (push/pop/translate)

#### PUSH

Add one VLAN tag

Add two VLAN tags

![](<../.gitbook/assets/Unknown image (1450)>)

#### POP

Remove one VLAN tag

Remove two VLAN tags

![](<../.gitbook/assets/Unknown image (1451)>)

#### TRANSLATE

1:1 VLAN Translation

1:2 VLAN Translation

2:1 VLAN Translation

2:2 VLAN Translation

![](<../.gitbook/assets/Unknown image (1452)>)

#### Cisco EVC configuration anatomy

![](<../.gitbook/assets/Unknown image (1453)>)

#### Configuring EVC global parameters

| Router(config)# interface \<slot/port>Router(config-if)# service instance ethernet Router(config-if-srv)# Router(config-if-srv)# Router(config-if-srv)# Router(config-if-srv)# | This command configures service instance or EFP (Ethernet Flow Point) The id is number from 1 to 8000 (4000 – depends of platform scale), id has only interface scope The keyword ethernet specifies type of service The name is optional and can be used to bind EFP to EVC. More EFPs can be bound to one EVC. instance id – number 1 to 4000 (locally significant = per interface scope) ethernet evc-name - (Optional) ethernet name is the name of a previously configured EVC. You do not need to use an EVC name in a service instance. = VLAN tags, MAC, CoS, Ethertype = VLAN tags pop/push/translation = bridge-domain, xconnect or local connect = QoS, ACL, etc |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Encapsulation matching is done on a best match

If a packet entering a port, does not match any of the Encapsulations on that port, then that packet is dropped. This “filtering” happens both on Ingress and Egress.

The Encapsulation matches the packet on the wire to determine filtering criteria.

“On the wire” is defined as packets ingressing the switch prior to any rewrites, and packets egressing the switch after all rewrites.

| `encapsulation dot1q {any \| vlan-id} etype ethertype`                         | Matches frames based on VLAN ID and payload ethertype (e.g., ipv4, ipv6, pppoe-all).                  |
| ------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- |
| `encapsulation dot1q vlan_id cos cos_value second-dot1q vlan_id cos cos_value` | Defines match criteria for S-Tag and C-Tag including specific CoS values (1–7).                       |
| `encapsulation dot1q any`                                                      | Matches any packet that contains one or more VLAN tags.                                               |
| `encapsulation untagged`                                                       | Matches native (untagged) Ethernet frames; only one EFP per port can use this.                        |
| `encapsulation default`                                                        | Acts as a catch-all for all ingress frames; cannot coexist with other EFPs in the same bridge domain. |
| `encapsulation priority-tagged`                                                | Specifies matching for priority-tagged frames (VLAN ID 0) with CoS values 0–7.                        |

#### Examples

Match traffic with dot1q tag 10

Router (config)# interface gigabitethernet0/1

Router (config-if)# service instance 1 Ethernet \[name]

Router (config-if-srv)# encapsulation dot1q 10

Match traffic with dot1q tags 22-44

Router (config)# interface gigabitethernet0/1

Router (config-if)# service instance 2 Ethernet \[name]

Router (config-if-srv)# encapsulation dot1q 22-44

Match traffic with dot1q tags 50,51

Router (config)# interface gigabitethernet0/1

Router (config-if)# service instance 3 Ethernet \[name]

Router (config-if-srv)# encapsulation dot1q 50,51

Match traffic with dot1q S tag 30 and C dot1q tags 1-100

Router (config)# interface gigabitethernet0/1

Router (config-if)# service instance 4 Ethernet \[name]

Router (config-if-srv)# encapsulation dot1q 30 second-dot1q 1-100

Cisco IOS XR offers two ways to configure EoMPLS for untagged frames. An attachment circuit can be a main interface, where the l2transport command is configured under the interface configuration mode, or a subinterface, where the l2transport keyword is configured after the subinterface number. In the latter case, which is depicted in this scenario, you can match the frames using the encapsulation untagged command.

![](<../.gitbook/assets/Unknown image (1454)>)

#### Rewrite command

The rewrite command is critical in the Cisco Ethernet Virtual Circuit (EVC) framework because it explicitly defines how VLAN tags are manipulated as a frame crosses the boundary of a Service Instance (EFP). Unlike traditional switching where tag behavior is assumed by the port type (e.g., trunk or access), EVCs require you to define the tag action (pop, push, or translate) based on the service being offered.

It defines the necessary tag actions for frames classified by the encapsulation command before they enter the Bridge Domain (BD) or Layer 2 transport.

POP Operations example

| rewrite ingress tag pop 1 symmetric |   |
| ----------------------------------- | - |
| rewrite ingress tag pop 2 symmetric |   |

PUSH Operations example

| rewrite ingress tag push dot1q 10 symmetric                 |   |
| ----------------------------------------------------------- | - |
| rewrite ingress tag push dot1q 10 second-dot1q 20 symmetric |   |

TRANSLATION Operations example

| rewrite ingress tag translate 1-to-1 dot1q 100 symmetric                  |   |
| ------------------------------------------------------------------------- | - |
| rewrite ingress tag translate 1-to-2 dot1q 100 second-dot1q 200 symmetric |   |
| rewrite ingress tag translate 2-to-1 dot1q 100 symmetric                  |   |
| rewrite ingress tag translate 2-to-2 dot1q 100 second-dot1q 200 symmetric |   |

Service instance must be attached to bridge domain if we want to configure multipoint services

Matching traffic with encapsulation command must be configured before configuring bridge domain

| Router(config-if-srv)# bridge-domain bridge-id \[ split-horizon \[ group group-id ] ] | To bind a service instance (EFP) or a MAC tunnel to a bridge domain instance bridge-id - Numerical identifier for the bridge domain instance. The range is an integer from 1 to the platform-specific maximum (or upper) limit. (ASR1000 limit is 4096) split-horizon – (Optional) Configures a port or service instance as a member of a split-horizon group. (not supported in MAC-in-MAC tunnel) group group-id - (Optional) Identifier for the split-horizon group. Range is 1 to 65533. (ASR1000 support only 0 or 1) EFPs in the same bridge domain and split-horizon group cannot forward traffic between each other, but can forward traffic between other EFPs in the same bridge domain but not in the same split-horizon group. |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

![](<../.gitbook/assets/Unknown image (1455)>)

#### Configuring point-to-point services

Note If you use the old CLI method of configuring of L2VPN's, it will automatically create pseudowires with an ID generated by the system. In newer method you define the int pseudowire id like regular logical interface.

Older:

interface GigabitEthernet5

no ip address

negotiation auto

no mop enabled

no mop sysid

service instance 141 ethernet

encapsulation dot1q 141

rewrite ingress tag pop 1 symmetric

xconnect 172.17.0.50 41 encapsulation mpls

backup peer 172.17.0.60 51

NEW:

interface pseudowire32

encapsulation mpls

neighbor 172.17.0.40 32

interface GigabitEthernet1

service instance 231 ethernet

encapsulation dot1q 231

rewrite ingress tag pop 1 symmetric

l2vpn xconnect context LAB003

member pseudowire32

member GigabitEthernet1 service-instance 231

Point-to-point local connect

| interface GigabitEthernet0/3/4 no ip address negotiation auto service instance 1 ethernet encapsulation dot1q 1 ! interface GigabitEthernet0/3/7 no ip address negotiation auto service instance 5 ethernet encapsulation default ! l2vpn xconnect context localconnect2 interworking vlan member GigabitEthernet0/3/4 service-instance 1 member GigabitEthernet0/3/7 service-instance 5 |   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

Point-to-point xconnect

| Router(config)# xconnect encapsulation mpls | interface GigabitEthernet0/0 service instance 11 ethernet encapsulation dot1q 101 xconnect 10.0.0.3 101 encapsulation mpls |
| ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |

#### Configuring multipoint services

You set up a VPLS by first creating a virtual forwarding instance (VFI) on each participating PE device. The VFI specifies the VPN ID of a VPLS domain, the addresses of other PE devices in the domain, and the type of tunnel signaling and encapsulation mechanism for each peer PE device.

The set of VFIs formed by the interconnection of the emulated VCs is called a VPLS instance; it is the VPLS instance that forms the logic bridge over a packet switched network. After the VFI has been defined, it needs to be bound to an attachment circuit to the CE device. The VPLS instance is assigned a unique VPN ID.

| Router(config)# bridge-domain \[split-horizon]                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                                                                                                                                                                                   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Local Bridging interface GigabitEthernet0/0 service instance 2 ethernet encapsulation dot1q 101-1000 bridge-domain 100 interface GigabitEthernet0/1 service instance 3 ethernet encapsulation dot1q 101-1000 bridge-domain 100 interface GigabitEthernet0/2 service instance 1 ethernet encapsulation dot1q 101-1000 bridge-domain 100 \[split-horizon group <>] | Disables communication between leaf Service Instances in Split Horizon Group                                                                                                                                                                                                                                                                      |
| EVC VPLS Config Example l2 vfi Manual-VPLS manual vpn id 1111 bridge-domain 18 neighbor 172.20.0.2 encapsulation mpls neighbor 172.20.0.3 encapsulation mpls neighbor 172.20.0.4 encapsulation mpls ! interface GigabitEthernet0/0/1 service instance 18 ethernet encapsulation dot1q 301 second-dot1q 18 rewrite ingress tag pop 2 symmetric bridge-domain 18   | Configures a VPN ID for a VPLS domain. The emulated VCs bound to this Layer 2 virtual routing and forwarding (VRF) instance use this VPN ID for signaling. The VPN ID configured in the VFI mode must match up with the VC ID configured in Cisco IOS XR with the neighbor command. The bridge domain links the attachment circuits with the VFI. |

| <p>interface Gi0/1 service instance 1 ethernet<br>encapsulation dot1q 10<br>rewrite ingress pop 1 symmetric<br>bridge-domain 8000 split-horizon group 1 service Instance 2 ethernet encapsulation dot1q 99<br>rewrite ingress pop 1 symmetric<br>bridge-domain 8000 split-horizon group 1 interface Gi0/2 service Instance 3 ethernet<br>encapsulation dot1q 10<br>rewrite ingress pop 1 symmetric<br>bridge-domain 8000 split-horizon group 2 service instance 4 ethernet encapsulation dot1q 99 rewrite ingress pop 1 symmetric<br>bridge-domain 8000</p> | In this example, Service Instances 1 and 2 cannot forward and receive packets from each other. Service Instance 3 can talk to everyone in Bridge-Domain 8000 since no one is in Split-Horizon Group 2. Service Instance 4 can talk to everyone in Bridge-Domain 8000 since it has not joined any Split-Horizon Groups. |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### Configuring Layer 3 services (IRB)

The routing exchange with the outside world could be provided by a customer device inside a Layer 2 bridge domain. Alternatively, you can configure a VPLS peer to perform the routing function for the bridge domain. To do this, you need to configure a Layer 3 interface that plugs into a bridge-domain to route packets in and out of the bridge-domain. In Cisco IOS XR Software, the routed VPLS feature is implemented using the BVI. In Cisco IOS XE Software, the routing instance for the virtual bridge domain is referred to as the BDI. VPLS-integrated routing and bridging (IRB) is also known as routed PW and routed VPLS.

Normally the router would remove the ethernet and vlan header and replace it with new, thus the vlan id would not be preserved. Bridge group allows router to function as a bridge and treat forwarding from one interface to another as a switch, without altering with Ethernet and VLAN header

Bridge Domain Interface (BDI) | Bridge-Group Virtual Interface (BVI)

is a logical interface used to provide Layer 3 connectivity (IP routing) for a bridge domain - a group of ports that can forward traffic between each other at Layer 2

BDIs are similar to SVIs, but they are associated with bridge domains, which can encompass multiple VLANs or even different Layer 2 technologies, unlike SVIs which provide L3 gateway for one specific VLAN. BDIs, BVIs, SVIs, 802.1Q subinterfaces, and VLANs all have in common is the use of the 802.1Q tag to identify which "network" (or broadcast domain) a particular interface belongs to (whether it's a physical or logical/virtual interface).

Note: the rewrite pop operation is mandatory for IP termination Since the BVI is a Layer 3 interface and cannot process Layer 2 VLAN tags, you must use rewrite ingress tag pop X symmetric to strip all tags before the frame is passed to the routing engine.

![](<../.gitbook/assets/Unknown image (1456)>)

#### Layer 2 protocol tunneling

Customers at different sites that are connected across a service-provider network need to use various Layer 2 protocols to scale their topologies to include all remote sites, as well as the local sites. STP must run properly, and every VLAN should build a proper spanning tree that includes the local site and all remote sites across the service-provider network. Cisco Discovery Protocol (CDP) must discover neighboring Cisco devices from local and remote sites. VLAN Trunking Protocol (VTP) must provide consistent VLAN configuration throughout all sites in the customer network.

When protocol tunneling is enabled, edge device on the inbound side of the service-provider network encapsulate Layer 2 protocol packets with a special MAC address and send them across the service-provider network. Core devices in the network do not process these packets but forward them as normal packets. Layer 2 protocol data units (PDUs) for CDP, STP, or VTP cross the service-provider network and are delivered to customer devices on the outbound side of the service-provider network. Identical packets are received by all customer ports on the same VLANs

[https://www.cisco.com/c/dam/en/us/td/docs/switches/lan/catalyst9600/software/release/17-2/configuration\_guide/lyr2/configuring\_layer2\_protocol\_tunneling.html](https://www.cisco.com/c/dam/en/us/td/docs/switches/lan/catalyst9600/software/release/17-2/configuration_guide/lyr2/configuring_layer2_protocol_tunneling.html)

| interface GigabitEthernet0/4 service instance 20 ethernet encapsulation untagged, dot1q 200 second-dot1q 300 l2protocol tunnel cdp stp vtp dtp pagp lacp lldp udld bridge-domain 10 | To enable L2PT Valid include: cdp, dtp, lacp, pagp, stp, vtp, lldp, udld If a protocol is not listed in , then it is dropped at the interface. |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |

#### Monitoring EFP

| **show ethernet service \[id**_evc-id_\*\* \| interfac&#x65;_**interface-id**_] \[detail]\*\* | **Displays information about EVC**                                             |
| --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| **show bridge-domain**_\[n]_                                                                  | Displays all the members of the specified bridge-domain                        |
| **show bridge-domain**_n_\*\* split-horizon \[group {_**group\_id**_ \| all}]\*\*             | Displays all the members of bridge-domain n that belong to split horizon group |
| **show mac-address-table**                                                                    | Displays dynamically learned or statically configured MAC security addresses   |
| **show bridge-domain**_n_**mac dynamic address**                                              | Displays MAC address table information for the specified bridge domain.        |

| Router(config)# no mac-address-table learning {vlan vlan-id \| interface interface slot/port} | By default, MAC address learning is enabled on all interfaces and bridge domains or VLANs on the router. You can disable learning on a bridge domain by entering the global configuration command When you disable MAC address learning for a BD/VLAN or interface, the router that receives packet from any source on the BD, VLAN or interface, the addresses are not learned. Since addresses are not learned, all IP packets floods into the Layer 2 domain. Note We recommend that you disable MAC address learning only in VLANs with two ports. |
| --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Router(config)# mac-address-table aging time \[0 \| 10-1000000]                               | Dynamic addresses are aged out if there is no frame from the host with the MAC address. The default for aging dynamic addresses is 5 minutes. You can configure dynamic address aging time per VLAN by entering the command. The range is in seconds. An aging time of 0 means that the address aging is disabled. MAC address movement is detected when the host moves from one port to another.                                                                                                                                                      |

### Virtual Private LAN Service (VPLS)

Provides Ethernet Multipoint Services over MPLS network. VPLS operation emulates an IEEE Ethernet bridge, it does the same mechanisms as traditional L2 switch - flooding,forwarding, learning, aging, MAC withdrawal upon topology changes

In a VPLS network, each site is connected to a VPLS edge device (PE), which terminate pseudowires and perform encapsulation of Ethernet frames with MPLS labels and forwards them to the other VPLS edge devices (PE). The edge devices maintain a mapping between the MAC addresses of the devices at each site and the MPLS labels for forwarding

There are two ways in which this can be accomplished. One way is to have a control plane signaling to carry information about MAC addresses between PEs. Another way is to have a scheme that is based on MAC address learning. VPLS takes the latter approach by having each PE take the responsibility for learning which remote PE is associated with a given MAC address. This way, an ingress PE simply needs to identify which frames need to be sent to egress PEs, and egress PEs take care of identifying which local ports to forward the packet to. By inspecting the source MAC address of the frame arriving on a port, whether an actual local port or a PW from a remote PE, and by creating a corresponding entry in the forwarding table, the PE learns where to send future frames with that destination MAC address.

With VPLS Layer 2 VPNs, the customer can connect with a switch or a router.

If connecting with a router, the VPLS just looks like a switch to the routing protocols on each side. No MAC learning is done beyond the MAC address of the directly connected CE router interface.

If Ethernet switches are used as CE devices and connected to PE routers, the PEs need to learn the MAC addresses of individual hosts that are attached to the switches. So, if a host is plugged into the office network that is served by a switch as a CE, the effect will be felt by all PEs. Therefore, for a large deployment, it is better to use routers as CEs than switches.

#### AC (Attachment Circuit)

Connection to CE device, it could be Ethernet physical or logical port. One or multiple ACs can belong to same VFI

#### VC (Virtual Circuit)

EoMPLS data encapsulation, tunnel label is used to reach remote PE, VC label is used to identify VFI. One or multiple VCs can belong to same VFI

#### VFI (Virtual Forwarding Instance)&#x20;

Also called VSI (Virtual Switching Instance) is a virtual Layer 2 Forwarding entity that defines the VPLS domain membership and resembles virtual switches on PE routers, it creates L2 multipoint bridging among all ACs and VCs. It’s L2 broadcast domain like VLAN

The VSI learns remote MAC addresses and is responsible for proper forwarding of the customer traffic to the appropriate end nodes. It is also responsible for guaranteeing that each VPLS domain is loop-free. The VSI is responsible for several functions, namely MAC address management, dynamic learning of MAC addresses on physical ports and VCs, aging of MAC addresses, MAC address withdrawal, flooding, and data forwarding

Multiple VFI can exist on the same PE box to separate user traffic like VLAN

![](<../.gitbook/assets/Unknown image (1457)>)

![](<../.gitbook/assets/Unknown image (1458)>)

#### Data plane

Although VPLS simulate multipoint virtual LAN service, the individual VC is still point-to-point EoMPLS. It uses the same data encapsulation as point-to-point EoMPLS

#### Loop prevention

How to avoid loop in VPLS (multipoint bridging) network? Spanning tree is possible but not desirable

VPLS relies on flooding unknown unicast frames to all participating PEs (within the broadcast domain). STP's blocking of redundant paths would interfere with this flooding process, potentially leading to connectivity issues and preventing proper learning of MAC addresses

VPLS uses MAC address learning to populate its forwarding tables. STP's blocking and re-convergence events can disrupt this learning process, leading to temporary forwarding issues and potential packet loss.

STP's convergence process (electing a root bridge, blocking ports, etc.) can take time, leading to temporary network outages or disruptions. This is undesirable in a service-oriented environment like VPLS.

Running STP in a VPLS network adds extra processing overhead on the PE routers, which are already handling significant traffic loads. This can impact overall performance and scalability.

VPLS use split-horizon to avoid loop - Packet received on VPLS VC can only be forwarded to ACs, not the other VPLS VCs (H-VPLS is exception). This also means that the PE's must have full mesh of pseudowires.

The full mesh of PWs in the service provider network allows the implementation of the split-horizon principle that is similar to the split horizon of an internal BGP mesh.

Frames that are received over a PW are generally not forwarded to another PW. This concept provides loop-free forwarding. Therefore, there is no need for STP in the service provider network. The customer STP BPDUs are transparently tunneled over the PWs to detect loops on the customer layer.

All PWs in a VFI are placed by default into the same split-horizon group, which effectively prevents traffic from forwarding to other PWs in the same VFI.

The forwarding of unicast,broadcast and multicast frames follow this same rule

LDP enhanced with additional MAC list TLV (label withdrawal)

MAC timers refreshed with incoming frames

#### Control plane

Signaling - Same as EoMPLS, using targeted LDP session to exchange VC information

Cisco IOS uses LDP to signal the setup, maintenance and teardown of a PW between PE devices. Once a PE discovers that other PEs have an association for a particular VPLS instance, the PEs signal using T-LDP to other PEs that a PW is required to be setup between PEs. When a PE device is associated with a particular VSI, LDP transmits a label-mapping message with VC-Type 0x0005 and a 4-byte VC-ID value. If the remote PE has an association with that particular VC ID, it will accept the LDP label-mapping message and respond with its own label-mapping message. Once the two uni-directional VCs are operational, they are combined to form a bi-directional PW.

There are a number of mechanisms that can be used to distribute VPLS associations between PE devices, which includes extensions to BGP version 2 (multiprotocol BGP), LDP-based, DNS-based, RADIUS-based, and static.

LDP signalling requires that each PE is identified and a targeted LDP session is active for auto-discovery; it has no inherent auto-discovery, so the pseudowires must be manually configured or some external auto-discovery mechanism must be used. The overall scalability is poor as a PE must be associated with all other PEs for LDP discovery to work, which can lead to a large number of targeted LDP sessions. LDP can signal additional attributes but additional configuration is required from NMS/OSS or static.

BGP signalling requires that a PE associated with a particular VPLS is configured under a BGP process. BGP then advertises VPLS membership information using NLRIs. Hence, BGP has inherent mechanism for auto-discovery and so frees the user from having to configure the pseudowires manually. BGP cannot easily distribute attributes such as bandwidth profiles without introducing additional overhead.

#### VPLS configuration

The VPLS deployment process includes these steps:

PE routers must have a /32 address on their Loopback interfaces. This is required for binding the VC label and the transport LDP into a label stack.

PE loopback addresses cannot be summarized in the core. Aggregation in the network would break the LSP path.

MPLS must be enabled in the core.

MTU sizes on the core links must be able to accommodate the customer maximum transmission unit (MTU) with the added label overhead.

Once the MPLS infrastructure is ready, you will:

Configure Layer 2 frame transport or Ethernet service instance.

Make sure the MTU is the same on both endpoint interfaces

Configure the bridge group and bridge domain

Assign interfaces to bridge domain.

Configure the VFI with statically defined PWs.

Optionally configure routing for the bridge domain using a bridge-group virtual interface (BVI) (Cisco IOS XR) or BDI (Cisco IOS XE).

The attachment circuit on the Cisco IOS XR PE is enabled for Layer 2 transport using the l2transport keyword. in Cisco IOS XE you configure the attachment circuit using a service instance configured under the main interface. The Cisco IOS XR VPLS configuration is performed within the Layer 2 VPN (l2vpn command), bridge group, and bridge domain configuration mode. The bridge domain contains two main configuration elements: a list of the local attachment circuits, and the VFI that contains the PWs. The Cisco IOS XE VPLS configuration consists of two constructs: the VFI and the bridge domain. The VFI is the set of pseudowires configured with the member command. The VPN ID configured in the VFI mode must match up with the VC ID configured in Cisco IOS XR with the neighbor command. The bridge domain links the attachment circuits with the VFI. The attachment circuit (service instance 111 on GigabitEthernet0/0/0) and the VFI (vfi111) are configured as members of the bridge domain (111). The names of the bridge group and domain have local significance.

![](<../.gitbook/assets/Unknown image (1459)>)

VLAN 112 is connected to PE1 (Cisco IOS XR Software). VLAN 121 is connected to PE2 (Cisco IOS XE). Each PE has a bridge domain that links the local attachment circuit with the VFI. The VFI is configured with a static configuration of the PW to the peer PE. Each PE is performing an 802.1Q tag rewrite operation on its local attachment circuit. PE1 pops one 802.1Q tag on its GigabitEthernet0/0/0/0.112 interface. PE2 pops one tag on the service instance 121 of the GigabitEthernt0/0/0 interface. The frames exchanged between the VLAN 112 and 121 are thus forwarded untagged over the PW 112 connecting PE1 and PE2.

![](<../.gitbook/assets/Unknown image (1460)>)

#### Hierarchical VPLS (H-VPLS)

Since the VPLS Split-horizon require full mesh VPLS VCs it creates the total of VCs - N\*(N-1)/2 ; N = number of PE nodes, which limits scalability for traditional VPLS

Hierarchical VPLS is an extension of standard VPLS designed to improve the scalability and management of large-scale Layer 2 VPN deployments. It introduces a two-level hierarchy to reduce signaling and replication overhead in the service provider core.

Full mesh for Core tier (Hub) only

Minimizes signaling overhead

Expansion affects new nodes only (no re-configuring existing PEs)

H-VPLS Reduces the number of PWs by dividing a VPLS network into a backbone domain and edge domains - hub-and-spoke model.

User-facing PE (u-PE): These are the spoke routers. They have an aggregation role and are responsible for some packet replication and MAC address learning. They connect directly to the customer sites. They are also called Access PE's - APE

Packets to unknown destination are replicated to all ports in the service including spoke PW. Once the MAC addresses of CE devices connected to the same u-PE device are learned, traffic between them is switched localled, saving the capacity of the spoke PW to n-PEs. Similarly, traffic between remotely connected CE devices to different u-PE devices is switched directly onto spoke PW and sent to n-PEs over the point-to-point PW.

Network-facing PE (n-PE): These are the hub routers. They connect the u-PEs to the core network and benefit from less signaling and reduced packet replication.

The n-PE will switch traffic between spoke PW, hub PWs, and ACs once it has learned the MAC addresses.

Split horizon must be disabled on the n-PE towards the u-PE's, since u-PE doesn’t connect to other U-PEs. - with no split horizon parameter

Split horizon is enabled only between N-PEs over pseudowires to avoid loops.

![](<../.gitbook/assets/Unknown image (1461)>)

RFC 4762 defines 2 mechanisms for access domain in H-VPLS:

#### H-VPLS with Q-in-Q in Access layer

S-tag is used as VPLS VFI service delimiter. Customer tag is invisible. Each Ethernet access network can have 4K customers, 4K\*4K customer vlans

Used when u-PE is directly connected to n-PEs, Q-in-Q encapsulation can be used for spoke PW.

#### H-VPLS with MPLS in Access layer

The VPLS core PWs (hub) are augmented with access PWs (spoke) to form a two-tier H-VPLS. In figure 2, 3 customer sites are connected to u-PE devices. The u-PE devices have single connection (PW) to n-PE routers. The n-PE routers are connected in a basic VPLS full mesh service. For each VPLS service, a single spoke PW is setup between u-PE and n-PE devices. Unlike traditional PWs that terminate on a physical or logical port, a spoke PW terminates on a VSI on u-PE and n-PE devices. The u-PEs and n-PEs treat each spoke connection like an attachment circuit of the VPLS service. The PW label is used to associate the traffic from the spoke PW to a VPLS instance. E.g there is another MPLS network between u-PE and n-PE

![](<../.gitbook/assets/Unknown image (1462)>)

#### PBB-VPLS (Provider Backbone Bridging)

is a scalable Layer 2 VPN technology that combines Provider Backbone Bridging (PBB) with VPLS to increase the capacity and efficiency of carrier networks. It achieves this by using MAC-in-MAC (PBB) to hide customer MAC addresses and consolidate them into fewer provider backbone MAC (B-MAC) addresses

PBB-VPLS divides the network into two domains:

Backbone VPLS (B-VPLS): This is the core network domain that transports traffic based on the provider's backbone MAC addresses (B-MACs). The B-VPLS only learns the B-MAC addresses of the provider edge (PE) routers.

Instance VPLS (I-VPLS): This is the access domain where customer MAC addresses (C-MACs) are learned. Each I-VPLS is bound to a B-VPLS and uses a unique Service Instance Identifier (I-SID) to distinguish different customer services.

#### VPLS with BGP autodiscovery

VPLS Autodiscovery enables each VPLS PE router to discover the other PE routers that are part of the same VPLS domain.

VPLS Autodiscovery also tracks when PE routers are added to or removed from the VPLS domain.

BGP store endpoint provisioning information, which is updated each time any Layer 2 VFI is configured. Prefix and path information is stored in the L2VPN database, allowing BGP to make decisions on the best path.

When BGP distributes the endpoint provisioning information in an update message to all its BGP neighbors, the endpoint information is used to configure a pseudowire mesh to support L2VPN-based services.

#### Config

Usually RR is used, so first part consists of activating the bgp rr neighbor under l2vpn vpls afi

Under l2vpn config you must specify which protocol to use for signaling either bgp or ldp

Then specify rd,vpn id, RT, and vpls id

![](<../.gitbook/assets/Unknown image (1463)>)

| l2vpn bridge group DC-RUD-GR-SDB bridge-domain VLAN2002 interface Bundle-Ether10.2002 ! vfi vfi2002 vpn-id 2002 autodiscovery bgp rd auto route-target import 106:1 route-target export 106:1 signaling-protocol ldp vpls-id 10.106.0.0:1 |                                                                                                                                                                                                                                                                                                                                                          |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bgp router-id 3.3.3.3 address-family l2vpn vpls-vpws neighbor 1.1.1.1 remote-as 100 update-source Loopback0 address-family l2vpn vpls-vpws                                                                                                | IOS-XR - you enable the signaling under address-family l2vpn vpls-vpws                                                                                                                                                                                                                                                                                   |
| show l2vpn bridge-domain autodiscovery bgp detail                                                                                                                                                                                         | RP/0/RP0/CPU0:PE22#show l2vpn bridge-domain bd-name ? VLAN620 Named bridge domain instance 'VLAN620' VLAN100 Named bridge domain instance 'VLAN100' VLAN900 Named bridge domain instance 'VLAN900' VLAN1748 Named bridge domain instance 'VLAN1748' VLAN2002 Named bridge domain instance 'VLAN2002' RP/0/RP0/CPU0:PE22#show l2vpn bridge-domain bd-name |

#### VPLS interoperability (IOS / IOS-XR signaling)

#### BGP update message

IOS Software - NLRI length is 1 byte

IOS XR Software - NLRI length is 2 bytes

BGP neighbor with VPLS-VPWS address family between IOS and IOS XR, NLRI mismatch can happen, leading to flapping between neighbors.

To avoid this conflict, IOS supports prefix-length-size 2 command that needs to be enabled for IOS to work with IOS XR. When the prefix-length-size 2 command is configured in IOS, the NLRI length is encoded in bytes.

On IOS/IOS-XE for XR interoperability:

| router bgp 1 address-family l2vpn vpls neighbor 5.5.5.2 activate neighbor 5.5.5.2 prefix-length-size 2 --------> NLRI length = 2 bytes exit-address-family |   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

<\<SP2\_CML\_LAB\_Guide\_2.2.docx>>

#### Cisco and Huawei interoperability notes (VPWS/VPLS autodiscovery)

**Interworking and VC type selection (IOS XR note)**

Interworking in IOS XR L2VPN is not a signaling feature and it does not control VC type selection; it is a data-plane adaptation function that defines how frames are translated when two attachment circuits with different Layer-2 semantics are stitched through the same pseudowire service. From first principles, a pseudowire assumes identical Ethernet service characteristics on both sides. Interworking exists only to relax that assumption when the local AC type and the remote AC type are not the same.

In IOS XR, interworking ethernet under an xconnect mp2mp instance means Ethernet-to-Ethernet adaptation without VLAN translation. It is effectively a statement that the service is homogeneous at Layer 2 and that no Ethernet-to-ATM, Ethernet-to-Frame-Relay, or VLAN-to-port semantic conversion is required. When the output shows “interworking none,” it simply means no adaptation is being performed because the AC and PW types already match. This is expected for VPLS and for Ethernet VPWS and has no bearing on whether VC Type 4 or VC Type 5 is advertised.

The important distinction is that bridge-domain VFI and xconnect mp2mp are two different service abstractions that happen to look similar in CLI but are not interchangeable. A bridge-domain with a VFI is classic VPLS. An xconnect mp2mp is VPWS multipoint, which is a BGP-signaled collection of point-to-point services sharing a common VPN ID. IOS XR treats these as fundamentally different control planes.

Your existing configuration is clearly classic VPLS: a bridge-domain, a VFI, BGP autodiscovery, and LDP signaling with FEC 129. That combination hardwires the PW type to Ethernet (VC Type 5) because the VPLS FEC does not carry VLAN semantics. The fact that the AC is a VLAN subinterface is local to the PE and terminates at the bridge-domain edge; it is not exported into the PW signaling. This is why the operational output shows “PW type Ethernet” and why no pw-class or transport-mode knob appears under the VFI.

When you move into xconnect mp2mp, IOS XR is no longer configuring VPLS at all. You are configuring VPWS with BGP autodiscovery and BGP signaling. That is why the only signaling-protocol option there is bgp and why Huawei does not support it in your environment. Huawei traditionally supports BGP autodiscovery with LDP signaling for VPLS and either static or LDP-signaled VPWS; BGP-signaled VPWS is often unsupported or implemented differently. IOS XR does not allow LDP signaling under mp2mp with BGP autodiscovery because the mp2mp construct is specifically the RFC 6624 BGP VPWS model.

This also explains why pw-class and transport-mode appear only in the xconnect hierarchy. VC Type selection is a property of VPWS, not VPLS, in IOS XR. In VPWS, each pseudowire is a service, so IOS XR lets you choose VLAN or Ethernet transport mode and thus VC Type 4 or 5. In VPLS, the service is the bridge, not the PW, so the VC type is fixed by the service model and cannot be overridden.

The interworking command you explored is therefore a red herring for your problem. It neither changes VC type nor alters Huawei interoperability. The real blocker is that your production Huawei is expecting a VLAN-based pseudowire model for what is operationally a multipoint service, while IOS XR is strictly enforcing the classic VPLS model where VLANs are local and PWs are Ethernet-typed.

In practical terms, you have only three interoperable options. Keep classic VPLS and accept VC Type 5 on both sides by changing Huawei to true Ethernet VPLS behavior. Convert the service to VPWS semantics on both sides, which allows VC Type 4 control but loses native VPLS bridging unless you emulate it. Or migrate both sides to EVPN, where VLAN identity is explicitly signaled and this entire VC Type ambiguity disappears. There is no supported configuration on IOS XR that makes a bridge-domain VFI advertise VLAN-based VC Type 4, and interworking will not change that because it operates strictly after the control plane has already fixed the PW type.

### Routed PON L2VPN scenario

This configuration demonstrates a Routed PON setup designed to achieve port-level redundancy for ONT connectivity while maintaining Layer 3 termination for reachability testing (e.g., ICMP ping). The objective is to ensure that when the primary OLT connected through interface TenGigE0/0/0/5 fails, the secondary OLT on TenGigE0/0/0/11 can immediately take over without service disruption. Both ports must share identical Layer 2 configurations so that the IOS-XR system can correctly recognize and handle the same VLAN expected from the CPE connected to the ONT

To enable this redundant Layer 2 connectivity and maintain consistent VLAN handling between both physical interfaces, a bridge-domain must be implemented under an L2VPN configuration, linking both access ports to the routed BVI100 interface that provides the Layer 3 termination point for the test network.

| interface TenGigE0/0/0/5.100 l2transport encapsulation dot1q 100 rewrite ingress tag pop 1 symmetric mtu 9198 ! interface TenGigE0/0/0/11.100 l2transport encapsulation dot1q 100 rewrite ingress tag pop 1 symmetric mtu 9198 ! interface BVI100 vrf PON ipv4 address 10.0.100.1 255.255.255.0 ! l2vpn bridge group PON100 bridge-domain 100 interface TenGigE0/0/0/5.100 ! interface TenGigE0/0/0/11.100 ! routed interface BVI100 |   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | - |

### MPLS L2VPN multihoming

For L2VPN Multihoming to work the redundant PE devices must cooperate with the access network.

Three possible solutions:

#### Access Gateway

The transport part must cooperate with the access network, as if it was part of the access network (e.g sending BPDU’s to negotiate STP blocking ports etc..)

#### Multi-chassis Etherchannel (MC-LAG/mLACP)

Redundant PE devices looking like a single device to the access network

Multi-Chassis Link Aggregation Group (ASR 9K - MC-LAG)

Multi-Chassis Link Aggregation control Protocol (Cisco 7600 - mLACP)

Note: MC-LAG connected to another MC-LAG does not work

#### Cluster

Two or more devices act as a single device to the access network ike a switch stack

Stackwise-Virtual, VSS (deprecated), vPC

Shortcomings of L2VPN Multihoming

VPLS creates a virtual Ethernet switch across an MPLS network, emulating a single LAN segment. To prevent broadcast storms and Layer 2 loops, VPLS relies on mechanisms like split-horizon rules, which prevent traffic from being forwarded back to the same pseudowire or interface it came from.

In an MC-LAG setup, multiple chassis (switches) appear as a single logical device to the downstream device (e.g., a customer switch). However, to avoid loops and duplicate traffic in the VPLS domain or from the VPLS domain to the access network, only one chassis is allowed to actively forward traffic for a given VPLS instance at a time. This leads to an active-standby configuration, where one chassis is active for forwarding traffic, and the other is in standby mode, ready to take over in case of failure.

Even with Stackwise Virtual, the VPLS core’s split-horizon rule and pseudowire setup may enforce active-standby behavior for traffic entering the MPLS network.

True active-active forwarding is achieved with EVPN-based implementations, which are designed to support multi-homing and load balancing.

#### Spanning Tree access gateway

Cisco Solution for Open L2 Ring Scenarios

Used in dual Homed scenarios

Light weight implementation of access protocols

MST Access Gateway

PVST Access Gateway

REP Access Gateway

Loop free access, with load balancing

Detect and re-converge on failures.

Faster Convergence and Better Scalability

![](<../.gitbook/assets/Unknown image (1464)>)

**Multiple Spanning Tree (MST) Access Gateway (MSTAG)**

PE (MST Gateway) sends “prepared” BPDUs to the access network.

Reacts to “Topology Change Notifications” from access.

Sends MAC withdrawal to the VPLS

| **Access Network Protocol** | **Access Gateway Variant**     |
| --------------------------- | ------------------------------ |
| MSTP                        | MST Access Gateway (MSTAG)     |
| REP                         | REP Access Gateway (REPAG)     |
| PVST+                       | PVST+ Access Gateway (PVSTAG)  |
| PVRST                       | PVRST Access Gateway (PVRSTAG) |

REPAG - is supported when the access device interfaces that connect to the gateway devices are configured with REP MSTP Compatibility mode.

PVSTAG and PVRSTAG - Topology Change Propagation is not supported

![](<../.gitbook/assets/Unknown image (1465)>)

**Resilient Ethernet Protocol (REP)**

is a Cisco propriety protocol that provides an alternative to Spanning Tree Protocol (STP) to support L2 resiliency and fast switchover with Ethernet networks

It is not even recommended - since MSTP and ITU-T handle it way better

**ITU-T G.8032 Ethernet Ring Protection Switching (ERPS)**

The ITU-T G.8032 Ethernet Ring Protection Switching feature implements protection switching mechanisms for Ethernet layer ring topologies.

This feature uses the G.8032 Ethernet Ring Protection (ERP) protocol, defined in ITU-T G.8032, to provide protection for Ethernet traffic in a ring topology, while ensuring that no loops are within the ring at the Ethernet layer.

The loops are prevented by blocking traffic on either a predetermined link or a failed link.

Uses dedicated Control Channel (VLAN) carrying control messages – Ring APS

Leverages Ethernet CFM (Connectivity Fault Management) / ITU-T Y.1731 for Fault Detection (CCM) = Connectivity Check Messages

Single Ring or Multi-Ring network topologies

Supports MAC flushing, load-balancing, revertive / non-revertive switching and administrative switching commands

![](<../.gitbook/assets/Unknown image (1466)>)

G.8032 protects against any single Link, Port or Node failure within a ring A: Failure of a port within the ring B: Failure of a link within the ring C: Failure of a node within the ring

G.8032 v2 supports multiple ERP instances over a ring

ERP instance – entity responsible for the protection of subset of VLANs carried over the physical ring

Each ERP instance is independent of other ring instances that may be configured on the ring

Each ring instance should configure its own R-APS channel, RPL, RPL Owner Node and RPL Neighbor Node

Enables load-balancing over the ring

![](<../.gitbook/assets/Unknown image (1467)>)

G.8032 v2 specifies support for a network of interconnected rings

One Major ring (closed) / multiple Sub-rings (open)

A given link must belong to a single ring

Interconnection node – node common to two or more rings (e.g. Nodes D & E)

Major Ring – Ethernet ring that is connected on two ports to Interconnection nodes (e.g. Ring A-B-C-E-D)

Sub-Ring –An Ethernet ring that is connected to other rings through Interconnection Nodes. A Sub-Ring does not constitute a closed ring (e.g. Ring D-F-G-H-E)

![](<../.gitbook/assets/Unknown image (1468)>)

Loop avoidance in G.8032

At any time, traffic may flow on all but one of the ring links (the RPL)

Under failure condition, RPL owner node is responsible to unblock its end of RPL

Once a ring port has been blocked, it may be unblocked only if it is known that there remains at least one other blocked port in the ring

Ring APS (R-APS) protocol used to coordinate protection actions over the ring

Protection algorithm-based transmission of local status and local switch requests to all nodes via R-APS

R-APS channel VLAN is always blocked at the same ring ports where channel is blocked

Except on sub-rings without R-APS virtual channel

![](<../.gitbook/assets/Unknown image (1469)>)

\[1] Ring Nodes detect link failure via Link Down Event (PHY based Loss of Signal) or timeout of CFM CCMs

\[2] Nodes adjacent to failed link block their ports and flush MAC tables

\[3] Nodes adjacent to failed link send R-APS messages with Signal Fail (SF) state onto ring

\[4] Remaining Ring Nodes receiving R-APS SF messages flush MAC forwarding tables

\[5] Upon reception of R-APS SF, RPL Owner (and RPL neighbor if present) unblocks RPL

![](<../.gitbook/assets/Unknown image (1470)>)

**ASR9k config example (multiple G.8032 / ERPS instances)**

l2vpn

ethernet ring g8032 G-RING

port0 interface GigabitEthernet0/2/0/22

monitor interface GigabitEthernet0/2/0/22.1

!

port1 none

open-ring

instance 1

profile profile1

inclusion-list vlan-ids 5,100-200

aps-channel

level 2

port0 interface GigabitEthernet0/2/0/22.1

port1 none

!

!

!

!

!

ethernet cfm

domain 'MD-ERPS' level 5

service 'MA-link' down-meps

continuity-check interval 1s

efd

!

!

!

l2vpn

bridge group RING

bridge-domain RING1

interface GigabitEthernet0/2/0/22.1

!

interface GigabitEthernet0/2/0/22.1 2transport

encapsulation dot1q 5

![](<../.gitbook/assets/Unknown image (1471)>)

#### Multi-chassis Etherchannel (MC-LAG / mLACP)

ICCP allows two or more devices to form a Redundancy Group (RG)

ICCP provides a control channel for synchronizing state between devices

ICCP rides on targeted LDP session

Various redundancy applications can use ICCP:

mLACP

Pseudowire redundancy

Standardized at the IETF: RFC 7275

ICC Protocol Transport Requirements

Reliable Message Exchange

In-order Delivery

Sequence Numbers

Timeouts/Retransmissions

use widely deployed LDP protocol.

Extend LDP with a small set of new messages:

RG Connect Message

RG Disconnect Message

RG Notification Message

RG Application Data Message

Use LDP Capability to bootstrap ICCP.

Application layer specific TLVs.

![](<../.gitbook/assets/Unknown image (1472)>)

**Node failure detection**

BFD

Detection upon loss of BFD keepalives

Requires nodes to be co-located, with a direct link connection

No split-brain protection, mandates link to be port-channel over two different line cards

/32 Route-watch

Detection upon loss of IP routing adjacency

Split-brain tie-break via MPLS network

Depends on IGP timers - OSPF/ISIS fast convergence tuning is required

Link Aggregation Control Protocol

System attributes:

System MAC address: MAC address that uniquely identifies the switch

System priority: determines which switch’s Port Priority values win

Aggregator (bundle) attributes:

Aggregator key: identifies a bundle within a switch (per node significance)

Maximum links per bundle: maximum number of forwarding links in bundle – used for Hot Standby configuration

Minimum links per bundle: minimum number of forwarding links in bundle, when threshold is crossed the bundle is disabled

Port attributes:

Port key: defines which ports can be bundled together (per node significance)

Port priority: specifies which ports have precedence to join a bundle when the candidate ports exceed the Maximum Links per Bundle value

Port number: uniquely identifies a port in the switch (per node significance)

![](<../.gitbook/assets/Unknown image (1473)>)

**Extending LACP across multi-chassis: mLACP**

mLACP uses ICCP to synchronize LACP configuration & operational state between PoAs, to provide Dual Homed device (DHD) the perception of being connected to a single switch

All PoAs use the same System MAC Address & System Priority when communicating with DHD

Configurable or automatically synchronized via ICCP

Every PoA in the RG is configured with a unique Node ID (value 0 to 7). Node ID + 8 forms the most significant nibble of the Port Number

For a given bundle, all links on the same Point of Aggregation (PoA) must have the same Port Priority

![](<../.gitbook/assets/Unknown image (1474)>)

LACP provides a mechanism by which a set of one or more links within a LAG are placed in standby mode to provide link redundancy between the devices.

This redundancy is normally achieved by configuring more ports with the same key than the number of links a device can aggregate in each LAG (due to hardware or software restrictions, or due to configuration).

For active/standby redundancy, two ports are configured with the same port key, and the maximum number of allowed links in a LAG is configured to be 1.

If the DHD and PoAs are all capable of restricting the number of links per LAG by configuration, three operational variants are possible.

DHD-based Control

> selection Active/Standby link is DHD responsibility

DHD control does not use the mLACP hot-standby state on the standby PoA, which results in higher failover times than the other variants

mLACP – DHD / PoA control

In PoA control, the PoA is configured to limit the maximum number of links per bundle to be equal to the number of links (L) going to the PoA.

The DHD is configured with that parameter set to some value greater than L.

> selection of the active/standby links becomes the responsibility of the PoA.

Shared Control (PoA and DHD)

In shared control, both the DHD and the PoA are configured to limit the maximum number of links per bundle to L—the number of links coming to the PoA. In this configuration, each device independently selects the active/standby link.

Shared control is advantageous in that it limits the split-brain problem in the same manner as DHD control, and shared control is not susceptible to the active/active tendencies

A disadvantage of shared control is that the failover time is determined by both the DHD and the PoA, each changing the standby links to SELECTED and waiting for each of the WAIT\_WHILE\_TIMERs to expire before moving the links to IN\_SYNC. The independent determination of failover time and change of link states means that both the DHD and PoAs need to support the LACP fast-switchover feature to provide a failover time of less than one second.

PoA-Based Control

Each PoA is configured to limit the maximum number of links per bundle

Limit must be set to L, where L is the minimum number of links from DHD to any single PoA

DHD max link should be set > L

In order to ensure that it is slave of the POA

This will allow faster convergence

Selection of active/standby links is the responsibility of the PoAs

Advantages: Faster switchover times compared to other variants, and Minimum Link policy on PoA can be flexible

Disadvantage: If ICCP transport is lost, Split Brain condition could occur

This is the most used variant

![](<../.gitbook/assets/Unknown image (1475)>)

mLACP Offers Protection Against 5 Failure Points:

A: DHD Port Failure

B: DHD Uplink Failure

C: Active PoA Port Failure

D: Active PoA Node Failure

E: Active PoA Isolation from Core Network

Failover Operation – Port/Link Failures

Step 1 – For port/link failures (A,B,C), active PoA evaluates number of surviving in bundle:

If >= M, then no action

If < M, then trigger failover to standby PoA

Step 2 – Active PoA signals failover to standby PoA over ICCP

Step 3 – Failover is triggered on DHD by one of:

Dynamic Port Priority Mechanism: real-time change of LACP Port Priority on active PoA to cause the standby PoA links to gain precedence

Links are either Hot-Standby or Up

Brute-force Mechanism: change the state of the surviving links on active PoA to admin down

Links are either Err-disabled or Up

Step 4 – Standby PoA and DHD bring up standby links per regular LACP procedures

Failover Operation – Node Failure

Step 1 - Standby PoA detects failure of Active PoA via one of:

IP Route-watch: loss of IP routing adjacency

BFD: loss of BFD keepalives

Step 1B – DHD detects failure of all its uplinks to previously active PoA

Step 2 – Both Standby PoA and DHD activate their Standby links per regular LACP procedures

Failover Operation – Uplink Failure

Step 1 – Active PoA detects all designated core interfaces are down

interchassis group 21

backbone interface TenGigabitEthernet4/1

backbone interface TenGigabitEthernet1/4

Really useful if no direct connection between POA

Step 2A – Active PoA signals standby PoA over ICCP to trigger failover

Step 2B – Active PoA uses either Dynamic Port Priority or Brute-force Mechanism to signal DHD of failover

Step 3 – Standby PoA and DHD bring up standby links per regular LACP procedures

**mLACP config**

| Router(config)# mpls ldp graceful-restart Router(config)# mpls label protocol ldp Router(config)# redundancy Router(config-red)# interchassis group rg-id Router(config-r-ic)# monitor peer \[bfd \| route-watch] Router(config-r-ic)# member ip ip\_address Router(config-r-ic)# backbone interface interface\_id Router(config-r-ic)# mlacp node-id node\_id | rg-id - interchassis group number same as other PoA ip\_address – Configures the IP address of the mlacp peer member group. node\_id - Defines the node ID to be used in the LACP port-id field. Valid value range is 0 - 7, and the value should be different from the peer values. |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

**mLACP config: attachment-circuit port-channel**

| Router(config)# interface Port-channel id Router(config-if)# lacp max-bundle max-bundles Router(config-if)# lacp failover {brute-force\| non-revertive} Router(config-if)# interchassis group rg-id Router(config-if)# mlacp lag-priority lag\_pri Router(config-if)# lacp fast-switchover Router(config)# interface name\_num Router(config-if)# channel-group id mode active | max-bundles - Configures the max-bundle links that are connected to the PoA. The value of the max-bundles argument should not be less than the total number of links in the LAG that are connected to the PoA. Determines whether the redundancy group is under DHD control, PoA control, or both. Range is 1 to 8. Default value is 8. brute-force – (Optional) Sets the mLACP switchover to nonrevertive or brute force. Default value is revertive (with 180-second delay). If you configure brute force, a minimum link failure for every mLACP failure occurs or the dynamic lag priority value is modified. lag\_pri - set lacp port priority for all mlacp member links. Should be same on all links on the PoA – only Active/Stanby setup is supported lacp fast-switchover - Enables LACP 1-to-1 link redundancy (only 2 ports Active/Standby). When you enable LACP 1:1 link redundancy, based on the system priority and port priority, the port with the higher system priority chooses the link as the active link and the other link as the standby link. When the active link fails, the standby link is selected as the new active link without taking down the port channel. When the original active link recovers, it reverts to its active link status. During this switch over, the port channel is also up |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**E-LAN availability model: mLACP with VPLS**

Coupled Mode (Default):

Pseudowire (PW) state follows the Attachment Circuit (AC) state.

If at least one AC in the VFI is active, then all PWs in that VFI are active.

If all ACs are standby, then all PWs go to standby.

Requires control-plane signaling to update PW status.

Slower failover due to signaling and coordination.

![](<../.gitbook/assets/Unknown image (1476)>)

Decoupled Mode:

Pseudowires always stay active, even if ACs go to standby.

The AC-PW state is independent — there’s no signaling to mark PWs standby.

Enables faster failover (no waiting for PW signaling).

But: May cause flooded/multicast traffic loss on the PE with a standby AC, because the PW is forwarding but the AC is not.

![](<../.gitbook/assets/Unknown image (1477)>)

| Router(config)# l2 vfi vfi\_name manual Router(config-vfi)# status decoupled |   |
| ---------------------------------------------------------------------------- | - |

![](<../.gitbook/assets/Unknown image (1478)>)

E-LINE availability model mLACP with VPWS

Uses Two-Way Pseudowire Redundancy

Every PE decides the local status of the PW: Active or Standby

A PW is selected as primary for forwarding if it is active on both local & remote PEs

A PW is considered as backup if it is declared as Backup by either local or remote PE

Local site access failure does not trigger LACP failover at remote site (i.e. control-plane separation between sites)

VPWS – two-way coupled:

When AC changes state to Active, both PWs will advertise Active

When AC changes state to Standby, both PWs will advertise Standby

| <p>pseudowire-class class_name<br>encapsulation mpls<br>status peer topology dual-homed</p> |   |
| ------------------------------------------------------------------------------------------- | - |

![](<../.gitbook/assets/Unknown image (1479)>)

Config example PoA1 and PoA2

![](<../.gitbook/assets/Unknown image (1480)>)

Config example PoA3 and PoA4

![](<../.gitbook/assets/Unknown image (1481)>)

**P-mLACP (Pseudo mLACP) concept**

P-mLACP provides VLAN based redundancy - allowing to configure one primary and one secondary interface pair for each member VLAN

DHD is configured with two separate port-channels aggregating to one LAG on POAs

![](<../.gitbook/assets/Unknown image (1482)>)

| PoA1 config                                                                                                                                                                                                                                                                                                                                      | PoA2 config                                                                                                                                                                                                                                                                                                                                      |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| iccp group 100 redundancy iccp group 100 member neighbor 222.222.222.222 ! backbone interface GigabitEthernet0/0/0/10 interface GigabitEthernet0/0/0/11 interface GigabitEthernet0/0/0/12 ! ! l2vpn nsr redundancy iccp group 100 multi-homing node-id 1 interface Bundle-Ether100 primary vlan 1 -100 secondary vlan 101 -200 recovery delay 60 | iccp group 100 redundancy iccp group 100 member neighbor 111.111.111.111 ! backbone interface GigabitEthernet0/0/0/10 interface GigabitEthernet0/0/0/11 interface GigabitEthernet0/0/0/12 ! ! l2vpn nsr redundancy iccp group 100 multi-homing node-id 2 interface Bundle-Ether100 primary vlan 101 -200 secondary vlan 1 -100 recovery delay 60 |

#### Other multi-chassis link aggregation solutions

Stackwise-Virtual (SwV) \[Catalyst 9k]

Virtual Switching System (VSS) \[Catalyst 6500]

Network Virtualization (nV) Cluster \[ASR 9000]

**Virtual Port-Channel (vPC)**

Virtual Port-Channel (vPC) \[Nexus product line] is a multi- chassis link aggregation mechanism with active/active redundancy model, flow-based load-balancing, Independent control plane on the peers.

Requires a dedicated interconnect between peers (vPC Peer Link) to synchronize state between peers

Carry normal flooded traffic and all traffic in case of vPC member port failure

Uses Cisco Fabric Services (CFS) protocol

Does state synchronization and configuration validation between vPC peer devices

Uses a Peer Keepalive link to detect split-brain condition

Can be either dedicated link or L3 interconnect over infrastructure

![](<../.gitbook/assets/Unknown image (1483)>)

**Virtual Switching System (VSS) / Stackwise-Virtual (SwV)**

Two physical switches appear as a single virtual device.

Single control-plane (e.g. one IP endpoint)

Single management point

Dual active forwarding planes

Requires dedicated interconnect: Virtual Switch Link (VSL)/SVL

Used for control plane synchronization and data traffic.

Uses special Virtual Switch Header for encapsulating data.

All control plane state is SSO synchronized from Active to Standby.

![](<../.gitbook/assets/Unknown image (1484)>)
