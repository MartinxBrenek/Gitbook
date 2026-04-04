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

# EIGRP

### Overview

Enhanced Interior Gateway Routing Protocol (EIGRP) is a successor of IGRP and also called as Enhanced Distance vector routing protocol with Link-state properties - hybrid routing protocol

EIGRP makes decisions based on metric information it receives from neighbors. The metric is calculated based on the path properties such as bandwidth, delay, reliability, load and MTU

EIGRP use DUAL algorithm to calculate the best path to the destination and advertise only the best route to his neighbors

Does not send full routing table in a periodic fashion, and instead sends updates only when there is a change in the network, with improved rapid convergence

It maintains topology and neighbor table, which are link-state properties

### Tables

#### Neighbor table

List of directly connected EIGRP routers (neighbors)

![](<../.gitbook/assets/Unknown image (746)>)

#### Topology table

Topology table containing list of paths learned from neighbors - similar to link-state protocols

each network prefix learned from neighbors, with it's status (Active/Passive); Path attributes (hop count, minimum path bandwidth, and total path delay); Next hop EIGRP neighbors that advertised prefix and their metrics (RD)

#### Active routes

the cost is still being computed (by querying)/convergence in progress. Active timer 3min (for neighbor to reply back)

EIGRP router indicate that a path computation is required for a specific route by sending a query packet with the delay set to infinity

#### Passive routes

DUAL algorithm has converged

![](<../.gitbook/assets/Unknown image (747)>)

### Packet types

#### Hello

used to discover and maintainneighbors. Multicasted every 5sec to 224.0.0.10 (IPv4) or FF02::A (IPv6) over IP protocol number 88 in the header

Hold-time 15sec by default (tells neighbor how often to expect hellos). Doesn't have to match as in OSPF. Unicast is used with static neighbors. AS number must match

After peering with neighbors is established, the packets will be unicast between them

#### Acknowledgement (ACK)

Acknowledgement (ACK) is a hello packet with no data that confirms that the destination device received the packet (unicast hello)

#### Request

used to get specific info from one or more neighbors

#### Update

used to send topology updates, they contain prefix, prefix-length, k-values, MTU and hop count. Not set at defined intervals. Always require ACK for each update packet

EIGRP neighbors exchange the entire routing table when forming an adjacency, and they advertise only incremental updates as topology changes occur within a network

Partial (only changed routing info) Bounded (routers that need routing updates receive them)

Reliable Transport Protocol (RTP) is used to transport EIGRP update messages with requiring explicit acknowledgment

UPDATE packets are sent as unicast to newly discovered neighbors. UPDATE packets are sent as multicast when a link or metric changes

#### Query

used by a router to advertise that a route is in the ACTIVE state and to find an alternate path to a particular destination when it has lost the primary path to the destination. Requires ACK. If no answer(s) is received by the neighbor(s) the packet will be resent as unicast to the nonresponsive neighbor(s)

If there’s no reply after 16 attempts, the neighbor relationship will be reset

#### Reply

used to respond to query when it has loop-free route.

#### Goodbye message

The router on which the EIGRP process is shutdown (Graceful shutdown) will broadcast a GOODBYE message (= HELLO packet with all K-values set to 255)

This allows neighbors to quickly converge because they won’t wait until the holdtime to the particular router expires

![](<../.gitbook/assets/Unknown image (748)>)

### Diffusing Update Algorithm (DUAL)

All routes to a destination network are stored in the topology table, but only the best route (when load-balancing is NOT configured) with the lowest FD is stored in the RIB

#### Computed distance (CD)

locally calculated metric value (Metric to neighbor + Reported Distance)

#### Feasible Distance (FD)

metric value for the lowest-metric path to reach a destination

#### Reported Distance (RD)

distance reported by a router (neighbor) to reach a prefix

#### Successor route

route with the lowest path metric to a destination

#### Successor

first next-hop router for the successor route

#### Feasible Successor (FS)

provides a pre-computed, loop-free route, if successor route fails. FC has to be met

