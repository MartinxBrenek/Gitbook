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

{% file src="../.gitbook/assets/SP7-SRv6-v2.06.pptx" %}

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

### uSID Blocks

**Global ID block (GIB):** The set of IDs available for globally scoped uSID allocation.\
A globally scoped uSID is the type of uSID that provides reachability to a node. A globally scoped uSID typically identifies a shortest path to a node in the SR domain. An IP route (for example, /48) is advertised by the parent node to each of its globally scoped uSIDs, under the associated uSID block. The parent node executes a variant of the END behavior.\
The “nodal” uSID (uN) is an example of a globally scoped behavior defined in uSID architecture.\
A node can have multiple globally scoped uSIDs under the same uSID blocks (for example, one uSID per IGP flex-algorithm). Multiple nodes may share the same globally scoped uSID (Anycast).

\
**Local ID block (LIB):** The set of IDs available for locally scoped uSID allocation.\
A locally scoped uSID is associated to a local behavior, and therefore must be preceded by a globally scoped uSID of the parent node when relying on routing to forward the packet.\
A locally scoped uSID identifies a local micro-instruction on the parent node; for example, it may identify a cross-connect to a direct neighbor over a specific interface or a VPN context. Locally scoped uSIDs are not routeable.

\
**Wide LIB (W-LIB):** The extended set of IDs available for local uSID allocation.\
The extended set of IDs is useful when a PE with large-scale Pseudowire termination requires more local uSIDs than provided from the LIB.

#### SRH versus uSID

![](<../.gitbook/assets/Unknown image (1384)>)

In the SRv6 uSID example, when node R1 receives the packet, it performs SRv6 uN behavior. It removes its outer DA (0100) and advances the microprogram to the next microinstruction by doing the following:

It pops its own uSID (0100).

It shifts the remaining DA by 16 bits to the left.

It fills the remaining bits with 0000 (End-of-Carrier).

It performs a lookup for the shortest path to the next DA (2001:db8:0200::/48).

It forwards it by using the new DA 2001:db8:0200:0300:0400:0000:0000:0000.

`RP/0/RP0/CPU0:r1# configure`

`RP/0/RP0/CPU0:r1(config)# segment-routing`

`RP/0/RP0/CPU0:r1(config-sr)# srv6`

`RP/0/RP0/CPU0:r1(config-srv6)# locators`

`RP/0/RP0/CPU0:r1(config-srv6-locators)# locator MAIN`

`RP/0/RP0/CPU0:r1(config-srv6-locator)# micro-segment behavior unode psp-usd`

`RP/0/RP0/CPU0:r1(config-srv6-locator)# prefix 2001:db8:1::/48`

`RP/0/RP0/CPU0:r1(config-srv6-locator)# commit`

`RP/0/RP0/CPU0:r1(config-srv6-locator)# end`

`RP/0/RP0/CPU0:r1#`

Use the show segment-routing srv6 locator command to verify the allocation of SRv6 local SIDs off the locator.

Use the show segment-routing srv6 sid all command to display SID information across locators.

Use the show cef ipv6 command to verify that the End function is programmed in the Cisco Express Forwarding.

The following example shows how to configure the IGP protocol to ensure it is aware of that locator and that the locator is advertised into IGP.

`RP/0/RP0/CPU0:r1(config)# router isis 1`

`RP/0/RP0/CPU0:r1(config-isis)# address-family ipv6 unicast`

`RP/0/RP0/CPU0:r1(config-isis-af)# segment-routing srv6`

`RP/0/RP0/CPU0:r1(config-isis-srv6)# locator MAIN`

`RP/0/RP0/CPU0:r1(config-isis-srv6-loc)# commit`

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

```
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
```

Once SRv6 is used for L3VPN, packets are encapsulated with destination functions, but you have to configure the source address. The following output shows you how to configure the IPv6 source address.

```
RP/0/RP0/CPU0:r1(config)# segment-routing
RP/0/RP0/CPU0:r1(config-sr)# srv6
RP/0/RP0/CPU0:r1(config-srv6)# encapsulation
RP/0/RP0/CPU0:r1(config-srv6-encap)# source-address 2001::1
RP/0/RP0/CPU0:r1(config-srv6-encap)# commit
```

