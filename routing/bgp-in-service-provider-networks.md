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

# BGP in Service Provider Networks

### Common service provider network

The typical service provider network consists of a network core that connects various edge devices. Some of the edge devices connect customers; others connect to other service providers.

The edge devices that connect to other service providers use EBGP to exchange routing information. The edge devices that connect customers use either static routing or EBGP.

Unless MPLS is configured on the service provider backbone, routers in a transit path also must have full routing information. Therefore, these routers take part in the IBGP routing exchange.

An IGP like IS-IS or OSPF is also required within the service provider network. The IGP is used to carry internal routes, including the loopback interface addresses of IBGP-speaking routers.

The IGP provides reachability information to establish IBGP sessions and to perform the recursive routing lookup for the BGP next hop.

The IGP and BGP goes hand in hand to ensure the SP core network, each of them have specific responsibilities

Internal links can always be summarized because they are not used as BGP next-hop addresses. To facilitate proper route summarization, assign internal links and loopback interfaces on a router addresses from two different address spaces. Also, you should assign addresses to the internal links of a router depending upon which POP the routers belong to.

![](<../.gitbook/assets/Unknown image (551)>)

### BGP and IGP interaction

The volume of routing information that the BGP carries passed the limits of what is possible to carry in any IGP

BGP carries external and customer routes and no routes external to the transit AS should ever be redistributed by any router from BGP into the IGP

All external routes should be in BGP only. The IGP carries only core subnets and is needed to resolve BGP next hops (recursive lookup) and perform fast convergence after a failure in the core network. Failures internal to the network do not affect BGP as long as the BGP next hop remains reachable.

#### Reasons

Each BGP route redistributed into OSPF generates one or more LSAs, depending on the OSPF version and configuration. A large number of BGP routes can lead to a massive increase in LSA generation and flooding. This surge in LSAs consumes significant CPU, memory, and bandwidth resources on OSPF routers.

Each OSPF router builds a complete topology view based on received LSAs. A large number of redistributed BGP routes results in a massive routing table on each OSPF router.

OSPF's convergence time is directly related to the number of LSAs. A large number of LSAs can significantly increase convergence time after network changes

OSPF and EIGRP can handle several thousand prefixes efficiently. IS-IS is generally considered more scalable and can handle tens of thousands of prefixes

{% hint style="info" %}
Ideally, there will be no interaction between BGP and the IGP. The only link should be recursive next-hop resolution.
{% endhint %}

Both BGP and the configured IGP should be configured on all core routers inside the transit AS. The IGP should carry as little information as possible.

Ideally, this information includes only the links within the core network, the loopback interfaces or possibly the external subnets that are used in EBGP sessions with the neighboring AS

This information is enough to establish IBGP sessions and resolve next-hop addresses. The IGP will also work better if it carries less routing information.

In autonomous systems that provide customer connectivity (not transit service only), it is also highly recommended that the customer networks be carried in BGP

This approach reduces the amount of information in the IGP and increases IGP stability.

Routers should never accept information about local subnets from an external source

Network administrators should install inbound filters on all EBGP sessions to filter incoming routes and to reject routing information about networks local to the AS

| **no synchronization** | Disabled by default BGP synchronization requires that a BGP route must be present in the IGP routing table before it can be advertised to external peers (eBGP) BGP synchronization has to be disabled in modern Transit AS designs on all BGP routers as they don’t rely on redistribution of BGP routes into IGP |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

![](<../.gitbook/assets/Unknown image (552)>)

### Transit autonomous system

Propagates routes between remote Autonomous Systems and routes packets between remote networks

A transit AS requires interaction between EBGP and IBGP and between IBGP and an IGP in the transit AS

Routes between autonomous systems are always exchanged via External BGP (eBGP)

The only protocol that can transport all BGP attributes across the backbone is BGP inside autonomous system, called Internal BGP (iBGP)

iBGP session must be established between transit AS border routers to propagate eBGP routes

All core routers must know about all external routes

Because Redistribution of BGP routes into IGP is not scalable

Default routing is not applicable in transit AS core

#### iBGP split horizon

The AS-path attribute is not changed over an IBGP session, because the BGP update has not crossed the AS boundary. However, the AS-path attribute is the primary means of detecting routing information loops. A BGP router that encounters its own AS in the AS path of an incoming BGP update silently ignores the information. Because the BGP-speaking routers modify the AS path only during EBGP sessions, this loop-preventing mechanism is only useful between autonomous systems, not within them.

The IBGP split horizon prevents routing information loops within the AS. Routing information that is received through an IBGP session is never forwarded to another IBGP neighbor, only toward EBGP neighbors. Because of the BGP split horizon, no router can relay IBGP information within the AS—all routers must be directly updated from the border router that received the EBGP update.

**Solution**: Full mesh of IBGP sessions is required for proper IBGP update propagation

Full mesh of IBGP sessions has to be established between all BGP-speaking routers in the AS for proper IBGP route propagation

{% hint style="info" %}
Most routers perform an AS-path check before sending an update to a neighboring AS. This prevents loops early and avoids unnecessary updates.
{% endhint %}

![](<../.gitbook/assets/Unknown image (553)>)

The IBGP full-mesh is only a logical mesh of TCP sessions, physical full mesh is not required

Always run your IBGP sessions between loopback interfaces

Physical failure of an interface will not drop the IBGP session

IBGP session can be established over different path than via the direct link between iBGP neighbors

There is no automatic recovery after a failure inside the transit AS

IGP will reestablish the path between loopback interfaces

The IBGP session is not affected

To establish BGP connectivity between the loopback interfaces, the IP addresses of these interfaces have to be reachable by both routers. The IGP must carry information about the subnets that are assigned to each loopback interface so that the interfaces are reachable by all BGP routers in the AS.

The IBGP sessions that are established between loopback interfaces have increased stability. These sessions will not go down if one of the physical interfaces goes down. As long as the IGP can find any path between the two routers, the IBGP session will remain up. BGP will not notice that the IGP has changed the traffic path between the two routers.