#### Feasibility condition (FC)

backup route is installed, only if the RD from neighbor is lower than FD (RD < FD)

"If the backup router (FS) is closer to the destination than myself"

#### Loop prevention with FC

In case link to L fails, the A router knows that feasible successor router C have less cost to reach the S and won't create a loop, which may occur, if C has higher cost on direct link to the S than going back via A and through the middle

![](<../.gitbook/assets/Unknown image (749)>)

Successor .3 and FS .4 is installed as it is closer to the destination that S1 is (CD / RD)

CD of FS .4 was increased intentionally with delay on the path between S1 and FS .4

![](<../.gitbook/assets/Unknown image (750)>)

Anyway, only Successor .3 is installed to global RIB for the best metric (Metric is scaled down in RIB (metric/128))

![](<../.gitbook/assets/Unknown image (751)>)

Another example shows that FC is not met as RD = FD and there is strict less than comparison

| all-links | displays all available paths, however in general topology output it displays only Successor ans FS. Here FS is not listed, as FC hasn't been met |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |

![](<../.gitbook/assets/Unknown image (752)>)

![](<../.gitbook/assets/Unknown image (753)>)

When the FD is tagged as Inaccessible in the EIGRP topology table, the router is not using that EIGRP route in its routing table.

Usually, the route is overridden by another routing protocol that has lower administrative distance.

![](<../.gitbook/assets/Unknown image (754)>)

### Redistribution loop prevention

EIGRP already differentiates between routes learned from within the autonomous system and routes learned from outside the autonomous system by assigning different administrative distance:

• Internal EIGRP: 90

• External EIGRP: 170

Remember that the seed metric is, by default, set to infinity (unreachable). Therefore, if you fail to manually set the metric using any of the options listed earlier in the chapter, routes will not be advertised to the other routers in the EIGRP autonomous system

### Commands

| show eigrp address-family ipv4 \[neighbors\| interfaces \| topology ]                                                                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                        |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show \[ip \| ipv6] eigrp \[topology \| neighbors \| interfaces] show \[ip \| ipv6] eigrp topology all-links                                                                                                                                                                                                                       | to view tags aswell The “Q count” must be 0 when an adjacency is properly formed because >0 means that there are packets enqueued which are not acknowledged by the neighbor                                                                                           |
| sh ip prot \| s eigrp                                                                                                                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                        |
| sh ip route eigrp                                                                                                                                                                                                                                                                                                                 |                                                                                                                                                                                                                                                                        |
| Base Config                                                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                                                                        |
| router eigrp router-id <>                                                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                                        |
| network 0.0.0.0                                                                                                                                                                                                                                                                                                                   | enables EIGPR on all interfaces                                                                                                                                                                                                                                        |
| network <> <>                                                                                                                                                                                                                                                                                                                     | enabled EIGRP for interfaces/network that if within the network statement's range                                                                                                                                                                                      |
| (config-router)# neighbor \<ip \| ipv6>                                                                                                                                                                                                                                                                                           | When multicast can’t be used, neighbors can be specified manually to be established with unicast                                                                                                                                                                       |
| redistribute \[bgp\|rip\|ospf\|static\|connected] \<PID/AS> metric route-map \<map\_name>                                                                                                                                                                                                                                         | metric values have to be specified. Redistributed routes have AD of External EIGRP which is 170 To advertise default route you can either use #redistribute the static or use Summary Address under interface level config #ip summary-address eigrp 1 0.0.0.0 0.0.0.0 |
| Authentication between EIGRP peers R9(config)# key chain KC\_CCNP R9(config-keychain)# key 1 R9(config-keychain-key)# key-string CCNP R9(config-keychain-key)# cryptographic-algorithm md5 R9(config)# int g1/1 R9(config-if)# ip authentication key-chain eigrp 100 KC\_CCNP R9(config-if)# ip authentication mode eigrp 100 md5 | Only the key hash will be exchanged and compared against each other, not the key itself                                                                                                                                                                                |
| show key chain                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                        |

