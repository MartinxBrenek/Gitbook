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
  anchors:
    visible: true
---

# Multicast

IP multicast is a network communication mechanism in which a sender transmits a single stream of packets to a multicast group identified by a special IP multicast destination address. The sending host places the multicast group address in the IP destination field, and the network is responsible for delivering the traffic only to receivers that have explicitly joined that group. Any host can send traffic to a multicast group, but only subscribed members receive and process the packets.

IP multicast routers and multilayer switches forward incoming IP multicast packets out all interfaces that lead to members of the multicast group. Any host, regardless of whether it is a member of a group, can send to a group. However, only the members of a group receive the message.

Unicast video streaming consumes more bandwidth, because the server has to send separate stream for each end host and maintain session state info for all hosts and broadcast consumes CPU resources of uninterested workstations, because they must still process the broadcast packets, whereas multicast can be turned off on the NIC level, and unlike broadcast, multicast can be routed. For Multicast communication the server maintain only one session for all network devices that selectively request to receive the stream.

A single data packet is sent and replicated on the transit links as it traverses the multicast distribution tree (MDT) established in a network.

Routers process fewer packets because they receive only a single copy of the packet. This packet is then multiplied and sent on outgoing interfaces where there are receivers.

Because downstream routers perform packet multiplication and delivery to receivers, the sender or source of multicast traffic does not have to know the unicast addresses of the receiver.

![](<../.gitbook/assets/Unknown image (1816)>)

### Multicast application types

Most multicast applications are UDP-based. This foundation results in some undesirable consequences when compared to similar unicast TCP applications.

There are different types of multicast applications. Here are two of the most common models:

**One-to-many**, where one sender sends data to many receivers.

**Many-to-many**, where a host can simultaneously be a sender and a receiver.

Other models are also used, especially in financial applications and networks. These include **many-to-one**, where many receivers are sending data back to one sender, and **few-to-many**.

Many new multipoint applications are emerging as demand for them grows:

**Real-time applications** include live broadcasts, financial data delivery, whiteboard collaboration, and video conferencing

**Not-real-time applications** include file transfer, data and file replication, and VoD.

The figure presents major IP multicast application categories.

![](<../.gitbook/assets/Unknown image (1817)>)

Best-effort delivery results in occasional packet drops. These losses may affect many multicast applications that operate in real time (for example, video and audio). Also, requesting retransmission of the lost data at the application layer in these not-quite-real-time applications is not feasible. Heavy drops on voice applications result in jerky, missed speech patterns that can make the content unintelligible when the drop rate gets high enough. Sometimes, moderate to heavy drops in video appear as unusual artifacts in the picture. However, even low drop rates can severely affect some compression algorithms. This action causes the picture to become jerky or to freeze for several seconds while the decompression algorithm recovers.

Duplicate packets may occasionally be generated as multicast network topologies change. Applications must expect occasional duplicate packets to arrive, and they should be designed accordingly.

Out-of-sequence delivery of packets to the application may also occur during network topology changes or during other network events that affect the flow of multicast traffic.

UDP has no reliability mechanisms, so reliability issues have to be addressed in multicast applications where reliable data transfer is necessary.

It remains a challenge to restrict multicast traffic to only a selected group of receivers. Eavesdropping issues are not sufficiently solved yet. Some commercial applications (for example, financial data delivery) will only be possible when reliability and security issues are properly solved.

![](<../.gitbook/assets/Unknown image (1818)>)

### Addressing

#### IPv4 multicast group addresses (Layer 3)

According to RFC 3171, addresses 224.0.0.0 through 239.255.255.255, the former Class D addresses, are designated as multicast addresses in IPv4. The sender sends a single datagram (from its unicast address) to the multicast address, and the intermediary routers take care of making copies and sending them to all receivers that have registered their interest in receiving the multicast data for that multicast address.

The Class D IP addresses (224.0.0.0 to 239.255.255.255) define each multicast group. Addresses are allocated dynamically and represent receiver groups, not the individual hosts.

**Local network control block (224.0.0/24)** are link-local addresses used for protocol control traffic that is not forwarded out a broadcast domain. Packets that are multicast to these addresses are always transmitted with a TTL of 1, so that they do not leave the local link.

**Global-scope/Internetwork control block (224.0.1.0/24)** used for protocol control traffic that may be forwarded through the Internet

**Administratively scoped block (239.0.0.0/8)** limited to a local group or organization; free to use in any private domain; similar to IP private ranges

The administratively scoped multicast address space is divided into the following scopes:

**Local scope** (239.255.0.0/16, and grows downward to 239.254.0.0/16, 239.253.0.0/16)

**Organization local scope** (239.192.0.0/14 with possible expansion to ranges 239.0.0.0/10, 239.64.0.0/10, and 239.128.0.0/10)

**GLOP block (233.0.0.0/8)** was interim solution providing any content/service provider or an organization with their own registered 16-bit public AS number with their own /24 (255 addresses) globally unique multicast IP address range. Two middle octets of GLOP multicast address block are derived from assigned 16-bit Autonomous System Numbers (ASN). Today a 32-bit ASN number is used

![](<../.gitbook/assets/Unknown image (1819)>)

**Source Specific Multicast (SSM) block (232.0.0.0/8)** forwards traffic to receivers from only those multicast sources which they explicitly subscribed for

By limiting the source in this manner, SSM reduces demands on the network and improves security. This protocol will always allow the building of the distribution tree that is rooted at the source for any group address from the range 232.0.0.0/8 (232.0.0.1-232.255.255.255)

SSM is best understood in contrast to ASM (Any-Source Multicast). In the ASM service model, a receiver expresses interest in traffic to a multicast address. The multicast network must discover all multicast sources sending to that address, and route data from all multicast sources to all interested receivers.

In the SSM service model, in addition to the receiver expressing interest in traffic to a multicast address, the receiver expresses interest in receiving traffic only from specific multicast sources sending to that multicast address. This relieves the network of discovering many multicast sources, and reduces the amount of multicast routing information that the network must maintain.

![](<../.gitbook/assets/Unknown image (1820)>)

![](<../.gitbook/assets/Unknown image (1821)>)

#### Ethernet multicast MAC mapping (Layer 2)

Every IP multicast group address have it's associated special MAC address that allows Ethernet interfaces to identify multicast packets to a specific group

Multicast has fixed reserved Layer 2 MAC prefix: 01:00:5E

The first byte of MAC address is the individual/group bit (I/G) bit, that is always set to 1, indicating multicast frame, and the 25th bit is always 0

Last 23 bits are added from the second 23-bit part of the IP multicast group

Example mapping the multicast IP address 239.255.1.1 into multicast MAC address

The first 25 bits are always fixed; the last 23 bits are copied directly from the multicast IP address

![](<../.gitbook/assets/Unknown image (1822)>)

A multicast IP address also has 32 bits, but the first 4 bits are always the same (1110) because we use the 224.0.0.0 – 239.255.255.255 range. This means that each multicast IP address has 28 unique bits.

Now if we want to map our 28 bit multicast IP address to our 23 bit MAC address, we have a problem…we miss 5 bits of mapping information:

This means we have to map multiple Multicast IP addresses to the same Multicast MAC address. We don’t have enough MAC addresses to give each multicast IP address its own MAC address.

We miss 5 bits of mapping information. This means we will map 32 multicast IP addresses to 1 multicast MAC address.

The mapping of thirty-two multicast IP addresses to a single MAC address is a direct consequence of the mismatch between the bit space available in the IP multicast range and the bit space allocated within the IEEE 802.1 multicast MAC address format. IPv4 multicast addresses occupy the Class D range from 224.0.0.0 to 239.255.255.255, which is defined by the high-order four bits being set to 1110. This leaves 28 bits available to identify specific multicast groups. However, the IANA-reserved MAC address prefix for IPv4 multicast is 01-00-5e, followed by a low-order bit set to 0, which leaves only 23 bits of the MAC address available to represent the IP multicast group. To bridge the gap between the 28-bit IP group ID and the 23-bit MAC suffix, the low-order 23 bits of the IP address are mapped directly into the MAC address. This process discards the 5 bits located between the fixed 1110 prefix and the final 23 bits of the IP address. In binary mathematics, the number of unique values that can be represented by a specific number of bits is determined by 2^n, where n is the number of bits. Since 5 bits are ignored during the mapping process, there are 2^5 possible combinations of those discarded bits for any given 23-bit suffix. This results in exactly 32 different IP multicast addresses mapping to the same MAC address. For example, the IP addresses 224.1.1.1, 224.129.1.1, and 239.129.1.1 all share the same low-order 23 bits, causing them to resolve to the same Layer 2 destination of 01-00-5e-01-01-01. This oversubscription requires the network interface card to pass all traffic for that MAC address up to the IP stack, where the software must then perform further filtering to determine if the specific IP group is actually desired

The multicast IP addresses above all map to the same multicast MAC address (01-00-5E-01-01-01). This can cause some problems in our networks. For example, a host that listens to the 239.1.1.1 multicast IP address will configure its network card to listen to MAC address 01-00-5E-01-01-01. If someone else is streaming to the 224.1.1.1 multicast IP address, it will also end up at our host because the MAC address is the same. The host will have to look at the IP address of the received frame to see if it’s for 239.1.1.1 and discard frames that are meant for 224.1.1.1.

Therefore, the freely useable part of an IPv4 multicast address is 5 bits longer than that of a MAC multicast address. How can you map IPv4 multicast addresses deterministically to MAC multicast addresses?

The simple answer is: You cannot do so.

This does not cause any functional problems, but it may lead to suboptimal multicast traffic distribution. Several multicast streams may be delivered to an endpoint, even though it only subscribed to one. However, the endpoint would discard unneeded traffic. Since this typically happens on the switch before the endpoint, there is normally no problem.

When deciding on IP multicast addressing, it is preferable if the last 23 bits of the address are unique.

#### Multicast session discovery (SDR / SAP)

In the multicast backbone, the session directory application served as a means for announcing available sessions and to assist in creating new sessions. The initial session directory tool was revised, resulting in the SDR tool. SDR is an applications tool that allows the following:

Session description and its announcement.

Transport of session announcement via well-known multicast groups (224.2.127.254).

SDR uses SAP (Session Announcement Protocol), which will periodically multicast a session announcement packet describing a particular session. SAP announcement packets can be received by a multicast receiver by joining the well-known group 224.2.127.254. A user can then select the option to receive traffic for a multicast group.

### Multicast service model

Receivers may dynamically join or leave an IPv4 multicast group at any time using IGMP messages, and may dynamically join or leave an IPv6 multicast group at any time using MLD messages. Messages are sent to the last-hop routers, which manage group membership.

Routers use multicast routing protocols—for example, DVMRP (Distance Vector Multicast Routing Protocol), MOSPF (Multicast Open Shortest Path First), CBT (Core Based Tree), PIM (Protocol Independent Multicast), and so on—to efficiently forward multicast data to multiple receivers. The routers listen to all multicast addresses and create multicast distribution trees, which are used for multicast packet forwarding.

Routers identify multicast traffic and forward the packets from senders toward the receivers. When the source becomes active, it starts sending the data without any indication. First-hop routers, to which the sources are directly connected, start forwarding the data to the network. Receivers that are interested in receiving IPv4 multicast data register to the last-hop routers using IGMP membership messages. Last-hop routers are those routers that have directly connected receivers. Last-hop routers forward the group membership information of their receivers to the network, so that the other routers are informed about which multicast flows are needed.

For a multicast network to function properly, a multicast distribution tree should be built and maintained by network multicast routers.

Multicast receivers listen to the session announcements or use some other mechanism to learn about the available multicast sessions. Receivers may select certain multicast groups and report their interest by sending a host membership report to the last-hop router. The last-hop router then starts forwarding traffic for a specified multicast group to the receivers. After the receivers start receiving multicast data, they maintain their group membership and may also provide feedback to the source about the received data. When receivers want to stop receiving certain multicast data, they send a leave message to the router.

#### Intradomain and interdomain multicast routing protocols

Multicast protocols may differ depending on where in a multicast network they are implemented.

There is no multicast protocol for source registering used between the source and the first-hop router, such as IGMP, which is used between the last-hop router and the receiver.

Inside the multicast network, various multicast routing protocols are used. The multicast routing protocols may be separated into these two groups:

**Intradomain:** DVMRP, PIM, MOSPF, and CBT

**Interdomain:** MP-BGP with MSDP

Between the last-hop router and the receivers, IGMP or MLD are used. Receivers use IGMP (for IPv4) or MLD (for IPv6) to report their multicast group membership to the router.

Protocol Independent Multicast (PIM) is a L3 protocol that ensures dynamic creation of multicast distribution trees for multicast forwarding.

PIM monitors the unicast routing table and also maintains its own multicast routing table to track Incoming interface (IIF) and Outgoing interface (OIF) for multicast traffic

**Outgoing Interface List (OIL)** is a group of OIFs that are forwarding multicast traffic. Multicast route entries are displayed in the format (S, G) (\*,G)

PIM comes in two versions: PIM-SM (PIM Sparse Mode) and PIM-DM (PIM Dense Mode). Much of the effort of the IETF (Internet Engineering Task Force) group that is working toward a multicast protocol is focused on PIM-SM. PIM uses the underlying unicast routing table (in any unicast routing protocol)

Internet Group Management Protocol (IGMP) is L3 protocol used by hosts in a local segment to communicate over L2 with routers about their interest in receiving multicast traffic for specific groups. This action, in turn, permits the IP multicast router to join the specified multicast group and to begin forwarding the multicast traffic onto the network segment.

![](<../.gitbook/assets/Unknown image (1823)>)

![](<../.gitbook/assets/Unknown image (1824)>)

In IP multicast, there is no dedicated multicast control protocol between sources and their first-hop routers. Multicast sources are unaware of multicast routing and do not signal their presence using a multicast-specific protocol. A source simply sends packets to a multicast destination address, and the first-hop router treats those packets as regular IP packets whose destination lies in the multicast address space. The router then applies multicast forwarding logic based on its existing multicast routing state.

Within a single domain, multicast control signaling exists only on two planes. On the receiver side, IGMPv2 or IGMPv3 operates between receivers and the last-hop router to signal interest in multicast groups or specific (S,G) channels. Inside the routing domain, PIM-Sparse Mode runs between routers, including any rendezvous points, to build and maintain multicast distribution trees. PIM is responsible for creating (\*,G) and (S,G) state, sending joins and prunes, and ensuring traffic flows only where receivers exist.

The absence of a protocol between sources and first-hop routers is a deliberate design choice. Source discovery in ASM is handled indirectly through the rendezvous point mechanism, where the first-hop router encapsulates initial multicast traffic in PIM Register messages and sends them to the RP. This is not a protocol interaction with the source itself; it is purely router-to-router signaling triggered by data-plane traffic. In SSM, even this mechanism disappears, because receivers explicitly request traffic from a known source and the network never needs to discover sources dynamically.

Note The DVMRP, MOSPF, and CBT protocols are outside of the scope of this course and will not be discussed further.

DVMRP evolved from version 1 to version 3. DVMRP version 1 (RFC 1075) is obsolete and is never used. DVMRP version 2 is an old Internet draft and is the implementation that was used throughout the multicast backbone. DVMRP version 3 is the current Internet draft, although most vendors have not completely implemented it. DVMRP uses a special RIP-like multicast routing table in addition to the following:

Poison reverse: The special metric of infinity (32) and the originally received metric are used to signal that the router must be placed on the distribution tree for the source network.

Prunes and grafts: Routers send prunes and grafts up the distribution tree, just as they do in PIM-DM.

MOSPF is defined in RFC 1584. Because of the nature of OSPF, it is not scalable when the number of groups in an area increases or when multicast groups are very active. MOSPF uses the underlying OSPF unicast routing protocol link-state advertisements (LSAs) to advertise the existence of directly connected receivers. This information is then used to build (S,G) trees. Each router maintains an up-to-date image of the topology of the entire network (more explicitly, of an entire area).

CBT is defined in RFC 2189, but it does not have significant usage or industry support, nor is it a primary focus of the IETF working groups. CBT uses the underlying unicast routing table and the join-prune-graft mechanisms (much like PIM-SM)

### Reverse Path Forwarding (RPF) check

In unicast routing, when the router receives the packet, the decision whether to forward the packet is made depending on the destination address of the packet. In multicast routing, where to forward the multicast packet depends on where the packet came from.

Multicast routers must know the origin of the packet in addition to its destination, which is the opposite of unicast routing. In multicast origination, the source IP address denotes the known multicast source, and the destination IP address denotes a group of unknown receivers. Multicast sources use the group address as the IP destination address in their data packets. The receiver uses this group address to inform the network that they are interested in receiving the packets that are sent to that group.

Multicast routing depends on the reverse path because when a receiver (host) sends an (\*,G) join, the multicast router needs to build the SPT

PIM uses the unicast routing table to perform loop prevention RPF checks

When a router receives a multicast packet, it performs a reverse path check by looking at the source address of the packet.

The router checks its unicast routing table to determine the interface (egress) that it would normally use to reach that source

If the incoming multicast packet arrives on the interface that the router would use to reach the source in a unicast scenario, the packet is considered valid, and the router forwards it

The multicast packet is forwarded out of each interface that is in the OIL. OIL entries list the interfaces of the multicast neighbors that are downstream of the current router.

The incoming interface (or RPF interface) on which the packet was received is never in the OIL. Therefore, the multicast packet is never forwarded back out of the RPF interface.

If this outgoing interface does not match the interface where the packet was received, the router silently drops the packet

This can happen when unicast shortest paths not matching multicast distribution trees (bad, PIM-enabled interface does not follow the unicast routing table) or packet is being flooded out of the wrong interface (the intended loop prevention)

