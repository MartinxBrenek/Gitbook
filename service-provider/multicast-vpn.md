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

# Multicast VPN

### Overview

There are two ways that multicast VPN (MVPN) traffic can transport over the core network:

#### Rosen Draft GRE (native)

**Rosen Draft GRE (native):** uses GRE with unique multicast distribution tree (MDT) forwarding to enable scalability of native IP Multicast in the core network. MVPN introduces multicast routing information to the VPN routing and forwarding table (VRF), creating a Multicast VRF. In Rosen GRE, the multicast customer packets (c-packets) are encapsulated into the provider multicast packets (p-packets), so that the PIM protocol is enabled in the provider core (P routers), and mrib/mfib is used for forwarding p-packets in the core. MPLS LDP is not used to transport multicast traffic. However, since this method is essentially MPLS independent, it cannot benefit of Fast Reroute (FRR). In this model, default and data MDTs are defined, but it is not possible to use partitioned MDTs. Partitioned MDTs do not include all PEs of a VPN and are therefore more scalable. Furthermore, all MDTs have to be explicitly configured.

#### mLDP/LSM (MPLS label-switched multicast)

**mLDP/LSM:** MVPN allows a service provider to configure and support multicast traffic in an MPLS VPN environment. This type supports routing and forwarding of multicast packets for each individual VPN routing and forwarding (VRF) instance, and it also provides a mechanism to transport VPN multicast packets across the service provider backbone. In the MLDP case, the regular label switch path forwarding is used, so core (P) routers do not need to run PIM protocol. In this scenario, the c-packets are encapsulated in the MPLS labels and forwarding is based on the MPLS Label Switched Paths (LSPs), similar to the unicast case.

In LSM with multipoint LDP, the same control and data plane is used as in the unicast MPLS core. Packet switching is based on labels, and those labels are negotiated via multipoint LDP in the case of multicast. Since multipoint LDP is just an extension to LDP, it is truly the same protocol. This allows the use of FRR. In addition to default and data MDT also partitioned MDTs are supported, and there is no configuration needed for data MDTs, which simplifies configuration and troubleshooting.

