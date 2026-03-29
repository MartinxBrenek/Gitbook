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

# Inter domain MPLS VPN

### Unified MPLS (intra-AS interconnection)

(Interconnecting areas within one domain)

Each SP IGP has grown to the point that the processing power to maintain LSDB were too high, the new IGP domain has been implemented. But when we want to create LSP throughout the IGP domains we need to redistribute all the loopbacks between domains, which destroys the purpose of having multiple IGPs, since we would still have to redistribute all prefixes from one domain to another and also manually configure boundary router

**Unified MPLS** is an improved version of MPLS that uses a hierarchical approach to solving scaling and convergence issues associated with a large-scale MPLS deployment, while ensuring end-to-end service provisioning and monitoring.

Unified MPLS adopts a strategy in which the core, aggregation, and access networks are partitioned in different MPLS/IP domains that are isolated at the IGP level, but are still integrated through BGP labeled-unicast (BGP-LU) for the forwarding of unicast traffic

The MPLS label-mapping information for the route is carried in the BGP update message that contains the information about the route.

If the next hop is not changed, the label is preserved and the label changes if the next hop changes. In Unified MPLS, the next hop changes at Area Border Routers (ABRs).

Configure BGP-LU between ABR routers of each IGP domain to carry MPLS label information in NLRI.

![](<../.gitbook/assets/Unknown image (1522)>)

#### BGP labeled-unicast (BGP-LU)

Label mapping information is carried as part of the Network Layer Reachability Information (NLRI) in the Multiprotocol Extensions attributes inside of a BGP UPDATE message (Picture 2). AFI value 1 identifies the address family of the associated route. SAFI 4 indicates that NLRI contains a label. A single UPDATE message can carry multiple routes, each route has its own label.

BGP speaker uses BGP-LU to attach the MPLS label to an advertised Interior Gateway protocol (IGP) prefix and distribute the MPLS label mapped to the prefix to its peers.

The speaker indicates its capability to carry the labels mapping information inside of an OPEN message. If the peer does not support this capability, it sends a NOTIFICATION message to the speaker with the Error Subcode set to Unsupported Optional Parameter, as it is defined in RFC5492. This ensures that the speaker sends the label mapping information inside of the BGP UPDATE message only when the peer can actually process an UPDATE message.

IPv6 labeled unicast is used in a 6PE scenario, where the provider’s core MPLS network is IPv4 and is being used to connect the IPv6 speaking PE routers. IPv4 labeled unicast is used to connect multiple regions that may run different IGPs, with no redistribution from one IGP to another. For instance, one region can operate IS-IS while other region can operate OSPF. When a single IGP is used, regions consist of individual OSPF areas or IS-IS levels. No IGP routing information, LDP or RSVP signaling are exchanged between the regions, BGP-LU is used for the routes distribution along with their labels. For this reason, we must implement a configuration that prevents exchanging routing information outside of a region within the OSPF area or the IS-IS level.

BGP-LU is used to provide connectivity between regions by advertising PE loopbacks and label bindings to the Regional Border Routers (RBR). RBRs then advertise the loopbacks and label bindings to remote PEs in other regions. BGP-LU advertisements only impact the PE routers and the border routers and not the transport routers in the middle of the connectivity chain.

When the BGP peers are adjacent, a BGP speaker can push the label advertised by its neighbor for a certain prefix onto an MPLS packet for forwarding. However, when the BGP peers are not adjacent, separated by the MPLS network, there must be a label-switched path (LSP) between the label switch routers (LSRs). Every LSR router in the region must take an appropriate action (push or swap) on LDP or a label distributed by LSR (RFC3107)

In IOS-XE

| (config-router-af)# send-label | must be explicitly configured to enable labeled unicast AFI to exchange labels with the neighbor |
| ------------------------------ | ------------------------------------------------------------------------------------------------ |

In IOS-XR

| allocate-label all                                                                                                                                            |                                                      |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| address-family ipv4 labeled-unicast next-hop-self neighbor 172.16.100.3 remote-as 1 update-source Loopback0 address-family ipv4 labeled-unicast next-hop-self | must be enabled for each neighbor to exchange labels |

#### Operation

When packet forwarding occurs, the routers within an area or segment still use label distribution protocol (LDP) labels to forward the packet towards the closest border router. The router that is adjacent to the border router will pop (remove) the LDP label, so that the border router will receive the packet with the BGP label exposed, identifying the target router that is in another area. The border router will then know where to forward the packet based on this label and will prepend the LDP label belonging to the LSP of the next border router.

