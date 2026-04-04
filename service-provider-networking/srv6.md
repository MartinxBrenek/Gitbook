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

# SRv6

### Overview

**Segment Routing for IPv6 (SRv6)** is the implementation of segment routing over the IPv6 data plane. SRv6 uses an extension header called a **Segment Routing Header (SRH)**. Segments in an SRH are encoded in a list of IPv6 addresses.

In SRv6, the routing path is encoded directly into the IPv6 packet header using a sequence of **Segment Identifiers (SIDs)**. These SIDs represent specific network functions or instructions, such as forwarding packets to a particular node, applying services, or steering traffic along a defined path. Each SID is encoded as a 128-bit IPv6 address, ensuring compatibility with IPv6 infrastructures.

The following are the IPv6 as segment routing data plane characteristics:

Use RFC 8200 provision for source routing extension header.

One segment equals one IPv6 address.

A segment list equals an address list in the SRH.

The active segment is indicated by the destination address of the packet, and the next segment is indicated by a pointer in the SRH

Segment Left - pointer to active Segment from Segment List\
Only value of Pointer is changed - Segments List \[xxx] is not removed like MPLS label

SRv6 introduces the network programming framework that enables a network operator or an application to specify a packet processing program by encoding a sequence of instructions in the IPv6 packet header. Each instruction is implemented on one or several nodes in the network and identified by an SRv6 SID in the packet.

![](<../.gitbook/assets/Unknown image (1374)>)

### IPv6 Segment Routing Header (SRH) format

The SRv6 uses an IPv6 header with the Next Header field equal to 43

The IPv6 routing extension header uses a generic header format that is defined in the RFC 2460. The Next Header field can be IPv4, TCP, or UDP. Any IPv6 device can skip the Hdr Ext Len header field. If Segments Left is zero, the node must ignore the Routing header and proceed to process the next header in the packet, whose type is identified by the Next Header field in the Routing header.

![](<../.gitbook/assets/Unknown image (1375)>)

#### Routing header types

0 is the Source Route header and is deprecated since 2007.

1 is the Nimrod header and is deprecated since 2009.

2 is the Mobility header that is defined in the RFC 6275.

3 is the RPL Source Route header that is defined in the RFC 6554.

4 is the Segment Routing header that is defined in the RFC 8754.

The SRv6 SRH is documented in the RFC 8754 named IPv6 Segment Routing Header (SRH).

**Next Header:** The field identifies the type of header immediately following the SRH.

**Hdr Ext Len (Header Extension Length):** Indicates the length of the SRH in 8-octet units, excluding the first eight octets.

**Segments Left:** Specifies the number of remaining route segments. That means the number of explicitly listed intermediate nodes must be visited before reaching the final destination.

**Last Entry:** Contains the zero-based index of the last element of the segment list.

**Flags:** Contains 8 bits of flags.

**Tag:** Tags a packet as part of a class or group of packets like packets sharing the same set of properties.

**Segment List:** A list of 128-bit IPv6 addresses representing the nth segment in the segment list. The segment list encoding starts from the last segment of the segment routing policy path. That means the first element of the segment list (segment list \[0]) contains the last segment of the segment routing policy, the second element contains the penultimate segment of the segment routing policy, and so on.

Why one diagram says “First Segment”

“First Segment” is an informal label; “Last Entry” is the correct RFC name — both point to the same field that marks the start of SRv6 processing.

Some older or simplified diagrams (and many presentations) label this field as “First Segment”, meaning:

The starting index of the segment list

Historically derived from older IPv6 routing header terminology

But:

In SRv6, the segment list is processed from last to first

So the “first segment to visit” is actually Segment List\[Last Entry]

That’s why RFC 8754 standardized the name to Last Entry — it removes ambiguity.

![](<../.gitbook/assets/Unknown image (1376)>)

Segments are encoded in reverse order:

The Last Segment index is marked as 0.

The First Segment index is marked as First Segment.

The Active Segment index is marked as Segments Left.

The Active Segment is copied in the Destination Address field of the IPv6 header.

Additional data can be stored in the Optional field of the SRH.

![](<../.gitbook/assets/Unknown image (1377)>)

### Node roles

In an SRv6 network, nodes are assigned specific roles that determine how they handle and forward packets. These roles are fundamental to the operation of SRv6, as they dictate the behavior of routers in processing segment routing instructions, enabling efficient traffic engineering and policy-driven routing.

These roles include:

**Source:** The originating node that creates an SRv6 packet, encoding a segment list in the IPv6 header.

