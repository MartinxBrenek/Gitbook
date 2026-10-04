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

# VRF

### Virtual Routing and Forwarding (VRF)

**VRF** is a routing virtualization construct that allows multiple independent L3 forwarding instances on a single physical device. Each VRF maintains its own logical routing context with separate RIB and FIB, independent routing protocol instances, and independent policy, which enables true L3 segmentation and overlapping IP address space. Conceptually, each VRF behaves like a virtual router sharing the same forwarding hardware.

In MPLS L3VPN and related BGP-based VPN technologies, the Route Distinguisher (RD) is a 64-bit value prepended to an IPv4 prefix to create a globally unique VPNv4 address propagated in NLRI. The resulting address is 96 bits, consisting of RD plus the 32-bit IPv4 prefix. The sole function of the RD is to disambiguate identical prefixes originating from different VRFs or different PEs so that BGP can carry them simultaneously. RD does not encode policy, topology, or VPN membership.

**VRF Global** refers to the Global routing table (GRT). A customer VRF (or an interface assigned to a VRF) refers to the service VPN.

![](<../.gitbook/assets/Unknown image (1137)>)

### Route Distinguisher (RD)

#### RD formats (RFC 4364)

Type Field (2) + Administrator Subfiled (2) + Assigned number subfiled (4) = 0 AS #id e.g. 65212:123456

Type Field (2) + Administrator Subfiled (4) + Assigned number subfiled (2) = 1 IPv4 #id e.g. 12.13.14.15:12345

Type Field (2) + Administrator Subfiled (4) + Assigned number subfiled (2) = 2 AS4 #id e.g. 128216:12345

Best practice is to use a unique RD per VRF per PE.

In other words:

Same PE with multiple VRFs → different RD per VRF

RD identifies the route origin, not the VPN membership

Route Targets control import/export policy. The RD’s role is only to make routes unique. Using a per-PE RD ensures that identical prefixes from different PEs are always distinct in VPNv4/VPNv6 BGP.

RD format is always: `<administrator>:<assigned-number>`

or

In practice you’ll see it as: `<as-number>:<nn>` or `<ipv4-address>:<nn>` (depending on the RD type).

{% hint style="info" %}
In lab scenarios (also shown in my notes) you may encounter that the RD for a specific VRF is same on all PE routers participating on the VRF. This has the effect that routers will see the same RD from multiple PEs. This is not an issue. In real deployments, use a unique RD per VRF per PE.
{% endhint %}

### Route Target (RT)

Route Targets (RT) define the import and export policies for VRF, determining which routes are allowed to enter or leave a specific VRF

VRFs with shared RTs can exchange routes, as these RTs are imported/exported in their respective configurations.

![](<../.gitbook/assets/Unknown image (1138)>)

Data structures associated with VRF

VRF IP routing table contains prefixes which should be accessible for appropriate a group of sites

List of interfaces VRF interfaces (physical interface, sub-interface, logical interface) are assigned to VRFs

More interfaces per VRF

Each interface can be assigned only to one VRF

Derived Cisco Express Forwarding (CEF) table

Group of rules and routing protocols (routing protocol contexts) determining what will be placed into the forwarding table

Other information assigned to VRF

Route distinguisher

Group of import and export route targets

| Router(config)# ip vrf myvrf Router(config-vrf)# OR Router(config)# vrf definition myvrf Router(config-vrf)#address-family ipv4 unicast | There are two main methods to configure VRF each using a slightly different syntax Both commands are used to achieve the same goal of creating a VRF vrf definition is the recommended since it allows for more modular approach of configuration define address family that is intended to be deployed, or error message will appear when you will configure IPv4 on an interface vrf name is case-sensitive |
| --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-if)# vrf forwarding OR (config-if)# ip vrf forwarding                                                                           | The command `ip vrf` is used to create a single-protocol IPv4-only VRF, and `vrf definition` creates a multiprotocol VRF. The command `vrf upgrade-cli multi-af-mode common-policies` automatically converts IPv4-only VRF commands into multiprotocol VRF commands without service impact.                                                                                                                   |
| (config)# vrf upgrade-cli multi-af-mode {common-policies \| non-common-policies} \[vrf ]                                                | This command forces migration from older ip vrf version to multi-AF vrf definition version non-common-policies will autmatically sort the existing IPv4 configuration under ipv4 AFI, the other will leave it in the main config                                                                                                                                                                              |