Because the network is a single BGP autonomous system (AS), all sessions are IBGP sessions. Each segment runs its own IGP and LDP LSP paths within the IGP domain.

Within Cisco Unified MPLS, the routers (ABRs) that join the segments together must be BGP inline route reflectors, with the next-hop-self and RFC 3107, to carry IPv4 prefixes and their corresponding labels, as configured in the BGP sessions. This is to prevent full mesh scalability issues, where without Route reflector all iBGP peers would have to have peering between each other

**Control plane view**

![](<../.gitbook/assets/Unknown image (1523)>)

![](<../.gitbook/assets/Unknown image (1524)>)

**Data plane view**

3 labels - the bottom most for VPN, the second label for BGP label describing the border router of the orignating domain and the topmost LDP label used to label switch throughout individual domain

![](<../.gitbook/assets/Unknown image (1525)>)

![](<../.gitbook/assets/Unknown image (1526)>)

### Inter-domain MPLS VPN solutions

**Inter-domain MPLS VPN** is primarily used to extend a single customer VPN across multiple autonomous systems operated by different service providers or distinct administrative domains of the same provider.

A second use case is architectural migration, where an existing MPLS or legacy WAN environment must be interconnected with a new network domain during phased technology or provider transitions.

Inter-domain MPLS VPN enables coexistence between old and new architectures, allowing customers to remain operational while sites are gradually migrated, rehomed, or readdressed

The inter-domain boundary acts as a controlled demarcation point where routing policies, label distribution, and QoS behaviors can be adapted without requiring simultaneous changes across the entire network.

Two methods are available: Inter-AS with options and separate solution called CSC

Both solutions introduces techniques to establish MPLS VPNs across multiple autonomous systems.

**Inter-AS** is a peer-to-peer type model that allows the extension of VPNs through multiple provider or multidomain networks. This solution enables service providers to peer up with one another and offer end-to-end VPN connectivity over extended geographical locations for those subscribers who may be out of reach for a single provider. Customer’s sites distribute reachability information directly to the participating Service Providers

**CSC (Carrier Supporting Carrier)** is a client-server model type where an MPLS VPN provider is a customer of another MPLS VPN backbone provider. CSC backbone does not have to know about end-user sites and their IP addresses.

![](<../.gitbook/assets/Unknown image (1527)>)

#### Inter-AS option A: Back-to-back VRF

The **back-to-back VRF inter-AS method** is the simplest method suitable when two service providers offer jointly only a small number of MPLS VPNs.

ASBRs are interconnected using multiple interfaces or subinterfaces. Each interface or subinterface is used to carry the traffic of its own VPN - each is assigned to the customer VRF

Packets are forwarded as IP packets between the ASBRs. Each ASBR is acting as a PE router for its customers and a CE router for customers of other service providers. An IGP or BGP are used to exchange customer routing information between ASBRs.

The two ASBRs treat one another as CE routers and advertise unlabeled IPv4 routes through an EBGP session.

The sub-interfaces facing the other AS doesn’t transport labeled traffic, only regular IP traffic. In order to exchange routing information with the remote ASBR, any routing protocol can be used.

ASBR needs to process routes of all VPN customers.

Suitable when the number of VPNs is small. Not scalable.

The isolation of VRFs per VPN at the ASBR is accomplished by defining:

One sub-interface per VRF

One VRF per VPN (the VRF needs to be instantiated on the ASBR)

One EBGP (if BGP is used) routing session per VRF

![](<../.gitbook/assets/Unknown image (1528)>)

#### Inter-AS option B: Single-hop MP-eBGP (MP-BGP)

The **single-hop MP-eBGP method** provides more scalability than the back-to-back VRF method. Only one link is configured between service providers.

MP-BGP is used to exchange VPNv4 prefix routing and label information between directly attached ASBRs.

To construct the LSP path, next-hop addresses should be reachable. Routes to BGP peers can be redistributed to the provider IGP, or next-hop-self can be used by having the ASBR replace the next hop with its IP address.

mpls bgp forwarding and “no bgp default route-target filter” must be configured on ASBR’s

