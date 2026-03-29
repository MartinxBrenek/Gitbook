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

# ISIS

### Overview

Intermediate System to Intermediate System (IS-IS) is an IGP link-state protocol similar to OSPF used primarily in the service provider networks to it's better flexibility and less overhead in compare to OSPF, which is commonly used in enterprise networks, but IS-IS is also the recommended underlay protocol for the SD-Fabric within enterprise

IS-IS also forms neighbor adjacencies, exchanges link-state packets, builds an LSDB and runs the Dijkstra SPF algorithm to find the best path to each destination, which is installed in the routing table. It has the same path selection method - for intra-area uses link-state routing and for inter-area it uses distance vector routing

### History

Back when the OSI model protocols were used over TCP/IP protocols, the networks run on OSI model network layer protocols developed by ISO - CLNP/CLNS and CMNS/CON, which was something similar to the IP and UDP in TCP/IP.

IS-IS (intermediate system e.g. router to router) served as the routing protocol that distributed the CLNP/CLNS routing information

The protocol ES-IS (End-system to Intermediate-System e.g host to router) ensured ARP, ICMP and DHCP functionalities at once.

This was called Level 0 routing and Level 1-3 routing was to represent routing between networks

The IS-IS has Level 1 routing describing intra-area routing and the Level 2 routing describing inter-area routing

![](<../.gitbook/assets/Unknown image (649)>)

### OSI Suite

CLNP (Connectionless-mode Network Protocol) protocol similar to IP in TCP/IP, carrying upper-layer data and error indications over connectionless links

Connectionless Network Service (CLNS) provides network layer services to the transport layer through CLNP. CLNS provides best-effort delivery, which means that no guarantee exists that data will not be lost, corrupted, misordered, or duplicated. CLNS relies on transport layer protocols to perform error detection and correction

CONP(Connection-Oriented Network Protocol) it is a network layer service that acts as the interface between the transport layer and CMNS

Connection-Mode Network Service (CMNS) include connection setup, maintenance, and termination (protocol X.25)

![](<../.gitbook/assets/Unknown image (650)>)

### Integrated IS-IS

An improved version that support IPv4,IPv6, CLNP routing and also MPLS TE

Link-state protocol scaling limitation is primarily related to the number of nodes (routers). The Dijkstra algorithm, used by OSPF and IS-IS, has computational complexity proportional to the square of the number of nodes (routers). Large networks can have thousands of nodes making it increasingly difficult to expand further. Additionally, every router has to hold a large LSDB containing details of the entire network.

Levels and areas are used to improve the scalability of IS-IS. Each level has a smaller number of nodes so that the Dijkstra algorithm no longer requires excessive amounts of CPU power to calculate the SPT from a meshed topology.

IS-IS same as OSPF can be deployed as a single area or Multiarea with Hierarchical design, but in ISIS the other areas do not have to connect to a common backbone area

This is what increases the flexibility and scalability for service providers

The backbone is not one specific area, but a contiguous collection of Level-2 routers each of which might be in a different area

Each router holds the topology information for its own area only.

**Flat design**: Every router is in the same area and has the same Level. No summarization. Best practise is to configure interface for both L1 and L2 to further expand to hierarchical if needed

**Hierarchical design**: Dedicated backbone (Level 2) area. L1-L2 routers at the edge of every area. Summarization at the edge of every area.

**Hybrid design**: No dedicated backbone (Level 2) area but multiple areas connected through L1-L2 routers. Summarization at the edge of every area

![](<../.gitbook/assets/Unknown image (651)>)

### IS-IS vs. OSPF comparison

OSPF assigns areas per interface, allowing a single router to participate in multiple areas at the same time.

IS-IS assigns the area to the router itself, defined by the Area Address in the NET; all interfaces on the router belong to that same IS-IS area.

In IS-IS, hierarchy is not created by placing interfaces into different areas, but by controlling Level-1 and Level-2 adjacencies on a per-interface basis, which permits for more flexible approach to extending the backbone

IS-IS Level-1 and Level-2 are not equivalent to OSPF areas; they represent intra-area and inter-area routing roles within a single area assignment

The backbone is extended by simply adding more Level 2 and Level 1-2 routers, which is a less complex process than with OSPF.

OSPF floods multiple, function-specific LSAs, each describing a portion of the topology or routing information, whereas IS-IS groups all routing information generated by a router into one or more LSPs owned by that router.