{% hint style="info" %}
Sometimes `ip vrf forwarding <vrf-name>` won’t apply cleanly on an SVI/interface. If that happens, try `vrf forwarding <vrf-name>` instead.
{% endhint %}

### VRF Lite

**VRF Lite** is a non-MPLS deployment of VRF used when VRFs are required on an internal router that does not participate in VPN services offered by MPLS.

It doesn't support label exchange, LDP adjacency or labeled packets, also no RD/RT are configured in VRF-lite implementation

### AFI / SAFI

**AFI (Address Family Identifier)** is a numerical value used to define and differentiate the type of address family being carried or processed in networking protocols like MP-BGP or OSPFv3.

Separate routing databases and configurations are maintained for different address families. Examples include IPv4 (AFI = 1), IPv6 (AFI = 2), and address families supporting unicast and multicast routes or advanced VPN implementations like Layer-2 and Layer-3 VPNs (e.g., EVPN and MPLS VPN).

Subsequent Address Family Identifier (SAFI) complements the AFI by specifying the type of routing information within the address family, such as unicast (SAFI = 1) or multicast (SAFI = 2).

This distinction enables support for advanced networking architectures, including multipoint tunneling or multicast VPNs.

Together, AFI and SAFI provide a modular and scalable identification mechanism, allowing the configuration and management of multiple address families and network protocols within a single routing protocol instance or VRF

| Router(config-router)# address-family \[ipv4\|ipv6] \[unicast \| multicast]                 | Configures standard IPv4 or IPv6 routing (either unicast or multicast)                                                                                                                                                           |
| ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Router(config-router)# address-family \[vpnv4\|vpnv6] \[unicast \| multicast]               | Configures routing for VPNv4 or VPNv6, which is used in MPLS or VPN configurations                                                                                                                                               |
| Router(config-router)# address-family \[ipv4\|ipv6] \[unicast \| multicast] vrf \[vrf-name] | Configures BGP for a specific VRF, isolating routing information per customer or virtual network context (config-bgp)# vrf // IOS-XR called routing context - only routing protocols that support VPN routing context allow this |

After entering the desired AFI, it will navigate you to the sub config level (`config-router-af`), where you can configure all routing protocol parameters as in global configuration mode (for example `network`, `neighbor`, `redistribute`, etc.).

{% hint style="info" %}
Neighbors must be activated per AFI/SAFI using `neighbor x.x.x.x activate`.
{% endhint %}

### VRF leaking (MPLS)

Used in complex or simple MPLS VPN use cases explained in MPLS section.

Complex VPNs - memberships and relationships between different customers/VRF's

To achieve route leaking between VRFs, you must use the Route Target Import Export functionality

RDs make customer routes unique so the provider can store and process them appropriately. However, RDs do not control which routes are exchanged between VRFs; that is the role of **Route Targets (RTs)**, which are an additional BGP attribute. Extended BGP community attributes encode these as 64-bit values (as opposed to normal 32-bit communities).

Extended communities carry the meaning of attribute together with their value. Any number of RT can be attached to a single route.

**Export route targets (RTs)** define VPN membership by tagging customer routes when they are converted into VPNv4 routes

**Import route targets** match export RTs, enabling the import of the corresponding routes into the appropriate VPN

Example Scenario:

On PE routers are two VRFs for each customer (A and B) - CE-1-A and CE-1-B for site 1 CE-2-A and CE-1-B for site 2 and CE-3-A and CE-3-B:

ip vrf A

rd 1:100

route-target export 1:100

route-target import 1:100

ip vrf B

rd 1:200

route-target export 1:200

route-target import 1:200

