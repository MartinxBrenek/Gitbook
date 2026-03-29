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

# OSPFv2

### Overview

OSPF is an open-standard Link-state routing protocol, that shares its link state with other routers

OSPF is a link-state routing protocol. You can think of a link as an interface on a router. The state of the link is a description of that interface and of its relationship to its neighboring routers. A description of the interface would include, for example, the IP address of the interface, the subnet mask, the type of network to which it is connected, the routers that are connected to that network, and so on. The collection of all these link states forms an LSDB. All routers in the same area share the same LSDB. Routers in other OSPF areas will have different LSDBs.

Routing updates are exchanged between OSPF routers with Link-state update packets, that contains Link-state Advertisements (LSA) describing all networks that the router is connected to

The OSPF dynamic routing protocol does the following:

OSPF routers first establish neighbor adjacencies.

Creates a neighbor relationship by exchanging hello packets

Propagates LSAs rather than routing table updates:

**Link**: Router interface

**State**: Description of an interface and its relationship to neighboring routers

Floods LSAs to all OSPF routers in the area, not just to the directly connected routers

Pieces together all the LSAs that OSPF routers generate to create the OSPF Link-state database (LSDB)

LSDB is identical for all routers within one OSPF domain/area

Essentially, an LSDB is an overall map of the networks in relation to the routers. It contains the collection of LSAs that all routers in the same area have sent. Because the routers within the same area share the same information, they have identical topological databases.

Uses the SPF algorithm to calculate the shortest path to each destination and places it in the routing table

Each router then uses the LSDB information to run SPF algorithm to determine the best path to individual network

Each OSPF router maintains neighbor table, which is adjacency database of directly connected neighbors, topology table, e.g. LSDB containing all routers and their links in the area and RIB, containing a list of best path to destinations

A router sends LSA packets immediately to advertise its state when there are state changes. Moreover, the router resends (floods) its own LSAs every 30 minutes by default as a periodic update. The information about the attached interfaces, the metrics that are used, and other variables are included in OSPF LSAs. As OSPF routers accumulate link-state information, they use the SPF algorithm to calculate the shortest path to each network.

OSPF can be deployed as a single Area called a Flat design, which is suitable for small network, but if larger network is needed, Multiarea design should be used, with two-level Hierarchy model

### Multi-Area OSPF

OSPF domain (AS) can be segmented into multiple sub-domains called Areas, each of them have their respective Area ID. Areas are logical subdivisions of the autonomous system.

This reduces the size of the LSDB for each area, which relieves the load on the router's CPU, speeds up SPF tree calculations, and reduces LSDB flooding between routers in response to link flapping.

The optimal number of routers per area varies based on factors such as network stability, but the general recommendation is to have no more than 50 routers per single area.

The hierarchy defines Backbone Area 0 and nonbackbone areas. Each area has its LSDB topology with its SPT calculated, that is not visible from outside the area

The Backbone Area 0 serves as central area that is used for exchanging inter-area routes between all other areas

To avoid routing loops and adhere to the hierarchical structure all nonbackbone areas must have either physical or virtual connections to the backbone - e.g Area 0 must to be contiguous e.g. there should be no physical disconnections within Area 0