#### Annual key rotation

![](<../.gitbook/assets/Unknown image (755)>)

### EIGRP Named Mode

provides modular configuration of all options and features within the routing process, improving the shortcoings of classic mode that configuration was distributed in routing process and on the interface. Automatically use wide metrics. When configuring an IPv6 AS, EIGRP is automatically enabled on all IPv6-configured interfaces

| router eigrp                                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| address-family \[ipv4 ipv6] unicast autonomous-system <>                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| \[eigrp] router-id <>                                                                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| network <> <>                                                                                                                  | defines the specific network to be injected to eigrp and form a neighborship accordingly                                                                                                                                                                                                                                                                                                                                            |
| topology base                                                                                                                  | Address-family Topology Configuration Mode This mode provides several configuration options which operate on the EIGRP topology table Things like redistribution, distance, offset list, variance and so on can be configured under this mode                                                                                                                                                                                       |
| af-interface <>                                                                                                                | Address-family Interface Configuration Mode This mode takes all the interface specific commands that were previously configured on an actual interface (logical or physical). EIGRP authentication, split-horizon, and summary-address configuration are some of the options that are now configured here instead of on the actual interface it does not enable the eigrp on that interface, the network command must be still used |
| address-family ipv6 unicast autonomous-system MY-EIGRP-AS-NUMBER                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| router-id 2001:db8::1                                                                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| (config-router-af-interface)# summary-address 0.0.0.0/0                                                                        | this will advertise default route to neighbors and creates summarry static pointing to the null0 interface on the originating router                                                                                                                                                                                                                                                                                                |
| Configuring LFA FRRs per Prefix                                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| address-family ipv4 autonomous-system <>                                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| topology base                                                                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| fast-reroute per-prefix {all \| route-map route-map-name}                                                                      | show ip eigrp topology frr                                                                                                                                                                                                                                                                                                                                                                                                          |
| fast-reroute load-sharing disable                                                                                              | disables load sharing in case primary path is an ECMP with multiple LFAs                                                                                                                                                                                                                                                                                                                                                            |
| fast-reroute tie-break {interface-disjoint \| linecard-disjoint \| lowest-backup-path-metric \| srlg-disjoint} priority-number | enable tie-breaking rules The lower the assigned priority value the higher the priority of the tie-breaking attribute                                                                                                                                                                                                                                                                                                               |

### EIGRPv6

EIGRP module for IPv6

Similarities with EIGRP for IPv4

Same protocol number 88

Router ID stays 32 bits (must be configured explicitly if there is no IPv4 interface on the router)

Uses MD5 like for IPv4 (IPsec authentication will be available soon)

Same metrics and DUAL algorithm used for path selection

Incremental updates

EIGRPv6 is activated on a per-interface basis (network command) with link-local IPv6 address configured in order to form an adjacency

Hellos are sourced from the link-local address and destined to FF02::A (all EIGRP routers)

Automatic summarization is disabled by default for IPv6 (unlike IPv4)

The IPv6 EIGRP process must be enabled explicitly (“no shutdown” under the process)

EIGRPv6 has its own unique configuration commands and syntax that are different from those used in EIGRP named mode for IPv4 and IPv6.

If there’s no IPv4 address on any interface configured, the EIGRPv6 router-id must be set explicitly

| (config-if)# ipv6 eigrp                                             | Configures EIGRP for IPv6 on an interface |
| ------------------------------------------------------------------- | ----------------------------------------- |
| (config-if)# ipv6 summary-address eigrp as-number prefix/mask \[AD] | Configures summarization on an interface  |
| no ipv6 split-horizon eigrp as-number                               | Disables split horizon on an interface    |
| ipv6 bandwidth-percent eigrp as-number percent                      | percentage                                |
| show ipv6 eigrp \[topology\|neighbors]                              |                                           |
| show ipv6 route eigrp                                               |                                           |

### Stuck in Active (SIA)

Happens when a router with a route in ACTIVE state sends a QUERY but doesn’t get a REPLY from an adjacent neighbor within a certain amount of time