This will ensure that each customer will receive routes from their sites - by MPLS L3 VPN and the respective import export

If we want to allow communication between customer A (site 1 CE-1-A) and customer B (site 2 CE-2-B), we would have to define another route-target that would be used to import and export routes for these two sites. This will have to be configured on the PE-001-IOS:

ip vrf A

rd 1:100

route-target export 1:100

route-target export 1:150

route-target import 1:100

route-target import 1:150

and on PE-002-IOS:

ip vrf B

rd 1:200

route-target export 1:200

route-target export 1:150

route-target import 1:200

route-target import 1:150

A VRF can have multiple route targets (RTs) configured. By adding common RTs to multiple VRFs, you can selectively allow route sharing between specific customer sites or VRFs.

Each route target acts as a “tag” for routes, allowing granular control over route propagation

{% hint style="info" %}
If you simply import `1:100` into VRF `B` and `1:200` into VRF `A`, you will also pull in routes for other sites that use those same RTs.
{% endhint %}

![](<../.gitbook/assets/Unknown image (1139)>)

IOS-XE Configuration

| vrf definition vpn1                 | Creates a VRF instance named vpn1                                                                                                         |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| rd 100:1                            | Specifies the RD value for VRF vpn1 as 100:1                                                                                              |
| route-target export 100:1           | Marks VRF routes for sharing with the Route Target value 100:1                                                                            |
| route-target import 100:1           | Allows VRF to accept routes with the Route Target value 100:1 or we can use route-target both [xx:xx](xx:xx) to include import and export |
| route-target import 200:1           | Allows VRF to accept routes with the Route Target value 200:1 or we can use route-target both [xx:xx](xx:xx) to include import and export |
| !                                   |                                                                                                                                           |
| vrf definition vpn2                 |                                                                                                                                           |
| rd 200:1                            |                                                                                                                                           |
| route-target export 200:1           |                                                                                                                                           |
| route-target import 200:1           |                                                                                                                                           |
| route-target import 100:1           |                                                                                                                                           |
| !                                   |                                                                                                                                           |
| interface Gi1                       |                                                                                                                                           |
| ip vrf forwarding vpn1              | Associate interface `Gi1` with VRF `vpn1`.                                                                                                |
| ip address 10.1.2.5 255.255.255.252 |                                                                                                                                           |
| !                                   |                                                                                                                                           |
| interface Gi2                       |                                                                                                                                           |
| ip vrf forwarding vpn2              |                                                                                                                                           |
| ip address 10.0.2.1 255.255.255.0   |                                                                                                                                           |
| router bgp 65000                    |                                                                                                                                           |
| !                                   |                                                                                                                                           |
| address-family ipv4 vrf vpn2        | this is referred to as VRF-aware routing                                                                                                  |
| redistribute connected              |                                                                                                                                           |
| !                                   |                                                                                                                                           |
| address-family ipv4 vrf vpn1        |                                                                                                                                           |
| redistribute connected              |                                                                                                                                           |

{% hint style="warning" %}
Adding or removing `ip vrf forwarding <vrf-name>` on an interface strips the interface IP address. Set the VRF first, then reapply the IP address.
{% endhint %}

### Monitoring VRFs

| sh ip vrf              | Shows a list of every VRFs configured on the router                      |
| ---------------------- | ------------------------------------------------------------------------ |
| sh vrf all             | Shows a list of every VRFs configured on the router                      |
| show ip vrf detail     | Shows the detailed VRF configuration                                     |
| show ip vrf interfaces | Shows interfaces assigned to the VRFs. sh ip vrf all interface brief /XR |

IOS-XR Configuration

| VRF configuration vrf A rd 1:100 address-family ipv4 unicast import route-target 1:100 1:400 ! export route-target 1:100 | Configuring BGP for VRF router bgp 1 address-family ipv4 unicast ! vrf A address-family ipv4 unicast redistribute connected ! neighbor 10.30.1.2 remote-as 300 address-family ipv4 unicast route-policy PASS in route-policy PASS out |
| ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RP/0/RP0/CPU0:router(config-if)# vrf vrf-name                                                                            | Assigns an interface to the VRF                                                                                                                                                                                                       |