The reason for this star-like topology is that OSPF inter-area routing uses the distance-vector approach and a strict area hierarchy permits avoidance of the ["counting to infinity" problem.](https://onenote/#Routing\&section-id={6EC9C333-3E04-41C1-8C01-0E8D85C128A8}\&page-id={23330675-9420-47AE-8368-54C860A78BB9}\&object-id={AE859F1F-9B86-0FDB-3ADE-4A69025D1702}&10\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE/L3.one)

Another reason is that the propropagation of routing changes may be considerably delayed if the areas were connected in chain

The routing update from area 1 up to the area 10 would have to pass through 8 other areas in between

When ABRs are connected to area 0 they can easily send the route another ABR through area 0, with just one backbone area to cross, which significantly improves convergence

![](<../.gitbook/assets/Unknown image (107)>)

### Configuration

| show ip ospf \[ interface \| rib \| summary-address ]                                             | interface shows DR and BDR                                                                                                                                                                                                                                                                                                                                           |
| ------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip ospf neighbor \[detail]                                                                   | to verify adjacency uptime                                                                                                                                                                                                                                                                                                                                           |
| show ip ospf database \[router \| network \| summary\| asbr-summary \| external \| nssa-external] | to view each LSA type that is being advertised by router 1 2 3 4 5 7                                                                                                                                                                                                                                                                                                 |
| show ip ospf database self-originate                                                              | displays LSAs from the local router                                                                                                                                                                                                                                                                                                                                  |
| show ip ospf border-routes                                                                        | Displays the internal OSPF routing ABR and ASBR table entries.                                                                                                                                                                                                                                                                                                       |
| show ip ospf database self-originate \| begin Summary\_Net                                        | Verification on ABR self originated Type 3 LSA                                                                                                                                                                                                                                                                                                                       |
| show ip route ospf                                                                                |                                                                                                                                                                                                                                                                                                                                                                      |
| clear ip ospf process                                                                             | restarts ospf process                                                                                                                                                                                                                                                                                                                                                |
| IOS-XE Base Config                                                                                |                                                                                                                                                                                                                                                                                                                                                                      |
| router ospf                                                                                       | OSPF process ID (PID) is a locally significant identifier used to distinguish multiple OSPF routing instances running on a single router (config-router)# domain-id type <> value <> example: domain-id type 0205 value 111111222222                                                                                                                                 |
| router-id \<x.x.x.x>                                                                              |                                                                                                                                                                                                                                                                                                                                                                      |
| neighbor                                                                                          | When multicast can’t be used, neighbors can be specified manually to be established with unicast                                                                                                                                                                                                                                                                     |
| network \<x.x.x.x> \<x.x.x.x> area                                                                | The network statement tells OSPF to: Enable OSPF on interfaces whose IP addresses fall within the specified network and wildcard mask. Advertise those networks into the OSPF domain. Sends OSPF packets out of the included interfaces The area-id can be specified in two ways: Decimal value: 0 to 4294967295 Dotted decimal notation: 0.0.0.0 to 255.255.255.255 |
| network 0.0.0.0 255.255.255.255 area <>                                                           | include all connected networks                                                                                                                                                                                                                                                                                                                                       |
| (config-if)# ip ospf area <>                                                                      | OSPF enabled directly under interface config instead                                                                                                                                                                                                                                                                                                                 |

{% hint style="info" %}
You can omit `ip` in `show ip ospf ...` to display output for both AFIs / OSPF versions (platform-dependent).

If you run two OSPF processes on one router and filter/redistribute between them, the router behaves like an **ASBR** (not an ABR).
{% endhint %}

{% hint style="info" %}
OSPF advertises loopback networks as `/32`, even if you configured a different mask. This is RFC behavior.

To advertise the configured mask instead, set `ip ospf network point-to-point` under the loopback.
{% endhint %}

### VRF-aware OSPF

{% hint style="info" %}
The 32-process limitation was removed in 12.3(4)T: OSPF support for unlimited software VRFs per Provider Edge (PE) router.
{% endhint %}

| Router(config)#router ospf process-id vrf VRF-name       | /IOS This command starts up per-VRF OSPF routing process. At least one IP interface has to be “up” in VRF !!! |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| RP/0/RP0/CPU0:router#router ospf process-id vrf VRF-name | /XR                                                                                                           |

#### IOS-XR

| router ospf 1 router-id 172.16.100.30 area 0 interface Loopback0 passive enable ! interface GigabitEthernet0/0/0/0 ! ! area 3 interface GigabitEthernet0/0/0/1 |                  |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- |
| show route ipv4 ospf                                                                                                                                           |                  |
| show ip ospf rib                                                                                                                                               | to show ospf RIB |

OSPF domain-id is inherited from the OSPF PID, it can can also be set manually so that it is independent from the OSPF PID

In IOS XE, the domain ID is typically not required unless MPLS Layer 3 VPNs are in use with OSPF as the PE-CE routing protocol.

XR, designed for service providers, makes the domain ID more prominent in multi-tenant OSPF setups.

In IOS XR, the Domain ID is used to define an extended community value that identifies the OSPF routing domain in an MPLS VPN environment. This configuration is particularly important for OSPF routing across a Service Provider MPLS backbone

In an MPLS Layer 3 VPN, OSPF is often used as a PE-CE (Provider Edge to Customer Edge) routing protocol.

The OSPF Domain ID helps differentiate between OSPF domains when redistributing routes between the CE and PE.

| RP/0/RP0/CPU0:PE-003-XR(config)#router ospf 100 RP/0/RP0/CPU0:PE-003-XR(config-ospf)#vrf B RP/0/RP0/CPU0:PE-003-XR(config-ospf-vrf)#domain-id type 0005 value 000000640200 | type parameter in the domain-id configuration specifies the type of OSPF Domain ID extended community to be used 0005 is the most common and widely used type in MPLS Layer 3 VPN environments when OSPF is the PE-CE protocol. 0105 is required for IPv6 OSPF (OSPFv3) configurations in MPLS VPNs. 0205 might be chosen for flexibility in non-standard deployments. 8005 is rarely used and typically specific to certain vendor implementations or custom scenarios. |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

### OSPF packet format

The OSPF packets are not using TCP or UDP for transport, instead they use dedicated IP protocol number 89 in the IP protocol type field

![](<../.gitbook/assets/Unknown image (108)>)

**Version number**: Version 2 for OSPF with IPv4 and version 3 for OSPF with IPv6.

**Type**: Differentiates the five OSPF packet types.

**Packet length**: The length of the OSPF packet in bytes.

**Router ID**: Defines which router is the source of the packet.

**Area ID**: Defines the area where the packet originated.

**Checksum**: Used for packet-header error detection to ensure that the OSPF packet was not corrupted during transmission.

**Authentication type**: An option in OSPF that describes no authentication, cleartext passwords, or encrypted Message Digest 5 (MD5) formats for router authentication.

**Authentication**: Used in the authentication scheme.

**Data**: Each of the five packet types includes different packet type described below

### OSPF packets

#### OSPF packet header

Every OSPF packet starts with a standard 24 byte header. This header contains all the information necessary to determine whether the packet should be accepted for further processing

![](<../.gitbook/assets/Unknown image (109)>)

#### Hello (Type 1)

Type 1 Hello Messages are responsible for establishing and maintaining neighbor relationships and ensuring that communication between neighbors is bidirectional. Bidirectional communication is indicated when the router sees itself listed in its neighbor's Hello Packet

Hello messages are sent periodically on OSPF-enabled interface as a multicast to all OSPF routers IP 224.0.0.5 (MAC 01:00:5E:00:00:05) on which all OSPF routers listen to form and maintain neighborships (keepalives)

The hello frequency is defined by the network type and it contains RID, Area ID, Router priority, Authentication, Hello/Dead Timers, Stub Flag, DR/BDR IP, Media type, RIDs of neighbors

Nowadays, the majority of connections between neighboring routers are based on ethernet. To achieve a faster routing performance, it makes sense to convert the media types of both neighbors from the default value “broadcast” to “point-to-point”.

Each router that received the hello packet sends a unicast reply hello packet to R1 with its corresponding information

Hello Timer is configurable time interval for sending hello messages out of an ospf-enabled interface

Dead Timer is the time interval within which the router expects to receive Hello packets from its neighbor

When the Dead Timer expires (it is 4x Hello interval - 40sec by default) without receiving a Hello packet, the OSPF neighbor relationship is deemed to be down, and the routers transition to the Down state

![](<../.gitbook/assets/Unknown image (110)>)

![](<../.gitbook/assets/Unknown image (111)>)

These packet types are used to exchange the individual LSA's:

#### Database Description (DBD) (Type 2)

Type 2 Database Description (DBD) used during the initial exchange of summarized LSDB between neighboring routers to synchronize their link-state databases

Wait Timer is associated with the DBD process during the OSPF neighbor formation, specifically in the transition from the ExStart state to the Exchange state. If the router receives the expected DBD packet before the Wait Timer expires, it transitions to the next state. However, if the timer expires without receiving the expected packet, the router may re-enter the Exchange state.

![](<../.gitbook/assets/Unknown image (112)>)

#### Link-state request (LSR) (Type 3)

Type 3 Link-state request (LSR) used to request missing LSA's to complete the full LSDB. It is also used to request LSA if the LSDB is found to be out of date

#### Link-state update (LSU) (Type 4)

Type 4 Link-state Update (LSU) each router advertise separate LSU for each neighbor in its neighbor table, each LSU contains all LSA's that the individual neighbor (adv router) is connected to. LSU is also used to respond to the LSR or to update all routers of a link-state change (link went down or neighbor is unreachable)

![](<../.gitbook/assets/Unknown image (113)>)

#### Link-state acknowledgement (LSAck) (Type 5)

Type 5 Link-state Acknowledgement (LSAck) message to confirm the successful receipt of LSA flood within LSU packet

{% hint style="info" %}
Type 4 and Type 5 packets are sent to multicast IPs, except when retransmitting, when sent across a virtual link, and when sent on nonbroadcast networks.

All other packets are sent to unicast IPs.
{% endhint %}

#### Options field

The OSPF Options field is present in OSPF Hello packets, Database Description packets and all LSAs. The Options field enables OSPF routers to support (or not support) optional capabilities, and to communicate their capability level to other OSPF routers. Through this mechanism routers of differing capabilities can be mixed within an OSPF routing domain.

1. DN (Down bit)

Purpose: Used in MPLS/VPN environments to prevent routing loops.

Description: The DN bit is set in LSAs that are sent from PE (Provider Edge) routers in MPLS to prevent reintroduction of routes back into the MPLS backbone.

2. O (Opaque bit)

Purpose: Indicates support for Opaque LSAs.

Description: Routers that understand Opaque LSAs will set this bit. Opaque LSAs are used to carry additional information such as traffic engineering data or other application-specific information.

3. DC (Demand Circuits)

Purpose: Indicates support for Demand Circuits.

Description: Routers with this bit set can suppress sending periodic Hello packets and LSAs over demand circuits (e.g., dial-up links) to reduce overhead.

4. L (Link-local signaling bit)

Purpose: Used for link-local signaling in OSPF.

Description: Indicates that a router supports link-local signaling, which allows OSPF to use local mechanisms for communication between routers on the same link.

5. NP (NSSA bit, Type 7 LSAs)

Purpose: Indicates support for Not-So-Stubby Areas (NSSAs).

Description: Routers with this bit set understand Type 7 LSAs, which are used in NSSAs to advertise external routes.

6. MC (Multicast bit)

Purpose: Indicates support for OSPF multicast routing.

Description: Routers with this bit set can participate in multicast OSPF (MOSPF), which extends OSPF to support IP multicast routing.

7. E (External Routing Capability)

Purpose: Indicates the ability to process Type 5 External LSAs.

Description: Routers with this bit set are capable of routing external routes. If the bit is clear, the area is considered a stub area.

8. T (Type of Service Routing)

Purpose: Indicates support for Type of Service (ToS) routing.

Description: Although ToS-based routing is no longer commonly used, this bit indicates that the router can calculate separate routes for each ToS.

![](<../.gitbook/assets/Unknown image (114)>)

### Adjacency rules

Routers that share common segment become neighbors, they have to fall within same subnet

Router ID (RID) is a 32-bit identifier that must be unique as it is used to identify originating router in LSAs. If not configured, then the highest loopback IP, if no loopback, then the highest IP of any interface is used

The Area ID, Area type flags, OSPF Hello and Dead timers must match as well as MTU, because OSPF does not support fragmentation

OSPF passive interface disables the sending and receiving of OSPF Hellos and routing updates on an interface, preventing adjacency to establish

This can be useful when the link is flapping and, but we don't want to shut the link, and rather prevent the routing protocol from using it

{% hint style="info" %}
In EIGRPv6 and OSPFv3, passive interfaces still advertise the associated prefix. They just don’t form adjacencies.
{% endhint %}

| (config-router)# passive-interface \[ default ] | default keyword disables advertisement propagation on all interfaces |
| ----------------------------------------------- | -------------------------------------------------------------------- |

### Adjacency states

**DOWN**: initial state of a neighbor relationship as router has not received any OSPF hello packets yet

**ATTEMPT**: relevant to NBMA networks indicating that Hello Packets haven’t been received within the dead interval

**INIT**: hello packet has been received from another router, no bidirectional communication yet

**2-WAY**: bidirectional communication has been established. If a DR or BDR is needed, the election occurs during this state

If the link type is a broadcast network, a DR and BDR must first be selected. This process must occur before the routers can begin exchanging link-state information

**EXSTART**: routers negotiate who will be the master - that is with the highest RID and who slave in order to synchronize their LSDB.

**WAIT**: can be rarely observed, referring to the wait timer

**EXCHANGE**: routers are exchanging link states by using DBD packets. This state can also indicate that interface MTU does not match.

**LOADING**: LSR packets are sent to the neighbor, requesting more recent LSAs that haven't been received in the Exchange state

**FULL**: the adjacency is established and the routers can exchange LSA's and build their LSDB

### Authentication

Authentication material is inserted into OSPF header of every OSPF packet and checked by the other router to prevent undesired adjacencies and rogue routes to be inserted into OSPF

An attacker that might "poison" the routing table of the router by sending a route toward one of the networks, using good cost, and traffic to that network would be diverted to the attacker router. Support: T0: None (Null); T1: Clear Text; T2: Crypto (MD5/SHA)

| ip ospf authentication message-digest ip ospf message-digest-key md5      | applies authentication without key chain both commands required as confirmed in the lab The interface on the OSPF peer must use the same key ID and key value as the configured interface. for XR type without ip |
| ------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| area <> authentication <>                                                 | applies authentication for all the interfaces in a particular area                                                                                                                                                |
| ip ospf authentication key-chain KEY1                                     | key chain applied to ospf interface; keyword null disables auth                                                                                                                                                   |
| key chain KEY1 key 1 key-string CCNP cryptographic-algorithm hmac-sha-256 | specifies key chain (key template with additional options in compare to above auth)                                                                                                                               |

### Graceful shutdown

Drop all neighbor adjacencies

Flush all LSAs that the router originated by setting the age to 3600 seconds

Send HELLO packets with the DR/BDR set to 0.0.0.0 and an empty neighbor list

This will trigger other routers to fall back to INIT state

Stop sending/receiving OSPF packets

This allows neighbors to quickly converge because they won’t wait until the hold time to the particular router expires

| (config-router)# shutdown (config-router-af)# shutdown (config-if)# ip ospf shutdown (config-if)# ospfv3 \[ipv4 \| ipv6] shutdown |   |
| --------------------------------------------------------------------------------------------------------------------------------- | - |

### Network types

Broadcast DR and BDR is automatically elected based on priority. Timers: Hello 10; Dead 40.

{% hint style="info" %}
Ethernet defaults to the **broadcast** network type.
{% endhint %}

Point to point (P2P) direct link between neighbors that are discovered automatically with Hellos. No DR BDR election. Timers: Hello 10; Dead 40.

{% hint style="info" %}
Serial links default to the **point-to-point** network type.
{% endhint %}

Non-Broadcast Multiaccess (NBMA) relies only on unicast traffic, so neighbors must be manually configured. DR and BDR election is required. Timers: Hello 30; Dead 120

Point to Multipoint treats the nonbroadcast network as a collection of point-to-point links and form a adjacency with a multiple neighbors over a single link and IP subnet

In this environment, the routers automatically identify their neighboring routers, but do not elect a DR and BDR. This configuration is typically used with partially meshed networks

Duplicate LSU are sent out of the single interface to each neighbor

Can leverage per-neighbor costs for route prioritization. Hello 30; Dead 120

Point-to-Multipoint Non-Broadcast requires neighbors to be manually configured and does not require DR and BDR election

This mode is used in special cases where broadcast and multicast cannot be used, so the neighbors cannot be automatically discovered

Compatible Network types Broadcast to Broadcast, Point-to-Point to Point-to-Point, Broadcast to Non-broadcast (adjust hello/dead timers), Point-to-Point to Point-to-Multipoint

| ip ospf network \[broadcast \| non-broadcast \| point-to-multipoint \| point-to-multipoint non-broadcast \| point-to-point] | specifies network type for an interface |
| --------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- |

### Router roles

#### Internal router

A router that has all its interfaces connected to only one OSPF area. This router is completely internal to the area.

#### Backbone router

A router that has at least one interface that is connected to the backbone area.

#### Area Border Router (ABR)

A router that has interfaces that are connected to at least two different OSPF areas, including the backbone area. ABRs contain LSDB information for each area, make route calculations for each area, and advertise routing information between areas.

The filtering and summarization must occur on ABR

ABR is distinguished by setting the B (border) bit in its router LSA to signal other routers in the same area of its ABR status

ABR Loop Prevention rules to maintain hierarchical structure

Since there is no common topology shared among different areas, loop prevention should be based on distance-vector principles

There are three main rules of generating and receiving inter-area routes (type-3 LSAs) in OSPF that prevent control-plane routing loops:

1. Type 1 LSAs received from an area create type 3 LSAs into the backbone area and non-backbone areas
2. If ABR receives a type 3 LSA from Area 0, it generates a new type 3 LSA for the nonbackbone area and lists itself as the advertising router, with the additional cost metric
3. If ABR receives a type 3 LSA from nonbackbone areas, it will install it into its LSDB, but it will not use it for SPF calculation and it will not create Type 3 LSA for nonbackbone and backbone areas. This is split-horizon-like is a mechanism used in OSPF for inter-area loop prevention, which prevents ABR from using other nonbackbone areas to reach an inter-area network.

{% hint style="info" %}
ABR has to have a **FULL** adjacency in Area 0 to start ignoring summary LSAs received over non-backbone areas. If it does not have FULL over Area 0, it is safe to keep accepting them. Otherwise it could create loops by re-flooding into Area 0.
{% endhint %}

<\<Loop-Prevention-in-OSPF.pdf>>

#### Autonomous System Boundary Router (ASBR)

A router that has at least one interface that is connected to an OSPF area and at least one interface that is connected to an external domain.

e.g redistributing routing information into the OSPF domain

{% hint style="info" %}
Summary and external LSAs can be blocked by an ABR. You can send only a default route instead.
{% endhint %}

![](<../.gitbook/assets/Unknown image (115)>)

#### Designated Router (DR)

to reduce the router-related traffic on a multi-access network the router with a highest priority or highest RID is elected as a DR, which serves as a central point for all DROTHER routers

{% hint style="info" %}
If priority is equal for all routers, the router with the highest IP is elected DR.
{% endhint %}

Routers on a segment have FULL state only with DR and BDR and share LSA only to the DR. DROTHER are all other routers in OSPF domain

In the event of DR failure, a Backup Designated Router (BDR) becomes new DR

No preemption when DR fails and BDR takes over, the failed router will not take over DR when it comes back up

Communication to DR on broadcast network is for All DR routers IP 224.0.0.6 (MAC 01:00:5E:00:00:06)

![](<../.gitbook/assets/Unknown image (116)>)

### Operation

If DROTHER one of its links changes state or learns new route it sends LSU with the updated LSA to allDRouters via .6 address

DR sends an unicast ack for DROTHER

DR floods the LSU to all DROTHER's via AllSPFRouters .5 add

### Link-state advertisements (LSAs) and LSDB

Each LSA serves a specific purpose and they all fit together, so that OSPF's SPF algorithm can link different LSA's together to supply end-to-end connectivity

For a router in Area 1 to route to external domain connected to Area 3 it has to look to Type5 LSA that provides the information about the external route, but to be able to route to that external route it has to look into Type 4 LSA to find the route to the ASBR, of it's ABR, to route it out of it's Area 3 to the Area 1, where the ASBR resides

Then we have to look at the Type-3 to get to that remote ABR. Finally we look at the Type-1 or Type-2 LSAs in our area to determine how to get to our closest ABR

#### LSA header

![](<../.gitbook/assets/Unknown image (117)>)

#### LSA operation

Each LSA entry has its own aging timer, LSA Age, which gives the time in seconds since the LSA was originated.

An LSA record will reset its maximum age when it receives a new LSA update

The MaxAge of the LSA is 3600 seconds, and the refresh time is 1800 seconds. If the link-state age reaches 3600 seconds, the LSA must be removed from the database.

Once the LSA age exceeds 30 minutes, the originating router floods LSU with the summary of the LSA again with the original age

Each time that a record is flooded, the LSA Sequence number is incremented by one (4byte number field ranging from 0x80000001 and the last one is 0x7FFFFFFF)

If the LSA does not already exist, the router adds the entry to its LSDB, sends back a link-state acknowledgment (LSAck), floods the information to other routers, runs SPF, and updates its routing table.

If the entry already exists and the received LSA has the same sequence number, the router ignores the LSA entry.

If the entry already exists but the LSA includes newer information (it has a higher sequence number), the router adds the entry to its LSDB, sends back an LSAck, floods the information to other routers, runs SPF, and updates its routing table.

If the entry already exists but the LSA includes older information, it sends an LSU to the sender with its newer information.

Suppose following example to interpret the screenshots within LSA Types

![](<../.gitbook/assets/Unknown image (118)>)

### LSA types

#### Type 1: Router LSA

is used to exchange information about nodes within an area. These LSAs will never leave an area.

router advertise each of its connected link in the LSU packet as individual Type 1 LSAs

LSA Type 1 Types: Type 1: P2P to another router, Type 2: Connection to transit (IP of DR), Type 3: Connection to a stub network, Type 4: Virtual router

![](<../.gitbook/assets/Unknown image (119)>)

{% hint style="info" %}
In `show ip ospf database`, you only see the router-id. To display connected networks, use `show ip ospf database router`.
{% endhint %}

![](<../.gitbook/assets/Unknown image (120)>)

![](<../.gitbook/assets/Unknown image (121)>)

![](<../.gitbook/assets/Unknown image (122)>)

![](<../.gitbook/assets/Unknown image (123)>)

#### Type 2: Network LSA

LSA type 2 is used to exchange information about multiaccess links within an area. These LSAs will also never leave an area.

in a multiaccess/transit network, such as Ethernet, the DR is responsible for flooding LSU with the Type 2 LSA to all routers within the transit area

It contains all attached routers that make up the transit network, including the DR itself and the subnet mask that is used on the link

{% hint style="info" %}
Type 2 LSAs never cross an area boundary. The link-state ID for a Network LSA is the DR’s interface IP.
{% endhint %}

Transit network is the opposite to the stub network/area, that serves as an intermediary network to pass traffic between segments or areas (for example broadcast network type, where the traffic passes through the shared segment and then to the final destination network of one of the routers in a broadcast segment)

![](<../.gitbook/assets/Unknown image (124)>)

The second link between CE1 and PE1 is configured to be broadcast to simulate broadcast transit network, so that DR generates the Type 2 LSA:

![](<../.gitbook/assets/Unknown image (125)>)

![](<../.gitbook/assets/Unknown image (126)>)

#### Type 3: Network Summary LSA

each type 1 and type 2 LSA received by the ABR from routers in one of the areas to which it is attached is atomized by the ABR into individual Type 3 LSAs with the boundary bit set

eg if a non-backbone area has 5 routers each advertising one LSU with one type 1 LSA, the ABR includes each individual type 1 LSA and advertises it as a type 3 LSA in the LSU packet to other areas, in other words

Atomization is the opposite of summarization because even though the ABR sends it in one LSU packet it still describes each Type 1 or Type 2 as separate Type 3 LSA, and sets himself as an advertising router to other areas. This can be adjusted using summarization or filtering on the ABR

{% hint style="info" %}
In a dual-ABR design between two areas, both ABRs propagate Type 3 LSAs into the area.
{% endhint %}

![](<../.gitbook/assets/Unknown image (127)>)

Those are Type 3 LSA's distributed by PE1 to the CE1

We can see that these Type 3 LSA's are the same as individual entries for each LSA Type 1 or LSA Type 2

![](<../.gitbook/assets/Unknown image (128)>)

![](<../.gitbook/assets/Unknown image (129)>)

#### Type 4: ASBR Summary LSA

is generated by an ABR and flooded into other areas whenever an ASBR exists within an area. Its purpose is to inform routers in different areas about the ASBR’s presence and provide a path to reach it.

The Link-State ID in this LSA is set to the ASBR’s Router ID, while the advertising router is the ABR. When routers in other areas receive a Type-5 External LSA, they learn about external routes but don’t automatically know how to reach the ASBR. That’s where the Type-4 LSA comes in—it tells those routers that traffic destined for the ASBR’s external routes should first be sent to the ABR, which acts as the gateway.

{% hint style="info" %}
Routers in the same area as the ASBR recognize it via the **E-bit** set in the ASBR’s Type 1 (Router) LSA. They won’t have a Type 4 LSA in their LSDB.
{% endhint %}

These LSAs are flooded throughout the backbone area to the other ABRs. The link entries are not flooded into totally stubby areas or totally NSSAs.

![](<../.gitbook/assets/Unknown image (130)>)

Example

By definition the LSA Type 4 is sourced by an ABR ,it is flooded only in one area. It tells to other routers located in this area how to reach an ASBR.

And since the LSA Type 4 is flooded only in area 1 so R3 will never has an LSA 4 in its LSDB , and in order to know how to reach the ASBR , R4 the ASBR set the E-bit

field to 1 in its LSA Type 1 to tell R3 that the advertising router of this LSA 1 is an ASBR.

Now how R3 in the area 0 with ASBR knows about ASBR, when the ABR floods the LSA to area 1? They receive an LSA 1 (router LSA) with E-bit set to 1 to indicate that the router is the ASBR

![](<../.gitbook/assets/Unknown image (131)>)

![](<../.gitbook/assets/Unknown image (132)>)

#### Type 5: AS External LSA

ASBRs generate AS external link advertisements. External link advertisements describe routes to destinations that are external to the AS and are flooded everywhere except for stub areas, totally stubby areas, NSSAs, and totally NSSAs - with the ASBR bit set

The link-state ID is the external network number. Because of the flooding scope and depending on the number of external networks, the default lack of route summarization can also be a major issue with external LSAs. Therefore, you should always attempt to summarize blocks of external network numbers at the ASBR to reduce flooding problems

{% hint style="info" %}
If the ASBR is in a normal area, it generates Type 5 LSAs. The ABR also floods Type 4 LSAs into other areas to advertise reachability to the ASBR.
{% endhint %}

{% hint style="info" %}
In a dual-ASBR design between two areas, both ASBRs propagate Type 5 LSAs into the area.
{% endhint %}

![](<../.gitbook/assets/Unknown image (133)>)

![](<../.gitbook/assets/Unknown image (134)>)

#### Type 7: AS External NSSA LSA

redistribution from external routing domain performed on ASBR in the NSSA creates this special type 7 LSA, which can exist only in an NSSA

This is then translated into LSA Type 5 on the NSSA ABR, then propagated as LSA Type 5 by subsequent ABR

Routers that are operating NSSA areas set the n-bit to signify that they can support type 7 NSSAs. These option bits must be checked during neighbor establishment. They must match for an adjacency to form.

The advertising router is set to the router ID of the router that injected the external route into OSPF a router inside this NSSA - which is ASBR

This address is also set as the "Forward Address" for this prefix - the address that is used to determine the path to take toward this external destination.

{% hint style="info" %}
If the ASBR is in a normal area, no Type 4 LSA is needed. Routers learn the ASBR via the Type 1 LSA E-bit. External routes are carried in Type 5 LSAs.
{% endhint %}

![](<../.gitbook/assets/Unknown image (135)>)

![](<../.gitbook/assets/Unknown image (136)>)

#### Other special LSA types

Type 6 used in multicast OSPF applications. Multicast OSPF is not supported by Cisco and is not widely used in general

Type 8 are specialized LSAs that are used in internetworking OSPF and Border Gateway Protocol (BGP). In OSPFv3, link LSAs (type 8) have link-local flooding scope and are never flooded beyond the link with which they are associated. Link LSAs provide the link-local address of the router to all other routers attached to the link, inform other routers that are attached to the link of a list of prefixes to associate with the link, and allow the router to assert a collection of options bits to associate with the network LSA that will be originated for the link.

The opaque LSAs—types 9, 10, and 11—are designated for future upgrades to OSPF for application-specific purposes. For example, Cisco uses opaque LSAs for MPLS with OSPF. Standard LSDB flooding mechanisms are used for distribution of opaque LSAs. Each of the three types has a different flooding scope.

In OSPFv3, a router can originate multiple intra-area-prefix LSAs (type 9) for each router or transit network, each with a unique link-state ID. The link-state ID for each intra-area-prefix LSA describes its association to either the router LSA or the network LSA and contains prefixes for stub and transit networks.

### Area types

The characteristics that are assigned to an area control the type of route information that it receives.

#### Backbone (Area 0)

is the central entity to which all other areas connect to, so that they can exchange and route information. Includes all the properties of a standard OSPF area.

#### Normal area

This default area accepts link updates, route summaries, and external routes.

#### Stub area

is a non-transit area at the edge of an OSPF domain. It connects only to the upstream router and does not connect the OSPF domain to any other network domain or area

This area does not accept information about external sources e.g. LSA Type 4 and 5, instead, ABR of the stub network floods only Type 3 with a default route

Configuring a stub area reduces the size of the LSDB inside the area, resulting in reduced memory requirements for routers in that area

#### Totally stub area (no-summary)

Cisco proprietary extension that does not accept any inter-area routes from other areas or external from ASBR

Totally stub Area ABR advertises only default route, which even further reduces the LSDB for this area

{% hint style="info" %}
Stub and totally stub areas do not support an ASBR inside the area. If you need redistribution inside the area, use (totally) NSSA.
{% endhint %}

![](<../.gitbook/assets/Unknown image (137)>)

#### Not-so-stubby area (NSSA)

similar to the stub and totally stub, however it allows ASBR - thus allowing to inject external prefixes into OSPF domain

Redistribution into an NSSA creates a special type of LSA known as a type 7 LSA, which can exist only in an NSSA. An NSSA ASBR generates this LSA, and an NSSA ABR translates it into a type 5 LS, sets him as advertising rouer, and propagate it into the OSPF domain. Type 7 LSAs have a propagate bit in the LSA header to prevent propagation loops between the NSSA and the backbone area.

{% hint style="info" %}
By default, an NSSA ABR does not originate a default route into the NSSA. Use `area nssa default-information originate`.
{% endhint %}

#### Totally NSSA area (no-summary)

Cisco proprietary extension that does not accept any internal routes from other areas or external from ASBR, thus blocks Type 3,4,5 LSAs and allows only default route from NSSA ABR

It support ASBR inside the area, injecting Type 7 LSA

![](<../.gitbook/assets/Unknown image (138)>)

| area \[stub \| stub no-summary \| nssa \| nssa no-summary ] | to change area type and influence the LSA flood                                                                                           |
| ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| area default-cost                                           | defines the stub's advertised external route metric                                                                                       |
| max-metric router-lsa \[arguments]                          | is used to set the metric of the router's LSA to the maximum value, effectively preventing the router from being used for transit traffic |

{% hint style="info" %}
Area type must be configured consistently on **all routers in the area**.
{% endhint %}

### Path selection / SPF algorithm

Edsger Dijkstra designed a mathematical algorithm for calculating the best paths through complex networks. Link-state routing protocols use Dijkstra's algorithm to calculate the best paths through a network. They assign a cost to each link in the network and place the specific node at the root of a tree, and then sum the costs toward each given destination. In this way, they calculate the branches of the tree to determine the best path to each destination. The best path is calculated with respect to the lowest total cost of links to a specific destination and is put into the forwarding database (routing table). For OSPF, the default behavior is that the interface cost is calculated based on its configured bandwidth. You can also manually define an OSPF cost for each interface, which overrides the default cost value.

A metric is an indication of the overhead that is required to send packets across a certain interface. OSPF uses cost as a metric. A smaller cost indicates a better path than a higher cost. By default on Cisco devices, the cost of an interface is inversely proportional to the bandwidth of this interface, so a higher bandwidth indicates a lower cost. For example, there is more overhead, a higher cost, and more time delays that are involved in crossing a 10-Mbps Ethernet line than in crossing a 100-Mbps Ethernet line.

By default on Cisco devices, the cost is calculated based on the interface bandwidth.

Cost = Reference Bandwidth / Interface Bandwidth

The cost value is a 16-bit positive number between 1 and 65,535, where a lower value is a more desirable metric and is calculated Cost = (10^8) / bandwidth (in bps)

The default reference bandwidth is 100 Mbps

All links that are faster than Fast Ethernet will have an OSPF cost of 1.

The path cost is the cumulative cost of all links on the path to the destination.

Each router has its own view of the topology even though all the routers build the shortest path trees by using the same LSDB.

Each router places itself as the root of a tree and then runs the SPF algorithm.

Each router uses the information in its topological database to calculate a shortest path tree, with itself as the root. The router then uses this tree to determine the best routes, which are offered to the routing table to route network traffic.

![](<../.gitbook/assets/Unknown image (139)>)

On Cisco devices, the formula used to calculate OSPF cost is cost = reference bandwidth / interface bandwidth (in bits per second).

The default reference bandwidth is 108, which is 100,000,000, or the equivalent of the bandwidth of Fast Ethernet. Therefore, the default cost of a 10-Mbps Ethernet link will be 108 / 107 = 10, and the cost of a 100-Mbps link will be 108 / 108 = 1. The problem arises with links that are faster than 100 Mbps. Because the OSPF cost has to be an integer, all links that are faster than Fast Ethernet will have an OSPF cost of 1.

There are three approaches you can take to influence the cost to be more realistic, especially on high-speed links:

Reference bandwidth: You can set the reference bandwidth on the router globally to provide granular link costs.

To adjust the reference bandwidth for a link, use the ospf auto-cost reference-bandwidth reference-bandwidth command that is configured in the OSPF routing process configuration mode.

Interface cost: You can choose to use arbitrary cost numbers on every interface.

To override the cost that is calculated for an interface for the OSPF routing process, use the ip ospf cost cost interface configuration command.

Interface bandwidth: You can configure the bandwidth kilobits-per-second command on an interface to override the default bandwidth.

{% hint style="info" %}
Whether you choose the reference bandwidth method, interface cost method, or interface bandwidth method for adjusting OSPF link costs, apply it consistently on every router. Inconsistent costs cause suboptimal path selection.

Prefer changing **OSPF cost** over changing **interface bandwidth**. Bandwidth changes can affect other protocols that use bandwidth (for example STP).
{% endhint %}

The OSPF cost is recomputed after every topology or interface bandwidth change, and Dijkstra’s algorithm determines the best path by adding all link costs along a path.

| ip ospf cost <>                  | interface without ip ospf cost configured wins over the one that has increased cost                                                                                                                                                                                                                                                                                                                                                                                     |
| -------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| auto-cost reference-bandwidth <> | since the default reference bandwidth is divided by 10^8, it returns the same value for 100Mbps+ links, which is 1 This command recalibrates the reference bandwidth so that 100 and 1000 and 10000 Mbps links can be differentiated with a proper cost It is a best practice to set the same reference bandwidth for all OSPF routers - adjust at least to 10Gbps auto-cost reference-bandwidth 100000 - to precisely differentiate between 100g 10g and 1g interfaces |
| (route-map)#set ospf-metric <>   | Configure OSPF metric value                                                                                                                                                                                                                                                                                                                                                                                                                                             |

For Intra-area routes, OSPF uses pure link-state routing - it calculates the topology from it's own perspective based on the LSA's

For Inter-area routes, uses distance vector routing - the ABR generates the Type 3 to other area describing the inter-area route (describing network behind it), which is a distance vector behavior

It selects the most suitable path depending on the transmission speed.

Interface with higher transmission speed have lower cost than the interface with lower transmission speed

The path with the lowest total path metric is installed in the OSPF RIB and presented to the global RIB

If there is a metric match, both routes are installed in the RIB, triggering ECMP load balancing

{% hint style="info" %}
Default load balancing is per-destination. Per-flow is usually not recommended. It adds overhead.
{% endhint %}

| maximum-paths <> | defines maximum allowed paths for one destination to be installed into RIB for ECMP; default is 4; 16 for IPv6 |
| ---------------- | -------------------------------------------------------------------------------------------------------------- |

It is important to remember that cost is not the primary influencer of the best path selection, especially in multiarea environments.

![](<../.gitbook/assets/Unknown image (140)>)

### Route types

Intra-Area (O) indicate a route learned from current area

Intra-area routes have the highest priority and are preferred over other types of routes

Inter-Area (O\*IA) indicate a route learned from a different area

Inter-area routes are preferred over external routes

External Type 1 (O\*E1) (Metric Type 1) external routes with the cost calculated as the sum of the cost within OSPF domain to the ASBR and the external cost of the route

E1 routes are preferred over E2 routes

{% hint style="info" %}
To avoid suboptimal routing when two ASBRs advertise the same external route, use **Type 1** metrics (E1/N1).
{% endhint %}

This is because both the shortest and "longest or worst" have the same cost to reach the external network

External Type 2 (O\*E2) (Metric Type 2) routes learned from external sources are advertised with a fixed metric value (this is the default type added to an external route with a metric of 20)

This metric type is used when you don't want the cost of the external routes to be affected by the internal OSPF link costs

{% hint style="info" %}
Metric type can be hardcoded in `redistribute`, or set via a route-map.
{% endhint %}

O\*N1: NSSA Type 1 indicates the redistributed external route from NSSA with a metric like external type 1 E1

O\*N2: NSSA Type 2 indicates the redistributed external route from NSSA with a metric like external type 2 E2

| (route-map)#set metric-type {type-1 \| type-2} | Configure OSPF metric type |
| ---------------------------------------------- | -------------------------- |

### Next-hop determination

When calculating routes, the next hop is not always the originating router (the one advertising the network).

Instead, the next hop is typically the first router along the shortest path (often the directly connected neighbor).

On P2P OSPF links, the routing table will always point to the direct neighbor on that link as the next hop.

Only if the originating router is directly connected to the calculating router (on the same link) will the next hop equal the originating router.

This ensures proper hop-by-hop forwarding across the OSPF domain.

Example:

![](<../.gitbook/assets/Unknown image (141)>)

R3 originates a route (say 10.0.3.0/24).

R1 installs the route, but the next hop in R1’s routing table is R2, not R3.

Reason: packets must traverse R2 first to reach R3.

R1#show ip route

O\*E2 0.0.0.0/0 \[110/120] via 10.88.0.129, 00:29:09, GigabitEthernet0/0

1.0.0.0/32 is subnetted, 1 subnets

O 1.1.1.1 \[110/3] via 10.88.0.129, 00:29:25, GigabitEthernet0/0

2.0.0.0/32 is subnetted, 1 subnets

O 2.2.2.2 \[110/2] via 10.88.0.129, 00:31:42, GigabitEthernet0/0

10.0.0.0/8 is variably subnetted, 7 subnets, 4 masks

O E2 10.1.24.32/29 \[110/1] via 10.88.0.129, 00:24:14, GigabitEthernet0/0

O 10.88.0.0/29 \[110/3] via 10.88.0.129, 00:29:09, GigabitEthernet0/0

O 10.88.0.32/29 \[110/2] via 10.88.0.129, 00:31:32, GigabitEthernet0/0

C 10.88.0.128/28 is directly connected, GigabitEthernet0/0

L 10.88.0.130/32 is directly connected, GigabitEthernet0/0

C 10.88.32.0/24 is directly connected, Loopback32

L 10.88.32.1/32 is directly connected, Loopback32

R1#show ip int bri

Interface IP-Address OK? Method Status Protocol

GigabitEthernet0/0 10.88.0.130 YES manual up up

GigabitEthernet0/1 unassigned YES NVRAM administratively down down

GigabitEthernet0/2 unassigned YES NVRAM administratively down down

GigabitEthernet0/3 unassigned YES NVRAM administratively down down

Loopback32 10.88.32.1 YES manual up up

R1#show ip route 10.88.0.1

Routing entry for 10.88.0.0/29

Known via "ospf 747", distance 110, metric 3, type intra area

Redistributing via bgp 65035

Advertised by bgp 65035 match internal external 1 & 2

Last update from 10.88.0.129 on GigabitEthernet0/0, 00:28:16 ago

Routing Descriptor Blocks:

* 10.88.0.129, from 1.1.1.1, 00:28:16 ago, via GigabitEthernet0/0

Route metric is 3, traffic share count is 1

R2#show ip route

O\*E2 0.0.0.0/0 \[110/120] via 10.88.0.37, 00:29:28, GigabitEthernet0/1

1.0.0.0/32 is subnetted, 1 subnets

O 1.1.1.1 \[110/2] via 10.88.0.37, 00:29:48, GigabitEthernet0/1

2.0.0.0/32 is subnetted, 1 subnets

C 2.2.2.2 is directly connected, Loopback0

10.0.0.0/8 is variably subnetted, 7 subnets, 3 masks

O E2 10.1.24.32/29 \[110/1] via 10.88.0.37, 00:24:37, GigabitEthernet0/1

O 10.88.0.0/29 \[110/2] via 10.88.0.37, 00:29:28, GigabitEthernet0/1

C 10.88.0.32/29 is directly connected, GigabitEthernet0/1

L 10.88.0.36/32 is directly connected, GigabitEthernet0/1

C 10.88.0.128/28 is directly connected, GigabitEthernet0/0

L 10.88.0.129/32 is directly connected, GigabitEthernet0/0

O 10.88.32.1/32 \[110/2] via 10.88.0.130, 00:32:06, GigabitEthernet0/0

R2# show ip int bri

Interface IP-Address OK? Method Status Protocol

GigabitEthernet0/0 10.88.0.129 YES manual up up

GigabitEthernet0/1 10.88.0.36 YES manual up up

GigabitEthernet0/2 unassigned YES unset administratively down down

GigabitEthernet0/3 unassigned YES unset administratively down down

Loopback0 2.2.2.2 YES manual up up

S\* 0.0.0.0/0 \[1/0] via 10.88.0.1

1.0.0.0/32 is subnetted, 1 subnets

C 1.1.1.1 is directly connected, Loopback0

2.0.0.0/32 is subnetted, 1 subnets

O 2.2.2.2 \[110/2] via 10.88.0.36, 00:30:31, GigabitEthernet0/1

10.0.0.0/8 is variably subnetted, 7 subnets, 3 masks

B 10.1.24.32/29 \[120/0] via 10.88.0.1, 00:25:17

C 10.88.0.0/29 is directly connected, GigabitEthernet0/0

L 10.88.0.2/32 is directly connected, GigabitEthernet0/0

C 10.88.0.32/29 is directly connected, GigabitEthernet0/1

L 10.88.0.37/32 is directly connected, GigabitEthernet0/1

O 10.88.0.128/28 \[110/2] via 10.88.0.36, 00:30:31, GigabitEthernet0/1

O 10.88.32.1/32 \[110/3] via 10.88.0.36, 00:30:31, GigabitEthernet0/1

R3# show ip int bri

Interface IP-Address OK? Method Status Protocol

GigabitEthernet0/0 10.88.0.2 YES manual up up

GigabitEthernet0/1 10.88.0.37 YES manual up up

GigabitEthernet0/2 unassigned YES unset administratively down down

GigabitEthernet0/3 unassigned YES unset administratively down down

Loopback0 1.1.1.1 YES manual up up

### OSPFv2 inter-area route summarization

Route summarization is a key to scalability in OSPF. Route summarization helps solve two major problems: large routing tables and frequent LSA flooding throughout the AS. Every time that a route disappears in one area, routers in other areas get involved in shortest-path calculation. To reduce the size of the area database, you can configure summarization on an area boundary or an AS boundary.

Normally, type 1 and type 2 LSAs are generated inside each area and translated into type 3 LSAs in other areas. With route summarization, the ABRs or ASBRs consolidate multiple routes into a single advertisement. ABRs summarize type 3 LSAs, and ASBRs summarize type 5 LSAs. Instead of advertising many specific prefixes, you need to advertise only one summary prefix.

If the OSPF design includes many ABRs or ASBRs, suboptimal routing is possible (one of the drawbacks of summarization).

Route summarization requires a good addressing plan with an assignment of subnets and addresses based on the OSPF area structure and that lends itself to aggregation at the OSPF area borders.

All routers within the same area must have synchronized LSDB; therefore, LSAs cannot be filtered within an area, but only between areas only on ABR

The summarization of internal routes can be done only by ABRs. Without summarization, all the prefixes from an area are passed into the backbone as type 3 interarea routes

When summarization is enabled, the ABR intercepts this process and, instead, injects a single type 3 LSA, which describes the summary route into the backbone. Multiple routes inside the area are summarized.

A summary route is generated if at least one subnet within the area falls in the summary address range. The summarized route metric is equal to the lowest cost of all the subnets within the summary address rang

ABR creates a route to Null0 to avoid loops in the absence of more specific routes. For example, if the summarizing router receives a packet from an unknown subnet that is part of the summarized range, the packet matches the summary route based on the longest match. The packet is forwarded to the Null0 interface (in other words, it is dropped), which prevents the router from forwarding the packet to a default route and possibly creating a routing loop.

Network numbers in areas should be assigned contiguously to ensure that these addresses can be summarized into a minimal number of summary addresses.

![](<../.gitbook/assets/Unknown image (142)>)

![](<../.gitbook/assets/Unknown image (143)>)

| area 0 range 192.168.1.0 255.255.0.0 \[advertise \| not-advertise] \[cost metric] | \[Advertise \| Not-Advertise] specifies whether the summarized route should be advertised into the OSPF routing domain or not                                                                                                                     |
| --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| area 1 filter-list prefix \<prefix\_list> {in \| out}                             | filter specific prefixes on ABR level from being advertised to other area                                                                                                                                                                         |
| distribute-list { ACL \| prefix-list-name \| route-map } in\|out                  | used on ASBR level to distribute prefixes from OSPF domain to other domains Restriction a distribute list should not be used for filtering of prefixes between areas because Type 3 LSA generation occurs before the distribute list is processed |

| ## Injecting an OSPF default route within normal areas Router(config-router)# default-information-originate \[always \| metric \| metric-type \| route-map] ## Injecting an OSPF default route within a NSSA Router(config-router)# area nssa default-information-originate \[metric \| metric-type \| route-map \| nssa-only] | default route will be injected as E2 route (Type 5 LSA) parameter always will originate defaul route even if there is no gateway of last restort (e.g it is not in ASBR's RIB) metric can be specified in case we want to prioritize default route of the primary ASBR The second would have increased metric |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

{% hint style="info" %}
Default routes originated by OSPF are injected as **E2** by default (Type 5 LSA).
{% endhint %}

{% hint style="info" %}
By default, an ABR advertises a default route with cost `1`. Change it using `default-cost` (IOS XR) or `area default-cost` (IOS/IOS XE).
{% endhint %}

This configuration prevents default route advertisements when prefixes in the prefix lists become unreachable and are subsequently withdrawn from the RIB

As a result, the backup router at the site with the default route can take over traffic and advertise its default route

![](<../.gitbook/assets/Unknown image (144)>)

![](<../.gitbook/assets/Unknown image (145)>)

### OSPF redistribution and external route summarization

Summarization of external routes is performed on ASBR level and can also be done by the ABR in NSSAs, where the ABR router creates type 5 summary routes from type 7 external routes

A summary route to Null0 is created automatically for each summary range.

| summary-address                                                                                                            | summary-prefix prefix on IOS-XR Use the not-advertise option to filter out the prefixes that are within the summary range.                                                                                                                                                                                                                                                                                                                                                                              |
| -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| redistribute \<connected \| static \| bgp \| eigrp \| ospf \| rip \<AS/PID>\[ metric <> \| metric-type \[1 \| 2] \[tag <>] | Redistributes either connected, static, or routing protocols specifying protocol and it's process id. default is all classless and classful networks are redistributed (old keyword subnets) Metric and metric type can be specified to add seed metric (default is 20 with E2 type). Tag can be specified as in other routing protocols when redistributing OSPF routes into BGP, by default only internal routes are redistributed without specifying the match keyword which include external routes |
| show ip ospf rib redistribution                                                                                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| default-metric                                                                                                             | sets the default metric for all redistributed routes                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

{% hint style="info" %}
Redistribution into OSPF injects routes as **external** (Type 5) by default.
{% endhint %}

### Virtual link (VL)

#### Discontiguous Area 0

If Area 0 is not designed to be contiguous, we can encounter issue shown below, where R2 (ABR) won't propagate (per ABR rules) Area 34 to Area 12, because it was learned from non-backbone area 23. Real world scenario is rather that link between routers in backbone fails, and causes area 0 to be partitioned. Workaround is to use GRE tunnel or Virtual link

![](<../.gitbook/assets/Unknown image (146)>)

![](<../.gitbook/assets/Unknown image (147)>)

Virtual link an extension to the OSPF backbone and allows a router to connect logically to the backbone, even though there is no direct physical link. Between the two routers that are involved in the creation of the virtual link, there is a nonbackbone area. The routers at each end become part of the backbone, and both act as ABRs.

In real world scenario is to connect two remote sites in area 0 over the public internet

It allows traffic to be routed between the two non-backbone areas without having to pass through the backbone area and may reduce traffic that passes thrugh backbone

The virtual link relies on intra-area routing, and its stability depends on the stability of the underlying area. Virtual links cannot run through more than one area or over stub areas; they can only run through normal nonbackbone areas. If a virtual link must be attached to the backbone across two nonbackbone areas, two virtual links are required—one virtual link to serve each area.

Virtual links are used for two purposes:

Linking an area that does not have a physical connection to the backbone.

Patching the backbone in case a discontinuity with Area 0 occurs.

The area through which you configure the virtual link is known as a transit.

Virtual links have the following characteristics and limitations:

They serve as an extension to the backbone area.

They are carried over a regular nonbackbone area.

They cannot traverse stubby or NSSA areas.

They also cannot traverse unnumbered links.

Virtual link cost equals the cost of the path to the destination

The OSPF database treats the virtual link as a direct link between ABRs. For greater stability, the loopback interface is used as a router ID and virtual links are created using these loopback addresses.

Before deciding which design approach to use, you should consider the listed guidelines and recommendations.

Do not use virtual links as primary OSPF design tool.

Solve intermittent issues by connecting non-contiguous backbone areas or remote areas.

A valid virtual link use case is where there is one physical link between two ABRs in the same area, and the link should be available to the backbone area and nonbackbone area.

| show ip ospf virtual-link |   |
| ------------------------- | - |

![](<../.gitbook/assets/Unknown image (148)>)

#### Capability transit

is a feature that allows an OSPF router to transit packets for a different OSPF domain

This is enabled by default and has the effect that R4 will route traffic directly to R10 instead of routing traffic back to R5 and then via VL

![](<../.gitbook/assets/Unknown image (149)>)

### OSPF fast convergence

OSPF convergence depends on several factors. Some factors relate to the environment, such as the size of the network, and other factors relate to OSPF design and default timers.

By default, OSPF can take more than 40 seconds to converge upon link or device failures. Several techniques can be used to improve convergence and stability of OSPF routing:

Tune OSPF timers (hello and dead intervals, SPF, LSA)

Cisco Nonstop Forwarding (NSF)

Cisco Nonstop Routing (NSR)

Bidirectional Forwarding Detection (BFD) for OSPF

The following techniques are available to improve the convergence times of OSPF in case of link or node failures:

Tune OSPF timers such as hello and dead intervals, SPF timers, LSA arrival, and flooding pacing, which is usually the first option when faster convergence is required. Make sure that neighbors use compatible timers to prevent flapping in the network.

Cisco NSF is a technique that enables modular routers to continue forwarding traffic in times of route processor failures and/or switchovers. This technique requires neighboring routers to support it.

Cisco NSR allows the traffic to be forwarded while a route processor switchover happens (or process restarts). This technique does not rely on neighbors to support it as well.

BFD protocol enables subsecond link or neighbor failure detection, thus triggering state change much sooner.

For moderate convergence improvements you can tune the OSPF hello and dead timers. By default, OSPF uses a 40-second dead interval, which means it can take up-to 40 seconds for a router to determine that the neighbor is no longer reachable.

There is also an alternative where you can set the dead interval to 1 second though it is recommended to use other means when requiring subsecond convergence (for example BFD).

#### Multi-area adjacency

By default, an interface can only belong to one OSPF Area. This can not only cause sub-optimal routing in the network, but it can also lead to other issues if the network is not designed correctly.

When Multi-Area Adjacency is configured on an interface, the OSPF speakers form more than one Adjacency (ADJ) over that link. The Multi-Area interface is a logical, point-to-point interface over which the ADJ is formed

The requirement is that there must be only two OSPF speakers on the link, and in a broadcast network, you must manually change the OSPF network type to Point-to-Point on the link.

interface Ethernet0/1

ip address 192.168.23.3 255.255.255.0

ip ospf network point-to-point

ip ospf multi-area 99

ip ospf 1 area 0 [http://cisco.com/c/en/us/support/docs/ip/open-shortest-path-first-ospf/118879-configure-ospf-00.html#:\~:text=When%20Multi-Area%20Adjacency%20is,which%20the%20ADJ%20is%20formed](http://cisco.com/c/en/us/support/docs/ip/open-shortest-path-first-ospf/118879-configure-ospf-00.html)

#### Loop-Free Alternate (LFA)

IP FRR (Fast reroute) allows to install an existing backup path in the FIB, reducing routing transition to less than 50ms (Only configurable within OSPFv2 (on IOS-XE)

Without FRR, OSPF has to re-run the SPF algorithm in the specific area

OSPF LFA Tie-Breaking Rules

SRLG (Shared Risk Link Groups): Don’t select a LFA of the same SRLG than the primary path.

Primary Path: Prefer a LFA that’s part of ECMP.

Interface Disjoint: Don’t select a LFA that uses the same outgoing interface.

Lowest Metric: Always select the LFA with the lowest metric.

Linecard Disjoint: Don’t select a LFA that uses an interface on the same linecard.

Node Protecting: Prefer a LFA that doesn’t pass through the same next-hop router.

Broadcast Interface Disjoint: Don’t select a LFA that passes through the same broadcast area as the primary path.

Downstream: Don’t select a LFA whose neighbor metric is higher than our own metric (comparable to EIGRP FC).

Secondary Path: Prefer a LFA that’s not part of EMCP.

Equation used for LFA computation is Inequality1: dist(N,D) < dist(S,D) + dist(N,S)

dist(X,Y): measures the path cost, or “distance”, between X and Y.

S: source router

E: primary next-hop router (not present in Inequality 1 but appears later)

N: candidate next-hop router

D: destination IP prefix

| Router(config-router)# fast-reroute per-prefix enable area prefix-priority \[high \| low] Router(config-router)# fast-reroute keep-all-paths | High: FRR is only calculated for /32 prefixes Low: FRR is calculated for all prefixes Keep-all-Paths: Used to install not only one but all available repair-paths to a destination show ip route repair-paths |
| -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Remote LFA Provides FRR for MPLS environments where if a LFA path from a direct neighbor is not available, traffic can be tunneled to a remote router that delivers the traffic to the destination. Function is based on up and running LDP and requires targeted LDP sessions to be allowed. Packets destined for the to-be-protected prefix will be sent to the PQ node via MPLS double-tagged packets. When the primary path fails, the IP packet is imposed with an extra label

| Router(config)# mpls ldp discovery targeted-hello accept Router(config-router)# fast-reroute per-prefix enable prefix-priority \[high \| low] Router(config-router)# fast-reroute per-prefix remote-lfa tunnel mpls-ldp | show ip ospf fast-reroute remote-lfa tunnels |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |

#### Prefix suppression

refers to the process of selectively omitting the prefix information related to transit links in LSA. This optimization technique aims to reduce unnecessary information in LSDB and SFP calculation and updates, specifically for LSAs of Type 1 (Router LSAs) and Type 2 (Network LSAs). By default, OSPF advertises all transit link LSAs to every router within the OSPF area. However, transmitting detailed information about transit links to all routers may be considered excessive.

| (config-router)# \[no] prefix-suppression (config-if)#ip ospf prefix-suppression \[disable] (config-if)# ospfv3 \[ipv4 \| ipv6] prefix-suppression \[disable] |   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

#### LSA and SPF throttling

provides a dynamic mechanism to slow down link-state advertisement updates in OSPFv3 during times of network instability.

It also allows faster OSPFv3 convergence by providing LSA rate limiting in milliseconds.

BFD should be used whenever available since it’s more lightweight, therefor less CPU-intensive and failure detection can be as low as 150ms

| Tuning LSA and SPF Timers for OSPFv3 Fast Convergence         |                                                                                                                          |
| ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| timers lsa arrival milliseconds                               | Sets the minimum interval at which the software accepts the same LSA from OSPFv3 neighbors.                              |
| timers pacing flood milliseconds                              | Configures LSA flood packet pacing.                                                                                      |
| timers pacing lsa-group seconds                               | Changes the interval at which OSPFv3 LSAs are collected into a group and refreshed, checksummed, or aged.                |
| timers pacing retransmission milliseconds                     | Configures LSA retransmission packet pacing in IPv4 OSPFv3.                                                              |
| timers throttle spf spf-start spf-hold spf-max-wait           | Turns on SPF throttling. Used to bundle several incoming LSAs together to run the SPF only once instead of several times |
| timers throttle lsa start-interval hold-interval max-interval | Sets rate-limiting values for OSPFv3 LSA generation.                                                                     |
| (config-if)# ip ospf dead-interval minimal hello-multiplier   | With OSPF fast hellos it’s possible to reduce the convergence time to under 1 second                                     |

%OSPF-4-FLOOD\_WAR This log message indicates issues with Type-2 LSAs when duplicate IP addresses are present in the network, or with Type-5 LSAs when there is a duplicate router ID in different OSPF Areas

#### GTSM (Generic TTL Security Mechanism)

Protects against OSPF spoofing attacks by discarding incoming packet that have a TTL below the configured threshold

| ## Enabling GTSM globally for all OSPFv2 interfaces Router(config)# router ospf Router(config-router)# ttl-security all-interfaces \[hops ] ## Disabling or Enabling GTSM for OSPFv2 on a per-interface basis Router(config)# interface Router(config-if)# ip ospf ttl-security \[disable] \[hops ] ## Enabling GTSM for a specific OSPFv2 virtual-link or sham-link Router(config)# router ospf Router(config-router)# area virtual-link \| sham-link ttl-security hops ## Enabling GTSM for a specific OSPFv3 virtual-link Router(config)# router ospfv3 Router(config-router)# address-family ipv6 Router(config-router-af)# area virtual-link ttl-security hops |   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

### Forwarding address (FA) in Type 5 LSAs

The Effects of the Forwarding Address (FA) on Type 5 LSA Path Selection

is a feature is designed to avoid extra hops when forwarding traffic to an external domain (field within the Type 5 LSA)

It specifies the next-hop ASBR's IP address for reaching the external destination, however there might be situations when this behavior is not desirable, so they have introduced the concept of FA in order to avoid extra hops in the path

FA field is set to zero (0.0.0.0) indicates that the originating ASBR is the next-hop router for reaching the external network specified in the LSA

FA field is set to a non-zero value (x.x.x.x) indicates that the originating ASBR is not the next-hop router for the external network, instead, it sets the next hop IP of the delegated ASBR

To have non-zero FA field, the next-hop interface must be a broadcast interface that is natively advertised in OSPF

The scenario explained in [https://www.cisco.com/c/en/us/support/docs/ip/open-shortest-path-first-ospf/25493-type5-lsa.html](https://www.cisco.com/c/en/us/support/docs/ip/open-shortest-path-first-ospf/25493-type5-lsa.html)

Forwarding address is chosen to be priority of Loopback's interface IP or IP of first interface on the OSPF interface list #show ip ospf int brief, where the interface on top will be the last interface which was attached to OSPF.

Changing the configuration of Et0/0 to default configuration will make it detach from OSPF. Adding the configuration again will attach it back to OSPF. After this Et0/0 will be listed on top of "show ip ospf interface brief" output.

This change can be unpredictable and would result in network convergence so it is advisable to have a loopback IP address as forwarding address as shown on picture 6

![](<../.gitbook/assets/Unknown image (150)>)

![](<../.gitbook/assets/Unknown image (151)>)

![](<../.gitbook/assets/Unknown image (152)>)

![](<../.gitbook/assets/Unknown image (153)>)

![](<../.gitbook/assets/Unknown image (154)>)

It is advisable to have a loopback IP address as forwarding address

| R3(config)#int lo0 R3(config-if)#ip address 192.168.3.3 255.255.255.255 R3(config-if)#router ospf 1 R3(config-router)#network 192.168.3.3 0.0.0.0 area 1 |   |
| -------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

![](<../.gitbook/assets/Unknown image (155)>)
