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

# MPLS L3VPN

### VPN fundamentals

**VPN** is a collection of sites that share a common routing information

Virtual Private Networks replace dedicated point-to-point links with emulated point-to-point links sharing a common infrastructure

Customers use VPNs primarily to reduce their operational cost

To overcome the scalability issue of overlay VPN, the peer-to-peer VPN concept was introduced

The service provider actively participates in customer routing, carrying all routes from customers and transporting them across the service provider backbone, and finally propagating them to other customer sites. Thus SP becomes responsible for the customer convergence and must have more detailed IP routing knowledge

#### VPN topologies

**Overlay VPNs** are sorted according to the topology of virtual circuits:

Hub-and-spoke topology (Redundant- dual star)

Partial-mesh topology

Full-mesh topology

#### VPN categories (business needs)

**Intranet VPNs** – interconnect sites within an organization

**Extranet VPNs** – interconnect different organizations in a secure way

**Access VPNs** – Virtual Private Dialup Networks (VPDNs) offer a dial-up access to customer’s network - e.g remote access VPNs - provide remote users with secure access to a private network (such as a company’s internal network) over a public network, typically the internet

#### VPN categories (connectivity requirements among sites)

**Simple VPN** – each site can communicate with every other site within its VPN

**Overlapping VPN** – some sites are connected to more than one VPN

**Central Services VPN** – every sites can communicate with central servers but not with each other

**Managed Network** – a dedicated VPN established for CE routers management

### VPN service delivery models

#### Traditional VPN (point-to-point overlay)

Service Provider offers virtual point-to-point links between the customer’s sites

The formula to calculate how many point-to-point links or VCs are needed is (\[n]\*\[n-1])/2, where n is the number of sites to be connected

For example, if a customer wants to have a full mesh between 10 sites, it would need 10\*9/2=45 point-to- point links. This would certainly be a scalability issue.

#### Peer-to-peer VPN

To overcome the scalability issue and provide the customer with optimum data transport across the service provider backbone, the peer-to-peer VPN concept was introduced

The service provider actively participates in customer routing, accepting customer routes, transporting those customer routes across the service provider backbone, and finally propagating them to other customer sites.

![](<../.gitbook/assets/Unknown image (1771)>)

### MPLS L3VPN

**MPLS L3VPN** are deployed by service providers to provide L3 network connectivity for customer's remote location using an MPLS network transport.

It is a combination of MPLS, OSPF, BGP and VRFs to provide secure and scalable L3 VPNs

BGP was chosen as the universal prefix redistribution protocol for MPLS VPNs. To support these new features, BGP functionality has been enhanced to handle the VRF specific routes.

In the context of MPLS L3 VPN **VRF** is a routing and forwarding instance for a group of sites with the same connectivity requirements

In the context of an MPLS L3 VPN, the Forwarding Equivalence Class (FEC) is the VPN-IPv4 prefix, which is constructed by concatenating the Route Distinguisher (RD) of the VPN with the customer's IPv4 prefix. Also the FEC can be just the RD distinguishing the VPN- e.g per VPN/VRF label and not for each prefix in a VPN.

A new special MP-BGP (multiprotocol BGP) address family named VPNv4 (VPN IPv4) has been added to BGP along with a new NLRI format.

Every VPNv4 prefix has the RD associated with it and the corresponding MPLS label, in addition to the normal BGP attributes.

This allows for transporting different VPN routes together and performing best-path selection independently for each different RD.

**Route Distinguisher (RD)** is 64-bit (8-byte) distinguisher prepended to each 32bit IPv4 route within VRF instance, the resulting 96-bit address is called a VPNv4 address

The VPNv4 is then propagated via MP-BGP to the other PE neighbor

The RD’s role is only to make routes unique. Using a per-PE RD ensures that identical prefixes from different PEs are always distinct in VPNv4/VPNv6 BGP.

Best practice is to use a unique RD per VRF per PE.

In other words:

Same PE with multiple VRFs → different RD per VRF

RD identifies the route origin, not the VPN membership

RD = \<IP>:\<ID>

or

RD = \<ASN>:\<ID>

**Route Targets (RT)** is an additional 64bit BGP Extended community attribute attached to VPNv4 BGP routes to control VPN membership e.g. import/export policy. Any number of “route targets“ can be attached to a single route

![](<../.gitbook/assets/Unknown image (1772)>)

### Architecture

**Provider Edge (PE)** routers are running separate VRF for each customer, allowing customers to use the same overlapping address space

PE establish static or dynamic routing with **Customer edge (CE)** routers to receive and install customer routes in appropriate VRF and transport it over the MPLS network, to another PE router via MP-BGP which also installs them to the appropriate VRF table and removes the Route Distinguisher from the VPNv4 prefix, the result is 32-bit IPv4 that the PE sends to the CE

Since RIP and older version of routing protocols doesn't support VPN and can run only single process within IOS, the PE and CE routing is implemented either as several instances of one routing process (eBGP, RIPv2, EIGRP, IS-IS) or as several routing processes (OSPF) - called routing context

PE routers form an IGP (OSPF or IS-IS) neighborship with the P routers to exchange loopback IP's

PE and P routers also establish LDP neighborships, so that the PE routers can label packets with MPLS labels and P routers can use these labels for fast label-switching packets.

Routes are then exchanged between PE devices using the MP-BGP IBGP MP-BGP VPNv4 peering between PE/P loopbacks which exchanges inner VPNv4 labels

A full mesh of IBGP sessions is required among PE-routers - as the BGP path attributes must be preserved - acting like an transit AS

For scalability reasons, service provider core P routers do not have any customer routing information, they are fully transparent to the customer networks devices

There are special limitations for iBGP peering sessions that you want to enable for VPNv4 prefix exchange. First, they must be sourced from a Loopback interface, and second, this interface must have a /32 mask (this is not a strict requirement on all platforms). This is needed because the BGP peering IP address is used as the NEXT\_HOP for the locally originated VPNv4 prefixes. When the remote BGP router receives those prefixes, it performs a recursive routing lookup for the NEXT\_HOP value and finds a label in the LFIB. This label is used as the transport label in the receiving router. Effectively, the NEXT\_HOP is used to build the tunnel or the transport LSP between the PEs.

To inject a particular VRF’s routes into BGP, you must activate the respective address-family under the BGP process and enable route redistribution (such as static or connected).

All the respective routes belonging to that particular VRF will be injected into the BGP table with their RDs and have their VPN labels generated.

![](<../.gitbook/assets/Unknown image (1773)>)

The MP-BGP update contains:

VPNv4 address

Extended communities (route targets, optionally site-of-origin)

Labels used for VPN packet forwarding

Other BGP attributes (AS-Path, Local Preference, MED, standard community …)

### MPLS VPN packet forwarding (data plane)

If only one label was used, it would be used by the P routers to label-switch the packet to the egress PE router, however as soon as the egress PE pops the label it would not know to which VRF/VPN the IP packet belongs to

![](<../.gitbook/assets/Unknown image (1774)>)

**VPN Label** is used to switch the customer's traffic itself - the PE router needs to know for which VRF this packet is intended, which it determines based on the label with which the packet arrived, which is signaled via MP-BGP PE to neighbors as VPNV4 route with the appropriate route distinguisher

Simply put - VPN label is used for the data plane itself e.g labeling and label swapping the packets themselves, whereas the Route distinguisher is used for the initial signaling of the control plane - i.e. association of VPNv4 routes with the label as well as route targets (to dictate the import/export of routes)

For MPLS L3 VPN to work two labels in the MPLS stack is required; one label (the topmost) is the transport label, which is being swapped along the entire path between the PEs, and the other label (innermost) is the VPN label to find the target VRF, so that the egress PE can then perform an IP lookup within the VRF and forward it to the outgoing exit interface

Labels are propagated within the MP-BGP VPNv4 routing updates.

Usually the PHP is implemented on the penultimate P routers