### Selective VRF import/export

#### Import map

Controls which routes are imported into a VRF by filtering or modifying attributes such as RTs

Selective route import uses a route map that can filter the routes that are selected by the RT import filter. The routes that are imported into a VRF are BGP routes, so you can use match conditions in a route map to match any BGP attribute of a route. These attributes include communities, local preference, MED, AS path, and so on.

VRF import criteria might be more specific than just the match on the RT. For example:

Import only routes with specific BGP attributes (such as community).

Import routes with specific prefixes or subnet masks (only loopback addresses).

A route map can be configured in a VRF to make the route import more specific.

It is used in scenario:

Deploy advanced MPLS VPN topologies (for example, a managed router services topology).

Increase the security of an extranet VPN by allowing only predefined subnetworks to be inserted into a VRF, thus preventing an extranet site from inserting unapproved subnetworks into the extranet.

| Router(config-vrf)# import map                                                                                                                                                                                                                            | IOS-XE syntax  |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| vrf SiteA rd 1:317 address-family ipv4 unicast import route-policy RTMAP import route-target 1:317 ! export route-target 1:317 ! prefix-set PS-RTMAP 192.168.30.0/24 end-set ! route-policy RTMAP if destination in PS-RTMAP then pass endif end-policy ! | IOS-XR example |

#### Export map

Allows control over which RT is assigned during export. Routes from VRF can be exported with different RTs: An example can be export of management routes with corresponding RT.

Restriction: The export route map can only set RTs and cannot modify other attributes. It does not perform any filtering function.

Routes not explicitly matched by the route-map are still exported, but they are exported without any modifications (default behavior).

This is similar to PBR, where unmatched packets fall back to normal routing instead of being dropped

| Router(config)# route-map route-map-name permit seq match condition set extcommunity rt \[additive]                                                                                                                                                                                             | Every exported routes still get RTs configured through route-target export in VRF A route that has a match in export route-map will have additional (if “additive” is used) RTs assigned. (or rewrite already present without "additive" keyword) |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Router(config-vrf)# export map                                                                                                                                                                                                                                                                  | In this case the route-map is not "filter dropping not matched in the end"                                                                                                                                                                        |
| vrf A rd 1:100 address-family ipv4 unicast import route-target 1:100 1:400 ! export route-policy NMC export route-target 1:100 ! ! extcommunity-set rt NMC 1:401 end-set ! route-policy NMC if destination in (192.168.255.0/24 ge 32) then set extcommunity rt NMC additive endif end-policy ! | XR and XE example![](<../.gitbook/assets/Unknown image (1140)>)                                                                                                                                                                                   |

### Route Target rewrite

Feature used mainly on ASBR's to perform the replacement of route targets on AS boundaries

Of course, the route-map can be extended for rewrite for individual VPNs

| set extcomm-list extended-community-list-number delete |   |
| ------------------------------------------------------ | - |

![](<../.gitbook/assets/Unknown image (1141)>)

Example for multiple VPNs

| ! For VPN A (100:1 -> 200:1) ip extcommunity-list VPN-A-MATCH permit rt 100:1 ! route-map INTERAS-IN permit 10 match extcommunity VPN-A-MATCH set extcomm-list VPN-A-MATCH delete set extcommunity rt 200:1 additive ! ! For VPN B (100:2 -> 200:2) ip extcommunity-list VPN-B-MATCH permit rt 100:2 ! route-map INTERAS-IN permit 20 match extcommunity VPN-B-MATCH set extcomm-list VPN-B-MATCH delete set extcommunity rt 200:2 additive ! ! Final Route-Map Application router bgp 200 address-family vpnv4 unicast neighbor route-map INTERAS-IN in |   |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

### Limiting route count in a VRF

Service providers offering MPLS VPN services risks denial-of-service attacks analogous to Internet service providers (ISPs) that offer BGP connection.

Each customer can generate as many routes as he wants and so he can utilize PE routers’ resources.

