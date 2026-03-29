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

# MPLS TE

### Overview

WAN connections are an expensive item in the service provider budget. The cost saving that results from a more efficient use of resources, will help to reduce the overall cost of operations. Also, more efficient use of bandwidth resources means that a service provider can avoid a situation where some parts of a network are congested, while other parts are underutilized.

In a Layer 3 routing network, packets are forwarded hop-by-hop. In each hop, the destination address of the packet is used to make a routing table lookup. The routing tables are created by an interior gateway protocol (IGP), which finds the least-cost route, according to its metric, to each destination in the network.

In many networks, this method works well. But in some networks, the destination-based forwarding results in the overutilization of some links, while others are underutilized. This imbalance will happen when there are several possible routes to reach a certain destination. The IGP selects one of them as the best, and uses only that route. In the extreme case, the best path may have to carry so large a volume of traffic that packets are dropped, while the next-best path is almost idle.

Better network load distribution could be achieved by fine-tuning the metrics alone, but this approach of changing the link metrics may prove too difficult and impractical on a large network, due to the large number of interfaces, with difficult to predict net effects on the network as a whole.

Policy-based routing (PBR) is able to discriminate among packet flows, based on the source, but it suffers from low scalability and the same static routing restrictions on using redundancy.

![](<../.gitbook/assets/Unknown image (1709)>)

### Cisco MPLS RSVP Traffic Engineering

Cisco MPLS TE supports Constraint-Based Routing (CBR), in which the path for a traffic flow is the shortest path that meets the resource requirements (constraints) of the traffic flow.

It diverts the traffic from destination-based forwarding, allowing to dynamically distribute traffic load by establishing LSPs based on specific traffic engineering constraints and policies defined

It augments the use of link cost by also considering other factors, such as bandwidth availability or link attributes, when choosing the path to a destination.

Each Cisco MPLS TE tunnel crosses a number of routers which are called headend, tail end, and midpoints, depending on the relative location. The headend router is the router that initiates the tunnel towards the tail end. Cisco MPLS TE tunnels are always unidirectional, so the traffic that returns from tailend to headend will follow the usual MPLS path, which is actually the least costly IGP path.

Because TE can be used to control traffic flows, it can also be used to provide protection against link or node failures by providing backup tunnels.

Finally, when combined with MPLS COS - quality of service (QoS) functionality, TE can provide enhanced service level agreements (SLAs).

One of the problems that TE solves is persistent network congestion over a long period.

TE does not solve temporary network congestion that is caused by traffic bursts. This type of problem is better managed by an expansion of capacity or by classic techniques such as various queuing algorithms, rate limiting, and intelligent packet dropping. TE does not solve problems when the network resources themselves are insufficient to accommodate the required load.

![](<../.gitbook/assets/Unknown image (1710)>)

For Cisco MPLS TE, manual assignment and configuration of the labels can be used to create label-switched paths (LSPs) to tunnel the packets across the network on the desired path. However, to increase scalability, Resource Reservation Protocol is used to automate the procedure.

For circuit-style forwarding (e.g. circuit switched type of forwarding), Cisco MPLS TE uses TE tunnels. For signaling, RSVP is used with various extensions to set up the Cisco MPLS TE tunnels.