{% hint style="info" %}
BGP runs over TCP. Short connectivity loss may not drop the session. The IGP must reconverge before the BGP keepalive/hold timers expire.
{% endhint %}

![](<../.gitbook/assets/Unknown image (554)>)

### BGP next-hop behavior

BGP is an AS-by-AS routing protocol and not a router-by-router routing protocol.

The next hop is the IP address that is used to reach the next AS, so when an update is sent to iBGP peer, it preserves the next-hop as the IP of the router in other AS

This can be changed with next-hop-self

![](<../.gitbook/assets/Unknown image (555)>)

#### Transit network using external next hops

The next-hop attribute is not changed on IBGP updates. So when the border router forwards the BGP update on IBGP sessions, the next-hop address is still set to the IP address of the far end of the EBGP session. Therefore, the receiver of IBGP updates will see the next-hop information indicating a destination that is not directly connected. To resolve this problem, the router will check its routing table and see if and how it can reach the next-hop address. This process is known as recursive routing.

All EBGP peers must be reachable by all BGP-speaking routers within the AS

EBGP next hops must be propagated by IGP running under the iBGP in the AS

To solve this we can redistribute connected interfaces into IGP at the edge routers or include links to EBGP neighbors into IGP and make them passive interfaces

However you never want to have external routes in your internal routing domain. Thus the most commonly used is to have edge routers as the next hop

![](<../.gitbook/assets/Unknown image (556)>)

#### Transit network using edge routers as next hops

Next-hop attribute is modified at the edge routers

Edge routers set themselves as the next-hop in IBGP updates, thus no redistribution of external subnets is necessary

| **neighbor next-hop-self** | BGP next hop being set to the loopback address of the service provider edge router and not to the external access link Bypass the BGP next-hop processing and announce the local IP address as the BGP next hop in outgoing updates sent to the specified neighbor Must be set on all iBGP neighbors to fully bypass iBGP next-hop processing (or set only on RRs). |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

{% hint style="info" %}
Whenever identical routes are received from iBGP and eBGP peers, the eBGP-learned route is preferred.
{% endhint %}

One of the default goals of transit packet forwarding is to propagate the transit packet toward the downstream AS as soon as possible. A border router that receives otherwise equivalent routes to the same destination over both an EBGP session and an IBGP session will prefer the information that is received through the EBGP session.

![](<../.gitbook/assets/Unknown image (557)>)

![](<../.gitbook/assets/Unknown image (558)>)

#### Unplanned transit routing issues

If the User want to reach server in AS400, then the AS300 selects the path through AS500 and not AS100 due to shorter AS\_Path and this results in making the AS500 transit network, which has an impact on network performance for AS500. This is avoided by applying outbound BGP route policies that only allow local BGP routes to be advertised to other ASes

![](<../.gitbook/assets/Unknown image (559)>)

### Scaling iBGP

#### Scalability limits of full-mesh iBGP

The general design rule in classic IBGP is to have a full mesh of IBGP sessions. But a full mesh of IBGP sessions between n number of routers would require (n \* (n – 1)) / 2 IBGP sessions. For example, an AS that contains 300 routers would require each router to be configured with 299 IBGP sessions and the total number of sessions that need to be maintained would equal (300 \* (300 – 1)) / 2 = 44850 IBGP sessions.

Every IBGP session uses a single TCP session to another IBGP peer. An update that must be sent to all IBGP peers must be sent on each of the individual TCP sessions. If a router is attached to the rest of the network over just a single link, this single link has to carry all TCP/IP packets for all IBGP sessions. This requirement results in multiplication of the update over the single link.

Route reflectors modify the classic IBGP split-horizon rule and allow a particular router to forward incoming IBGP updates to an outgoing IBGP session under certain conditions. This router becomes a concentration router, or a route reflector (RR).

BGP confederations introduce the concept of various smaller autonomous systems within the original AS. The small autonomous systems exchange BGP updates between them using intra-confederation EBGP sessions.

#### When to use route reflectors and confederations

**Route Reflectors**:

**When to use**: Whenever there are more than five BGP-enabled routers. Practically the only exception is for multihomed enterprises that only have two or three edge routers.

**Primary purpose**: To reduce the complexity of IBGP and optimize its performance.

**Effect**: Route reflectors modify IBGP split-horizon rules

**Confederations**:

**When to use**: when a large network consists of multiple administrative domains with differing routing policies.

**Primary purpose**: Confederations make it easier to split a network into smaller pieces where inbound and outbound policies can be applied on intra-confederation EBGP boundaries.

**Effect**: BGP confederations modify IBGP AS-path processing.

Also combine with route reflectors within intra-confederation AS for scalability and manageability reasons.

## Route reflector (RR)

is a BGP router that acts as a central point for other iBGP peers to exchange routing information. It an advertise iBGP learned routes to another iBGP peer and, hence, can reflect routes

IBGP peers of the route reflector fall under two categories:

**Clients** – IBGP peers that establish neighborship and accept routes from the route reflector

The route reflector and its clients form a "cluster"

Clients can have any number of EBGP sessions but may have only one IBGP session, the session with their route reflector. Clients conform to the classic IBGP split-horizon rules and forward a received route from EBGP on their IBGP neighbor sessions. But the route reflector conforms to the route reflector split-horizon rules and recognizes that it has an IBGP session to a client.

**Nonclients** - All IBGP peers of the route reflector that are not part of the cluster

Non-route reflector capable routers can participate in IBGP full-mesh or be route reflector clients

Disadvantage of route reflection is that the RR makes all decisions about the best path, which can result in that the exit/egress point of a RR client is not the best one from his perspective

![](<../.gitbook/assets/Unknown image (560)>)

![](<../.gitbook/assets/Unknown image (561)>)

![](<../.gitbook/assets/Unknown image (562)>)

#### Route reflector split-horizon rules

When the IBGP update is received from the client, the route reflector forwards the update to other IBGP neighbors, therefore alleviating the IBGP full-mesh requirement for its clients.

Similarly, when the route reflector receives an IBGP update from a neighbor that is not its client (nonclient routers), it forwards the update to all of its clients.