![](<../.gitbook/assets/Unknown image (1775)>)

#### VPN label requirements

MPLS VPN packet forwarding works correctly only if the router specified as the BGP next hop in the incoming BGP update is the same PE router that assigned the second label in the label stack. Here are three scenarios that can cause the BGP next hop to be different from the IP address of the PE router assigning the VPN label:

If the customer route is received from the CE router via an EBGP session, the next hop of the VPNv4 route is still the IP address of the CE router (the BGP next hop of an outgoing IBGP update is always identical to the BGP next hop of the incoming EBGP update). You must configure the next-hop-self command on the MP-BGP sessions between PE routers to make sure that the BGP next hop of the VPNv4 route is always the IP address of the PE router, regardless of the routing protocol used between the PE router and the CE router.

The BGP next hop should not change inside an AS.

#### Broken LSP path

For successful propagation of MPLS VPN packets across an MPLS backbone, there must be an unbroken LSP tunnel between PE routers. This requirement exist because the second label in the stack is recognized only by the egress PE router that has originated it, and will not be understood by any other router if it ever becomes exposed.

MPLS relies on IGP to provide exact prefix reachability so the LDP (or RSVP-TE) can build a label binding for that destination.

If a router receives a summary route (e.g., 10.0.0.0/16) instead of the specific loopback (e.g., 10.0.1.1/32), it won’t have a specific label mapping for that PE’s loopback.

If the P routers perform summarization of the address range within which the IP address of the egress PE router lies, the LSP tunnel will be disrupted at the summarization point.

In the figure, the P router summarizes the loopback address of the egress PE router. The LSP tunnel is broken at a summarization point, so the summarizing router needs to perform full IP lookup. In an MPLS network, the P router would request PHP for the summary route, and the upstream P router (or a PE router) would remove the LDP label, exposing the VPN label to the P router. Because the VPN label is assigned not by the P router but by the egress PE router, the label will not be understood by the P router and the VPN packet will be dropped or misrouted.

Since in OSPF the ABRs would aggregate certain area, the core should be only in a single area to ensure full visibility of loopback interfaces

However this scenario less likely to occur:

In OSPF Aggregation is only allowed on ABRs, so if your core is a flat backbone (as it usually is in MPLS), there is no natural aggregation point for PE loopbacks.

In IS-IS Even though IS-IS allows summarization at level boundaries, core networks are typically Level-2 only, so again — no summarization is typically done in the core.

![](<../.gitbook/assets/Unknown image (1776)>)

### Configuration

1. Configuration of VRF instances and assigning the interface under the VRF on PE routers
2. Configuration of Multi-Protocol BGP sessions between PE routers

| router bgp 1 neighbor 172.16.0.13 remote-as 1 neighbor 172.16.0.13 update-source Loopback0                                     | Every MP-BGP neighbors must be configured in the global BGP routing configuration MP-IBGP sessions should be running between loopback interfaces                                                                                                                                                                                                                                                                                                                                                                                                                |
| ------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-router)# no bgp default ipv4-unicast                                                                                   | VPNv4 address-family capability is activated per-neighbor using the respective address-family configuration. By default, when you create a new BGP neighbor using the command neighbor \<I\P> remote-as , the default IPv4 unicast address-family is activated for this neighbor. If for some reason you don’t want this behavior and only need the VPNv4 prefixes to be sent, you may disable the default behavior via the command no bgp default ipv4-unicast                                                                                                 |
| address-family vpnv4 neighbor 172.16.0.13 activate neighbor 172.16.0.13 send-community both neighbor 172.16.0.13 next-hop-self | Configure BGP address family VPNv4. Activate configured BGP neighbor for VPNv4 route exchange. Configures the propagation of the standard and extended BGP communities - for VPNv4 prefixes Extended BGP communities are propagated by default because their propagation is mandatory for successful MPLS VPN operation. Note XR don’t need this command, it is default for iBGP! For address family IPv4 you can use: send-community-ebgp / send-extended-community-ebgp VPNv4 is re-originated from IPv4 -> next-hop-self not needed anymore, but recommended |
| show ip bgp vpnv4 all summary                                                                                                  | monitoring                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

3. Configuration of PE-CE routing

Can be configured either with static or dynamic routing protocol:

Static - for simple scenarios, always use fully specified static route, nextop hop int and IP

RIP - RIPv2 must be used to support routing contexts e.g. VRFs for one routing process

OSPF (not recommended, but commonly deployed in real environments)

BGP (preferred)

EIGRP (not recommended)

| router bgp 100 no synchronization bgp log-neighbor-changes network 10.1.1.0 mask 255.255.255.0 neighbor 100.1.1.1 remote-as 1 no auto-summary                                       | CE                                                                                                                                                                                                                                                                             |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| router bgp 1 address-family ipv4 vrf A-1 redistribute connected redistribute rip neighbor 100.1.1.2 remote-as 100 neighbor 100.1.1.2 activate exit-address-family                   | PE config CE neighbors are configured within the VRF context, not in the global BGP config                                                                                                                                                                                     |
| IOS XE Static routing from PE perspective example ip route vrf Customer\_A 10.0.0.0 255.0.0.0 10.250.0.2 ! router bgp 65173 address-family ipv4 vrf Customer\_A redistribute static | IOS XR Static routing from PE perspective example RP/0/RSP0/CPU0:Router-IOS-XR(config)# router static vrf Customer\_A address-family ipv4 unicast 10.0.2.0/24 192.168.0.1 ! router bgp 64500 vrf Customer\_A rd 64500:1 address-family ipv4 unicast redistribute static commit |

PE-CE routing verification commands

| show ip route vrf               |                                                                                       |
| ------------------------------- | ------------------------------------------------------------------------------------- |
| show ip prot vrf                |                                                                                       |
| show ip bgp vpnv4 unicast       |                                                                                       |
| show ip bgp vpnv4 rd rd-value   |                                                                                       |
| show ip cef vrf VRF-name detail | first label is LDP pointing to the next-hop, the second label is for the VPN labeling |
| sh ip bgp vpnv4 all labels      |                                                                                       |

Example: On PE1 - Label 31 is used as the VPNv4 label - there is per VPN label and not per IP prefix

![](<../.gitbook/assets/Unknown image (1777)>)

![](<../.gitbook/assets/Unknown image (1778)>)

Label 30 is used to send it to the next hop LSR (LDP label)

![](<../.gitbook/assets/Unknown image (1779)>)

{% hint style="info" %}
On IOS-XR, each IP prefix within a VRF is assigned a label.
{% endhint %}

![](<../.gitbook/assets/Unknown image (1780)>)

Another example IOS-XE

![](<../.gitbook/assets/Unknown image (1781)>)

Another example IOS-XR

![](<../.gitbook/assets/Unknown image (1782)>)

### PE-CE routing protocol options

#### BGP between PE and CE routers

If two sites of the same customer use BGP and have the same ASN, they don’t receive their routes, due to AS\_path loop prevention.

This can be fixed with either allowas-in on the CE side site or as-override on the PE side. However this bypass introduces another source of potential loops, since CE routers accepts routes from the same AS and the PE routers might advertise routes back to their origin without recognizing loops. This scenario can be fixed with Site of Origin (SOO)

MPLS VPN packet forwarding works correctly only if the router specified as the BGP next hop in the incoming BGP update is the same PE router that assigned the second label in the label stack. Here are three scenarios that can cause the BGP next hop to be different from the IP address of the PE router assigning the VPN label:

If the customer route is received from the CE router via an EBGP session, the next hop of the VPNv4 route is still the IP address of the CE router (the BGP next hop of an outgoing IBGP update is always identical to the BGP next hop of the incoming EBGP update). You must configure the next-hop-self command on the MP-BGP sessions between PE routers to make sure that the BGP next hop of the VPNv4 route is always the IP address of the PE router, regardless of the routing protocol used between the PE router and the CE router.

The BGP next hop should not change inside an AS.

The BGP next hop is always changed on an EBGP session. If the MPLS VPN network spans multiple public autonomous systems, special provisions must be made in the AS boundary routers to reoriginate the VPN label at the same time that the BGP next hop is changed.

![](<../.gitbook/assets/Unknown image (1783)>)

#### EIGRP between PE and CE routers

Routes between two sites over MPLS are considered as internal EIGRP routes. EIGRP vector values get encoded into the packets as MP-BGP extended communities

The IGP metric is always copied into the multi-exit discriminator (MED) attribute of the BGP route when an IGP route is redistributed into BGP. Within a standard BGP implementation, the MED attribute is used only as a route-selection criterion. The MED attribute is not copied back into the IGP metric. The metric must be configured for routes from external EIGRP autonomous systems and non-EIGRP networks before these routes can be redistributed into an EIGRP CE router.

In an MPLS VPN environment, the original EIGRP metrics must be carried inside MP-BGP updates. This configuration is achieved by using BGP extended community attributes to carry and preserve EIGRP metrics when crossing the MP-IBGP domain. These communities define the intrinsic characteristics that are associated with EIGRP, such as the AS number or EIGRP cost metric (bandwidth, delay, load, reliability, and MTU, for example).

XE and XR example

| Router-IOS/XE(config)# router eigrp autonomous-system-number address-family ipv4 vrf vrf-name autonomous-system as-number redistribute bgp as-number metric metric-value | RP/0/RSP0/CPU0:Router-IOS-XR(config)# router eigrp autonomous-system-number vrf vrf-name address-family ipv4 autonomous-system as-number redistribute bgp as-number metric metric-value commit |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Routing loops and suboptimal routing generally occur because of mutual redistribution taking place between EIGRP PE-CE and MP-BGP in an MPLS VPN environment. Routing loops can occur in the following scenarios:

A route that is received by a multihomed site from the backbone through one link can be forwarded back to the backbone through the other link.

A route that originated in a multihomed site and that was sent to the backbone through one link can come back through the other link.

The SOO attribute is needed only for customer networks with multihomed sites. Loops can never occur in customer networks that have only stub sites.

In conjunction with BGP Cost Community, EIGRP, BGP, and the Routing Information Base (RIB) ensure that paths over the MPLS VPN core are preferred over backdoor links.

![](<../.gitbook/assets/Unknown image (1784)>)

#### OSPF between PE and CE routers

From the MPLS VPN perspective, areas can be customer sites, so area 0 must be between the PE and CE or else a virtual-link between the PE and the nearest ABR of area 0 is needed

A separate OSPF routing process is configured for each VRF running OSPF.

OSPF route type is not preserved during the OSPF route to BGP redistribution.

Every OSPF routes from a site are inserted as an external (type 5 LSA) routes into the other sites.

It is hard to implement OSPF route summarization and stub area.

![](<../.gitbook/assets/Unknown image (1785)>)

Goals for OSPF support in MPLS VPN

OSPF continuity must be provided between OSPF sites:

Internal OSPF routes must remain internal OSPF routes.

External OSPF routes must remain external OSPF routes.

Non-OSPF routes that are redistributed into OSPF must appear as external OSPF routes in OSPF.

OSPF metrics and metric types (external 1 or external 2) must be preserved.

Moreover there can be scenario, where area 0 is on multiple customer locations

By using an MPLS backbone for VPN with OSPF on the customer's site, you can solve these problems by introducing a third level in the hierarchy of the OSPF model. This third level is called the MPLS VPN Super Backbone, which is above backbone Area 0

OSPF superbackbone behaves exactly as Area 0 in a traditional OSPF:

PE routers behave as OSPF area border routers (ABRs)

Routes redistributed from BGP to OSPF appears as either inter-area summary routes or external routes within the other areas (according to their original LSA type)

OSPF paths’ attributes are bound as extended BGP communities to the OSPF routes which are redistributed to MP-BGP.

The OSPF intra-area route, described in the OSPF router LSA or network LSA, is inserted into the OSPF superbackbone by redistributing the OSPF route into MP-BGP. Route summarization can be performed on the redistribution boundary by the PE router.

The MP-BGP route is propagated to other PE routers and inserted as an OSPF route into other OSPF areas.

OSPF cost is copied to the MED attribute

Routes that are redistributed from MP-BGP to OSPF will receive proper attributes. Only internal are redistributed by default, thus always specify in the redistribute commands all OSPF route types

BGP extended attribute for OSPF format: OSPF RT::\<LSA\_Route\_Type>:\<Metric type (related only for LSA 5 and LSA 7, if it is other LSA there is 0)>

| RP/0/RP0/CPU0:Router-IOS-XR(config)# router bgp as-number vrf vrf-name address-family ipv4 unicast redistribute ospf process-id \[match {external \[1\|2] \| internal}] commit | Router-IOS/XE(config)# ! router ospf process-id vrf vrf-name ... Standard OSPF parameters ... redistribute bgp as-number subnets |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------- |

{% hint style="info" %}
Area 0 will receive routes from another Area 0 site as LSA Type 3.
{% endhint %}

<figure><img src="../.gitbook/assets/Unknown image (1786)" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Q:** Should I use the same process number while configuring OSPF on multiple routers within the same network?

**A:** OSPF does not check the process number when forming adjacencies or exchanging routes. The main exception is OSPF on PE-CE links in an MPLS L3VPN. PE routers use the process number to derive the OSPF domain attribute. If process numbers differ on PEs in the same VPN, use `domain-id ospf` to treat them as one domain.

In practice, process numbers can differ and still work. Use consistent process numbers anyway. It simplifies ops.

**IOS-XR output**
{% endhint %}

<figure><img src="../.gitbook/assets/Unknown image (1787)" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Unknown image (1788)" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Routes from the MP-BGP backbone that did not originate in OSPF are still subject to standard redistribution behavior when inserted into OSPF.

These routes are inserted into the OSPF database as Type 5 externals (or Type 7 in NSSA), using the default OSPF metric (not the MED value).
{% endhint %}

<figure><img src="../.gitbook/assets/Unknown image (1789)" alt=""><figcaption></figcaption></figure>

Potential routing loops in OSPF with BGP deployment

As shown in the picture belw, the issue may arise, when you look from the pint of view of the middle PE router, it would see route to Area 1 via left PE and via right PE router, and it may choose the route via right PE (can have better BGP PA metric thant left PE router), which creates routing loop

Solution: Additional down bit was introduced in Options field of OSPF LSA header.

PE routers set the down bit during the MPLS-BGP to OSPF routes redistribution.

PE routers never redistributes OSPF route with down bit set to MP-BGP

![](<../.gitbook/assets/Unknown image (1790)>)

Potential cross-domain routing loops

The down bit helps prevent routing loops between MP-BGP and OSPF, but not when external routes are announced. For example, in case of redistribution between multiple OSPF domains or when external routes are injected in an area that is dual-homed to the provider network. The PE router redistributes an OSPF route from a different OSPF domain into an OSPF domain as an external route. The down bit is not set because LSA Type 5 does not support the down bit. The redistributed route is propagated across the OSPF domain.

A non-MPLS router can then redistribute the OSPF route into another OSPF domain. The OSPF route is propagated through the other OSPF domain, again without the down bit. A PE router receives the OSPF route. When the down bit is missing, the route is redistributed back into the MP-BGP backbone, resulting in a routing loop.

![](<../.gitbook/assets/Unknown image (1791)>)

This is not standard deployment, but it can be solved by traditional manual tagging (since OSPF routes have no native tag field). The tag value is then matched and filtered on the PE router

| redistribute ospf tag |   |
| --------------------- | - |

![](<../.gitbook/assets/Unknown image (1792)>)

Optimizing packet forwarding over MPLS VPN Backbone - with PE connected to two distinct customer sites

To prevent the customer sites from acting as transit parts of the MPLS VPN network, the OSPF route selection rules in PE routers need to be changed.

![](<../.gitbook/assets/Unknown image (1793)>)

The PE-routers must ignore all OSPF routes with the down bit set, as these routes originated in the MP-BGP backbone and the MP-BGP route should be used as the optimum route toward the destination. This rule is implemented with the routing bit in the OSPF LSA.

For routes with the down bit set, the routing bit is cleared and these routes never enter the IP routing table, even if they are selected as the best routes by the Shortest Path First (SPF) algorithm

The routing bit is Cisco’s extension to OSPF and is used only internally in the router. It is never propagated between routers in LSA updates

| IOS: Router (config-router)# capability vrf-lite                  | To prevent the CE to act as PE / To override down-bit/routing-bit processing: Capability VRF-Lite is an MP-BGP/MPLS related feature. When VRF-aware OSPF is configured, the router behaves like a MPLS PE router If you configure OSPF with VRFs on your CE router then it now uses the same checks as the PE routers because it believes it is directly connected to the MPLS network in the way the PE is, even though it isn't. When an OSPF sham-link is configured in the MPLS superbackbone, the capability does NOT need to be enabled |
| ----------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IOS XR: RP/0/0/CPU0:router(config-ospf-vrf)# disable-dn-bit-check |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |

![](<../.gitbook/assets/Unknown image (1794)>)

**Sham-link**

is similar to virtual link, however meant for specific scenario when OSPF is used as routing protocol between PE-CE, while there is a direct (backdoor) link between CE's

If the backdoor links between sites are used only for backup purposes and do not participate in the VPN service, the default route selection may not be desired.

To re-establish the desired path selection over the MPLS VPN backbone, you must create an extra OSPF intra-area (logical) link between ingress and egress VRFs on the relevant PE routers.

This link is called a sham link.

The concept of the sham link is to make Inter-area routes/External Routes learned from provider MPLS VPN as Intra-area routes

A sham link is required between any two VPN sites that belong to the same OSPF area and share an OSPF backdoor link. If no backdoor link exists between the sites, no sham link is needed.

Sham link can be used to allow two different customer locations in the same VPN to be considered part of a single OSPF area (e.g. Area 0), and routes between them to be advertised as intra-area routes using LSA Type 1 (Router LSA) or LSA Type 2 (Network LSA), instead of Type 3 (Summary LSA). A sham link simulates a direct connection between PE routers within the same area, eliminating the inter-area behavior of a superbackbone.

The decision whether the traffic is transmitted across backdoor route or sham-link route is controlled by the cost parameter which is configured for each sham-link.

Sham-Link configuration (on PE router)

Create a loopback interface within the customer VRF on each PE router

Advertise the loopback interface with BGP under the address-family ipv4 vrf with the network statement

Filter the sham-link routes on the PE routers from redistribution into OSPF by using a route-map to deny specific tagged routes

Configure the sham-link under the OSPF VRF process between the PE routers

Important: When using OSPF w/ VRFs on CE routers, the capability vrf-lite command must be entered or else the CE router will block all incoming routes (because the DOWN-bit is set)

| PEx(config)# router ospf process-id vrf VRF-name PEx(config-router)# area area-id sham-link source-address destination-address cost number | XE |
| ------------------------------------------------------------------------------------------------------------------------------------------ | -- |
| PEx# show ip ospf sham-links                                                                                                               |    |
| PEx# show ip ospf database router                                                                                                          |    |
| router ospf vrf VRF-name domain-id type 0005 value 000000640200 redistribute bgp <>area sham-link                                          | XR |

![](<../.gitbook/assets/Unknown image (1795)>)

![](<../.gitbook/assets/Unknown image (1796)>)

#### OSPFv3 as PE-CE routing protocol

In Cisco IOS XR: OSPFv3 supports only IPv6

OSPFv3 in Cisco IOS/XE: Supports both IPv4 and IPv6 address families Provides PE-CE and VRF-Lite capabilities

### MPLS VPN identifier (ID)

is an optional feature that allows you to identify VPNs by a VPN identification number. A VPN ID is useful for remote access applications, such as RADIUS and DHCP that can use the MPLS VPN ID to identify a VPN. RADIUS can use the VPN ID to assign dial-in users to the proper VPN, based on the authentication information of each user.

The VPN ID is stored in the corresponding VRF structure for the VPN. To ensure that the VPN has a consistent VPN ID, assign the same VPN ID to all the routers in the service provider network that service that VPN. The MPLS VPN ID feature is not used to control the distribution of routing information or to associate IP addresses with MPLS VPN ID numbers in routing updates.

Each VPN ID that is defined by RFC 2685 consists of these elements:

An Organizational Unique Identifier (OUI), a three-octet hexadecimal number that is assigned by the IEEE

A VPN index, a four-octet hexadecimal number that identifies the VPN within the company.

| RP/0/RSP0/CPU0:Router-IOS-XR(config-vrf)# vpn id oui:vpn-index |   |
| -------------------------------------------------------------- | - |
| RP/0/RSP0/CPU0:Router-IOS-XR(config-vrf)# commit               |   |
| Router-IOS/XE(config-vrf)# vpn id oui:vpn-index                |   |

### Design considerations

To ensure scalability of the SP MPLS core, an router reflector is implemented, to serve as an central point for route exchange between PE routers, thus each PE router establish peering only with route reflector instead of full mesh neighborship with every other PE router. In SP1 the P-003 router is acting as an route reflector

![](<../.gitbook/assets/Unknown image (1797)>)

MPLS PE-only Setup - Collapsed Core

Is an MPLS network setup consisting only of PE (Provider Edge) routers without dedicated P (Provider) routers is possible and is known as a "Collapsed Core" or "PE-only Network" design. In this setup, the PE routers are directly interconnected and handle both the edge and core functions.

### Complex MPLS L3VPN topologies

Customer having multiple departments or it wants to communicate with other customer, or even they want to access to shared resources

One site can participate in different VPNs

VPN can be understand as a “community of interest“ (Closed User Group — CUG)

A complex VPN topology require more than one virtual routing table per one VPN

RD make customer routes unique so the provider can store and process them appropriately. However, RDs do not control which routes are exchanged between VRFs; that is the role of Route Targets (RTs), which is additional BGP attribute. Extended BGP community attribute encode these attributes as 64-bit values (as opposed to normal 32-bit communities). - Extended communities carry the meaning of attribute together with their value. Any number of RT can be attached to a single route.

Export route targets (RTs) define VPN membership by tagging customer routes when they are converted into VPNv4 routes

Import route targets match the export RTs, enabling the import of the corresponding routes into the appropriate VPN

e.g. A route with a route-target export attribute set in a specific VRF is installed in another VRF only if that VRF has a matching route-target import attribute configured

By default, all prefixes redistributed from a VRF into a BGP process are tagged with the extended community X:Y specified under the VRF configuration via the command route-target export X:Y You may specify as many export commands as you want to tag prefixes with multiple attributes. On the receiving side, the VRF will import the BGP VPNv4 prefixes with the route-targets matching the local command route-target import X:Y. The import process is based entirely on the route-targets, not the RDs.

When a route is imported into a VRF, its RD is replaced with the RD of the importing VRF to integrate it seamlessly into that VRF’s routing table, which ensures that the route appears local to the VRF

| R1(config)# ip vrf BLUE R1(config-vrf)# route-target both 65534:1 | route-target both X:Y means import and export statements at the same time. |
| ----------------------------------------------------------------- | -------------------------------------------------------------------------- |

#### Advanced VPN topologies

VPNs can also be categorized according to the connectivity required between sites:

Simple VPN: Every site can communicate with every other site.

Overlapping VPN: Some sites participate in more than one simple VPN.

Central services VPN: All sites can communicate with central servers but not with each other.

Managed network: A dedicated VPN is established to manage CE routers.

**Overlapping VPNs**

![](<../.gitbook/assets/Unknown image (1798)>)

![](<../.gitbook/assets/Unknown image (1799)>)

![](<../.gitbook/assets/Unknown image (1800)>)

| PE1(config)# ip vrf Cust\_A PE1(config-vrf)# rd 123:750 PE1(config-vrf)# route-target both 123:750 PE1(config)# ip vrf Cust\_B PE1(config-vrf)# rd 123:760 PE1(config-vrf)# route-target both 123:760 PE1(config)# ip vrf A\_Central PE1(config-vrf)# rd 123:751 PE1(config-vrf)# route-target both 123:750 PE1(config-vrf)# route-target both 123:1001 | PE2(config)# ip vrf Cust\_A PE2(config-vrf)# rd 123:750 PE2(config-vrf)# route-target both 123:750 PE2(config)# ip vrf Cust\_B PE2(config-vrf)# rd 123:760 PE2(config-vrf)# route-target both 123:760 PE2(config)# ip vrf B\_Central PE2(config-vrf)# rd 123:761 PE2(config-vrf)# route-target both 123:760 PE2(config-vrf)# route-target both 123:1001 | VRFs with shared RTs can exchange routes, as these RTs are imported/exported in the vrf configurations. |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |

**Central services VPN**

This topology can be used in these situations:

The service provider offers services to all customers in a common VPN.

Two (or more) companies want to exchange information by sharing a common set of servers.

A security-conscious company separates its departments and allows them to access common servers only

Client VRFs contain server routes; clients can talk to servers. - Client routes need to be exported to the server site.

Server VRFs contain client routes; servers can talk to clients. - Server routes need to be exported to client and server sites.

Client VRFs do not contain routes from other clients; clients cannot communicate. - No routes are exchanged between client sites.

Make sure that there is no client-to-client leakage across server sites.

![](<../.gitbook/assets/Unknown image (1801)>)

![](<../.gitbook/assets/Unknown image (1802)>)

| PE1(config)# ip vrf Cust\_A PE1(config)# rd 1:10 PE1(config)# route-target export 1:10 PE1(config)# route-target import 1:10 PE1(config-vrf)# route-target export 1:102 PE1(config-vrf)# route-target import 1:101 PE1(config-vrf)# exit PE1(config)# ip vrf Cust\_B PE1(config)# rd 1:20 PE1(config)# route-target export 1:20 PE1(config)# route-target import 1:20 PE1(config-vrf)# route-target export 1:102 PE1(config-vrf)# route-target import 1:101 | PE3(config)# ip vrf Server PE3(config-vrf)# rd 1:100 PE3(config-vrf)# route-target both 1:100 PE3(config-vrf)# route-target export 1:101 PE3(config-vrf)# route-target import 1:102 The customer A and customer B will not import routes between each other due to implementing one RT for import and different RT for export If there was one RT for both, the customer A and customer B would import the routes between each other |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

**Managed network VPN**

If the service provider is managing the customer routers, it is convenient to have a central point that has access to all CE routers but does not have access to the other destinations at the customer sites. This requirement is usually implemented by deploying a separate VPN for management purposes. This VPN needs to see all the loopback interfaces of all the CE routers. All CE routers need to see the network management VPN. The design is similar to that of the central services VPN; the only difference is that you mark only loopback addresses to be imported into the network management VPN

The central server NMS needs access to the loopback addresses of all CE routers.

Very similar to central services and simple VPNs:

All the CE routers participate in the central services VPN.

Only the loopback addresses of the CE routers need to be exported into the central services VPN

{% hint style="info" %}
The routing protocol between PE and CE routers must be secured (distribute-lists / prefix-lists). Otherwise, customers can announce prefixes in the management address space and gain two-way access to the NMS.
{% endhint %}

<figure><img src="../.gitbook/assets/Unknown image (1803)" alt=""><figcaption></figcaption></figure>

| PE1 configuration ip vrf Cust\_A route-target import 1:100 export map Managed ! ip vrf Cust\_B route-target import 1:100 export map Managed ! route-map Managed permit 10 match ip address 10 set extcommunity rt 1:101 additive ! access-list 10 permit 10.0.1.49 0.0.0.0 access-list 10 permit 10.0.2.49 0.0.0.0 | PE3 configuration ip vrf Managed route-target both 1:100 route-target import 1:101 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------- |

Another example from SP1 lab

Routes not explicitly matched by the route-map are still exported, but they are exported without any modifications (default behavior).

This is similar to PBR, where unmatched packets fall back to normal routing instead of being dropped

| XR ! vrf A address-family ipv4 unicast import route-target 1:100 1:400 ! export route-policy NMC export route-target 1:100 ! ! ! vrf B address-family ipv4 unicast import route-target 1:200 1:300 1:400 ! export route-policy NMC export route-target 1:200 1:301 ! ! ! extcommunity-set rt NMC 1:401 end-set ! route-policy NMC if destination in (192.168.255.0/24 ge 32) then set extcommunity rt NMC additive endif end-policy ! | XE ip prefix-list MGMT seq 10 permit 192.168.255.0/24 ge 32 ! route-map NMS permit 10 match ip address prefix-list MGMT set extcommunity rt 1:401 additive ! ip vrf A rd 1:100 export map NMS route-target import 1:400 ! ip vrf B rd 1:200 export map NMS route-target import 1:400 ! end | NMS PE configuration ip vrf NMS rd 1:400 route-target export 1:400 route-target import 1:401 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------- |

### Internet access for VPN customers

#### Customer internet connectivity via a shared MPLS backbone

Because Internet access is one of the most popular services that service providers offer their customers, many service providers offer Internet access as well as MPLS VPN service on their shared backbone

Network designers who want to offer Internet access and MPLS VPN services on the same backbone can choose between these two major design models:

Internet access that is implemented through global routing on the PE routers and is not a VPN service, or as Internet access that is implemented as a separate VPN in the ISP network

Implementing Internet access through global routing is identical to building a traditional IP backbone that offers Internet services. Depending on whether the customer sites need full Internet routes, static routes or BGP is used for routing to the Internet.

For full Internet routes, BGP is deployed between the PE routers and the PE Internet gateway to exchange Internet routes, and the global routing table on the PE routers is used to forward the customer traffic toward Internet destinations. The PE routers might or might not have the full Internet routing table. The PEs and the provider Internet gateway are in the same IBGP area.

This setup is easy to implement, but a separation of Internet and VPN connectivity requires either two separate physical links or a single physical link with subinterface encapsulation

**Shared PE and backbone:** Single PE may connect both Internet and VPN customers

Benefit: One backbone, one network and single PE, Easier management, Can offer centralized services

Drawback Security and Performance (memory/CPU)

![](<../.gitbook/assets/Unknown image (1804)>)

**Partially shared:** P routers are shared, but use separate PE for Internet and for VPN

Benefit One backbone, one network, Separate PE, Security, performance, Can offer centralized services

Drawback: Price for additional hardware dedicated PE

![](<../.gitbook/assets/Unknown image (1805)>)

**Full Separation:** No routers shared

Benefit Physical separation, Separate IGP and EGP , Security, performance

Drawback Maintain two separate networks, Price

![](<../.gitbook/assets/Unknown image (1806)>)

#### Internet access through route leaking

In route leaking, a static default route in the VRF points to the global next-hop address of the Internet gateway. This requires global routing to resolve next-hop addresses, and VPN subnets must be advertised in the global routing table. However, this method is not secure and not advisable for production environments

Benefits: No separate connection needed for Internet traffic.

Drawbacks: Insecure, as Internet and corporate traffic are mixed. Difficult to apply security policies on mixed traffic. Limited scalability for full Internet routing. Negates VPN isolation, posing security risks.

Design: PE has Internet routes in global space and VPNs in MPLS , Customer (CE) have Internet + VPN in one routing table and default traffic is routed to Internet GW (0.0.0.0/0)

![](<../.gitbook/assets/Unknown image (1807)>)

#### Internet access through a separate sub-interface

Physical interfaces can be used, one for VPN connectivity and second for internet connectivity, to avoid separate physical links for VPN and Internet traffic, subinterfaces can be used to create two logical links over a single physical link.

This approach separates VPN and Internet traffic by configuring subinterfaces on both the PE and CE routers. Each subinterface is assigned a unique VLAN ID and IP address for traffic classification.

Design:

PE: Uses separate sub-interface to differentiate traffic from the customer to route VPN traffic to a VRF and Internet traffic to the global routing table. Internet is in global space and VPNs in MPLS.

CE: Uses separate sub-interfaces to tag VPN and Internet traffic with VLAN IDs. Internet in VRF-lite (or global) and VPN in global

Internet access for every customer site can be implemented by configuring the Internet VRF at every location. This solution adds complexity for the customer because firewall and NAT support might be needed at every site, unless the service provider offers a central managed firewall service

{% hint style="info" %}
This implementation requires either public addressing per customer, or NAT on each CE/PE. That does not scale.
{% endhint %}

<figure><img src="../.gitbook/assets/Unknown image (1808)" alt=""><figcaption></figcaption></figure>

| PE example configuration ip vrf VPN-A rd 100:1 route-target export 100:1 route-target import 100:1 # VPN Subinterface (VLAN 100 → VRF) interface GigabitEthernet0/1.100 encapsulation dot1Q 100 vrf forwarding VPN-A ip address 192.168.46.4 255.255.255.0 # Internet Subinterface (VLAN 200 → Global Routing Table) interface GigabitEthernet0/1.200 encapsulation dot1Q 200 ip address 62.215.1.1 255.255.255.252 # Advertise VPN routes in BGP router bgp 100 neighbor remote-as 100 address-family ipv4 vrf VPN-A network 10.1.1.0 mask 255.255.255.0 address-family ipv4 network 192.168.1.0 mask 255.255.255.0 neighbor activate neighbor next-hop-self | CE example configuration # VPN Subinterface (VLAN 100 → Corporate VPN) interface GigabitEthernet0/0.100 encapsulation dot1Q 100 ip address 192.168.46.5 255.255.255.0 # Internet Subinterface (VLAN 200 → Internet Access) interface GigabitEthernet0/0.200 encapsulation dot1Q 200 ip address 62.215.1.2 255.255.255.252 # Static Routes ip route 0.0.0.0 0.0.0.0 62.215.1.1 name #Default route for Internet# |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### Internet access as a separate VPN

The MPLS VPN architecture can provision a separate VPN to provide Internet access for VPN customers.

Under this design model, the provider Internet gateways appear as CE routers to the MPLS VPN backbone

A provider Internet gateway is connected as a CE router to the MPLS VPN backbone.

In this design, the Internet VPN should not contain the full set of global Internet routes because that would make the solution completely nonscalable. The provider Internet gateway routers should announce a default route toward the PE routers. To optimize local routing, the local and regional Internet routes could be inserted in the Internet VPN

Involves separate sub-interface – one for VPN connectivity and second for Internet connectivity

Every customer site that needs Internet access is assigned to the same Internet VPN as the Internet gateway - similar to the central services VPN

Internet access provided via route target import + export from/to customer VRF

Each customer must have unique IP subnet within the separate internet VRF

Benefits:

Access-control using route-targets.

Increased security for the provider backbone because Internet hosts can reach only PE routers, not the core P routers.

The VPN customers are connected to the Internet simply through an extra VRF at the PE

Customers are tied to upstream service providers simply by placing the PE-CE link into the VRF that is associated with the upstream service provider. Changing an ISP becomes as easy as reassigning the interface into a different VRF and attending to address allocation issues. For customers who are using access methods that support dynamic address allocation (for example, dialup or cable), the new customer IP address is assigned automatically from the address space of the new ISP.

Limitations:

The model requires two dedicated physical links between the PE and CE routers or specific WAN or LAN encapsulations that might not be suitable for all customers.

The PE routers must be able to perform hop-by-hop Internet routing and either use the default route to reach the Internet or carry the full Internet routing table.

Advanced Internet access services (centrally managed firewall service or wholesale Internet access service) cannot be realized with this model.

Scalability restriction – only default route and regional routes can be provided, full BGP table isn't feasible

![](<../.gitbook/assets/Unknown image (1809)>)

One caveat to be mentioned regarding this design option (as mentioned in the CCDE study guide) as we are importing routes (not default route in this example), what will happen is that a label is assigned per each prefix (currently we are having one prefix)

In order to clearly demonstrate this, we will configure a new route on the INET and advertise it to be reachable via the customers:

R2-ASBR#show ip bgp vpnv4 vrf INET labels

Network Next Hop In label/Out label

Route Distinguisher: 10:10 (INET)

3.3.3.3/32 212.118.23.3 22/nolabel

10.10.6.0/24 4.4.4.4 nolabel/19

10.10.8.0/24 4.4.4.4 nolabel/25

13.13.13.13/32 212.118.23.3 23/nolabel

192.168.46.0 4.4.4.4 nolabel/21

192.168.48.0 4.4.4.4 nolabel/18

Now assuming a customer needs full routing table to be available, what will happen per the current label allocation mode (which is per prefix), is that every prefix will be assigned a label and from a scalability perspective this could overwhelm the table

R4-PE#show ip bgp vpnv4 vrf MSSK labels

Network Next Hop In label/Out label

Route Distinguisher: 1:1 (MSSK)

3.3.3.3/32 2.2.2.2 nolabel/22

10.10.6.0/24 192.168.46.6 19/nolabel

10.10.7.0/24 5.5.5.5 nolabel/20

13.13.13.13/32 2.2.2.2 nolabel/23

192.168.46.0 0.0.0.0 21/nolabel(MSSK)

192.168.57.0 5.5.5.5 nolabel/16

R4-PE#show ip bgp vpnv4 vrf ABC labels

Network Next Hop In label/Out label

Route Distinguisher: 2:2 (ABC)

3.3.3.3/32 2.2.2.2 nolabel/22

10.10.8.0/24 192.168.48.8 25/nolabel

13.13.13.13/32 2.2.2.2 nolabel/23

192.168.48.0 0.0.0.0 18/nolabel(ABC)

What can be done to properly utilize our available resources is to change the label allocation mode to per VRF instead of per prefix:

ASBR:

| mpls label mode vrf INET protocol bgp-vpnv4 per-vrf |   |
| --------------------------------------------------- | - |

R2-ASBR#show ip bgp vpnv4 vrf INET labels

Network Next Hop In label/Out label

Route Distinguisher: 10:10 (INET)

3.3.3.3/32 212.118.23.3 IPv4 VRF Aggr:24/nolabel

10.10.6.0/24 4.4.4.4 nolabel/19

10.10.8.0/24 4.4.4.4 nolabel/25

13.13.13.13/32 212.118.23.3 IPv4 VRF Aggr:24/nolabel

192.168.46.0 4.4.4.4 nolabel/21

192.168.48.0 4.4.4.4 nolabel/18

R4-PE#show ip bgp vpnv4 vrf MSSK labels

Network Next Hop In label/Out label

Route Distinguisher: 1:1 (MSSK)

3.3.3.3/32 2.2.2.2 nolabel/24

10.10.6.0/24 192.168.46.6 19/nolabel

10.10.7.0/24 5.5.5.5 nolabel/20

13.13.13.13/32 2.2.2.2 nolabel/24

192.168.46.0 0.0.0.0 21/nolabel(MSSK)

192.168.57.0 5.5.5.5 nolabel/16

R4-PE#show ip bgp vpnv4 vrf ABC labels

Network Next Hop In label/Out label

Route Distinguisher: 2:2 (ABC)

3.3.3.3/32 2.2.2.2 nolabel/24

10.10.8.0/24 192.168.48.8 25/nolabel

13.13.13.13/32 2.2.2.2 nolabel/24

