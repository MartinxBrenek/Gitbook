---
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
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

# Segment Routing

### Overview

**Segment Routing (SR)** is a label switching technology that evolves existing IP and MPLS networks and enhances traffic engineering

It uses a new and efficient way of routing which is more flexible and scalable compared to legacy MPLS LDP and RSVP technology

From a pure MPLS forwarding perspective, segment routing builds on top of the basic MPLS forwarding paradigm. The major difference between SR and legacy MPLS lies in the control plane -

SR replaces the LDP and RSVP. The label distribution mechanism is incorporated as an Type-Length-Value (TLV’s) extension into link-state routing protocols like OSPF or IS-IS

Segment routing can be directly applied to the MPLS architecture with no change in the forwarding plane.

Traditional routing is destination-based, where every router in the path makes local forwarding decision based on the destination IP and the shortest path

SR is source-based - the source node (edge router) is in charge of steering the packets and dictates the path between the source and destination

The source node encapsulates steering instructions in a packet header as an ordered list of segments that the packet should traverse to get to the destination

Those instructions are then executed by the routers participating in the SR-enabled environment

This traffic engineering utilizes the network bandwidth more effectively than traditional MPLS networks and offers lower latency.

**Segment Identifier (SID)** identifies any type of instruction (segment = label)

Service,Context,Locator,IGP-based forwarding construct,BGP-based forwarding construct,Local value or Global Index

**Segment** = instructions such as “go to node X using the shortest path”

**Segment list** is the full segment routing path from source to the destination (segment list = label stack)

![](<../.gitbook/assets/Unknown image (1289)>)

### Network simplification

A simplified network with fewer technology layers and protocols, helps to reduce complexity and potential failure scenarios as well as makes automation easier and more sustainable long-term

Key tenets of a simplified IP/MPLS network:

MP-BGP as the unified service overlay control plane

Segment Routing (SR) as the unified forwarding plane

SR-TE (Traffic Engineering) for advanced control of traffic

Centralized (SDN) Controller for network-wide orchestration of SR-TE policies

BGP-LS for export of topology link-state and TE information to Controller

BFD for fast failure detection

DiffServ QoS to isolate traffic classes and guarantee priority traffic

YANG model-driven programmability & telemetry for management and automation

Routed Optical Networking for greater network efficiencies and economics

#### Segment Routing (SR)

A programmatic IP source-routing architecture that provides the optimal balance between distributed intelligence and centralized control

Mass network simplification

Reduces control plane protocols (LDP, RSVP-TE, BGP-LU, MPLS OAM, IGP/LDP sync)

A permanent solution to LDP–IGP synchronization issue is segment routing

(SR) technology. With SR, when an IGP protocol advertises its own linkstate database to other adjacent nodes, it also advertises SR-based labels

(SIDs) at the same time. Thus, you end up with no delay in label

distribution or in traffic forwarding due to IGP or LDP process

initialization.

Unified forwarding plane for all services (IP, MPLS VPN, Ethernet, Private Line, Wave)

Automatic topology independent 50 msec FRR protection

Mass network scaling

No stateful TE tunnels throughout the infrastructure, On-Demand path instantiation

Transport route summarization between network domains (SRv6)

Advanced network capabilities

Advanced TE: e.g., intent-based, ECMP-aware, multi-domain, circuit-style, on-demand SR path instantiation (ODN), automated traffic steering, network slicing, service chaining, and integrated performance measurements

### Global Segment ID

**Global Segment ID** is a 32-bit numeric value that has significance inside the entire SR domain. Every node in the SR domain knows this value and installs it in its forwarding table

Global Segments always distributed as a label range (SRGB)+Index

Index must be unique in Segment Routing Domain

This uniqueness of the node assigned global segment allows for simpler troubleshooting and a more intuitive way of tracking the flows through the network.

In contrast to classical LDP-based MPLS, this label is likely to be different on every router along the path

In such scenarios, where the SRGB is the same on all nodes, the label swapping operation on the intermediate nodes still happens, but since the incoming and outgoing labels are the same, the actual label value does not change

Default reserved label range for Cisco nodes used for these purposes is 16000 - 23999 and is referred as Segment Routing Global Block (SRGB).

SRGB under IGP instance has precedence over SRGB in global configuration. Multiple IGP instances can use the same SRGB or use different nonoverlapping SRGBs.

Nondefault SRGB are Allocated between 16,000 and 1,048,575 (or up to platform limit)

It is strongly recommended that you use the default SRGB range on all nodes of your SR domain. Different SRGB definitions on different nodes can operate without problems when using basic functionality but may break some advanced SR functionalities such as Anycast routing. In case there is a need for redefining the SRGB range, it can be manually changed for a specific IGP.

**Node Segment ID** is a global segment ID identifying the node. It is assigned to loopback prefixes to uniquely identify the node and represent the shortest path to a node/router as determined by the IGP. It is allocated from a reserved SRGB block and retained by the node after reboot

Likewise, we can assign a segment ID to a specific prefix, called a **Prefix SID**. Index is zero-based; that is, first index = 0. Label = prefix SID index + SRGB base.

Example: Prefix 1.2.3.4/32 with prefix SID index 65 gets label 16065

Global segments are always distributed as a label range + index within IGP

![](<../.gitbook/assets/Unknown image (1290)>)

### Segment Routing Local Block (SRLB)

**Segment Routing Local Block (SRLB)** default range is 15,000 – 15,999

Range of labels used for static configuration of locally significant SIDs

(e.g. Adjacency SID, Binding SIDs)

24000 – 1048575 (220)

This ranges are reserved and used by other protocols (LDP, BGP, RSVP-TE)

![](<../.gitbook/assets/Unknown image (1291)>)

Non-default SRGB can be configured per IGP instance

Multiple IGP instances

can use the same SRGB

or use different non-overlapping SRGBs

| segment-routing global-block 18000 19999 ! router ospf 1 segment routing mpls segment-routing global-block 20000 21999 | Segment Routing Global Block can be configured in global configuration (IOS XR 6.0) SRGB under IGP instance has precedence over SRGB in global configuration |
| ---------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |

### Local Segment ID

**Local Segment ID** is a numeric value with local significance. It identifies links towards other routers, so it is used for forwarding packets within the boundaries of a single node.

As this range is only relevant for that particular node, therefore these values are not allocated using SRGB range but only via the locally configured label range

**Adjacency SID** identifies the link between adjacent nodes - automatically generated for each adjacency outside of the reserved block of node ID (SRGB)

These SID’s are advertised by OSPF or ISIS within the TLV extensions

They can also change across reboots of the router

![](<../.gitbook/assets/Unknown image (1292)>)

Local router label allocation is managed by the Label Switching Database (LSD). MPLS applications must register as client with the LSD to allocate labels. MPLS applications are, for example, IGP, LDP, RSVP, MPLS static.

Label range \[0-15] reserved for special purposes

Label range \[16-15,999] reserved for static MPLS labels - when segment routing is enabled, the 1500 label space with 1000 size gets prepended to the mpls forwarding table

Label range \[16,000-23,999] preserved for segment routing use (SRGB)

Label range \[24,000-1048575] used for dynamic label allocation

### SR data plane

A segment is encoded as an MPLS label. An ordered list of segments is encoded as a stack of labels. The segment to process is on the top of the stack. The related label is popped from the stack, after the completion of a segment.

A node imposes a prefix-SID label on a packet if:

The destination itself, or the next-hop that the destination resolves on, matches a FEC with a Prefix-SID

The downstream neighbor is SR enabled

The node is configured to prefer SR label imposition or the matching FEC

It does not have an associated LDP label

Supports PHP and Explicit-Null functionalities

Default: PHP is enabled

Explicit-Null label can be enabled"

Full interoperability with standard LDP-based MPLS

An LSP can seamlessly traverse multiple LDP and SR islands

If both SR and LDP labels exist for a destination, LDP is preferred. Use the SR-prefer functionality to prefer SR label switched paths over LDP ones. This process can also be used as the LDP to SR migration mechanism

![](<../.gitbook/assets/Unknown image (1293)>)

![](<../.gitbook/assets/Unknown image (1294)>)

Example

Node R4 prefix 1.1.1.4/32 will have the same label (Segment ID), 16004. This label will be attached to it on all nodes in your network. In such scenarios, where the SRGB is the same on all nodes, the label swapping operation on the intermediate nodes still happens, but since the incoming and outgoing labels are the same, the actual label value does not change

![](<../.gitbook/assets/Unknown image (1295)>)

Node 4 requests the default PHP functionality (noPHP-flag=0, ExpNull-flag=0)

Explicit-Null behavior is configurable for a prefix-SID

If the originator of a Prefix-SID wants to receive the packet with the original EXP/TC bits (e.g. QoS pipe model), he can set the E-flag in the Prefix-SID. This causes the penultimate-hop to swap the prefix-SID with explicit-null (hence preserving the EXP/TC) instead of popping the Prefix-SID

To set the E-flag on a locally originated Prefix-SID:

| router isis 1 interface Loopback0 address-family ipv4 unicast prefix-sid absolute 16004 explicit-null |   |
| ----------------------------------------------------------------------------------------------------- | - |

### SR control plane

Segment routing was designed based on many principles, but the key principle is simplicity. Often, the simplest way forward is to do more with what you already have installed within your network. In this case, it is utilizing your existing IGP, IS-IS, or OSPF to pass MPLS labels and segments, instead of relying on an extra label distribution protocol such as LDP or RSVP.

SR features a less complex control plane using fewer protocols:

IGP (IS-IS or OSPF)

BGP-LU

LDP and RSVP are no longer needed! The role of other protocols that are designed specifically for label exchange and setup (LDP, RSVP) has been relegated to the IGP, making such protocols obsolete and, at the same time, simplifying greenfield SR deployments.

Optional use of an SDN controller

Facilitates multidomain SR-TE with the use of BGP-LS and PCEP

It integrates with the rich multi service capabilities of MPLS, including Layer 3 VPN (L3VPN), Virtual Private Wire Service (VPWS), Virtual Private LAN Service (VPLS), and Ethernet VPN (EVPN).

#### Control plane with IS-IS

IS-IS has been modified to support segment routing in both the IPv4 and IPv6 control planes. IS-IS type, length, and value (TLV) extensions have been added to provide the necessary support for segment routing. The TLV extensions provide the necessary support for prefix segment identifiers (SIDs), adjacency SIDs, and prefix-to-SID mapping advertisements. Multiple adjacency-SIDs are supported.

IS-IS: Different Adjacency-SID for L1 and L2 adjacencies between same neighbors

IS-IS: Different Adjacency-SID for IPv4 and IPv6 address-families

For each protected P2P/LAN adjacency, IS-IS allocates two Adj-SIDs. The backup Adj-SID is only allocated and advertised when FRR (local LFA) is enabled on the interface. If FRR is disabled, then the backup adjacency-SID is released. The persistence of protected adj-SID in forwarding plane is supported. When the primary link is down, IS-IS delays the release of its backup Adj-SID until the delay timer expires. This allows the forwarding plane to continue to forward the traffic through the backup path until the router is converged.

MPLS penultimate hop popping (PHP) and explicit-null signaling

SR for IS-IS introduces support for the following (sub-)TLVs:

SR Capability sub-TLV (2) IS-IS Router Capability TLV (242)

Prefix-SID sub-TLV (3) Extended IP reachability TLV (135)

Prefix-SID sub-TLV (3) IPv6 IP reachability TLV (236)

Prefix-SID sub-TLV (3) Multitopology IPv6 IP reachability TLV (237)

Prefix-SID sub-TLV (3) SID/Label Binding TLV (149)

Adjacency-SID sub-TLV (31) Extended IS Reachability TLV (22)

LAN-Adjacency-SID sub-TLV (32) Extended IS Reachability TLV (22)

Adjacency-SID sub-TLV (31) Multitopology IS Reachability TLV (222)

LAN-Adjacency-SID sub-TLV (32) Multitopology IS Reachability TLV (222)

SID/Label Binding TLV (149)

Example:

![](<../.gitbook/assets/Unknown image (1296)>)

IS-IS Router Capability TLV (242)

SR Capability sub-TLV (2)

Type 2 Flags – 1 octet

I-Flag: MPLS IPv4 Flag. If set, then the router is capable of processing SR-MPLS-encapsulated IPv4 packets on all interfaces.

V-Flag: MPLS IPv6 Flag. If set, then the router is capable of processing SR-MPLS-encapsulated IPv6 packets on all interfaces.

One or more SRGB Descriptor entries, each of which have the following format

Range - 3 octets – number of SRGB Elements (= size)