MLDP enables the use of a single MPLS forwarding plane for both unicast and multicast traffic. It also enables you to use existing MPLS protection (for example, MPLS Traffic Engineering/Resource Reservation Protocol (TE/RSVP link protection) and MPLS Operations Administration and Maintenance (OAM) mechanisms for multicast traffic. Lastly, it reduces operational complexity by eliminating the need for PIM in the MPLS core network.

There are alternatives to supporting multicast in an MPLS core network, such as IP based with PIM and Generic Routing Encapsulation (GRE) ("draft-rosen"), point-to-multipoint traffic engineering, and ingress replication. However, multipoint LDP plays a leading role for MVPN deployments.

### Multipoint LDP (mLDP) / Label Switch Multicast (LSM)

**Multipoint LDP (mLDP) / Label Switch Multicast (LSM)** is an extension of the existing LDP to support multicast. Multipoint LDP supports the creation of point-to-multipoint as well as multipoint-to-multipoint MDTs.

Multipoint LDP enables Label Switched Multicast (LSM), where the forwarding and replication of packets is based on MPLS labels, just like in unicast

Multipoint LDP runs exclusively in the MPLS core, just like LDP for unicast traffic.

PIM and MP-BGP are used in the control plane - PIM neighbors in a VRF are seen across an LSP virtual interface (LSP-VIF)

Multicast LDP is used in the data plane.

Seen from the customer edge (CE) router, the PE looks just like another router, and PIM-SM is the protocol between the two for multicast tree management. The PE is the only element that sees both overlay and core: On the overlay level, the next hop is the remote PE; on the core level, it interfaces P routers.

![](<../.gitbook/assets/Unknown image (1565)>)

The PIM routing protocol ensures the exchange of multicast groups and sources while MP-BGP ensures routing towards the source.

MVPN establishes a static default Multicast Distribution Tree (MDT) for each multicast domain.

The Default MDT includes all PEs and is used for signaling about all sources and all groups.

Traffic that belongs to those groups generally uses the default MDT when it is being distributed to PEs with receivers.

MVPN also supports the dynamic creation of MDTs for specific groups. The idea behind data MDT is to create separate MDTs for traffic belonging to specific groups that are related to a sub-set of PEs, instead of all PEs in a multicast domain. For specific (S, G) groups you can define a threshold to decide when to create a data MDT. Threshold can be zero, infinite, or a specific value. Zero means that immediately after the first packet for specific (S, G) multicast group is received, a separate MDT will be created; infinite means that data MDT will never be created for that specific (S, G) multicast group; a specific value means that when traffic for that specific (S, G) group reaches the threshold, the data MDT will be created. Data MDTs are a unique feature in Cisco IOS Software. The creation of the data MDT is signaled dynamically using MDT Join TLV messages.

Data MDTs with thresholds serve high-bandwidth sources such as full-motion video inside the VPN to ensure optimal traffic forwarding in the MPLS VPN core. You can configure the threshold for the dynamic creation of data MDTs on a per-router or a per-VRF basis. When the multicast transmission exceeds the threshold that you define, the sending PE router creates the data MDT and sends a UDP message that contains information about the data MDT to all routers on the default MDT. The router examines the statistics once every second to determine whether a multicast stream exceeds the data MDT threshold. After a PE router sends the UDP message, it waits 3 more seconds before switching over (13 seconds is the worst-case switchover time and 3 seconds is the best case). Data MDTs are created only for (S, G) multicast route entries within the VRF multicast routing table. They are not created for (\*, G) entries regardless of the value of the individual source data rate.

![](<../.gitbook/assets/Unknown image (1566)>)

### Multicast Tunnel Interface (MTI)

The PE router creates a **Multicast Tunnel Interface (MTI)** for each MVRF in the multicast domain. The MVRF uses the tunnel interface to access the multicast domain to provide a conduit that connects an MVRF and the global MVRF. On the router, the MTI is a tunnel interface (created with the interface tunnel command) with a Class D multicast address. All PE routers that you configure with a default MDT for this MVRF create a logical network in which each PE router appears as a PIM neighbor (one hop away) to every other PE router in the multicast domain, regardless of the actual physical distance between them.

The MTI is automatically created when you configure an MVRF. The Border Gateway Protocol (BGP) peering address is assigned as the MTI interface source address, and the PIM protocol is automatically enabled on each MTI.

Unlike other tunnel interfaces that you commonly use on Cisco routers, the MVPN MTI is classified as a LAN interface, not a point-to-point interface. Although the MTI interface is not configurable, you can use the show interface tunnel command to display its status. The MTI interface is exclusively for multicast traffic over the VPN tunnel. The tunnel does not carry unicast-routed traffic.

When the PE router receives a multicast packet from the customer side of the network, it uses the incoming interface’s VRF to determine which MVRFs should receive it. The router then encapsulates the packet with Generic Routing Encapsulation (GRE). When the router encapsulates the packet, it sets the source address to that of the BGP peering interface and sets the destination address to the multicast address of the default MDT (or to the source address of the data MDT, if you configure it). The router then replicates the packet as necessary for forwarding on the appropriate number of MTI interfaces.

When the router receives a packet on the MTI interface, it uses the destination address to identify the appropriate default MDT or data MDT, which in turn identifies the appropriate MVRF. It then decapsulates the packet and forwards it out the appropriate interfaces, replicating it as many times as are necessary. It assigns a ring address as the MTI interface source address and automatically enables the PIM protocol on each MTI.

![](<../.gitbook/assets/Unknown image (1567)>)

### Packet forwarding (data plane)

LSM uses the same basic switching method as unicast label switching. Instead of using IP addresses, labels are used to determine the next hop for a unicast packet, or the possible several next hops for a multicast packet.

The **Label Forwarding Information Based (LFIB)** can be consulted with the show mpls forwarding-table command. It shows for each incoming packet, which incoming label is replaced with which outgoing label once the packet is sent out. In the case of a multicast table, there can be more than one outgoing interface and next hop.

For each packet coming in, MPLS creates multiple out-labels. Packets from the source network are replicated along the path to the receiver network. The CE1 router sends out the native IP multicast traffic. The Provider Edge1 (PE1) router imposes a label on the incoming multicast packet and replicates the labeled packet towards the MPLS core network. When the packet reaches the core router (P), the packet is replicated with the appropriate labels for the MP2MP default MDT or the point-to-multipoint data MDT and transported to all the egress PEs. Once the packet reaches the egress PE (edge routers), the label is removed and the IP multicast packet is replicated onto the VRF interface. Basically, the packets are encapsulated at headend and decapsulated at the tail end on the PE routers.

![](<../.gitbook/assets/Unknown image (1568)>)

There are different ways a Label Switched Path (LSP) built by mLDP can be used depending on the requirement and nature of the application, such as the following:

Point-to-multipoint LSPs for global table transit Multicast using in-band signaling.

P2MP/MP2MP LSPs for MVPN based on MI-PMSI or Multidirectional Inclusive Provider Multicast Service Instance (Rosen Draft).

P2MP/MP2MP LSPs for MVPN based on MS-PMSI or Multidirectional Selective Provider Multicast Service Instance (Partitioned E-LAN).

The router performs the following important functions for the implementation of MLDP:

Encapsulating VRF multicast IP packet with GRE/Label and replicating to core interfaces (imposition node).

Replicating multicast label packets to different interfaces with different labels (Mid node).

Decapsulate and replicate label packets into VRF interfaces (Disposition node).

![](<../.gitbook/assets/Unknown image (1569)>)

### Extranet MVPN

With an **intranet MVPN**, all the multicast clients are located within the same organization.

With an **extranet MVPN**, multicast clients that are external to the organization are allowed to access the intranet. The external users may be business partners, customers, or suppliers to the organization. It allows different closed user groups to share multicast information across multiple VPN customers

The Multicast VPN Extranet Support feature solves these challenges. First, the receiver and source MVRF multicast route (mroute) entries are linked. Also, the RPF check relies on unicast routing information to determine the interface through which the source is reachable. This interface is used as the RPF interface.

Two configuration options are available to provide extranet MVPN services.

Option 1 is to configure the receiver MVRF on the source PE router.

In the receiver MVRF configuration, you configure the same unicast routing policy on the source and receiver PE routers to import routes from the source MVRF to the receiver MVRF. In other words, you can use selective route-import (route-policies) to import unicast source IP address into receiver PE so RPF to the source is possible.

A multicast source behind PE1 sends a multicast stream to the MVRF for VPN-Green, and the interested receivers are behind PE2 and PE3, the receiver PE routers for VPN-Red and VPN-Green, respectively. After PE1 receives the packets from the source in the MVRF for VPN-Green, it independently replicates and encapsulates the packets in the MVRF for VPN-Green and VPN-Red and forwards the packets.

Option 2 is to configure the source MVRF on the receiver PE router.

On the receiver PE router, you configure the same unicast routing policy to import routes from the source MVRF to the receiver MVRF.

A multicast source behind PE1, the source PE router, sends a multicast stream to the MVRF for VPN-Green, and the interested receivers are behind PE2, the receiver PE router for VPN-Red, and behind PE3, the receiver PE router for VPN-Green. After PE1 receives the packets from the source in the MVRF for VPN-Green, it replicates and forwards the packets to PE2 and PE3, because both routers connect to receivers in VPN-Green. The packets that originate from VPN-Green then replicate on PE2 and forward to the interested receivers in VPN-Red, then replicate on PE3 and forward to the interested receivers in VPN-Green.

**Multicast VPN Extranet VRF Select**

Prior to the introduction of the Multicast VPN Extranet VRF Select feature, RPF lookups for a source address could perform only in a single VRF, that is, in the following:

The VRF where Internet Group Management Protocol (IGMP) or PIM joins are received.

The VRF that is learned from BGP imported routes.

The VRF that is specified in static mroutes (when RPF for an extranet MVPN is configured with static mroutes). In these cases, the source VRF is solely determined by the source address or the way that the source address is learned.

Multicast VPN Extranet VRF Select feature provides the capability for RPF lookups to perform to the same source address in different VRFs by using the group address as the VRF selector. This feature enhances extranet MVPNs by enabling service providers to distribute content streams that originate from different MVPNs and redistribute them from there.

You configure the Multicast VPN VRF Select feature by creating group-based VRF selection policies. You configure group-based VRF selection policies by using the ip multicast rpf select command. Use the ip multicast rpf select command to configure RPF lookups that originate in a receiver MVRF or in the global routing table to resolve in a source MVRF or in the global routing table based on group address. Use access control lists (ACLs) to define the groups to apply to group-based VRF selection policies.

In this topology, (S, G1) and (S, G2) PIM joins that originate from VPN-Green, the receiver VRF, forward to PE1, the receiver PE. Based on the group-based VRF selection policies that you configure, PE1 sends the PIM joins to VPN-Red and VPN-Blue for groups G1 and G2, respectively.

![](<../.gitbook/assets/Unknown image (1570)>)

### MVPN configuration

#### Multicast Distributed Switching (MDS) for MVPN

Cisco supports **Multicast Distributed Switching (MDS)** for MVPN. Prior to MDS, IP multicast traffic always switched at the route processor in Route Switch Processor (RSP)-based platforms.

Switching multicast traffic at the route processor had the following disadvantages:

It increased the load on the route processor, which affected important route updates and calculations (for BGP, among others) and a substantial multicast load could stall the router.

The net multicast performance was limited to what a single route processor could switch.

Router> enable

Router# configure terminal

Router(config)# ip multicast-routing distributed

Router(config)# interface ethernet 0

Router(config-if)# ip route-cache distributed

Router(config-if)# ip mroute-cache distributed

Router(config-if)# end

MDS is accomplished using a forwarding data structure called a **Multicast Forwarding Information Base (MFIB)**, which is a subset of the routing table. A copy of MFIB runs on each line card and always stays up-to-date with the MFIB table of the route processor.

If you do not enable MDS on an incoming interface that is capable of MDS, incoming multicast packets are not distributed-switched. Instead, the multicast packets are fast-switched at the route processor. Also, if the incoming interface is incapable of MDS, packets are fast-switched or process-switched at the route processor.

If you enable MDS on the incoming interface, and at least one of the outgoing interfaces cannot fast-switch, packets are process-switched. To configure MDS, you must enable it globally and on at least one interface, because MDS is an attribute of the interface.

#### mLDP configuration

You must activate multicast routing for every MVPN. You accomplish this goal by using the multicast-routing vrf vrf-name command. You must then enter into each address-family for which multicast traffic is going to be transported and define the root-node for the default MDT by using the mdt default mldp ipv4 root-node command where root-node is replaced by the IP address of the root-node. The root node can be IP address of a loopback or physical interface on any router (source PE, receiver PE, or core router) in the provider network. The root node address should be reachable by all the routers in the network. The router from where the signaling occurs functions as the root node. The default MDT must be configured on each PE router to enable the PE routers to receive multicast traffic for that particular MVRF. Same root-node must be configured on all PEs that are part of the same MDT. You can configure more that one root node, meaning that you can create, for example, two default MDTs for same VRF, using two different routers as root nodes, in such a way that you assign a primary default MDT and a backup default MDT for same MVRF. From each router, IGP metric to root node defines which one is used as primary and which one as backup.

Route distinguisher needs to be defined in BGP for each VRF. Data MDTs can be optionally configured. Up to 255 data MDTs can be defined for multicast VRF. Use the mdt data <1-255> command in the context of each multicast VRF to define the required amount of data MDT for that VRF. Depending on MLDP profiles to be used, you may have to enable address-family IPv4 MDT globally and for each remote PE or Route Reflector.

Configure MVPN routing and forwarding instance:

The MVPN configuration creates the default MDT. A static default MDT establishes for each multicast domain. It is used for customer control plane and low-rate data plane traffic. It connects all the PE routers with MVRFs in a particular multicast domain, and one will exist in every multicast domain, even if no source is active in the respective customer network.

The default MDT group must be the same group that you configure on all devices that belong to the same VPN. This address serves as an identifier for the MVRF community, because all provider-edge (PE) routers that are configured with this same group address become members of the group, which allows them to receive the PIM control messages and multicast traffic that other members of the group send.

multicast-routing vrf vrf\_name

address-family ipv4

mdt default mldp ipv4 root-node

Note You use the VPN-ID to create the default MDT, so manually defining a multicast group for default MDT is unnecessary.

Configure the route distinguisher:

router bgp AS Number

vrf vrf\_name

rd rd\_value

Configuring data MDTs (optional):

Before you configure a default MDT group, you must configure the VPN for multicast routing. After specifying the threshold, you can add and access-list to specify the (S, G) MVPN entries to be used in a data MDT pool, which would further limit the creation of a data MDT pool to the particular (S, G) MVPN entries defined in the access list specified for the access-list argument. You should configure all access lists that are necessary for the tasks in this section prior to beginning the configuration task.

multicast-routing vrf vrf\_name

address-family ipv4

mdt data <1-255>

Configure neighbors for BGP MDT address family:

You add the mdt keyword to the address-family ipv4 command to configure an MDT address-family session. You use MDT address-family sessions to pass the source PE address and MDT group address to PIM by using BGP MDT Sub-address Family Identifier (SAFI) updates.

Before MVPN peering can establish through an MDT address family, you must configure MPLS and Cisco Express Forwarding in the BGP network and multiprotocol BGP on PE devices that provide VPN services to CE devices.

In a single autonomous system, if the default MDT for an MVPN uses PIM sparse mode (PIM-SM) with a rendezvous point (RP), then PIM is able to establish adjacencies over the Multicast Tunnel Interface (MTI) because the source PE and receiver PE discover each other through the RP.

router bgp AS Number

address-family ipv4 mdt

neighbor x.x.x.x

address-family ipv4 mdt

Next-hop-self

Configure BGP VPNv4 address family:

The service provider is still required to differentiate overlapping IPv4 addresses from end customers. Address-family VPNv4 must be enabled in global BGP. Also, VPNv4 prefix exchange must be configured for each neighbor PE or Route Reflector.

router bgp AS Number

address-family vpnv4 unicast

Configure BGP IPv4 VRF address family:

router bgp AS Number

vrf vrf\_name

address-family ipv4 unicast

Configure PIM SM/SSM Mode for the VRFs:

Remember that multicast traffic is coming from end customer locations. It means that PIM protocol must be enabled in PE routers inside each VRF using multicast (MVRF). In configuration mode, use the router pim command, followed by vrf vrf-name command and address-family ipv4 command to enable PIM protocol in that VRF. Also, interfaces must be assigned to the correct VRF, and PIM must be enabled in the interface.

router pim

vrf vrf\_name

address-family ipv4

rpf topology route-policy my-policy

Configure route-policy:

You can configure MLDP in many ways by using profiles. Each profile needs some signaling mechanism in the core. For each MVRF, you can apply a route-policy to define the type of signaling in the core. To take that action, you use the rpf topology route-policy policy-map-name command. Then configure the route-policy with matching signaling. For example, you can use the set core-tree tree-type command inside the route-policy where tree-type could be replaced by mldp-inband.

route-policy my-policy

set core-tree tree-type

pass

end-policy

Note For each profile, a different route-policy is configured.

**IOS XE example**

| Device(config)# ip multicast-routing Device(config)# ip multicast-routing vrf VRF Device(config-vrf)# ip vrf VRF Device(config-vrf)# rd 50:11 Device(config-vrf)# vpn id 50:10 Device(config-vrf)# route target export 100:100 Device(config-vrf)# route target import 100:100 Device(config-vrf)# mdt preference mldp Device(config-vrf)# mdt default mpls mldp 172.30.20.1 Device(config-vrf)# mdt data mpls mldp 255 Device(config-vrf)# mdt data threshold 40 list 1 | Define the VRF instance Create a route distributor (RD) to make the VRF functional Set MLDP for MDT type Define default MDT group parameters, including root node Define data MDT parameters show mpls mldp database show ip pim vrf <> neighbor show ip mroute vrf VRF 238.1.200.2 10.5.200.3 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Device(config)# mpls mldp logging notifications Device(config)# mpls mldp forwarding recursive Device(config)# mpls mldp path multipath downstream                                                                                                                                                                                                                                                                                                                       | MLDP logging notifications MLDP recursive forwarding over a point-to-multipoint LSP Load-balancing different LSPs over available paths or TE tunnels                                                                                                                                           |
| Device(config)# ip multicast vrf cisco route-limit 200000 20000                                                                                                                                                                                                                                                                                                                                                                                                          | Configure Multicast Routes Limiting You configure multicast routes and information to limit the number of multicast routes that a device can add. An ACL can be used to filter the multicast device information request packets for all sources specified in the access list.                  |
| Device(config)# ip multicast mrinfo-filter 4                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                |

#### mLDP in-band signaling (Profile 6)

/\* Assign a route policy in PIM to select a reverse-path forwarding (RPF) topology \*/

RP/0/RP0/CPU0:router(config)#router pim

RP/0/RP0/CPU0:router(config-pim)#vrf one

RP/0/RP0/CPU0:router(config-pim-one)#address-family ipv4

RP/0/RP0/CPU0:router(config-pim-one-ipv4)#rpf topology route-policy rpf-vrf-one

/\* Configure route policy to set the MDT type to MLDP inband \*/

RP/0/RP0/CPU0:router(config)#route-policy rpf-vrf-one

RP/0/RP0/CPU0:router(config-rpl)#set core-tree mldp-inband

RP/0/RP0/CPU0:router(config-rpl)#end-policy

/\* Enable MLDP-inband signaling in multicast routing \*/

RP/0/RP0/CPU0:router(config)#multicast-routing

RP/0/RP0/CPU0:router(config-mcast)#vrf one

RP/0/RP0/CPU0:router(config-mcast-one)#address-family ipv4

RP/0/RP0/CPU0:router(config-mcast-one-ipv4)#mdt source loopback 0

RP/0/RP0/CPU0:router(config-mcast-one-ipv4)#mdt mldp in-band-signaling ipv4

RP/0/RP0/CPU0:router(config-mcast-one-ipv4)#interface all enable

/\* Enable MPLS MLDP \*/

RP/0/RP0/CPU0:router(config)#mpls ldp

RP/0/RP0/CPU0:router(config-ldp)#mldp

Example

router bgp 100

mvpn

!

multicast-routing

mdt source Loopback0

vrf v61

address-family ipv4

mdt mtu 1600

mdt mldp in-band-signaling ipv4

interface all enable

!

address-family ipv6

mdt mtu 1600

mdt mldp in-band-signaling ipv4

interface all enable

!

!

router pim

vrf v61

address-family ipv4

rpf topology route-policy mldp-inband

!

address-family ipv6

rpf topology route-policy mldp-inband

!

!

route-policy mldp-inband

set core-tree mldp-inband

end-policy

!

#### mLDP Profile 7 (global in-band signaling)

/\* Assign a route policy in PIM to select a reverse-path forwarding (RPF) topology \*/

RP/0/RP0/CPU0:router(config)#router pim

RP/0/RP0/CPU0:router(config-pim)#address-family ipv4

RP/0/RP0/CPU0:router(config-pim-default-ipv4)#rpf topology route-policy rpf-global

RP/0/RP0/CPU0:router(config-pim-default-ipv4)#interface TenGigE 0/0/0/21

RP/0/RP0/CPU0:router(config-pim-ipv4-if)#enable

/\* Configure route policy to set the MDT type to MLDP inband \*/

RP/0/RP0/CPU0:router(config)#route-policy rpf-global

RP/0/RP0/CPU0:router(config-rpl)#set core-tree mldp-inband

RP/0/RP0/CPU0:router(config-rpl)#end-policy

/\* Enable MLDP-inband signaling in multicast routing \*/

RP/0/RP0/CPU0:router(config)#multicast-routing

RP/0/RP0/CPU0:router(config-mcast)#address-family ipv4

RP/0/RP0/CPU0:router(config-mcast-default-ipv4)#interface loopback 0

RP/0/RP0/CPU0:router(config-mcast-default-ipv4-if)#enable

RP/0/RP0/CPU0:router(config-mcast-default-ipv4-if)#exit

RP/0/RP0/CPU0:router(config-mcast-default-ipv4)#mdt source loopback 0

RP/0/RP0/CPU0:router(config-mcast-default-ipv4)#mdt mldp in-band-signaling ipv4

RP/0/RP0/CPU0:router(config-mcast-default-ipv4)#interface all enable

/\* Enable MPLS MLDP \*/

RP/0/RP0/CPU0:router(config)#mpls ldp

RP/0/RP0/CPU0:router(config-ldp)#mldp

multicast-routing

address-family ipv4

mdt source Loopback0

mdt mldp in-band-signaling ipv4

ssm range Global-SSM-Group

interface all enable

!

address-family ipv6

mdt source Loopback0

mdt mldp in-band-signaling ipv4

ssm range Global-SSM-Group-V6

interface all enable

!

router pim

address-family ipv4

rpf topology route-policy mldp-inband

!

address-family ipv6

rpf topology route-policy mldp-inband

!

!

route-policy mldp-inband

set core-tree mldp-inband

end-policy

!

### Verification

show mpls mldp neighbors command to verify the following aspects:

MLPD Peer ID: IP address and label space used as neighbor’s LDP ID

Uptime: how long has been the neighbor in UP state

Current State: UP, DOWN, other

Neighbor capabilities: Graceful Restart (GR), point-to-multipoint, MP2MP, others

Policy filter in: applied policy in

Path count: number of paths to reach the neighbor

Paths: list of connected neighbors and interfaces to reach them

Adj list: list of LDP neighbor with established adjacencies

Peer Adr list: List of IP addresses configured in neighbor

To display the contents of the Label Information Base (LIB), use the show mpls mldp bindings command. In contrast to IPv4/IPv6 unicast address families which shows LDP labels for IPv4/IPv6 prefixes, this command shows labels for (S, G) multicast groups, either IPv4 or IPv6 multicast.

In the figure you can identify following important fields of information

LSP-ID which also identifies the LSP virtual interface (Lspvif)

Type of LSP: it can be point-to-multipoint for (S, G) or MP2MP for (\*, G)

Identification of addressing in the service provider backbone: Either VPNv4 or VPNv

Source for Multicast Traffic: IPv4 or IPv6 address of multicast source

Multicast Group: IPv4 or IPv6 multicast group

Local label: LDP label for (S, G) multicast group assigned by local router

Remote Labels: LDP labels assigned by remote PEs to (S, G) multicast group