The preceding example shows that BGP always allocates one uDT function per VRF. This is used for all directly connected prefixes. In principle, it is the equivalent of the aggregate label in MPLS.

#### PE-PE core configuration

`RP/0/RP0/CPU0:r1(config)# router bgp 1`

`RP/0/RP0/CPU0:r1(config-bgp)# bgp router-id 1.1.1.1`

`RP/0/RP0/CPU0:r1(config-bgp)# address-family vpnv4 unicast`

`RP/0/RP0/CPU0:r1(config-bgp-af)# neighbor 2001::3`

`RP/0/RP0/CPU0:r1(config-bgp-nbr)# remote-as 1`

`RP/0/RP0/CPU0:r1(config-bgp-nbr)# update-source Loopback0`

`RP/0/RP0/CPU0:r1(config-bgp-nbr)# address-family vpnv4 unicast`

`RP/0/RP0/CPU0:r1(config-bgp-nbr-af)# commit`

On the PE1 router, verify the BGP advertisement for CE2 Loopback0 (10.2.10.1/32) prefix.

Answer

On the PE1 router, use the show bgp vrf 1 10.2.10.1/32 command:

`RP/0/RP0/CPU0:PE1# show bgp vrf 1 10.2.10.1/32`

`BGP routing table entry for 10.2.10.1/32, Route Distinguisher: 1:1`

`Versions:`

`Process bRIB/RIB SendTblVer`

`Speaker 26 26`

`Last Modified: Feb 6 16:04:51.459 for 00:05:05`

`Paths: (1 available, best #1)`

`Advertised to CE peers (in unique update groups):`

`192.168.101.11`

`Path #1: Received by speaker 0`

`Advertised to CE peers (in unique update groups):`

`192.168.101.11`

`65008`

`2001:db8:10:2:2::2 (metric 21) from 2001:db8:10:2:2::2 (10.2.2.2)`

`Received Label 0xe0020`

`Origin incomplete, metric 0, localpref 100, valid, internal, best, group-best, import-candidate, imported`

`Received Path ID 0, Local Path ID 1, version 26`

`Extended community: RT:1:1`

`PSID-Type:L3, SubTLV Count:1`

`SubTLV:`

`T:1(Sid information), Sid:fcbb:bb00:4::, Behavior:63, SS-TLV Count:1`

`SubSubTLV:`

`T:1(Sid structure):`

`Source AFI: VPNv4 Unicast, Source VRF: 1, Source Route Distinguisher: 1:1`

`The SID for VRF 1 prefix 10.2.10.1/32 is the PE2 locator (fcbb:bb00:4::) and the received label (0xe0020). The received label may be different in your lab.`

{% hint style="info" %}
The End.DT4 function encodes a uDT SID by combining a fixed IPv6 prefix (the locator) with the received BGP label, allowing identification of the routing context for decapsulated IPv4 packets. The SID fcbb:bb00:4:e002:: is formed using the prefix fcbb:bb00:4::/48 assigned to PE2 and embedding the label 0xe0020 into the SID structure. This SID is programmed on PE2 with an End.DT4 behavior that maps the label 0xe0020 to a specific VRF or routing table, enabling PE2 to decapsulate SRv6 traffic and forward IPv4 packets (for example, to CE2 loopback 10.2.10.1) based on the correct routing context.
{% endhint %}

On the PE2 router, use the show route ipv6 fcbb:bb00:4:e002:: command. Label (:e002:) in the SRv6 uDT4 address may be different in your lab.

`RP/0/RP0/CPU0:PE2# show route ipv6 fcbb:bb00:4:e002::`

`Routing entry for fcbb:bb00:4:e002::/64`

`Known via "local-srv6 bgp-65001", distance 0, metric 0, SRv6 Endpoint uDT4, SRv6 Format f3216`

`Installed Feb 6 16:04:37.985 for 00:09:49`

`Routing Descriptor Blocks`

`::ffff:0.0.0.0 directly connected`

`Nexthop in Vrf: "1", Table: "default", IPv4 Unicast, Table Id: 0xe0000001`

`Route metric is 0`

`No advertising protos.`