SID/Label sub-TLV – first value of SRGB (= start)

IS-IS Router Capability TLV (135)

SR Capability sub-TLV (3) Prefix Segment Identifier

R-Flag: Re-advertisement flag. If set, then the prefix to which this Prefix-SID is attached, has been propagated by the router either from another level (i.e., from level-1 to level-2 or the opposite) or from redistribution (e.g.: from another protocol).

N-Flag: Node-SID flag. If set, then the Prefix-SID refers to the router identified by the prefix. Typically, the N-Flag is set on Prefix-SIDs attached to a router loopback address. The N-Flag is set when the Prefix-SID is a Node-SID as described in \[I-D.ietf-spring-segment-routing].

P-Flag: no-PHP flag. If set, then the penultimate hop MUST NOT pop the Prefix-SID before delivering the packet to the node that advertised the Prefix-SID.

E-Flag: Explicit-Null Flag. If set, any upstream neighbor of the Prefix-SID originator MUST replace the Prefix-SID with a Prefix-SID having an Explicit-NULL value (0 for IPv4 and 2 for IPv6) before forwarding the packet.

V-Flag: Value flag. If set, then the Prefix-SID carries a value (instead of an index). By default the flag is UNSET.

L-Flag: Local Flag. If set, then the value/index carried by the Prefix-SID has local significance. By default the flag is UNSET.

Other bits: MUST be zero when originated and ignored when received.

| RP/0/RP0/CPU0:PE1# show mpls label table detail | verify that the SR global block is used by the IS-IS process                                                                                                                                                                                           |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show isis segment-routing label table           | display the currently known prefix SIDs for both stacks. Note the four labels that are displayed belong to P2 and PE2, with an IPv4 and an IPv6 label each. You can see P2 and PE2 prefix SIDs because those routers are already preconfigured for SR. |
| show mpls forwarding labels \<label\_id> detail |                                                                                                                                                                                                                                                        |

Note Local prefix SIDs will never appear in the MPLS forwarding table

To see the local prefix SIDs of PE1, you can either run the show isis database verbose PE1 command and look for the prefix SIDs, or run the show isis segment-routing table command.

Config;

MPLS forwarding is enabled on all non-passive IS-IS interfaces

Adjacency-SIDs are allocated and distributed for all adjacencies

![](<../.gitbook/assets/Unknown image (1297)>)

#### Control plane with OSPF

OSPFv2 control plane (IPv4 only) OSPFv3 not supported

Multiarea support

IPv4 prefix segment ID (prefix-SID) for host prefixes on loopback interfaces

Adjacency segment ID (adj-SIDs) for adjacencies

OSPF: Same Adjacency-SID in all areas of Multi-Area Adjacency (multiple adjacencies, each for a different area, over same interface)

Nonprotected adj-SIDs and protected adj-SIDs

MPLS penultimate hop popping (PHP) and explicit-null signaling

OSPF Extensions for SR

OSPF extensions have been added to provide the necessary support for segment routing, including prefix segment identifiers, adjacency segment identifiers, and prefix-to-SID mapping advertisements. For OSPFv2, multiple adjacency-SIDs are supported in a similar way as for IS-IS. For each protected adjacency, OSPF allocates two Adj-SIDs if FRR is used.

LSA Type 10: Area-Scope Opaque LSA (Flooded within an OSPF Area)

This container is used to advertise information relevant to the entire area.

The Router Information LSA (Opaque Type 4) and the Extended Prefix LSA (Opaque Type 7) are placed inside Type 10 LSAs.

OSPF adds to the Router Information Opaque LSA (type 4):

SR-Algorithm TLV (8)

SID/Label Range TLV (9)

OSPF defines new Opaque LSAs to advertise the SIDs

OSPFv2 Extended Prefix Opaque LSA (type 7)

OSPFv2 Extended Prefix TLV (1)

Prefix SID Sub-TLV (2)

OSPFv2 Extended Link Opaque LSA (type 8)

OSPFv2 Extended Link TLV (1)

Adj-SID Sub-TLV (2)

LAN Adj-SID Sub-TLV (3)

![](<../.gitbook/assets/Unknown image (1298)>)

Note In a later release, SR forwarding is enabled by default. This config line will no longer be required in 6.1.x

segment-routing mpls is an ospf area command, can be applied per area

OSPF inheritence rules are applicable

segment-routing forwarding mpls is an ospf interface command, can be applied per interface

OSPF inheritence rules are applicable

In the example, SR is enabled for all interfaces in area 0, except Gi0/0/0/0

| router ospf 1 area 0 segment-routing mpls !! Area command segment-routing forwarding mpls !! Interface command interface GigabitEthernet0/0/0/0 segment-routing forwarding disable !! Interface command |   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

| PE3(config)# segment-routing mpls PE3(config)# router isis 1 PE3(config-router)# segment-routing mpls | ios-xe config |
| ----------------------------------------------------------------------------------------------------- | ------------- |

### IGP Prefix SID

IGP prefix segments represent the shortest path to the IGP prefix and are Equal-Cost Multipath (ECMP)-aware. The prefix segment is distributed through IS-IS or OSPF. Prefix SIDs are unique within the SR domain and are managed by the SRGB. Prefix SIDs are manually configured under the IGP-enabled loopback interfaces. One host route prefix, /32 for IPv4 and /128 for IPv6, can be used from the global routing table.

Node segment ID is a prefix segment that is associated with a host prefix that identifies a node. Equivalent to a router-id prefix, which is a prefix identifying a node. Node-SID is a prefix-SID with N-flag set in the advertisement. By default, each configured prefix-SID is also a Node-SID. The non node-SID prefix-SID, without the N-flag set, is configurable for IS-IS in Cisco IOS Software.

![](<../.gitbook/assets/Unknown image (1299)>)

Prefix-SID can be specified using:

an absolute value within the SRGB (“global mode”)

or an index (offset) from the lower bound of the SRGB.

| router isis 1 interface Loopback0 address-family ipv4\|ipv6 unicast prefix-sid {absolute\|index} {\|} | router ospf 1 area 0 interface Loopback0 prefix-sid {absolute\|index} {\|} |
| ----------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |

### IGP adjacency SID

Adjacency segments are local segments that have local significance. They are allocated from the dynamic label pool.

Adjacency segments are allocated automatically for each adjacency:

Per adjacency: a protected and an unprotected adjacency-SID - Segment routing traffic engineering

IS-IS: different adjacency-SID are allocated for Level 1 and Level 2 adjacencies between same neighbors

IS-IS: different adjacency-SID are allocated for IPv4 and IPv6 address-families

OSPF: the same adjacency-SID in all areas of multiarea adjacency (multiple adjacencies, each for a different area, over same interface)

Label persistency is desired -> allocate same label after failure recovery

IGP passes a label context when requesting label from LSD

IGP frees label when adjacency goes down

LSD keeps freed labels in a zombie label table for up to 30 minutes before recycling

When LSD gets a label request with the context of a zombie label, it will revive that label

No label persistency for a full chassis reload (incl. power-cycle)

No reload is required for SRGB allocation: the range becomes effective immediately upon commit in running configuration, with labels allocated from it as prefix-SIDs are assigned or advertised

By combining prefix segments and adjacency segments, you can steer traffic on any path through the network. This action allows for source routing where the path is specified in the packet header as a stack of labels. Therefore, no path is signaled and no per-flow state is created through the network.

![](<../.gitbook/assets/Unknown image (1300)>)

Shared segment

All nodes on a LAN advertise their adjacency to a pseudonode only

The pseudonode represents the network

For SR to steer traffic to each node on the LAN, an Adjacency-SID is needed to each other node on the LAN

These LAN-Adj-SIDs are associated with the adjacency to pseudonode

E.g. Node R1 will allocate and advertise a LAN Adj-SID to node R2 and one to node R3

![](<../.gitbook/assets/Unknown image (1301)>)

OSPF allocates an Adjacency-SID for each adjacency in the 2WAY state or higher

On a broadcast or NMBA network, a node advertises an Adj-SID using the Adj-SID sub-TLV for its adjacency to the DR and advertises Adj-SIDs using the LAN Adj-SID for other neighbors (e.g. BDR, DR-OTHER) on the network

RP/0/0/CPU0:xrvr-1#show ospf neighbor detail | i "interface address|State|SID”

Neighbor 1.1.1.4, interface address 66.0.0.4

Neighbor priority is 1, State is FULL, 6 state changes

Adjacency SID Label: 24002

Neighbor 1.1.1.5, interface address 66.0.0.5

Neighbor priority is 1, State is 2WAY, 2 state changes

Adjacency SID Label: 24009

Neighbor 1.1.1.3, interface address 66.0.0.3

Neighbor priority is 1, State is FULL, 6 state changes

Adjacency SID Label: 24000

RP/0/0/CPU0:xrvr-1#show isis database verbose xrvr-1

<...>

Metric: 10 IS-Extended xrvr-6.01

Interface IP Address: 99.1.6.1

Neighbor IP Address: 99.1.6.6

LAN-ADJ-SID: F:0 B:0 V:1 L:1 S:0 weight:0 Adjacency-sid:24009 System ID:xrvr-6

LAN-ADJ-SID: F:0 B:0 V:1 L:1 S:0 weight:0 Adjacency-sid:24007 System ID:xrvr-4

<...>

OSPF multi-area, IS-IS multi-level

Enabling Segment Routing does not change how multi-area, multi-level works

Prefix-SIDs are propagated between areas

Attached/associated with the propagated prefix

Some prefix-SID flags are modified when propagated

Adjacency-SIDs are not propagated between areas

When Area Border Router or L1L2 router propagates\* a non-local prefix with prefix-SID

Set ”PHP-off” flag\*\* of prefix-SID

Clear ”Explicit-null” flag of prefix-SID

IS-IS: Set “Re-advertisement” flag of prefix-SID

When Area Border Router or L1L2 router propagates a local prefix with prefix-SID

“PHP-off” flag\*\* as configured for the prefix-SID (default: PHP on)

“Explicit-null” flag as configured for the prefix-SID (default: no Exp-null)

IS-IS: Clear “Re-advertisement” flag of prefix-SID

### Terminology

advertise”:

When a node advertises a prefix, it includes that prefix in the link state advertisements it generates and sends to its neighbors

“originate”:

When a node originates a prefix, it advertises a local prefix, a prefix “owned” by the node

“propagate” (or “re-advertise”):

When a node propagates a prefix, it advertises a prefix in an area or level that it has received from another area or level, or that it has originated in another area or level

In this presentation, the node is an ABR or L1L2 router

A propagated prefix can be local or non-local

“PHP-off” flag is called P-flag in IS-IS draft, NP-flag in OSPF draft

### Anycast Prefix SID

Anycast prefix SID is used for coarse-grained traffic engineering, steering traffic via groups of routers to achieve high availability using a common anycast SID.

High availability is achieved by having two or more routers at the same location that is configured with the same anycast SID. If one of the routers fails, the policy survives. You can also add multiple locations with the same anycast SID for site redundancy and closest cost and latency routing.

Anycast prefixes: same prefix advertised by multiple nodes

Anycast prefix SID: prefix SID associated with the anycast prefix

Traffic is forwarded to one of the anycast prefix SID originators, based on best IGP path (closest router by metric).

If the closest node fails, traffic is automatically routed to the surviving closest node in the anycast group.

Note: Nodes advertising the same anycast prefix SID must have the same SRGB.

![16065 16100 A Payload PE1 16065 Payload Payload 16001 7 16100 1 160021 16065 PE2](<../.gitbook/assets/Unknown image (1302)>)

Configuration Example

On PE1, under the IS-IS process, interface Loopback 0, enable the SR transport by defining the prefix SIDs for IPv4 and IPv6. Use values of 16001 for IPv4 and 17001 for IPv6.

| RP/0/RP0/CPU0:PE1# configure RP/0/RP0/CPU0:PE1(config)# router isis 1 RP/0/RP0/CPU0:PE1(config-isis)# interface Loopback 0 RP/0/RP0/CPU0:PE1(config-isis-if)# address-family ipv4 unicast RP/0/RP0/CPU0:PE1(config-isis-if-af)# prefix-sid absolute 16001 RP/0/RP0/CPU0:PE1(config-isis-if-af)# exit RP/0/RP0/CPU0:PE1(config-isis-if)# address-family ipv6 unicast RP/0/RP0/CPU0:PE1(config-isis-if-af)# prefix-sid absolute 17001 RP/0/RP0/CPU0:PE1(config-isis-if-af)# commit RP/0/RP0/CPU0:PE1(config-isis-if-af)# end | XR |                                                                                                                                       |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -- | ------------------------------------------------------------------------------------------------------------------------------------- |
| PE3(config)# segment-routing mpls PE3(config-srmpls)# connected-prefix-sid-map PE3(config-srmpls-conn)# address-family ipv4 PE3(config-srmpls-conn-af)# 10.3.3.3/32 absolute 16003 range 1 PE3(config-srmpls-conn-af)# exit-address-family PE3(config-srmpls-conn)# end                                                                                                                                                                                                                                                    | XE | Router R21 has no SR neighbor so all transport is through "LDP" -> \[M] = Merged labels![](<../.gitbook/assets/Unknown image (1303)>) |