| (config-if)# mpls bgp forwarding                                                                               | Must be configured on the inter-as link between ASBRs By default, if a router interface is not running a standard label distribution protocol like LDP or RSVP-TE, it will strip any received MPLS labels and attempt to forward the packet based on a standard IP lookup The Cisco command mpls bgp forwarding (or an equivalent feature on other platforms) on the ASBR's inter-AS interface signals to the router that it should accept and process MPLS-encapsulated traffic that has been labeled through BGP, rather than stripping the labels. It essentially enables labeled switching over an interface that is not running another Label Distribution Protocol                                                                                                                                                 |
| -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| (config-router)#no bgp default route-target filter RP/0/RP0/CPU0:router(config-bgp-af)#retain route-target all | This command disable default route-target filtering and allows the propagation of all of the VPNv4 routes among AS With the commands: ASBR1 accepts and retains all VPNv4 routes, allowing them to reach PE1, which can and import them into the correct VRFs (e.g., VPN-A with RT 100:1). Related to Inter-AS Option B and C deployment By default, PE routers ignore VPNv4 updates that does not fit any of configured import route target set on the router If you configure the router for BGP route-target community filtering, all received exterior BGP (EBGP) VPN-IPv4 routes are discarded when those routes do not contain a route-target community value that matches the import list of any configured VPN routing/VRF. This is the desired behavior for a router configured as a provider edge (PE) router. |

![](<../.gitbook/assets/Unknown image (1529)>)

![](<../.gitbook/assets/Unknown image (1530)>)

#### Inter-AS option AB (Cisco proprietary)

Is a cisco proprietary solution combining the benefits of Option A & Option B

Single MP-eBGP peer session between ASBRs leads to better scaling and reduced configurations.

Separate per VRF interfaces between ASBRs forward data as in Option A.

This provides security and QoS benefits of IP forwarding on the I-AS link.

ASBRs are still required to hold VPN routes

[https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/mpls/b-mpls/m\_mp-vpn-ias-optab.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/mpls/b-mpls/m_mp-vpn-ias-optab.html)

#### Inter-AS option C: Multihop MP-eBGP (typically between RRs)

ASBRs do not hold any VPNv4 prefix/label information.

They exchange IPv4 routes with labels between directly connected ASBRs using eBGP

Only PE loopback addresses need to be exchanged (they are BGP next-hop addresses of the VPN routes)

MP-EBGP peering between route reflectors in different ASs.

VPNv4 routes are exchanged between the route reflectors.

BGP is used for label distribution between ASBRs.

End-to-end LSP is built from ingress PE to egress PE.

Requires Multihop MP-eBGP (with next-hop-unchanged command)

| neighbor next-hop-unchanged | BGP do not modify the BGP next-hop attribute when advertising prefixes to an external eBGP neighbor. |
| --------------------------- | ---------------------------------------------------------------------------------------------------- |

Note that multi-hop MP-eBGP does not HAVE to be between RRs, it could also be between ASBRs directly as shown in option 2 previously.

Pros

The multihop MP-EBGP method is highly scalable, because there is no route overhead on ASBRs.

Separation of control and forwarding planes

Route Reflectors exchange VPNv4 routes+labels

ASBRs exchange only IPv4 routes+labels and forwards MPLS packets

You can use a route map or route policy to filter the distribution of MPLS labels between routers.

Cons

Advertising PE addresses to another AS may not be acceptable to some providers.

QoS enforcement per VPN is not possible at ASBR

VPN context doesn’t exist at ASBRs

Not possible to perform policing, filtering or accounting with per VPN granularity at ASBR

![](<../.gitbook/assets/Unknown image (1531)>)

![](<../.gitbook/assets/Unknown image (1532)>)

#### Inter-AS deployment guidelines

Use ASN in the route-target i.e. ASN:xxxx

Max-prefix limit (both BGP and VRF) on PEs

Security (BGP MD5, BGP filtering, BGP max-prefix etc.) on ASBRs

By default the Router Target numbering convention and policy has to be end-to-end and the same between service providers. That's a huge limitation for adaptation between carriers - [Route-Target rewrite on ASBR](onenote:https://d.docs.live.net/B03DD2DFB2522723/Notes/L3.one#VRF\&section-id={6EC9C333-3E04-41C1-8C01-0E8D85C128A8}\&page-id={6F2A2144-4B35-477D-8069-44EE467E73B4}\&object-id={A96C6ACA-1E3E-04E6-2565-F08F26B7679B}&5E)

End-to-end QoS agreement on ASBRs

If SP1 is offering 3 types of IP ToS (0,4,5) and SP2 is offering 4 types of DSCP (EF, AF31, AF21, BE), then it could be a problem.