Forwarding of an IBGP update in a route reflector does not change the next-hop attribute or any other common BGP attribute

Nonclients conform to the classic IBGP split-horizon rules and forward a received route from EBGP on their IBGP neighbor sessions

![](<../.gitbook/assets/Unknown image (563)>)

![](<../.gitbook/assets/Unknown image (564)>)

The upper route reflector that receives a route from a nonclient sends the route to EBGP peers and clients only. This is the reason why the IBGP update is also not sent to the bottom route reflector. The client receives the IBGP update and sends it to the EBGP peers

![](<../.gitbook/assets/Unknown image (565)>)

#### Redundant route reflectors

A client may have IBGP sessions to more than one route reflector to avoid a single point of failure

Routing loop issues Each client will receive the same route from both of its reflectors

Both route reflectors will receive the same IBGP update from their client, and they will both reflect the update to the rest of the clients

As a result, each client will get two copies of all routes

**Solution**: Route Reflector clusters

![](<../.gitbook/assets/Unknown image (566)>)

### Route reflector clusters

Route reflector clusters prevent IBGP routing loops in redundant route reflector designs.

Implementation of route reflectors within the transit AS will create smaller areas (or groups) of routers called clusters

A cluster consists of route reflector routers, either redundant or nonredundant, and the client routers that are connected to them.

Each cluster must have a unique cluster-ID

Each time a route is reflected, the cluster-ID is added to cluster-list BGP attribute

The routes that already contain local cluster-ID in the cluster-list are not reflected

A route reflector router can reflect routes only within a single cluster.

A route reflector can, however, participate in another cluster but only as a client. A client can function as a client only to a route reflector belonging to the same cluster.

When a route is reflected, the reflector creates the cluster-list attribute and attaches it to the route if it does not exist. It then sets its cluster ID number in the CLUSTER\_LIST or adds its cluster ID number to an already existing cluster-list attribute. A route that is ever reflected back to the same reflector for any reason will recognize its cluster ID number in the cluster list and not forward it again. The first route reflector that reflects the route also sets a BGP attribute, called ORIGINATOR\_ID (RID of the originating/source router), and adds it to the BGP router ID of its client.

The cluster-list and originator-ID attributes are nontransitive optional BGP attributes that allow routers that do not support route reflector functionality to coexist with route reflectors and their clients in the same AS

Based on cluster-list and originator-ID attributes, routers can implement two loop-prevention mechanisms:

Any router that receives an IBGP update with the originator-ID attribute set to its own BGP router ID will ignore that update.

Any route reflector that receives an IBGP update with its cluster ID already in the cluster list will ignore that update.

BGP path selection rules have been modified to select the best route in scenarios where a router might receive reflected and nonreflected routes or several reflected routes:

The traditional BGP path selection parameters, such as weight, local preference, origin, and Multi-Exit Discriminator (MED), are compared first.

If these parameters are equal, the routes that are received from EBGP neighbors are preferred over routes that are received from IBGP neighbors.

When a router receives two IBGP routes, the nonreflected routes (routes with no originator-ID attribute) are preferred over reflected routes.

The reflected routes with shorter cluster lists are preferred over routes with longer cluster lists.

![](<../.gitbook/assets/Unknown image (567)>)

#### Design guidelines

Segment your AS into smaller clusters, each with either redundant or nonredundant route reflectors

Consider the peripheral (edge) routers as route reflector clients and the backbone routers as the route reflectors

Edge routers typically handle a significant amount of traffic. Adding the role of a route reflector to these devices can increase their load, potentially impacting performance.

Edge routers are often exposed to the internet, making them more vulnerable to attacks. Placing a route reflector on an edge router increases the attack surface.

It's generally recommended to place route reflectors in the network core for better performance, redundancy, and security.

If a client has IBGP sessions to other clients in the same cluster, those clients will receive unnecessary duplications of updates.

Make sure that no router belongs to two different clusters because this setup would represent an invalid configuration

Create a numbering plan that indicates how numbers are assigned to the clusters in the network. The plan must make sure to uniquely identify each of the clusters within the AS - recommended to use router ID as the Cluster ID

#### RR redundancy considerations

The more redundant RRs, the more:

BGP sessions per client

BGP I/O processing per client

BGP paths carried per client

RR clients also cannot remove an I-BGP route until a WITHDRAWN is received from each of its RRs

Delays or absence of a BGP WITHDRAWN for an unavailable route may slow convergence or disrupt traffic, respectively

In large scale BGP networks, it's recommended to avoid more than three (3) RRs in a cluster to reduce BGP overhead per above

#### Hierarchical route reflectors

In a large scale networks with the high number of clients, the count of iBGP sessions that the route reflector must maintain can reach to very high numbers, which will increase the resource utilization on each route reflector

The route reflector hierarchy can be implemented to reduce the count of iBGP sessions that each route reflector in a cluster must maintain

In the hierarchy each route reflector can be a client of another route reflector in a higher layer of hierarchy. The hierarchy can be as deep as needed

![](<../.gitbook/assets/Unknown image (568)>)

#### Configuration (IOS XR / IOS XE notes)

RRs are implemented on per address family basis (separately for IPv4 and IPv6).

![](<../.gitbook/assets/Unknown image (569)>)

| bgp cluster-id                                          | Optionally assigns a cluster-ID to the route reflector (default value is router-ID) Required only for clusters with redundant reflectors Cluster-ID cannot be changed after the first client is configured                                                                                                                                                           |
| ------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| neighbor <> route-reflector-client                      | On the route reflector, configures an IBGP neighbor to be a client of this reflector.                                                                                                                                                                                                                                                                                |
| neighbor <> next-hop-self                               | Configures the router as the next hop and enables BGP to send itself as the next hop for advertised routes                                                                                                                                                                                                                                                           |
| neighbor transport connection-mode \[active \| passive] | Active actively initiate TCP connection; Passive listens to TCP port 179. Used on PE/Route Reflector By default, the peer with the lowest BGP RID will be the active one and establish the connection                                                                                                                                                                |
| neighbor <> disable-connected-check                     | to disable the verification of whether the neighbor IP address is directly connected to the router or not. By default, BGP checks if the neighbor IP is connected to the router or not, and if not, it will not establish a BGP session with that neighbor. This is used to establish connection with peer that is not directly connected Note This is security risk |
| neighbor <> remove-private-as                           | removes private AS path range from AS\_Path to hide internal private ASes used in organization                                                                                                                                                                                                                                                                       |