**Transit:** An intermediate node that forwards packets based on the standard IPv6 routing table without processing SRv6 instructions. It can be non-SRv6 capable, sufficiently performing pure IPv6 forwarding

**Endpoint:** A node that processes SRv6 segments, performing operations such as decapsulation, segment swapping, or steering traffic based on segment routing policies.

#### Source node processing

SRH is created with:

Segment list in reversed order of the path.

Segment List | 0] is the LAST segment.

Segment List \[n - 1] is the FIRST segment.

Segments Left is set to n - 1.

First Segment is set to n - 1.

IPv6 destination address (DA) is set to the first segment.

The following figure depicts the initial encapsulation of an SRv6 packet at the source node A. It shows the IPv6 header with the destination address set to the first segment (B::) and the SR header containing the complete segment list (D::, C::, B::) with a Segment Left (SL) value of 2.

![](<../.gitbook/assets/Unknown image (1378)>)

#### Endpoint processing (decap)

Process the payload as follows:

For inner IPv4 or IPv6, lookup DA and forward.

For TCP or UDP, send to a socket

After IPv6 and SRH headers are removed, a packet will follow standard IPv4 or IPv6 processing. The final destination does not have to be segment routing-capable.

![](<../.gitbook/assets/Unknown image (1379)>)

### Segment formats (SID structure)

In SRv6, a SID represents a 128-bit value, consisting of the following three parts:

The **Locator** field is the first part of the SID with the most significant bits and represents an address of a specific SRv6 node.

The **Function** field is the portion of the SID that is local to the locator or owner node and designates a specific SRv6 function that is executed locally on a particular node.

The **Argument** field is optional and represents optional arguments to the function.

The locator part can be further divided into two parts:

**SID Block** field is the SRv6 network designator and is a fixed or known address space for an SRv6 domain. This is the most significant bit portion of a locator subnet.

**Node ID** field is the node designator in an SRv6 network and is the least significant bit portion of a locator subnet.

![](<../.gitbook/assets/Unknown image (1380)>)

SIDs can be globally or locally significant. Globally significant is unique across the SRv6 domain. Locally significant is valid only within the local context of a node or specific segment. A local IPv6 address is not a local SID by default. SIDs must be explicitly enabled as such on their parent node to perform their intended functions.

A local SID is specific to the parent node and might not be associated with any interface. This decouples SIDs from physical or logical interfaces, allowing flexible network design and functionality.

### Endpoint behaviors

In segment routing over IPv6 (SRv6), endpoint behaviors define the specific actions or functions a node performs when it processes a SID. These behaviors allow SRv6 to implement advanced routing, traffic engineering, and network programmability. Each behavior is executed locally on the node identified by the Locator part of the SID.

**Default endpoint behavior (node segment)**

Decrement Segments Left, update DA

Forward according to new DA

The following is a subset of defined SRv6 endpoint behaviors that can be associated with a SID:

**End** is the endpoint function. The SRv6 instantiation of a Prefix SID.

**End.X** is an endpoint with Layer-3 cross-connect. The SRv6 instantiation of an Adj-SID.

**End.DXn** allows a router to decapsulate an SRv6-encapsulated packet and cross-connect to an interface.

**End.DX6** is an endpoint with decapsulation and IPv6 cross-connect. The IPv6 L3VPN is equivalent to the per-CE VPN label.

**End.DX4** is an endpoint with decapsulation and IPv4 cross-connect. The IPv4 L3VPN is equivalent to the per-CE VPN label.

**End.DX2** is an endpoint with decapsulation and Layer 2 cross-connect used in the L2VPN use case.

**End.DTn** allows a router to decapsulate an SRv6-encapsulated packet and forward it based on a routing table

**End.DT6** is an endpoint with decapsulation and IPv6 table lookup. The IPv6 L3VPN is equivalent to the per-VRF VPN label.

**End.DT4** is an endpoint with decapsulation and IPv4 table lookup. The IPv4 L3VPN is equivalent to the per-VRF VPN label.

**End.B6.Encaps** is endpoint bound to an SRv6 policy with encapsulation. SRv6 instantiation of a Binding SID.

**End.B6.Encaps.RED** is a more compact version of End.B6.Encaps in SRv6. Both encapsulate incoming IPv4 or IPv6 packets into a new outer IPv6 header with an SRH, which is used to route traffic through a defined path. The difference is how the SRH is constructed.