When EIGRP loses a route and there’s no feasible successor the route will go from PASSIVE (converged) state to ACTIVE (not converged) state

If this happens, EIGRP will send out QUERIES on all interfaces except from the interface of the successor route asking for an alternate path for that route

When a route goes into ACTIVE state the active timer will be started (180 seconds by default) 90 seconds after a “normal” QUERY packet was sent, a SIA-QUERY packet will be sent. All neighbors must then respond with a SIA-REPLY packet and any router that fails before the active timer expires will be considered SIA

When the ACTIVE timer expires, the router will drop the neighbor relationship with the router who didn’t send a SIA-Reply and delete all routes from the neighbor

If a SIA-Reply is received, the neighbor relationship will not be terminated

In later versions, if R1 would sent query to downstream routers and reply packet on R4 dropped, then the R2 doesn't have what to forward to R1 and then R1 went to stuck in active state and flushed relationship also with R2 and all the below routers.

New SIA request is sent to fix, if reply packet is dropped somewhere in the path; also with lower Active timer of 90sec.

![](<../.gitbook/assets/Unknown image (756)>)

![](<../.gitbook/assets/Unknown image (757)>)

The output below tells you that EIGRP has been active for 10.1.2.0/24 for 38 seconds, has queried two neighbors, and is still waiting on a reply from 10.1.4.3.

The lowercase **r** indicates that the router is waiting for a reply to a query. A capital **R** indicates that it received a reply from this neighbor

#### Causes of SIA

Neighbor's memory or CPU limitations

Missing or incorrect bandwidth interface configuration parameter

Incorrect bandwidth configured to influence path selection

If the query range is the problem, do not increase the SIA timer; instead, reduce the query range

![](<../.gitbook/assets/Unknown image (758)>)

### Route Summarization

reduces Query scope, so when S2 loses connection to one if it's /26 subnet it won't query all routers (1st picture), that would cause high congestion for link and router CPU's.