Additionally, temporary RPF failures may happen in situations where the network is unstable and the routers keep changing their shortest path tables. The best way to find and isolate the RPF issue in your lab is probably to run the #debug ip mfib pak command on all routers, starting from the one closest to the destination and moving upstream to the source. This command will show you process-switched multicast packets and signal any RPF issues.

Note that you may need to enter the commands #no ip mfib cef input and #no ip mfib cef output on all interfaces where multicast packets are received; this will disable cef switching of multicast packets on the interfaces where it is specified

#### Static multicast mroute

Things like redistribution, PIM and/or IGP not enabled on a interface, \[…] can lead to an RPF failure. Compared to “normal” static routes which defines the destination prefix and next-hop/outgoing if, static multicast routes define the source prefix and the next-hop/incoming interface \[ ip | ipv6 ] mroute \[ | ] \[]

that will take precedence over the unicast routing table entry to the multicast source

**Reverse Path Forwarding (RPF) interface**: interface with the lowest-cost path (AD/metric) to the IP address of the source (SPT) or the RP. If cost is the same, then highest IP interface wins

**RPF neighbor** is the next hop with the best path to reach the multicast source

In the following figure, the left part illustrates a failed RPF check. The router on the left side receives a multicast packet from source 10.10.3.21 on interface Serial0. The router performs the RPF check by examining the unicast routing table. The unicast routing table indicates that interface Serial1 is the shortest path to the network 10.10.0.0/16. Because interface Serial0 is not the shortest path to the network from which the packet from the source 10.10.3.21 arrived, the RPF check fails, and the packet is discarded.

![](<../.gitbook/assets/Unknown image (1825)>)

#### Multicast scoping

TTL thresholds may be set on a multicast router interface to limit the forwarding of multicast traffic to outgoing packets with TTLs greater than the TTL threshold. A TTL threshold of 0 implies that no threshold has been set.

As a multicast packet arrives, the TTL is decremented by 1. If the resulting TTL is less than or equal to 0, it is dropped. If a multicast packet is forwarded out of an interface with a TTL threshold configured, then its TTL is checked against the configured TTL threshold.

If a packet TTL is less than or equal to the specified TTL threshold, the packet is not forwarded out of the interface. If a packet TTL is greater than the specified TTL threshold, the packet is forwarded out of the interface. Typically, TTL thresholds are set on the boundary multicast routers to make sure that traffic does not cross where it should not - to keep enterprise multicast delivery within domain

Although TTL scoping seems easy to implement, there are many problems with it. If you use TTL scoping with broadcast-and-prune multicast protocols, the router—which discards the multicast packet—will not be able to prune any upstream sources. It is also impossible to configure overlapping zones with TTL scoping.

Address scoping allows the multicast boundaries to be established per multicast group address, and is much more flexible than TTL scoping. The multicast traffic that does not match the address within a scope is dropped on an incoming interface and on an outgoing interface.

If you use address scoping for overlapping zones, you must use separate address spaces within these zones, which makes the administration of multicast addresses difficult.

![](<../.gitbook/assets/Unknown image (1826)>)

### Internet Group Management Protocol (IGMP)

dynamically registers individual hosts to join a multicast group. When a receiver wants to receive multicast traffic from a source, it sends an IGMP join to its router

When you enable PIM on interface, IGMP version 2 will be enabled automatically by default.

#### IGMP querier

**IGMP Querier** is a process that runs on a switch or router. Its responsibility is to send out IGMP group membership queries on a timed interval, to retrieve IGMP membership reports

from active Group members (any client that has joined a multicast group), and to allow updating of the group membership tables. There is one active Querier per subnet. If there is more than one Querier then the Queriers hold an election and the one with the lowest IP address is chosen to be active.

#### IGMPv1

Multicast routers periodically (usually every 60 to 120 seconds) send membership queries to the all-hosts multicast address (224.0.0.1) to solicit the multicast groups that are active on the local network.

Hosts that want to receive specific multicast group traffic send membership reports. Membership reports, with a TTL of 1, are sent to the multicast address of the group from which the host wants to receive traffic. Hosts either send reports asynchronously (when they want to first join a group—unsolicited reports) or in response to membership queries. In the latter case, the response is used to maintain the group in an active state so that traffic for the group continues to be forwarded to the network segment.

In IGMPv1, there is no election of an IGMP querier. If more than one router on the segment exists, all the routers send periodic IGMP queries. IGMPv1 has no special mechanism by which the hosts can leave the group. If the hosts are no longer interested in receiving multicast packets for a particular group, they simply do not reply to the IGMP query packets sent from the router. The router continues sending query packets. If the router does not hear a response in three IGMP queries, the group times out and the router stops sending multicast packets on the segment for the group. If the host later wants to receive multicast packets after the timeout period, the host simply sends a new IGMP join to the router, and the router begins to forward the multicast packets again

After a multicast router sends a membership query, there may be many hosts that are interested in receiving traffic from specified multicast groups. To suppress a membership report storm from all group members, a report suppression mechanism is used among group members. Report suppression saves CPU time and bandwidth on all systems.

![](<../.gitbook/assets/Unknown image (1827)>)

#### IGMPv2

Because of some limitations that were discovered in IGMPv1, work was begun on IGMPv2 in an attempt to remove these limitations. Most of the changes between IGMPv1 and IGMPv2 were made primarily to address the issues of leave and join latencies, in addition to address ambiguities in the original protocol specification.

A group-specific query that was added in IGMPv2 allows the router to query its members only in a single group instead of all groups. This action is an optimized way to quickly find out if any members are left in a group without asking all groups for a report. The difference between the group-specific query and the membership query is that a membership query is multicast to the all-hosts address (224.0.0.1), whereas a group-specific query for group G is multicast to the group G multicast address.

A leave-group message allows hosts to tell the router that they are leaving the group. This information reduces the leave latency for the group on the segment when the member who is leaving is the last member of the group.

Unlike IGMPv1, IGMPv2 has a querier election mechanism. The lowest unicast IP address of the IGMPv2-capable routers will be elected as the querier. By default, all IGMP routers are initialized as queriers, but must immediately relinquish that role if a lower-IP-address query is heard on the same segment. The query-interval response time has been added to control the burstiness of reports. This time is set in queries to convey to the members how much time they have to respond to a query with a report. IGMPv2 is backward-compatible with IGMPv1.

#### IGMP message types

are encapsulated in an IP packet and forwarded only within a subnet with TTL set to 1

**Version 2 membership report / IGMP join** (type value 0x16) used by receivers to join a multicast group or to respond to a local router’s membership query message

**Version 1 membership report** (type value 0x12) is for IGMPv1 compatibility

**Version 2 leave group** (type value 0x17) stop receiving multicast traffic for a group they joined

**General membership query** (type value 0x11) sent periodically to the all-hosts group add 224.0.0.1 to monitor, if there are any active receivers in the attached subnet

**Group address field** is set 0.0.0.0.

**Group specific query** (type value 0x11) is sent in response to a leave group message. The group address is the destination IP address of the IP packet

![](<../.gitbook/assets/Unknown image (1828)>)

**Max response time** set only in general and group-specific membership query messages (type value 0x11); it specifies the maximum allowed time before sending a responding report in

1/10 th of a second. In all other messages, it is set to 0x00 by the sender and ignored by receivers

**Checksum** standard checksum algorithm used by TCP/IP

**Group address** This field is set to 0.0.0.0 in general query messages and is set to the group address in group-specific messages

#### IGMPv2 operation

When a receiver wants to receive a multicast stream, it sends an unsolicited membership report (IGMP join) to the local router for the group it wants to join (MLD for IPv6)

The local querier/ router then sends this request upstream towards the source using a PIM join message

When the local router starts receiving the multicast stream, it forwards it to downstream subnet where the receiver resides

The router then starts periodically sending general membership query messages into the subnet, to the all-hosts group address 224.0.0.1, to check and see if there is still a listener on the network segment (10sec by default)

In response to this query, receivers set an internal random timer between 0 and 10 seconds. When the timer expires, receivers send membership reports for each group they belong to.

If a receiver receives another receiver’s report for one of the groups it belongs to while it has a timer running, it stops its timer for the specified group and does not send a report; this is meant to suppress duplicate reports, since for the router it is sufficient to receive one response, so he can continue to forward the stream

When a receiver wants to leave a group, if it was the last receiver to respond to a query, it sends a leave group message to the all-routers group address 224.0.0.2

Otherwise, it can leave quietly because there must be another receiver in the subnet. When the leave group message is received by the router, it sends a specific membership query to the group multicast address to determine whether there are any receivers interested in the group remaining in the subnet. If there are none, the router removes the IGMP state for that group.

If there is more than one router in a LAN segment, an IGMP querier election takes place to determine which router will be the querier.

They sends query messages with their interface address to 224.0.0.1 and the one with lowest IP address wins

If the querier router stops sending membership queries for any reason, the new election takes place after the 2x the query interval, which is 60s by default

Note While the first-hop router will use PIM to forward multicast traffic further into the network, the direct signaling of group membership from a host (which could be the source) to that first-hop router is done via IGMP

#### IGMPv3

The main intention of IGMPv3 is to allow hosts to signal that they only want to receive traffic from a particular source within a multicast group. This enhancement will allow routers in sparse mode to build a source distribution tree directly, thereby avoiding the RP.

Version 3 membership report now supports source filtering SSM, giving a capability to the receiver to pick the source they wish to accept multicast traffic from and how many sources

With IGMPv3, receivers signal membership to a multicast group address using a membership report in the following two modes:

**Include mode:** receiver announces membership to a multicast group address and provides a list of source addresses (include list) from which it wants to receive traffic

**Exclude mode:** provides a list of source addresses (exclude list) from which it does not want to receive traffic. Receiver then receives traffic only from sources whose IP addresses are not listed on the exclude list. To receive traffic from all sources, which is the behavior of IGMPv2, a receiver uses exclude mode membership with an empty exclude list.

![Internet Group Management Protocol (IGMP) - HackMD](<../.gitbook/assets/Unknown image (1829)>)

![IGMP Snooping breaking IGMPv3 SSM joins. | Ubiquiti Community](<../.gitbook/assets/Unknown image (1830)>)

#### IGMP snooping

Because switches treat multicast MAC addresses as unknown frames and flood them out all ports, potentially wasting the bandwidth, IGMPv3 introduces methods to reduce multicast flooding on a LAN segment.

First method is to configure Static MAC address entries, the second method is to configure IGMP Snooping, where a switch examines IGMP joins and leaves sent by receivers and maintains a table of those interfaces and forwards multicast traffic groups only out those ports

![](<../.gitbook/assets/Unknown image (1831)>)

| ip igmp snooping                                                                                                                                                                                                                | ! Enable IGMP snooping globally                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| interface range FastEthernet0/1 - 24 switchport access vlan ip igmp snooping vlan                                                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ip igmp snooping querier                                                                                                                                                                                                        | ! Set the querier address (optional, if no multicast router present)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| show ip igmp snooping                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| IGMP Filter                                                                                                                                                                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| R1(config)#ip access-list standard LIMIT\_IGMP R1(config-std-nacl)#deny host 239.2.2.2 R1(config-std-nacl)#permit 224.0.0.0 15.255.255.255 R1(config)#interface FastEthernet 0/0 R1(config-if)#ip igmp access-group LIMIT\_IGMP |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| IGMP snooping filter                                                                                                                                                                                                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| SW1(config)#ip igmp profile 1 SW1(config-igmp-profile)#deny SW1(config-igmp-profile)#range 239.3.3.3 SW1(config)#interface FastEthernet 0/2 SW1(config-if)#ip igmp filter 1                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| IGMP helper address - e.g. IGMP proxy                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| interface Vlan20 ip igmp helper-address 192.168.10.1                                                                                                                                                                            | The ip igmp helper-address command is used to forward IGMP control messages, specifically Membership Reports and Leave messages, to a remote multicast-capable router. Under normal operation, IGMP messages sent by hosts are processed only by the directly connected first-hop multicast router, and they are not propagated beyond that device. In some network designs, the first-hop router or Layer 3 interface receiving the IGMP is not the router that should make multicast forwarding decisions, which prevents the correct multicast router from learning about active receivers. By configuring ip igmp helper-address, the device that hears the IGMP messages forwards them unmodified to a specified multicast router, allowing that router to create the appropriate (\*,G) or (S,G) state even though it is not directly connected to the receivers. A typical scenario is an access router or Layer 2 switch with IGMP awareness connected to end hosts, while the actual multicast routing function resides on a downstream router; the helper mechanism ensures that IGMP joins received on an access VLAN are redirected to the correct multicast router. This feature forwards only IGMP control traffic and does not carry multicast data streams, nor does it replace multicast routing protocols such as PIM or their associated RPF checks; it exists solely to enable proper receiver registration in non-standard or asymmetric multicast topologies. |

ip igmp static-group: This command informs the router that there are interested receivers on that interface without forcing the router itself to join the group.

This allows the router to switch the multicast packets in hardware (Fast Switching or CEF) rather than sending them to the CPU.

It achieves the goal of forwarding multicast traffic out of those interfaces while drastically reducing the load on the processor.

### Protocol Independent Multicast (PIM)

is a multicast routing protocol enabling routers to route multicast traffic between network segments. It leverage any unicast routing protocol to identify the path between the source and receivers. "Protocol-independent" means that the multicast routing table can be built from the underlying unicast routing table. So regardless of whether the unicast routing table was built by RIPng, EIGRP, OSPFv3, or any other protocol, PIM can construct the multicast routing table. The only limitation in this case will be that multicast traffic and unicast traffic will follow the same path.

#### Trees and state: (S,G) and (\*,G)

PIM Distribution Trees/Multicast distribution trees (MDT) define the path that IP multicast traffic flows through the network to reach the receivers

Source Trees source is the root of the tree, and branches form a distribution tree through the network all the way down to the receivers

The special notation of (S, G), pronounced “S comma G”, enumerates an SPT where S is the IP address of the source and G is the multicast group address

When this tree is built, it uses the Shortest path tree (SPT) through the network from the source to the receivers of the tree

Shared Trees root of the shared tree is not the source but a router designated as the Rendezvous Point (RP), which is responsible for keeping track of multicast sources and building trees to distribute multicast to other routers. Uses notation (\*, G), to express that there is an interested receiver, where \* means all sources, and the G represents the multicast group, referred to as RP trees (RPTs). Used in sparse mode only

![](<../.gitbook/assets/Unknown image (1832)>)

![](<../.gitbook/assets/Unknown image (1833)>)

For an incoming multicast packet, the multicast routing table is consulted.

If there is an entry for (S,G), that is, the source and the multicast group, this entry is used.

If there is an entry for (\*,G), that is, the group entry exists, but the source is not in the table, this entry is used.