## BGP confederations

Technique to split large transit AS into sub-autonomous systems

BGP Confederations can be used to split a network into smaller networks where inbound and outbound policies can be applied on intra-confederation EBGP boundaries. When a large autonomous system consists of multiple internal administrative domains that have differing requirements and require the creation of different routing policies, you can use BGP Confederations to split the large network along those internal administrative boundaries. This, in turn, makes it easier to apply routing policies in inbound and outbound directions between intra-confederation autonomous systems.

Provides additional flexibility – improves routing efficiency and facilitate management of routing policies

Internal ASes are hidden to the public external ASes

Within a member AS, the classic IBGP rules apply. Therefore, all BGP routers inside the member AS must still maintain a full mesh of BGP sessions.

Used in conjunction with route reflectors to create a hierarchical structure that can further reduce the number of BGP peering's

Each sub-AS in a BGP confederation is assigned a unique 16-bit or 32-bit identifier

![](<../.gitbook/assets/Unknown image (570)>)

#### AS-path changes inside a BGP confederation

**IBGP session** - AS path is not changed

**Intra-confederation EBGP session** - Intra-confederation AS number is prepended to AS path

**EBGP session with external peer** - Intra-confederation AS numbers are removed from AS path. External AS number is prepended to the AS path

The NEXT\_HOP, MED and LOCAL\_ PREF attributes are preserved through the whole confederated AS

Intra-confederation AS path is encoded as a separate segment of the AS path

Displayed in parenthesis when using IOS show commands

All routers within the BGP confederation must support BGP confederation

A router not supporting BGP confederations will reject AS path with unknown segment type

![](<../.gitbook/assets/Unknown image (571)>)

![](<../.gitbook/assets/Unknown image (572)>)

#### Configure BGP confederations

The following are the main configuration tasks when configuring BGP confederations:

Logically divide the autonomous system into smaller areas. The intra-confederation EBGP sessions must use the directly connected IP addresses to establish a session so it is recommended to align these sessions with physical interfaces. Alternatively, you must use the ebgp-multihop command.

Allocate private AS numbers to intra-confederation autonomous systems. These numbers are used to configure the BGP process.

Configure the real AS number on every router in the AS.

![](<../.gitbook/assets/Unknown image (573)>)

Cisco IOS XE will ignore the local AS number if supplied in the bgp confederation peers command:

PE03(config-router)# bgp confederation peers 65001 65002 65003

65001 Local member-AS not allowed in confed peer list, skipping

![](<../.gitbook/assets/Unknown image (574)>)

Intra-confederation AS numbers are displayed in parentheses.

The routes learned from intra-confederation EBGP neighbors are shown as confed-external.

![](<../.gitbook/assets/Unknown image (575)>)

![](<../.gitbook/assets/Unknown image (576)>)

### Multi-cluster route reflector designs

In general, there are three main variations of multi-cluster BGP Route Reflector (RR) designs:

1. Full-mesh design
2. Multi-plane design
3. Multi-tier design

Combinations of these variations can also be implemented together as hybrid designs, depending on scale, resiliency, and operational requirements.

![](<../.gitbook/assets/Unknown image (577)>)

![](<../.gitbook/assets/Unknown image (578)>)

![](<../.gitbook/assets/Unknown image (579)>)

![](<../.gitbook/assets/Unknown image (580)>)

![](<../.gitbook/assets/Unknown image (581)>)

### Other route reflector considerations

Recommend the use of Multi-instance BGP or separate BGP RRs per address family for increased fault isolation and scaling:

BGP Internet service routes (IPv4 unicast, IPv6 unicast, 6PE)

BGP VPN service routes (VPNv4, VPNv6, EVPN)

BGP transport routes (i.e., BGP-LU aka IPv4 labelled unicast)

BGP-LS for IGP topology export

Configure E-BGP max peer limits per address family based on the expected scale, but discard any extra paths instead of terminating the session

Also configure a SYSLOG warning threshold per address family

## BGP dynamic neighbors

BGP-speaking router does not discover another BGP-speaking device automatically

Established by manual configuration between routers to create a TCP session on port 179

BGP dynamic neighbor allows routers to automatically discover and establish BGP peering sessions with other routers in the network

BGP dynamic neighbors are configured using a range of IP addresses and BGP peer groups.

The router then passively listens on the TCP session initiations

TCP session is initiated by another router for an IP address in the subnet range and the new BGP neighbor is dynamically created as a member of that group

A dynamic BGP neighbor will inherit any configuration for the peer group. In larger BGP networks, implementing BGP dynamic neighbors can reduce the amount and complexity of CLI configuration and save CPU and memory usage. Only IPv4 peering is supported.

Use the following guidelines when configuring BGP Dynamic Neighbors:

Use with care as it may be a security concern (use authentication as well).

Use when there are a large number of neighbors with similar configurations and from a contiguous range of IP addresses.

Some use cases: route reflector clients, route server clients, IXP neighbors.

![](<../.gitbook/assets/Unknown image (582)>)

#### Configuration overview

Defining a peer-group

Defining a subnet range and associating it with the peer group

Limiting the maximum number of dynamic neighbors (Optional)

![](<../.gitbook/assets/Unknown image (583)>)

| bgp listen range \<network/mask> peer-group | allows Route Reflector to passively wait for client to connect without explicitly configuring their IP's, then dynamically establish peering                                                                                                                                     |
| ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bgp listen limit                            | Configure the maximum number of simultaneous BGP connections that a router will allow. This limit helps prevent the router from being overwhelmed by too many BGP connections If the router reaches the configured listen limit, it will reject any new incoming BGP connections |