If they use fixed mapping for the Inter-AS, then VPN customers may suffer (since some vpn customers could map EF and AF31 to ToS=5, whereas others could map only EF to ToS=5)

But they can certainly use Inter-AS option (a) which is a back-to-back VRF.

### Carrier Supporting Carrier (CSC)

**Carrier Supporting Carrier (CSC)** creates a hierarchical structure with a first-level service provider as the backbone carrier and a second-level service provider as customer carrier. Many customer carrier sites, also called POP sites, can be interconnected using the backbone carrier.

The customer service providers do not have to operate their own long-distance network. They purchase that MPLS VPN service from the CSC backbone carrier, so that they can distribute PE loopacks between domains in order to exchange labels to establish LSP's - it is essentially MPLS in MPLS

Different customer carriers can use different addressing schemes. The customer carriers will be in separate MPLS VPNs/VRF inside the CSC backbone.

![](<../.gitbook/assets/Unknown image (1533)>)

#### CSC operation

The CSC architecture relies on the presence of an MPLS VPN. The CSC backbone is providing an MPLS VPN service to which the customer carriers are connected as VPN sites. MPLS is used between the CSC backbone provider edge (PE) routers and the VPN sites of the customer carriers.

VRF tables are enabled on the CSC PE routers.The label exchange between PE1 and PE2 establishes a LSP from CE1 via the CSC backbone to CE2. Another LSP is also established in the other direction. The CSC backbone does not have to know about end-user sites and their IP addresses.

![](<../.gitbook/assets/Unknown image (1534)>)

The customer carrier can connect to the backbone carrier in the following ways:

**Using LDP and an IGP**

MPLS has to be enabled on the link between the backbone carrier and the customer carrier. LDP is used for label distribution. IGP is used for route exchange. Less common and generally not used.

![](<../.gitbook/assets/Unknown image (1535)>)

The label stack consists of two labels in the customer carrier network and of three labels in the backbone carrier network.

![](<../.gitbook/assets/Unknown image (1536)>)

**Using MP-eBGP**

You can use MP-EBGP, instead of LDP, to exchange the labels of the CSC-PE Loopback addresses. This method is often preferred by service providers who traditionally lean towards BGP for inter-domain routing purposes. LDP must be disabled on the links connecting the customer carrier and the backbone carrier networks, because the two label signaling methods are mutually exclusive.

![](<../.gitbook/assets/Unknown image (1537)>)

The label stack consists of two labels in the customer carrier network and of three labels in the backbone carrier network.

CSC-CE1 swaps the LDP labels against a MP-EBGP label (BGP2) received from PE1 for the CSC-PE2's Loopback network. PE1 swaps the BGP label against a VPN label of the backbone carrier (VPN1), received via MP-IBGP from PE2, and inserts a third label (LDP) to enable the packet to reach PE2. When the packet reaches PE2, the PE2 swaps the VPN label of the backbone carrier (VPN1) against the BGP label (BGP1) received via MP-EBGP from CSC-CE2. CSC-CE2 swaps this BGP label against an LDP label that identifies the LSP towards CSC-PE2. Due to penultimate hop popping, the packet arrives at CSC-PE2 with a single label: the VPN label. The VPN label identifies the packets from this customer.

![](<../.gitbook/assets/Unknown image (1538)>)

### Inter-AS L2VPN options

#### Option A

Physical interface, per VLAN OR sub-interface per vlan

each SP treats the other as CE

PW terminates at ASBR

The link between ASBR is AC instead of a PW

Granular QoS control between ASBR

Simple solution, no special feature required

Not scalable, max 4k sub-interface per ASBR link (-> max 4K PW across the link)

ASBR redundancy supported (MC-LAG or MST AG and MST)

![](<../.gitbook/assets/Unknown image (1539)>)

#### Option B

Multisegmented pseudowires are created to establish inter-as connections between the terminating PE's in each AS

The first segment establishes a path between the TPE in AS1 to ASBR1.

The next segment establishes a path between the ASBR1 and ASBR2

The final segment establishes a path between ASBR2 to the TPE in AS2.

When you are configuring the PEs for the L2VPN VPLS Inter-AS Option B feature, use the terminating-pe tie-breaker command to negotiate the mode of the TPE.

Then use the mpls ldp discovery targeted-hello accept command to ensure that a passive TPE can accept (LDP) sessions from the LDP peers

Two options how to configure: Manual PW stitiching or using VFI and BGP autodiscovery to automatically set up the stitching (in the SP2 lab guide)