| RP/0/RP0/CPU0:PE1(config)# router isis 1 RP/0/RP0/CPU0:PE1(config-isis)# address-family ipv4 unicast RP/0/RP0/CPU0:PE1(config-isis-af)# segment-routing mpls sr-prefer | Note When you are running LDP and SR-based label switching at the same time, LDP will be preferred. To start using the SR label switching, you have to specifically instruct every edge node to do so For IPv6 there is no issue with preferring SR paths over LDP paths. LDP does not natively support IPv6, meaning, the label switching of IPv6 traffic takes place when you have IPv6 SR configuration in place. |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Segment routing and LDP interworking

The MPLS architecture permits concurrent usage of multiple label distribution protocols

LDP, RSVP-TE, … and SR control plane can co-exist without interaction

Each node’s Label Manager

Reserves a label range (SRGB) for SR control-plane

Ensures that all dynamic labels are outside the SRGB block

Ensures that a dynamic label is uniquely allocated

Each LSR must ensure that it can uniquely interpret its incoming labels

Adjacency segment: locally unique label allocated by the Label Manager

Prefix segment: operator ensures the unique allocation of each label within the allocated SRGB

Multiple IP2MPLS entries (e.g. LDP and SR) for the same prefix path cannot co-exist

These label imposition forwarding entries are indexed on the prefix

A forwarding table lookup returns one or more paths to the destination

Each path has a single IP2MPLS entry programmed

If multiple paths lead to the destination, each path has its own IP2MPLS entry

E.g. one path imposing an LDP label, another path imposing an SR label

For IP2MPLS forwarding, LDP XOR SR entry can be inserted into FIB

Only one IP2MPLS entry can exists for each prefix path

Default: LDP label imposition is preferred

Note Both entries are present regardless of the preference setting

To configure preference of SR entries over LDP:

| router isis 1 address-family ipv4 unicast segment-routing mpls sr-prefer ! address-family ipv6 unicast segment-routing mpls sr-prefer | router ospf 1 segment-routing mpls segment-routing sr-prefer | XE segment-routing mpls ! set-attributes address-family ipv4 sr-label-preferred exit-address-family ! |
| ------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- |

### Mapping server

For some of the internetworking models there is a requirement for an SR-mapping server. Whenever an SR enabled router is a source of the traffic and the destination for the traffic lies in the non-SR part of the network, the source SR node still needs to address that destination with a segment. The non-SR devices do not advertise prefix or adjacency SIDs in the IGP packets required for SR functioning. If there was no mapping server in the network, the originating SR node would have no reference how to send the traffic to the destination and therefore drop the traffic.

Mapping server Advertise Prefix-to-SID mappings in IGP on behalf of other non-SR-capable nodes

prefix-to-sid mappings are configured on the Mapping Server

Enable SR-capable nodes to interwork with (non-SR-capable) LDP nodes, a Mapping Server is required for SR/LDP interworking

Position of mapping server is comparable to a BGP Route-reflector:

Mapping server is a control plane mechanism

Mapping server doesn’t have to be in the data path

Mapping server must be resilient, redundancy should be provided

Q: Can I use Mapping Server for centralized SID advertisement?

A: No

A Mapping Server is intended for advertising prefix-to-SID mappings for non-SR-capable nodes

Prefix-SIDs received from a Mapping Server have an implicit PHP-off behavior: the penultimate hop will not pop the prefix-SID label

packets will arrive at the destination with prefix-SID label on top

packet with local prefix-SID as top label would require two label lookups at the receiving node to forward packet: top-label lookup, pop top-label, next-label or address lookup => performance impact

In IOS XR, no mpls forwarding entry is installed for local prefix-SIDs

packets with local prefix-SID as top label are dropped

OSPF partially removes this limitation for intra-area prefixes:

If the intra-area prefix is local to the nexthop router, then OSPF will pop the label, even if the prefix-SID is received from the mapping server

IS-IS

Mapping server advertisements are currently not propagated between levels

a mapping server is required per IS-IS area

OSPF

Mapping server advertisements are propagated between areas

The prefix-SID received via a “regular” advertisement is preferred

Note

When configuring the mapping server, ensure that the mapping index starts at a value of approximately 1000 or higher. If index 0 is used, the mapping server will advertise mapped prefixes starting from the beginning of the Segment Routing label pool, which is 16000. This label range is already allocated to SR-only loopback prefixes, and using index 0 would therefore cause an overlap and unintended label consumption.

Example:

segment-routing

mapping-server

prefix-sid-map

address-family ipv4

10.64.205.1/32 1000 range 100

### Topology-independent loop-free alternate (TI-LFA)

Topology-independent loop-free alternate (TI-LFA) is a segment routing–based fast reroute mechanism that provides sub-50 ms convergence for link and node failures. Unlike classical LFA and Remote LFA, which rely on specific SPF conditions and topology characteristics, TI-LFA computes repair paths based on the post-failure topology and encodes them using segment routing

Classic per-prefix LFA Fast Reroute is topology-dependent and attempts to find a loop-free alternate among directly connected neighbors. The alternate path must satisfy strict SPF inequality conditions to avoid loops - Is reachable without going back through the failed link/node and has a strictly lower cost to the destination than the failed path.

If no direct neighbor satisfies the loop-free condition, no protection is possible.

LFA can only use existing shortest paths and existing neighbors. It cannot force traffic onto a safe path.

Result: partial coverage, topology-dependent behavior

Remote LFA extends classic LFA by allowing traffic to be tunneled to a remote loop-free alternate (PQ node) that is more than one hop away. While this improves coverage, Remote LFA is still topology-dependent and requires the existence of a suitable PQ node. Additionally, it relies on targeted LDP sessions, increasing configuration complexity and operational overhead, and still cannot guarantee full coverage in all topologies.

Topology-independent LFA fundamentally changes the fast reroute model by no longer relying on the existence of an alternate shortest path. Instead, TI-LFA precomputes the post-convergence path after a failure and encodes that path using segment routing node or adjacency segments. This allows the repairing router to explicitly steer traffic along a safe path, as long as the post-failure topology remains connected.

Because TI-LFA is computed automatically by the IGP and does not require targeted LDP sessions or manual tunnel configuration, deployment is simpler and more scalable than Remote LFA. Repair paths are preinstalled in the forwarding plane and triggered immediately upon local failure detection, without waiting for IGP reconvergence.

TI-LFA provides full protection for both data-plane and control-plane traffic. Since the repair mechanism operates at the forwarding level, all traffic types—including IP, IGP control packets, and MPLS traffic—are protected during failure events. This makes TI-LFA a comprehensive and deterministic fast reroute solution compared to classical LFA and Remote LFA.

Post-convergence path: path that will be used after IGP has converged following a failure

When a failure occurs, the traffic is fast-rerouted onto the path that it will also follow after IGP has converged. Because of the traffic-steering capabilities of SR, you can always use the post-convergence path, and you can do so without creating any additional state on the nodes of the backup path. If the post-convergence path would loop the traffic back to you, SR will force the traffic over the path by encoding it as an explicit list of segments, the Explicit Post Convergence path.

In this network, a provider edge router (PE4) is connected to the core routers. The links to the provider edge router have high metrics, because they are low-capacity links and should not be used for transiting traffic. Node 2 is the point of local repair (PLR), which protects the traffic traversing the link to 3. When you use LFA, by default it selects PE4 as the LFA to protect the traffic to 5 over link 2-3. When the link fails, traffic to 5 is sent across PE4 and the low-capacity links to PE4, possibly congesting these links. TI-LFA protects the traffic by using the post-convergence path.

TI-LFA selects node 7 as an intermediate node, the so-called PQ node (a node that is a member of both the extended P-space and the Q-space), to steer the traffic over the core links to destination 5. TI-LFA automatically selects this more optimal path, without any additional tuning or manual intervention.

![](<../.gitbook/assets/Unknown image (1304)>)

For each adjacency two Adj-SIDs are advertised

Adj-SID with Backup-flag=1

This Adj-SID may be protected

Note: Adj-SID with B=1 is advertised even if Adj-SID is not actually protected

Adj-SID with Backup-flag = 0

This Adj-SID is not protected

In case of failure, the traffic is switched to other converged path (no FRR)

Example use case: TDM based service

If a link fails then the end-to-end path must fail, higher layer provides redundancy

With path-selection for SR-TE tunnel, you can select the tunnel to use:

Only protected Adj-SIDs

Only unprotected Adj-SIDs

The default is to use any Adj-SID, preferring the unprotected ones

Routes pointing TE tunnel do not have backup paths in RIB, because tunnel itself has backup

![](<../.gitbook/assets/Unknown image (1305)>)

The implementation of TI-LFA uses the concepts and calculations of the P and Q spaces.

P space: A set of nodes reachable from node S (point of local repair) without using protected link L.

Q space: A set of nodes that can reach destination D without using the protected link L.

P node: Any node that is only in the P space.

Q node: Any node that is only in the Q space.

PQ node: A node that is a member of both the extended P space and the Q space

Extended P-space are the set of routers that the neighbors can reach without traversing the protected path.

The extended P space is the combination of the P spaces of all neighbors of the source S. The extended P space exists because source S has full control over the first hop of the backup path. Regardless of the metrics, the source S can always send packets to a specific neighbor. What is most important is which nodes the neighbors of the source S can reach.

The calculation or selection of P and Q nodes is designed to prevent looping scenarios in the network. This means that if the PLR sends traffic over the backup path, the nodes in the backup path should not send the packet back to the PLR. To achieve this, the PLR pushes the SID stack by selecting P,Q nodes to ensure traffic is forwarded correctly over the backup path.

How can you steer the packets to the Q node? You use a list of segments to steer the packets to this Q node on the post-convergence path.

What happens once you enable TI-LFA protection for an interface?

1. Run rSPF for post-convergence path by removing the protected link from the graph.
2. Select the P-space
3. Select the extended P-space
4. Select the Q-space
5. If a PQ node is available on the post-convergence path, use the Prefix-SID for the PQ node.
6. If no PQ node is available on the post-convergence path, use the P and Q nodes, then push the Prefix-SID for the P node and the Adj-SID between the P and Q nodes.
7. Push the backup path into the FIB with SID values, then conclude the TI-LFA process.

Note by default two MPLS labels gets assigned to single IP address, this is Because of PROTECTED and UNPROTECTED Adjacency. In this case there is no protection configured yet so we have 2 MPLS labels allocated for 1 adjacency doing the same (UNPROTECTED) ... but only one label is propagated by IGP (ISIS)

![](<../.gitbook/assets/Unknown image (1306)>)

![](<../.gitbook/assets/Unknown image (1307)>)

#### Zero-Segment Implementation

A zero-segment backup path is a post-convergence path that requires no additional segments.

For the zero-segment backup path, the topology is a ring topology with traffic traveling from A to Z. The point of local repair is R1, and it is protecting the link to R2. All link metrics are 10, except the link R1-R5, which is 1000.

The link between R1 and R2 fails. A packet with destination Z arrives on R1. If it is an unlabeled packet, R1 pushes the prefix-SID label of Z as usual, and R1 sends the packet on the backup path toward R5. If the packet is labeled, R1 simply forwards it toward R5 without pushing any additional label. The packet goes all the way to destination Z.

This behavior is applied on a per-prefix basis. Therefore, for each prefix, the primary link changes and the post-convergence path is computed accordingly, together with the P and Q properties. The algorithm is proprietary (local behavior that is not in the scope of IETF standardization) and scales extremely well.

![](<../.gitbook/assets/Unknown image (1308)>)

RP/0/0/CPU0:iosxrv-1# show ospf 1 routes 10.1.1.6/32 backup-path

RP/0/0/CPU0:iosxrv-1# show isis ipv4 fast-reroute 10.1.1.6/32 detail

{% hint style="info" %}
To validate functional TI-LFA use show ospf backup-path or show isis fast-reroute sr-only, you can also use basic show ip route or show ip cef to observe backup path.
{% endhint %}

#### Single-Segment Implementation

The single-segment backup path within TI-LFA is used when a zero-segment path is not possible, because the direct neighbor causes a loop.

The topology in the figure illustrates the single-segment TI-LFA backup path and is similar to the topology used in the previous example. The only difference is that, in this topology, all link metrics are equal.

The point of local repair is R1 and it is protecting the link to R2.

Notice that, with these metrics, R1 cannot send the packets toward R5 for protection, because R5 will loop the packets back to R1, due to the metrics (R5 is not an LFA in this case).

TI-LFA at R1 starts by calculating the P and Q spaces, which in this case overlap and include both R3 and R4. These nodes, present in both spaces, are referred to as PQ nodes.

R1 steers the traffic through R4 by pushing the prefix SID of R4 on the packet.

Upon failure of the link, R1 steers the traffic destined to Z through R4.

R4 is in the Q space, so R4 can reach the destination without crossing the protected link.

![](<../.gitbook/assets/Unknown image (1309)>)

#### Double-Segment Implementation

The double-segment backup path within TI-LFA is used in more complex scenarios where single-segment is not sufficient.

TI-LFA selects a P node, R4, and an adjacent Q node, R3, on the post-convergence path.

R1 installs the backup path through P and Q nodes R4 and R3 in its forwarding table.

R1 steers the traffic to R4 by pushing the prefix SID of R4 on the packet.

The packet is forced over the high-metric link to R3 using the adjacency SID or R4 toward R3.

From R3, the packet follows the post-convergence path to the destination.

![](<../.gitbook/assets/Unknown image (1310)>)

#### TI-LFA and segment routing LDP interworking

TI-LFA and segment routing LDP can be used to protect each other's paths. When protecting LDP with TI-LFA, the backup path for LDP MPLS-to-MPLS uses LDP labels if they are available. When protecting segment routing with TI-LFA, the backup path for segment routing MPLS-to-MPLS uses segment routing labels if the next hop is segment routing-capable.

Meaning that the TI-LFA can be deployed and work in heterogenous network for the same implementations mentioned above

If the point of local repair has LDP enabled and TI-LFA has calculated a PQ node for backup, the point of local repair sends targeted LDP (tLDP) hellos to PQ.

If PQ accepts tLDP hellos, an LDP session is established between the point of local repair and PQ.

If the point of local repair receives an LDP label for destination D from the PQ, it uses that label in the backup path for LDP traffic.

Segment routing/LDP interworking mechanism works for all labels on the TI-LFA backup path

![](<../.gitbook/assets/Unknown image (1311)>)

### Microloop avoidance

IP hop-by-hop routing may induce microloops (uLoops) at any topology transition. Microloops are a day-one IP challenge. Microloops are brief packet loops that occur in the network following a topology change:

Link down or up (remote or local)

Metric increase or decrease (remote or local)

OSPFv2 only — Single-node cost-out: This occurs when a Router LSA (Link State Advertisement) is received with all non-stub links set to the maximum metric (link cost of 65535). It indicates that the node is being taken out of the routing path by setting the links to an unreachable state.

OSPFv2 only — Single-node cost-in: This occurs when a Router LSA is received that brings the node back into the routing path by changing at least one non-stub link from the maximum metric mode.

Microloops are caused by the non-simultaneous convergence of different nodes in the network. If a node converges and sends traffic to a neighbor node that has not converged yet, traffic may be looped between these two nodes, resulting in packet loss, jitter, and out-of-order packets.

Segment Routing can be used to resolve the microloop problem. A router with the Segment Routing Microloop Avoidance feature detects if microloops are possible for a destination on the post-convergence path following a topology change associated with a remote link event.

If a node determines that a microloop could occur on the new topology, the IGP computes a microloop-avoidant path by updating the forwarding table and temporarily (based on a RIB update delay timer) installing the SID-list imposition entries associated with the microloop-avoidant path for the destination. Traffic is steered to that destination loop-free.

After the RIB update delay timer expires, IGP updates the forwarding table and removes the microloop-avoidant SID list. Traffic now natively follows the post-convergence path.

SR microloop avoidance is a local behavior and therefore not all nodes need to implement it to get the benefits.

Before Ti-LFA

Only for local link-down event

Microloop avoidance = “use backup path in case of local link-down” + “transition to post convergence (PC) with a delay”

Now:

Local and remote – link-down and link-up events

Microloop avoidance = {

Stage 1: At time of learning remote event: compute forced SR path over new PC

Stage 2: Regular convergence (same path as stage 1) }

Must be configured:

| microloop avoidance segment-routing | either local or SR microloop avoidance is enabled |
| ----------------------------------- | ------------------------------------------------- |

| microloop avoidance rib-update-delay \[delay] | Default 5 second |
| --------------------------------------------- | ---------------- |

### Segment Routing Traffic Engineering (SR-TE)

SR-TE and Resource Reservation Protocol Traffic Engineering (RSVP-TE) are two methods that are used to manage and optimize traffic flow in networks. RSVP-TE uses signaling protocols to establish LSPs, requiring the maintenance of state information at each hop along the path. In contrast, SR-TE uses a source routing approach where the ingress node defines the path by appending a sequence of SIDs to the packet header, eliminating the need for per-hop state maintenance. This distinction results in differences in scalability, operational complexity, and compatibility with modern networking paradigms such as software-defined networking (SDN).

SR-TE provides a simple, automated, and scalable architecture to engineer traffic flows in a network. The concept of traffic engineering is the same, already seen in legacy Cisco MPLS TE applications

Traffic engineering state only at headend: In SR-TE, only the ingress node (headend) maintains the traffic engineering state, reducing the burden on intermediate nodes.

Engineering for SDN: SR-TE aligns well with SDN principles, offering centralized control and simplified network management.

Path determination: The SR-TE policy is based on a SID list, where each segment can follow the IGP Equal-Cost Multipath (ECMP) if available. In contrast, RSVP-TE tunnels adhere to a circuit model, establishing a fixed path through the network.

SR-TE can be deployed using a centralized controller or can be distributed on individual nodes within the network. SR-TE paths can be computed locally or distributed on the nodes in the network or by a centralized controller.

"Centralized" does not necessarily mean a complete state-of-the-art SDN controller. It may be simply a conventional stateful PCE running on a physical or virtualized router.

Similar to traditional RSVP-based Cisco MPLS TE tunnels, segment routing policies are unidirectional in nature and can be either intra-area or interarea. One distinguishing factor is the multitude of ways in which a segment routing policy can be instantiated. Available options include the following:

Configurable from the CLI

Initiated by an SDN controller (PCE)

Netconf/YANG

Tunnel interface-based configurations, as seen with RSVP-based MPLS tunnels, are deprecated.

![](<../.gitbook/assets/Unknown image (1312)>)

#### SR-TE policy

SR-TE policies provide granular control over packet forwarding paths within a network by defining specific routing instructions through a sequence of SIDs. These policies consist of various components, constraints, metrics, and attributes that collectively determine how traffic is managed and routed.

An SR-TE policy path is expressed as a SID list. - The list of segments that specifies the path

If a packet is steered into an SR-TE policy, the SID list is pushed on the packet by the headend.

The rest of the network executes the instructions that are embedded in the SID list (source routing).

An SR-TE policy is identified as an ordered list (headend, color, endpoint):

Head-End: The ingress node where the SR-TE policy is instantiated, responsible for applying the SID list to packets.

Color: A numerical value that distinguishes between two or more policies to the same node pairs (headend: endpoint). Every SR-TE policy has a color value. Every policy between the same node pairs requires a unique color value. Color can be used to indicate a certain treatment (SLA, policy) provided by an SR policy. This attribute helps to separate and identify different classes of traffic, ensuring that routing decisions are aligned with specific quality of service and operational requirements.

Only one SR policy with a given color (C) can exist between a given node pair (headend \[H], endpoint \[E]). In other words: each SR policy triplet (H, C, E) is unique.

Endpoint: The destination of the SR-TE policy

Candidate Path: A potential path comprising a SID list, which can be dynamic or explicit, representing one way to reach the endpoint.

Binding Segment: A mechanism that assigns a Binding SID (BSID) to an SR-TE policy, allowing it to be referenced as a single segment. This simplifies policy management and enables hierarchical policies by nesting SR-TE policies within others.

Constraints: Rules that define specific requirements or limitations for path selection, such as avoiding certain links or nodes.

#### SR-TE metrics

SR-TE policies utilize various metrics to determine optimal paths through the network.

These metrics guide the path computation process to meet specific performance objectives:

Hop Count: Minimizes the number of hops a packet traverses, aiming for the shortest path in terms of node count.

IGP Metric: Utilizes Interior Gateway Protocol metrics to find the path with the lowest cost based on routing protocol calculations.

Latency: Prioritizes paths with the lowest end-to-end delay, essential for time-sensitive applications.

Traffic Engineering (TE) Metric: Employs TE-specific metrics that may consider factors like bandwidth availability and link utilization to optimize the path.

#### SR-TE constraints

Constraints define specific conditions that must be met when selecting paths for an SR-TE policy. They ensure that the selected path meets the performance and policy requirements of the network.

Common SR-TE constraints include the following:

Affinity: Specifies preferred or disallowed paths based on link attributes, such as including or excluding certain link colors.

Bandwidth: Ensures that the selected path can accommodate the required bandwidth for the traffic flow.

Administrative Groups: Uses administrative tags to include or exclude links from path consideration.

Shared Risk Link Group: Avoids paths that share common risk factors, enhancing fault tolerance

Disjointness: Ensures path diversity by selecting routes that are disjoint from other paths, improving redundancy.

Delay Variation: Limits acceptable delay variation to maintain quality of service for sensitive applications.

SR-TE supports both dynamic and explicit path selection. Dynamic paths are automatically computed based on defined constraints and metrics, adapting to network changes to maintain optimal routing. Explicit paths, on the other hand, are manually specified by the network operator, providing precise control over the exact route that traffic will take through the network.

In SR-TE, policy management can be either distributed or centralized, with each approach offering distinct advantages:

Distributed Policies: In this model, each head-end node independently computes paths based on locally available information and predefined constraints. This approach improves scalability and reduces dependence on a centralized entity, allowing for rapid adaptation to local network changes. However, it can result in suboptimal global resource utilization due to the lack of a comprehensive network view.

Centralized Policies: Here, a centralized controller, often part of a software-defined networking (SDN) architecture or simply a conventional stateful path computation element (PCE) running on a physical or virtualized router, computes paths for all nodes in the network. The controller has a holistic view of network health, enabling globally optimized path calculations that consider overall resource utilization and traffic patterns. This centralized approach facilitates consistent policy enforcement and simplifies complex traffic engineering tasks. However, it introduces a single point of failure and can present scalability challenges in very large networks.

#### Configuring segment routing policy

Cisco IOS XR Software sorts its configuration based on processes. All segment routing-based policies are configured within the segment routing traffic engineering portion of the configuration.

Components of the policy configuration include the following:

Policy name

Color tuple: While the policy name is how you may reference the policy in the configuration, the color and endpoint tuple are what make the policy unique on the headend.

Binding SID: The binding SID is an optional attribute. You can either define a SID value from the static range of labels (16 through 15,999) or not configure it to activate the default behavior of dynamically assigning the label value.

Candidate path: Within the candidate paths, you can specify one or more potential paths by specifying each with a different preference value. The higher the preference value, the more preferred the path option.

To specify an explicit path, a segment list is used. The segment list can also be created within the segment-routing traffic-eng configuration section. One of the two types of values can be used to create a segment list.

The first option is to use IP addressing. In the example, you can see three hops outgoing interface from the first SID to Node 2 defined with the use of IP addressing:

Loopback IP addresses will be converted to the prefix SIDs.

Next-hop interface IP addresses are converted to adjacency SIDs.

segment-routing

traffic-eng

policy POLICY1

color 1 end-point ipv4 10.1.1.4

candidate-paths

preference 100

explicit segment-list SIDLIST1

segment-list name SIDLIST1

index 10 address ipv4 10.1.1.2

index 20 address ipv4 10.2.3.3

index 30 address ipv4 10.1.1.4

The second option is to utilize MPLS labels to directly configure the SID list. In the example, you can see three hops outgoing interface from the first SID to Node 2 defined with the use of MPLS labels:

Prefix SIDs

Adjacency SIDs

The two methods can be mixed and matched by using IP addressing and MPLS labels.

MPLS labels must be used for destinations in interdomain policies where the topology database has no information to convert IP addresses to segments.

It is also possible to specify a binding SID value as a last hop within the segment list.

segment-routing

traffic-eng

policy POLICY2

color 2 end-point ipv4 10.1.1.4

candidate-paths

preference 100

explicit segment-list SIDLIST2

segment-list name SIDLIST2

index 10 mpls label 16002

index 20 mpls label 30203

index 30 mpls label 16004

Example:

On the PE1 router, enable MPLS traffic engineering and configure OSPF to feed the SR-TE database with link state information.

RP/0/RP0/CPU0:PE1(config)# mpls traffic-eng

RP/0/RP0/CPU0:PE1(config-mpls-te)# router ospf 1

RP/0/RP0/CPU0:PE1(config-ospf)# distribute link-state

RP/0/RP0/CPU0:PE1(config-ospf)# commit

RP/0/RP0/CPU0:PE1(config)# router isis 1

RP/0/RP0/CPU0:PE1(config-isis)# distribute link-state

On the PE1 router, configure loopback 0 as a TE Router ID and enable MPLS TE support in the OSPF area 0.

| RP/0/RP0/CPU0:PE1(config)# router ospf 1 RP/0/RP0/CPU0:PE1(config-ospf)# mpls traffic-eng router-id Loopback 0 RP/0/RP0/CPU0:PE1(config-ospf)# area 0 RP/0/RP0/CPU0:PE1(config-ospf-ar)# mpls traffic-eng RP/0/RP0/CPU0:PE1(config-ospf-ar)# commit                                                                             | PE3(config)# router ospf 1 PE3(config-router)# mpls traffic-eng router-id Loopback0 PE3(config-router)# mpls traffic-eng area 0  |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| RP/0/RP0/CPU0:PE1(config)# router isis 1 RP/0/RP0/CPU0:PE1(config-isis)# address-family ipv4 unicast RP/0/RP0/CPU0:PE1(config-isis-af)# mpls traffic-eng router-id loopback 0 RP/0/RP0/CPU0:PE1(config-isis-af)# mpls traffic-eng level-2-only RP/0/RP0/CPU0:PE1(config-isis-af)# commit RP/0/RP0/CPU0:PE1(config-isis-af)# end | PE3(config)# router isis 1 PE3(config-router)# mpls traffic-eng router-id Loopback0 PE3(config-router)# mpls traffic-eng level-2 |

RP/0/RP0/CPU0:PE1# configure terminal

RP/0/RP0/CPU0:PE1(config)# segment-routing

RP/0/RP0/CPU0:PE1(config-sr)# traffic-eng

RP/0/RP0/CPU0:PE1(config-sr-te) # policy POLICY1

RP/0/RP0/CPU0:PE1(config-sr-te-policy)# color 20 end-point ipv4 10.1.1.4

RP/0/RP0/CPU0:PE1(config-sr-te-policy)# binding-sid mpls 1000

RP/0/RP0/CPU0:PEl(config-sr-te-policy)# candidate-paths

RP/0/RP0/CPU0:PE1(config-sr-te-policy-path)# preference 100

RP/0/RP0/CPU0:PE1(confiq-sr-te-policy-path-pref)# dynamic

RP/0/RP0/CPU0:PEl(config-sr-te-pp-info)# metric type te

RP/0/RP0/CPU0:PE1(config-sr-te-pp-info)# exit

RP/0/RP0/CPU0:PE1(config-sr-te-policy-path-pref)# constraints

RP/0/RP0/CPU0:PE1(confiq-sr-te-path-pref-const)# affinity

RP/0/RP0/CPU0:PE1(confiq-sr-te-path-pref-const-aff)# exclude-any color red

RP/0/RP0/CPU0:PE1(config-sr-te-path-pref-const-aff)# exit

RP/0/RP0/CPU0:PEI(config-sr-te-path-pref-const)# exit

RP/0/RP0/CPU0:PE1(config-sr-te-policy-path-pref)# exit

RP/O/RP0/CPU0:PE1(config-sr-te-policy-path)# preference 200

RP/0/RP0/CPU0:PE1(confiq-sr-te-policy-path-pref)# explicit segment-list SIDLIST1

RP/0/RP0/CPU0:PE1(config-sr-te-po-info)# exit

RP/0/RP0/CPU0:PEl(config-sr-te-policy-path-pref)# exit

RP/0/RP0/CPU0:PE1(config-sr-te-policy-path)# exit

RP/0/RP0/CPU0:PE1(config-sr-te-policv)# exit

RP/0/RP0/CPU0:PE1(config-sr-te)# segment-list name SIDLIST1

RP/0/RP0/CPU0:PEl(config-sr-te-sl)# index 10 mpls label 16002

RP/0/RP0/CPU0:PEl(config-sr-te-sl)# index 20 mpls label 16003

RP/0/RP0/CPU0:PEl(confiq-sr-te-sl)# index 30 mpls label 16004

RP/0/RP0/CPU0:PE1(config-sr-te-sl)# exit

RP/0/RP0/CPU0:PE1(config-sr-te)# affinity-map

RP/0/RP0/CPU0:PE1(config-sr-te-affinity-map)# red bit-position 0

RP/0/RP0/CPU0:PE1# ping sr-mpls nil-fec policy binding-sid 24004 source 10.1.1.1

RP/0/RP0/CPU0:PE1# traceroute sr-mpls nil-fec policy binding-sid 24004

#### Policy traffic steering

Traffic steering mechanisms include several approaches:

Locally programmed: Direct, on-device control.

Remotely programmed: Centralized policy management.

Classic mechanisms (autoroute, pbts, static route): Proven, traditional routing methods can also be used but are not the primary mechanisms for SR-TE.

Note A binding SID can also be allocated for RSVP-TE tunnels (configurable).

Binding SIDs (BSIDs) is a local label identifying an SR-TE policy, it is statically or dynamically allocated for each SR-TE policy by default. It is a fundamental building block of SR-TE that steer traffic into the SR-TE policy and across domain borders, creating seamless end-to-end interdomain SR-TE policies. Each domain controls its local SR-TE policies; local SR-TE policies can be validated and rerouted if needed, independent from the remote domain's headend. Using binding segments isolates the headend from topology changes in the remote domain. Packets received with a BSID as top label are steered into the SR-TE policy that is associated with the BSID. When the BSID label is popped, the SR-TE policy's SID list is pushed.

![](<../.gitbook/assets/Unknown image (1313)>)

The SR-TE policies can be nested within another layer of policies using the BSIDs, resulting in seamless end-to-end SR-TE policies. When the policy crosses domain boundaries, the path is a chain of SR-TE policies that are stitched together using the binding-SIDs of intermediate policies, providing a seamless end-to-end path. When stitching policies together, the local headend pushes two SIDs {remote headend, binding SID}, and the remote headend pops the binding SID and steers in the related tunnel.

The instruction that is associated with a binding segment is "Pop and steer into SR-TE policy."

Binding SID is reported to PCE in PCReport object for SR-TE policy. On-Demand Next Hop (ODN) functionality uses the binding SID of an SR-TE policy to steer traffic into that SR-TE policy. Binding SID label has local significance to the ingress node of the corresponding TE path. When a stateful PCE is deployed for setting up TE paths, it may be desirable to report the binding label or SID to the PCE for enforcing end-to-end TE policy.

Forwarding Plane - A valid SR Policy installs a BSID-keyed entry in the forwarding plane with the action of steering the packets matching this entry to the selected path of the SR Policy.

The SR-TE policy path goes from node 1 to node 10 via node 4.

Node 1 allocates a binding SID (24012) for the SR-TE policy.

![](<../.gitbook/assets/Unknown image (1314)>)

In a segment-routing TE (Traffic Engineering) tunnel environment, when a link is lost and there is no secondary backup path in place, the following happens:

The headend router detects the loss of the primary path.

Since there is no secondary path configured, the headend router starts an invalidation timer.

During the invalidation timer period, the headend router continues to forward traffic using the stale label stack.

Once the invalidation timer expires, the headend router declares the tunnel as down and stops forwarding traffic on the tunnel.

SR-TE Autoroute is a feature that automatically routes traffic into an SR-TE policy based on the prefixes learned by the IGP. Instead of always following the IGP's shortest path, the router can be configured to "include" certain prefixes in the SR-TE policy. If a prefix is eligible and listed in the Autoroute configuration, the router installs its route with the SR-TE policy as the next-hop, overriding the default IGP path. This feature may be applied to all prefixes (using autoroute include all) or only to specific prefixes (using autoroute include ipv4 ). Also, the route metric (either absolute or relative) can be adjusted to influence path selection and prevent unintended load balancing.

segment-routing

traffic-eng

policy POL-100

autoroute

include ipv4 172.16.100.0/24

!

!

policy POL-200

autoroute

include ipv4 172.16.200.0/24

![](<../.gitbook/assets/Unknown image (1315)>)

### SRLG-aware path computation

Shared Risk Link Groups (SRLGs) are groups of interfaces that are likely to fail simultaneously because they share a common physical resource or conduit or rely on the same power source. For example, multiple VLAN interfaces configured on the same physical interface form an SRLG. If one of these logical interfaces fails due to a physical fault, the other interfaces in the group are likely to fail as well. SRLG information is flooded by the IGP.

In SRLG-constrained path computation, the headend or PCE first identifies the SRLG values that are associated with links used by existing LSPs using information that is disseminated via the IGP. During the computation, any candidate path that contains links with overlapping SRLGs is excluded, ensuring that the new LSP avoids common physical risks and maintains disjointness from previously established LSPs.

![](<../.gitbook/assets/Unknown image (1316)>)

Existing loop-free alternate implementations in IGPs support SRLG protection; however, they consider only directly connected links when computing the backup path. Therefore, SRLG protection may fail if a nondirectly connected link sharing the same SRLG is included in the backup path computation. The global weighted SRLG protection feature enhances path selection by assigning weights to SRLG values and incorporating these weights during backup path computation.

To support global weighted SRLG protection, information about SRLGs on all links within the area topology is required. SRLG data for remote links can be propagated using IS-IS or configured manually on those links.

This example shows how to configure the local SRLG with a global weighted SRLG protection feature.

router isis 1

address-family ipv4 unicast

fast-reroute per-prefix srlg-protection weighted-global

fast-reroute per-prefix tiebreaker srlg-disjoint index 1

interface Gi0/0/0/0

point-to-point

address-family ipv4 unicast

fast-reroute per-prefix

fast-reroute per-prefix ti-lfa

srlg

name group1

admin-weight 5000

The remote router configuration for global weighted SRLG protection with remote SRLG flooding is as follows:

router isis 1

address-family ipv4 unicast

advertise application lfa link-attributes srlg

SRLG-Aware Autotunnel Backup

Autotunnel Backup allows routers to automatically create backup tunnels. This eliminates the need to preconfigure and assign individual backup tunnels to protected interfaces. When a primary tunnel is at risk because its associated interface shares an SRLG with a failing link, the router instantly computes and activates a backup tunnel that bypasses the vulnerable segment. Only automatically created backup tunnels can bypass SRLGs or the protected interfaces that they serve.

To globally activate the autotunnel backup feature, enter the mpls traffic-eng auto-tunnel backup command.

Implementing SRLG

![](<../.gitbook/assets/Unknown image (1317)>)

| srlg interface GigabitEthernet0/0/0/1 name SRLG\_1 ! interface GigabitEthernet0/0/0/4 name SRLG\_1 ! name SRLG\_1 value 77                                                                                         |                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| segment-routing traffic-eng policy POL-200 color 200 end-point ipv4 10.0.0.3 autoroute include ipv4 172.16.200.0/24 ! candidate-paths preference 10 dynamic pcep ! ! constraintsdisjoint-path group-id 5 type srlg | The group-id acts as a unique identifier that groups multiple SR-TE policies. When multiple policies share the same group id and are configured with the SRLG disjoint constraint, the PCE computes paths to ensure that none of these LSPs traverse links with the same SRLG values. |

### Color-only automated steering

Color-only steering is a traffic steering mechanism where a policy is created with given color, regardless of the endpoint.

[https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/segment-routing/25xx/configuration/guide/b-segment-routing-cg-cisco8000-25xx/configuring-sr-te-policy.html#:\~:text=initiated%20SR%20policy-,Color%2DOnly%20Automated%20Steering,-Color%2Donly%20steering](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/segment-routing/25xx/configuration/guide/b-segment-routing-cg-cisco8000-25xx/configuring-sr-te-policy.html)

You can create an SR-TE policy for a specific color that uses a NULL end-point (0.0.0.0 for IPv4 NULL, and ::0 for IPv6 NULL end-point). This means that you can have a single policy that can steer traffic that is based on that color and a NULL endpoint for routes with a particular color extended community, but different destinations (next-hop).

Note

Every SR-TE policy with a NULL end-point must have an explicit path-option. The policy cannot have a dynamic path-option (where the path is computed by the head-end or PCE) since there is no destination for the policy.

You can also specify a color-only (CO) flag in the color extended community for overlay routes. The CO flag allows the selection of an SR-policy with a matching color, regardless of endpoint Sub-address Family Identifier (SAFI) (IPv4 or IPv6). See Setting the Color-Only Flag.

Configure Color-Only Steering

Router# configure

Router(config)# segment-routing

Router(config-sr)# traffic-eng

Router(config-sr-te)# policy P1

Router(config-sr-te-policy)# color 1 end-point ipv4 0.0.0.0

Router# configure

Router(config)# segment-routing

Router(config-sr)# traffic-eng

Router(config-sr-te)# policy P2

Router(config-sr-te-policy)# color 2 end-point ipv6 ::0

### PCEP architecture

The PCE-PCC architecture is a client/server architecture, which helps you atomate the creation of multidomain SR-TE policies at scale.

At the heart of multidomain SR-TE is the concept of software defined networking (SDN) and the introduction of a network controller.

Path Computation Element Protocol (PCEP) is a protocol for communications between a Path Computation Client (PCC) and a path computation element (PCE), or between two PCEs. Such interactions include path computation requests and path computation replies as well as notifications of specific states that are related to the use of a PCE in the context of Multiprotocol Label Switching (MPLS) and SR Traffic Engineering. PCEP is designed to be flexible and extensible so as to easily allow for the addition of further messages and objects, should further requirements be expressed in the future. Use of TCP port 4189 as the transport protocol

Traffic engineering database (TED) is a component of the PCE that stores the topology of the network. A special version of SPF called Constrained Shortest Path First (CSPF) to calculate best paths based on the inputs provided by the operator on the controller. CSPF takes into account both the information in the traffic engineering database (TED) and the constraints that you impose on the SR policy or a tunnel. In other words, the path that is computed with CSPF is the shortest path fulfilling a set of constraints. Once the path is computed, TE is responsible for establishing and maintaining a forwarding state along such a path.

SR PCE functionality is available on any physical or virtual IOS XR node, activated with a single configuration command

SR PCE is fundamentally distributed - Not a single all-overseeing entity (“god box”), but distributed across the network; RR-alike deployment

Intermediate router must have MPLS TE enabled, so that TE attributes are sent to PCE - globally enabled and under IGP

Enabling PCE on intermediate router is optional

The following are PCE functions:

Compute Path: Calculates the best path based on network topology, constraints, and segment routing policies.

Optionally Steer Traffic onto Policy: If configured, the PCE can directly influence traffic forwarding by installing paths into routers.

![](<../.gitbook/assets/Unknown image (1318)>)

#### Stateless PCE

The head of the tunnel, as a PCC, can connect to a PCE (or rely on an existing connected PCE) and request that it compute the path of the LSP, based on constraints and metrics provided by the PCC. The PCE replies with the path information, which the PCC then uses to signal the LSP and establish the tunnel.

When the PCE has replied with the path information, it has no further dealings with the tunnel or LSP, which is solely the responsibility of the PCC. The PCC might make further requests of the PCE—for example, to reoptimize the path if the network changes. However, each request is managed by the PCE as a single transaction, unrelated to earlier or later requests for this LSP. For that reason, this model is termed stateless - does not keep the LSP database

#### Stateful PCE

The Label Switched Path (LSP) database is component of a stateful PCE that keeps track of all provisioned policies in the network, whether they were deployed directly on the devices or by using the PCEP controller. A stateful PCE is a PCE that has access not only to the network state, but also to the set of active paths and their reserved resources for its computations. A stateful PCE might also retain information regarding LSPs under construction to reduce churn and resource contention. The additional state allows the PCE to compute constrained paths while considering individual LSPs and their interactions.

Creation (or instantiation) of an LSP is a procedure by which a PCE instructs a PCC to create an LSP respecting certain attributes. For LSPs created in this manner, the PCE is delegated control automatically.

Stateful PCE describes a set of procedures by which a PCC can report and delegate control of headend tunnels that are sourced from the PCC to a PCE peer. The PCE peer can request the PCC to update and modify parameters of LSPs it controls

As with the stateless PCE model, the PCE must learn TE topology information to compute paths. There are several ways to obtain this information, and it may vary by deployment. One approach is for the PCE to learn the topology by using IGP (OSPF or IS-IS). Another approach relies on a recently proposed mechanism that involves learning topology and link-state information through BGP-LS.

State Reporting: refers to the PCC sending information to PCEs about the state of LSPs. When a topology change occurs and the LSP changes state, the PCC reports the state change to the PCE to keep the PCE informed of changes. State reporting is also used as part of state synchronization and delegation.

State Updating: refers to the PCE sending information to a PCC to alter the attributes of an LSP. State updating is allowed only if the PCE has previously been delegated control of the LSP. State updating is also used to return delegated control.

BGP-LS local vs. remote

Local = distribute IGP-LS info into BGP on PCE

Remote = PCE has BGP-LS sessions to key routers in each area/AS

![](<../.gitbook/assets/Unknown image (1319)>)

PCEP devices exchange different control messages in the process of facilitating SR-TE policies. PCEP messages are defined as:

Open and Keepalive: Messages that are used to initiate a PCEP session, and to maintain a PCEP session, respectively.

PCReq: A PCEP message sent by a PCC to a PCE to request a path computation.

PCRep: A PCEP message sent by a PCE to a PCC in reply to a path computation request. A PCRep message can contain either a set of computed paths if the request can be satisfied, or a negative reply if not. The negative reply might indicate the reason why no path could be found.

PCNtf: A PCEP notification message, either sent by a PCC to a PCE or sent by a PCE to a PCC, to notify the recipient of a specific event.

PCErr: A PCEP message sent when a protocol error occurs.

Close message: A message that is used to close a PCEP session.

The PCC sends a PCRequest to the PCE to request calculation of a path.

The PCE calculates the path.

The PCE sends a PCReply to the PCC with the calculated path information (explicit route object \[ERO]).

The PCC sets up the SR-TE policy.

The PCC sends a PCReport to the PCE, informing it of the LSP path and the identity of the LSP.

The PCE updates its LSP database with the LSP path information.

![](<../.gitbook/assets/Unknown image (1320)>)

As in the example on the figure, PCEP sessions are usually configured between all PE (edge) nodes and the PCE server. This is because it is most likely that PE devices will have the requirement to create tunnels or SR-TE policies.

PCE deployment model is similar to BGP route reflectors.

Different PCCs can use different PCEs.

Service headend (PCC) establishes a PCEP session with one or more PCEs.

![](<../.gitbook/assets/Unknown image (1321)>)

#### PCE and BGP-LS

BGP Link State (BGP-LS) is a special BGP address family for exchanging underlying IGP topology between LS speaking BGP neighbors. The topology information is needed by the PCE to compute paths and this way the PCE can be far removed from the IGP, and not participate in the IGP as a speaker. The information can be conveyed by BGP-LS, which can be multiple hops away and uses standard BGP TCP connectivity between the neighbors.

BGP-LS sessions are typically established with two (for redundancy) routers inside an IGP domain (or an area or level). Since all routers within the same IGP area or level will have identical IGP databases. The IGP database will be converted into BGP information and carried inside BGP updates to BGP-LS neighbors. This way a remote router (PCE) can learn the exact and up-to-date IGP topology. If the IGP topology changes, BGP-LS speakers will send BGP updates to the PCE notifying it of failed links.

PCE has a complete view of the entire network.

PCE combines the different domains to compute end-to-end paths.

![](<../.gitbook/assets/Unknown image (1322)>)

Routers that send link-state information to the PCE must have the distribute link-state option enabled under the IGP configuration.

The following is a PCE configuration example:

show segment-routing traffic-eng pcc ipv4 peer

RP/0/0/CPU0:R10# show pce ipv4 peer

On the PCC, the configuration is performed under segment-routing traffic-engineering. The IP address of the PCE peer is configured under the PCC configuration.

Source IP addressing may be specified, if there are multiple paths to one or more PCEs this is considered a best practice. It is no longer required to specify stateful versus stateless connectivity to the PCE.

The PCC can be configured to communicate with multiple PCEs. A precedence value determines which PCE's computation path to prefer. Lower-precedence values are preferred.

| PCE                                                                                                                                                                                                                                                                                                                    | PCC                                                                                                                                                                                              |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| router bgp 65000 address-family link-state link-state ! neighbor 10.0.0.5 remote-as 65000 update-source Loopback0 address-family link-state link-state ! ! neighbor 10.0.0.6 remote-as 65000 update-source Loopback0 address-family link-state link-state ! ! ! pce address ipv4 10.0.0.10 state-sync ipv4 10.0.0.11 ! | router isis LAB distribute link-state ! segment-routing traffic-eng pcc source-address ipv4 10.0.0.1 pce address ipv4 10.0.0.10 precedence 10 ! pce address ipv4 10.0.0.11 precedence 20 ! ! ! ! |

#### Redundant Path Computation Elements

For redundancy it may be required to deploy redundant PCE servers. A PCC uses precedence to select stateful PCEs for delegating LSPs. Precedence can take any value between 0 and 255. The default precedence value is 255. When there are multiple stateful PCEs with active PCEP session, PCC chooses the PCE with the lowest precedence value. In case where primary PCE server session goes down, PCC router re-delegates all tunnels to next available PCE server. You can use the following CLIs in the case of redundant PCEs:

R2(config)#mpls traffic-eng pcc peer 10.77.77.77 source 10.22.22.22 precedence 255

R2(config)#mpls traffic-eng pcc peer 10.88.88.88 source 10.22.22.22 precedence 100

If the precedence for two PCEs is same, PCE with lower IP address has a higher precedence.

You can accelerate the re-evaluation of LSP delegation from a PCC to PCE servers after you have changed the precedence of PCE servers or added new PCE servers. To do so, manually trigger TE reoptimization using the following command in privileged EXEC mode: mpls traffic-eng reoptimize

Which two actions describe LSP delegation to PCE servers? (Choose two.)

A. removing TE re-optimization timer timeouts

B. changing the precedence of any of the PCE servers

C. entering the mpls traffic-eng reoptimize command

D. adding a new PCE server with lower precedence than the primary PCE

E. adding a new PCE server with higher precedence than the primary PCE

![](<../.gitbook/assets/Unknown image (1323)>)

### On-Demand Next Hop (ODN)

a dynamic feature that automates the instantiation of segment routing policies to a BGP next hop on demand. By using BGP color-extended communities and preconfigured templates, ODN simplifies traffic management, improves scalability, and ensures SLA-compliant path selection without extensive pre-configuration

Scalability: ODN reduces the number of policies that are required by creating one policy for each service level (for example, SLA) rather than for each destination PE, significantly lowering configuration overhead.

Automation: It automatically triggers policy instantiation through BGP events and Path Computation Element (PCE) calculations, ensuring dynamic and efficient path computation

Efficiency: ODN delivers improved performance and resource utilization by computing optimal paths based on traffic engineering metrics (such as TE metric or latency).

Integration: ODN uses existing BGP and Extended Community mechanisms to seamlessly steer traffic based on route coloring and policy matching

segment-routing traffic-eng

on-demand color 100

candidate-paths

preference 1

constraints

segments

dataplane mpls

!

association-group

identifier 1

disjointness type node

source 10.0.0.0

!

!

dynamic

pcep

metric

type delay

### Egress Peer Engineering (EPE)

BGP Prefix SID Used between BGP domains to establish LSPs. The BGP prefix segment is a global segment advertised by BGP for a prefix that is used for routing traffic along the shortest path through the network. BGP prefix SID attribute—a new BGP attribute

Used within a BGP domain to control outbound traffic from the local AS. It uses the BGP peer segment to steer traffic onto a BGP peer or over specific links to that peer. The BGP peer segment is a local segment signaled by BGP link state (topology information) to an SDN controller.

PeerNode SID: Pop and forward on any interface to the peer.

PeerAdj SID: Pop and forward on the related interface.

PeerSet SID: Pop and forward on any interface to the set of peers.

The following figure demonstrates several examples of PeerNode and PeerAdj SIDs.

Routers in AS1, which is also an SR domain will label the destination packets for networks in AS4 and AS5 or AS6 with different labels depending which peer or link they want to send the traffic to. The MPLS labels are popped before the traffic is sent to the peers, that is, the traffic is unlabeled when it leaves your BGP AS (AS1).

Examples for BGP peering SIDs:

PeerNode SIDs:

30024: Pop and forward to peer 4

30025: Pop and forward to peer 5, on either of the two links (load balancing)

30035: Pop and forward to peer 5

PeerAdj SIDs:

30125: Pop and forward to peer 5 on top link

30225: Pop and forward to peer 5 on bottom link

![](<../.gitbook/assets/Unknown image (1324)>)

### BGP SR-TE

BGP signals an explicit SR-TE policy to a remote peer, which triggers the setup of a TE tunnel with specific characteristics and explicit paths. On the receiver side, a TE tunnel that corresponds to the explicit path is setup by BGP. The packets for the destination mentioned in the BGPupdate follow the explicit path described by the policy. Each policy can include multiple explicit paths, and TE will create a tunnel for each path.

The BGP Extended Community is used to carry the opaque color attribute throughout the network. This opaque attribute is a non-significant numeric identifier that informs routers of specific traffic flows and the corresponding routing decisions.

NLRI Length: The address family determines the overall length; it sets it to 104 bits for IPv4 endpoints and 232 bits for IPv6 endpoints.

Distinguisher: A 32-bit field that uniquely distinguishes the policy, allowing multiple policies with similar attributes to coexist.

Policy Color: A 24-bit identifier that can be used to mark the policy for administrative grouping or prioritization.

Endpoint: This field contains the destination address, 32 bits for an IPv4 address, and 128 bits for an IPv6 address.

The assignment and use of these colors differ between the egress provider edge (PE) and ingress PE routers:

Egress PE Color Assignment through BGP Extended Communities: The egress PE assigns a color to a route by appending a BGP color-extended community to its advertisement. This color represents the intended service level or path preference for the route. For example, a route might be assigned a color indicating a low latency requirement. Route policies typically manage this association by specifying the appropriate color-extended community for each route.

Ingress PE Color Assignment within SR-TE Policies: Upon receiving a route advertisement with an associated color, the ingress PE uses an ODN policy to interpret the color. The ODN policy is preconfigured to map specific colors to corresponding SR-TE policies. When a route with a particular color is received, the ingress PE automatically instantiates the SR-TE policy that is associated with that color.

On egress PE, colors are assigned using BGP extended communities (for example, through an extcommunity value), while on ingress PE, colors are configured within the SR-TE policy.

![](<../.gitbook/assets/Unknown image (1325)>)

These ext-community values are advertised R3’s neighbors through BGP based on the route policy that is assigned to those neighbors.

extcommunity-set opaque COLOR-100

100

end-set

!

extcommunity-set opaque COLOR-200

200

end-set

!

route-policy SET-COLOR

if destination in (172.16.100.0/24) then

set extcommunity color COLOR-100

elseif destination in (172.16.200.0/24) then

set extcommunity color COLOR-200

else

pass

endif

end-policy

!

router bgp 65000

bgp router-id 10.0.0.3

neighbor 10.0.0.1

remote-as 65000

update-source Loopback0

address-family ipv4 unicast

route-policy SET-COLOR out

The ingress PE router R1 is configured with two ODN policies, one with color 100 and the other with color 200. One policy uses the TE metric, the other uses hop count, and both rely on a PCE for path calculation.

segment-routing

traffic-eng

on-demand color 100

dynamic

pcep

!

metric

type hopcount

!

!

!

on-demand color 200

dynamic

pcep

!

metric

type te

show cef

show bgp

show segment-routing traffic-eng policy

The following configuration shows an SR-TE headend with a BGPv4 session toward a BGP SR-TE controller. This BGP session is used to signal both IPv4 and IPv6 segment routing policies.

router bgp 65000

bgp router-id 10.1.1.1

!

address-family ipv4 sr-policy

!

address-family ipv6 sr-policy

!

neighbor 10.1.3.1

remote-as 10

address-family ipv4 sr-policy

route-policy PASS in

route-policy PASS out

!

address-family ipv6 sr-policy

route-policy PASS in

route-policy PASS out

<\<SRTE\_TOI\_dev\_v20b.pdf>>

### Segment Routing Flexible Algorithms (Flex-Algo)

Flex-Algo assigns a specific set of "algorithms" to a Segment. The algorithm identifies a specific computation constraint the segment supports. There are standards based algorithm definitions such as least cost IGP path and latency, or providers can define their own algorithms to satisfy their business needs. Agile Services Networking supports computation of Flex-Algo paths in intra-domain and inter-domain deployments. Flex-Algo limits the computation of a path to only those nodes participating in that algorithm. This gives a powerful way to create multiple network domains within a single larger network, constraining an SR path computation to segments satisfying the metrics defined by the algorithm.

Operators can solve most traffic engineering use cases with Flex-Algo. Capabilities of Flex-Algo are continually being enhanced, supporting additional metrics and constraints for path computation

To guarantee loop-free forwarding for paths that are computed using a particular Flexible Algorithm, all routers in the network must share the same definition of the Flexible Algorithm. A consistent definition is achieved when dedicated routers advertise the definition for each Flexible Algorithm. Such advertisements carry a priority that ensures all routers agree on a single, consistent definition for each Flexible Algorithm.

The definition of a Flexible Algorithm includes the following:

Metric type

Affinity constraints

Each node must advertise all Flexible Algorithms in which it is participating:

Nodes 0 and 9 participate in Algorithm 0 and 128 and 129.

Nodes 1, 2, 3, and 4 participate in Algorithm 0 and 128.

Nodes 5, 6, 7, and 8 participate in Algorithm 0 and 129.

![](<../.gitbook/assets/Unknown image (1326)>)

#### Flex-Algo metrics

Delay

Delay utilizes the measured or statically configured delay of each link to compute an end to end lowest latency path. Delay values are computed or defined using SR Performance Measurement.

Generic

The generic metric type is used by operators to build a custom topology based on their own metrics. The generic metric for each link is defined in the IS-IS configuration for each link. The Flex-Algo topology will only include nodes/links with the generic metric defined, and will follow the lowest cost path using those user-defined metrics.

TE

The TE metric is an additional metric which can be assigned to each link. The TE metric is advertised in the standard IS-IS traffic engineering TLVs.

#### Flex-Algo constraints

Affinity:

Affinity uses standard unidirectional traffic engineering affinities (groups) to include or exclude links from the topology. IOS-XR also supports reverse affinities, meaning if the affinity is being sent from node B to A, A will take that link into account when computing the Flex-Algo. One use case is if an affinity is being applied by the remote node on the link due to packet errors.

Minimum Bandwidth:

The minimum bandwidth metric is uses to prune links below a certain bandwidth value. This is useful for networks with a mix of high and low speed links to ensure traffic does not take a low speed path. An example is a network with 10G access rings connected to a higher speed aggregation network. High speed traffic between aggregation locations should never traverse the access rings, and this is easily achievable using the minimum bandwidth constraint.

Maximum Delay:

Maximum delay uses the measured or statically set SR-PM delay values to prune high delay links from the network. While the delay metric will calculate the lowest delay path, this will ensure that path never takes high delay links.

Each router supporting a Flexible Algorithm creates a special MPLS label (a prefix SID) that identifies a forwarding path. Only traffic that is destined for network addresses that are marked with this special label is sent down that path.

When a router supports a Flexible Algorithm, it usually announces a corresponding prefix SID. As shown in the following example:

Node 9 might announce the following:

Prefix SID 16009 for algorithm 0

Prefix SID 16809 for algorithm 128

Prefix SID 16909 for algorithm 129

When calculating the best (shortest) path for a given algorithm, the router follows these rules:

Exclude unsupported nodes: Any node not advertising support for the flexible algorithm will be ignored.

Exclude Certain Links: If the algorithm specifies that certain links (using attributes called affinities) should not be used, then any link with those attributes will also be ignored.

Use specific metrics: The router will only consider links that provide the required metric value that is defined by the algorithm. Links without this metric are ignored.

The router also calculates backup paths, called Loop-Free Alternate (LFA) paths, using the same rules. These alternate paths also rely on the advertised prefix SIDs to ensure that traffic can still be safely routed if the primary path fails.

#### Flexible Algorithm with exclude SRLG constraint

This feature allows the Flexible Algorithm definition to specify SRLGs that the operator wants to exclude during the Flexible Algorithm path computation. A set of links that share a resource whose failure can affect all links in the set constitute an SRLG. An SRLG provides an indication of which links in the network might be at risk from the same failure.

This allows the setup of disjoint paths between two or more Flexible Algorithms by using deployed SRLG configurations. For example, multiple Flexible Algorithms could be defined by excluding all SRLGs except one. Each Flexible Algorithms will prune the links belonging to the excluded SRLGs from its topology on which it computes its paths.

This provides a new alternative to creating disjoint paths with Flexible Algorithms, in addition to using Flexible Algorithms with link admin group (affinity) constraints.

The following example shows how to enable the Flexible Algorithm ASLA-specific advertisement of SRLGs and to exclude SRLG groups from Flexible Algorithm path computation:

srlg

interface Gi0/0/0/0

name groupX

!

interface TenGigE0/0/0/1

name groupX

!

name groupX value 100

Example Flex-algo config

![](<../.gitbook/assets/Unknown image (1327)>)

R1 config

router isis 1

address-family ipv4 unicast

advertise application flex-algo link-attributes srlg

flex-algo 128

advertise-definition

srlg exclude-any groupX

!

interface Loopback0

address-family ipv4 unicast

prefix-sid index 1

prefix-sid algorithm 128 absolute 16101

segment-routing

traffic-eng

on-demand color 200

dynamic

pcep

!

!

constraints

segments

sid-algorithm 128

Configure and Verify ODN and Flexible Algorithm

The purpose of this task is to configure ODN policies on PE1 for VRF VOICE\_DATA. The policies will steer the prefixes of a single VRF through a path based on the IGP metric or latency. Prefix 172.16.80.0/24 that is consuming high data will be steered through a policy by using the IGP metric, while the prefix 172.16.90.0/24 that requires low latency or high SLA will be steered through a low-latency path.

Following is the existing VRF VOICE\_DATA configuration on the PE2 router. Two loopback interfaces (480 and 490) are exported into VRF with different colors (480 and 490).

vrf VOICE\_DATA

address-family ipv4 unicast

import route-target

1:244

!

export route-policy SET\_COLOR\_VOICE\_DATA

export route-target

1:244

!

interface Loopback480

description Voice Data HighBandwidth

vrf VOICE\_DATA

ipv4 address 172.16.80.80 255.255.255.0

!

interface Loopback490

description Voice Data LowLatency

vrf VOICE\_DATA

ipv4 address 172.16.90.90 255.255.255.0

!

extcommunity-set opaque color480-igp

480

end-set

!

extcommunity-set opaque color490-latency

490

end-set

!

prefix-set LowLatency

172.16.90.0/24 eq 24

end-set

!

prefix-set HighBandwidth

172.16.80.0/24 eq 24

end-set

!

route-policy SET\_COLOR\_VOICE\_DATA

if destination in HighBandwidth then

set extcommunity color color480-igp

else

if destination in LowLatency then

set extcommunity color color490-latency

endif

endif

end-policy

On the PE1 router, configure an ODN policy for the color 480 with a metric type of IGP.

Answer

On the PE1 router, use the following commands:

RP/0/RP0/CPU0:PE1# configure

RP/0/RP0/CPU0:PE1(config)# segment-routing traffic-eng

RP/0/RP0/CPU0:PE1(config-sr-te)# on-demand color 480

RP/0/RP0/CPU0:PE1(config-sr-te-color)# dynamic metric type igp

RP/0/RP0/CPU0:PE1(config-sr-te-color)# commit

RP/0/RP0/CPU0:PE1(config-sr-te-color)# end

RP/0/RP0/CPU0:PE1#

On the PE1 router, configure an ODN policy for color 490 with a metric type of latency.

Answer

On the PE1 router, use the following commands:

RP/0/RP0/CPU0:PE1# configure

RP/0/RP0/CPU0:PE1(config)# segment-routing traffic-eng

RP/0/RP0/CPU0:PE1(config-sr-te)# on-demand color 490

RP/0/RP0/CPU0:PE1(config-sr-te-color)# dynamic metric type latency

RP/0/RP0/CPU0:PE1(config-sr-te-color)# commit

RP/0/RP0/CPU0:PE1(config-sr-te-color)# end

RP/0/RP0/CPU0:PE1#

On the PE1 router, verify the color of individual prefixes in VRF VOICE\_DATA. Prefixes received from the PE2 router are 172.16.80.0/24 and 172.16.90.0/24.

Answer

On the PE1 router, use the show bgp vpnv4 unicast vrf VOICE\_DATA 172.16.80.0/24 command.

RP/0/RP0/CPU0:PE1# show bgp vpnv4 unicast vrf VOICE\_DATA 172.16.80.0/24

BGP routing table entry for 172.16.80.0/24, Route Distinguisher: 1:244

Versions:

Process bRIB/RIB SendTblVer

Speaker 14 14

Last Modified: Feb 20 09:09:56.239 for 01:42:12

Paths: (1 available, best #1)

Not advertised to any peer

Path #1: Received by speaker 0

Not advertised to any peer

Local

10.2.2.2 (metric 30) from 10.2.2.2 (10.2.2.2)

Received Label 24008

Origin incomplete, metric 0, localpref 100, valid, internal, best, group-best, import-candidate, imported

Received Path ID 0, Local Path ID 1, version 14

Extended community: Color:480 RT:1:244

Source AFI: VPNv4 Unicast, Source VRF: VOICE\_DATA, Source Route Distinguisher: 1:244

The VRF VOICE\_DATA prefix 172.16.80.0/24 that is received from the PE2 router has the extended community color 480.

On the PE1 router, use the show bgp vpnv4 unicast vrf VOICE\_DATA 172.16.90.0/24 command.

RP/0/RP0/CPU0:PE1# show bgp vpnv4 unicast vrf VOICE\_DATA 172.16.90.0/24

BGP routing table entry for 172.16.90.0/24, Route Distinguisher: 1:244

Versions:

Process bRIB/RIB SendTblVer

Speaker 15 15

Last Modified: Feb 20 09:09:56.239 for 01:43:17

Paths: (1 available, best #1)

Not advertised to any peer

Path #1: Received by speaker 0

Not advertised to any peer

Local

10.2.2.2 (metric 30) from 10.2.2.2 (10.2.2.2)

Received Label 24008

Origin incomplete, metric 0, localpref 100, valid, internal, best, group-best, import-candidate, imported

Received Path ID 0, Local Path ID 1, version 15

Extended community: Color:490 RT:1:244

Source AFI: VPNv4 Unicast, Source VRF: VOICE\_DATA, Source Route Distinguisher: 1:244

The VRF VOICE\_DATA prefix 172.16.90.0/24 received from the PE2 router has the extended community color 490.

### L3 multicast using SR-MPLS Tree-SID

Tree-SID Overview

Tree-SID utilizes the programmability of SR-PCE to create and maintain an optimized multicast tree from source to receiver across an SR-only IPv4 network. Each node in the network maintains a session to the same set of SR-PCE controllers. The SR-PCE creates the tree using PCE-initiated segments. TreeSID supports advanced functionality such as TI-LFA for fast protection and disjoint trees.

Static Tree-SID

Multicast traffic is forwarded across the tree using static S,G mappings at the head-end source nodes and tail-end receiver nodes. Providers needing a solution where dynamic joins and leaves are not common, such as broadcast video deployments, can be benefit from the simplicity static Tree-SID brings, eliminating the need for distributed BGP mVPN signaling. Static Tree-SID is supported for both default VRF (Global Routing Table) and mVPN.

Dynamic Tree-SID using BGP mVPN Control-Plane

IOS-XR supports using fully dynamic signaling to create multicast distribution trees using Tree-SID. Sources and receivers are discovered using BGP auto-discovery (BGP-AD) and advertised throughout the mVPN using the IPv4 or IPv6 mVPN AFI/SAFI. Once the source head-end node learns of receivers, the head-end will create a PCEP request to the configured primary PCE. The PCE then computes the optimal multicast distribution tree based on the metric-type and constraints specified in the request. Once the Tree-SID policy is up, multicast traffic will be forwarded using the tree by the head-end node. Tree-SID optionally supports TI-LFA for all segments, and the ability to create disjoint trees for high available applications.

All routers across the network needing to participate in the tree, including core nodes, must be configured as a PCC to the primary PCE being used by the head-end node.

L3 IP Multicast and mVPN using mLDP

The Agile Metro design supports using mLDP for multicast distribution when using SR-MPLS or SRv6 for unicast transport. Using BGP signaling adds additional scale to the network over in-band mLDP signaling and fits with the overall design goals of CST. More information about deployment of profile 14 can be found in the Agile Metro implementation guide. The Agile Services Networking design supports mLDP-based label switched multicast within a single doman and across IGP domain boundaries. In the case of the Agile Metro design multicast has been tested with the source and receivers on both access and ABR PE devices.

Supported Multicast Profiles Description

Profile 6 mLDP VRF using in-band signaling

Profile 7 mLDP global routing table using in-band signaling

Profile 14 Partitioned MDT using BGP-AD and BGP c-multicast signaling

Profile 14 is recommended for all service use cases and supports both intra-domain and inter-domain transport use cases.

### SR Performance Measurement (SR-PM)

Segment Routing Performance Measurement (SR-PM) is a feature of modern networks that enables service providers to monitor, analyze, and optimize network performance. Operators can ensure compliance with service level agreements by integrating performance measurement into segment routing, quickly diagnosing problems, and maintaining high-quality, reliable network services in dynamic environments.

The router uses the extended traffic engineering link-delay metric (minimum delay value) to compute paths for segment routing policies, either as an optimization metric or an accumulated delay node.

You can verify the end-to-end delay values before activating the candidate path or the segment routing policy segment lists in the forwarding table

The network itself can perform dynamic performance measurement between any two endpoints in the network. Performance measurement includes delay and loss calculations. It is recommended SR-PM be enabled on all logical links across the network.

SR-PM provides various profiles that facilitate different types of delay measurements:

Use the interfaces delay profile type for link-delay measurement.

Use the sr-policy delay profile type for segment routing policy-delay measurements.

In addition to accurate delay measurements, the SR-PM solution integrates robust liveness monitoring to continuously verify the operational status of network nodes and paths. By periodically sending probe packets in loopback mode along defined segments, the system quickly identifies nonresponsive devices or links, enabling rapid troubleshooting and proactive maintenance. Liveness checks are essential for maintaining network availability and reliability.

#### One-way and two-way mode

There are two SR-PM modes, one-way and two-way measurement. Both modes require sender and reflector Precision Time Protocol (PTP) capable hardware and hardware time stamping. The PTP clock synchronization between the sender and reflector is only required in the one-way SR-PM mode.

The one-way measurement mode provides the most precise form of one-way delay measurement. PTP-capable hardware and hardware time stamping are required on both the sender and the reflector, with PTP clock synchronization between the sender and the reflector. Delay measurement in one-way mode is calculated as (T2–T1).

The delay measurement feature utilizes TWAMP-Lite as the transport mechanism for probes and responses. PTP is a requirement for accurate measurement of one-way latency across links and is recommended for all nodes. In the absence of PTP a "two-way" delay mode is supported to calculate the one-way link delay.

Legacy hardware also now supports a software CPU based timestamp method to enable SR-PM delay measurements for hardware without PTP timing hardware.

To measure one-way delay, performance monitoring (PM) queries and responses are as follows:

The local-end router sends performance monitoring query packets periodically to the remote side after the egress line card on the router applies timestamps on packets.

The ingress line card on the remote-end router applies timestamps on packets when they are received.

The remote-end router sends the performance monitoring packets containing timestamps back to the local-end router.

To measure one-way delay, use the timestamp values in the performance monitoring packet.

A router calculates delay measurements in two-way mode as follows:

Two-way delay = (T4–T1)–(T3–T2)

One-way delay = two-way delay/2

To measure two-way delay, the performance monitoring query and response are as follows:

The local-end router sends performance monitoring query packets periodically to the remote side after the egress line card on the router applies timestamps on packets.

The ingress line card on the remote-end router applies timestamps on packets when they are received.

The remote-end router sends the performance monitoring packets containing timestamps back to the local-end router. The remote-end router timestamps the packet just before sending it for two-way measurement.

The local-end router timestamps the packet when the packet is received for two-way measurement.

The router measures one-way delay and, optionally, two-way delay using the timestamp values in the performance monitoring packet.

Delay and loss measurement is also available for SR-TE Policy paths to give the provider an accurate latency and loss measurement for all services utilizing the SR-TE Policy. This information is available through SR Policy statistics using the CLI or model-driven telemetry. The latency measurement is done for all active candidate paths.

#### Static delay

Real-time delay measured by TWAMP fluctuates constantly due to queueing, load variation, and hardware behavior. If those fluctuations were advertised, IGP and SR policies would continuously react, causing recomputation, path changes, and instability. Static delay prevents that by forcing the router to advertise a fixed, operator-defined delay value regardless of measured variation.

When you configure advertise-delay 2000, the router immediately advertises a delay of 2000 microseconds for that interface. This value becomes authoritative for SR and IGP calculations. The advertised minimum, average, and maximum delay are all set to the same static value, with zero variance. Threshold checks are disabled, so measured delay values never trigger new advertisements.

The reason this mechanism exists is operational predictability. In engineered networks, operators often know the physical or expected latency of a link and want routing decisions to reflect that stable value rather than transient congestion. Static delay ensures deterministic SR policy behavior, prevents control-plane churn, and avoids frequent path re-optimization caused by minor delay variations.

If the static delay configuration is removed, the router does not immediately flood new values. It resumes normal threshold-based advertisement, and at the next scheduled check it advertises the real measured delay statistics. This controlled transition avoids instability.

performance-measurement

interface GigabitEthernet0/2/3

delay-measurement

advertise-delay 2000

next-hop ipv4 172.16.62.1

!

!

protocol twamp-light

measurement delay

unauthenticated

querier-dst-port 11222 querier-src-port 11333

#### Accelerated and periodic advertisement parameters

The accelerated command accelerates advertisement parameters.

The periodic command sets periodic advertisement parameters.

The twamp-light command enables the TWAMP-light protocol.

Accelerated advertisement is disabled by default. The threshold checks the minimum-delay metric change for threshold crossing for accelerated advertisement. The default value is 20 percent, ranging from 0 to 100 percent. The minimum default value is 500 microseconds, ranging from 1 to 100000 microseconds.

Periodic advertisement is enabled by default. The default interval value is 120 seconds, and the interval range is from 30 to 3600 seconds. The threshold checks the minimum-delay metric change for threshold crossing for periodic advertisement. The default value is 10 percent, ranging from 0 to 100 percent. The default minimum-change value is 500 microseconds (µsec), ranging from 0 to 100000 microseconds.

#### Delay and loss anomaly detection

It is possible to set thresholds for both delay and loss and trigger behaviors based on the type of resource being monitored using SR-PM. In the case of a SR-Policy, a candidate path can be deactivated if the delay or loss exceeds the configured thresholds. In the case of a logical link, the IGP advertising the link will set the "A" or anomaly bit for the link. When a receiving node receives the IGP link state data with the A bit set, it can raise the IGP cost on the link.

The "anomaly-check" command is used for delay thresholds, and the "anomaly-loss" command is used for loss thresholds.

performance-measurement

interface TenGigE0/0/0/5

delay-measurement

!

!

interface TenGigE0/0/0/23

delay-measurement

!

!

interface HundredGigE0/0/1/0

delay-measurement

!

!

interface HundredGigE0/0/1/1

delay-measurement

!

!

delay-profile interfaces default

advertisement

accelerated

threshold 25

!

anomaly-check upper-bound 1000 lower-bound 100

periodic

interval 120

threshold 10

!

anomaly-loss upper-bound 20 lower-bound 10

!

probe

measurement-mode two-way

protocol twamp-light

!

!

protocol twamp-light

measurement delay

unauthenticated

querier-dst-port 12345

show performance-measurement

### Circuit Style Segment Routing (SR-MPLS)

Circuit Style Segment Routing (CS-SR) is another Cisco advancement bringing TDM circuit like behavior to SR-TE Policies. These policies use deterministic hop by hop routing, co-routed bi-directional paths, hot standby protect paths with end to end liveness detection, and bandwidth guaranteed services. Standard Ethernet services not requiring bit transparency can be transported over a Segment Routing network similar to OTN networks without the additional cost, complexity, and inefficiency of an OTN network layer

Circuit-Style SR-TE with Bandwidth Admission Control using CNC Circuit Style Manager

Crosswork Network Controller Circuit Style Manager provides Bandwidth Admission Controller and guaranteed bandwidth paths for Circuit-Style Policies. CNC also supports full provisioning, monitoring, and visualization of Circuit-Style SR-TE Policies.