If there is no entry, then the source has just started sending (remember, there is no protocol for a source to announce that it's going to start sending; it just sends.) The router uses a PIM register packet to encapsulate and forward the traffic to the RP.

The incoming interface is checked. Traffic is expected to arrive on the "incoming interface." If the multicast packet was received on the correct interface, it is forwarded. If not, it is discarded. The RPF check is performed to avoid multicast routing loops.

The forwarding is based on the OIL. The multicast packet is sent to each interface in the OIL.

If there is no source sending yet, but receivers have joined the (\*,G), then the state on the RP looks as shown on the following figure.

In the mroute table, there is a state for (\*,G). However, if no source is sending yet, the incoming interface is Null, and the RPF neighbor is also set to 0.0.0.0.

However, in the OIL you can see the beginning of the RP sourced shared tree. Every interface over which PIM joins have been received for this group will show up on this OIL. Note that since it is multicast, you do not see individual receivers listed here, nor next hop routers. The interface information is sufficient—Once multicast traffic flows, it is sent out those interfaces in the OIL, and the routers that need the traffic downstream will see that multicast flow and pass it on again to their interfaces on the OIL.

![](<../.gitbook/assets/Unknown image (1834)>)

#### PIM control packets

PIM-SM for PIMv2 packets are encapsulated into IP packets with protocol number 103. PIMv1 uses IGMP packets

Hello messages used to discover and maintain adjacency between PIM routers. They are sent by default every 30 seconds out each PIM-enabled interface to learn about the neighboring PIM routers on each interface to the all PIM routers address.

In addition, the hold time indicates how long a router will keep a neighbor that is no longer sending messages, and an option is available in the options field that assists in electing the DR. The DR priority may be set to force a router on a multiaccess segment to become the DR.

Join/Prune Messages

Join messages are sent by routers to upstream neighbors, expressing interest in receiving multicast traffic for a specific group. Prune messages are sent to stop the flow of traffic for a particular group.

Assert message

To prune duplicate multicast streams, where the winner is lowest metric to source or the one with the highest IP add

Bootstrap messages are used in PIM-SM (Sparse Mode) to discover the RP for a multicast group

PIM register and register-stop messages: For registering multicast sources. The designated first-hop router sends the unicast PIM register message to the RP when the source starts multicast traffic. A PIM register-stop message is sent from the RP toward the sender via unicast. After the first-hop router receives this message, it stops encapsulating multicast packets in the PIM register message and starts sending them via multicast. The register-stop message is sent as follows:

Soon after the first packet arrives natively down the SPT built between the RP and the first-hop router

Immediately if there is no shared tree for the group for which the first-hop router is registering to the RP

Candidate-RP Advertisement message (or Cisco Annunce messages) are used in Auto-RP and BSR to announce the candidacy of a router to become the RP

The BSR in PIMv2 collects RP information and distributes it in PIM bootstrap messages. PIM bootstrap messages are also used to dynamically elect the BSR.

The PIM Auto-RP mechanism is the Cisco implementation for automated distribution of group-to-RP mappings in a PIM-SM network. The function, similar to BSR, is assigned to a mapping agent in Auto-RP—the mapping agent is responsible for sending Cisco discovery messages to the Cisco Discovery (224.0.1.40) multicast group.

PIM RP reachability message (specific to Cisco): Used by the RP to report its reachability to all the nodes on a shared tree.

State refresh message Once (S,G) is pruned, traffic re-flooded about every 3 minutes and state refresh ensures the keep alive for prune state or for the join state

Graft message used in dense mode to unprune (S,G), when it receive the IGMP join message from host

![](<../.gitbook/assets/Unknown image (1835)>)

![Understand the PIM Assert Mechanism - Cisco](<../.gitbook/assets/Unknown image (1836)>)

#### PIM-SM (ASM) high-level operation

Receiver sends an IGMP join to the LHR to join the multicast group. Last-hop router (LHR) is a leaf responsible for sending PIM joins (\*,G) from the receivers upstream toward the RP, forming a shared tree from the RP to the LHR. First-hop router (FHR) is directly attached to source, and is responsible for sending register messages to RP

The RP then sends a PIM join (S,G) to the FHR, forming a source tree between the source and the RP. In essence, two trees are created: an SPT from the FHR to the RP (S,G) and a shared tree from the RP to the LHR (\*,G). Multicast starts flowing down from the source to the RP (using source tree) and from the RP to the LHR and then finally to the receiver (using shared tree)

PIM register tunnel from the FHR to the RP is establihed to send encapsulated multicast messages and remains in an active up/up state, even when there are no active multicast streams

![](<../.gitbook/assets/Unknown image (1837)>)

#### PIM operating modes

Dense mode protocols are DVMRP, MOSPF, and PIM-DM. Dense mode protocols are not likely to be used in the service provider network.

By using the pull model, the protocol enables multicast traffic to be sent only where it is requested. This kind of behavior may be described as explicit join behavior, because the source assumes that no receivers are interested unless the receivers explicitly join the group. Sparse mode protocols are PIM-SM and CBT. Sparse mode protocols maybe used in the access, aggregation, IP edge, and core Cisco IP NGN infrastructure layers of the service provider network.

PIM-SM is appropriate for the wide-scale deployment of both densely and sparsely populated groups in the enterprise network. It is the optimal choice for all production networks, regardless of size and membership density.

There are many optimizations and enhancements to PIM, including the following:

BIDIR-PIM mode is designed for many-to-many applications.

SSM is a variant of PIM-SM that only builds source-specific SPTs and does not need an active RP for source-specific groups. The address range 232.0.0.0/8 is used

![](<../.gitbook/assets/Unknown image (1152)>)

#### PIM Dense Mode (PIM-DM)

push model multicast routing where routers assume that all networks are interested in receiving multicast traffic unless explicitly pruned

Initially, routers forward multicast traffic out of all interfaces to all other Dense mode routers. If there are no interested receivers on a particular network, routers send prune message towards source for that network. Note when traffic flow stop, (S,G) remains in table. PIM-DM does not use RPs and is not efficient.

PIM-DM is similar to DVMRP because they both use the flood-and-prune mechanism. However, unlike DVMRP, PIM-DM does not have prebuilt truncated broadcast trees when using its own routing protocol. Instead, PIM-DM uses the RPF-check (Reverse Path Forwarding check) mechanism to accept incoming multicast traffic

PIM-DM is appropriate for small implementations and trial networks, and is interoperable with DVMRP.

PIM Forwarder is a mechanim for PIM-DM to stop duplicate flows with PIM assert mechanism. Routers send assert messages to compare their AD or metric to the source and the one with best AD or metric wins and became forwarder for LAN and the losing router prunes its interface

IOS-XR doesn't support dense mode

#### PIM Sparse Mode (PIM-SM)

is based on explicit pull model, where routers assume that no network is interested in receiving multicast traffic unless explicitly requested.

PIM-SM uses shared distribution trees rooted at the rendezvous point (RP), but it may also switch to the source-rooted distribution tree, while dense mode use only source tree. Similar to PIM-DM, PIM-SM operates independently of underlying unicast protocols

PIM-SM uses an RP to coordinate the forwarding of multicast traffic from a source to its receivers. You must choose one or more routers to operate as a rendezvous point (RP). A rendezvous point is a single common root placed at a chosen point of a shared distribution tree. A rendezvous point can be either configured statically in each box or learned through a dynamic mechanism. Senders register with the RP and send a single copy of multicast data through it to the registered receivers. Group members are joined to the shared tree by their local designated router. A shared tree that is built this way is always rooted at the RP.

To get multicast traffic to the RP for distribution down the shared tree, first-hop routers with directly connected senders send PIM register messages to the RP. Register messages cause the RP to send an (S,G) join toward the source. This activity enables multicast traffic to flow natively to the RP via a SPT, and then down the shared tree.

Routers may be configured with an SPT threshold, which, once exceeded, will cause the last-hop router to join the SPT. This action will cause the multicast traffic from the source to flow down the SPT directly to the last-hop router.

The RPF check is done differently, depending on tree type. If traffic is flowing down the shared tree, the RPF check mechanism will use the IP address of the RP to perform the RPF check. If traffic is flowing down the SPT, the RPF check mechanism will use the IP address of the source to perform the RPF check.

Designated Router (DR) in PIM-SM is elected on all PIM-enabled interfaces, including point-to-point links, to maintain protocol consistency, even though it’s not functionally critical on P2P links due to no risk of duplicate traffic. The DR election is based on the highest DR priority #ip pim dr-priority (default 1), with the highest IP address as a tiebreaker, and the DR handles tasks like sending PIM Register messages to the RP for sources

DR hold time is 3.5 times the hello interval, or 105 seconds with TTL as 1. If there are no hellos after this interval, a new DR is elected

You can tune the detection speed by adjusting the hello interval using the command ip pim query-interval (e.g., ip pim query-interval 10 sets the interval to 10 seconds, reducing the hold time to 35 seconds).

Example

Here the PE1 is RP, so the P1 and P2 must agree which one is the DF for the segment where the source is connected to, based on that the one who wins registers the source and the second who prunes the SPT - indicated by flags

![](<../.gitbook/assets/Unknown image (1153)>)

![](<../.gitbook/assets/Unknown image (1154)>)

Note the RPF nbr 0.0.0.0 means that the router doesn’t need to forward traffic upstream to another router to reach the source—it’s the root of the shared tree for (,G) or handles the (S,G) state directly after receiving Register messages

![](<../.gitbook/assets/Unknown image (1155)>)

Although it is common for a single RP to serve all groups, it is possible to configure different RPs for different groups or group ranges. This approach is accomplished via access lists. Access lists permit you to place the RPs in different locations in the network for different group ranges. The advantage to this approach is that it may improve or optimize the traffic flow for the different groups. However, only one RP for a group may be active at a time.

RPs may be configured statically on each router. If you perform static configuration, you must ensure that all routers in the network agree on the RPs, otherwise the network will be broken.

| (config)# ip pim rp-address | an tunnel inteface will be automatically established. Meant for smaller networks > not scalable; no failover; no load splitting |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |

#### RP discovery mechanisms

However, a better solution is to use one of the following mechanisms, which automatically distribute the RP information:

Auto-RP (Auto-Rendezvous Point), which is the Cisco implementation

Standard PIMv2 bootstrap mechanism, by using BSRs (Bootstrap Router)

Multicast traffic forwarding in a PIM-SM network is first attempted using any matching (S,G) entries in the MRIB (Multicast Routing Information Base). If no matching (S,G) state exists, then the traffic is forwarded using the matching (\*,G) entry in the multicast routing table.

PIM-SM Shared Tree Join

In this figure, an active receiver has joined multicast group G by multicasting an IGMP membership report. A DR (designated router) on the LAN segment will receive IGMP membership reports.

The DR knows the IP address of the RP router for group G and sends a (\*,G) join for this group toward the RP. To succeed, the last-hop router looks up the unicast routing table to find the next hop for the RP.

This (\*,G) join travels hop by hop toward the RP, building a branch of the shared tree that extends from the RP to the last-hop router directly connected to the receiver.

At this point, group G traffic may flow down the shared tree to the receiver.

Note that the PIM join is sent to the "All PIM Routers" multicast group 224.0.0.13, therefore, it is a link local multicast and not sent unicast to the next hop.

![](<../.gitbook/assets/Unknown image (1156)>)

As soon as an active source for group G starts sending multicast packets, its first-hop DR registers the source with the RP. To register a source, the DR encapsulates the multicast packets in a PIM register message and sends the message to the RP using unicast.

When the RP receives the unicast register message, it de-encapsulates the multicast packets inside the unicast register message and starts sending the multicast packets down the shared tree toward the receivers. At the same time, the RP initiates the building of an SPT from the source to the RP by sending (S,G) joins toward the source. The building of an SPT causes the creation of an (S,G) state in all routers along the SPT, including the RP.

After the SPT is built from the first-hop router to the RP, the multicast traffic starts to flow from the source (S) to the RP without being encapsulated in the unicast register messages.

Therefore, for a while the RP now receives traffic both ways, through the PIM register tunnel and natively through the SPT.

When the RP begins receiving multicast data down the SPT from the source, it sends a unicast PIM register-stop message to the first-hop router. The PIM register-stop message informs the first-hop router that it may stop sending the unicast register messages.

When the first-hop router receives the PIM register stop from the RP, it stops sending the PIM register messages unicast to the RP.

At this point, the multicast traffic from the source is flowing down the SPT to the RP and, from there, down the shared tree to the receivers.

If there is no shared MDT toward receivers at this point (no interfaces listed in the OIL), the multicast traffic is not forwarded further, and the RP waits for receivers to join the shared tree.

![](<../.gitbook/assets/Unknown image (1157)>)

Traffic is now forwarded only natively in multicast down the SPT, following the OIL for the specific (S,G) entry. The keyword "Registering" is no longer visible in the incoming interface entry for (S,G).

Now the final state is reached for the first-hop router—for (S,G). (S,G) entries are kept alive by matching multicast traffic itself, (\*,G) entries are kept alive by periodic PIM join messages from the downstream neighbors.

The incoming interface for the (\*,G) is Null on the RP, since it is the root of the shared tree. For the (S,G) entries, the incoming list points toward that source S.

![](<../.gitbook/assets/Unknown image (1158)>)

Receivers Along the SPT Scenario

Receivers joining "behind" the RP follow the same join process explained earlier—They join the shared tree toward the RP.

What if a receiver joins between the source and RP?

Assume a new receiver joins at router B (\*,G)

Could join (S,G) directly on router B.

But: Needs to find all (!) sources for group G: (\*,G)

![](<../.gitbook/assets/Unknown image (1159)>)

Like in the normal creation of the shared tree to the RP, the PIM (\*,G) join is sent toward the RP.

When the RP receives the PIM join from router B, it notes that in the (\*,G) state as shown in the following figure.

The interface this PIM join was received on is now included in the OIL for the (\*,G) multicast routing entry. This OIL indicates where the receivers are located (behind which interface). In the example, interface Serial3, pointing to router B, is now also in the OIL. Receiver A has successfully joined the shared tree to the RP.

Intuitively, it would now appear that traffic from the source on the left in the example would have to first be forwarded to the RP, and then down the shared tree, back to B, which does not happen.

From the IGMP join, router B has Ethernet0 in the OIL for both (\*,G) and (S,G). When multicast traffic from the source at the left reaches router B, the traffic is automatically forwarded to the interface where Receiver A is located.

And the RP know it does not have to send the traffic back out onto the same interface on which it was received. Therefore, multicast traffic distribution is automatically optimal for this case.

![](<../.gitbook/assets/Unknown image (1160)>)

PIM SPT Switchover

RP is normally needed only to start new sessions (Shared tree/RP tree)) with sources and receivers.

As receivers join the multicast group and the network converges, routers may dynamically establish a more direct path, known as the Shortest Path Tree (SPT), between the source and the receivers

PIM-SM allows the LHR to switch from the shared tree to an SPT for a specific source. When the LHR receives the first multicast packet from the RP, it becomes aware of the IP address of the multicast source. At this point, the LHR checks its unicast routing table to see which is the shortest path to the source, and it sends an (S,G) PIM join hop-by-hop to the FHR to form an SPT. Once it receives a multicast packet from the FHR through the SPT, if necessary, it switches the RPF interface to be the one in the direction of the SPT to the FHR, and it then sends a PIM prune message to the RP to shut off the duplicate multicast traffic coming from it through the shared tree. This can be disabled as it is default behavior of cisco router

PIM-SM enables the last-hop router (the router with directly connected receivers) to switch to the SPT and bypass the RP if the multicast traffic rate is above a set threshold. This threshold is called the SPT threshold.

In Cisco routers, the default value of the SPT threshold is 0. Thus, as soon as the first packet arrives via the (\*,G) shared tree, the default action for Cisco PIM-SM routers that are attached to active receivers is to immediately join the SPT to the source.

In the procedure, the router first sets a "J" flag for a group, visible in the mroute entry. When the next (S,G) packet for that group arrives, it issues the join process.

![](<../.gitbook/assets/Unknown image (1161)>)

When the next (S,G) data packet arrives at the router, it clears the J flag again, and creates a specific entry for (S,G).

However, since other sources may well send traffic down this path, the (\*,G) entry still shows the interface toward router A.

![](<../.gitbook/assets/Unknown image (1162)>)

In this figure, the last-hop router sends an (S,G) join message toward the source to join the SPT and bypass the RP.

This (S,G) join message travels hop by hop to the first-hop router (the router that is connected directly to the source), thereby creating another branch of the SPT. This action also creates an (S,G) state in all routers along this branch of the SPT.

(S,G) traffic is now flowing directly from the source to the receiver via the SPT. A special (S,G) RP-bit prune message is sent up the shared tree to prune the (S,G) traffic from the shared tree. If the prune message were not sent, duplicate (S,G) traffic would continue flowing down the shared tree to the receiver.

![](<../.gitbook/assets/Unknown image (1163)>)

After the (S,G) RP-bit prune message has reached the RP, the branch of the SPT from the source to the RP still exists. However, when the RP has received the (S,G) RP-bit prune message via all branches of the shared tree, the RP no longer needs this (S,G) traffic. This result occurs because all of the receivers in the network are receiving the traffic via an SPT, bypassing the RP.

The RP no longer needs the (S,G) traffic. Therefore, it will send (S,G) prune messages back toward the source to shut off the flow of now unnecessary (S,G) traffic.

![](<../.gitbook/assets/Unknown image (1164)>)

When the (S,G) prune message reaches the first-hop router, it prunes the branch of the (S,G), thus tearing down the SPT that was built between the source and the RP. The traffic is now only flowing down the remaining branch of the (S,G)—the SPT built between the source and the last-hop router that initiated the switchover. Because of the SPT switchover mechanism, PIM-SM also supports the construction and use of SPT (S,G) trees, but in a more economical way than the PIM-DM forwarding state.

![](<../.gitbook/assets/Unknown image (1165)>)

On the CE1, CE2, PE1, and PE2 routers, define the SPT threshold as infinity. - This action should force the routers to always stay on the shared tree.

CE1# configure terminal

Enter configuration commands, one per line. End with CNTL/Z.

CE1(config)# ip pim spt-threshold infinity

CE1(config)# end

CE2# configure terminal

Enter configuration commands, one per line. End with CNTL/Z.

CE2(config)# ip pim spt-threshold infinity

CE2(config)# end

RP/0/RP0/CPU0:PE1# configure

Mon Oct 28 13:35:23.666 UTC

RP/0/RP0/CPU0:PE1(config)# router pim address-family ipv4

RP/0/RP0/CPU0:PE1(config-pim-default-ipv4)# spt-threshold infinity

RP/0/RP0/CPU0:PE1(config-pim-default-ipv4)# commit

Mon Oct 28 13:35:37.540 UTC

RP/0/RP0/CPU0:PE1(config-pim-default-ipv4)# end

RP/0/RP0/CPU0:PE2# configure

Mon Oct 28 13:36:10.255 UTC

RP/0/RP0/CPU0:PE2(config)# router pim address-family ipv4

RP/0/RP0/CPU0:PE2(config-pim-default-ipv4)# spt-threshold infinity

RP/0/RP0/CPU0:PE2(config-pim-default-ipv4)# commit

Mon Oct 28 13:36:22.360 UTC

RP/0/RP0/CPU0:PE2(config-pim-default-ipv4)# end

PIM-SM State Maintanance

In PIM-SM, join and prune state information has a normal expiration time of 3 minutes. If a periodic join and prune message is not received to refresh this state information, it automatically expires and is deleted. Therefore, a PIM router sends periodic join and prune messages to its PIM neighbors to maintain this state information.

When a join message is received from a PIM neighbor, the expiration timer of the interface (in the OIL) is reset to 3 minutes. If the interface expiration timer counts down to 0, the interface is removed from the OIL

This action may trigger a prune if the removal of the interface causes the OIL to become null (empty).

When a prune message is received in PIM sparse mode, the interface on which the prune was received is normally just removed from the OIL. The exception is the special case of (S,G) RP-bit prunes, which are used to prune (S,G) traffic from the shared tree. In this case, periodic (S,G) RP-bit prunes must be sent to refresh the prune state in the upstream PIM neighbor toward the RP.

All (S,G) entries have entry expiration timers, which are reset to 3 minutes by the receipt of an (S,G) packet via the SPT. If the source stops sending, this expiration timer counts down to 0, and the (S,G) entry is deleted.

PIM-SM State Information

Mroute table / Multicast RIB (MRIB) multicast route table, derived from the unicast routing table; PIM Multicast Forwarding Information Base (MFIB) multicast FIB equivalent

Entries in the multicast route table are composed of (\*,G) and (S,G) entries, each of which contains the following:

RPF information, consisting of an RPF interface or an IIF (incoming interface) and the IP address of the upstream RPF neighbor router in the direction of the source. With PIM-SM, the information in a (\*,G) entry points toward the RP.