As network size increases, OSPF tends to generate a higher number of control packets, while IS-IS typically floods fewer, larger packets, reducing flooding overhead.

Because of this LSP-based model and its simpler flooding behavior, IS-IS generally scales to very large areas (hundreds to thousands of routers) more predictably than OSPF.

OSPF runs directly over IP, while IS-IS runs directly over Layer-2 using CLNS, making IS-IS independent of IP and allowing routing adjacencies to form even before IP is fully operational.

In practice, IS-IS is often considered more operationally efficient in large topologies, due to simpler LSP processing, fewer flooding events, and less protocol state complexity.

### Network Service Access Point (NSAP)

In OSI 20-byte L3 address is called NSAP, which is a single address that identifies entire node and not interface e.g it uniquely represents the router within an area, regardless of how many interfaces it has. This means that an IS-IS router, along with all its interfaces, can only belong to a single area, with boundaries defined at the links.

In contrast, OSPF allows an ABR to be associated with multiple areas, establishing boundaries at the router level

Because IS-IS was originally designed for CLNS, IS-IS requires CLNS addresses - NSAP, even if the router is only used for routing IP, they run separately and don't interfere with each other

The OSI protocols (hello PDUs) are used to form the neighbor relationship between routers, and the SPF calculations rely on a configured NET address to identify the routers.

#### NET (Network Entity Title)

NET (Network Entity Title) is an NSAP address with NSEL set to 00, used in IS-IS to uniquely identify a router, like 49.0001.0000.0000.0001.00

It consists of the AFI (e.g., 49), Area ID (e.g., 0001), System ID (e.g., 0000.0000.0001), and NSEL (00), defining the router’s identity in the IS-IS domain.

#### NSAP format

49.0001.0000.0000.0001.00

**49 (AFI)**: specifies the format of the address and the authority that is assigned to that address. 49 is analugous for RFC 1918

**0001 (Area ID)**: all L1 routers in the same area have the same Area ID

**0000.0000.0001 (System ID)**: Router ID (RID) - can be just simple numerical representation, IP address of loopback or one of the MAC address that belongs to the router

**00 (NSEL)**: (NSAP selector) identifies the service, where 00 identifies a router

![](<../.gitbook/assets/Unknown image (652)>)

### Router types (L1 / L2)

#### Level 1 router

is an intra-area router that know topology only of its own area and exchanges information with other Level-1 routers

#### Level 1-2 router

Configures the router to establish an adjacency and exchange information with both a Level 1 and Level 2 routers in a different area. (This is the default in IOS)

It has two link databases a Level 1 LSDB for intra-area routing and Level 2 LSDB for interarea routing

{% hint style="info" %}
By default, an L1/L2 router propagates prefixes from Level 1 into Level 2, but not in reverse. A default route is sent from L1/L2 routers to L1 routers by default.
{% endhint %}

However, if it is required to move prefixes from L2 Area to L1 Area, a redistribute/propagate command under IS-IS configuration is required.

This is the most significant difference to OSPF, where ABR automatically advertise networks via LSA Type 3 to other areas

#### Level 2 router

Inter-area only router exchanges information with other Level-2 routers

Level1-2 and Level-2 routers have the visibility of everything in the entire domain.

{% hint style="info" %}
Level 1 is similar to an internal OSPF router. Level 1/2 is similar to an OSPF ABR. Level 2 is similar to an OSPF backbone router.
{% endhint %}

![](<../.gitbook/assets/Unknown image (653)>)

### Configuration

{% hint style="info" %}
Be particularly careful with IP addressing. IS-IS troubleshooting is harder when IP is misconfigured.
{% endhint %}

The IS-IS neighbor relationships are established over OSI CLNS, not over IP.

Because of this approach, two ends of a CLNS adjacency can have IP addresses on different subnets, with no impact to the operation of IS-IS.

{% hint style="info" %}
On IOS XR, you must configure the AFI under the interface. Otherwise IS-IS will not run on that interface.
{% endhint %}

![](<../.gitbook/assets/Unknown image (654)>)

![](<../.gitbook/assets/Unknown image (655)>)

| (config-router)# summary-address \[level { 1 \| 2 }]              | This command also reduces the size of the link-state packets (LSPs) and thus the link-state database. It also helps ensure stability, because a summary advertisement depends on many more specific routes. If one more-specific route flaps, in most cases, this flap does not cause a flap of the summary advertisement. The meaning of summary options: level-1: Only routes redistributed into Level 1 are summarized with the configured address and mask value. level-1-2: Summary routes are applied when redistributing routes into Level 1 and Level 2 IS-IS, and when Level 2 IS-IS advertises Level 1 routes as reachable in its area. level-2: Routes learned by Level 1 routing are summarized into the Level 2 backbone with the configured address and mask value. Redistributed routes into Level 2 IS-IS will be summarized also. |
| ----------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-router)# summary-prefix prefix/length \[level { 1 \| 2 }] | The Cisco IOS XR command to create an aggregate addresses for the IS-IS protocol.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| maximum-paths                                                     | Sets the maximum number of paths allowed for a route learned via IS-IS IPv6. The default number is four.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| default-information originate                                     | Configures origination of the default route                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| show isis \[neighbors \| topology \| protocol \| database]        | to view connection to certain neighbor use the LSPID (e.g hostname) detail - found in the first column of the isis database                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Router(config-router)# set-overload-bit                           | OVERLOAD-bit Function-wise comparable to the stub-router feature of OSPF (max-metric router-lsa) Originally used to signal other routers of system resource exhaustion (CPU and/or memory) so that the given router isn’t used for transit anymore Can be set manually forever or temporarily for a given amount of time after startup                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

![](<../.gitbook/assets/Unknown image (656)>)

Quick config for lab purposes

router isis 1

net 49.0001.0100.0100.0002.00

address-family ipv4 unicast

metric-style wide level 2

segment-routing mpls sr-prefer

!

interface Loopback1

address-family ipv4 unicast

prefix-sid index 6 php-disable

!

!

interface xxx

point-to-point

hello-padding disable

address-family ipv4 unicast

!

!

!

router isis 1

net 49.0001.0100.0100.0001.00

address-family ipv4 unicast

metric-style wide level 2

redistribute ospf 1

segment-routing mpls sr-prefer

!

interface Bundle-Ether2

point-to-point

address-family ipv4 unicast

!

!

interface Loopback1

address-family ipv4 unicast

!

!

interface xxx

point-to-point

hello-padding disable

address-family ipv4 unicast

!

!

!

### IS-IS packet format

IS-IS PDUs are encapsulated directly into an OSI data-link frame

It’s not encapsulated in an IP packet like other routing protocols and is not depended on CLNP either

The IS-IS interface MTU is 1497 bytes since there is a 3 byte LLC (Logical-Link Control) field in the header

![IS-IS PDU Addressing Ethernet](<../.gitbook/assets/Unknown image (657)>)

### Packet types

The first eight octets of all IS-IS PDUs are header fields that are common to all PDU types. The TLV information is stored at the very end of the PDU.

Different types of PDUs have a set of currently defined TLV codes. Any TLV codes that are not recognized by a router should be ignored and passed through unchanged.

**Hello PDU:** This type of PDU is used to establish and maintain adjacencies.

The default hello interval is every 10 seconds; however, the hello interval timer is adjustable. Dead time is three times of Hello timer: so default is 30sec

**End system hello (ESH):** Announces the presence of an end system. ESHs are sent by all end systems. Intermediate systems (routers) listen for these hellos to discover the end systems.

**Intermediate system hello (ISH):** Announces the presence of an intermediate system (router). ISHs are sent by all intermediate systems. End systems listen for these hellos to discover the intermediate systems.

**IS-IS Hello (IIH):** Enables the intermediate systems to detect IS-IS neighbors and form adjacencies.

The IS-IS by default pads the Hello packets to the full interface Maximum Transmission Unit (MTU). This is in order to detect MTU mismatches. The MTU on either side of the link should match.

The padding can also be used in order to detect the real MTU value of the technology that lies beneath. For example, for Layer 2 (L2) transport over Multi Protocol Label Switching (MPLS) scenarios, the MTU of the transport technology might be much lower than the MTU on the edge. For example, the MTU can be 9,000 bytes on the edge, while the MPLS transport technology has an MTU of 1,500 bytes.

If the MTU values match on either side, then the padding can be disabled. As such, unnecessary usage of bandwidth and buffers by IS-IS Hello packets can be avoided. The router command that is used in order to disable the Hello padding is no hello padding \[multi-point|point-to-point]. The interface command that is used in order to disable the Hello padding is no isis hello padding.