![](<../.gitbook/assets/Unknown image (1540)>)

For the manual stitch you attach the two pseudowires on the ASBR to the VFI - same for IOS-XR

![](<../.gitbook/assets/Unknown image (1541)>)

| l2vpn xconnect group SPx-SPy p2p service-inter-as neighbor 2.2.2.2 pw-id 100 neighbor 3.3.3.3 pw-id 101 |   |
| ------------------------------------------------------------------------------------------------------- | - |

#### Option C

End to End Pseudowire, like Option C for L3VPN

Instead of termination of the PW on ASBR, the PW is extended end to end between the two ASes

Target LDP session is established directly between PEs across AS boundary

MPLS LSP is required between two PEs, thus PE loopback address is leaked into other AS (IPv4+label)

Single physical interface between ASBRs

![](<../.gitbook/assets/Unknown image (1542)>)

### Pseudowire headend (PWHE)

In a setup where an MPLS L2 VPN access network of the service provider is used to extend the L3 service to customer, it is typically required to use two separate PEs — one to terminate the L2 VPN (pseudowire) between the CE and the L3 PE, which then terminate the L3VPN service

**PWHE (Pseudowire Headend)** is a technology that allows termination of access PWs directly into an L3 domain (VRF or global), eliminating the need to maintain separate interfaces or devices for the pseudowire and the L3VPN service.

PWHE introduces the construct of a "pw-ether" interface on the PE device. This virtual pw-ether interface terminates the PWs carrying traffic from the CPE device and maps directly to an MPLS VPN VRF on the provider edge device. Any L3,QoS and ACLs are applied to the pw-ether interface

Benefits:

Dissociates the Customer Facing Interface (CFI) of the Service PE from the underlying physical transport media of the access or aggregation network

Normally, a Customer-Facing Interface (CFI) on a PE is tied to a physical port (GigE, TenGigE, etc.).

With PWHE, the CFI can instead be a logical pw-ether interface, which isn’t bound to a single physical cable.

Example: you can have two uplinks (say, from an access/aggregation network), both carrying pseudowires, but they terminate on the same pw-ether - Similar to loopback

Result: the customer sees one logical interface, while the provider gets built-in redundancy/failover/loadbalancing

Without PWHE, scaling CFIs means more physical ports or more BDIs, which hit platform limits quickly.

PWHE allows you to create many virtual interfaces (pw-ether) — far more scalable than BDIs - up to 1k pw-ether interfaces, while BDI is limited to 255 interfaces

This means you can onboard more customers or services without being constrained by physical port count.

Reduces CAPEX in the access or aggregation network and service PE - lower hardware cost, fewer optics, less cabling.

Distributes and scales the customer-facing Layer 2 UNI interface set - Customer UNI ports no longer need to map 1:1 with PE hardware ports. With PWHE, a large set of customer-facing UNI interfaces can be aggregated logically over pseudowires into the service PE. This “distribution” means you can offer services deeper into the access/aggregation network, while still managing everything centrally at the service PE.

Expands the Reach of Layer 3 Services

pw-ether interfaces also supports egress HQOS or outbound ACL's - BDI doesn't

Without PWE:

![](<../.gitbook/assets/Unknown image (1543)>)

With PWE:

![](<../.gitbook/assets/Unknown image (1544)>)

#### PWHE in IOS XR

Access device is configured with XConnect on the interface connecting to the branch/campus router. The XConnect peer is configured as the PE loopback address. On PE PW-ether, an interface is created on which the XConnect is terminating. The same PW-ether interface is also configured with VRF and L3VPN service is configured on it. The PE and CE can use any routing protocol to exchange route information over PW-Ether Interface. BFD is used between PE and CE for fast failure detection.

![](<../.gitbook/assets/Unknown image (1545)>)

| generic-interface-list il1 interface TenGigE0/0/0/0 interface TenGigE0/0/0/3 interface pw-ether 100 vrf vpn-green ipv4 address 10.1.1.2/24 service-policy input pw\_in service-policy output pw\_out ipv4 access-group pw-in-acl in ipv4 access-group pw-out-acl out attach generic-interface-list il1 l2vpn xconnect group pwhe p2p PWE3 interface pw-ether 100 neighbor 100.100.100.100 pw-id 28 | A generic interface list contains a list of physical or bundle interfaces connected to the MPLS access network - since it can be connected to two P routers allowing for load-balancing/failover capability |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