It is necessary to limit resources utilized by a single customer.

Cisco IOS software offers two solutions:

Limitation of the number of routes that were received from a BGP neighbor

Limitation of the total amount of routes in VRF.

VRF route limit restricts sum of routes that are imported into VRF:

Routes coming from CE routers

Routes coming from other PEs (imported routes)

Route limit is configured for each VRF.

In case that the route count exceeds the route limit:

A syslog message is generated

A route (optionally) is not inserted into VRF

| Router(config-vrf)#maximum routes limit { warning-percent \| warn-only} /IOS RP/0/RP0/CPU0:router(config-vrf-af)#maximum prefix limit warning-percent /XR | limit - Specifies the maximum number of routes allowed in a VRF. You may select from 1 to 4,294,967,295 routes to be allowed in a VRF. warning-percent – Generate SYSLOG messages for routes when the threshold limit is reached. The threshold limit is a percentage of the limit specified, from 1 to 100. restart interval - An optional Time interval (in minutes) that a peering session is reestablished. The range is from 1 to 65535 minutes. warning-only - (optional) Allows the router to generate a log message when the Maximum-Prefix limit is exceeded, instead of terminating the peering session. |
| --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

BGP Next-Hop Loopback in VRF Configuration

| bgp next-hop Loopback1 | Sets the BGP next-hop for all prefixes in the specified VRF to the IP address of the Loopback1 interface. This is typically used to ensure a stable and reachable next-hop across MPLS or TE core networks, enabling recursive routing via tunnels or engineered paths |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Static routing between GRT and VRFs (VRF lite)

| ip route 192.168.12.1 255.255.255.0 GigabitEthernet0/1 | route in a global routing table to point to the interface which is in the vrf                                                                                                 |
| ------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ip route 192.168.23.3 255.255.255.0 GigabitEthernet0/2 |                                                                                                                                                                               |
| ip route vrf BLUE x.x.x.x x.x.x.x 192.168.12.1 global  | adds a VRF-specific static route in the "BLUE" VRF. It routes traffic destined for IP address 1.1.1.1 to the next-hop IP address 192.168.12.1 using the global routing table. |
| ip route vrf RED x.x.x.x x.x.x.x 192.168.23.3 global   |                                                                                                                                                                               |

### Route leaking with VRF-aware routing

![Route Leaking Topology for Scenario 1](<../.gitbook/assets/Unknown image (1142)>)

| vrf definition A rd 65000:1 ! address-family ipv4 route-replicate from vrf global unicast eigrp 100 route-map EIGRP\_TO\_VRF ! global-address-family ipv4 route-replicate from vrf A unicast bgp 65000 route-map VRF\_TO\_EIGRP ! interface GigabitEthernet1 vrf forwarding A ip address 10.0.0.1 255.255.255.252 ! interface GigabitEthernet2 ip address 192.168.1.1 255.255.255.0 | This replicates the routes from the GRT to the VRF A you can apply the route-map to limit prefixes to be replicated the global-address-family provides the replication VRFs into the GRT |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

| ip prefix-list EIGRP\_TO\_VRF seq 5 permit 192.168.11.0/24 ! ip prefix-list VRF\_TO\_EIGRP seq 5 permit 172.16.10.0/24 ! route-map VRF\_TO\_EIGRP permit 10 match ip address prefix-list VRF\_TO\_EIGRP ! route-map EIGRP\_TO\_VRF permit 10 match ip address prefix-list EIGRP\_TO\_VRF | You can define prefix list for route-maps to limit the prefixes to be leaked between them You can use access-lists aswell                                         |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| router eigrp NAMED ! address-family ipv4 unicast autonomous-system 100 ! topology base redistribute vrf A bgp 65000 metric 1000 1 255 1 1500 exit-af-topology network 192.168.1.0 eigrp router-id 3.3.3.3                                                                                | you can aswell here apply route-map to limit the redistribution of the leaked routes to the neighbors                                                             |
| router bgp 65000 bgp router-id 1.1.1.1 bgp log-neighbor-changes bgp redistribute-internal ! address-family ipv4 vrf A redistribute vrf global eigrp 100 route-map EIGRP\_TO\_VRF neighbor 10.0.0.2 remote-as 65001 neighbor 10.0.0.2 activate                                            | redistribute-internal has to be configured to leak BGP routes to IGP then you can redistribute the leaked routes to the neighbors and limit it with the route-map |