192.168.48.0 0.0.0.0 18/nolabel(ABC)

#### Wholesale internet access

offers connectivity to multiple ISPs where a separate VPN is created for each upstream ISP.

Each ISP gateway announces the default route to the VPN.

Internet access from every customer site can be implemented by configuring the Internet VRF on a second interface at every location and importing and exporting dedicated route-target

Customers are assigned into the VRF that corresponds to the VPN of the desired upstream ISP.

The upstream ISP allocates a portion of its address space to the end users that are connected to the Internet access backbone

![](<../.gitbook/assets/Unknown image (1810)>)

#### Redundant internet access

Multiple Internet gateways (acting as CE routers) need to be connected to the MPLS VPN backbone to ensure router and link redundancy.

All Internet gateways advertise the default route to the PE routers, resulting in routing redundancy.

Internet gateways could also announce local Internet routes.

Default routes can be assigned different BGP attributes - it is recommended to use MED, to establish primary and backup internet gateway connectivity

The redundancy that has been established so far covers the path between customer sites and the Internet-Gateway routers. A failure in the Internet backbone might break the Internet connectivity for the customers if the Internet-Gateway routers announce the default route unconditionally. Conditional advertisement of the default route is therefore configured on the Internet-Gateway routers, which announce the default route to the PE routers only if the Internet-Gateway routers can reach an upstream destination

![](<../.gitbook/assets/Unknown image (1811)>)

**Internet breakout from PE (lab example)**

Implemeting Redundant Internet access with VRF-Aware NAT on edge PE routers - in the form of default route advertisement into the customer VRF

In environments where there are multiple L3VPN customers that need the internet (so-called Internet breakout from PE), Egress PE NAT is often used.

Since customers may have the same IP addresses (e.g. 10.0.0.0/8 private IP), the router must be able to distinguish which VRF the translation comes from.

The NAT table on the PE router therefore contains the VRF identification as part of the translation - i.e. the mapping is VRF-specific.

As the NAT is done on the ASBR, overlapping IP address space can be served and this is one of the advantages of this option - Users from different VRFs can share the same public IP address because the translation is also distinguished by the VRF context.

{% hint style="info" %}
In real deployments, the PE/Internet gateway typically forwards traffic to an edge firewall (or is the firewall). This gives you better control and observability for Internet NAT.
{% endhint %}

| ISP 1 config interface Loopback0 ip address 8.8.8.8 255.255.255.255 ! interface GigabitEthernet0/0 ip address 100.100.100.1 255.255.255.252 ISP 2 config interface Loopback0 ip address 8.8.8.8 255.255.255.255 ! interface GigabitEthernet0/0 ip address 200.200.200.1 255.255.255.252 PE-INTERNET-1 config vrf definition A rd 1:100 ! address-family ipv4 route-target export 1:100 route-target import 1:100 route-target import 1:400 exit-address-family ! address-family ipv6 route-target export 1:100 route-target import 1:100 exit-address-family ! vrf definition B rd 1:200 ! address-family ipv4 route-target export 1:200 route-target export 1:301 route-target import 1:200 route-target import 1:400 route-target import 1:300 exit-address-family ! vrf definition ISP1 rd 1:1 route-target export 1:1 route-target import 1:1 ! address-family ipv4 exit-address-family ! address-family ipv6 exit-address-family ! track 1 ip sla 1 reachability ! interface Loopback0 ip address 10.255.255.1 255.255.255.255 ! interface GigabitEthernet0/0 vrf forwarding ISP1 ip address 100.100.100.2 255.255.255.252 ip nat outside ip virtual-reassembly in duplex auto speed auto media-type rj45 ! interface GigabitEthernet0/3 ip address 10.1.1.1 255.255.255.252 ip nat inside ip virtual-reassembly in duplex auto speed auto media-type rj45 mpls ip ! router ospf 1 router-id 10.255.255.1 network 10.2.2.0 0.0.0.3 area 0 network 10.255.255.2 0.0.0.0 area 0 network 0.0.0.0 255.255.255.255 area 0 ! router bgp 1 bgp router-id 1.1.1.1 bgp log-neighbor-changes neighbor 172.16.100.3 remote-as 1 neighbor 172.16.100.3 update-source Loopback0 ! address-family ipv4 no neighbor 172.16.100.3 activate exit-address-family ! address-family vpnv4 neighbor 172.16.100.3 activate neighbor 172.16.100.3 send-community both neighbor 172.16.100.3 next-hop-self exit-address-family ! address-family ipv4 vrf A network 0.0.0.0 route-map MAP exit-address-family ! address-family ipv4 vrf B network 0.0.0.0 route-map MAP exit-address-family ! ip forward-protocol nd ! ! no ip http server no ip http secure-server ip nat inside source list VPN interface GigabitEthernet0/0 vrf A overload ip nat inside source list VPN interface GigabitEthernet0/0 vrf B overload ip route vrf A 0.0.0.0 0.0.0.0 GigabitEthernet0/0 100.100.100.1 track 1 ip route vrf B 0.0.0.0 0.0.0.0 GigabitEthernet0/0 100.100.100.1 track 1 ip route vrf ISP1 0.0.0.0 0.0.0.0 GigabitEthernet0/0 100.100.100.1 ! ip access-list standard VPN permit 10.0.0.0 0.255.255.255 ! ip sla 1 icmp-echo 8.8.8.8 source-interface GigabitEthernet0/0 vrf ISP1 frequency 5 ip sla schedule 1 life forever start-time now ipv6 ioam timestamp ! route-map MAP permit 10 set metric 100 ! route-map MAP permit 20 ! ! mpls ldp router-id Loopback0 ! PE-INTERNET-2 config route-map MAP permit 10 set metric 1000 ! route-map MAP permit 20 | <p>To implement redundant internet access attach a route map to the network statement to set the Multi-Exit Discriminator (MED) value, which will influence which PE internet gateway router acts as the primary. This approach allows one PE to serve as the primary for Customer A and secondary for Customer B, while the other PE serves as the reverse (secondary for A, primary for B) Important: Be sure to include a default permit statement at the end of the route map to avoid inadvertently dropping routes. Additionally, IP SLA should be implemented to ensure failover between PE-INTERNET-1 and PE-INTERNET-2. Apply the IP SLA to the static route. A corresponding static route must also be added in ISP2 to ensure the IP SLA configuration works as expected Ping from CE-1A,CE-1B</p><div data-gb-custom-block data-tag="file" data-src="../.gitbook/assets/MPLS_L3VPN_INET_ACCESS.yaml"></div> |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

{% hint style="info" %}
PE-INTERNET routers also run `ospf 1` toward the MPLS core.
{% endhint %}

<figure><img src="../.gitbook/assets/Unknown image (1813)" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Unknown image (1812)" alt=""><figcaption></figcaption></figure>

**Configuration example: internet access as a separate VPN**

Add the following config to the routers, you can remove the NAT and other unnecessary config from the previous example

| PE-001-IOS ! vrf definition ISP1 rd 1:1 ! address-family ipv4 route-target export 1:1 route-target import 1:1 exit-address-family ! address-family ipv6 route-target export 1:1 route-target import 1:1 exit-address-family ! interface GigabitEthernet0/0 description >> PE-CE1A Interconnect << no ip address duplex auto speed auto ! interface GigabitEthernet0/0.10 description >> PE-CE1A Interconnect << encapsulation dot1Q 10 vrf forwarding A ip address 10.10.1.1 255.255.255.0 ipv6 address 6:1/64 ! interface GigabitEthernet0/0.100 encapsulation dot1Q 100 vrf forwarding ISP1 ip address 5.5.5.5 255.255.255.0 ! router bgp 1 address-family ipv4 vrf ISP1 redistribute connected | CE-1-A interface GigabitEthernet0/0 description >> PE-CE Interconnect << no ip address duplex auto speed auto ! interface GigabitEthernet0/0.10 description >> PE-CE Interconnect << encapsulation dot1Q 10 ip address 10.10.1.2 255.255.255.0 ipv6 address 6:2/64 ! interface GigabitEthernet0/0.100 encapsulation dot1Q 100 ip address 5.5.5.10 255.255.255.0 ! ip route 0.0.0.0 0.0.0.0 GigabitEthernet0/0.100 5.5.5.5 | ISP1 router bgp 101 bgp router-id interface Loopback0 bgp log-neighbor-changes neighbor 100.100.100.2 remote-as 1 ! address-family ipv4 network 0.0.0.0 neighbor 100.100.100.2 activate neighbor 100.100.100.2 default-originate exit-address-family |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Repeat the same for the ISP2 to ensure redundant internet access (static route on the CE to the secondary ISP would have higher AD) or instead the default can be originated by the PE and from there with BGP metrics we can influence the secondary and backup ISP