OIL, which contains a list of interfaces over which the multicast traffic is going to be forwarded. Multicast traffic must arrive on the IIF (RPF interface) before it can be forwarded to these OIL interfaces. If multicast traffic does not arrive on the IIF, it is simply discarded.

There are three ways to put an interface on the OIL for a given multicast group: when a PIM join for this group is received (indicating receivers further down), when an IGMP message is received (indicating a receiver joined on this segment), or when the group is statically joined via configuration.

To keep the OIL accurate over time, PIM uses two refresh mechanisms:

Periodic (_,G) Joins reset (_,G) expiration timers, ensuring that even in the absence of data, the shared tree remains intact as long as there are interested receivers.

Data-driven (S,G) refreshes reset (S,G) timers on receipt of multicast packets, so that active sources maintain their state automatically.

| show ip mroute       |   |
| -------------------- | - |
| show mrib ipv4 route |   |

The (_, 234.1.1.1) entry that is shown in the sample output is the (_,G) entry. If there is no matching entry for a particular (S,G) entry, this entry is used to forward the 234.1.1.1 multicast group traffic down the shared tree out the Gi0/0/0/0 interface.

In the multicast routing table you can see all (\*,G) and (S,G) entries that the router is aware of.

The timers always show first the uptime, then the expiry time. The first number shows how long the entry has been in the multicast routing table; the second, when the entry will expire.

The incoming interface shows the RPF neighbor, and the interface on which the traffic is expected to arrive. The outgoing interface list shows where traffic for the given group needs to be sent to.

![](<../.gitbook/assets/Unknown image (1166)>)

PIM-SM Shared Tree Pruning

When receivers stop subscribing to multicast groups, sources stop sending, or the multicast distribution topology changes, it becomes necessary to remove links from the MDT. The concept of removing a link from the MDT is called "pruning."

Pruning can happen explicitly, by sending PIM prune messages, or implicitly, using a time-out.

In a first instance, pruning is driven by receivers leaving or timing out: When nobody is listening, traffic can be pruned back.

A single receiver leaving a group may not trigger a prune yet. But when all receivers on a segment leave a group G, then the last-hop router removes the interface from the OIL of all (\*,G) and (S,G) entries.

The router may however have several interfaces on the OIL of those entries. When the router deletes the last interface on a (_,G) entry, it has no receivers downstream anymore, and the MDT can be pruned for (_,G). It then sends a PIM-SM prune message on the incoming interface for (\*,G).

When a locally connected host sends an IGMP leave (or an IGMP state times out in the router, if IGMPv1 is used) for group G, the interface is removed from the (\*,G) and from all (S,G) entries in the multicast routing table.

The (\*,G) entry in the last-hop router does not necessarily mean that the traffic is actually flowing down the shared tree. The entry only indicates that there are active group members and that the branch of a shared tree for the active group was established. For an exact check of the traffic flow, the group counters have to be inspected.

The incoming interface in the (_,G) entry in router B is E0. The OIL contains E1, and the C flag is set in the (_,G), which denotes that there is a locally connected host for this group (Receiver A).

![](<../.gitbook/assets/Unknown image (1167)>)

![](<../.gitbook/assets/Unknown image (1168)>)

1. When the last-hop router B receives an IGMP group-leave message from Receiver A for group G, it performs the normal IGMP leave processing. It also finds that Receiver A was the last host to leave. The IGMP state for group G on interface E1 is deleted.
2. This action causes interface E1 to be removed from the OIL of the (\*,G) entry and from any (S,G) entries (in this case, there are none) in the multicast routing table.
3. Because E1 was the only interface in the (_,G) entry, its OIL becomes null, which results in a (_,G) prune sent up the shared tree via E0 toward the RP.

![](<../.gitbook/assets/Unknown image (1169)>)

4. Router A will receive a (_,G) prune message and remove interface E0 from the OIL of the (_,G) entry. Router A delayed pruning E0 from the (\*,G) entry for 3 seconds because this is a multiaccess network. As such, it needed to wait for a possible overriding join from another PIM neighbor. Because no overriding join was received, the interface was pruned.
5. Because the (_,G) OIL is now null, a (_,G) prune message is forwarded on up the shared tree (RPT) via E1 toward the RP.
6. This pruning continues back toward the RP, or until a router is reached whose (\*,G) OIL does not transition to null because of the prune.

![](<../.gitbook/assets/Unknown image (1170)>)

PIM-SM SPT Pruning

Both (\*,G) and (S,G) entries exist.

The J flag is set in the (S,G) entry. This setting indicates that the (S,G) state was created in response to the SPT threshold being exceeded and the switchover being performed.

The T flag is set in the (S,G) entry. This setting indicates that (S,G) traffic is being successfully received via the SPT.

The incoming interface is the same for the (\*,G) and the (S,G) entries. This similarity indicates that the shared tree and the SPT overlap at this branch of the distribution tree

![](<../.gitbook/assets/Unknown image (1171)>)

Both the (\*,G) and (S,G) entries exist.

The incoming interface is different for the (\*,G) and the (S,G) entries. This difference indicates that the shared tree and the SPT diverge at this point (on router A)

![](<../.gitbook/assets/Unknown image (1172)>)

1. When the last-hop router B receives an IGMP group-leave message from ReceiverA for group G, it performs the normal IGMP leave processing and finds that ReceiverA was the last host to leave. The IGMP state for group G on interface E1 is deleted.
2. This action causes interface E1 to be removed from the OIL of the (_,G) entry and from any (S,G) entries in the multicast routing table. Because E1 was the only interface in the (_,G) and the (S,G) entries, their OILs become null.
3. Because the (_,G) OIL is now null, a (_,G) prune message is sent up the shared tree via E0 toward the RP.

![](<../.gitbook/assets/Unknown image (1173)>)

4. Because the (Si,G) OIL is now null, router B stops sending periodic (Si,G) join messages up the SPT. This action results in the removal of the interfaces from the (Si,G) OIL in the upstream router A after a certain amount of time (typically three times the periodic \[Si,G] join interval).

![](<../.gitbook/assets/Unknown image (1174)>)

5. Router A will receive a (_,G) prune message and remove interface E0 from the OIL of the (_,G) entry. Router A delayed pruning E0 from the (\*,G) entry for 3 seconds because this is a multiaccess network. As such, it needed to wait for a possible overriding join from another PIM neighbor. Because no overriding join was received, the interface was pruned.
6. Because the (_,G) OIL is now null, a (_,G) prune message is sent up the shared tree via E1 toward the RP.

![](<../.gitbook/assets/Unknown image (1175)>)

![](<../.gitbook/assets/Unknown image (1176)>)

Because the IGMP leave message for group G was received on the E1 interface to the last-hop router B, the interface was removed from both the (\*,G) and (S,G) entries. This action caused the OIL to transition to null in both entries, and it set the P flag.

![](<../.gitbook/assets/Unknown image (1177)>)

In response to the (_,G) prune message received by router A, the router removed the E0 interface from the OIL for the (_,G) entry.

Additionally, the interface was removed from the (S,G) entry (the entry contains the copy of the OIL from the parent \[\*,G] entry). And because periodic (S,G) joins are no longer being sent by the downstream routers, the group expiration timer counts down, and approaches 0.

7. Because router A is no longer receiving (Si,G) join messages from router B, the (Si,G) state eventually times out.
8. This action causes an (Si,G) prune message to be sent up the SPT toward the source Si. The null OIL in the (Si,G) entry does not result in prunes being sent up the SPT. Only periodic (Si,G) joins cease. A prune is sent only if triggered by the traffic that is still flowing down the SPT or by the (Si,G) entry expiration.

![](<../.gitbook/assets/Unknown image (1178)>)

9. After the (Si,G) prune is sent by router A, the traffic stops flowing down the SPT. This result occurs because the upstream router removes the interface (on which the prune was received) from the (Si,G) OIL.

![](<../.gitbook/assets/Unknown image (1179)>)

Config

![](<../.gitbook/assets/Unknown image (1180)>)

Within the scope of the CCIE Lab Exam you will not have a multicast server or client so we have to simulate these

Multicast server will be a dedicated device who will send a ping to the multicast group address. This will simulate a stream of multicast traffic

Normally a multicast client would be a PC who wants to receive the multicast traffic. Within the CCIE Lab Exam to simulate a multicast client we will use the #ip igmp join-group on Lo int

To simulate multicast stream from the source, simply ping to the multicast group IP address from the souce node - either on cisco router or windows machine with parameter -t to send continuous pings

Example:

RP/0/RP0/CPU0:PE1(config)# router igmp

RP/0/RP0/CPU0:PE1(config-igmp)# interface Loopback0 join-group 224.1.1.1

![](<../.gitbook/assets/Unknown image (1181)>)

Troubleshooting PIM-SM

Check whether receivers are active.

Check whether a group is present at the last-hop router and the RP.

Check whether sources are active.

Check whether a group is present at the first-hop router.

Check the RP configuration on the routers between the source and the RP.

Check the RP configuration on the routers between the RP and the receivers.

Debug the creation of distribution trees.

to verify in which multicast address group is interface participating show ip int <> | i Multicast

to verify, whether the multicast is operational, you can try to ping the group IP from receiver

mtrace \<source\_address> \<group\_address>

show ip mroute

show ip pim \[ interface | neighbor | rp mapping | bsr-router | interface df ] \* indicates the active DF for given link

show ip igmp \[interface | membership | groups]

show ip rpf

show ip mrib route

show pim rpf

#### PIM snooping (Layer 2)

when you enable PIM snooping, the switch learns which multicast router ports need to receive the multicast traffic within a specific VLAN by listening to the PIM hello messages, PIM join and prune messages, and bidirectional PIM designated forwarder-election messages to reduce unnecessary multicast traffic and optimize bandwidth utilization in L2 networks

To use PIM snooping, you must enable IGMP snooping on the switch. IGMP snooping restricts multicast traffic that exits through the LAN ports to which hosts are connected.

IGMP snooping does not restrict traffic that exits through the LAN ports to which one or more multicast routers are connected

| ip pim snooping                                                                                                           | ! Enable PIM snooping globally; You do not need to configure an IP address or IP PIM in order to run PIM snooping |
| ------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| vlan configuration 10 ip pim snooping OR interface range FastEthernet0/1 - 24 switchport access vlan ip pim snooping vlan | ! Enable PIM snooping on specific VLANs                                                                           |
| show ip pim snooping                                                                                                      | ! Verify PIM snooping status                                                                                      |

### ASM vs SSM

PIM sparse and dense mode use IGMPv2 which is also known as ASM (Any Source Multicast), meaning all multicast sources are accepted

In multicast, there are two fundamental architectural methods to create a multicast distribution tree, ASM and SSM. ASM allows for more than one source and receivers join "any" source (\*,G). SSM supports a single source only per tree and receivers join that specific source and group (S,G). In ASM, the network must find the sources, which makes the network architecture more complex than in SSM. In SSM, since the receiver explicitly joins a source, the complexity on the network is significantly lower, especially for interdomain topologies.

Generally, SSM is the simpler and more optimal way to create MDT. The typical applications are audio or video streaming, where a single source sends to a group of receivers. In SSM, the MDT is always optimal in terms of path and delay. Security is higher than in ASM, because rogue senders cannot simply start streaming into a multicast group, as in ASM.

ASM allows for several sources in a single MDT, but it is more complex because it requires an RP. This RP is required for receivers to find the sources.

SSM is normally preferred, unless there are several sources, for example in a multipoint video-conferencing solution. However, for several senders the SSM keeps more states on the routers, because there is one MDT per source.

In ASM, a receiver does not join a specific source, but just a group (\*,G). Therefore, the network itself needs to find one or more sources for a given multicast group "G." ASM uses an RP for receivers to discover the sources. The creation of the MDT goes through several phases.

In the first step, receivers build a tree to the RP. This is a source-specific tree, with the RP as the (intermediate) source. It is similar to the creation of an SSM tree, but here you build a tree to the RP and not to the source.

![](<../.gitbook/assets/Unknown image (1182)>)

#### PIM Source-Specific Multicast (PIM-SSM)

SSM (Source Specific Multicast) is an PIM-SM extension that allows receivers to join to the specific group (S,G), eliminating RPs and shared trees and only builds a SPT

In an environment where a single source is sending multicast traffic, the general PIM-SM architecture is overly complex. Compared to a PIM-SM Any-Source Multicast (ASM) architecture, SSM is much simpler—It does not require rendezvous points (RP), and there is a single multicast distribution tree (MDT) for each source, thus, no need to switch between shared and source trees.

SSM is usually preferable in deployments with few sources, for example, in TV or audio streaming services. Also, interdomain is advantageous, because the group addressing does not need to be coordinated up front—Any source can use any SSM group address.

In environments with many sources, SSM would create a source tree for each source, and thus introduce more state in the network than an ASM solution. BIDIR-PIM provides a good solution for this scenario.

The prerequisite for SSM deployment is a mechanism that allows hosts to report not only the group that they want to join, but also the source for the group.

This mechanism is built into the IGMPv3 standard. With IGMPv3, last-hop routers may receive IGMP membership reports requesting a specific multicast source and group traffic flow. The router responds by simply creating an (S,G) state and triggering an (S,G) join toward the source.

Exactly how a host learns about the existence of sources may occur via directory service, session announcements directly from sources, or some out-of-band mechanisms (for example, web pages). SSM work only with IGMPv3 with pre-defined range of 232.0.0.0/8 but can be changed, if needed (SSM is automatically enabled in PIMv6 (IPv6))

Routers are prevented from building a shared tree for any of the groups from this address range. The address range 232.0.0.0/8 is assigned for global well-known sources. Implementations in routers must not build any shared tree for those groups. An SSM router must ignore all non-source-specific membership reports (such as IGMPv2 host reports) for groups in the SSM range.

SSM allows the last-hop router to immediately send an (S,G) join toward the source. Thus, the PIM-SM (\*,G) join toward the RP is eliminated, and the first-hop routers start forwarding the multicast traffic on the SPT from the very beginning. The SPT is built by receiving the first (S,G) join.

SSM must only be configured on the edge router nearest to the receiver and since the application already knows the source, there is no need for an RP in the network, therefore each tree is a SPT and never a RPT

SSM is suitable for use when there are well-known sources either within the local PIM domain or within another PIM domain. The MSDP, which is needed for interdomain multicast routing when regular PIM-SM is used within a domain, is no longer needed for SSM.

There are several benefits of immediately building SPTs to a well-known source without the need for first building a shared tree. One of the main benefits concerns address management. Traditionally, it was necessary to acquire a unique IP multicast group address to ensure that a content source (such as a streaming video broadcast of an event) would not conflict with other possible sources sending on the shared tree

In SSM, a unique IP multicast group address is no longer necessary. Traffic from each source is uniquely forwarded using only an SPT. Thus, different sources may use the same SSM multicast group addresses without concern about intermixing traffic flows.

For instance, the SSM channel (192.168.45.7, 232.7.8.9) is different than (192.168.3.104, 232.7.8.9), and hosts subscribed to one will not receive traffic from the other

Note Source-specific groups may coexist with other groups in PIM-SM domains.

Operation

1. The receiver learns about the source and group address out-of-band (OOB), from a web page, an application, or similar.
2. The application in the receiver then sends an IGMPv3 membership report to the last hop router. In this request, it specifies the specific (S,G) that it wants to receive traffic from. Note that IGMPv2 does not support subscribing to a specific source and therefore cannot be directly used for SSM. A workaround for this issue is SSM mapping.
3. PIM join messages are sent hop by hop, to "all PIM routers" in a multicast group 224.0.0.13. The next hop is determined by a lookup in the unicast routing table. This method will send PIM join messages hop by hop, until the first hop router is reached. This leads to the optimal path of the source (as defined in the unicast table), while on each router on the way—multicast forwarding state is established.
4. The source sends its multicast stream to the first hop router.

Other receivers follow the same procedure. When two paths come together in the network (in the figure on router C), this router notes in its multicast forwarding table that it has two outgoing interfaces for a given (S,G).

A receiver can stop traffic by sending a "leave" message. If the receiver does not respond to IGMP membership queries anymore, the multicast stream will eventually time out. When a last-hop router has no more receivers, it sends a PIM prune message upstream to stop receiving the stream.

![](<../.gitbook/assets/Unknown image (1183)>)

#### SSM mapping

SSM mapping supports SSM transition in cases where IGMPv3 is not available, or when supporting SSM on the end system is impossible or unwanted because of administrative or technical reasons.

SSM mapping enables you to leverage SSM for video delivery to existing set-top boxes that do not support IGMPv3. The mapping can also be used for applications that do not take advantage of the IGMPv3 host stack.

SSM mapping introduces a means for the last-hop router to discover the sources sending to the groups. When SSM mapping is configured, if a router receives an IGMPv1 or IGMPv2 membership report for a particular group, the router translates this report into one or more (S,G) channel memberships for the well-known sources associated with this group. SSM mapping enables the last hop router to determine the source addresses either by a statically configured table on the router or by consulting a DNS server. When the statically configured table is changed, or when the DNS mapping changes, the router will leave the current sources associated with the joined groups.

SSM mapping only needs to be configured on the last-hop router connected to receivers. No support is needed on any other routers in the network.

There are two ways to configure SSM mapping: static, and Domain Name System (DNS)-based.

In static SSM mapping, the last-hop routers are statically configured with a static mapping between group and source (G > S).

The static SSM mapping configuration command is shown in the following table:

Parameters

ip igmp static ssm-map group-list ssm-source

The group-list is a standard access control list (ACL) listing all the groups that a particular source is sending to.

The ssm-source is the IP address of the source sending to those groups.

Now when a last-hop router receives an IGMPv2 (\*,G) membership report (instead of an IGMPv3 \[S,G]) it looks up the group in the ACL, and if it finds that group, it uses the source that is specified in the command.