IS-IS or Open Shortest Path First (OSPF) with extensions (TLV's) is used to carry resource information, such as the available bandwidth on the link. Both link-state protocols, OSPF and IS-IS, use new attributes to describe the nature of each link with respect to the constraints. A link that does not have the required resource will not be included, or it will not be a part of the path of the Cisco MPLS TE tunnel.

In OSPF this information is carried in OSPF Type 10 Opaque LSAs, which allow routers to flood TE attributes through the OSPF domain. The headend router uses this information to calculate a constrained shortest path (CSPF) that satisfies given traffic engineering policies, and then establishes a unidirectional RSVP-TE LSP (Label Switched Path) to the destination.

ISIS TLVs for traffic engineering:

TLV Type 2: includes a 8-bit default metric

TLV Type 135: support a 32-bit metric and an up/down bit

TLV Type 134: carries a 32-bit router ID for traffic engineering

Type-10 Opaque :advertisement are flooded thoughout the domain

Type 22: Contains information about the link and include other sub-TLVS

TLV 22 can include several sub-TLVs that provide granular details about the network links:​

Administrative Group (Color): Indicates link attributes for policy-based routing.

Maximum Link Bandwidth: Specifies the maximum bandwidth of the link.

Maximum Reservable Bandwidth: Indicates the maximum bandwidth that can be reserved.

Unreserved Bandwidth: Provides available bandwidth at various priority levels.

TE Default Metric: Specifies a metric for traffic engineering purposes, separate from the standard IS-IS metric.

Link Protection Type: Indicates the protection capabilities of the link (e.g., extra traffic, unprotected).

The label-switching router (LSR) uses a special version of Shortest Path First algorithm (SPF), called Constrained Shortest Path First (CSPF) to calculate best paths based on the inputs provided by the operator. CSPF takes into account both the information in the traffic engineering database (TED) and the constraints that you impose on the tunnel.

In other words, the path that is computed with CSPF is the shortest path fulfilling a set of constraints. Once the path is computed, TE is responsible for establishing and maintaining a forwarding state along such a path.

Traffic tunnel is simply a collection of data flows that share some common attribute:

Most simply, this attribute might be the sharing of the same entry point to the network and the same exit point.

This attribute could be augmented by defining separate tunnels for different classes of service. For example, in an ISP model, corporate customers could be given a preferential throughput over home users. This preferential treatment might be greater guaranteed bandwidth, or lower latency and higher precedence. Even though the traffic enters and leaves the ISP network at the same points, different characteristics could be assigned to these types of users by defining separate traffic tunnels for their data

Configuring the traffic tunnels includes defining the characteristics and attributes that it requires

The routers that are identified as the tunnel headends are usually on the edge of the network. The traffic tunnels link these routers across the core of the network.

The attribute could be that all traffic is sharing the same entry point to the network and the same exit point.

A tunnel is created by the administrator, specific (or all) traffic can be steered into it

A tunnel is assigned labels that represent the path (LSP) through the system.

Forwarding within the MPLS network is based on the labels (no Layer 3 lookup)

MPLS TE have a stack of two labels that are imposed by the ingress router. The topmost label identifies a specific LSP or TE tunnel to use to reach another router at the other end of the tunnel. The second label indicates what the router at the far end of the tunnel should do with the packet.

Every LSR must see the entire topology of the network (only OSPF and IS-IS hold the entire topology).

Every LSR needs additional information about links in the network. This information includes available resources and constraints. OSPF and IS-IS have extensions to propagate this additional information.

The result of the constraint-based calculation is a list of routers that form the path to the destination. The path is a list of IP addresses that identify each next hop along the path.

However, this list of routers is known only to the router at the headend of the tunnel, the one that is attempting to build the tunnel. Somehow, this now-explicit path must be communicated to the intermediate routers. The intermediate routers do not make their own CSPF calculations; they merely abide by the path that is provided to them by the headend router.

Therefore, some signaling protocol is required to confirm the path, to check and apply the bandwidth reservations, and finally to apply the MPLS labels to form the MPLS LSP through the routers. The MPLS working group of the IETF has adopted RSVP to confirm and reserve the path and apply the labels that identify the tunnel. LDP is used to distribute the labels for the underlying MPLS network.

Two types of tunnels can be established across the links with matching attributes:

Dynamic—using the least-cost path computed by OSPF or IS-IS

| tunnel mpls traffic-eng bandwidth 100 tunnel mpls traffic-eng priority 1 1 tunnel mpls traffic-eng path-option 1 dynamic |   |
| ------------------------------------------------------------------------------------------------------------------------ | - |

Explicit—using a path that is defined with Cisco IOS configuration commands

| ip explicit-path identifier 1 next-address 209.165.200.1 next-address 172.16.0.1 | Router(config-path)# next-address \[ loose \| strict ] ip-address loose – (Optional) Specifies that the previous address (if any) in the explicit path do not need to be directly connected to the next IP address, and that the router is free to determine the path from the previous address (if any) to the next IP address strict - (Optional) Specifies that the previous address (if any) in the explicit path must be directly connected to the next IP address. |
| -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

Constraint-based path computation selects the path that the traffic tunnel will take, based on the administrative weight (TE cost) of each individual link. This administrative weight is, by default, equal to the IGP link metric.

If there are more candidates for the LSP (several paths with the same metric), then the selection criteria is as follows (in sequential order):

The highest minimum bandwidth on the path takes precedence.

The smallest hop count takes precedence.

If more than one path still exists after applying both of these criteria, a path is randomly chosen.

The result of a constraint-based path computation is a unidirectional Cisco MPLS TE tunnel (traffic tunnel) that is seen only at the tunnel endpoints (headend and tail end)

From the perspective of IGP routing, the traffic tunnel is not seen as an interface at all and is not included in any IGP route calculations (except for other IP tunnels such as GRE tunnels). The traffic-engineered tunnel, when established, does not trigger any link-state update or any SPF calculation.

Cisco IOS Software and Cisco IOS XR Software use the tunnel mainly for visualization. The rest of the actions that are associated with the tunnel are done by MPLS forwarding and other Cisco MPLS TE-related mechanisms.

The IP traffic that will actually use the traffic-engineered tunnel is forwarded to the tunnel only by the headend of the tunnel. In the rest of the network, the tunnel is not seen at all (no link-state flooding).

With the autoroute feature, the traffic tunnel has the following characteristics:

Appears in the routing table

Has an associated IP metric (cost equal to the best IGP metric to the tunnel endpoint)

Even with the autoroute feature, the tunnel itself is not used in link-state updates and other networks do not have any knowledge of it.

### TE information distribution (IGP extensions → TED)

Because the resource attributes are configured locally for each link, they must be distributed to the headend routers of traffic tunnels. These resource attributes are flooded throughout the network using extensions to link-state intradomain routing protocols, either IS-IS or OSPF to build a Traffic Engineering Database (TED). These attributes help routers compute constraint-based paths (CSPF) and establish TE tunnels dynamically. Also interface and neighbor address is included in those advertisements

Note: The CSFP computation and TED maintenance can be offloaded to centralized controller

The flooding takes place under these conditions:

Link-state changes occur

Network administrator reconfigures the resource class of a link

The amount of available bandwidth crosses one of the preconfigured thresholds.

The frequency of flooding is bounded by the OSPF and IS-IS timers.

### Path selection (CSPF)

Path selection for a traffic tunnel takes place at the headend routers of the traffic tunnels. Using extended IS-IS or OSPF, the edge routers have knowledge of both network topology and link resources. For each traffic tunnel, the tail-end router starts from the destination of the traffic tunnel and attempts to find the shortest path toward the headend router (using the CSPF algorithm). The CSPF calculation does not consider the links that are explicitly excluded by the resource class affinities of the traffic tunnel or the links that have insufficient bandwidth. The output of the path selection process is an explicit route consisting of a sequence of label switching routers. This path is used as the input to the path setup procedure.

Constraint-based path computation, which takes place at the headend of the traffic-engineered tunnel, must be provided with link-resource attributes. The link-resource attributes provide information on the resources of each link.

### Link-resource attributes

provide information on the resources of each link in the topology.

For the tunnel to dynamically discover its path through the network, the headend router must be provided with information on which to base this calculation.

There are four Cisco MPLS TE link-resource attributes. Constraint-based path computation, which takes place at the headend of the traffic-engineered tunnel, must be provided with link-resource attributes. The link-resource attributes provide information on the resources of each link.

The following link resource attributes (link availability) can be configured on the router interfaces:

Maximum bandwidth

This component provides information on the maximum bandwidth that can be used on the link, per direction, given that the traffic tunnels are unidirectional. This parameter is usually set to the configured bandwidth of the link.

Maximum reservable bandwidth

This component provides information on the maximum bandwidth that can be reserved on the link per direction. By default, it is set to 75 percent of the maximum bandwidth.

Unreserved bandwidth provides information on the remaining bandwidth that has not yet been reserved.

Constraint-based specific metric - TE Metric

The TE metric is not related to the IGP metric, and even though the default values of those metrics might be the same, the TE metric can be set to any value on any link, independently of the IGP metric. The TE metric is also known as the administrative weight on Cisco routers.

| Router(config-if)# mpls traffic-eng administrative-weight weight | Specifies the traffic engineering metric for the link - On the physical interface |
| ---------------------------------------------------------------- | --------------------------------------------------------------------------------- |

The TE metrics could reflect the link delay or geographical closeness, but in any event, when nondefault values are required, and this is normally the case, the TE metrics need to be specified by the administrator.

Link Resource Class Affinity (Affinity bits) also called Attibute Flags

Allows the operator to administratively include or exclude links in path calculations. The link is characterized by a 32-bit resource class attribute.

The definition of the tunnel may include a reference to particular "affinity bits." The tunnel affinity bits are matched against the link resource class to determine whether a link may be used as part of the LSP. For simplicity only 3 bits are depicted in below example

It goes hand in hand with the tunnel attribute Tunnel Resource Class Affinity, where the tunnel's configured affinity policy (e.g., include-any, include-all, exclude-any) is logically matched against the affinity bits of the link's resource class to determine path eligibility during constraint-based routing

![](<../.gitbook/assets/Unknown image (1711)>)

#### Flexible name-based tunnel constraints

MPLS-TE Flexible Name-based Tunnel Constraints provides a simplified and more flexible means of configuring link attributes and path affinities to compute paths for the MPLS-TE tunnels.

In traditional TE, links are configured with attribute-flags that are flooded with TE link-state parameters using Interior Gateway Protocols (IGPs), such as Open Shortest Path First (OSPF).

MPLS-TE Flexible Name-based Tunnel Constraints lets you assign, or map, up to 32 color names for affinity and attribute-flag attributes instead of 32-bit hexadecimal numbers. After mappings are defined, the attributes can be referred to by the corresponding color name.

Example:

Red links: Low-latency paths for voice traffic.

Blue links: High-bandwidth paths for video streaming.

Green links: Backup links.

TE tunnels can be configured to use only links with specific colors.

[https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/mpls/24xx/configuration/guide/b-mpls-cg-cisco8000-24xx/implementing-mpls-traffic-engineering.html#concept\_ecm\_wbh\_skb:\~:text=tt1001%3A36%20%20%20%20%20%20%20%20Ready-,Configuring%20Flexible%20Name%2DBased%20Tunnel%20Constraints,-MPLS%2DTE%20Flexible](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/mpls/24xx/configuration/guide/b-mpls-cg-cisco8000-24xx/implementing-mpls-traffic-engineering.html#concept_ecm_wbh_skb)

### Traffic tunnel attributes

characterize the requirements for the tunnel itself

Two of the Cisco MPLS TE tunnel attributes affect the path setup and maintenance of the traffic tunnel:

Traffic Parameters: are the resources that are required by the tunnel, such as minimum required bandwidth (capacity of TE tunnel is in kbps).

The traffic characteristics may include peak rates, average rates, permissible burst size, and so on.

Path selection and management specifies the way in which the headend routers should select explicit paths for traffic tunnels. The path can be configured manually or computed dynamically by using the constraint-based path computation

Tunnel Resource Class Affinity allows the network administrator to apply path-selection policies by administratively including or excluding network links

This restriction can also be accomplished by using the IP address exclusion feature.

Each link may be assigned a resource class attribute. Resource class affinity specifies whether to explicitly include or exclude links with resource classes in the path selection process. The resource class affinity is a 32-bit string that is accompanied by a 32-bit resource class mask. The mask indicates which bits in the resource class need to be inspected. The link is included in the constraint-based LSP when the resource class affinity string or mask matches the link resource class attribute.

These two items determine whether we include or exclude specific links for our tunnel interface.

With affinity, we set the attribute flag value we want to check, and with the mask, we tell the router what bits we care about or don’t care about. This is similar to how a subnet mask works:

The link is characterized by the link resource class.

Default value of bits is 0.

Tunnel is characterized by:

Tunnel resource class affinity

Default value of bits is 0.

Tunnel resource class affinity mask

(0 = do not care, 1 = care)

Default value of the tunnel mask is 0x0000FFFF

Tunnels use the following default affinity and mask:

PE1#show mpls traffic-eng tunnels Tunnel 1 | include Affinity

Bandwidth: 750 kbps (Global) Priority: 7 7 Affinity: 0x0/0xFFFF

The default affinity is 0x0 with a mask of 0xFFFF.

Let me show all 32 bits:

Affinity 00000000 00000000 00000000 00000000

Mask 00000000 00000000 11111111 11111111

For simplicity, only the four affinity and resource bits (of the 32-bit string) are shown

![](<../.gitbook/assets/Unknown image (1712)>)

In the final configuration of the attribute flags/affinity it looks like this:

Create affinity or affinity map associating bits with colors/names

| R1(config)# affinity-map red 0 R1(config)# affinity-map green 1 | Old hex configuration equvivalent is P1(config-if)#mpls traffic-eng attribute-flags 0x1 |
| --------------------------------------------------------------- | --------------------------------------------------------------------------------------- |

Assign it to the desired link

| interface GigabitEthernet0/0 ip rsvp bandwidth mpls traffic-eng tunnels affinity red |   |
| ------------------------------------------------------------------------------------ | - |

Exclude or include this affinity under the TE tunnel configuration

| interface Tunnel0 ip unnumbered Loopback0 tunnel mode mpls traffic-eng tunnel destination 10.1.1.1 tunnel mpls traffic-eng path-option 1 dynamic tunnel mpls traffic-eng affinity exclude-any red | Old hex method would be: PE1(config-if)#tunnel mpls traffic-eng affinity 0x4 mask 0xFFFFFFFF exclude-group in IOS-XR |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |

The affinity use case in real world might look like this:

| **Link type**           | **Binary** | **Hexadecimal** |
| ----------------------- | ---------- | --------------- |
| Fiber                   | 0000 0001  | 0x1             |
| Satellite               | 0000 0010  | 0x2             |
| DSL                     | 0000 0100  | 0x4             |
| Link to another country | 0000 1000  | 0x8             |
| Encrypted link          | 0001 0000  | 0x10            |

Note I’m only using five attributes in this example, so I’m only showing the eight least significant bits. The attribute flag is 32 bits, so the first 24 bits are all zeroes.

A link could have multiple attributes. For example, let’s say a link has these three attributes:

Fiber (0000 0001)

Encrypted (0001 0000)

Connects to another country (0000 1000)

We set the corresponding bits to 1:

| **Link type**                                    | **Binary** | **Hexadecimal** |
| ------------------------------------------------ | ---------- | --------------- |
| Fiber + encrypted link + link to another country | 0001 1001  | 0x19            |

Setup Priority and Holding priority (pre-emption): Traffic tunnels can be assigned a priority (0 to 7, 0-highest, 7-lowest) that signifies their "importance."

Setup-Priority: Used when a tunnel is being established. Lower values are higher priority (0 = highest priority, 7 = lowest).

Hold-Priority: Defines whether an existing tunnel can be preempted by a higher-priority tunnel.

When you are setting up a new tunnel or rerouting, a higher-priority tunnel can tear down (pre-empt) a lower-priority tunnel; in addition, a new tunnel of lower priority may fail to set up because some tunnels of a higher priority already occupy the required bandwidth of the lower-priority tunnel.

By default the same value, setup can NOT be better or numerically lower than hold, so it must always be setup >= hold

Example

If both tunnels try to reserve bandwidth, Tunnel1 (priority 1) will always get preference.

If Tunnel2 is already established and Tunnel1 is created later, Tunnel2 will be torn down (preempted) if there’s insufficient bandwidth.

Tunnel2 may fail to establish if bandwidth is occupied by higher-priority tunnels.

Tunnel 1: High-Priority TE Tunnel (Critical Traffic)

interface Tunnel1

ip unnumbered Loopback0

tunnel mode mpls traffic-eng

tunnel destination 192.168.1.1

tunnel mpls traffic-eng bandwidth 50000 ! 50 Mbps reserved

tunnel mpls traffic-eng priority 1 1 ! Setup-priority = 1, Hold-priority = 1 (High Priority)

tunnel mpls traffic-eng path-option 1 dynamic

Tunnel 2: Low-Priority TE Tunnel (Best Effort)

interface Tunnel2

ip unnumbered Loopback0

tunnel mode mpls traffic-eng

tunnel destination 192.168.1.2

tunnel mpls traffic-eng bandwidth 50000 ! 50 Mbps reserved

tunnel mpls traffic-eng priority 7 7 ! Setup-priority = 7, Hold-priority = 7 (Lowest Priority)

tunnel mpls traffic-eng path-option 1 dynamic

Note This tunnel can be preempted by higher-priority tunnels (like Tunnel1).

Adaptability: indicates whether the traffic tunnel should be reoptimized, and consequently rerouted to another path, primarily because of the changes in resource availability.

Resilience: define the behavior of the tunnel in faulty conditions or if the tunnel becomes noncompliant with tunnel attributes (for example, the required bandwidth).

The resilience attribute determines the behavior of the tunnel under faulty conditions; it can specify the following behavior:

Not to reroute the traffic tunnel at all.

To reroute the tunnel through a path that can provide the required resources.

To reroute the tunnel though any available path, irrespective of available link resources.

### RSVP-TE path setup and admission control

Path setup is initiated by the headend routers. To signal the calculated path across the network, an RSVP Path message is sent to the tail-end router by the headend router for each tunnel the headend creates. This process occurs in the MPLS control plane.

RSVP is used to establish TE tunnels and to propagate the labels. The new LSP established by RSVP is essentailly an overlay LSP with newly allocated labels

RSVP plays a significant role in path setup for LSP tunnels and supports both unicast and multicast applications.

RSVP dynamically adapts to changes either in membership (for example, multicast groups) or in the routing tables.

Additionally, RSVP transports traffic parameters and maintains the control and policy over the path. The maintenance is done by periodic refresh messages that are sent along the path to maintain the state.

In the normal usage of RSVP, the sessions are run between hosts. In TE, the RSVP sessions are run between the routers on the tunnel endpoints. The following RSVP message types are used in path setup:

Path

Resv

PathTear - to reuse router resources for other reservation requests

ResvErr

PathErr

ResvConf

ResvTear

The RSVP Path message carries the explicit route (the output of the path selection process) computed for this traffic tunnel, consisting of a sequence of label switching routers. The RSVP Path message always follows this explicit route. Each intermediate router along the path performs trunk admission control after receiving the RSVP Path message. When the router at the end of the path (tail-end router) receives the RSVP Path message, it sends an RSVP RESV message in the reverse direction toward the headend of the traffic tunnel. As the RSVP RESV message flows toward the headend router, each intermediate node reserves bandwidth and allocates labels for the traffic tunnel. When the RSVP RESV message reaches the headend router, the LSP for the traffic tunnel is established.

Trunk admission control is used to confirm that each device along the computed path has sufficient provisioned bandwidth to support the resource requested in the RSVP Path message. When a router receives an RSVP Path message, it checks whether there is enough bandwidth to honor the reservation at the setup priority of the traffic tunnel. If there is enough provisioned bandwidth, the reservation is accepted, otherwise the path setup fails. When the router receives the RSVP Resv message, it reserves bandwidth for the LSP. If pre-emption is required, the router must tear down existing tunnels with a lower priority. As part of trunk admission control, the router must do local accounting to keep track of resource utilization and trigger IS-IS or OSPF updates when the available resource crosses the configured thresholds.

The RSVP Path message contains several objects, including the session identification, which uniquely identifies the tunnel. The traffic requirements for the tunnel are carried in the session attribute. The label request that is present in the message is handled by the tail-end router, which allocates the respective label for the LSP.

New RSVP-TE objects were introduced to enable label signaling for MPLS-TE:

LABEL\_REQUEST (PATH) Sent in PATH messages. Indicates that a downstream node (next hop) needs a label from the upstream node.

LABEL (RESV) Sent in RESV messages. Carries the label assigned by a downstream node to an upstream node. The LFIB is populated accordingly

EXPLICIT\_ROUTE (ERO) sent in path message - defines the explicit route for the LSP. Overrides standard routing decisions to enforce traffic engineering policies.

RECORD\_ROUTE (RRO) Can be carried in both PATH and RESV messages. Allows the Ingress LER (the head-end router) to receive a record of the actual path the LSP has traversed through the network. Each router that processes an RRO message adds its own interface address to the object.

SESSION\_ATTRIBUTE (PATH) sent in path message - Contains attributes such as setup priority, holding priority, and other LSP parameters.

The explicit route object (ERO) is populated by the list of next hops, that are either manually specified or calculated by CBR. The previous hop (PHOP) is set to the outgoing interface address of the router. The record route object (RRO) is populated with the same address as well.

As the next-hop router (R2) receives the RSVP Path message, the router checks the ERO and looks into the L bit regarding the next-hop information. If this bit is set and the next hop is not on a directly connected network, the node performs a CBR calculation (path calculation, or PCALC) using its TE database and specifies this loose next hop as the destination.

Then the intermediate routers along the path (indicated in the ERO) perform the traffic tunnel admission control by inspecting the contents of the session attribute object. If the node cannot meet the requirements, it generates a PathErr message. If the requirements are met, the node is saved in the RRO.

The R2 router takes the PHOP IP address and interface from the received RSVP Path message and places that information into its path state block (PSB). That information will be used later to send RSVP Resv message back to the headend router. The R2 router also removes its interface entry from the ERO. R2 adjusts the PHOP to the address of its own interface and adds that egress address to the RRO. The Path message is then forwarded to the next hop in the ERO.

Several other functions are performed at each hop as well, including traffic admission control.

![](<../.gitbook/assets/Unknown image (1713)>)

The result of receiving RSVP Path message with label request object by the tail-end router (the endpoint of the tunnel) is generation of the RSVP Resv message and allocation of the path label for the specified LSP tunnel (session). The label is placed in the corresponding label object of the RSVP Resv message. The RSVP message is sent back to the headend following the reverse path that is recorded in the ERO and is stored at each hop in its path state block.

Because R5 is the tail-end router, it does not allocate a specific label for the LSP tunnel. The implicit null label is used instead (the value "POP" in the label object).

The Resv message is forwarded to the next-hop address in the path state block of the tail-end router.

The next-hop information in the path state block was established when the Path message was traveling in the opposite direction (headend to tail end).

The RSVP Resv message travels back to the headend router. On each hop (in addition to the admission control itself) label handling is performed. As you can see from the RSVP Resv message that is shown in the figure, the following actions were performed at the intermediate hops (R4, R3, R2): The interface address of an intermediate router is put into the PHOP field and added to the beginning of the RRO list. The incoming labels are allocated for the specified LSP at each hop by the local router.

When the RSVP Resv message flows back toward the sender, the intermediate nodes reserve the bandwidth and allocate the label for the tunnel. The labels are placed into the Label object of the Resv message.

![](<../.gitbook/assets/Unknown image (1714)>)

### Traffic steering

A traffic tunnel doesn't normally show up in the IP routing table or SPF (Shortest Path First) calculations.

There are four ways to direct traffic through a tunnel:

1. Static Routes – Manually set routes that point to the tunnel.

The routing table lists all eight loopback routes and associated information. Only the statically configured destinations (R4 and R5) list tunnels as their outgoing interfaces. For all other destinations, the normal IGP routing is used and results in physical interfaces (along with next hops) as the outgoing interfaces towards these destinations. The metric to the destination is the normal IGP metric. Even for the destination that is behind each of the tunnel endpoints, the normal IGP routing is performed if there is no static route to the traffic-engineered tunnel.

![](<../.gitbook/assets/Unknown image (1715)>)

2. Policy-Based Routing (PBR) – Uses policies to forward traffic through the tunnel.

The first two methods are manual and not scalable. Static routes only send specific traffic through the tunnel, while everything else follows the normal IGP path

3. Autoroute Announce feature – The autoroute feature enables the headend router to see the Cisco MPLS TE tunnel as a directly connected interface and to use it in its modified SPF computations. With the autoroute feature, the traffic-engineered tunnel appears in the IP routing table as well, but this appearance is restricted to the tunnel headend only. Normally, an MPLS TE tunnel does not appear in the IP routing table by default.

The autoroute feature enables all the prefixes that are topologically behind the Cisco MPLS TE tunnel endpoint (tail end) to be reachable via the tunnel itself (unlike with static routing, where only statically configured destinations are reachable via the tunnel).

The autoroute feature affects the headend router only and has no effect on intermediate routers. These routers still use normal IGP routing for all the destinations.

During installation of the best paths to the destination, the tunnel metric is compared to other existing tunnel metrics and to all the native IGP path metrics. The lower metric is better and if the Cisco MPLS TE tunnel has an equal or lower metric than the native IGP metric, it is installed as a next hop to the respective destinations.

If there are tunnels with equal metrics, they are installed in the routing table and provide for load balancing. The load balancing is done proportionally to the configured bandwidth of the tunnel.

Default metric:

The routing table shows all destinations at the endpoint of the tunnel and behind it (R8) as reachable via the tunnel itself. The metric to the destination is the normal IGP metric.

The LSP for T1 follows the path R1-R2-R3-R4 and the tunnel cost is the best IGP metric (30) to the tunnel endpoint. The metric to R8 is 40 (T1 plus one hop).

The LSP for T2 follows the path R1-R6-R7-R4-R5. Although the LSP passes through the R7-R4 link, the overall metric of the tunnel is 40 (the sum of metrics on the best IGP path R1-R2-R3-R4-R5).

In the routing table, all the networks that are topologically behind the tunnel endpoint are reachable via the tunnel itself. Because, by default, the Cisco MPLS TE tunnel metric is equal to the native IGP metric, the tunnel is installed as a next hop to the respective destinations.

![](<../.gitbook/assets/Unknown image (1716)>)

Depending on the ability of the IGP to support load sharing, the native IP path may also show up in the routing table as a second path option.

In this example, there appear to be two paths to R4, while there is only one physical path. For R5, there appear to be three paths, two of which do not follow the desired tunnel path.

The tunnel metrics can be tuned. Either relative or absolute metrics can be used to resolve this issue.

![](<../.gitbook/assets/Unknown image (1717)>)

Absolute Metric: Assigns a fixed value to the tunnel metric (e.g., setting it to 35) without considering the original IGP metric.

Relative Metric: Adjusts the tunnel metric by modifying the existing IGP metric (e.g., native IGP metric minus 2). This keeps the metric relative to the original path.

In this example, the relative metric is used to control path selection. T1 is given a value of the native IGP value -2 (28), making it preferred to the native IP path.

T2 could have been given the native IGP value -4 (36), giving it preference to the native IP path and the T1-R4 path. However, in this example, it was given an absolute value of 35.

![](<../.gitbook/assets/Unknown image (1718)>)

4. Class-Based Tunnel Selection (CBTS) in IOS-XE Policy-Based Tunnel Selection (PBTS) in IOS-XR selects the tunnel based on the classification criteria of the incoming packets, which are based on the experimental (EXP), differentiated services code point (DSCP), or type of service (ToS) field in the packet and forwards traffic to TE tunnels associated with that class-map.

A class-map is defined for various types of packet, and associates this class-map with a forward-class. The class-map defines the matching criteria for classifying a particular type of traffic, while the forward-class defines the forwarding path these packets should take. After a class-map is associated with a forwarding-class in the policy-map, all the packets that match the class-map are forwarded as defined in the policy-map. The egress TE tunnel interfaces route traffic based on the forwarding-class. For each forwarding-class, specify the TE interface explicitly or implicitly in the case of default value with the forward group.

Default-class for paths is always zero (0). If there’s no MPLS TE tunnel for a given forward-class, then the default-class (0) is applied. If there’s no default-class, then the packet is dropped.

[https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/mpls/24xx/configuration/guide/b-mpls-cg-cisco8000-24xx/implementing-mpls-traffic-engineering.html#:\~:text=enabled%20with%20PBTS.-,Configure%20Policy%2DBased%20Tunnel%20Selection,-Perform%20the%20following](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/mpls/24xx/configuration/guide/b-mpls-cg-cisco8000-24xx/implementing-mpls-traffic-engineering.html)

5. Forwarding Adjacency (FA)

To include tunnels in the SPF calculations, use forwarding adjacencies. Forwarding adjacency is a mechanism to allow the announcement of established tunnels via IGP:

MPLS TE tunnels are unidirectional, but IS-IS or OSPF assume links are bidirectional. Because forwarding adjacency advertises the TE tunnel as a link in your IGP, we need tunnels in both directions. If you only configure unidirectional TE tunnels and enable forwarding adjacency, the IGP won’t use the link.

All routers in the area are now aware of the tunnel and can run their SPF accordingly

The headend still needs to steer traffic that reaches it, into a tunnel, using the above forwarding methods

Mechanism for:

Better intra- and inter-POP load balancing

Tunnel sizing independent of inner topology

Routers can then use these advertisements in their IGPs to compute the SPF even if they are not the head end of any TE tunnels

Note Do not specify the tunnel mpls traffic-eng autoroute announce command in your configuration when you are using forwarding adjacency.

The same tunnel appears twice in the IGP—once as a route (via autoroute) and once as a link (via FA).

This confuses SPF calculations, causes looping, or results in suboptimal path selection.

autoroute announce Adds the TE tunnel as a route to the RIB.

Forwarding adjacency Adds the TE tunnel as a link in the IGP topology database.

Instead use static, PBR,PCE or SR-TE to steer the traffic to the tunnel

| Router(config-if)# tunnel mpls traffic-eng forwarding-adjacency \[holdtime value ] | holdtime value - (Optional) Time in milliseconds (ms) that a TE tunnel waits after going down before informing the network. The range is 0 to 4,294,967,295 ms. The default value is 0. |
| ---------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Example

Imagine we have 25 Points of Presence (POP), where each POP has two PE routers and ten CE routers.

25 POPs x 10 CE routers = 250 CE routers.

All CE routers have to communicate with each other, so we need a full mesh of TE tunnels. The full mesh formula looks like this:

number of TE tunnels = N \* (N-1) / 2

Where N is the number of CE routers. This means we’ll have the following:

250 x (250 - 1) / 2 = 31.125 unidirectional TE tunnels.

TE tunnels are unidirectional, so you’ll need two tunnels if you want to load balance from CE1 to CE2 and vice versa. We’ll have 31.125 x 2 = 62.250 bidirectional TE tunnels. That is a lot of LSPs!

We can solve the scalability issue by moving the TE tunnels up one level in the hierarchy:

![](<../.gitbook/assets/Unknown image (1719)>)

We now have a TE tunnel between PE1/PE3 and PE2/PE4. The CE routers won’t have to run MPLS TE anymore. This also dramatically reduces the number of required TE tunnels.

With 25 POPs and 50 PE routers, we’ll have the following:

50 x (50 - 1) / 2 = 1.225 undirectional TE tunnels.

We want to load balance in both ways, so we’ll have 1.225 x 2 = 2.450 bidirectional TE tunnels. That is a significant improvement.

However, this introduces another issue…

The problem is that our CE routers don’t know about the TE tunnels on the PE routers. Enabling autoroute doesn’t solve this issue because it’s a local change in the routing table of the PE routers. The PE routers won’t advertise anything to other routers.

This is why we need forwarding adjacency. It allows the PE routers to advertise the TE tunnel as a link in your IGP.

![](<../.gitbook/assets/Unknown image (1720)>)

Traffic flow without Forwarding Adjacency :

Tunnels created and announced to IP with autoroute announce (visible only for Head-end router of TE tunnel) with equal cost to load-balance.

All the POP-to-POP traffic exits via the routers on the IGP shortest path (Router A do not know anything about MPLS TE Tunnels):

No load balancing

All traffic flows on path: A -> B {tunnel B -> D} -> F

![](<../.gitbook/assets/Unknown image (1721)>)

Traffic flow with Forwarding Adjacency:

2 MPLS TE tunnels, the same static IGP metric, propagated as Point-to-Point link to IGP - load balancing achieved

Tunnels must have a "manually" set COST (the same) so that from the point of view of IGP or SPF, there is an equal cost load balancing

Inner Topology does not affect Tunnel Sizing:

Change in the core topology does not affect the load balancing in the POP (MPLS TE IGP cost stay unchanged)

Routers outside MPLS TE domain will see topology in MPLS TE domain as regular link advertised via LSA

Note: MPLS TE forwarding adjacency tunnels must be configured bidirectionally.

![](<../.gitbook/assets/Unknown image (1722)>)

### L2 tunnel selection (pseudowire preferred-path)

Preferred-path command under pw-class is an MPLS-TE steering mechanism that allows to select an MPLS-TE tunnel for a specific pseudowire

disable-fallback (Optional) Disables the router from using the default path when the preferred path is unreachable.

| IOS-XE(config-pw-class)# preferred-path interface tunnel1 \[disable fallback] | IOS-XR(config-l2vpn-pwc-mpls)#preferred-path interface tunnel-te 100 fallback disable |
| ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |

### Path maintenance (reoptimization and restoration)

refers to two operations: path reoptimization and restoration.

Path rerouting may result from link failures that affect the LSP or from the reoptimization due to a change in network resources.

Because the tunnel is not linked to the LSP that is carrying it, the actual path can dynamically change without affecting the tunnel.

The reoptimization is done on a periodic basis. At certain intervals, a check for the most optimal paths for LSP tunnels is performed and if the current path is not the most optimal, tunnel rerouting is initiated.

The device (headend router) first attempts to signal a better LSP, and only after the new LSP setup has been established successfully, will the traffic be rerouted from the former tunnel to the new one. Default reoptimization period of a TE tunnel is 1 hour.

| (config)# mpls traffic-eng reoptimize timers frequency 30 |   |
| --------------------------------------------------------- | - |

The following figure shows how the nondisruptive rerouting of the traffic tunnel is performed. Initially, the explicit route object (ERO) lists the LSP R1-R2-R6-R7-R4-R9, with R1 as the headend and R9 as the tail end of the tunnel.

The changes in available bandwidth on the link R2-R3 dictate that the LSP be reoptimized. The new path R1-R2-R3-R4-R9 is signaled, and parts of the path overlap with the existing path. Still, the current LSP is used.

After the new LSP is successfully established, the traffic is rerouted to the new path and the reserved resources of the previous path are released. The release is done by the tail-end router, which initiates an RSVP PathTear message

![](<../.gitbook/assets/Unknown image (1723)>)

### Implementation

Before yu begin: configure the IGP as well as MPLS in the core

[https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/mpls/b-mpls/m\_mp-te-autotunnel-0.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/mpls/b-mpls/m_mp-te-autotunnel-0.html)

Enable Cisco MPLS TE in the core globally and on the interface

![](<../.gitbook/assets/Unknown image (1724)>)

Configure RSVP in the core

Use the bandwidth command to set the reservable bandwidth, the maximum RSVP bandwidth that is available for a flow and the subpool bandwidth on this interface:

| bandwidth total-bandwidth max-flow sub-pool sub-pool-bw | xr |
| ------------------------------------------------------- | -- |
| ip rsvp bandwidth \[interface-kbps \[single-flow-kbps]] | xe |

![](<../.gitbook/assets/Unknown image (1725)>)

Enable Cisco MPLS TE support in the core IGP with OSPF or IS-IS.

Cisco MPLS TE support for IGP should be enabled on all routers on the path from the headend to the tail end of the Cisco MPLS TE tunnel.

A router identifier must be present in IGP configuration. This router identifier acts as a stable IP address for the TE configuration. This stable IP address is flooded to all nodes. For all TE tunnels that originate at other nodes and end at this node, the tunnel destination must be set to the TE router identifier of the destination node, because that identifier is the address that the TE topology database at the tunnel head uses for its path calculation.

| show ospf mpls traffic-eng link |   |
| ------------------------------- | - |

![](<../.gitbook/assets/Unknown image (1726)>)

To turn on flooding of Cisco MPLS TE link information into the indicated IS-IS level, use the mpls traffic-eng command in router configuration mode on Cisco IOS XE Software. This command appears as part of the routing protocol tree and causes link resource information (for instance, the bandwidth available) for appropriately configured links to be flooded in the IS-IS link-state database

![](<../.gitbook/assets/Unknown image (1727)>)

Configure Cisco MPLS TE tunnels

![](<../.gitbook/assets/Unknown image (1728)>)

Example:

Now build another Cisco MPLS TE tunnel, this time on a Cisco IOS XE device. On PE3, build the Cisco MPLS TE tunnel between routers PE3 and PE1, using these attributes:

Assign the Loopback 0 interface as the source address of the tunnel.

Set the bandwidth that is required on this interface to 300 kbps.

Set the setup and hold priority to 0.

Assign the PE1 Loopback 0 interface as the destination IP address of the tunnel.

Set the path option to dynamic.

Use the autoroute feature to automatically route traffic to prefixes behind router PE1 by using an absolute metric of 1.

Answer

| PE3(config)# interface Tunnel1 PE3(config-if)# ip unnumbered Loopback0 PE3(config-if)# tunnel mode mpls traffic-eng PE3(config-if)# tunnel destination 10.1.1.1 PE3(config-if)# tunnel mpls traffic-eng autoroute announce PE3(config-if)# tunnel mpls traffic-eng autoroute metric absolute 1 PE3(config-if)# tunnel mpls traffic-eng priority 0 0 PE3(config-if)# tunnel mpls traffic-eng bandwidth 300 PE3(config-if)# tunnel mpls traffic-eng path-option 1 dynamic | setup-priority – 0 (best) to 7 (worst). Default is 7 hold-priority – 0 (best) to 7 (worst). Default is 7 | RP/0/RP0/CPU0:PE1(config)# interface tunnel-te1 RP/0/RP0/CPU0:PE1(config-if)# ipv4 unnumbered Loopback0 RP/0/RP0/CPU0:PE1(config-if)# priority 0 0 RP/0/RP0/CPU0:PE1(config-if)# signalled-bandwidth 100 RP/0/RP0/CPU0:PE1(config-if)# autoroute announce RP/0/RP0/CPU0:PE1(config-if-tunte-aa)# metric absolute 1 RP/0/RP0/CPU0:PE1(config-if-tunte-aa)# exit RP/0/RP0/CPU0:PE1(config-if)# destination 10.3.3.3 RP/0/RP0/CPU0:PE1(config-if)# path-option 1 dynamic |   |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

Configure routing onto Cisco MPLS TE tunnels

After your tunnel is up and running, you need to route traffic into a Cisco MPLS TE.

The autoroute feature evaluates Cisco MPLS TE tunnel interface with an IGP, and automatically routes traffic to prefixes behind the Cisco MPLS TE tunnel, based on IGP metrics.

Another option to route traffic into a Cisco MPLS TE tunnel is by using a static route. You should route traffic to the prefixes behind MPLS tunnel l to the interface tunnel, in the example tunnel-te1. This configuration is used for static routes when the autoroute announce command is not used.

![](<../.gitbook/assets/Unknown image (1729)>)

### Monitoring RSVP

| show rsvp session                                          | verifies that all routers on the path of the LSP are configured with at least one path state block (PSB) and one reservation state block (RSB) per session. |
| ---------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show mpls traffic-eng link-management bandwidth-allocation |                                                                                                                                                             |

![](<../.gitbook/assets/Unknown image (1730)>)

| show mpls traffic-eng tunnels            | To display information about Cisco MPLS TE tunnels                                                                    |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| show mpls forwarding prefix 10.3.10.1/32 | verify that the traffic towards CE3 Loopback 0 network (10.3.10.1/32) is MPLS switched via the tunnel interface (tt1) |

![](<../.gitbook/assets/Unknown image (1731)>)

### Unequal-cost load balancing

By default link-state routing protocols don't support Unequal Cost Load Balancing, but with TE we can specify the bandwidth for each tunnel and if the tunnels will be configured with the same metric it will load balance across the both tunnel unequally based on the bandwidth assigned to each bandwidth = more traffic over higher bandwidth tunnel

![](<../.gitbook/assets/Unknown image (1732)>)

### Path rerouting on link failure

For example, a flapping link can result in headend routers being constantly involved in constraint-based computations. Because the time that elapses between link failure detection and the establishment of a new LSP can cause delays for critical traffic, there is a need for pre-established alternative paths (backups).

When a link that makes up a certain traffic tunnel fails, the headend of the tunnel detects the failure in one of two ways:

The IGP (OSPF or IS-IS) sends a new link-state packet with information about changes that have happened.

RSVP alarms the failure by sending an RSVP PathTear message to the headend.

Link-failure detection, without any preconfigured or precomputed path at the headend, results in a new path calculation (using a modified SPF algorithm) and consequently in a new LSP setup.

The tunnel interface that is used for the specified traffic tunnel (LSP) goes down if the specified LSP is not available for 10 seconds. In the meantime, the traffic that is intended for the tunnel continues using a broken LSP, resulting in black hole routing.

When the router along the dynamic LSP detects a link failure, it sends an RSVP PathTear message to the headend.

This message signals to the headend that the tunnel is down. The headend clears the RSVP session, and a new path calculation (PCALC) is triggered using a modified SPF algorithm.

There are two possible outcomes of the PCALC calculation:

No new path is found. The headend sets the tunnel interface down.

An alternative path is found. The new LSP setup is triggered by RSVP signaling, and adjacency tables for the tunnel interface are updated. Also, the Cisco Express Forwarding table is updated for all the entries that are related to this tunnel adjacency.

In the presence of two tunnels, static routing is deployed with two floating static routes pointing to the tunnels.

When the primary tunnel (Tunnel-te 1) fails, the static route is gone and the traffic is transitioned to the secondary tunnel. The traffic is returned to the primary tunnel if the conditions support the re-establishment of traffic.

Other options include spreading the load proportionally to the requested bandwidth using the Cisco Express Forwarding mechanism, load balancing, or by having one group of static routes pointing to Tunnel-te 1 and another to Tunnel-te 2.

The example shows two configured tunnels: Tunnel-te 1 (following the LSP path Core 3-Core 2-POP A) and Tunnel-te 2 (using a dynamic path).

When the primary tunnel (Tunnel-te 1) fails, the static route is gone and the traffic is transitioned to the secondary tunnel. The traffic is returned to the primary tunnel if the conditions support the re-establishment of traffic.

![](<../.gitbook/assets/Unknown image (1733)>)

![](<../.gitbook/assets/Unknown image (1734)>)

#### Drawbacks of backup TE tunnels

There is an obvious benefit to having a preconfigured backup tunnel. However, the solution presents some drawbacks as well:

The backup tunnel requires all the mechanisms that the primary one requires. The labels must be allocated and bandwidth must be reserved for the backup tunnel as well.

From the RSVP perspective, the resource reservations (bandwidth) are counted twice.

Having two pre-established paths is the simplest form of Cisco MPLS TE path protection. Another option is to use the precomputed path only and establish the LSP path on demand. In the latter case, there is no overhead in resource reservations.

### Fast reroute (FRR)

is a mechanism for protecting a Cisco MPLS TE LSP from link and node failures by locally repairing the LSP at the point of failure. The FRR mechanism allows data to continue to flow while the headend router attempts to establish a new end-to-end LSP that bypasses the failure. FRR locally repairs any protected LSPs by rerouting them over backup tunnels that bypass failed links or nodes. The headend is notified of the failure through the IGP and through RSVP. The headend router uses a presignaled LSP to bypass the failure point.

MPLS TE FRR specifications offer two protection techniques: facility backup and one-to-one backup.

Facility backup uses label stacking to reroute multiple protected TE LSPs using a single backup TE LSP.

One-to-one backup does not use label stacking, and every protected TE LSP requires a dedicated backup TE LSP.

FRR is enabled/signaled via RSVP PATH messages using the SESSION\_ATTRIBUTE object, where two specific bits are set:

| **Flag Name**            | **RFC Bit** | **Function**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ------------------------ | ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Local Protection Desired | 0x20        | Informs routers along the path (typically PLRs) that this LSP should be protected by local repair (e.g., Fast Reroute).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Label Recording Desired  | 0x10        | Requests routers along the path to record labels, necessary for local protection so the PLR can know what label to swap. Example: If the backup tunnel (B→E→D) is already set up, and the PLR (B) is the headend of that tunnel, doesn't it already know what label to use to send traffic into it? Why does it need the Label Recording Desired flag on the primary LSP?" The PLR does know the label for the backup tunnel (B→E→D). BUT! The real reason for Label Recording Desired isn't about the backup tunnel. It’s about how to mimic the forwarding behavior of the primary LSP, even when traffic is rerouted. The Real Purpose: Matching the Primary LSP Label Stack Let’s build the picture properly: Primary Tunnel A → B → C → D RSVP sets up an LSP from A to D B is the PLR C is the next hop (potential failure point) Backup Tunnel B → E → D (a separate tunnel, pre-signaled) B is headend of this backup What does B (PLR) do during a failure? Let’s say the B→C link fails. Now, B wants to reroute packets into the backup tunnel. When B was forwarding traffic to C, it was swapping labels based on the primary LSP. But now, it has to recreate the exact same behavior — the label(s) that D expects to see at the end. And that’s where the Label Recording Desired bit comes in: It allows B to learn what label D is expecting in the context of the primary LSP, so that when B reroutes traffic via the backup tunnel, it pushes the correct label that matches what D expects. Because the backup tunnel (B→E→D) is just a pipe — it doesn’t know anything about the primary LSP! So to preserve the forwarding behavior, B must: Use the backup tunnel to reach the same endpoint (D). Still send the label that was being used in the primary LSP so that D can process the packet as if nothing broke The reason we must preserve the original (primary) label stack — and not use the backup tunnel's label stack — is because the backup tunnel is not end-to-end from the ingress (A) to the egress (D). It's only a local detour, built from the PLR (e.g., B) to some downstream router (like D or a merge point). So, the packet is still part of the original primary LSP, and the downstream routers (like D) are expecting the label assigned in the original RESV path. If the PLR were to use the label stack of the backup tunnel (which might be for a completely different LSP), router D wouldn't recognize the inner label — it would drop the packet or misroute it. Preserving the primary label stack ensures the tail-end router sees the same label as before, processes it identically, and has no idea a failure even happened. That’s how Fast Reroute achieves traffic restoration in <50ms — no re-signaling needed. PLR adds two labels to the packet (one for the backup tunnel, one for the primary LSP). The tail-end router (D) only cares about the inner label (the original label for the LSP). The merge point router (like D) pops the outer label (backup tunnel) and forwards the packet using the inner label (the original LSP label). |

A PLR (Point of Local Repair) listens to PATH messages coming from upstream.

If it sees the Local Protection Desired bit, it knows this LSP should be protected.

It also needs Label Recording Desired so it knows the downstream label (for label swap during FRR)

Note Both are set to 1 with the fast reroute command

![Mpls Te Frr Rsvp Link Protection Desired](<../.gitbook/assets/Unknown image (1735)>)

#### Link protection

Backup tunnels that bypass only a single link of the LSP path provide link protection.

These tunnels are referred to as next-hop backup tunnels because they terminate at the next hop of the LSP beyond the point of failure.

Three mechanisms cause routers to switch LSPs onto their backup tunnels:

Interface down notification

Loss of Signal

RSVP Hello neighbor down notification

The node that redirects the traffic onto the preset backup path is called the point of local repair (PLR), and the node where a backup LSP merges with the primary LSP is called the merge point (MP). The PLR in the example is the Core-1 router, and MP is the Core-6 router.

Paths for LSPs are calculated at the LSP headend. Under failure conditions, the headend determines a new route for the LSP

However, because of messaging delays, the headend cannot recover as fast as making a repair at the point of failure.

To avoid packet flow disruptions while the headend is performing a new path calculation, the FRR option of Cisco MPLS TE is available to provide protection from link or node failures (failure of a link or an entire router).

The function is performed by routers that are directly connected to the failed link, because they reroute the original LSP to a preconfigured tunnel and therefore bypass the failed path.

The reaction to a failure, with such a preconfigured tunnel, is almost instant. The local rerouting takes less than 50 ms, and a delay is only caused by the time that it takes to detect the failed link and to switch the traffic to the link protection LSP.

When the headend of the tunnel is notified of the path failure through the IGP or through RSVP, it attempts to establish a new LSP.

Normally, FRR protects a complete link. At a given time, any number of TE LSPs with a certain amount of bandwidth might be crossing the protected link. But, the backup tunnel does not reserve bandwidth, therefore, if the protected tunnels use backup tunnel, there may be chances of some traffic getting dropped.

![](<../.gitbook/assets/Unknown image (1736)>)

#### Node protection

Backup tunnels that bypass next-hop nodes along LSP paths are called next-next-hop (NNHOP) backup tunnels because they terminate at the node following the next-hop node of the LSP path, thereby bypassing the next-hop node. They protect LSPs, if a node along their path fails, by enabling the node upstream of the failure to reroute the LSPs and their traffic around the failed node to the next-next hop.

FRR supports the use of RSVP hellos to accelerate the detection of node failures. Next-next hop backup tunnels also provide protection from link failures, because they bypass the failed link and the node. In node protection, next-next hop is the MP router.

When a router link or a neighboring node fails, the router often detects this failure by an "interface down" notification. On a typical router interface, this notification is very fast. When a router notices that an interface has gone down, it switches LSPs going out that interface onto their respective backup tunnels (if any).

RSVP hellos can also be used to trigger FRR. If RSVP hellos are configured on an interface, messages are periodically sent to the neighboring router. If no response is received, the hellos declare that the neighbor is down. This action causes any LSPs going out that interface to be switched to their respective backup tunnels.

![](<../.gitbook/assets/Unknown image (1737)>)

Point of Local Repair (PLR) swaps next-hop label and pushes backup label

![](<../.gitbook/assets/Unknown image (1738)>)

![](<../.gitbook/assets/Unknown image (1739)>)

#### FRR implementation

Enable FRR on LSPs.

Create a backup tunnel to the next hop or to the next-next hop.

Assign backup tunnels to a protected interface.

IOS XR

![](<../.gitbook/assets/Unknown image (1740)>)

IOS XE

![](<../.gitbook/assets/Unknown image (1741)>)

### End-to-end path protection

provides full LSP backup by pre-signaling a secondary path, ensuring immediate traffic rerouting if the primary path fails anywhere along its route. It relies on failure detection mechanisms like BFD, IGP, and RSVP to trigger the switchover. In contrast, Backup Tunnels (such as Link or Node Protection) focus on protecting specific links or nodes by quickly rerouting traffic locally around the failure, without waiting for the headend to recompute the path

The failure detection mechanisms trigger a switchover to a secondary tunnel by:

Path error or resv-tear from RSVP signaling

Notification from the BFD protocol that a neighbor is lost

Notification from the IGP that the adjacency is down

Local teardown of the protected tunnel LSP due to pre-emption to signal higher priority LSPs, a POS alarm, OIR, and so on

Coexistence of FRR and path protection is supported; this means FRR and path-protection can be configured on the same tunnel at the same time.

Path protection can be used within a single area (OSPF or IS-IS), eBGP, and static routes.

![](<../.gitbook/assets/Unknown image (1241)>)

### MPLS TE autotunnel

The MPLS Traffic Engineering-Autotunnel Primary and Backup feature enables a router to dynamically create one-hop primary tunnels on all interfaces that have been configured with MPLS traffic. The tunnels are created with zero bandwidth. The constraint-based shortest path first (CSPF) is the same as the shortest path first (SPF) when there is zero bandwidth, so the router’s choice of the autorouted one-hop primary tunnel is the same as if there were no tunnel. Because it is a one-hop tunnel, the encapsulation is tag-implicit (that is, there is no tag header).

AutoTunnel Primary and Backup feature has the following benefits:

Backup tunnels are built automatically, eliminating the need for users to preconfigure each backup tunnel and then assign the backup tunnel to the protected interface.

Protection is expanded – does protect IP and LDP traffic

If no backup tunnels exist, the following types of backup tunnels are created:

Next hop (NHOP)

Next-next hop (NNHOP)

Explicit paths are used to create backup autotunnels as follows:

NHOP excludes the protected link's IP address.

NNHOP excludes the NHOP router ID.

The explicit-path name is auto-tunnel\_tunnelxxx, where xxx matches the dynamically created backup tunnel ID.

The interface used for the ip unnumbered command defaults to Loopback0. You can configure this to use a different interface.

Primary tunnels:

Create protected one-hop tunnels on all TE links

does NOT assign bandwidth dynamically. Both primary and backup autotunnels use:

Zero bandwidth

This simplifies CSPF and avoids reservation

That’s why it doesn’t conflict with true bandwidth-engineered TE tunnels — it’s low priority, low impact.

Priority 7/7

Bandwidth 0

Affinity 0x0/0xFFFF

Auto-BW OFF

Auto-Route ON

Fast-Reroute ON

Forwarding-Adj OFF

Load-Sharing OFF

Tunnel interfaces not shown on router configuration

Auto-route forwards all traffic through one-hop tunnel

Traffic logically mapped to tunnel but no label imposed (imp-null)

| mpls traffic-eng tunnels mpls traffic-eng auto-tunnel primary onehop mpls traffic-eng auto-tunnel primary tunnel-num min 900 max 999 | Create “one-hop” auto tunnel over all MPLS TE enabled interfaces All traffic will use “tunnel” as nexhop interface |
| ------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------ |

#### Backup tunnels

Create backup tunnels automatically as needed

Detect if a primary tunnel requires protection and is not protected

Verify that a backup tunnel doesn’t already exist

Calculates a backup path that excludes the protected NHOP and NNHOP

Optionally, consider shared risk link groups (sharing same physical resource) during backup path computation

| mpls traffic-eng tunnels mpls traffic-eng auto-tunnel backup nhop-only mpls traffic-eng auto-tunnel backup tunnel-num min 1900 max 1999 mpls traffic-eng auto-tunnelbackup timers removal unused 7200 mpls traffic-eng auto-tunnelbackup srlg exclude preferred | Create automatically backup tunnel for all already unprotected tunnels (e.g. Primary autotunnels) Explicit paths are used to create backup autotunnels as follows: For NHOP Backup Autotunnels: NHOP excludes the protected link's local IP address. NHOP excludes the protected link’s remote IP address. The explicit-path name is \_autob\_nhop\_tunnelxxx, where xxx matches the dynamically created backup tunnel ID. For NNHOP Backup Autotunnels: NNHOP excludes the protected link’s local IP address. NNHOP excludes the protected link’s remote IP address NNHOP excludes the NHOP router ID of the protected primary tunnel next hop. The explicit-path name is \_autob\_nnhop\_tunnelxxx, where xxx matches the dynamically created backup tunnel ID. |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### Bandwidth protection

Integrated in the Autotunnel feature

Ensures the Backup TE LSP has enough capacity to handle Primary TE LSP traffic during a failure, avoiding congestion

Associates bandwidth with the backup tunnel (may or may not reserve it), and the PLR selects the best backup path based on NHOP/NNHOP, traffic type, and protection level (link or node)

### BFD over MPLS TE LSPs

BFD for TE tunnel is enabled at the headend by configuring BFD parameters under the tunnel. When BFD is enabled on the already up tunnel, TE waits for the bring-up timeout before bringing down the tunnel. BFD is disabled on TE tunnels by default.

![](<../.gitbook/assets/Unknown image (1242)>)

### DiffServ-aware traffic engineering (DS-TE)

Constraint-based routing selects a routing path, satisfying the service constraint.

Bandwidth that is reservable on each link for CBR is managed through two pools:

Global pool (regular TE tunnel bandwidth)

Subpool (bandwidth for "guaranteed" traffic)

There is a separate bandwidth reservation for different traffic classes.

The link bandwidth is distributed in pools or with bandwidth constraints.

DS-TE retains the same overall operation framework of Cisco MPLS TE (link information distribution, path computation, signaling, and traffic selection). However, it introduces extensions to support the concept of multiple classes and to make per-class constraint-based routing possible.

DS-TE must keep track of the available bandwidth for each class of traffic. For this reason, class types (CT) are defined.

Suppose that a network supports voice and data traffic with voice being EF PHB (EF queue) and data being best-effort (BE queue), CT1 can be mapped to the EF queue, while CT0 can be mapped to the BE queue. Separate TE LSPs are established with separate bandwidth requirements from CT0 and from CT1.

All aggregate (known as bandwidth global pool) Cisco MPLS TE traffic is mapped to CT0 by default. Cisco allows only two class types to be defined—CT0 and CT1. CT1 is known as bandwidth subpool.

Note The Cisco version, often called the "pre-standard" or "traditional" DS-TE, was developed before the IETF standards and is proprietary. It uses the concept of bandwidth pools—specifically a global pool and a sub-pool—which later mapped to the IETF's Class-Type 0 (BC0) and Class-Type 1 (BC1), respectively.

IOS XR

| Command                                                                                                                                                                                                                                    | Description                                                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **rsvp interface t\_ype interface-path-id\_** **Example:** RP/0/RP0/CPU0:router(config)# **rsvp interface pos0/6/0/0**                                                                                                                     | Enters RSVP configuration mode and selects an RSVP interface.                                                                                                                             |
| **bandwidth** \[_**total reservable bandwidth**_] \[**bc0\_ bandwidth\_**] \[**global-pool** _**bandwidth**_] \[**sub-pool&#x20;**_**reservable-bw**_] **Example:** RP/0/RP0/CPU0:router(config-rsvp-if)# **bandwidth 100 150 subpool 50** | Sets the reserved RSVP bandwidth that is available on this interface by using the prestandard DS-TE mode. The range for the _**total reservable bandwidth**_ argument is 0 to 4294967295. |
| **interface tunnel-te \_tunnel-id\_Example:** RP/0/RP0/CPU0:router(config)# **interface tunnel-te 2**                                                                                                                                      | Configures a Cisco MPLS-TE tunnel interface.                                                                                                                                              |
| **signalled-bandwidth** {_**bandwidth**_\[**class-type** ct] \| _**subpool bandwidth**_} **Example:** RP/0/RP0/CPU0:router(config-if)# **signalled-bandwidth subpool 10**                                                                  | Sets the bandwidth that is required on this interface. Because the default tunnel priority is 7, tunnels use the default TE class map (class type 1, priority 7).                         |

IOS XE

| Command                                                                                                       | Description                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Router(config)# **interface** _**interface-id**_                                                              | Moves configuration to the interface level, directing subsequent configuration commands to the specific interface that is identified by the interface ID. |
| Router(config-if)# **ip rsvp bandwidth** _**interface-kbps single-flow-kbps**_ \[**subpool&#x20;**_**kbps**_] | Enables RSVP for IP on an interface and specifies the amount of interface bandwidth (in kbps) allocated for RSVP flows (for example, TE tunnels)          |
| Router(config)# **interface tunnel-if** _**num**_                                                             | Enters tunnel interface configuration mode.                                                                                                               |
| Router(config-if)# **tunnel mpls traffic-eng bandwidth** \[_**kbps**_\| **subpool** _**kbps**_]               | Indicates that the tunnel should use bandwidth from the subpool or the global pool.                                                                       |

#### DS-TE allocation models (IETF)

Maximum Allocation Model (MAM)

BW pool applies to one class

BC (Bandwidth Constraint): Bandwidth limits for each class (e.g., BC0, BC1, BC2)

Current implementations of this model support bandwidth (BC) limits for two classes – BC0 and BC1.

![](<../.gitbook/assets/Unknown image (1243)>)

Russian Dolls Model (RDM)

BW pool applies to one or more classes

Current implementations of this model support bandwidth constraints (BC) limits for two classes – BC0 and BC1.

![](<../.gitbook/assets/Unknown image (1244)>)

![](<../.gitbook/assets/Unknown image (1245)>)

### Class-based / policy-based tunnel selection (CBTS/PBTS)

Enables EXP-based selection of tunnels to the same destination, allowing traffic differentiation based on MPLS EXP (Experimental) bits.

Head-End Policy: Policies are applied locally at the head-end router to map traffic to specific tunnels using EXP values or other criteria (e.g., destination prefixes).

![](<../.gitbook/assets/Unknown image (1246)>)

Example configuration of MPLS TE Class-Based Tunnel Selection – IOS-XE

| Router(config)# interface Tunnel65 ip unnumbered loopback0 tunnel destination 10.1.1.1 tunnel mode mpls traffic-eng tunnel mpls traffic-eng bandwidth sub-pool 30000 tunnel mpls traffic-eng exp 5 tunnel mpls traffic-eng autoroute announce tunnel mpls traffic-eng autoroute metric relative -2 ! interface Tunnel66 ip unnumbered loopback0 tunnel destination 10.1.1.1 tunnel mode mpls traffic-eng tunnel mpls traffic-eng bandwidth 50000 tunnel mpls traffic-eng exp default tunnel mpls traffic-eng autoroute announce tunnel mpls traffic-eng autoroute metric relative -2 | tunnel mpls traffic-eng exp default is used in MPLS Traffic Engineering (TE) class-based tunnel selection (CBTS) to specify a default tunnel for packets that do not have a specific EXP (Experimental) value assigned to any other tunnel in the set. When a packet arrives with an EXP value that isn't matched by a specific tunnel, and at least one tunnel is configured with the default keyword, the packet will be sent down that default tunnel. This prevents the need to manually assign every possible EXP value to a tunne |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### MPLS TE autobandwidth

Dynamically adjust bandwidth reservation based on measured traffic

Optional minimum and maximum limits

Sampling and resizing timers

Tunnel resized to largest sample since last adjustment

![](<../.gitbook/assets/Unknown image (1247)>)

| Router(config)# mpls traffic-eng auto-bw timers \[frequency seconds]                                                                                                                                     | Each X seconds monitors load average counters (5-min) for selected tunnels Default = 300 (seconds)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Router(config-if)# tunnel mpls traffic-eng auto-bw \[collect-bw] \[frequency seconds ] \[adjustment-threshold percent] \[overflow-limit number overflow-threshold percent] \[max-bw kbps] \[min-bw kbps] | collect-bw - collects output rate information for the tunnel, but does not adjust the tunnel’s bandwidth frequency seconds - interval between bandwidth adjustments. The specified interval can be from 300 to 604800 seconds (5 minutes to 7 days). Do not specify a value lower than the output rate sampling interval specified in the mpls traffic-eng auto-bw command in global configuration mode. Every frequency seconds, the router adjusts the tunnel's bandwidth reservation based on the highest 5-minute average output rate observed during that period. max-bw kbps - the maximum automatic bandwidth, in kbps, for this tunnel. The value can be from 0 to 4294967295. min-bw kbps - the minimum automatic bandwidth, in kbps, for this tunnel. The value can be from 0 to 4294967295. |