## BGP peer groups

Typical service provider networks usually contain BGP-speaking routers that consist of many neighbors that are configured with the same administrative policies. These policies can be outbound route maps, distribute lists, filter lists, update source, and so on. You can group neighbors with the same update policies into peer groups to simplify configuration and, more importantly, to make BGP updates more efficient. Also you may find that all the BGP sessions of PE router to customer routers have almost identical configurations

BGP peer groups provides the required flexibility and efficiency by allowing to configure a template, or peer group, with all the BGP parameters that are to be applied to many BGP peers. Actual BGP neighbors are bound to the peer group, and you apply the peer group configuration on each of the BGP sessions. BGP neighbors of a single router can be divided into several groups, each group having its own BGP parameters. Actual neighbors are then bound to the appropriate group, resulting in an optimum BGP configuration

BGP peer group creates a neighbor parameter template

Configurable parameters include:

Community propagation

Update source and next-hop self

EBGP multihop

Authentication password

Neighbor weight

Prefix lists, filter lists and route maps

Parameter Overriding Individual parameters defined in a peer group can be overridden on a per-neighbor basis if specific configuration needs differ.

Input settings (e.g., inbound route filtering) can be defined individually for each member of the peer group.

Output settings (e.g., default-originate, outbound distribute-lists, filter-lists, and route-maps) must be uniform for all members of the peer group

#### Peer groups as a performance tool

By default Cisco IOS builds individual BGP updates for each BGP neighbor

The CPU load imposed by the BGP process is proportional to the number of BGP neighbors

BGP peer groups were introduced primarily for CPU usage optimization

The use of peer groups allows the router to run the BGP update (including all outgoing filter processing) only once for the entire peer group

CPU utilization that is imposed by BGP update generation is substantially reduced

After the router has finished building the BGP update, it sends the update to each member of the peer group

Use peer groups whenever possible to reduce the CPU load

#### Peer group restrictions

IBGP and EBGP neighbors cannot be mixed in a peer group (for example some parameter such as AS-path and local preference or even MED is handled by each of the two types differently)

Per-neighbor BGP parameters that affect outbound updates cannot be changed for peer group members (as mentioned in the peer groups as BGP performance tool, one update is replicated to all peers in a peer group)

All neighbors have to belong to the same peer group and address family. Neighbors that are configured in different peer groups can’t belong to different address families.

If any of the customers require routing information that differs from that of other members of the peer group, then that neighbor must be removed from the peer group and configured as an individual neighbor.

#### IOS XE configuration

Create a BGP peer group

| neighbor peer-group | Creates a BGP peer group (peer groups names are case-sensitive) |
| ------------------- | --------------------------------------------------------------- |

Specify parameters for the BGP peer group

| neighbor | Specifies any BGP parameter for the peer group - suc has remote-as, update-source Loopback, etc.. |
| -------- | ------------------------------------------------------------------------------------------------- |

Create and assign a BGP Neighbor into a Peer Group

| neighbor peer-group | Assigns a BGP neighbor into a peer group. The neighbor inherits all the BGP parameters specified for the peer group |
| ------------------- | ------------------------------------------------------------------------------------------------------------------- |
| neighbor            | Overrides a BGP parameter specified for the peer group with a neighbor-specific parameter                           |

Note In newer IOS versions automatic „update-group“ config is used, which is not related to update optimization...

#### Verification

| show ip bgp peer-group \[summary]          | Displays the definition of the specified peer group or all peer groups                                                                  |
| ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| clear ip bgp peer-group \[\[soft] in\|out] | Clears BGP session with all peer-group members                                                                                          |
| show ip bgp update-group                   | Displays automatically created „update groups“                                                                                          |
| show ip bgp peer summary                   | The printout is identical to a “show ip bgp summary” printout but displays only neighbors that are members of the specified peer-group. |

#### Configuration example

| IOS XR router bgp 123 bgp cluster-id 321 address-family ipv4 unicast network 1.0.0.0/21 redistribute ospf 1 route-policy setCOM ! neighbor-group IBGP\_peers remote-as 123 password encrypted 070C285F4D06 update-source Loopback10 address-family ipv4 unicast route-policy PASS in route-reflector-client route-policy PASS out ! ! neighbor 10.0.1.3 use neighbor-group IBGP\_peers ! ! neighbor 10.0.1.4 use neighbor-group IBGP\_peers | IOS-XE router bgp 1 neighbor RR-Client peer-group neighbor RR-Client remote-as 1 neighbor RR-Client update-source Loopback0 neighbor 172.16.100.1 peer-group RR-Client neighbor 172.16.100.2 peer-group RR-Client neighbor 172.16.100.10 peer-group RR-Client neighbor 172.16.100.20 peer-group RR-Client neighbor 172.16.100.30 peer-group RR-Client ! address-family ipv4 bgp scan-time 15 redistribute connected exit-address-family ! address-family vpnv4 bgp scan-time 15 neighbor RR-Client send-community extended neighbor RR-Client route-reflector-client neighbor 172.16.100.1 activate neighbor 172.16.100.2 activate neighbor 172.16.100.10 activate neighbor 172.16.100.20 activate neighbor 172.16.100.30 activate exit-address-family ! address-family ipv6 neighbor RR-Client send-community extended neighbor RR-Client route-reflector-client neighbor RR-Client send-label neighbor 172.16.100.1 activate neighbor 172.16.100.2 activate neighbor 172.16.100.10 activate neighbor 172.16.100.20 activate neighbor 172.16.100.30 activate | As you can see, the neighbors are associated with the peer group in the global configuration of the BGP process. This peer group configuration can be extended within each address family by configuring address family specific neighbor parameters - in this example the router reflector configuration. Each neighbor is activated within the relevant address family, and address-family-specific parameters are automatically applied to the neighbors because they are already associated with the peer group in the global configuration. |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

### BGP peer templates

Addresses the limitations of the peer-group

Peer templates are reusable and support inheritance

Peer template may to inherit a configuration from another peer template

BGP neighbor cannot be configured to work with both peer groups and peer templates