A more dynamic way to map multicast groups to sources is dynamic SSM mapping. This method uses a DNS server to find one or more sources for a given group (list).

Configuring SSM

Use the Cisco IOS/IOS XE ip pim ssm default | range command to define the SSM range of IP multicast addresses. The default range is 232.0.0.0/8. When an SSM range of IP multicast addresses is defined, no MSDP SA messages will be accepted or originated in the SSM range.

![](<../.gitbook/assets/Unknown image (1184)>)

### PIM bidirectional (BIDIR-PIM)

PIM-SM is unidirectional in its native form. The traffic from sources to the RP initially flows encapsulated in register messages.

This activity presents a significant burden because of the encapsulation and de-encapsulation mechanisms. Additionally, an SPT is built between the RP and the source, which results in (S,G) entries being created between the RP and the source, which takes up control plane space as in the regular PIM-SM - 2 entries: (\*,G) and (S,G)

Several multicast applications use a many-to-many multicast model where each participant is a receiver as well as a sender. The (\*,G) and (S,G) entries appear at points along the path from participants and the associated RP. Additional entries in the multicast routing table increase memory and CPU utilization. An increase of the overhead may become a significant issue in networks where the number of participants in the multicast group grows quite large.

A good example is stock-trading applications where thousands of stock market traders perform trades via a multicast group. BIDIR-PIM eliminates the registration/encapsulation process and the (S,G) state. Packets are natively forwarded from a source to the RP using the (_,G) state only. This capability ensures that only (_,G) entries appear in multicast forwarding tables. The path that is taken by packets flowing from the participant (source or receiver) to the RP and from the RP to the participant will be the same by using a bidirectional shared tree.

PIM bidirectional does not use the PIM register / register-stop mechanism to register sources to the RP. Each source is able to start sending to the source whenever they want

When the multicast packets arrive at the RP they will be forwarded down the shared tree (if there are receivers) or dropped (when we don’t have receivers)

Unlike PIM-SM normal operations, RP does not need to be in that data path but just must be reachable by all multicast routers

PIM bidirectional has removed RPF and replaced it with Designated Forwarder (DF), which establishes a loop-free SPT rooted at the RP.

To be more precise, a designated forwarder is elected on each link for a specific RP. If more than one RP is in use, each RP will result in an RP-specific designated forwarder on each link.

The DF assumes the role of a designated router and has the following responsibilities:

It is the only router that forwards packets traveling downstream (toward receiver segments) onto the link.

It is the only router that picks up upstream-traveling packets (away from the source) off the link and forwards them toward the RP.

DF is multicast router that is able to forward (\*,G) state in 2 different directions (bidirectional) for same group address

A single DF for a particular BIDIR-PIM group exists on every link within a PIM domain. Elected on both multiaccess and point-to-point links, the DF is the router on the link with the best unicast route to the RP. If the metric is equal then the router with the highest IP address will become the DF.

If the elected DF fails, it is detected via the normal PIM hello mechanism, and a new DF election process will be initiated.

Designated Forwarder Election Messages

In the designated forwarder election process, the routers use messages to negotiate their respective role. All messages are for a single link, and for a single RP, and sent in PIM control messages. These message types are the following:

Offer: The router advertises its own local metrics to reach the RP.

Winner: The router asserts its status as a designated forwarder (At the initial election, and later it re-asserts the winner status.)

Backoff: The router sees that another router has better metrics and tells every other router to hold for a backoff time. The designated forwarder maintains its role as a designated forwarder during this time.

Pass: An acting designated forwarder sees a better metric from another router and passes its responsibility to a better candidate. The old designated forwarder stops the role of the designated forwarder at this point, and the new one assumes it.

![](<../.gitbook/assets/Unknown image (1185)>)

Operation

Initially, the routers responsible for sending (\*,G) joins toward the RP and the routers responsible for forwarding group traffic toward the RP have to identify the group as bidirectional. This identification may be accomplished statically or dynamically via Auto-RP or the BSR mechanism. In PIMv2, routers discover that a group operates in bidirectional mode from the encoded-group address fields in PIM bootstrap and candidate-RP advertisement messages. The encoded-group address field has the bidirectional bit set. Regular PIM-SM groups may coexist with bidirectional groups.

When a router receives a join message for a bidirectional group G, it must determine whether it is the DF on the link for this group. The router either inspects the (_,G) state or the RP DF election information when there is no (_,G) entry. If the router is the DF for the group, it follows the standard procedure for (\*,G) joins. If the router is not the DF for the group, it ignores the received join.

By forwarding (\*,G) joins toward the RP, the network establishes branches of a shared tree. When sources start sending, their traffic is forwarded natively hop by hop toward the RP and further down established shared trees.

Regardless of how many sources send the traffic for a certain group, only one (\*,G) entry is created and used for this group.

For BIDIR groups, since the tree is used in both directions, there is no "incoming interface," but instead a "bidirectional upstream" interface, always pointing to the RP. There is an Outgoing Interface List (OIL) just like in PIM-SM. The interface on which traffic from source A is received is added to the OIL, in this case on router DF.

Regardless of the number of sources or receivers, there is only one (\*,G) entry for each bidirectional group, which is the reason why bidirectional is preferable with many sources—It creates significantly less state than PIM-SM or PIM-SSM.

BIDIR-PIM router must forward the packet if either of these conditions exists:

The packet is received on the IIF (incoming interface) of the entry. This activity will prompt the router to forward downstream-traveling packets. This is the same procedure that exists with the regular PIM-SM model.

The router is the DF for the respective RP and group for the interface on which the packet was received (only the DF forwards upstream).

When the packet is forwarded, it is forwarded on all the interfaces in the OIL (outgoing interface list). Additionally, the packet is forwarded on the IIF toward the RP, excluding the interface on which the packet was received.

The two conditions that are benefits of BIDIR-PIM (Bidirectional PIM) are:

There are no (S,G) states. In BIDIR-PIM, multicast distribution trees are built based on the rendezvous point (RP) and the group address. Source-specific (S,G) states, which track the path from a specific source to a specific group, are not maintained. This simplifies the multicast routing table and reduces state overhead on the routers.

The first packets from the source are not encapsulated. Unlike some other multicast protocols or modes, BIDIR-PIM typically forwards the initial multicast packets natively towards the RP without requiring encapsulation. Encapsulation might be used in specific scenarios or for inter-domain communication, but it's not a fundamental requirement for the initial flow from the source to the RP in the BIDIR-PIM domain.

Forwarding and Tree Building Process

1. The DF is responsible for sending (_,G) joins toward the RP for the active bidirectional group. Downstream routers address their (_,G) joins to upstream DFs. This is accomplished by putting the IP address of the upstream DF in the upstream router field of a PIM join message.
2. When the DF receives a (_,G) join, it adds the link to the OIL of the (_,G) entry and joins toward the RP. If the interface exists in the OIL, the interface timer is refreshed.
3. The DF has the responsibility of forwarding multicast traffic in BIDIR-PIM. When multicast traffic is received on a link for which the router is the DF, the router must forward that traffic via its RPF interface toward the RP. Furthermore, the DF must forward the received traffic out all other interfaces in the (\*,G) OIL, excluding the interface on which the traffic was received.

![](<../.gitbook/assets/Unknown image (1186)>)

Restrictions

BiDir in conjunction with Anycast RP is not possible, since Anycast RP relies on sharing (S,G) pairs.

Important: Enabling Bidirectional PIM disables SPTs and SSM!

There is however no way for the RP to tell the source to stop sending multicast traffic. The RP should be placed in the middle between the sources and receivers in the network.

Configuring Bidirectional PIM

The Cisco IOS/IOS XE ip pim bidir-enable command enables BIDIR-PIM on the router. The Cisco IOS XR router has BIDIR-PIM enabled by default. A router with BIDIR-PIM enabled will send PIM hello messages with the bidirectional mode option. When BIDIR-PIM is enabled, bidirectional PIM can be configured with static RP or dynamic RP methods, as shown on the figure.

![](<../.gitbook/assets/Unknown image (1187)>)

Note the flag D is set for the entry. The flag indicates that traffic will be dropped for this entry.

### Multicast security and boundaries

can be defined to prevent Auto-RP messages from entering the PIM domain, by creating an ACL to deny packet destined for .39 and .40, which carry Auto-RP information

Configured on interfaces to limit the scope of a multicast domain This allows the same multicast group addresses to be reused in a different administrative domain

Configuration considerations:

IPv4 configuration:

Configuration is done per interface with an ACL and can be inbound, outbound or both ways.

The permitted IP addresses within the ACL are the multicast groups and not the multicast source addresses.

IPv6 configuration:

Configuration is done per interfaces with different options:

With the block source keyword all incoming multicast traffic on an interface will be blocked. The implementation is most commonly implemented at the first-hop router.

With the scope keyword the behavior depends on the interface:

RPF interface: Packets are not accepted that belong to scopes less or equal the configured one

Outgoing interface: Packets are not forwarded that belong to scopes less or equal the configured one

Additionally PIM Register/BSR messages are also not accepted for multicast group addresses that belong to scopes less or equal the configured one.

| access-list 1 permit 238.0.0.0 0.255.255.255 ip pim send-rp-announce loop0 scope 2 group-list 1                                                                                                                                   | We can make RP candidate router announce its self as RP for specific multicast group address                                                                                                                                                                                                                                                                                                                                                                                            |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ip pim rp-announce-filter rp-list 1 group-list 2 access-list 1 permit 10.0.0.1 access-list 2 permit 224.0.0.0 15.255.255.255 ip/ipv6 pim bsr-candidate accept-rp-candidate \[ACL] Router(config-if)# \[ip \| ipv6] pim bsr border | ip pim rp-announce-filter prevents unwanted candidate RP announcement messages from being processed by the mapping agent accept announcements from RP addresses 10.0.0.1, where group-list specifies the announcements for specified groups Important: If the local router is an AutoRP mapping agent + RP candidate or BSR candidate + BSR RP candidate at the same time, it must be included in the ACL! Configured on interfaces towards other routers to disallow BSR advertisement |
| int Gi0/0 ip pim neighbor-filter 1 ip multicast boundary 1 ipv6 multicast boundary \[ block source \| scope ]                                                                                                                     | to prevent PIM neighborship for specified PIM neighbor IP IP Multicast Boundary - allows to keep PIM neighbor relationship, but restricts RP announcements to his neighbor scope keyword specifies the TTL of the multicast, which should be tailored according to the number of hops in the multicast domain                                                                                                                                                                           |
| no ip pim dm-fallback                                                                                                                                                                                                             | Note: PIM-DM will be used if we typed “ip pim sparse-dense-mode” for groups with no RP, we can disable this behavior using “no ip pim dm-fallback”                                                                                                                                                                                                                                                                                                                                      |
| ip pim register-rate-limit 10                                                                                                                                                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| R3(config-if)#no ip mroute-cache R3#debug ip mpacket                                                                                                                                                                              | If you want to debug multicast traffic you have to disable multicast route caching on the required interface and use no logging console and logging buffered debuggin                                                                                                                                                                                                                                                                                                                   |
| (config-if)#ip pim passive                                                                                                                                                                                                        | command will cause an interface to not send out or accept any PIM messages from other routers. The router will instead consider that it is the only PIM router on the network, and thus act as the D                                                                                                                                                                                                                                                                                    |

### RP high availability and discovery

#### Static RP disadvantages

Addresses that are within the 224.0.0.0 to 224.0.0.255 range are considered as link-local multicast addresses. Packets that are multicast to these addresses are always transmitted with a TTL of 1, and they are not forwarded.

Manual RP information configuration does not scale to large environments. There are several problems that are associated with its static nature:

The initial configuration of many routers is cumbersome.

Any change to the static configuration has a negative impact on the maintenance effort, which in the long term turns out to be even more troublesome than the initial provisioning.

Static RP information configuration does not support many scenarios with redundant RPs, in which the network reachability changes, as in the redundant setup in the following figure.

In the preceding figure, the enterprise has two uplinks to two service providers that offer multicasting services. The providers multicast to the same groups but have independent sources and RPs. The enterprise has PIM and Border Gateway Protocol (BGP). These protocols are peering with both service providers to exchange routing information and build multicast distribution trees. SP1 is the primary provider, while SP2 is the backup, from which the multicast traffic should be received only if the connection to SP1 fails. Static RP information configuration does not support such requirements, because the RP of SP1 has a different address than the RP of SP2. If the RP of SP1 were configured on the enterprise routers, the RP selection would never fall back to the RP of SP2, if the primary link went down.

![](<../.gitbook/assets/Unknown image (1188)>)

Manual RP information configuration does not scale to large environments.

RPs can be almost anywhere. Theoretically, they do not even need to be on the path from the sources to the receiver. On the Cisco IOS, IOS XE, and IOS XR Software platforms, the default SPT threshold is set to 0. This default means that the distribution trees immediately switch to SPT, and the traffic itself does not flow via the RP.

For other values of the SPT threshold, especially infinity, a placement closer to the source is recommended, as the RP can become a congestion point.

In principle, the RP can be anywhere in the multicast domain, however:

If the Shortest Path Tree (SPT) threshold is 0, which is the default on Cisco IOS and Cisco IOS XR:

Last-hop router will switch to SPT right away (and away from RP)

Traffic does not flow through the RP, except the first packets.

RP position not overly important. It is used to connect new sources and new receivers, but not for the actual traffic.

If the SPT threshold is high or infinite:

Multicast traffic goes through the RP

Placement more important: Best close to sources.

The RP can become a congestion point.

In addition to RP high-availability options, Dual Route Processor platforms have some special features for multicast high-availability:

Multicast Cisco Nonstop Forwarding with Stateful Switchover (SSO): Allows forwarding of multicast traffic during the switchover.

Protocol Independent Multicast (PIM) triggered joins: Improves PIM convergence after a switchover by triggering adjacent PIM neighbors to resend join messages.

#### Auto-RP

(also referred to as PIM v1) is a Cisco proprietary mechanism that automates the distribution of group-to-RP mappings in a PIM network.

Can use multiple RPs within a network to serve different group ranges. Allows load splitting among different RPs as well as providing backup to each other

Candidate RPs (C-RPs) advertises its willingness to be an RP via RP announcement messages using announce group (S,224.0.1.39) that are sent each 60s with groups they are willing to service.

RP announcements contain the default group range 224.0.0.0/4. Hold time is 3x announce interval - 180s. If multiple RPs advertise the same group range, the C-RP w highest IP is elected

Mapping Agents (MP) listens for (\*,224.0.1.39) to learn about Candidate RPs and store the information contained in the RP announcements in a group-to-RP mapping cache, along with hold times.

It saves the announcement in the group-to-RP mapping cache.

It selects the candidate RP with the highest IP address as the RP for the group range.

The hold times are used to expire an entry in the cache if a candidate RP fails and is no longer sending periodic candidate-RP announcements. Via RP-discovery messages, the mapping agent periodically sends information on the elected RPs from its group-to-RP mapping cache to all routers in the network. RP-discovery messages are multicast to the Auto-RP-discovery group (224.0.1.40). They are sent every 60 seconds or when a change to the information in the group-to-mapping cache takes place.

All PIM-enabled routers join 224.0.1.40 and store the RP mappings in their private cache - No configuration is required to join this group. Multiple RP MAs can be configured in the same network to provide redundancy in case of failure.

There is no election mechanism between them, and they act independently of each other, they all advertise identical group-to-RP mapping information to all routers in the PIM domain

![](<../.gitbook/assets/Unknown image (1189)>)

Chicken-and-Egg Issue in PIM-SM Auto-RP design

In PIM-DM, routers flood multicast traffic to all parts of the network by default.

When Auto-RP is configured on routers in PIM-DM, they automatically join the designated Auto-RP multicast groups (224.0.1.39 and .40) and start participating in the Auto-RP process.

Routers in PIM-DM discover the RP through the Auto-RP process, where routers send Auto-RP announcements, and the one with the highest priority becomes the RP.

However in the PIM-SM, routers expect explicit joins, they don't automatically start to flood the Auto-RP multicast groups .39 and .40, thus they are unable to discover the RP.

To fix this we have to configure the #ip pim autorp listener command. This command makes routers behave as if they are in Dense Mode for the Auto-RP multicast groups .39 and .40. Once the Auto-RP process is completed and RP with the highest IP elected, the ip pim autorp listener command switches them back into Sparse mode.

Or we can enable PIM Sparse Dense Mode where routers will use sparse mode if possible, if there is no RP, routers will use dense mode

Auto-RP requires the flooding of rp-announce and rp-discovery messages in PIM dense mode across the entire multicast domain. This requirement has several important and not obvious consequences:

Today, PIM dense mode is generally not recommended, because it is very resource-intensive. Therefore, normal deployments, by default, do not (and should not) enable PIM dense mode.

However, to use Auto-RP, dense mode must be enabled. There are two ways of enabling it:

Configure all interfaces in the network with pim sparse-dense, and thus enabling dense mode. This approach is not recommended, because also normal multicast groups can then fall back to dense mode under specific circumstances. This fallback can again be controlled with specific commands, like no ip pim dm-fallback, but the overall solution becomes more complex than necessary. It is much easier to use the ip pim autorp listener command.

Configure ip pim autorp listener on all multicast routers in the domain. This command explicitly forwards all rp-announce and rp-discovery messages in dense mode, on all multicast enabled interfaces. Other multicast groups cannot fall back to dense mode with this configuration.

Cisco IOS XR does not support PIM dense mode. Auto-RP messages require a dense mode, which might lead to the impression that Cisco IOS XR could not support Auto-RP. However, there is an exception for Auto-RP messages. Those messages are forwarded in dense mode automatically, without any specific configuration. So, Cisco IOS XR forwards 224.0.1.39 and 224.0.1.40 in dense mode, but no other groups.