## IPv6 over MPLS (6PE / 6VPE)

Many service providers have already deployed MPLS in their IPv4 backbone for various reasons

MPLS can be used to facilitate IPv6 integration

IPv6 connectivity can be achieved over MPLS with:

**Layer 2 MPLS VPN**

Uses pseudowires (e.g., EoMPLS, VPLS, EVPN) to connect sites at Layer 2.

IPv6 traffic is transparent to the MPLS core—CEs use IPv6, but the core just sees it as payload.

**IPv6 CE-to-CE IPv6 over IPv4 tunnels**

Used as a transition mechanism.

**Native IPv6 MPLS**

Requires IPv6-enabled IGP (like OSPFv3 or IS-IS for IPv6) or SRv6 on all P and PE routers

#### IPv6 provider edge router (6PE) over MPLS

#### IPv6 VPN provider edge (6VPE) over MPLS

{% hint style="info" %}
Although VPNv6 implies IPv6 neighbor addresses, VPNv6 still relies on an IPv4 MPLS core. An IPv6-only MPLS core is not implemented in Cisco IOS.
{% endhint %}

### IPv6 provider edge router (6PE) over MPLS and 6VPE

are transition techniques, allowing to connect IPv6 networks over an IPv4-only MPLS backbone

They are based on dual-stack PE routers that exchange IPv6 routing information through MP-BGP, while following the same procedure as for IPv4 MPLS - topmost label is the LDP label to label switch the packet to the egress PE and the second label is label assigned to the IPv6 prefix

The next-hop is dynamically set to the special IPv4 mapped IPv6 address ::FFFF:

PEs and Ps use the IPv4 routing protocol to exchange routing information, and PEs use MPLS to establish LSPs with each other

This allows service providers to offer IPv6 to their customers without making major changes to the core routers in their MPLS network.

CE routes are exchanged in the global routing table between 6PE routers, whereas 6VPE is the equivivalent to IPv4 MPLS L3 VPNS, maintaining multiple VPNs in respective VRFs

IPv6 provider edge router (6PE) over MPLS

![](<../.gitbook/assets/Unknown image (1814)>)

Life of a Packet (Control Plane) - 6PE

![](<../.gitbook/assets/Unknown image (1815)>)

| 6PE-1 XE Configuration ipv6 cef ! mpls label protocol ldp ! router bgp 100 no synchronization no bgp default ipv4 unicast neighbor 2001:DB8:1::1 remote-as 65014 neighbor 200.10.10.1 remote-as 100 neighbor 200.10.10.1 update-source Loopback0 ! address-family ipv6 neighbor 200.10.10.1 activate neighbor 200.10.10.1 send-label neighbor 2001:DB8:1::1 activate redistribute connected no synchronization exit-address-family                          | IN IOS-XE: send-label must be explicitly configured to enable labeled unicast AFI to exchange labels with the neighbor                                                                                                                                                                                                                                                                                                                                         |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 6PE-1 XR Configuration router bgp 1 address-family ipv6 unicast redistribute connected redistribute static allocate-label all ! address-family ipv6 labeled-unicast next-hop-self ! iBGP (to BGP RR, remote PE..) neighbor 172.16.100.3 remote-as 1 update-source Loopback0 address-family ipv6 labeled-unicast next-hop-self ! eBGP (PE – CE) neighbor 2001:300::2 remote-as 5000 address-family ipv6 unicast route-policy PASS in route-policy PASS out ! | In IOS-XR allocate-label all command must be configured under the IPv6 address family in BGP to ensure that labels are allocated for all IPv6 prefixes, otherwise, the routes wouldn't have MPLS labels assigned to them and thus couldn't be transported across the MPLS core address-family ipv6 labeled-unicast must be enabled for each neighbor to exchange labels Do not configure ipv6 unicast afi for the neighbor! just configure the labeled-unicast |

The CE router, CE1, is configured to forward its IPv6 traffic to the 6PE1 router. The P1 router in the core of the network is assumed to be running MPLS, a label distribution protocol, an IPv4 IGP, and Cisco Express Forwarding or distributed Cisco Express Forwarding. P1 does not require any new configuration to enable the Cisco 6PE feature. New configuration tasks are also not required for the CE1 router.

The Cisco 6PE routers, 6PE1 and 6PE2, must be members of the core IPv4 network. The Cisco 6PE router interfaces that are attached to the core network must run MPLS, the same LDP, and the same IPv4 IGP as in the core network.

The 6PE routers must also be configured to be dual stack to run both IPv4 and IPv6.

Restrictions

These restrictions apply when implementing the Cisco 6PE feature:

Core MPLS routers support MPLS and IPv4 only, so they cannot forward or create any IPv6 Internet Control Message Protocol (ICMP) messages.

Cisco 6PE does not provide load balancing between an MPLS path and an IPv6 path. If both are available, the MPLS path is always preferred. Load balancing between two MPLS paths is possible.

When MP-IBGP multipath is enabled on the Cisco 6PE router, all labeled paths are installed in the forwarding table with MPLS information (label stack), when MPLS available. This functionality enables Cisco 6PE to perform load balancing.

### IPv6 VPN provider edge (6VPE) over MPLS

Enable IPv6 routing and Cisco Express Forwarding (CEF) for IPv6. Configure MP-BGP for VPN route exchange. Configure the customer IPv6 VRF tables. Configure the customer VRF interfaces. Configure the customer VRF (PE-CE) IPv6 routing protocols or static IPv6 routes. Redistribute the CE-PE routing protocol VRF routes into MP-BGP.

Assumptions (already completed)

* Loopback interface is configured.
* LDP is configured.
* MPLS is enabled on interfaces.
* Backbone IGP (OSPF or IS-IS) is configured.

{% stepper %}
{% step %}
Enable IPv6 routing and IPv6 Cisco Express Forwarding (CEF).

* Ensure IPv6 is globally enabled on the router.
* Enable CEF for IPv6 to optimize forwarding.
{% endstep %}

{% step %}
Configure MP-BGP for VPN route exchange.

* Configure MP-BGP sessions between PE routers.
* Enable address-family ipv6 vpn for VRF route exchange.
{% endstep %}

{% step %}
Configure the customer IPv6 VRF tables.

* Create VRFs for each customer.
* Assign RD/RT as required for the VPN.
{% endstep %}

{% step %}
Configure the customer VRF interfaces.

* Create and assign IPv6 addresses to the VRF interfaces.
* Bind interfaces to the corresponding VRF.
{% endstep %}

{% step %}
Configure the customer VRF (PE-CE) IPv6 routing protocols or static IPv6 routes.

* Run eBGP/OSPF/OSPFv3/IS-IS in the VRF or configure static IPv6 routes on the PE and CE as needed.
{% endstep %}

{% step %}
Redistribute the CE-PE routing protocol VRF routes into MP-BGP.

* Import VRF IPv6 routes into the MP-BGP address-family ipv6 vpn so they are distributed to other PEs.
* Verify route-target import/export settings for correct route distribution.
{% endstep %}
{% endstepper %}