If the padding is disabled at the start, the router still sends Hello packets at full MTU. In order to avoid this, disable the padding with the interface command and use the always keyword. In this case, all of the IS-IS Hello packets are not padded.

{% hint style="info" %}
Cisco recommends not disabling IS-IS Hello padding. It helps detect MTU mismatches and prevents broken adjacencies.
{% endhint %}

[https://www.cisco.com/c/en/us/support/docs/ip/integrated-intermediate-system-to-intermediate-system-is-is/119399-technote-isis-00.html](https://www.cisco.com/c/en/us/support/docs/ip/integrated-intermediate-system-to-intermediate-system-is-is/119399-technote-isis-00.html)

Link State Packets (LSPs) contains all IP information of connected links/prefixes.

Routers in an IS-IS network flood LSPs to all their neighbors. All valid LSPs received by a router are stored in LSDB, and they describe the topology of an area.

Routers use this LSDB to calculate the shortest-path tree.

IS-IS uses a two-level area hierarchy. The link-state information for these two levels is distributed separately, which results in Level 1 LSPs and Level 2 LSPs

Intermediate systems (ISs) are considered as nodes and the IP information is advertised by the ISs as the leaves hanging off the nodes in the shortest path tree.

The separation of IP reachability from the core IS-IS network architecture provides Integrated IS-IS better scalability than, for example, OSPF. e.g IP information does not influence the calculation of the SPF tree.

OSPF sends LSAs for individual IP subnets. If an IP subnet fails, the LSA floods through the network and all routers must run a full SPF calculation, which is extremely CPU-intensive.

Integrated IS-IS builds the SPF tree from CLNS information. If an IP subnet fails, the IS-IS LSP floods through the network, which is the same with OSPF.

However, if this is a leaf (stub) IP subnet (that is, if the loss of the subnet does not affect the underlying CLNS architecture), the SPF tree is unaffected; therefore, only a Partial Route Calculations (PRC) in the IP routing table for that particular subnet occurs.

The PRC generates best-path choices for IP routes and offers the routes to the IP routing table, where they are accepted, based on normal IP routing table rules

Each LSP includes specific information about networks and stations that are attached to a router. This information is found in multiple TLV fields that follow the common header of the LSP

The TLV structure is a flexible way to add data to the LSP, and an easy mechanism for adding new data fields that may be required in the future.

An LSP header includes these elements:

The PDU type and length

The LSP ID

The LSP sequence number, used to identify duplicate LSPs and to ensure that the latest LSP information is stored in the topology table.

The remaining lifetime for the LSP, which is used to age out LSPs

Some examples of TLV fields include the following:

Type Code 1 = area addresses

Type Codes 2 and 6 = intermediate system neighbors

Type Code 3 = end system neighbors

Type Code 10 = authentication information

Type Code 128 = IP internal reachability information

Type Code 129 = protocols supported

Type Code 130 = IP external reachability information

Type Code 132 = IP interface addresses

![](<../.gitbook/assets/Unknown image (658)>)

#### PSNP and CSNP

**Partial sequence number PDU (PSNP)**: messages used to acknowledge and request missing pieces of link-state information.

**Complete sequence number PDU (CSNP)**: messages used to describe the complete list of LSPs in the LSDB of a router

Separate CSNPs and PSNPs are used for Level 1 and Level 2 adjacencies. Adjacent IS-IS routers exchange CSNPs to compare their LSDB.

In broadcast subnetworks, only the DIS transmits CSNPs. All adjacent neighbors compare the LSP summaries received in the CSNP with the contents of their local LSDBs to determine if their LSDBs are synchronized

CSNPs are sent periodically by multicast (every 10 seconds) by the DIS on a LAN to ensure LSDB accuracy. If there are too many LSPs to include in one CSNP, the LSPs are sent in ranges.

The CSNP header indicates the starting and ending LSP ID in the range. If all LSPs fit in the CSNP, the range is set to default values.

In the example, R1 compares this list of LSPs with its topology table and realizes that it is missing one LSP.

Therefore, it sends a PSNP to the DIS (R2) to request the missing LSP. The DIS reissues only that missing LSP (LSP 77), and R1 acknowledges it with a PSNP.

![](<../.gitbook/assets/Unknown image (659)>)

### LSDB and LSP aging

Routers in an IS-IS network flood LSPs to all their neighbors.

All valid LSPs received by a router are stored in LSDB, and they describe the topology of an area. Routers use this LSDB to calculate the shortest-path tree.

If a router reloads, the sequence number is set to 1. The router then receives its previous LSPs from its neighbors.

These LSPs have the last valid sequence number before the router reloaded. The router records this number and reissues its own LSPs with the next higher sequence number.

Each LSP has a remaining lifetime that is used by the LSP aging process to ensure the removal of outdated and invalid LSPs from the topology table after a suitable time.

This process is known as the count-to-zero operation; 1200 seconds is the default start value.

Single-level routers maintain a single LSDB and one SPF calculation takes place, while Level 1 and Level 2 routers maintain two LSDBs and two SPF calculations take place, which poses larger router resource requirements

Each intermediate system originates its own LSP (one for Level 1 and one for Level 2). These LSPs are identified by the system ID of the originator and an LSP fragment number starting at 0.

If an LSP exceeds the maximum transmission unit (MTU), it is fragmented into several LSPs, numbered 1, 2, 3, and so on.

IS-IS maintains the Level 1 and Level 2 LSPs in separate LSDBs.

When an intermediate system receives an LSP, it examines the checksum and discards any invalid LSPs, flooding them with an expired lifetime age

If the LSP is valid and newer than what is currently in the LSDB, it is retained, acknowledged, and given a lifetime of 1200 seconds.

The age is decremented every second until it reaches 0, at which point the LSP is considered to have expired.

When the LSP has expired, it is kept for an additional 60 seconds before it is flooded as an expired LSP.

![](<../.gitbook/assets/Unknown image (660)>)

Routers insert the best paths in the CLNS routing table; the OSI forwarding database (Step 3 in the figure)

![](<../.gitbook/assets/Unknown image (661)>)

### Adjacency

To establish an adjacency, following parameters has to match: Network Type, Area ID, MTU, Authentication, IPv4 subnet and IS-Type. The System ID must be unique

**Down (2):** Adjacency not established. No Hello have been received from the neighbor.

**Initializing (1):** Hello packet received from neighbor but it’s not clear yet if the neighbor received our Hello

**Up (0):** Adjacency established. Bi-directional communication is working.

Example

The routers from one area accept Level 1 IIH PDUs only from their own area and therefore establish Level 1 adjacencies only with their own area Level 1 routers.

The routers from a second area similarly accept Level 1 IIH PDUs only from their own area.

The Level 2 routers (or the Level 2 process within any Level 1-2 router) accept only Level 2 IIH PDUs and establish only Level 2 adjacencies.

On point-to-point links, the IIH PDUs are common to both levels but announce the level type and the area address in the hellos as follows:

Level 1 routers in the same area exchange IIH PDUs that specify Level 1 and establish a Level 1 adjacency.

Level 2 routers exchange IIH PDUs that specify Level 2 and establish a Level 2 adjacency.

Two Level 1-2 routers in the same area establish both Level 1 and Level 2 adjacencies and maintain these with a common IIH PDU that specifies the Level 1 and Level 2 information.

Two Level 1 routers that are physically connected, but that are not in the same area, can exchange IIHs, but they do not establish adjacency because the area addresses do not match.

![](<../.gitbook/assets/Unknown image (662)>)

### Authentication

Unlike EIGRP/OSPF, the authentication process within IS-IS is divided into two parts:

**Auth. of HELLO packets:** Each HELLO packet will be authenticated. If not successful, no adjacencies are established. Can be enabled for either L1, L2 or both (default is both).

**Auth. of LSP packets:** Each LSP packet will be authenticated. If not successful, information about networks attached to other routers isn’t received.

Can be configured per area (Level-1) and per routing-domain (Level-2). Level can’t be specified.

Authentication configuration process for IOS/IOS-XE:

Auth. of HELLO packets: Configured under the interface.

Auth. of LSP packets: Configured under the routing process.

| Router(config-if)# isis authentication mode Router(config-if)# isis authentication key-chain                                                                           | ## IS-IS IIH authentication define key chain aswell key chain AUTH\_ISIS key 0 key-string C1sc0! |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| Router(config-router)# area-password Router(config-router)# authentication mode Router(config-router)# authentication key-chain Router(config-router)# domain-password | ## IS-IS LSP area authentication (level-1)                                                       |

### Network types

**Point-to-point:** Only two IS nodes can exist on the link, where no DIS election occur

A single adjacency is formed over the circuit for both Levels and a single P2P HELLO as unicast is sent over the circuit for both Levels

**Broadcast:** Used for LAN or multipoint WAN interfaces (default for Ethernet interfaces)

Designated Intermediate System (DIS) is elected PER LEVEL, which is similar to the DR in OSPF, however there is no backup DIS

Dijkstra’s algorithm requires a virtual router (a pseudonode), represented by the DIS, to build a directed graph for broadcast media.

The DIS is the router that creates the pseudonode and acts on behalf of the pseudonode.

Two major tasks that are performed by the DIS include creating and updating the pseudonode LSP and flooding LSPs over the LAN.

Independent adjacencies are formed between ALL routers per Level and separate IIHs are sent per Level as a multicast.

On a LAN, separate Level 1 and Level 2 IIHs are sent periodically as multicasts to a multicast MAC address. Level 1 announcements are sent to the AllL1IS multicast MAC address 0180.C200.0014, and Level 2 announcements are sent to the AllL2IS multicast MAC address 0180.C200.0015

The router with the highest priority (the priority value is configurable) is elected as DIS

IS-IS has no concept of nonbroadcast multiaccess (NBMA) networks. Using point-to-point links, such as point-to-point subinterfaces, over NBMA networks is recommended

Cisco router interfaces have a default Level 1 and Level 2 priority of 64. You can configure the priority from 0 to 127.

A selected router is not guaranteed to remain the DIS. Any adjacent intermediate system with a higher priority automatically takes over the DIS role.

This behavior is called preemptive. Because the IS-IS LSDB is synchronized frequently on a LAN, giving priority to another intermediate system over the DIS is not a significant issue

![](<../.gitbook/assets/Unknown image (663)>)

### Route types

i L1 = IS-IS Level 1 - in ipv6 it's I1

i L2 = IS-IS Level 2 - in ipv6 it's I2

i ia = IS-IS inter-area - in ipv6 it's IA

i su = IS-IS summary - in ipv6 it's IS

Level-1 Router Default Route (ATTACH-Bit): Set by the L1-L2 router so that the L1 router will install a default route. Will be only set if the L1-L2 router is attached to another area.

Level-1 routes will be automatically propagated into Level-2 areas by L1-L2-routers

| Router(config-router)# redistribute isis ip level-2 into level-1 route-map \[ROUTE-MAP] | redistribution |
| --------------------------------------------------------------------------------------- | -------------- |

### IS-IS path selection

Level: The originating level has the highest preference.

Level 1 route has the highest preference.

Level 2 route is an alternative

For paths from the same level:

Internal route has the highest preference

External route is an alternative

For routes from the same level and origin type, IS-IS will prefer the lowest metric.

Use wide metric on all routers in the autonomous system.

Change the default metric (10) to a high value to prevent new unconfigured slow interfaces from attracting traffic.

Design a metric plan as there is no automatic metric calculation in IS-IS.

You can borrow the OSPF formula to linearly map the link speeds to IS-IS metric.

Metric

regardless of the bandwidth, all links in IS-IS have the same metric and thus the route through the fewest links/hops is chosen to forward the traffic

So the IS-IS metric becomes similar to the hop count metric that is used by the distance vector protocols

Default IS-IS cost for all interfaces is 10

IS-IS metric is similar to OSPF cost. The main difference is that IS-IS does not have a mechanism to automatically assign a link metric based on link bandwidth.

IS-IS originally only had a metric range from 1 to 63, which is insufficient for the large range of links used in modern networks. Enable wide metric to enable a configurable range between 1 and 16777214.

You should also configure a high default metric to prevent new slow interfaces from accidentally attracting traffic.

Narrow Metric: This Default metric type in IS-IS allowing 6-bit link metric and 10-bit path metric - not suitable for high speed networks

Wide Metric allowing 24-bit link metric and 32-bit path metric. Used for used for large, high-speed service provider networks to transport additional attributes (e.g for MPLS-TE,..)

{% hint style="info" %}
Narrow and wide metrics are not compatible. Use “transition mode” to advertise both types during migrations.
{% endhint %}

![](<../.gitbook/assets/Unknown image (664)>)

Within route-map

| (route-map)#set metric-type {external \| internal}      | Configure IS-IS metric type                    |
| ------------------------------------------------------- | ---------------------------------------------- |
| (route-map)#set isis-metric value                       | Configure IS-IS metric value                   |
| (route-map)#set level {level-1 \| level-2 \| level-1-2} | Configure IS-IS level for redistributed routes |

### Route leaking

In general, route propagation from Level 1 to Level 2 is automatic. You might want to use the command shown in the figure below to better control which Level 1 routes can be propagated into Level 2.

Propagating Level 2 routes into Level 1 is called route leaking. Route leaking is disabled by default. That is, Level 2 routes are not automatically included in Level 1 link-state protocol data units. If you want to leak Level 2 routes into Level 1, you must enable that behavior

Routes can be leaked from Level-2 into Level-1 under the routing process

#### Loop prevention with the up/down bit

To implement route leaking, an up/down bit in the TLV is used to indicate whether the route that is identified in the TLV has been leaked

If the up/down bit is set to 0, the route originated within that Level 1 area and is eligible for leaking

If the up/down bit is set to 1, the route has been redistributed into the area from Level 2

Level 1-2 router does not re-advertise (into Level 2) any Level 1 routes that have the up/down bit set to 1

![](<../.gitbook/assets/Unknown image (665)>)

![](<../.gitbook/assets/Unknown image (666)>)

### Prefix suppression

In addition to using multiple levels, you may use prefix suppression to limit the size of the IS-IS database—only advertise edge networks and BGP next-hop addresses. Prefix suppression also reduces IS-IS convergence time. Make sure you distribute all next-hop addresses. If you filter out next-hop addresses, you may break routing and MPLS LSPs.

The easiest way to reduce the size of the LSDB is to exclude all transport links from IS-IS. If all loopback and edge interfaces are marked as passive, then only passive interfaces can be propagated via IS-IS.

![](<../.gitbook/assets/Unknown image (667)>)

There are two ways to configure prefix suppression:

On per-interface basis

On per-router basis

![](<../.gitbook/assets/Unknown image (668)>)

### IS-IS fast convergence

Some of the suggested IS-IS convergence design approaches:

Use BFD to enable subsecond convergence of IS-IS.

Prefer using BFD over IS-IS fast hellos.

Use carrier delay to minimize the time to process a link failure.

Use point-to-point IS-IS mode on core links to prevent unnecessary Designated Intermediate System (DIS) election.

Tune timers to improve processing and flooding of LSAs.

IS-IS throttles the following events:

SPF computation

Partial route calculation (PRC) computation

LSP generation

Throttling slows down convergence and not throttling can cause melt-downs, so you have to find a balance to improve convergence while ensuring stability. The scope is to react fast to the first events but, under constant churn, slow down to avoid collapse. This exponential backoff timer dynamically controls the time between the receipt of a trigger and the processing of the related action. In stable periods (rare triggers), the actions are processed promptly. As the stability decreases (trigger frequency increases), the mechanism delays the processing of the related actions.

Carrier Delay

If a link goes down and comes back before the carrier delay timer expires, the down state is effectively filtered, and the rest of the software on the router is not aware that a link-down event has occurred. Therefore, a large carrier delay timer results in fewer link-up/link-down events being detected. However, setting the carrier delay time to 0 means that every link-up/link-down event is detected.

In most environments a lower carrier delay is better than a higher one. The exact value that you choose depends on the nature of the link outages that you expect in your network and how long you expect those outages to last.

If data links in your network are subject to short outages, especially if those outages last less than the time required for your IP routing to converge, you should set a relatively long carrier delay value to prevent these short outages from causing disruptions in your routing tables. If outages in your network tend to be longer, you might want to set a shorter carrier delay so that the outages are detected sooner and the IP route convergence begins and ends sooner.

![](<../.gitbook/assets/Unknown image (669)>)

### Integrated IS-IS for IPv6

IS-IS is a multiprotocol routing protocol so it can accommodate IPv6 in addition to IPv4.

IS-IS multi-topology support for IPv6 allows IS-IS to maintain a set of independent topologies within a single area or domain. This mode removes the restriction that all interfaces on which IS-IS is configured must support the identical set of network address families. It also removes the restriction that all routers in the IS-IS area (for Level 1 routing) or domain (for Level 2 routing) must support the identical set of network layer address families. Multiple SPFs are performed, one for each configured topology. Therefore, connectivity existing among a subset of the routers in the area or domain is sufficient for a given network address family to be routable.

Two TLVs are added in IS-IS for IPv6 support. These two TLVs are used to describe IPv6 reachability and IPv6 interface addresses:

IPv6 reachability TLV (0xEC or 236)

Describes network reachability (routing prefix, metric, options)

Equivalent to IPv4 internal and external reachability TLVs (type code 128 and 130)

IPv6 interface address TLV (0xE8 or 232)

Equivalent to IPv4 interface address TLV (type code 132)

For hello PDUs, which must contain the link-local address

For LSPs, which must only contain the non-link-local address

Protocols-supported TLV (type code 129) lists the supported Network Layer Protocol Identifiers (NLPIDs). All IPv6-enabled IS-IS routers advertise an NLPID value of 0x8E (142). The NLPID of IPv4 is 0xCC (204).

Single-topology IS-IS setup where one SPF instance is maintained for both IPv4 and IPv6, the same links must carry IPv4 and IPv6 simultaneously.

In single-topology IPv6 mode, the configured metric is always the same for both IPv4 and IPv6. The reason for this is that IS-IS establishes routing adjacencies and builds the network topology using CLNS. IPv4 and IPv6 are just routed protocols; for routing information exchange, CLNS is used.

#### Multitopology IS-IS

provides some flexibility when you are transitioning to IPv6.

A separate topology is kept for both IPv4 and IPv6 networks; because some links may not be able to carry IPv6, IS-IS specifically keeps track of those links, minimizing the possibility for the traffic to be "black-holed." Multiple SPFs are performed, one for each configured topology e.g. AFI

All routers in the area or domain must use the same type of IPv6 support, either single topology or multitopology. A router operating in multitopology mode will not recognize the ability of the single-topology mode router to support IPv6 traffic, which will lead to routing holes in the IPv6 topology. To transition from single-topology support to the more flexible multitopology support, a multitopology transition mode is provided.

#### Migration: IPv4-only to multitopology

When you migrate from a purely IPv4 environment to a dual-stack environment, a discrepancy in supported protocols would cause adjacencies to fail.

The intermediate system performs consistency checks on hello packets and will reject hello packets that do not have the same set of configured address families

For example, a router running IS-IS for both IPv4 and IPv6 will not form an adjacency with a router running IS-IS for IPv4 or IPv6 only.

To facilitate a seamless upgrade, the engineer may disable consistency checks during the upgrade to maintain adjacencies active even in a heterogeneous environment.

Suppressing adjacency checking on intra-area links (Layer 1 links) is primarily done during a transition from single-topology (IPv4) to multitopology (IPv4 and IPv6) IS-IS networks.

Imagine that a service provider is integrating IPv6 into a network and it is not practical to shut down the entire provider router set for a coordinated upgrade.

Without disabling adjacency checking—because routers were enabled for IPv6 and IS-IS for IPv6—adjacencies would drop with IPv4-only routers, and IPv4 routing would be severely impacted. With consistency check suppression, IPv6 can be turned up without impacting IPv4 reachability.

#### Multitopology transition mode

allows a network operating in single-topology IS-IS IPv6 support mode to continue to work while upgrading routers to include multitopology IS-IS IPv6 support. While in transition mode, both types of TLVs (single topology and multitopology) are sent in LSPs for all configured IPv6 addresses, but the router continues to operate in single-topology mode.

After all routers in the area or domain have been upgraded to support multitopology IPv6 and are operating in transition mode, transition mode can be removed from the configuration.

When multi-topology support for IPv6 is used, use the metric-style wide Cisco IOS, Cisco IOS XE, and Cisco IOS XR command to configure IS-IS to use new-style types, lengths, values (TLVs). TLVs used to advertise IPv6 information in LSPs are defined to use only wide metrics.

| router isis no adjacency-check                                    | IOS-XE - suppresses IPv6 checks only. IS-IS IPv4 also checks the protocol support of neighbors and will not allow an adjacency between a router running IS-IS IPv4 and a neighbor not supporting IPv4 |
| ----------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| router isis 1 address-family ipv6 unicast adjacency-check disable | IOS-XR                                                                                                                                                                                                |

{% hint style="info" %}
If the shortest path to an IPv6 destination would go via a non-IPv6 neighbor, the route is not installed in the IPv6 routing table.
{% endhint %}

{% hint style="info" %}
IOS XE defaults to IPv6 **single-topology** IS-IS. IOS XR defaults to IPv6 **multi-topology** IS-IS.
{% endhint %}

![](<../.gitbook/assets/Unknown image (670)>)

### Exam topics

Correct answer: A

![](<../.gitbook/assets/Unknown image (671)>)