BGP peer routers using peer templates also benefit from automatic update group configuration. With the configuration of the BGP peer templates and the support of the BGP dynamic update peer groups feature, you no longer need to configure peer groups in BGP. You benefit from improved configuration flexibility and faster convergence.

Two types of peer templates are available:

**Peer session templates**: Used to group and apply the configuration of general session commands that are common to all address-family configuration modes

Example parameters that can be configured: description, disable-connected-check, ebgp-multihop, local-as, password, remote-as, shutdown, timers, translate-update, update-source and more..

**Peer policy template**: Used to group and apply the configuration of commands that are applied within specific address-family configuration modes

Example parameters that can be configured: advertisement-interval, allowas-in, as-override, capability, default-originate, distribute-list, filter-list, inherit peer-policy, maximum-prefix, next-hop-self, next-hop-unchanged, prefix-list, remove-private-as, route-map, route-reflector-client, send-community, send-label, soft-reconfiguration, unsuppress-map, weight and more..

Inheritance capability is an important component of peer-template operation. Inheritance in a peer template is similar to the node and tree structures that are commonly found in general computing—for example, file and directory trees. A peer template can directly or indirectly inherit a configuration from another peer template. The directly inherited peer template represents the tree in the structure, and the indirectly inherited peer template represents a node in the tree. Because each node also supports inheritance, branches can be created that apply the configurations of all indirectly inherited peer templates within a chain that traces back to the directly inherited peer template or the source of the tree. This structure eliminates the need to repeat configuration statements that are commonly reapplied to groups of neighbors. Common configuration statements can now be applied once and then indirectly inherited by peer templates that are applied to neighbor groups with common configurations.

BGP peer templates inheritance characteristics are as follows:

A session template can inherit configuration from another session template.

A policy template can inherit configuration from another policy template.

Neighbors can inherit from a session and a policy template.

Peer session templates support direct and indirect inheritance. A peer can be configured with only one peer session template at a time, and that peer session template can contain only one indirectly inherited peer session template.

Note If you attempt to configure more than one inherit statement with a single peer session template, an error message will be displayed.

This behavior allows a BGP neighbor to directly inherit only one session template and indirectly inherit up to seven additional peer session templates. This practice allows you to apply a maximum of eight peer session configurations to a neighbor: the configuration from the directly inherited peer session template and the configurations from up to seven indirectly inherited peer session templates. Inherited peer session configurations are evaluated first and applied starting with the last node in the branch and ending with the directly applied peer session template configuration at the source of the tree. The directly applied peer session template will have priority over inherited peer session template configurations. Any configuration statements that are duplicated in inherited peer session templates will be overwritten by the directly applied peer session template. That is, if a general session command is reapplied with a different value, the subsequent value will have priority and overwrite the previous value that was configured in the indirectly inherited template.

Peer policy templates are used to configure BGP policy commands that are configured for neighbors that belong to specific address families. As with peer session templates, peer policy templates are configured once and then applied to many neighbors through the direct application of a peer policy template or through inheritance from peer policy templates.

As with peer session templates, a peer policy template supports inheritance. However, there are minor differences. A directly applied peer policy template can directly or indirectly inherit configurations from up to seven peer policy templates. That is, a total of eight peer policy templates can be applied to a neighbor or neighbor group. Inherited peer policy templates are configured with sequence numbers like route maps. An inherited peer policy template, like a route map, is evaluated starting with the inherit statement with the lowest sequence number and ending with the highest sequence number.

The directly applied peer policy template and the inherit statement with the highest sequence number will always have priority and be applied last. Commands that are reapplied in subsequent peer templates will always overwrite the previous values.

| BGP Peer-Session Templates                                                      |                                                                                                                                                                                                                                                  |
| ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| (config-router)# template peer-session                                          | used to create a template for BGP peer sessions. This template includes parameters related to the establishment and maintenance of BGP sessions. Inherited under the routing process directly                                                    |
| (config-router-ptmp)# inherit peer-session                                      | Configures a peer session template to inherit the configuration of another peer session template                                                                                                                                                 |
| (config-router)# neighbor ip-address inherit peer-session session-template-name | Sends a peer session template to a neighbor so that the neighbor can inherit the configuration                                                                                                                                                   |
| show ip bgp template peer-policy \[policy-template-name]                        | Displays locally configured peer policy templates                                                                                                                                                                                                |
| BGP Peer-Policy Templates                                                       |                                                                                                                                                                                                                                                  |
| (config-router)# template peer-policy                                           | Enter policy-template configuration mode and creates a peer policy template Used to create a template for BGP peer policies. This template includes attributes and settings related to BGP policies. They are inherited under the address-family |
| (config-router-ptmp)# inherit peer-policy                                       | Configures the peer policy template to inherit the configuration of another peer policy template                                                                                                                                                 |
| (config-router)# neighbor ip-address inherit peer-policy                        | Sends a peer policy template to a neighbor so that the neighbor can inherit the configuration                                                                                                                                                    |
| show ip bgp template peer-policy \[policy-template-name]                        | Displays locally configured peer policy templates                                                                                                                                                                                                |

### BGP dynamic update groups

The BGP dynamic update groups feature separates peer group configuration from update group generation

Contains an algorithm that dynamically calculates and optimizes update groups of neighbors that share outbound policies and can share the update messages

BGP neighbor configuration is no longer restricted by outbound routing policies, and update groups can belong to different address families

Router optimizes BGP update message generation automatically - no configuration required

When a change to the configuration occurs, the router automatically recalculates update group memberships and applies the changes by triggering an outbound soft reset after a 3-minute timer expires.

This behavior is designed to provide the network operator with time to change the configuration if a mistake is made

| clear ip bgp update-group \[index-group \| ip-address] | Clears BGP update-group member sessions                                     |
| ------------------------------------------------------ | --------------------------------------------------------------------------- |
| debug ip bgp groups \[index-group \| ip-address]       | Displays information that is related to the processing of BGP update groups |
| show ip bgp replication \[index-group \| ip-address]   | Displays update replication statistics for BGP update groups                |