Since dense mode traffic gets forwarded whether there are receivers, dense mode traffic risks also leaving the administrative domain, and "leak" to other domains. It is therefore important to configure multicast boundaries when using Auto-RP (Although it is always recommended to do so, also for sparse mode traffic.)

Config

![](<../.gitbook/assets/Unknown image (1190)>)

On the candidate RPs, a loopback interface should be configured for this function. This loopback address must be routed and reachable in the interior gateway protocol (IGP) area. The candidate RP function itself is then configured with these commands:

Cisco IOS Software:

ip pim send-rp-announce interface scope \[1-255] \[group-list \[1-99]]

Cisco IOS XR Software:

router pim

auto-rp candidate-rp interface scope \[1-255] \[group-list access-list-name] \[interval seconds]

Configuration of Other Routers

All other routers in the network need to do the following two things:

Listen to the rp-discovery packets on group 224.0.1.40, which all Cisco routers do by default.

Forward them across all other multicast-enabled interfaces in dense mode. This action is done by Cisco IOS XR routers by default, but not Cisco IOS routers.

The ip pim autorp listener command is misleading, because all Cisco routers by default listen to the Auto-RP packets. 224.0.1/24 is considered as control traffic and therefore is automatically listened to. So, what does the command do then? In fact, it does not enable the listening, but the forwarding of the Auto-RP messages.

On Cisco IOS Software, the command enables the forwarding of 224.0.1.39 and 224.0.1.40 in dense mode on all multicast enabled interfaces.

On Cisco IOS XR Software, the command is not necessary. Cisco IOS XR Software automatically forwards those two groups in dense mode.

Configuration of Mapping Agent Filters

By default, mapping agents will listen to anyone announcing on 224.0.1.39. It is, therefore, possible that a fake router or host announces itself as an RP and potentially affects multicast distribution. You will learn later, that a multicast boundary should always be configured at the edge of the network—this command would filter all multicast traffic from the "outside," including rp-announce and rp-discovery messages.

The command that is shown in the following figure is an extra safe-guard. It also protects from fake RPs inside the domain.

To prevent mapping agents from listening to fake RPs, mapping agents should always define in an ACL the valid RPs and which groups the RPs are allowed to announce. This is done with the ip pim rp-announce-filter rp-list access-list group-list access list command. The first ACL defines the valid RPs and the second ACL defines the valid groups.

Note This command is not available on Cisco IOS XR.

Auto-RP Scoping

Care must be taken in the selection of the TTL scope of RP-announcement messages that are sent by candidate RPs to ensure that the RP announcement messages reach all the required mapping agents.

Care must also be taken in the selection of the TTL scope of RP-discovery messages that are sent by mapping agents to ensure that the messages reach all PIM-SM routers in the network.

In the figure, an arbitrary scope of 16 was used in the Cisco IOS and IOS XE ip pim send-rp-announce command or the Cisco IOS XR auto-rp candidate-rp router PIM command on the candidate-RP router. However, the maximum diameter of the network is greater than 16 hops, and in this case one mapping agent (router B) is farther away than 16 hops. As a result, this mapping agent does not receive the RP-announcement messages from the candidate RP. This lapse can cause the two mapping agents to have different information in their group-to-RP mapping caches. If this event occurs, each mapping agent will advertise a different router as the RP for a group, which will have disastrous results.

Also, the candidate RP is fewer than 16 hops way from the edge of the network. This arrangement can cause RP-announcement messages to leak into adjacent networks and produce Auto-RP problems in those networks.

![](<../.gitbook/assets/Unknown image (1191)>)

This lapse can result in the router having no group-to-RP mapping information. If this event occurs, the router will attempt to operate in dense mode for all multicast groups while other routers in the network work in sparse mode.

Also, the mapping agent is fewer than 16 hops away from the edge of the network. This arrangement can cause RP-discovery messages to leak into adjacent networks and produce Auto-RP problems in those networks.

![](<../.gitbook/assets/Unknown image (1192)>)

The multicast boundary command blocks the specified multicast groups in both directions, from entering or leaving the network. It is configured on an interface.

On Cisco IOS, it is configured with the ip multicast boundary access-list command, where the access list denies the multicast groups that are not supposed to traverse the boundary—in this case (at least) 224.0.1.39 and 224.0.1.40.

On Cisco IOS XR, the boundary access-list-name command is under the interface configuration in the multicast routing section.

Note

It is best practice to configure a multicast boundary for all multicast groups that are not required across domains. Therefore, the multicast boundary should, if possible, list explicitly the groups that are allowed cross-domain and deny everything else. Remember the multicast boundary works in both directions: Inbound and outbound.

![](<../.gitbook/assets/Unknown image (1193)>)

Announcing Specific Groups at the Auto-RP Boundary

In case where RPs are outside the administrative domain, you cannot filter all multicast traffic at the domain boundary. However, using the commands introduced before, you can restrict which RPs are permitted. It is also possible at the domain boundary to specify for Auto-RP packets which groups are allowed.

Use the following command to filter Auto-RP packets:

interface GigabitEthernet0/0

ip multicast boundary acl filter-autorp

!

access-list acl permit 224.1.1.1

Auto-RP Troubleshooting

To check the mapping agent functionality, use the show ip pim rp mapping command. It is issued on the mapping agent itself.

#### Bootstrap Router (BSR) (PIMv2)

Is open-standard replacement for the Cisco Auto-RP

BSR is similar in its behavior to Auto-RP. Like in Auto-RP, an independent router decides between RPs, here it is called the bootstrap router. A single router is elected as the BSR from a collection of candidate BSRs. If the current BSR fails, a new election is triggered. The election mechanism is preemptive based on the priority of the BSR candidate.

Whereas in Auto-RP two special multicast groups are used for the protocol (224.0.1.39 and 224.0.1.40), BSR uses the PIM protocol (224.0.0.13) as a control plane.

BSR also gives the possibility to configure a priority for candidate RPs, whereas Auto-RP decides purely on the higher RP address.

The keys for the BSR protocol are the PIMv2 BSR messages. They contain all information necessary for all the various steps of the process. Therefore, there is only one message type for bsr-messages, and one message from candidate RPs, which is sent unicast to the elected BSR router.

All configured BSRs send regular BSR messages. Those messages are normal PIMv2 messages, sent to 224.0.0.13, like all PIMv2 messages, with a TTL of 1 (all packets in the 224.0.0/24 range have TTL 1 and are not forwarded). Therefore, there is no special distribution method needed as in Auto-RP—the BSR information is propagated within standard PIM.

The BSR messages contain:

The address of the BSR sending the message and its priority.

All the known RPs, their respective priority, and the groups they are responsible for

Depending on the state of the negotiation, BSR messages may not yet contain all relevant information.

BSR uses two roles:

Candidate BSR: this is the router that collects information from all available RPs in the network and advertises is throughout the network. It’s function is similar to the mapping agent in AutoRP.

Candidate RP: these are routers that are advertising themselves who want to become the RP.

![](<../.gitbook/assets/Unknown image (1194)>)

BSR Operation

1. Each Candidate-BSR sends Bootstrap messages (BSMs) that contain a BSR Priority field to elect BSR

Higher-priority C-BSR suppresses lower-priority BSMs, the remaining C-BSR becomes the elected BSR.

2. Candidate-RPs send periodic C-RP-Adv messages to the elected BSR. C-RP-Adv includes priority and advertised group ranges. Routers can be BSRs and RP simultaneously

From the priority in incoming BSR messages, each router decides what to do:

If the priority of another BSR candidate is higher, it backs off.

If the priority of another candidate is lower, it elects itself as the elected BSR.

If the priority is the same, it decides on the IP address (like in Auto-RP)—higher address wins.

Now, one router has become the elected BSR, and now labels all its messages as such. The RP candidates have been waiting to see messages from an elected BSR.

RP candidates have been seeing BSR messages from BSR candidates so far, and not responded. They have been waiting to see messages from an elected BSR.

When candidate RPs see BSR messages from an elected BSR, they respond unicast to this BSR, with their role as candidate RP and all group information they are configured with.

Now the elected BSR has all the information about all the Candidate RPs in the multicast domain. It now chooses a best RP per group via a selection algorithm. The selection algorithm is not detailed here.

3. BSR selects a subset of C-RPs to form the RP-Set and load balance it - BSR can be used with the Hash fucntion to balance the load across multiple RPs.
4. BSR includes RP-Set info in Bootstrap messages, and floods them throughout the domain ensuring rapid RP-Set dissemination.

PIM routers use this info for group-to-RP mappings and multicast tree construction.

All candidate BSR routers keep sending the candidate BSR messages, as before. Uninterrupted sending allows the quick re-election of a new elected BSR router, in case the current elected router becomes unavailable, and does not send updates anymore. Since there is no response to any of those BSR messages, failover between elected BSRs is based on time-out.

BSR messages are sent every 60 seconds by default, or when a change occurs.

Sparse Mode with Bootstrap Router Example

There is no specific configuration required on all other routers, except the enabling of multicast globally and on the respective interfaces. Since BSR messages are normal PIM messages, a router will interpret them on any multicast enabled interface.

| ip multicast-routing ! interface GigabitEthernet0/0/0 ip address 172.69.62.35 255.255.255.240 ip pim sparse-mode ! interface GigabitEthernet1/0/0 ip address 172.21.24.18 255.255.255.248 ip pim sparse-mode ! interface GigabitEthernet2/0/0 ip address 172.21.24.12 255.255.255.248 ip pim sparse-mode ! ip pim bsr-candidate GigabitEthernet2/0/0 30 10 ip pim rp-candidate GigabitEthernet2/0/0 group-list 5 access-list 5 permit 239.255.2.0 0.0.0.255 | The RP candidates are configured like this: Cisco IOS Software: ip pim rp-candidate interface Cisco IOS XR Software: router pim bsr candidate-rp ip-address The BSR candidates are configured like this: Cisco IOS Software: ip pim bsr-candidate interface Cisco IOS XR Software: router pim bsr candidate-bsr ip-address |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Constraining BSR Messages

Since RPs are typically used within a multicast domain, and not from the outside, also the BSR process should normally stay within the multicast domain.

For this purpose, on each boundary router, on each outside-facing interface, the bsr-border command should be applied. This command stops BSR messages from entering or leaving the domain, while other multicast traffic, and other PIM information, may continue to flow.

If no multicast is exchanged on an external-facing interface, PIM should not be enabled on that interface in the first place.

To see the list of candidate BSRs, and the elected BSR, use the show pim bsr election command. It shows the list of candidate BSRs, and the elected BSR, along with uptime, BSR timer, and priority:

RP/0/RSP0/CPU0:PE5# show pim bsr election

PIM BSR Election State

Cand/Elect-State Uptime BS-Timer BSR C-BSR

Elected/Accept-Pref 00:35:09 00:00:05 10.5.1.1 \[1, 30] 10.5.1.1 \[1, 30]

To view the RP cache, which contains all the available RPs, use the show pim bsr rp-cache command.

RP/0/RSP0/CPU0:PE5# show pim bsr rp-cache

PIM BSR Candidate RP Cache

Group(s) 224.0.0.0/4, RP count 1

RP-addr Priority Holdtime(s) Uptime Expires

10.6.1.1 0 150 00:35:34 00:01:55

#### Anycast RP

allows multiple RPs to share the same IP address on loopback interfaces (with /32 mask), for multicast sources and receivers to dynamically discover and select the closest RP in terms of routing distance, providing redundancy and load balancing. Originally developed for interdomain multicast applications, MSDP used for Anycast RP is an intradomain feature

Because a source may register with one RP and receivers may join to a different RP in a different AS, a method is needed for the RPs to exchange information about active sources.

Without MSDP however, receiver 2 in the example may choose RP2 based on IGP proximity. If source 1 has chosen RP1, the two could never join the same Multicast Distribution Trees (MDT). Therefore, MSDP is required to do Source Active (SA) announcements between RPs. This way both RPs know mutually the sources on the other RP and can construct a joint MDT.

The normal PIM sparse mode (PIM-SM) RP behavior causes the RP to join the source tree of active sources in the entire network, as necessary.

All Anycast RPs are tied together via MSDP peering sessions. Those do not have to be a full mesh. The Cisco IOS and Cisco IOS XE ip msdp originator-id command or the Cisco IOS XR originator-id router MSDP command is used to control the IP address that is sent in SA messages. This precaution is taken to disambiguate which RP originated the SA message. If this action were not taken, all RPs would originate SM messages using the same IP address.

This information exchange is done with MSDP. The MSDP peering address must be different than the Anycast RP address (the loopback /32 IP)

In below scenatio the source creates the (S,G) SPT only with RP1, making the (S,G) entry unavailable for the RP2 (having only (\*,G)). So the MSPD peer RP1 provides the (S,G) mroute entry

Many routing protocols choose the highest IP address on loopback interfaces for the Router ID. A problem may arise if the router selects the Anycast RP address for the Router ID. We recommend that you avoid this problem by manually setting the Router ID on the RPs to the same address as the MSDP peering address (for example, the loopback 1) to the RP2, making the multicast available for his receiver

Config

Each RP is configured with two loopback interfaces:

One for the anycast RP address (the same address on all RPs, a /32 host prefix). This is Loopback 1 in the configuration example in the preceding graphic.

One as a source for the MSDP peering, with different IP addresses. This is Loopback 0 in the configuration example in the preceding graphic.

MSDP is configured as usual, using the corresponding loopback address as an MSDP source address. (Again, MSDP requires unique source addresses for each peer!)

The MSDP originator ID is set to the unique loopback address (not to the anycast loopback).

And finally, the RP address is configured to be the anycast address. This has to happen on all routers of the multicast network.

Enable multicast routing for the Loopback1 interface on the P1 router. Enable PIM on the interface and configure the IP address 10.9.9.9 as the RP router.

RP/0/RP0/CPU0:P1(config)# multicast-routing address-family ipv4

RP/0/RP0/CPU0:P1(config-mcast-default-ipv4)# interface Loopback1 enable

RP/0/RP0/CPU0:P1(config-mcast-default-ipv4)# exit

RP/0/RP0/CPU0:P1(config-mcast)# exit

RP/0/RP0/CPU0:P1(config)# router pim address-family ipv4

RP/0/RP0/CPU0:P1(config-pim-default-ipv4)# interface Loopback1 enable

RP/0/RP0/CPU0:P1(config-pim-default-ipv4)# rp-address 10.9.9.9

RP/0/RP0/CPU0:P1(config-pim-default-ipv4)# commit

Sun Nov 17 19:48:55.838 UTC

RP/0/RP0/CPU0:P1(config-pim-default-ipv4)# end

Configure a new Loopback interface on the P1 router. Use the interface ID 1 and configure IP address 10.9.9.9/32. Add the interface into the OSPF Area 0.

RP/0/RP0/CPU0:P1(config)# interface Loopback1

RP/0/RP0/CPU0:P1(config-if)# ipv4 address 10.9.9.9/32

RP/0/RP0/CPU0:P1(config-if)# exit

RP/0/RP0/CPU0:P1(config)# router ospf 1

RP/0/RP0/CPU0:P1(config-ospf)# area 0

RP/0/RP0/CPU0:P1(config-ospf-ar)# interface Loopback1

RP/0/RP0/CPU0:P1(config-ospf-ar-if)# commit

Sun Nov 17 19:42:47.838 UTC

RP/0/RP0/CPU0:P1(config-ospf-ar-if)# end

RP/0/RP0/CPU0:P1#

![](<../.gitbook/assets/Unknown image (1195)>)

Note Note that identical IP addresses in different parts of the network are an exception, which needs to be well managed. If you accidentally configure the OSPF or BGP router ID pointing to the anycast loopback interface, those protocols will break. Therefore, it is strongly recommended to use a dedicated loopback for the anycast address, and to not use that loopback for any other purpose. Also, not for the MSDP source address.

There are several ways to avoid accidental re-use of the anycast loopback address:

Use an otherwise unused loopback number, with an unusual number in your network, such as loopback99.

Configure the Anycast RP address as the lowest IP address.

Use a secondary IP address on the loopback for the Anycast IP address.

Use the router-id command in OSPF and BGP to statically configure the router ID.

Another Example config

![](<../.gitbook/assets/Unknown image (1196)>)

With more than 2 RP's

In scenarios where you have more than 2 RP's with IGP (intra-AS) instead of BGP, and since MSDP relies mainly on inter-AS with BGP, the MSDP RPF may fail

To avoid this we must configure the msdp mesh group (config)#ip msdp mesh-group

![](<../.gitbook/assets/Unknown image (1197)>)

#### Phantom RP

A Phantom RP is a virtual or non-existent RP address configured in a multicast network. These are Phantom RP characteristics:

Preventing Black Holes: Ensuring that multicast traffic is not disrupted due to RP unavailability or failure.​

Simplifying Network Designs: Allowing the network to operate without a physical RP for certain multicast groups.​

Security and Isolation: Restricting specific multicast groups from accessing a real RP.​

A Phantom RP is useful in scenarios where multicast groups or streams do not require a functional RP but still need to participate in the PIM-Sparse Mode framework.

Here are a few scenarios where Phantom RP becomes relevant:​

Multicast Group Isolation: In some cases, multicast traffic is intended to stay within a specific segment of the network. By using a Phantom RP, you can prevent unnecessary traffic from being forwarded to a central or real RP, isolating the group effectively.​

Simplified SSM: In SSM deployments, there is no need for an RP since multicast receivers already know the source IP address. By using a Phantom RP for non-SSM groups, the network avoids dependency on a real RP for those groups.​

RP Redundancy and Failover: In networks with Anycast RP configurations, a Phantom RP can act as a placeholder to ensure multicast traffic is not disrupted in the event of a failure or misconfiguration.​

Security: The Phantom RP prevents unauthorized or unexpected multicast traffic from reaching a real RP, adding a layer of security.​