`On the PE2 router, use the show cef ipv6 fcbb:bb00:4:e002:: command. Label (:e002:) in the SRv6 uDT4 address may be different in your lab.`

`RP/0/RP0/CPU0:PE2# show cef ipv6 fcbb:bb00:4:e002::`

`fcbb:bb00:4:e002::/64, version 97, SRv6 Endpoint uDT4, internal 0x1000001 0x0 (ptr 0x87166788) [1], 0x400 (0x8838c0f8), 0x0 (0x8984b828)`

`Updated Feb 6 16:04:37.988`

`Prefix Len 64, traffic index 0, precedence n/a, priority 0`

`gateway array (0x881f87a0) reference count 1, flags 0x0, source rib (7), 0 backups`

`[2 type 3 flags 0x8401 (0x882a5068) ext 0x0 (0x0)]`

`LW-LDI[type=3, refc=1, ptr=0x8838c0f8, sh-ldi=0x882a5068]`

`gateway array update type-time 1 Feb 6 16:04:37.988`

`LDI Update time Feb 6 16:04:37.988`

`LW-LDI-TS Feb 6 16:04:37.988`

`via ::ffff:0.0.0.0/128, 0 dependencies, weight 0, class 0 [flags 0x0]`

`path-idx 0 NHID 0x0 [0x87802198 0x0]`

`next hop VRF - '1', table - 0xe0000001`

`next hop ::ffff:0.0.0.0/128`

`Load distribution: 0 (refcount 2)`

`Hash OK Interface Address`

`0 Y recursive Lookup in table`

`SR`

## SRv6 Summarization

Among many reasons for the wide adoption of SRv6 uSID technology is ultimate scalability. SRv6 uSID currently provides full feature parity with SR-MPLS but in a much simpler manner, and with much higher scalability. The key concept for infinite scalability is the applicability of classless routing (CIDR) to SRv6 uSID networks.

Let’s say we have a midsized network with 30k routers. It is obvious that we cannot handle such a network as a single IGP domain. We must split it into multiple IGP domains. Either using the hierarchical structure of IGP protocols (ISIS levels or OSPF areas) or even using different IGP processes.

For simplicity, we split our network into 30 domains with 1000 nodes each. As we need to maintain any-to-any connectivity, an obvious option is to redistribute all SRv6 locators everywhere. But IGP protocols have their scalability limits as well. Attempt to redistribute all locators across would reach IGP limits.

SRv6 offers a very elegant solution to that problem: summarization. Every border router will propagate a few summary prefixes instead of all locators.