Example

![](<../.gitbook/assets/Unknown image (584)>)

## BGP route servers

BGP route server is a feature designed for internet exchange (IX) (also called Network access points (NAPs)) operators that provides an alternative to full eBGP mesh peering among the service providers who have a presence at the IX. The route server provides eBGP route reflection with customized policy support for each service provider.

That is, a route server context can override the normal BGP best path for a prefix with a different path based on a policy, or suppress all paths for a prefix and not advertise the prefix.

Instead of maintaining individual, direct eBGP peerings with every other provider, an SP maintains only a single connection to the route server operated by the IX

Peering with only the route server reduces the configuration complexity on each border router, reduces CPU and memory requirements on the border routers, and avoids most of the operational overhead incurred by individualized peering agreements.

The route server provides AS-path, MED, and nexthop transparency so that peering SPs at the IX still appear to be directly connected.

In reality, the IX route server mediates this peering, but that relationship is invisible outside of the IX.

![Ας 700 10.0.0.7 Ας 600 10. 0.8 10.005 Αξ 500 Ας 100 1ho. 1 ιο.ο.ο.4 Ας 400 Ας 200 10.0.0.2 10.0,0.3 Αξ 300](<../.gitbook/assets/Unknown image (585)>)

![50.0.08 AS 700 10,0.0.7 ло.о,о.е AS 600 AS 500 AS 100 AS 200 10,0.0.2 10, о.о.з AS зоо AS 400](<../.gitbook/assets/Unknown image (586)>)

## Customer connectivity to the Internet

#### When to use BGP

Use BGP between customer and ISP when:

Customers multi-homed to the same Service Provider

Primary/Backup link design

Load-balancing design

Customer that needs dynamic routing protocol with the Service Provider to detect failures

Use private AS number for these customers

Smaller ISPs that need to advertise their routes in the Internet

#### When to use static routing

Use static routing when:

Customers with a single connection to the Internet

Static routing is easier than maintaining BGP peering with ISP

To propagate static routes in a service provider network, complete these steps:

1. Identify all different service levels that are offered to customers and then all the different combinations of these service levels.
2. Assign each combination its own tag value and its own community.
3. Configure a route map that selects routes with each of the assigned tags and sets the corresponding community value. Because the processing of a route map stops when the match clause of a statement is met, each route should be assigned a single combination of communities only. Therefore, you must take great care to assign a tag and a community combination to each combination of services that is provided.
4. Configure static routes. Before you configure a static route for a specific customer, you must identify the combination of the services that is provided to this customer. Then you must look up the corresponding tag value. After you have configured the route, you must assign the tag.

The route map is applied during the redistribution of customer static routes into BGP at the PE router. Because the route map has no "permit any" statement at the end, the static routes that are not assigned any of the tags being used are not redistributed. The route map filters these routes out, forcing the network operators to make a tag assignment to all customer routes

A static route to the customer is configured and assigned the appropriate tag value of 1000, which represents the specified services that are assigned to the customer.

![](<../.gitbook/assets/Unknown image (587)>)

![](<../.gitbook/assets/Unknown image (588)>)

#### BGP backup with static routes

The floating static route has an AD set to 250. This value is higher than any routing protocol. When the PE2 router no longer receives any routing protocol information about the customer networks, the router will automatically install the floating static route and then redistribute it into BGP.

Based on BGP route selection rules, the redistributed floating static route will always remain the preferred path if an extra BGP configuration is not performed on the PE router. This preference means that regardless of whether the primary link comes back, the PE2 selects the locally sourced route as the best route. Therefore, the PE2 continues to announce a path toward the customer network. The backup link does not go back to the idle state.

Floating static routes do not work correctly with BGP. After a floating static route is inserted, it is never removed from the routing table even if the primary link comes back.

![](<../.gitbook/assets/Unknown image (589)>)

![](<../.gitbook/assets/Unknown image (590)>)

Whenever you use floating static routes in combination with redistribution into BGP, you will need to take extra configuration steps. You must ensure that the BGP route selection algorithm selects the primary route as the best BGP route when it reappears:

When a router redistributes a floating static route into BGP, the weight value that is assigned to the floating static route must be reduced (for example, set to 0). Otherwise the floating static route will always be selected as the best BGP route after the first failure of the primary link occurs.

The router must also assign local preference values to the floating static route so that the floating static route has a lower local preference than the primary route (for example, 50). This assignment ensures that the primary route is selected as the best BGP route on other routers within the provider network.

![](<../.gitbook/assets/Unknown image (591)>)

![](<../.gitbook/assets/Unknown image (592)>)

#### Load sharing with static routes

Outgoing load sharing can be performed easily on the customer IGP level, however the return traffic by default would take only one of the link from the service provider, since BGP prefers only one best path

![](<../.gitbook/assets/Unknown image (593)>)

You can achieve load balancing of the incoming traffic by advertising different parts of the customer network by different PE routers. The upper PE router could advertise half of the address space, and the lower PE router could advertise the other half. For backup reasons, the PE routers also should advertise the entire address space as a larger route summary.

In the example, the customer address space 209.165.201.0/24 is partitioned into two smaller blocks: 209.165.201.0/25 and 209.165.201.128/25.

{% hint style="info" %}
Load sharing in this way is not equal per link. It’s a statistical distribution based on which prefixes get used.
{% endhint %}

![](<../.gitbook/assets/Unknown image (594)>)

#### Connecting a single-homed customer to one ISP with BGP

A customer that is connected to only one ISP does not require a public AS number. In that case, a private AS number in the range from 64512 to 65535 is sufficient.

The customer is responsible for its own advertisements. Because customers are much less likely to be experienced in BGP configuration than the ISP, they are more likely to make errors. Therefore, the ISP must protect itself and the rest of the Internet from those errors.

The ISP should reject routes to networks that are not expected to be in the customer AS. Routes that contain an AS path with unexpected AS numbers also should be rejected

The two customer edge routers must also run IBGP between them to make common decisions regarding BGP routing information.

Each CE router has an EBGP session with the PE router on the other side of the link. Over that EBGP session, the ISP announces only a default route to the customer AS. When EBGP receives the default route, it installs the route in the routing table and redistributes it into the IGP, in this case OSPF, of the customer.

![](<../.gitbook/assets/Unknown image (595)>)

The CE router should announce the customer address space into BGP.

The CE router should advertise the customer space only if the customer network is reachable from the CE router (conditional advertising).

The CE router sFhould stop advertising the customer address space if the CE router loses connectivity with the customer core.

The reachability of the customer core network is tested using a static route. The static route should point to the core network next hop that is learned via IGP

| PE1(config)# router bgp 300 PE1(config-router)# neighbor 172.16.33.3 remote-as 65001 PE1(config-router)# neighbor 172.16.33.3 default-originate PE1(config-router)# neighbor 172.16.33.3 prefix-list DefaultOnly out PE1(config-router)# neighbor 172.16.33.3 prefix-list Customer1 in PE1(config-router)# neighbor 172.16.33.3 filer-list 15 in PE1(config-router)# neighbor 172.16.33.3 route-map AllCustomerIn in PE1(config-router)# exit PE1(config)# ip as-path access-list 15 permit ^65001(\_65001)\*$ PE1(config)# ip prefix-list DefaultOnly permit 0.0.0.0/0 PE1(config)# ip prefix-list Customer1 permit 209.165.201.0/24 le 32 PE1(config)# ip prefix-list Provider permit 209.165.0.0/16 le 32 PE1(config)# route-map AllCustomersIn permit 10 PE1(config-route-map)# match ip prefix-list Provider PE1(config-route-map)# set community no-export additive PE1(config-route-map)# exit PE1(config)# route-map AllCustomersIn permit 1000 | The route map is a general route map that is used for all customers. It uses the prefix list Provider to check every route that is received. If the route is within the big block of PA address space that the ISP announces to the rest of the Internet, the customer route is marked with the no-export community. This mark means that the route is used within the ISP AS only and is not sent to the rest of the Internet. |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Private AS numbers should not be advertised into the Internet.

Private AS numbers must be removed from the AS path before the customer BGP routes are advertised to other service providers

Use this command on the service provider egress routers. Before the service provider advertises any of the customer routes of the ISP to the rest of the Internet, the private AS numbers must be removed. The command removes those AS numbers if they are at the tail end of the AS path.

![](<../.gitbook/assets/Unknown image (596)>)

| (config-router-af)# neighbor remove-private-as | Private AS numbers are removed from the tail of the AS path before the update is sent. Private AS numbers followed by public AS numbers are not removed because the command's visibility is only on the last (tail end) AS number. The AS number of the sender is prepended to the AS path after this operation. |
| ---------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Incoming traffic to the customer is controlled by using either AS path prepending or the MED. Because the customer has multiple connections to the same AS, the MED is the best attribute to use. When the customer announces its routes to the ISP, a worse (or high) MED value on the backup link is set. Also, a good (or low) value on the primary link is set.

![](<../.gitbook/assets/Unknown image (597)>)

[Load Sharing](https://onenote/#Path%20Selection\&section-id={6EC9C333-3E04-41C1-8C01-0E8D85C128A8}\&page-id={9616F63C-7B78-43B3-B57B-1B79B0CBAAE8}\&object-id={44F1E4FF-160B-0597-2225-D7B107E3C02C}\&F\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE/L3.one)

**Outgoing traffic:**

Load sharing is identical to the static routing scenario. The IGP determines it.

**Incoming traffic:**

Announce portions of the customer address space to each upstream router

Configure BGP multipath support in the service provider network

Use EBGP multihop in environments where parallel links run between a pair of routers

#### Connecting a multihomed customer to multiple service providers

A customer that requires the maximum redundancy in its network design should implement a multihomed strategy that uses multiple service providers.

This configuration requires specific considerations to be implemented properly

Primary/Backup link

BGP is almost mandatory for multi-homed customers

The service provider advertises default route or full Internet routing table

Multihomed customers should use provider-independent (PI) address space

If the customer uses ISP-assigned small address blocks, then there is no purpose in using BGP to provide redundant connectivity. NAT is easier to implement and solves the problem of the reverse path.

Multi-homed customers have to use public AS numbers. (otherwise each ISP would assign the AS from their space, which may provide two different AS numbers, thus causes inconsistencies in routin information)

When the routes with the private AS numbers removed are propagated to the rest of the Internet, the AS path looks as though the routes were originated within the public AS of the ISP. All information about the private AS lying behind the public AS is lost. In the case of a multihomed customer, the customer routes are first propagated into each of the ASs of its ISPs. In the next step, the private AS number is removed from the routes because the routes are propagated to the rest of the Internet. Now the customer routes appear to be originating in the ASs of both ISPs. To an outside observer, there is now an AS path inconsistency because the same route appears to belong to different ASs.

Outgoing link selection:

You can use the same solution as with multihomed customers connected to one service provider.

A higher local preference for the default route comes from the primary service provider.

Incoming link selection:

You cannot use the MED because it can be sent only to the neighboring AS and no farther.

You must use other means such as BGP communities or AS path prepending to achieve incoming link selection.

Only the default route is required from both service providers

![](<../.gitbook/assets/Unknown image (598)>)

Load sharing for outgoing traffic:

You can use the same solution as with multihomed customers that are connected to one service provider.

A higher local preference for half of the routes comes from one service provider. A higher local preference for the other half of the routes comes from the other service provider.

Load sharing for incoming traffic:

The only load-sharing option that you can use in this setup is to separate the address space into two or more smaller address blocks.

The volume of traffic that will be directed to each part of the customer address space is difficult to predict. You should monitor the results of changing route updates by watching the load on the links before and after implementing the change. If the load distribution is not satisfactory, you can further modify the division of the address space. You must then check the load on the links again and further fine-tune the configuration.

Some part of the customer address space may be advertised by the customer network with a longer AS path over one of the links to fine-tune the load

![](<../.gitbook/assets/Unknown image (599)>)