In the output, replicated/leaked routes are represented by a **+** sign next to the route leaked. Example: C+ denotes that a connected route was leaked.

![](<../.gitbook/assets/Unknown image (1143)>)

### VRF-aware PBR

Inherit VRF packets enter the VRF interface and are policy routed or forwarded out of the same VRF

| ip access-list standard 10 10 permit 133.33.33.0 0.0.0.255 ! route-map vrf1\_vrf1 permit 10 match ip address 10 set ip next-hop 135.35.35.2 ! interface GigabitEthernet1 ip policy route-map vrf1\_vrf1 vrf forwarding vrf1 ip address 100.1.1.1 255.255.255.0 ! interface GigabitEthernet2 vrf forwarding vrf1 ip address 135.35.35.1 255.255.255.0 |   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

Inter VRF packets enter a VRF interface and are policy routed or forwarded to another VRF interface

| ip access-list 10 permit 133.33.33.0 0.0.0.255                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| route-map vrf1\_vrf2 permit 10                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| match ip address 10                                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| set ip vrf vrf2 next-hop 135.35.35.2 OR set vrf vrf2 OR set ip default vrf vrf2 next-hop 135.35.35.2 | indicates where to route IPv4 packets that pass a match criteria of a route map using the next-hop specified for the VRF routes packets using a particular VRF table through any of the interfaces belonging to that VRF. If there is no route in the VRF table, the packet will be dropped default keyword verifies the presence of the IP address in the routing table of the VRF. If the IP address is present the packet is not policy routed but forwarded based on the routing table. If the IP address is absent in the routing table, the packet is policy routed and sent to the specified next hop |

| interface GigabitEthernet1/0/1 ip policy route-map vrf1\_vrf2 vrf forwarding vrf1 ip address 100.1.1.1 255.255.255.0 ! interface GigabitEthernet1/0/1 vrf forwarding vrf2 ip address 135.35.35.1 255.255.255.0 |   |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

VRF to Global Routing Table packets enter the VRF interface and are policy routed or forwarded out of the Global Routing Table

| ip access-list standard 10                               |   |
| -------------------------------------------------------- | - |
| 10 permit 133.33.33.0 0.0.0.255                          |   |
| route-map vrf1\_global permit 10                         |   |
| match ip address 10                                      |   |
| set ip default global next-hop 135.35.35.2 OR set global |   |
| interface GigabitEthernet1/0/1                           |   |
| vrf forwarding vrf1                                      |   |
| ip address 100.1.1.1 255.255.255.0                       |   |
| ip policy route-map vrf1\_global                         |   |
| interface GigabitEthernet1/0/2                           |   |
| ip address 135.35.35.1 255.255.255.0                     |   |

Global Routing Table to VRF: Packets enter a global interface and are policy routed or forwarded out of a VRF interface

| ip access-list standard 10                                                                                                                                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 10 permit 133.33.33.0 0.0.0.255                                                                                                                                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| route-map global\_vrf permit 10                                                                                                                                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| match ip address 10                                                                                                                                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| set ip vrf vrf2 next-hop 135.35.35.2 OR set ip default vrf vrf2 next-hop 135.35.35.2 OR set vrf vrf2                                                                                                             | Indicates where to route IPv4 packets that pass a match criteria of a route map using the next-hop specified for the VRF Verifies the presence of the IP address in the routing table of the VRF. If the IP address is present the packet is not policy routed but forwarded based on the routing table. If the IP address is absent in the routing table, the packet is policy routed and sent to the specified next hop Routes packets using a particular VRF table through any of the interfaces belonging to that VRF If there is no route in the VRF table, the packet will be dropped |
| interface GigabitEthernet1/0/1 vrf forwarding vrf1 ip address 100.1.1.1 255.255.255.0 ip policy route-map global\_vrf1 ! interface GigabitEthernet1/0/2 vrf forwarding vrf2 ip address 135.35.35.1 255.255.255.0 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