High availability approach, where we create the same loopback interface on different routers with the same IP address but different subnet mask (eg. Starting with /30, then /29, …)

Advertise the created loopback interfaces into the IGP

Set the RP address to an IP address in the range of the loopback interface but NOT the actual loopback IP address itself (eg. if the loopback is 10.0.0.1/30 then the Phantom RP is 10.0.0.2)

![](<../.gitbook/assets/Unknown image (1198)>)

### Interdomain multicast routing (ASM)

Interdomain multicast routing is required when multicast sources and receivers reside in different autonomous systems and must be interconnected in a scalable and policy-controlled manner. Within a single provider domain, PIM-SM is the dominant multicast routing protocol, but PIM-SM alone is insufficient across domain boundaries because it relies on rendezvous points and does not exchange multicast-specific reachability information between autonomous systems. Service providers also do not want to depend on rendezvous points located in external or competing domains, since RP failure or policy changes outside their control can disrupt service, and they require the flexibility to place multiple RPs anywhere within their own network.

Multicast becomes more complicated across administrative domains, because of administrative controls. For example, a company may require its own RP, where it can control which devices can act as senders. This company will typically not let other companies access its RP as you have seen previously, mainly for two reasons:

Because that would allow all receivers (PCs) in another company to send traffic directly to the RP. This is a security risk, as too many receivers may also overload the RP. Between domains, the preference is for a single, well-defined interface.

The company may want to control which PCs can be senders and which ones can be receivers. This control is hard to administer across a domain.

The currently viable interdomain multicast architecture combines PIM-SM, MP-BGP, and MSDP. MP-BGP is an extension of the unicast BGP protocol that allows autonomous systems to exchange multicast-specific RPF information using multicast NLRI. This information is used solely for RPF checks and does not itself build multicast distribution trees. By maintaining a separate multicast BGP table, routers can perform RPF checks for multicast traffic independently of unicast routing, allowing multicast and unicast topologies to be incongruent and enabling different routing policies for each. This aligns well with provider operational models, since MP-BGP uses the same configuration constructs, policy mechanisms, and operational practices as unicast BGP, minimizing operational complexity and training requirements.

Because MP-BGP only supplies RPF information, an additional protocol is required to signal the existence of active multicast sources across domains. That protocol is MSDP. MSDP operates between RPs in different PIM-SM domains and allows an RP to advertise information about active sources within its domain to RPs in other domains using source-active messages. When an RP learns that a source in another domain is active for a group that has local receivers, it triggers the creation of an interdomain (S,G) join toward the source. These joins extend the source-specific shortest-path tree across domain boundaries, allowing traffic to be pulled from the upstream domain into the downstream domain where receivers exist. Full MSDP meshing is not required; it is sufficient for an RP to have connectivity into the broader MSDP mesh to achieve global source visibility.

This architecture allows each provider to maintain full control over RP placement and policy while still supporting interdomain multicast delivery. MSDP ensures that RPs are not shared across competitors, while MP-BGP ensures that multicast forwarding decisions are based on explicit, policy-driven RPF information. Together, they form the only widely deployed and operationally proven solution for Any-Source Multicast across multiple autonomous systems today.

Source-Specific Multicast represents an alternative interdomain model that eliminates rendezvous points and shared trees entirely by requiring receivers to explicitly join a specific source and group. For SSM traffic, no additional interdomain protocol beyond PIM-SM and appropriate RPF information is required, which greatly simplifies the control plane and improves scalability and security. However, SSM is restricted to the 232.0.0.0/8 address range and is suitable only when the source is well known and fixed. As a result, while SSM is operationally simpler and preferred where applicable, PIM-SM combined with MP-BGP and MSDP remains necessary to support interdomain Any-Source Multicast deployments.

SSM is the preferred way to use multicast. If, however, there is a need to run ASM between domains, the architecture becomes more complicated. ASM may be required when many sources send to a single group to avoid the creation of an MDT per source.

As a side note, the IETF does not even plan for MSDP or an equivalent protocol for IPv6. This fact shows that the IETF clearly sees SSM as the future multicast model.

![](<../.gitbook/assets/Unknown image (1199)>)

#### Multicast Source Discovery Protocol (MSDP)

MSDP is a mechanism to connect multiple PIM-SM domains. MSDP allows multicast sources for a group to be known to all RPs in different domains. Each PIM-SM domain uses its own RPs and does not have to depend on RPs in other domains. An RP runs MSDP over TCP to discover multicast sources in other domains. Only SPTs are built between domains.

If the owners of two domains could agree to have a shared RP between them, which both domains could use, there would be no need for MSDP. The need for MSDP comes from the requirement to separate control and access for RPs between domains. In reality, most domain owners want to have and control their own RP, and to secure it toward the outside of their domain.

The idea builds on the active participation of the RP inside a domain and interconnection with RPs in other domains. The RP inside a domain is aware of active groups, since shared trees for these groups are rooted at the RP. The RP also knows active sources in a domain, because they register with the RP. The sources are only known within the domain.

To propagate information outside a domain, PIM-SM must first be implemented on interdomain links to provide for (S,G) join forwarding toward sources in other domains.

Before you configure MSDP, the addresses of all MSDP peers must be known in BGP. For multicast to travel through internet all hops must run multicast, but what if RPF check for multicast source is via unicast only peer? MSDP depends on multicast MP-BGP that solves this by separating unicast RPF and multicast RPF. GRE tunnels are used to connect multicast domain over inet

MSDP relies upon BGP path information to learn the MSDP topology for the SA peer RPF check. MSDP speakers must run BGP or MP-BGP. This requirement exists because the mechanism that performs the peer RPF check in an SA message uses AS path information that is contained in the MRIB or in the URIB.

There are some special cases where the requirement to perform a peer RPF check on the arriving SA message is suspended. This is the case when there is only a single MSDP peer connection or if the default MSDP peer (default MSDP route) is configured. Also, when MSDP mesh groups are in use, the peer RPF check is skipped for SA messages that arrive from the mesh group members. In these cases, BGP or MP-BGP is not necessary.

Here, you use the IPv4 multicast table. Note that this table does not contain multicast addresses. It contains just unicast addresses, which are used for the RPF check when building the MDT. The purpose is to allow for multicast to use a different routing policy than unicast traffic.

With MSDP each domain knows about senders and receivers in other domains. With MP-BGP, routing toward the sources in other domains is possible. You still need a protocol to establish the MDT. PIM-SM is also used for this purpose between domains.

This is the same PIM-SM as used intradomain, so that the overall creation of the MDT is the same across domains. Hop by hop, the multicast tree is built toward the source. At each hop, the RPF check is being done: "Where would I send traffic unicast to the source?" And that interface is used. With the only difference that between the domains, MP-BGP provides a specific table to be used for this RPF check.

Not shown here is further path optimization, where receivers would eventually build a direct branch in the tree toward the source, potentially bypassing one or more RPs. This is standard PIM behavior, and not specific to the interdomain case.

Note

MSDP mesh group is a group of MSDP speakers that have fully meshed MSDP connectivity between one another. Any SA messages received from a peer in a mesh group are not forwarded to other peers in the same mesh group. The MSDP mesh group has to be configured on each MSDP peer that should be a member of the mesh group.

Note

With MSDP, an RP tells another RP which sources it sees locally sending to which groups. Note that MSDP is not a routing protocol—it does not explain where to find a source. It only tells the other RP that this source S sending to group G exists.

MSDP message types, each encoded in their own TLV format:

SA messages are flooded throughout the Internet in a peer-RPF fashion (the RPF check is done to prevent SA looping). Can also carry initial multicast packet from the source

It contains: IP address of originating RP,number of (S,G) pairs advertised, list of active S,G pairs in the domain

SA request messages help decrease join latency, and are used to query the MSDP peers that cache SA messages. Instead of waiting for a periodic SA message, the active sources can be requested as soon as a new group joins a shared tree in a domain.

SA response messages are used in response to SA request messages. They are similar in structure to SA messages.

MSDP keepalive

Operation

RPs establish peering sessions using unicast TCP (UDP available aswell) port 639 to exchange multicast source (S,G). Lower-address peer initiates connection and Higher-address peer waits in listen state. If no keepalive messages or data are received for 75 seconds, the TCP connection is reset and reopened.

When source S sends traffic to group G, the RP detects it and encapsulates the multicast data in a Register Message and forwards it to other RPs, allowing them to de-encapsulate and forward the traffic to appropriate receivers. If the receiving MSDP peer is an RP, and the RP has a (\*, G) entry for the group in the SA (there is an interested receiver), the RP creates (S, G) state for the source and joins to the shortest path tree for the source. The encapsulated data is decapsulated and forwarded down the shared tree of that RP. When the last hop router (the router closest to the receiver) receives the multicast packet, it may join the shortest path tree to the source. RPs exchange Source Active (SA) Messages to share information about active multicast sources. The RPs create and maintain SA state, caching source/group pairs based on criteria defined by an ACL

The originating RP continues to send periodic SA messages for the (S,G) every 60 seconds as long as the source is sending packets to the group

When an MSDP speaker receives an SA message from one of its peers, it performs an RPF check, then forwards (floods) it to all its other peers. A peer RPF check is performed on the arriving SA message (using the originating RP address in the SA message) to ensure that it was received via the correct AS path. The BGP routing table is examined to determine which peer is the next hop toward the originating RP of the SA message. Only if this RPF check succeeds does the SA message get flooded downstream to its peers. This precaution prevents SA messages from looping through the Internet.

Receivers (group members) cause last-hop routers to send (_,G) joins to the RP to build branches of a shared tree for a group. If an RP has a (_,G) state for a group, and the OIL of the (\*,G) entry is not empty, the RP knows that it has active receivers for the group. Therefore, when it receives an SA message announcing an active source for group G in another domain, it can send (S,G) joins toward the source in the other domain.

Stub domains (that is, domains with only a single MSDP connection) do not have to perform this RPF check because there is only a single entrance or exit.

Anycast RP is a useful application of MSDP. Anycast RP is an intradomain feature that provides redundancy and load-sharing capabilities. With Anycast RP, a source may register with one RP and receivers may join a different RP, so a method is needed for RPs to exchange information about the active sources. This information exchange is done with MSDP.

MSDP speakers may cache SA messages, but they are typically not stored to minimize memory usage. However, by storing SA messages, you can reduce join latency. This reduction is enabled because an RP does not have to wait for the arrival of periodic SA messages when the first receiver joins the group. Instead, the RP can scan its SA cache to immediately determine which sources are active and then send appropriate (S,G) joins.

Noncaching MSDP speakers can query caching MSDP speakers in the same domain for information on active sources for a group.

Assume that a receiver in domain E joins multicast group 224.2.2.2. This join causes the DR that is labeled R to send a (\*,G) join for this group to the RP.

This activity builds a branch of the shared tree from the RP in domain E to the DR as shown.

When a source goes active in domain B, the first-hop router S sends a PIM register message to the RP. This informs the RP in domain B that a source is active in the local domain. The RP responds by originating an (S,G) SA message for this source and sending it to its MSDP peers in domains A and C. (The RP will continue to periodically—every 60 seconds—send these SA messages as long as the source remains active.)

When the RPs in domains A and C receive the SA messages, the RPF information is checked. Then, the SA messages are forwarded downstream to the MSDP peers of the domain E and D RPs.

The SA message traveling from domain A to domain C will fail the RPF check at the domain C RP (MSDP speaker). It will then be dropped because domain C has direct connectivity to domain B, where the source exists. However, the SA message arriving at domain C from domain B will pass the RPF check and be processed and forwarded to domains D and E and to A, but the SA from domain C to domain A will fail the RPF check and will be dropped. Similarly, the SA from domain D to domain E will fail the RFP check and will be dropped.

Once the (S,G) traffic reaches the last-hop router R in domain E, the last-hop router may optionally send an (S,G) join toward the source to bypass the RP in domain E.

![](<../.gitbook/assets/Unknown image (1200)>)

#### Interdomain SSM

You have seen that ASM requires MSDP to communicate the existence of sources between RPs. SSM is much simpler and does not require RPs, and thus no MSDP either.

However, routing still has to happen, so MP-BGP is still required to keep the multicast source table separate from the unicast one. And PIM-SM is used to create the MDT. But nevertheless, the architecture is much simpler, also from an administrative point of view.

This point is so important that for IPv6—no equivalent to MSDP has been planned. The assumption is that in the future, multicast will be much more SSM based, and that therefore there may never be a need for interdomain ASM.

![](<../.gitbook/assets/Unknown image (1201)>)

#### ASM interdomain summary

ASM is used when many sources send to the same multicast group avoiding the need to create a separate multicast distribution tree (MDT) for each source by using a single shared tree (\*,G). This reduces state in the network but adds complexity in locating sources, because receivers do not specify a source when joining.

Within a domain, an RP connects receivers to sources. Each service provider typically operates its own RP. However, sources and receivers in different domains cannot discover each other by default, because RPs only know about local sources.

MSDP solves this by connecting RPs across domains using a configuration similar to BGP multihop (over TCP). It sends "source active" messages to all RP peers, allowing RPs in other domains to learn about local sources, so receivers elsewhere can join them. MSDP does not create the MDT; PIM-SM still handles tree creation and must run between ASBRs. MP-BGP is also required between ASBRs, using multicast address families to generate a separate RPF table. This allows the multicast topology to differ from the unicast topology.

ASM’s shared-tree approach reduces state when many sources send to the same group, but it requires maintaining RPs and running MSDP between them. MSDP supports only IPv4, and there is no planned equivalent for IPv6, reflecting the IETF's lower priority for ASM interdomain. Where possible, SSM or other alternatives are preferred.

#### MP-BGP configuration notes

In the following figure, there are two links between the ABRs. One is supposed to be used for unicast traffic (the top link between .1 and .2), the other for multicast (the bottom link between .6 and .5). Therefore, there are two neighbor statements, and the corresponding address family is associated with the desired neighbor.

![](<../.gitbook/assets/Unknown image (1202)>)

PIM and IGMPv3

![](<../.gitbook/assets/Unknown image (1203)>)

#### MSDP configuration notes

Between domains, it is common to filter which sources and groups are allowed or denied. For this purpose, MSDP has the 'SA Filer" command. Like in BGP, filter lists can be defined in both directions. The ACL in the example filters two particular (_,G) entries: 'any" source (_) to "host 224.0.1.39" (and .40). In multicast notation, this statement is filtering (_,224.0.1.39) and (_,224.0.1.40). Any (S,G) matching these criteria will not be forwarded to MSDP peers. The two multicast groups in this example are used for Cisco RP announce and RP discovery protocols. These two protocols should always be filtered, because the protocol should act within the domain only.

MSDP functionally is very similar to BGP. Also, in MSDP, the #show msdp summary command displays a list of all MSDP peers, as shown in the following output.

To see which data MSDP is transmitting, use the #show msdp cache command.

![](<../.gitbook/assets/Unknown image (1204)>)

#### MSDP filtering

| ip msdp sa-filter \[ in \| out ] {peer-address \| peer-name} ip msdp sa-filter \[ in \| out ] {peer--address \| peer-name} list accesslist ip msdp sa-filter \[ in \| out ] {peer-address/name} route-map <>ip msdp cache-sa-state list 100 | Filters all SA messages from/to the specified MSDP peer From/To the specified MSDP peer, passes only those SA messages that pass the extended access list From/To the specified MSDP peer, passes only those SA messages that meet the match criteria in the route map map-tag value. controlling which source/group pairs are cached in the SA state based on an ACL |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Transport and scale features

#### Multicast over GRE

Unlike IPsec that does not support multicast, the GRE tunnels have no limitation on the types of traffic which can traverse it. It can route multiple subnets without multiple tunnels

However they can be combined to enable secure multicast communication over a VPN. GRE handles the multicast traffic encapsulation, and IPsec provides the necessary security services

Make sure the multicast TTL is large enough to cover all the router hops included in the new network.

| interface Tunnel0 ip address \<TUNNEL1\_IP\_ADDRESS> \<SUBNET\_MASK>tunnel source \<SOURCE\_INTERFACE1>tunnel destination \<ROUTER2\_IP\_ADDRESS>ip pim sparse-mode |   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

#### Multicast multipath (ECMP)

Normally multicast only utilizes one path even if there are multiple ECMP links available. This is because multicast relies on RPF which allows by default only one best neighbor

However, in scenarios where multiple paths are available, multicast multipath techniques aim to take advantage of this diversity for better load balancing, improved resilience, and optimized resource utilization.

ECMP Multicast Load Splitting Based on Source

When the #ip multicast multipath global configuration command is configured and multiple equal-cost paths exist, the path in which multicast traffic will travel is selected based on the source IP address. Multicast traffic from different sources will be load split across the different equal-cost paths. Load splitting will not occur across equal-cost paths for multicast traffic from the same source sent to different multicast groups. The S-hash algorithm mechanism ensures that traffic from the same source IP address consistently takes the same path

ECMP Multicast Load Splitting Based on Source and Group Address

The basic S-G-hash algorithm provides more flexible support for ECMP multicast load splitting than the the S-hash algorithm. Using the basic S-G-hash algorithm for load splitting, in particular, enables multicast traffic from devices that send many streams to groups or that broadcast many channels #ip multicast multipath s-g-hash basic

ECMP Multicast Load Splitting Based on Source Group and Next-Hop Address

provides even more flexible and granular control over load splitting # ip multicast multipath s-g-hash next-hop-based

Load splitting of IP multicast traffic can also be achieved by consolidating multiple parallel links (ex etherchannel or interface bundle interfaces) into a single tunnel over which the multicast traffic is then routed. This method of load splitting is more complex to configure than ECMP multicast load splitting. With the availability of ECMP multicast load splitting, tunnels typically only need to be used if per-packet load sharing is required.