![Figure 1 - Summarization](https://www.segment-routing.net/images/demo-upa/UPA_fig1.png)

_Figure 1 - Summarization_

In Figure 1 we can see an example of summarization. 1000 /48 Locator prefixes are summarized into the core as four /40 networks. As a result, in the core there will only be 120 summary routes instead of 30k, while still providing any-to-any connectivity. 120 networks in a single domain are simple to handle for any IGP routing protocol and easy to handle for any HW platform.

For a network of this size, we will probably not do any additional summarization towards Domain 0, but for very large networks hierarchical summarization will be necessary.

Now we have a perfectly scalable network thanks to the reduced level of information propagated among domains via summarization. But consequently, PE1 does not see any failure happening outside of its local Domain0. So, when PE11 in Domain2 fails it cannot trigger BGP Prefix Independent Convergence (BGP PIC) on PE1.

BGP Prefix Independent Convergence is a technology allowing fast switchover, in a prefix independent manner, to redundant paths in case of an egress PE failure. In a nutshell, the ingress PE1 receives many prefixes from a primary egress PE11 and the same set of prefixes from a secondary egress PE12. The ingress PE1 programs all primary prefix paths into the hardware forwarding table. the ingress PE1 also programs all prefix paths from the secondary egress PE12 into the hardware as backup paths. Then once the IGP of the ingress PE1 discovers the failure of the primary egress PE11, it just triggers an immediate switchover to the backup paths.

To trigger the switchover, ingress PE needs to be notified about the failure of the primary egress PE. This notification comes naturally from IGP update in a single IGP domain or in multi-domain networks that are not using summarization. In such cases the ingress PE triggers BGP PIC and traffic restoration is very fast.

However, with summarization, the ingress PE will never be notified about an egress PE failure. Because the egress PE failure is hidden by the summary route which is not changing at all. Traffic restoration following an egress PE failure relies on BGP convergence, which can be very slow.

This demonstration shows how BGP PIC can be triggered within very large networks where summarization is in place. Technology to achieve it is called UPA: Unreachable Prefix Announcement.

### Demonstration network description <a href="#demonstration-network-description" id="demonstration-network-description"></a>

Our demonstration network is shown in Figure 2.

![Figure 2 - Network](https://www.segment-routing.net/images/demo-upa/UPA_fig2.png)

_Figure 2 - Demonstration network_

There are basically two ISIS domains. For this presentation, we are using different ISIS levels, but the solution will work the same way with redistribution between two IS-IS instances.

In between domains we have a single Area Border Router (ABR). The ABR is responsible for routing information propagation between domains. This router can summarize routing information for each domain.

In Domain 2 we have two redundant PE routers: PE11 and PE12. Both PEs are connected to a single CE1 and this CE is connected to the IXIA traffic generator.

IXIA is advertising 800k IPv4 prefixes to the CE router. The CE router advertises all these prefixes to PE11 and PE12. Both PEs are advertising the prefixes to the Route Reflector in VPNv4 address family. The Route Reflector propagates them towards the ingress PE1. Note that the solution also works for other address-families.

So here we can see how many routes are in the VRF named “INTERNET” on PE1:

```
  RP/0/RP0/CPU0:PE1#sh route vrf INTERNET summary

  Route Source                     Routes     Backup     Deleted     Memory(bytes)
  connected                        1          0          0           216
  local                            1          0          0           216
  static                           1          0          0           216
  bgp 100                          799861     0          0           204764416
  dagr                             0          0          0           0
  application l2vpn_evpn           0          0          0           0
  Total                            799864     0          0           204765064
```

BGP PIC is enabled on ingress PE1, hence we can verify for one specific route (1.0.5.0/24) that the primary path is coming from PE11 and the backup path is coming from PE12:

```
RP/0/RP0/CPU0:PE1#sh route vrf INTERNET 1.0.5.0/24 detail
Sat Apr  2 07:45:30.457 EDT

Routing entry for 1.0.5.0/24
  Known via "bgp 100", distance 200, metric 0
  Tag 102
  Number of pic paths 1 , type internal
  Installed Apr  2 07:37:30.169 for 00:08:00
  Routing Descriptor Blocks
    fccc:cc00:2011::1, from fccc:cc00:9::1
      Nexthop in Vrf: "default", Table: "default", IPv6 Unicast, Table Id: 0xe0800000
      Route metric is 0
      Label: None
      Tunnel ID: None
      Binding Label: None
      Extended communities count: 0
      Source RD attributes: 0x0000:11:1
      NHID:0x0(Ref:0)
      SRv6 Headend: H.Encaps.Red [f3216], SID-list {fccc:cc00:2011:efa0::}
      MPLS eid:0x11c3000000001
    fccc:cc00:2012::1, from fccc:cc00:9::1, BGP backup path
      Nexthop in Vrf: "default", Table: "default", IPv6 Unicast, Table Id: 0xe0800000
      Route metric is 0
      Label: None
      Tunnel ID: None
      Binding Label: None
      Extended communities count: 0
      Source RD attributes: 0x0000:12:1
      NHID:0x0(Ref:0)
      SRv6 Headend: H.Encaps.Red [f3216], SID-list {fccc:cc00:2012:efa1::}
      MPLS eid:0x11c2f00000001
  Route version is 0xa1 (161)
  No local label
  IP Precedence: Not Set
  QoS Group ID: Not Set
  Flow-tag: Not Set
  Fwd-class: Not Set
  Route Priority: RIB_PRIORITY_RECURSIVE (12) SVD Type RIB_SVD_TYPE_REMOTE
  Download Priority 3, Download Version 223884738
  No advertising protos.
```

The IXIA port on the left sends 800k streams of measurement traffic, one to each of the BGP prefixes, to verify connectivity to all 800k prefixes. IXIA uses packet loss to measure convergence time in case of failure. We will simulate primary egress PE11 failure by shutting down its core facing interface.

To demonstrate the efficiency of UPA, we will do three different measurements of network convergence.

1. Without summarization
2. With summarization
3. With summarization and UPA enabled

### Convergence without summarization <a href="#convergence-without-summarization" id="convergence-without-summarization"></a>

Without any summarization configured on the ABR, ingress PE1 receives both PE11 and PE12 locators:

```
RP/0/RP0/CPU0:PE1#sh route ipv6 | incl fccc:cc00:20

i ia fccc:cc00:2011::/48
i ia fccc:cc00:2012::/48
```

Once we trigger the PE11 failure, IGP deletes the locator prefix of PE11 and PE1 triggers BGP PIC.

![Figure 3 - IXIA - Failure without summarization](https://www.segment-routing.net/images/demo-upa/UPA_fig3.png)

_Figure 3 - IXIA - Failure without summarization_

The IXIA measurement shows that convergence is very fast: 353 milliseconds. This is the sequence of events during that time:

1. The IGP in domain 2 detects PE11’s failure
2. The IGP floods the failure throughout domain 2
3. The ABR propagates that information into domain 1
4. The IGP in domain 1 floods the information
5. PE1 receives the information and triggers BGP PIC – switching to all preprogrammed backup paths

This solution can only be used for small networks, where the IGP scale allows all locators to be flooded within the domain.

### Convergence with summarization <a href="#convergence-with-summarization" id="convergence-with-summarization"></a>

In this test, we consider a larger network that requires summarization. Summarization configuration on the ABR, summarizing the domain 2 locator prefixes into domain 1:

```
router isis 100
 address-family ipv6 unicast
  summary-prefix fccc:cc00:2000::/40 level 2
```

After configuring the summary prefix, PE1 no longer receives individual locators of domain 2:

```
RP/0/RP0/CPU0:PE1#sh route ipv6 | incl fccc:cc00:20

i ia fccc:cc00:2000::/40

```

Therefore, the IGP of domain 1 no longer receives PE11 failure notifications and BGP PIC can’t be triggered anymore on PE1.

![Figure 4 - IXIA - Failure with summarization](https://www.segment-routing.net/images/demo-upa/UPA_fig4.png)

_Figure 4 - IXIA - Failure with summarization_

In this measurement, the convergence was very slow (more than 50 seconds) due to the delay of BGP detecting and propagating the failure. This is the sequence of events:

1. The Route Reflector detects the failure of the BGP session to PE11
2. The Route Reflector sends BGP withdraw messages to PE1 for all prefixes, one by one
3. PE1 reprograms the FIB entry for each prefix

The overall convergence time depends on the number of prefixes. The more prefixes the longer the convergence time will be. But we need fast BGP PIC convergence even in very large networks where summarization is used!

### Unreachable Prefix Announcement <a href="#unreachable-prefix-announcement" id="unreachable-prefix-announcement"></a>

The solution is UPA – Unreachable Prefix Announcement. What is it and how does it work?

Essentially, UPA is a regular IGP update that announces the unreachability of a prefix. UPA informs the ingress PE about an egress PE failure and enables the ingress PE to trigger BGP PIC.

The unreachability property of the prefix is carried by using an “unreachable” metric, which is already part of the ISIS protocol definitions (According to RFC5308, any prefix advertised metric larger than MAX\_V6\_PATH\_METRIC 0xfe000000 must not be considered during path computation and can be used for other purposes). Thus, UPA doesn’t require any protocol extension.

The figure below shows the example network in stable state. The IGP of PE11 advertises its LSP with locator /48 into domain 2. The ABR receives this LSP and advertises the /40 summary prefix into domain 1. PE1 receives this summary prefix that provides reachability to PE11.

![Figure 5 - UPA Stable state](https://www.segment-routing.net/images/demo-upa/UPA_fig5.png)

_Figure 5 - UPA Stable state_

When the ABR loses reachability to PE11 in domain 2, the ABR recognizes that the locator of PE11 is part of the summary prefix and generates a UPA for the locator of PE11. The UPA is flooded throughout domain 1. The IGP of PE1 receives the UPA and triggers BGP PIC for all BGP prefixes learned via PE11.

![Figure 6 - UPA Remote PE failure](https://www.segment-routing.net/images/demo-upa/UPA_fig6.png)

_Figure 5 - UPA Remote PE failure_

The goal of UPA is to notify about unreachability of prefixes so routers that are part of a remote domain can act upon this notification. Thus, UPA prefixes are not intended to be persistent.

After some time period the ABRs automatically withdraw the UPA. This time period is to allow full BGP convergence and is configurable.

For UPA to work, only the ABRs and ingress PEs routers need to be upgraded. All intermediate nodes will flood the UPA prefix seamlessly.

### Convergence with summarization and UPA <a href="#convergence-with-summarization-and-upa" id="convergence-with-summarization-and-upa"></a>

Unreachable Prefix Announcement configuration is very simple. It needs to be configured on the ABR – to generate the UPA route:

```
router isis 100
 address-family ipv6 unicast
  summary-prefix fccc:cc00:2000::/40 level 2 adv-unreachable
```

And also on the ingress PE router to react on UPA:

```
router isis 100 
address-family ipv6 unicast
  prefix-unreachable
   rx-process-enable
```

Now we can simulate a PE11 failure and measure convergence using IXIA.

![Figure 7 - IXIA - Failure with summarization and UPA](https://www.segment-routing.net/images/demo-upa/UPA_fig7.png)

_Figure 7 - Failure with summarization and UPA_

We can see that the convergence time is exactly the same as without summarization, precisely 353 milliseconds. This is the sequence of events during that time:

1. The IGP in domain 2 detects PE11’s failure
2. The IGP floods the failure throughout domain 2
3. The ABR receives the failure information, generates a UPA for PE11’s prefixes and sends it into domain 1
4. The IGP in domain 1 floods the UPA
5. PE1 receives the UPA and triggers BGP PIC – switching to all preprogrammed backup paths

This is a snippet of the ISIS LSP showing the UPA generated by the ABR:

```
ABR.00-00           * 0x0000071b   0x6e13        1068 /*            0/0/0
  Area Address:   49
  SRv6 Locator:   MT (IPv6 Unicast) fccc:cc00:4::/48 D:0 Metric: 0 Algorithm: 0
    Prefix Attribute Flags: X:0 R:0 N:0 E:0 A:0
    END SID: fccc:cc00:4:: uN (PSP/USD)
      SID Structure:
        Block Length: 32, Node-ID Length: 16, Func-Length: 0, Args-Length: 0
……SNIP………
        
  SRv6 Locator: MT (IPv6 Unicast) fccc:cc00:2011::/48 D:0 Metric: 4261412865 Algorithm: 0
    Prefix Attribute Flags: X:0 R:1 N:0 E:0 A:0
  NLPID:          0x8e
  Hostname:       ABR
……SNIP………
  MT:             IPv6 Unicast                                 0/0/0
  Metric: 0          MT (IPv6 Unicast) IPv6 fccc:cc00:4::/48
    Prefix Attribute Flags: X:0 R:0 N:0 E:0 A:0
  Metric: 20         MT (IPv6 Unicast) IPv6 fccc:cc00:5::/48
    Prefix Attribute Flags: X:0 R:1 N:0 E:0 A:0
  Metric: 4261412865 MT (IPv6 Unicast) IPv6 fccc:cc00:2011::/48
    Prefix Attribute Flags: X:0 R:1 N:0 E:0 A:0
  Metric: 10         MT (IPv6 Unicast) IPv6-External fccc:cc00:2000::/40
    Prefix Attribute Flags: X:0 R:0 N:0 E:0 A:0
……SNIP………
```

We can see that the ABR still advertises the summary /40 prefix. The ABR also advertises the UPA: prefix fccc:cc00:2011::/48 with metric 4261412865 (0xFE000001). This metric value is the default value of an “unreachable” metric in our implementation, it can be configured if necessary. As indicated before, the UPA is present in the ISIS LSP for a period of time to allow BGP convergence. It will be automatically removed from the ISIS LSP after configurable time (3 minutes by default).

[https://www.segment-routing.net/demos/upa/](https://www.segment-routing.net/demos/upa/)