### VRF receive (GRT connected into VRF)

| vrf definition RED rd 1:1 ! interface Gi3 description GLOBAL\_INTERFACE ip vrf select source ip vrf receive RED ip address 10.10.1.254 255.255.255.0 ! interface Gi4 description VRF\_RED ip address 10.10.3.254 255.255.255.0 ip vrf forwarding RED |   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

### FVRF and IVRF (front-door VRF)

Front-Door VRF (F-VRF) also known as "Internet/Global VRF", is associated with the routing table

The purpose of Front Door VRF is to isolate the underlay and overlay networks, both to avoid potential routing recursion issues as well as to add a layer of security by segmenting the underlay and overlay at L3

It is used for traffic entering or leaving a local network, separating underlay transport routing table from the GRE overlay routing table facing Internet/ISP (WAN)

It becomes advantageous when there's a need to handle dynamic routing, such as BGP, and prevent interference between underlay in internal network and overlay routes from WAN

Inside VRF (I-VRF) also referred to as "Customer/Internal VRF", which is associated with the internal (LAN) routing table

{% hint style="info" %}
Using only an FVRF on WAN (without an I-VRF) can fix recursive routing issues. Example: tunnel “bouncing” when OSPF is underlay and EIGRP is overlay.
{% endhint %}

| vrf definition inet rd 1:1 ! address-family ipv4 exit-address-family ! vrf definition inside rd 2:2 ! address-family ipv4 exit-address-family ! address-family ipv6 exit-address-family ! interface Tunnel1 vrf forwarding inside ip address 10.10.10.1 255.255.255.252 tunnel source GigabitEthernet1 tunnel destination 198.1.1.1 tunnel vrf inet ! interface GigabitEthernet1 vrf forwarding inet ip address 199.1.1.1 255.255.255.252 ! interface GigabitEthernet2 vrf forwarding inside ip address 1.1.1.2 255.255.255.0 ipv6 enable ospfv3 100 ipv4 area 0 ! router ospfv3 100 router-id 2.2.2.2 ! address-family ipv4 unicast vrf inside router bgp 65000 bgp router-id 1.1.1.1 bgp log-neighbor-changes ! address-family ipv4 vrf inet neighbor 199.1.1.2 remote-as 65001 neighbor 199.1.1.2 activate ! ip route vrf inet 0.0.0.0 0.0.0.0 199.1.1.2 ip route vrf inside 0.0.0.0 0.0.0.0 Tunnel1 | ![](<../.gitbook/assets/Unknown image (1144)>) The underlay transport through inet WAN is separated from the overlay, which is LAN within inside vrf The tunnel interface is placed under inside VRF to the overlay and the “tunnel vrf” command, the router is instructed to use the specified VRF’s routing table for the tunnel source and destination IP addresses to reach other LAN subnets on the other end of the tunnel |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Optional IPsec config example crypto ikev2 proposal KEY encryption aes-cbc-256 integrity sha512 group 14 ! crypto ikev2 policy POLICY match fvrf inet match address local 199.1.1.1 proposal KEY ! crypto ikev2 keyring RING peer PEER address 198.1.1.1 pre-shared-key cisco123 ! crypto ikev2 profile PROFILE match fvrf inet match identity remote address 198.1.1.1 255.255.255.255 authentication remote pre-share authentication local pre-share keyring local RING dpd 100 3 periodic ! crypto ipsec transform-set SET esp-aes 256 esp-sha512-hmac mode tunnel ! crypto ipsec profile PROFILE set security-association lifetime days 1 set transform-set SET set pfs group24 set ikev2-profile PROFILE interface Tunnel1 tunnel mode ipsec ipv4 tunnel key 100 tunnel protection ipsec profile PROFILE                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                  |