#### Multicast helper-map (broadcast ↔ multicast)

In financial trading networks where a legacy stock ticker application sends packets out as broadcast UDP so we need to convert broadcast to multicast ,The router on the attached segment can then convert the broadcast destination to multicast, send the packet into the multicast transit network, and then on the last hop router attached to the receiver translate the multicast packet back to a broadcast. Multicast helper-map command is similar in theory to how the unicast “ip helper-map” works. With the IP helper map feature, IP broadcast packets, such as UDP based DHCP requests, have their destination addresses translated to a unicast address, such as the DHCP server. With the IP multicast helper map feature, IP broadcast packets have their destination addresses translated to a multicast address.

Example: SW1 — R4 -– R3 — R2 — R1 — SW2

SW1 is the broadcast sender (i.e. the source application), SW2 is the receiver (i.e. the destination application), R4 is the first hop router, and R1 is the last hop router.

| R4# interface FastEthernet0/0 description TO SENDER APPLICATION – SW1 ip address 173.20.47.4 255.255.255.0 ip multicast helper-map broadcast 224.1.2.3 100 ! ip forward-protocol udp 31337 access-list 100 permit udp any any eq 31337 | This configuration means that if R4 receives a UDP broadcast going to port 31337 inbound on Fa0/0 it will be translated to the multicast address 224.1.2.3. Note that the use of the “ip forwardprotocol” command is necessary in order to process switch UDP traffic going to the port in question. Without process switching the helper-map feature can not correctly categorize and translate the traffic. |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

| R1# interface Serial0/0 description TO R2 ip address 173.20.12.1 255.255.255.0 ip pim dense-mode ip multicast helper-map 224.1.2.3 173.20.18.255 100 ! interface FastEthernet0/0 description TO RECEIVER – SW2 ip address 173.20.18.1 255.255.255.0 ip directed-broadcast ip forward-protocol udp 31337 access-list 100 permit udp any any eq 31337 | This configuration means that if R1 receives a UDP multicast going to the group address 224.1.2.3 at port 31337 inbound on S0/0.102 it will be translated to the directed broadcast address 173.20.18.255. Since the link 173.20.18.0/24 is directly connected and has the directed broadcast address of 173.20.18.255 by default, the configuration implies that traffic matching the helper map on S0/0.102 will be sent as a broadcast out Fa0/0. Note the use of the “ip forward-protocol” command as before in order to process switch the UDP traffic. Additionally the “ip directed-broadcast” command is enabled on the last hop outgoing interface since in current IOS versions this is disabled by default for security purposes. |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### PIM NBMA mode

There are several problems when it comes to multicast on NBMA networks. First PIM treats the interfaces connected to NBMA networks as broadcast capable, even though they are not. So Assert messages are sent out to elect the forwarder for the “broadcast” network. Second, due to the split-horizon rule, the hub will not forward multicast packets it receives on its hub interface to other spokes on that interface.

The solution is to inform PIM of the true Layer 2 topology (non-broadcast). PIM NBMA is a special extension to PIM protocol operation that treats the underlying Layer 2 network as a collection of Point-to-point circuits. PIM NBMA is only supported in sparse mode.

| R1(config)# ip multicast routing distributed R1(config)# ip pim sparse-mode R1(config)# ip pim nbma-mode |   |
| -------------------------------------------------------------------------------------------------------- | - |

### IPv6 multicast

#### Multicast Listener Discovery (MLD)

Multicast Listener Discovery (MLD) is the IPv6 equivalent of IGMP and is used by hosts and routers to manage multicast group memberships. The most recent version, MLD version 2 (MLDv2), introduces enhanced features compared to MLD version 1 (MLDv1), but not all end devices support it. While deploying MLDv2 in a network is typically feasible, limitations on end devices may need to fall back to MLDv1, thereby restricting the use of Source-Specific Multicast (SSM).

The following are MLDv2 characteristics:

Receivers can join specific sources.

Source filtering: A receiver can listen to (S1,G) but not (S2,G), providing more flexibility.

Efficient multicast forwarding: MLDv2 allows routers to construct source-based distribution trees directly, eliminating the need for RPs.

The following are MLDv2 considerations:

SSM Support: MLDv2 is required for SSM in IPv6 networks, as it enables receivers to specify the sources they want to receive traffic from.

Enhanced Filtering: MLDv2 allows receivers to define filters such as all sources except X, improving control over multicast traffic.

Different Group Addresses: MLDv2 uses specific IPv6 multicast addresses for control messages, such as FF02::16 for membership reports, which are multicast themselves.

When designing an IPv6 multicast solution, the key question is whether all receiving devices support MLDv2. If they do, MLDv2 is the preferred choice. However, if some end devices only support MLDv1, a general MLDv1-based solution must be deployed, preventing direct SSM use. A possible workaround is SSM mapping, where a last-hop router determines the source address on behalf of a receiver.

MLD is used by an IPv6 router to discover the presence of multicast listeners on directly attached links, and MLD is used to discover which multicast addresses are of interest to those neighboring nodes. MLDv2 is interoperable with MLDv1. MLDv2 adds the ability for a node to report interest in listening to packets with a particular multicast address only from specific source addresses or from all sources except for specific source addresses.

The filter mode may be either INCLUDE or EXCLUDE. In INCLUDE mode, reception of packets that are sent to the specified multicast address is enabled only from the source addresses that are listed in the source list. In EXCLUDE mode, reception of packets that are sent to the given multicast address is enabled from all source addresses except those listed in the source list.

The router uses PIM to build forwarding state directly back to the source; no RP is involved. In this way, MLDv2 enables SSM. If no source is specified, multicast state is built back to the RP as in any-source multicasting.

Nodes respond to these queries by reporting their per-interface multicast address listener state through current state report messages that are sent to a specific multicast address that all MLDv2 routers on the link listen to. On the other hand, if the listening state of a node changes, the node immediately reports these changes through a state change report message. The state change report contains filter mode change records, source list change records, or records of both types.

Different message types:

Multicast Listener Report: Will be sent from a host when he wants to join/receive a specific multicast group (unsolicited report) and in response to Multicast Listener Queries from a local multicast router (solicited report). Sent to FF02::16

Multicast Listener Query: Issued by local multicast routers at regular intervals to check if there’s a host on the segment interested in a specific multicast group. Also sent out when a host on a segment sends a Multicast Listener Done to check if there are still other receivers on the segment. Sent to the all-nodes multicast group FF02::1

Multicast Listener Done: Sent by hosts to the all-routers multicast group FF02::2 to inform that they don’t want to receive a specific multicast group anymore. After receiving the Multicast Listener Done message, the multicast router sends a Group-specific Multicast Listener Query to the segment to see if any host still wants to receive traffic for that multicast group

| ## Joining a specific IPv6 multicast group Router(config-if)# ipv6 mld join-group ## Joining a specific IPv6 multicast group and filter/limit the allowed multicast sources Router(config-if)# ipv6 mld join-group source-list \[IPv6 ACL] |   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | - |

#### MLD filtering

| R1(config)# ipv6 access-list MLD\_FILTER R1(config-acl)# permit ipv6 any ff08::/64 R1(config)# ipv6 multicast routing R1(config)# interface GigabitEtherne0/1 R1(config-if)# ipv6 mld access-group MLD\_FILTER R1(config-if)# ipv6 mld query-interval 10 R1(config)# interface GigabitEthernet0/2 R1(config-if)# no ipv6 pim | Notice the use of an IPv6 access-list below to control the groups that a host may join. The access-list entry \[permit\|deny] ipv6 allows for both SSM and normal multicast group filtering So it is possible with this one filter to permit or deny the specific multicast groups that can be joined by specific hosts |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### IPv6 multicast basics

As service providers migrate addressing and routing protocols to IPv6, all other services should migrate to IPv6 as well before IPv6 connectivity could be offered to customers. Among others, these services include multicast, DNS, DHCP, QoS, and troubleshooting and management tools

An IPv6 multicast address is an identifier for a set of interfaces that typically belong to different nodes. A node may belong to any number of multicast groups. A packet that is sent to a multicast address is delivered to all interfaces that are identified by the multicast group address.

[IPv6 Multicast structure is described in IPv6 section](onenote:#IPv6\&section-id={6EC9C333-3E04-41C1-8C01-0E8D85C128A8}\&page-id={9FEA2E6E-BBB1-477C-A51C-AC09C4FF8958}\&object-id={D3C31647-2AEF-0444-13FD-4C2E49344310}&83\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE/L3.one)

MP-BGP version 4 support allows population of separate routing paths for IPv6 unicast and multicast traffic.

The Cisco IPv6 implementation does not support PIM-DM, but it is defined as a standard in Protocol Independent Multicast-Dense Mode, RFC 3973. However, due to resource consumption, dense mode is very rarely used.

IPv6 multicast uses Multicast Listener Discovery (MLD), rather than Internet Group Management Protocol (IGMP) of IPv4, for group management (join and leave reports and queries).

IPv6 multicast is scoped using IPv6 scoping rules.

IPv6 uses a single RP for interdomain multicast

#### PIMv6

PIMv6 is a routing protocol. PIMv6 is used between routers so that they can track which multicast packets to forward to each other and to their directly connected LANs. PIMv6 works independently of the unicast routing protocol to perform, send, or receive multicast route updates like other protocols. Regardless of which unicast routing protocols are used in the LAN to populate the unicast routing table, Cisco IOS PIMv6 uses the existing unicast table content to perform the RPF check instead of building and maintaining its own separate routing table

Only supports Sparse Mode and SSM - You can configure IPv6 multicast to use either PIM-SM or PIM-SSM operations, or you can use both PIM-SM and PIM-SSM together in your network.

MLD is used to allow link-local receivers to report interest in receiving specific multicast groups to their link-local router. PIM allows DRs serving multicast receivers (or last-hop routers), DRs serving multicast sources (or first-hop routers), and RPs to discover optimal forwarding paths for multicast control and data packets. The regions of the multicast network where PIM and MLD (Multicast Listener Discovery) are active are shown in the figure. Note that routers can be first-hop routers for some multicast addresses and last-hop routers for other multicast addresses concurrently.

PIM packets are exchanged between all three of these main components:

DR (first hop) to and from RP: This part of PIM enables new multicast sources to register with the RP that is associated with a given multicast address. This is done via PIM tunnels, in which the multicast packets are tunneled by the first-hop router to the RP. Later, the RP can obtain the multicast stream directly from the source. This is called building the SPT from the RP to the multicast source.

DR (last hop) to and from RP: This part of PIM enables the DR router serving new multicast receivers to build a flooding path back to the RP through the intervening multicast routers in such a way that the DR can receive a specific stream. This results in the RP flooding packets toward the DR (last-hop) router that, in turn, results in a completed shared tree.

DR (last hop) to and from DR (first hop): This is a process called building the SPT, in which the DR serving the listeners desires to get the multicast stream directly from the DR serving the multicast source (DR source).

In phase 1 of the PIM-SM protocol overview, a multicast receiver expresses its interest in receiving traffic that is destined for a multicast group. Typically, the multicast receiver does this using MLD. One of the local routers of the receiver is elected as the DR for this subnet. Upon receiving the expression of interest from the receiver, the DR then sends a PIM join message toward the RP for this multicast group. This join message is known as a (_,G) Join because it joins group G for all sources to this group. The (_,G) Join travels hop by hop toward the RP for the group, and in each router it passes through, multicast tree state for group G is instantiated. Eventually, the (_,G) Join either reaches the RP or reaches a router that already has the (_,G) Join state for this group. When many receivers join the group, their join messages converge on the RP and form a distribution tree for group G that is rooted at the RP. This is known as the RPT and is also known as the shared tree because it is shared by all sources sending to this group. Join messages are re-sent periodically as long as the receiver remains in the group. When all receivers on a leaf-network leave the group, the DR will send a PIM (\*,G) Prune message toward the RP for this multicast group. However, if the prune message is not sent for any reason, the state will eventually time out.

![](<../.gitbook/assets/Unknown image (1205)>)

Phase 2 of the PIM-SM protocol overview is register-stop.

The process of encapsulating data packets to the RP is called registering, and the encapsulation packets are known as PIM register packets. At the end, multicast traffic is flowing encapsulated to the RP and then natively over the RPT to the multicast receivers.

Although register-encapsulation may continue indefinitely, for these reasons, the RP will normally choose to switch to native forwarding. To do this, when the RP receives a register-encapsulated data packet from source S on group G, it will normally initiate an (S,G) source-specific join toward S. This join message travels hop by hop toward S, instantiating the (S,G) multicast tree state in the routers along the path. The (S,G) multicast tree state is used only to forward packets for group G if those packets come from source S. Eventually, the join message reaches the S subnet or a router that already has the (S,G) multicast tree state, and then packets from S start to flow following the (S,G) tree state toward the RP. These data packets also may reach routers with the (\*,G) state along the path toward the RP; if they do, they can shortcut onto the RPT at this point.

While the RP is in the process of joining the source-specific tree for S, the data packets will continue being encapsulated to the RP. When packets from S also start to arrive natively at the RP, the RP will receive two copies of each of these packets. At this point, the RP starts to discard the encapsulated copy of these packets, and it sends a register-stop message back to the S DR to prevent the DR from unnecessarily encapsulating the packets.

At the end of phase 2, traffic will flow natively from S along a source-specific tree to the RP and from there along the shared tree to the receivers.

Note A sender may start sending before or after a receiver joins the group, and thus phase 2 may happen before the shared tree to the receiver is built.

Phase 3

Although having the RP join back toward the source removes the encapsulation overhead, it does not completely optimize the forwarding paths. For many receivers, the route via the RP may involve a significant detour when compared with the shortest path from the source to the receiver.

To obtain lower latencies or more efficient bandwidth utilization, a router on the receiver LAN, typically the DR, may optionally initiate a transfer from the shared tree to a source-specific SPT. To do this, it issues an (S,G) Join toward S. This instantiates state in the routers along the path to S. Eventually, this join either reaches the S subnet, or it reaches a router that already has the (S,G) state. When this happens, data packets from S start to flow following the (S,G) state until they reach the receiver.

At this point, the receiver (or a router upstream of the receiver) will receive two copies of the data: one from the SPT and one from the RPT. When the first traffic starts to arrive from the SPT, the DR or upstream router starts to drop the packets for G from S that arrive via the RPT. In addition, it sends an (S,G) prune message toward the RP. This is known as an (S,G,rpt) Prune. The prune message travels hop by hop, instantiating state along the path toward the RP, indicating that traffic from S for G should not be forwarded in this direction. The prune is propagated until it reaches the RP or a router that still needs the traffic from S for other receivers.

By now, the receiver will receive traffic from S along the SPT between the receiver and S.

#### Embedded RP in an IPv6 multicast address

Embedded RP is a mechanism that eliminates the need for static configuration of RPs on the multicast DRs. The RP address for a given multicast group (address) is encoded inside the multicast group.

Using prefix embedding, any enterprise with a /64-bit prefix can generate a unique multicast address (or group)—guaranteed to be unique Internet-wide—without consulting any organization or registering with IANA.

Routers that do not support embedded RP can be statically configured or use other methods such as BSR.

The figure shows the format for the multicast group address. It is derived via the mechanism that is described in RFC 3956, Embedding the Rendezvous Point (RP) Address in an IPv6 Multicast Address, and is also a unicast-prefix multicast group address. A multicast group address uses these special rules, with fields described from left to right:

FF: This is a multicast designator.

Flags: This field contains 4 bits, which is defined from left to right as follows:

First bit: This is reserved.

Second bit: This is known as the R field, which, when set to 1, specifies that this multicast address contains an embedded RP address.

Third bit: This is known as the P field, which specifies if the address contains a unicast prefix-based address. It is set to 1 if it does contain a unicast prefix-based address. The unicast prefix included in the address is for the multicast source home prefix, making this multicast group globally unique. If the R bit is set to 1, the P bit must be set to 1 as well.

Fourth bit: This is known as the T field, and it is set to 0 if this is a permanently-assigned, well-known multicast address and to 1 if temporary. For all cases of embedded RP, the T bit must be set to 1.

Scope: This field has the same values as for any multicast address.

Rsvd (reserved): This field is commonly set to all zeros. If this was not an embedded RP multicast address, this would be an 8-bit reserved field.

RPadr: This is a 4-bit field that specifies RPs 1 to 16 on the /64 network prefix embedded farther along in the address.

The 4 bits taken from the previously reserved range are used to specify one of 16 addresses, such as 1-15 (0 is reserved). Therefore, an address ending in ::16 or larger could not be an embedded RP.

Plen (prefix length): This value must not be 0 (SSM) or greater than 64. The value specifies how many bits of network prefix from the group address will be used to determine the RP address.

Network-Prefix: This field is the network prefix for the RP.

Group-ID: This is the multicast group ID, which is either a well-known ID, such as NTP (Network Time Protocol), or a temporary ID that is invented by an enterprise. Remember that the marks field identifies the address as well known or transient.

In the example, the mark bits are set to "0111," which means "embedded RP, unicast-based, temporary" multicast group. Plen is set to 40, which means that 64 (all) bits of the network prefix are used to determine the RP IP address. Group ID in the example is set to 12, and the RP address ("y") can range from 1 to 15.

![](<../.gitbook/assets/Unknown image (1206)>)

Example 2:

![](<../.gitbook/assets/Unknown image (1207)>)

Note The Cisco Auto-RP does not support IPv6.

Note The embedded RP solves the problem of helping multicast routers choose the right RP, but it does not address the redundancy of RPs if the RP goes down.

#### PIMv6 anycast RP

Provides anycast RP benefits for PIM-SM within IPv6 networks and doesn’t rely on MSDP (unlike IPv4 Anycast RP which requires MSDP)

Router(config)# ipv6 pim anycast-rp

Router# show ipv6 pim anycast-rp