With End.B6.Encaps, the first segment appears both in the IPv6 destination address and as the first entry in the SRH segment list. This duplicates the first SID. In End.B6.Encaps.RED, the first SID is used only as the IPv6 destination address and is omitted from the SRH. The segment list starts at the second SID, which is reflected in the Segments Left field. This saves space by reducing the SRH by one SID.

Both behaviors work the same from a routing perspective, but End.B6.Encaps.RED uses a smaller SRH, making it more efficient when the first segment is a directly connected next hop.

![](<../.gitbook/assets/Unknown image (1381)>)

### SRv6 micro-segment identifier (uSID)

**SRv6 micro-segment identifier (uSID)** is an extension of the SRv6 architecture. It uses the SRv6 network programming architecture to encode several SRv6 uSID instructions within a single 128-bit SID address. Such a SID address is called a **uSID Carrier**. Any SID in the destination address or SRH can be an SRv6 uSID carrier.

The uSID massively decreases the overhead by encoding instruction directly in a single IPv6 destination addresses, without any additional segment routing headers, however they can still be appended to be swapped as soon as the current uSID is executed.

12 uSID’s with an outer SRH holding one single additional uSID container - 6 in the DA, 6 in the SRH with 24-bytes of MTU overhead - 50% less overhead

![](<../.gitbook/assets/Unknown image (1382)>)

The following are the examples of scalability:

In the 16-bit uSID ID size, there are 65k uSIDs per domain block.

In the 32-bit uSID ID size, there are 4.3M uSIDs per domain block.

The uSID uses mature hardware capabilities and avoids extra lookup in indexed mapping tables. A microprogram with 6 or less uSIDs requires only legacy IP-in-IP encapsulation behavior.

Summarization of the uSIDs at an area or domain boundary provides a massive scaling advantage, and no routing extension is required; a simple prefix advertisement suffices.

A uSID might be used as a SID, where the carrier holds a single uSID. The inner structure of a segment routing policy can stay opaque to the source. A carrier with uSIDs is seen as a SID by the policy headend.

**Container SID** may contain up to 6 micro-instructions called uSID’s - the IETF term is “NEXT-CSID”

A uSID has an associated behavior, the SRv6 function associated with the given ID, for example, a node SID or Adjacency SID. The node at which an uSID is instantiated is called the Parent node. The uSID Carrier is carried in the packet destination address or the SRH.

**uSID:** An identifier that specifies a micro-segment.

**uSID Carrier:** A 128-bit IPv6 address.

![](<../.gitbook/assets/Unknown image (1383)>)

The **End-of-Carrier ID** is 0000. All empty uSID carrier positions must be filled with the End-of-Carrier ID. Therefore, a uSID carrier can have more than one End-of-Carrier.

#### SRH versus uSID

![](<../.gitbook/assets/Unknown image (1384)>)

In the SRv6 uSID example, when node R1 receives the packet, it performs SRv6 uN behavior. It removes its outer DA (0100) and advances the microprogram to the next microinstruction by doing the following:

It pops its own uSID (0100).

It shifts the remaining DA by 16 bits to the left.

It fills the remaining bits with 0000 (End-of-Carrier).

It performs a lookup for the shortest path to the next DA (2001:db8:0200::/48).

It forwards it by using the new DA 2001:db8:0200:0300:0400:0000:0000:0000.

RP/0/RP0/CPU0:r1# configure

RP/0/RP0/CPU0:r1(config)# segment-routing

RP/0/RP0/CPU0:r1(config-sr)# srv6

RP/0/RP0/CPU0:r1(config-srv6)# locators

RP/0/RP0/CPU0:r1(config-srv6-locators)# locator MAIN

RP/0/RP0/CPU0:r1(config-srv6-locator)# micro-segment behavior unode psp-usd

RP/0/RP0/CPU0:r1(config-srv6-locator)# prefix 2001:db8:1::/48

RP/0/RP0/CPU0:r1(config-srv6-locator)# commit

RP/0/RP0/CPU0:r1(config-srv6-locator)# end

RP/0/RP0/CPU0:r1#

Use the show segment-routing srv6 locator command to verify the allocation of SRv6 local SIDs off the locator.

Use the show segment-routing srv6 sid all command to display SID information across locators.

Use the show cef ipv6 command to verify that the End function is programmed in the Cisco Express Forwarding.

The following example shows how to configure the IGP protocol to ensure it is aware of that locator and that the locator is advertised into IGP.

RP/0/RP0/CPU0:r1(config)# router isis 1

RP/0/RP0/CPU0:r1(config-isis)# address-family ipv6 unicast

RP/0/RP0/CPU0:r1(config-isis-af)# segment-routing srv6

RP/0/RP0/CPU0:r1(config-isis-srv6)# locator MAIN

RP/0/RP0/CPU0:r1(config-isis-srv6-loc)# commit

Use the show segment-routing srv6 sid all command to display SID information across locators.

Use the show isis database command to verify the IS-IS database.

### BGP services for SRv6

In SRv6-based services, the egress PE signals an SRv6 Service SID with the BGP service route. The ingress PE encapsulates the payload in an outer IPv6 header where the destination address is the SRv6 Service SID advertised by the egress PE. BGP messages between PEs carry SRv6 Service SIDs to interconnect PEs and form VPNs.

**SRv6 Service SID** refers to a segment identifier associated with one of the SRv6 service-specific behaviors advertised by the egress PE router, such as:

uDT4 for Endpoint with decapsulation and IPv4 table lookup.

uDT6 for Endpoint with decapsulation and IPv6 table lookup.

uDX4 for Endpoint with decapsulation and IPv4 cross-connect.

uDX6 for Endpoint with decapsulation and IPv6 cross-connect.

#### PE-CE configuration

The following output shows how to configure the VRF and add the interfaces toward the CE routers to the VRF.

RP/0/RP0/CPU0:r1(config)# vrf 1

RP/0/RP0/CPU0:r1(config-vrf)# address-family ipv4 unicast

RP/0/RP0/CPU0:r1(config-vrf-af)# import route-target

RP/0/RP0/CPU0:r1(config-vrf-import-rt)# 1:1

RP/0/RP0/CPU0:r1(config-vrf-import-rt)# export route-target

RP/0/RP0/CPU0:r1(config-vrf-export-rt)# 1:1

RP/0/RP0/CPU0:r1(config-vrf-export-rt)# root

RP/0/RP0/CPU0:r1(config)# interface GigabitEthernet0/0/0/2

RP/0/RP0/CPU0:r1(config-if)# vrf 1

RP/0/RP0/CPU0:r1(config-if)# ipv4 address 10.1.6.1 255.255.255.0

RP/0/RP0/CPU0:r1(config-if)# commit

The following output shows how to configure the RD in the VRF and per-VRF SID allocation.

RP/0/RP0/CPU0:r1(config)# router bgp 1

RP/0/RP0/CPU0:r1(config-bgp)# address-family vpnv4 unicast

RP/0/RP0/CPU0:r1(config-bgp-af)# vrf 1

RP/0/RP0/CPU0:r1(config-bgp-vrf)# rd 1:1

RP/0/RP0/CPU0:r1(config-bgp-vrf)# address-family ipv4 unicast

RP/0/RP0/CPU0:r1(config-bgp-vrf-af)# segment-routing srv6

RP/0/RP0/CPU0:r1(config-bgp-vrf-af-srv6)# locator MAIN

RP/0/RP0/CPU0:r1(config-bgp-vrf-af-srv6)# alloc mode per-vrf

RP/0/RP0/CPU0:r1(config-bgp-vrf-af-srv6)# redistribute connected

RP/0/RP0/CPU0:r1(config-bgp-vrf-af)# commit

Once SRv6 is used for L3VPN, packets are encapsulated with destination functions, but you have to configure the source address. The following output shows you how to configure the IPv6 source address.

RP/0/RP0/CPU0:r1(config)# segment-routing

RP/0/RP0/CPU0:r1(config-sr)# srv6

RP/0/RP0/CPU0:r1(config-srv6)# encapsulation

RP/0/RP0/CPU0:r1(config-srv6-encap)# source-address 2001::1

RP/0/RP0/CPU0:r1(config-srv6-encap)# commit

The preceding example shows that BGP always allocates one uDT function per VRF. This is used for all directly connected prefixes. In principle, it is the equivalent of the aggregate label in MPLS.

#### PE-PE core configuration

RP/0/RP0/CPU0:r1(config)# router bgp 1

RP/0/RP0/CPU0:r1(config-bgp)# bgp router-id 1.1.1.1

RP/0/RP0/CPU0:r1(config-bgp)# address-family vpnv4 unicast

RP/0/RP0/CPU0:r1(config-bgp-af)# neighbor 2001::3

RP/0/RP0/CPU0:r1(config-bgp-nbr)# remote-as 1

RP/0/RP0/CPU0:r1(config-bgp-nbr)# update-source Loopback0

RP/0/RP0/CPU0:r1(config-bgp-nbr)# address-family vpnv4 unicast

RP/0/RP0/CPU0:r1(config-bgp-nbr-af)# commit

On the PE1 router, verify the BGP advertisement for CE2 Loopback0 (10.2.10.1/32) prefix.

Answer

On the PE1 router, use the show bgp vrf 1 10.2.10.1/32 command:

RP/0/RP0/CPU0:PE1# show bgp vrf 1 10.2.10.1/32

BGP routing table entry for 10.2.10.1/32, Route Distinguisher: 1:1

Versions:

Process bRIB/RIB SendTblVer

Speaker 26 26

Last Modified: Feb 6 16:04:51.459 for 00:05:05

Paths: (1 available, best #1)

Advertised to CE peers (in unique update groups):

192.168.101.11

Path #1: Received by speaker 0

Advertised to CE peers (in unique update groups):

192.168.101.11

65008

2001:db8:10:2:2::2 (metric 21) from 2001:db8:10:2:2::2 (10.2.2.2)

Received Label 0xe0020

Origin incomplete, metric 0, localpref 100, valid, internal, best, group-best, import-candidate, imported

Received Path ID 0, Local Path ID 1, version 26

Extended community: RT:1:1

PSID-Type:L3, SubTLV Count:1

SubTLV:

T:1(Sid information), Sid:fcbb:bb00:4::, Behavior:63, SS-TLV Count:1

SubSubTLV:

T:1(Sid structure):

Source AFI: VPNv4 Unicast, Source VRF: 1, Source Route Distinguisher: 1:1

The SID for VRF 1 prefix 10.2.10.1/32 is the PE2 locator (fcbb:bb00:4::) and the received label (0xe0020). The received label may be different in your lab.

Note

The End.DT4 function encodes a uDT SID by combining a fixed IPv6 prefix (the locator) with the received BGP label, allowing identification of the routing context for decapsulated IPv4 packets. The SID fcbb:bb00:4:e002:: is formed using the prefix fcbb:bb00:4::/48 assigned to PE2 and embedding the label 0xe0020 into the SID structure. This SID is programmed on PE2 with an End.DT4 behavior that maps the label 0xe0020 to a specific VRF or routing table, enabling PE2 to decapsulate SRv6 traffic and forward IPv4 packets (for example, to CE2 loopback 10.2.10.1) based on the correct routing context.

On the PE2 router, use the show route ipv6 fcbb:bb00:4:e002:: command. Label (:e002:) in the SRv6 uDT4 address may be different in your lab.

RP/0/RP0/CPU0:PE2# show route ipv6 fcbb:bb00:4:e002::

Routing entry for fcbb:bb00:4:e002::/64

Known via "local-srv6 bgp-65001", distance 0, metric 0, SRv6 Endpoint uDT4, SRv6 Format f3216

Installed Feb 6 16:04:37.985 for 00:09:49

Routing Descriptor Blocks

::ffff:0.0.0.0 directly connected

Nexthop in Vrf: "1", Table: "default", IPv4 Unicast, Table Id: 0xe0000001

Route metric is 0

No advertising protos.

On the PE2 router, use the show cef ipv6 fcbb:bb00:4:e002:: command. Label (:e002:) in the SRv6 uDT4 address may be different in your lab.

RP/0/RP0/CPU0:PE2# show cef ipv6 fcbb:bb00:4:e002::

fcbb:bb00:4:e002::/64, version 97, SRv6 Endpoint uDT4, internal 0x1000001 0x0 (ptr 0x87166788) \[1], 0x400 (0x8838c0f8), 0x0 (0x8984b828)

Updated Feb 6 16:04:37.988

Prefix Len 64, traffic index 0, precedence n/a, priority 0

gateway array (0x881f87a0) reference count 1, flags 0x0, source rib (7), 0 backups

\[2 type 3 flags 0x8401 (0x882a5068) ext 0x0 (0x0)]

LW-LDI\[type=3, refc=1, ptr=0x8838c0f8, sh-ldi=0x882a5068]

gateway array update type-time 1 Feb 6 16:04:37.988

LDI Update time Feb 6 16:04:37.988

LW-LDI-TS Feb 6 16:04:37.988

via ::ffff:0.0.0.0/128, 0 dependencies, weight 0, class 0 \[flags 0x0]

path-idx 0 NHID 0x0 \[0x87802198 0x0]

next hop VRF - '1', table - 0xe0000001

next hop ::ffff:0.0.0.0/128

Load distribution: 0 (refcount 2)

Hash OK Interface Address

0 Y recursive Lookup in table