With summarization of all /26 as one /24 will prevent this as the first hop router would respond that it doesn't have entry for /26 as it know only the /24 and won't propagate the query further. Auto Summarization (#auto-summary) is disabled by default since IOS v15, legacy feature; auto summarizes subnets as classful boundaries; configured under eigrp

![](<../.gitbook/assets/Unknown image (759)>)

![](<../.gitbook/assets/Unknown image (760)>)

| Router(config-if)# \[ip \| ipv6] summary-address eigrp / leak-map \[NAME] | ## Configuring EIGRP summarization (classic mode)    |
| ------------------------------------------------------------------------- | ---------------------------------------------------- |
| Router(config-router)# summary-metric / distance                          | ## Configuring a EIGRP summary metric (classic mode) |

### Stub Router

is a non-transit router that does not connect to any other router (also known as “end-of-the-line” router). Queries are not sent from non-stub routers to stub routers

By default, EIGRP stub routers only advertise connected and summary routes

| eigrp stub \[option] leak-map \[ROUTE-MAP-NAME] | connected stub router advertises connected routes matched with a network command summary stub router advertises summarized routes static stub router advertises statically configured routes, if the redistribute static command has been configured leak-map is used to apply a route-map that filters the routes that can be leaked from one EIGRP domain to another redistributed stub router advertises any redistributed routes receive-only stub router does not advertise any routes |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

![](<../.gitbook/assets/Unknown image (761)>)

### Metric calculation

Link variables Bandwidth, Delay, Reliability, Load and MTU are used for metric calculation

EIGRP uses a reference bandwidth of 10 Gbps with the default metrics. Bandwidth (BW) represents the slowest link in the path scaled to a 10 Gbps link (10^7)

BW is collected from configured BW on a link. The lower the bandwidth the greater the metric. Measured in Kbps

Delay is the cumulative (summary) of all interface delay along the path, measured in tens of microseconds (μs)

Load, Reliability and MTU are set to 0 by default. BW and Total delay to 1

#### Best practices

Never use the bandwidth parameter adjustment on interface to influence EIGRP path selection, as it also uses this bandwidth setting for EIGRP packet pacing

To influence EIGRP path selection on an interface basis, use the delay interface configuration parameter.

You should always ensure that the bandwidth parameter is set to the actual available bandwidth for the interface or subinterface

Mismatched K values can prevent neighbor relationships from being established and can negatively impact network convergence

![](<../.gitbook/assets/Unknown image (762)>)

#### Wide metric

Wide metric that addresses the issue of scalability with higher-capacity interfaces and scales with interface bandwidth >1G. Bandwidth is now called Throughput and Delay is now called Latency, Wide metrics are based on 64-bit calculations whereas classic metrics are based on 32-bit calculations

EIGRP automatically detects whether their neighbors also support wide metrics, uses the appropriate metric type (wide metrics are preferred) and switch back to classic metric on a per-link basis if the neighbor doesn’t support wide metrics

Instead of putting the CD as metric into the routing table (RIB), a scaled metric is now calculated

This is done because the RIB can only handle 32-bit integer values whereas the calculated wide metric values are 64-bit

The default RIB scale factor is 128 but can be modified if needed

Calculation: (CD/128) = RIB metric

![](<../.gitbook/assets/Unknown image (763)>)

![](<../.gitbook/assets/Unknown image (764)>)

### Load Balancing

EIGRP has the ability to load balance traffic equally or unequally

Equal Cost only Feasible Successor routes are considered for load balancing. By default, 4 paths will be installed and can be modified up to 32 paths

| maximum-paths | to enable specific number of paths to be installed for Load balancing |
| ------------- | --------------------------------------------------------------------- |

Unequal-Cost Load Balancing (UCMP) the load will be shared based on the ratio of the variance (disabled by default). If enabled, the Variance Keyword will be used for route consideration

Keyword Variance enables EIGRP to have multiple successor routes installed into it's RIB

Router's FD is multiplied by configured Variance number (default is 1), and if the Feasible successor's FD is within multiplied range, it is installed as a successor and load balance traffic across both successor paths according to link properties

FD for 256kbps link is within the range that we've got by multiplying the successor's FD of 512kbps link by configured variance of 2

FD of feasible successor < FD of successor \* multiplier

![](<../.gitbook/assets/Unknown image (765)>)

### Loop prevention

EIGRP advertise a route as unreachable when:

Query for that route has been sent

Two routers are in startup mode (they exchange topology tables for the first time)

For each table entry a router receives during startup mode, it advertises the same entry back to it's new neighbor with a maximum metric (poison route)

Topology table change is advertised when a route becomes unreachable, the router marks the route as unreachable in its topology table and updates the metric to infinity (infinite metric). Then, it advertises it to its neighboring routers with the poison reverse mechanism, indicating that the route is no longer reachable

### Site of Origin (SoO)

Assigns each EIGRP route an extended community tag so that the origin site can be identified

Useful for loop prevention, count-to-infinity and suboptimal routing on dual/multihomed sites

Prevents advertised EIGRP prefixes from getting advertised back by the PEs to the originating site

The configured SoO applies to all routes originated by the CEs (IPv4 and IPv6)

Configuration of SoO must be done on the PE routers towards the CE routers!

| Router(config)# route-map \[NAME] Router(config-route-map)# set extcommunity soo |                                                                                    |
| -------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Router(config-if)# ip vrf sitemap \[ROUTE-MAP-NAME]                              | ## Applying the route map on an interface on the PE routers towards the CE routers |

### Leak-map with Summary routes

EIGRP summarization automatically suppresses the advertisement of the individual summarized prefixes

This behavior can be influenced by using leak-maps (= route-maps) (comparable to BGP unsuppress-maps)

Example: Router A summarizes the subnets 10.0.0.0/24, 10.0.1.0/24, 10.0.2.0/24 and 10.0.3.0/24 to 10.0.0.0/22 but additionally wants to send out the more specific route 10.0.2.0/24. This can be achieved using a summary-address together with a leak-map.

| (config-if)# \[ip \| ipv6] summary-address eigrp / leak-map \[ROUTE-MAP-NAME] | ## Configuring EIGRP summarization with leak map (classic mode) |
| ----------------------------------------------------------------------------- | --------------------------------------------------------------- |
| (config-router-af-interface)# summary-address / leak-map \[ROUTE-MAP-NAME]    | ## Configuring EIGRP summarization with a leak map (named mode) |

![](<../.gitbook/assets/Unknown image (766)>)

match interface tells to which interfaces we are going to advertise specified leak map

### Fast Reroute (FRR) e.g Loop-Free Alternate (LFA)

feature provides a mechanism to quickly switch to precomputed backup routes, in case of primary route failures (esentially Feasible successor must be in place)

The backup route is installed in the FIB, which reduces the routing transition time to less than 50 ms. Only available in named mode

#### LFA Tie-Breaking Rules

in case there are multiple candidate LFAs for a given primary path

Interface-disjoint eliminates LFAs that share the outgoing interface with the protected path

Linecard-disjoint eliminates LFAs that share the line card with the protected path

Lowest-repair-path-metric eliminates LFAs whose metric to the protected prefix is high

Multiple LFAs with the same lowest path metric may remain in the routing table after this tie-breaker is applied

Shared Risk Link Group (SRLG) disjoint eliminates LFAs that belong to any of the protected path SRLGs. SRLGs refer to situations where links in a network share a common fiber (or a common physical attribute). If one link fails, other links in the group may also fail. Therefore, links in a group share risks.

### Add Path Support in EIGRP

Needed for DMVPN when multiple spokes advertise the same subnet (Only available in EIGRP named mode)

The DMVPN domain is in turn connected to two service providers—Service-Provider 1 and Service-Provider 2. Four spoke devices in this DMVPN domain—Spoke-1, Spoke-2, Spoke-3, and Spoke-4. Spoke-1 and Spoke-3 are connected to Service-Provider 1, and Spoke-2 and Spoke-4 are connected to Service-Provider 2. The Enhanced Interior Gateway Routing Protocol (EIGRP) is the routing protocol between the hubs and the spokes over the tunnel interfaces.

Spoke-1 and Spoke-2 are connected to a LAN with the network address 192.168.1.0/24. Both these spokes are connected to both the hubs through two different service providers, and hence, these spokes advertise the same LAN network to both hubs. Typically, spokes on the same LAN advertise the same metric; therefore, based on the metric, Hub-1 and Hub-2 have dual Equal-Cost Multipath (ECMP) routes to reach network 192.168.1.0/24. However, because EIGRP is a distance vector protocol, it advertises only one best path to the destination. Therefore, in this EIGRP-DMVPN domain, the hubs advertise only one route (for example, through Spoke-1) to reach network 192.168.1.0/24. When clients in subnet 192.168.2.0/24 communicate with clients in subnet 192.168.1.0/24, all traffic is directed to Spoke-1. Because of this default EIGRP behavior, there is no load balancing on Spoke-3 and Spoke-4. Additionally, if Spoke-1 fails or if the network of Service-Provider 1 goes down, EIGRP must reconverge to provide connectivity to 192.168.1.0/24.

The Add Path Support in EIGRP feature enables EIGRP to advertise up to four additional paths to connected spokes in a single DMVPN domain. If you configure this feature in the example topology discussed above, both Spoke-1 and Spoke-2 will be advertised to Spoke-3 and Spoke-4 as best paths to network 192.168.1.0, thereby allowing load balancing among all spokes in this DMVPN domain.

![](<../.gitbook/assets/Unknown image (767)>)

| Router(config)# router eigrp \[NAME] Router(config-router)# address-family \[ipv4 \| ipv6] autonomous-system Router(config-router-af)# af-interface \[ \| default] Router(config-router-af-interface)# no next-hop-self no-emcp-mode Router(config-router-af-interface)# add-path <1-4> |   |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |
