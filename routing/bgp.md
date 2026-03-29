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

# BGP

### Overview

Border Gateway Protocol (BGP) is a dynamic EGP designed to process and exchange large quantities of routing information in large WAN networks such as Internet

BGP is path vector routing protocol which is similar to the distance vector.

Border Gateway Protocol (BGP) exists primarily to manage routing between autonomous systems (ASes)—which are large, independently managed networks, typically owned by ISPs, enterprises, or large organizations. Unlike Interior Gateway Protocols (IGPs) such as OSPF or EIGRP, which work within a single AS, BGP is designed to handle routing between ASes on a global scale

It doesn't select the best path based on the link properties, instead it uses path attributes to provide greater control over the path selection

Routing between different ASes requires different approach than having it chosen by the IGP based on the fastest and thus best path.

Due to service level agreements with ISP's, that usually connect the company to other ASes and other company routing policies, we may want or must route certain traffic over specific path

For example, a company may prefer a cheaper ISP for general traffic but a high-performance ISP for critical services.

The internet is essentially a massive collection of independent networks (ASes), each with its own internal policies and routing strategies. Unlike an enterprise network, where a single entity controls all routers, no one controls the entire internet

Simply they don't share control of their infrastructure with others. This independence creates the need for a protocol like BGP to exchange routing information between these ASes

Interior Gateway Protocols (IGPs) like OSPF are designed for hierarchical networks within a single AS, they also maintain complete map of the network for fast convergence.

They assume centralized control for optimal path calculation, which doesn't apply when connecting multiple independent networks

BGP enables ASes to exchange only necessary routes instead of full network topologies. This prevents excessive routing overhead on internet-scale networks.

Unlike OSPF, which floods link-state updates to every router, BGP simply announces the best path to each destination.

BGP is stateful, it keeps the state of each individual prefix it received from neighbors

**Internal BGP (iBGP)** BGP peering between neighbors in the same AS (TTL set to 255 by default)

In large-scale networks an underlaying IGP must be implemented, so that the iBGP routers know how to reach the next-hop IP of another router in the AS

**External BGP (eBGP)** is used for inter-domain routing, where BGP peering is established between neighbors in a different AS

eBGP peering should always be between the directly connected links (TTL set to 1 by default). EBGP does not use the split horizon and instead uses the AS path to detect loops.

Internet Growth Over Time:

The IPv4 routing table has grown significantly over the years. In the early 2000s, it had only around 100,000 prefixes. By 2024, it's reached around 1 million.

IPv6, though not as large as IPv4, is also growing as adoption increases, with about 200,000 prefixes.

With the growth in prefix numbers, BGP routers connected to the global internet require increased memory to store these tables.

**Full BGP table** refers to the entire IPv4 and IPv6 routing information advertised on the Internet, commonly maintained on the edge on Internet-facing routers

**IPv4:** \~1 GB of memory for \~1 million prefixes

**IPv6:** \~500 MB of memory for \~200,000 prefixes

### When to use BGP

When packets intended to other autonomous systems (e.g. ISPs) pass through the AS

AS has multiple connections to other autonomous systems

The data flow entering or leaving the AS must be controllable

![](<../.gitbook/assets/Unknown image (156)>)

### BGP AS number

BGP domain is identified by 2byte (16-bit) AS number that is assigned by IANA on the Internet to ISP's or by the enterprise in private environment

16-bit AS numbers provide insufficient space for the increased demand of companies that require presence on the Internet

BGP 4-byte (32bit) AS numbers increased the available space for ASN numbers, providing up to 4294967296 ASNs, while ensuring backward compatibility with 16-bit systems to ease the migration

A 32-bit AS can be represented in either a single 32-bit number or two 16-bit numbers that are joined using a dot

32-bit AS numbers are carried in a new attribute AS4\_AGGREGATOR a AS4\_PATH to be compatible with older systems with 2byte AS numbering

**ASPLAIN** represents AS in regular decimal number

**ASDOT** represents ASN with 0. - same representation as ASDOT+, but the 2byte ASN is represented in decimal without dot - widely supported and used by RIRs

**ASDOT+** represents ASNs 65536 or higher. Where first part is a high-order value is multiplied by 65536 and second part is a lower-order value (very little support)

All of the older 2-byte ASNs can be represented in the low-order value, with the high-order value set to 0. So for example, 65535 is 0.65535. One more than that, 65536, is outside the value that can be represented in the low-order range alone, and is therefore represented as 1.0. 65537 would be 1.1

AS number = (1 \* 65536) + 10 = 65536 + 10 = 65546 Therefore, the AS number 1.10 is equivalent to the 4-byte AS number 65546

| bgp asnotation dot | Newer IOS have ASPLAIN set by default. ASPLAIN format does not need any changes in AS-path access-lists |
| ------------------ | ------------------------------------------------------------------------------------------------------- |

Reserved 0 and 65535 as well as 64495 to 64511 reserved for documentation

Private AS number range 64,512–65,534 (in 2byte/16-bit range) and 4,200,000,000–4,294,967,294 (in 4byte/32-bit range) can be used in private network

Public AS number range 1 to BGP AS Numbers

{% hint style="info" %}
Public ASNs and public IP ranges are maintained by IANA. IANA reserved AS65535 and AS4294967295.
{% endhint %}

#### 4-Byte / 2-Byte ASN compatibility

4-Byte capability is negotiated when establishing a BGP session

Two new optional transitive attributes: AS4\_PATH (00030000 value in optional transitive attributes) and AS4\_AGGREGATOR

AS 23456 (IANA-ASTRANS) is used as the ASN each time a 4-byte ASN needs to be represented on a router that doesn’t support 4-Byte ASN

In the AS path. This is a valid, reserved ASN – not a bogon

{% hint style="info" %}
AS\_TRANS (23456) must not be used as a real BGP AS.
{% endhint %}

Major restriction: Routers that do not support 4byte ASN cannot filter based on 4byte ASN

![](<../.gitbook/assets/Unknown image (157)>)

Formatting UPDATEs for a 4-Byte peer

Encode each AS number within the AS\_PATH in 4-bytes

Formatting UPDATEs for a non 4-Byte peer

Substitute AS\_TRANS (AS #23456) for each 4-byte AS

AS4\_AGGREGATOR and/or AS4\_ASPATH will contain a 4-byte encoded copy of the attribute

OLD speaker will blindly pass along NEW\_AGGREGATOR and NEW\_ASPATH attributes

![](<../.gitbook/assets/Unknown image (158)>)

### Message types

#### OPEN

OPEN Message is sent to set up BGP adjacency, it conveys BGP version, ASN of originating router, Hold time (default 180 sec) , BGP Identifier (RID), Authentication and capabilities (MP-BGP, route-refresh, Outbound Route Filtering Capability, graceful restart capability or support for 4byte AS number capability)

Based on the exchanged AS numbers, both routers will determine if the exchanged numbers match their configuration and select which type the session is (IBGP or EBGP)

For EBGP sessions, routers will check to see if the neighbor address is in the routing table as a directly connected address (default requirement)

IBGP sessions, on the other hand, can be several hops away

![](<../.gitbook/assets/Unknown image (159)>)

#### UPDATE

UPDATE Message convey Network Layer Reachability Information (NLRI) and its path attributes; update message act also as a keepalive - the router informs in the update whether to add or withdraw route it advertised to the neighbor

![](<../.gitbook/assets/Unknown image (160)>)

#### KEEPALIVE

KEEPALIVE Message ensures that BGP neighbors are still alive. Interval is 1/3 hold time (60sec). If set to 0 > no keepalives sent

#### NOTIFICATION

NOTIFICATION indicates an error condition to a BGP neighbor; the BGP connection is closed immediately after this is sent. Notification messages include an error code, an error subcode, and data that is related to the error. This can happen when hold Time expires, peer capabilities changed or session is reset) causing BGP peering to terminate

#### ROUTE-REFRESH

ROUTE-REFRESH is part of route refresh capability used to dynamically request a re-advertisement of already received NLRI without the need to hard reset peering

### Session establishment

BGP neighbors are not discovered = they must be configured manually

Configuration must be done on both sides of the connection

Both routers will attempt to connect to the other with a TCP session on port number 179

Only one session will remain if both connection attempts succeed

Source IP address of incoming connection attempts is verified against a list of configured neighbors

Since TCP does not provide peer availability service, the BGP have its own keepalive messages to ensure peer availability

The BGP sends BGP/TCP keepalives by default every 60 seconds.

After the connection is established, BGP peers exchange complete routing tables, then the BGP peers send only changes (incremental or triggered updates)

Each BGP router maintains Neighbor table, BGP table and IP routing table with the best paths pulled from the BGP table

### BGP router ID

**BGP router ID** is 32-bit number (in decimal format) identifying BGP router, it can be assigned manually (recommended) or automatically

Automatic assignment:

The highest Loopback interface IPv4 address

The highest active interface IPv4 address

In IPv6 it is necessary to configure RID manually - without RID BGP cannot run

| (config-router)# bgp router-id \<x.x.x.x>          |                                                                   |
| -------------------------------------------------- | ----------------------------------------------------------------- |
| (config-router)# bgp router-id interface Loopback0 | router id can also be explicitly inherit IP of specific interface |

### Neighbor states

**Idle**: Initially, all BGP sessions to the neighbors are idle. This state indicates that the neighbor is configured and the router is searching the routing table to see whether a route exists to reach the neighbor and tries to initiate a TCP connection.

**ConnectRetryTimer**: Set to 60 seconds. State Idle (PfxCt) indicates neighbor has sent more prefixes than the configured max-prefix limit.

**NoNeg**: Indicates that the neighbor does not support or have not enabled AFI.

**Connect**: The router found a route and has completed the three-way TCP handshake.

**Active**: If there is no response for five seconds after the Open message is sent it falls to Active state and starts a new three-way TCP handshake > Open message is sent with hold timer 4min > success OpenSent; Fail > back to Connect.

**OpenSent**: The TCP session is established. The open message is sent, with the parameters for the BGP session and the received Open message is checked by both routers for errors.

**OpenConfirm**: If a BGP open message response is received, te router proceeds to send a keepalive sent and waits for the first keepalive response.

**Established**: When keepalive has been received and the parameter negotiation is complepted, the routers transition to “established” state and begin exchanging BGP routes.

As long as UPDATE and KEEPALIVE messages are received, the hold timer is reset. If hold timer expires or find errors it falls into Idle state

PfxRcd: When the session is in the established state, this value represents the number of received BGP routes

| show bgp neighbor \[address] | Displays all capabilities received and advertised to neighbor, including all details about the session                                                                                                                                                                                                                                                     |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| neighbor ip-address shutdown | Disables communication with a BGP neighbor Used for debugging (troubleshooting) or if we want to shutdown connection to BGP neighbor over link with packet loss. This is the recommended method, since, if we would shut the link, we would not be able to perform ICMP testing to the BGP neighbor IP to test, whether the packet loss has cleared or not |
| bgp log-neighbor-changes     |                                                                                                                                                                                                                                                                                                                                                            |

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### BGP timers

BGP timer adjustment primarily involves the Keepalive and Hold Time to reduce the interval for detecting a failed neighbor session and subsequently speed up network convergence.

**BGP keepalive timer**: Defines a time between successive keepalive messages. Set to 60 seconds by default

**BGP hold-down timer**: Defines how long a router will wait from the last received keepalive or update message before declaring the session dead. Set to 3x keepalive = 180 seconds by default

Lowering of the keepalive and hold-down timers could lead to undesired session terminations when keepalives are not received due to network congestion.

| timers bgp      | Adjusting timers globally                                                |
| --------------- | ------------------------------------------------------------------------ |
| neighbor timers | per neighbor - you will have to reset the bgp to take it into the effect |

### Monitoring

| show bgp \[x.x.x.x/xx]                                            | Displays all paths for specific BGP route                                                                                                                                                                                                                                                          |
| ----------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show bgp \[ ipv4 \| ipv6 \| all] unicast \[summary \| neighbors ] | display bgp prefixes either for all or specific AFI. summary display BGP peerings. neighbors display specific peer                                                                                                                                                                                 |
| show bgp < vpnv4 \| vpnv6> unicast vrf \[summary]                 | displays specific vpnv4/6 VRF in case VRF is used for L3 VPN                                                                                                                                                                                                                                       |
| show bgp neighbors routes                                         | list BGP routes that are received from the BGP neighbor, after inbound filters are applied To see all routes before local inbound filters are applied you must have soft reconfiguration enabled, so that the router saves a copy of all received routes which can be checked with received-routes |
| show bgp neighbors advertised-routes                              | list BGP routes that are sent to the BGP neighbor                                                                                                                                                                                                                                                  |
| show \[ip \| ipv6] route bgp                                      | global RIB                                                                                                                                                                                                                                                                                         |
| debug ip tcp transaction                                          | Displays all TCP transactions (start of session, session errors …)                                                                                                                                                                                                                                 |
| debug ip bgp \[event \| updates] \[ACL]                           | Displays significant BGP events (neighbor state transitions, update runs). Or you can specify neighbor IP                                                                                                                                                                                          |

### Configuration

| router bgp \<AS\_number>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | enables BGP routing process with AS specified. Different AS - eBGP, Same AS - iBGP                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| neighbor remote-as <>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | specifies bgp neighbor/peer and it's remote AS. You can also specify the neighbor loopback IP that is advertised within IGP                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| neighbor <> update-source                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | <p>Configures the source interface for the TCP session that carries BGP traffic Loopback is commonly used as it is not depended on any physical interface, so the BGP session remains up even if any physical interface goes down Using loopback allows BGP to load balance across multiple physical interfaces, providing additional resiliency<br>The BGP peering won't establish if you specify the p2p IP from the subnet connecting two BGP peers while using update source loopback as shown on this packet capture: the router will send itself with source as loopback while trying to establish on the peer p2p IP - the routers expect the source IP from the specified neighbor command<img src="../.gitbook/assets/Unknown image (161)" alt=""> You should also must have configured either ebgp multihop command or disable connected check command to allow the router to establish a connection with ebgp peer even if it is trying to establish on not connected link - loopback In real deployment it is usually deployed without update-source Loopback0, since the peering ISP's would have to have static route in place or disabled connected check to establish peering with our ASBR, thus peering is established on the P2P internet connection link /30 subnet. And ofcourse towards internal AS iBGP peering the update-source Loopback0 is set</p> |
| IOS-XR                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| RP/0/RP0/CPU0:AS2(config)#router bgp 2 RP/0/RP0/CPU0:AS2(config-bgp)#address-family \[ipv6 \| ipv4] unicast RP/0/RP0/CPU0:AS2-001-XR(config-bgp-af)#network 2.0.0.0/21 RP/0/RP0/CPU0:AS2-001-XR(config-bgp)#neighbor 5.0.4.1 RP/0/RP0/CPU0:AS2-001-XR(config-bgp-nbr)#remote-as 5 RP/0/RP0/CPU0:AS2-001-XR(config-bgp-nbr)#address-family ipv4 unicast RP/0/RP0/CPU0:AS2-001-XR(config-bgp-nbr-af)#route-policy PASS\_ALL in RP/0/RP0/CPU0:AS2-001-XR(config-bgp-nbr-af)#route-policy PASS\_ALL out RP/0/RP0/CPU0:AS2-001-XR(config-bgp-nbr-af)#commit | The neighbor has its own sub-tree, where AS is specified Cisco IOS XR Software require a routing policy to advertise and receive routes. For quick lab purposes use RPL with pass statement to accept all in and out routes and apply it to the AFI                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |

{% hint style="info" %}
If you peer BGP using `update-source Loopback`, you need working routing between the loopbacks (static or IGP). Otherwise the session can’t come up.

IOS XR requires an explicit routing policy (RPL) to advertise and receive routes.
{% endhint %}

### Announcing networks in BGP

Only administratively defined networks are announced in BGP:

Manually configure networks to be announced - network command

Use redistribution from IGP - redistribute

Use aggregation to announce summary prefixes - aggregate-address

{% hint style="info" %}
Prefix-lists and route-maps do not originate routes. You still need `network`, `redistribute`, or `aggregate-address` first.
{% endhint %}

| (config-router)#network \<x.x.x.x> mask \<x.x.x.x>                                                         | starts advertising specified classless network into BGP. Important The exact route to the specific prefix length must be present in the RIB for BGP to intall it to the BGP table and advertise it to neighbors - it will not install and advertise prefix 10.0.0.0/24 if you have a route in the RIB for just 10.0.0.0/25 or 10.0.0.0/23 - Confirmed in LAB Static route pointing null 0 can be created to match the shorter prefix in case we want to advertise shorter prefix despite having only route to longer prefixes (e.g supernet) The meaning of “network” command in BGP is completely different from any other routing protocol – does not start BGP routing/processing on any interface !!! |
| ---------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-router)#network \<x.x.x.x>                                                                         | Allows advertising of classsful network into BGP. At least one of the subnets must be present in the routing table (if auto summary is turned on)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| (config-router)#redistribute \<eigrp \| ospf \| bgp \| connected> \[route-map <> \| metric <> \| match <>] | ## Redistribution                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| (config-bgp)# bgp redistribute-internal                                                                    | to allow redistributing internal BGP routes into an IGP                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

### Announcing default route in BGP

A default route (0.0.0.0/0) must exist in the RIB (Routing Information Base) before it can be advertised via BGP, unless using default-information originate (or neighbor-specific default-originate) which can advertise it unconditionally.

Purpose:

Reduces the size of BGP tables in private networks or towards customers.

Standard redistribution from IGP or static routes does not advertise the default route by default. To advertise a default originated from an IGP, default-information originate must be used.

IOS-XE Methods to Inject 0.0.0.0/0 into BGP

| (config-router-af)# network 0.0.0.0                   | Advertises 0.0.0.0/0 into BGP only if it exists in the RIB. Does not originate the default if it isn’t already in the routing table.                                                                                          |
| ----------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| redistribute static/IGP+default-information originate | Works if 0.0.0.0/0 exists in the RIB. If using redistribute default static, it injects 0.0.0.0/0 into BGP, but only when the default route exists in the routing table. Cannot originate a default from redistribution alone. |
| (config-router-af)# neighbor default-originate        | Sends 0.0.0.0/0 to BGP neighbors regardless of its presence in RIB or BGP table. Must be configured per neighbor.                                                                                                             |

On IOS-XR, only aggregate-address reliably injects a default route standalone:

vrf A

address-family ipv4 unicast

aggregate-address 0.0.0.0/0

Other commands like default-information originate, network 0.0.0.0/0, or redistribute static do not work standalone in XR.

aggregate-address essentially originates the default route without relying on RIB presence.

| show bgp regexp | example show bgp regexp ^6451\[1-4] - to display routes originated from 64511-4 you can also simply run show bgp regexp to simulate the \| include operator |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |

### BGP security

| (config-router)# neighbor password                                                                                                                                                                                                          | BGP Neighbor Authentication to secure the peering and prevent attempts to spoof routing information the MD5-based neighbor authentication mechanism must be used to ensure that only authorized peers can establish this BGP neighbor relationship, and that the routing information exchanged between these two devices has not been altered in transit Both routers must be configured with MD5 shared secret password. If the MD5 hash values are not identical, the receiving BGP neighbor discards the packet. configures authentication MD5 hash password with the peer. If authentication is configured after a BGP session is already established, it must be flapped in order to get authentication working. The hash is prepended as TCP option 19. IOS-XE                                                        |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-bgp-nbr)# password encrypted 02050D480809                                                                                                                                                                                           | IOS-XR                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| IOS-XE(config-router)# neighbor <> ttl-security hops RP/0/0/CPU0:R1(config)#router bgp 65534 RP/0/0/CPU0:R1(config-bgp)#neighbor 192.168.223.7 RP/0/0/CPU0:R1(config-bgp-nbr)#remote-as 65507 RP/0/0/CPU0:R1(config-bgp-nbr)#ttl-security ? | To prevent malicious actors from injecting forged BGP packets into the network, RFC 5082 introduced a security mechanism based on TTL values This command sets the TTL of BGP IP packets to 255 and enables BGP to establish connection with external peers residing on networks that are not directly connected. By enabling this feature, the received TTL from a BGP peer is compared with the difference "255 - hop-count". BGP messages coming with a TTL less than this value are not accepted. It also automatically disables the connected check. Command ebgp-multihop and ttl-security are mutually exclusive, and only one command is required to establish a multihop peering session If you attempt to configure both commands for the same peering session, an error message will be displayed in the console |
| (config-router-af)# neighbor maximum-prefix \<threshold-value-(%) > restart \[warning-only]                                                                                                                                                 | security measure to limit the number to control how many prefixes can be received from a neighbor <%-warning-value> = Defines, when warning log entries will be generated Optional threshold parameter specifies the percentage where a warning message is logged (default is 75%) restart = Defines, when a terminated peer connection will be re-established (in minutes, disabled by default). warning-only = Peer connection won’t be terminated but rather only log entries will be generated. The following default limits are used if the user does not configure the maximum number of prefixes for the address family: 512K (524,288) prefixes for IPv4 unicast, 128K (131,072) prefixes for IPv4 multicast, 128K (131,072) prefixes for IPv6 unicast, 512K (524,288) prefixes for VPNv4 unicast                   |

### BGP table

Version

One BGP best path change = BGP table version + 1

Routing table (RIB) update = main routing table + 1

BGP neighbor table update = TblVer + 1

Steady state => all numbers are the same

![](<../.gitbook/assets/Unknown image (162)>)

![](<../.gitbook/assets/Unknown image (163)>)

### Forwarding to external destinations

BGP learned routes do not have outgoing interface associated in the routing table

Recursive lookup is performed to forward IP packets toward external destinations

![](<../.gitbook/assets/Unknown image (164)>)

Recursive lookup

![](<../.gitbook/assets/Unknown image (165)>)

### Implementing changes in BGP policy

All filters apply only to new incoming and outgoing updates

To change outbound routing policy you have to force neighbor to resend updates by resetting the BGP session

This process can be very disruptive for internet routers or routers connected to large WAN with huge amount of routing information

Hard reset

Tears down the BGP session with all neighbors, specific neighbor or all neighbors in a peer-group

All BGP routes and connectivity to BGP neighbor are lost

New session is reestablished within 30 - 60 seconds

Full routing update is exchanged once the session is reestablished, resulting in enforcement of new routing policy

Processing the full Internet routing table can take a long time - hard reset of the BGP session is a very disruptive way to implement routing policies

| clear bgp \[ipv4 \| ipv6 \| vpnv4 \| ...] unicast \[neighbor \| \*] | This command forces all neighbors to resend their entire tables simultaneously |
| ------------------------------------------------------------------- | ------------------------------------------------------------------------------ |

### BGP soft reconfiguration

the router stores all received routes in memory before applying a prefix-list or any other filters. This setting allows you to apply new filters (e.g. prefix-lists or route-maps) without having to reset the BGP session

When you change the prefix-list, you can use the clear ip bgp soft in command, which causes the router to re-apply the new filters to the already stored routes (that is, the "backup" BGP table that soft-reconfiguration created). This command only restores the BGP routing information from the given neighbor and does not drop the connection or the session itself

Extra memory is required, since it will store entire BGP table for given neighbor. It is used for scenarios, where BGP routers doesn't support Route\_refresh capability

Example: A route-map is configured inbound changing the weight for specific routes. Somehow the BGP table differs from the expectation

The separate copy of the incoming routes table can be examined to see if the router is receiving the correct routes in the first place.

| neighbor soft-reconfiguration inbound                                      |                                                                                                                                                                                          |
| -------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RP/0/RP0/CPU0:(config-bgp-nbrgrp-af)#soft-reconfiguration inbound always   |                                                                                                                                                                                          |
| clear ip bgp \<neighbor\_IP> soft \[in\|out]                               | out Resends all BGP routes to the neighbors. Always enabled, not configurable in Enforces inbound BGP filters on the stored neighbor routes Only works with soft reconfiguration enabled |
| show bgp neighbor \[address]                                               | Displays whether route refresh is negotiated with the neighbor - should be displayed as advertised and received                                                                          |
| show bgp \[ipv4 \| ipv6 \| vpnv4 \| ...] unicast neighbors received-routes | this shows the copy of the BGP table sotred in ADJ-RIB-IN                                                                                                                                |

{% hint style="info" %}
On IOS XE you must manually trigger a soft reset to apply new policy. On IOS XR, automatic policy soft reset is enabled by default (and can be disabled).
{% endhint %}

| bgp auto-policy-soft-reset disable | to disable XR BGP automatic soft reset |
| ---------------------------------- | -------------------------------------- |

![](<../.gitbook/assets/Unknown image (166)>)

### BGP route refresh capability

does not reset the BGP session itself; instead, it triggers a process where the router signals its peer to send an UPDATE message containing the entire BGP routing table.

This capability is advertised by BGP routers during session establishment

This update allows the router to refresh its routing information without the need for a hard reset of the BGP session, thus avoiding any disruptions

In Outbound soft reset, the peer creates a new update message based on the latest outbound policy configured on the local router

This update includes withdrawal commands for any networks that the peer will no longer see based on the updated outbound policy

Conversely, an inbound soft reset is initiated by the peer router, prompting the local router to send a ROUTE\_REFRESH request if the peer has advertised support for the ROUTE\_REFRESH capability. This request allows the peer to refresh its routing table without tearing down the BGP session.

{% hint style="info" %}
Soft-reconfiguration must be explicitly enabled per neighbor. Route Refresh is negotiated automatically and used when both peers support it.
{% endhint %}

![](<../.gitbook/assets/Unknown image (167)>)

| clear ip bgp \<neighbor\_IP> soft \[in \| out]                                                              | same syntax as we do in BGP soft configuration, but instead the router sends route refresh message to request all BGP routes                               |
| ----------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| neighbor \[IP \| peer-group] \<filter-list \| distribute-list \| prefix-list \| route-map > <> \<in \| out> | ## Attaching a route map to a BGP neighbor Don't forget to soft reset BGP peering to let prefix list/distribute list to take effect #clear ip bgp \<AS/IP> |

### Outbound Route Filtering (ORF)

is a mechanism used by BGP routers to send and receive ORF capabilities for the purpose of minimizing the number of BGP updates sent between BGP peers, and thus can help reduce the amount of system resources required for generating and processing routing updates by filtering out unwanted routing updates at the source.

BGP ORF enables one BGP peer to send a prefix list to its neighbor to be used to filter outbound routes. In other words, one BGP peer tells the other BGP peer what routes not to send, thus reducing the number of BGP updates, and the size of local BGP tables

These prefix-lists are contained in ORF records included in Route Refresh messages

![](<../.gitbook/assets/Unknown image (168)>)

| neighbor \[IP \| peer-group] capability orf \[prefix-list] \[send \| receive \| both] | Allows negotiation of prefix-list ORF capability during BGP session establishment. Must be configured on both BGP routers “both” allows both sending and receiving of prefix-lists “send” allows only sending of prefix-lists “receive” allows only receiving of prefix-lists |
| ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| clear ip bgp in prefix-filter                                                         | Initiates route refresh message Contains prefix-list in route refresh message (if ORF set and supported on both neighbors)                                                                                                                                                    |
| show bgp neighbor neighbor-ip received prefix-filter                                  | Displays received prefix-list from the neighbor                                                                                                                                                                                                                               |

![](<../.gitbook/assets/Unknown image (169)>)

### Route summarization techniques

By default, the aggregated address and the individual prefixes get advertised

**Static route with Null0** involves configuring a static route to Null0 for the summary network prefix and advertising it with network statement

The drawback is that the summary route is always advertised, even if the individual networks within the summary are unreachable.

**Dynamic aggregation**: an aggregate network prefix is created dynamically when individual component routes that match the aggregate prefix are introduced into the BGP table

The originating router sets the next hop to Null0, this prevents any packets addressed to that summarized route from being forwarded, as they should match more specific routes

Aggregate Address act like new BGP routes with a shorter prefix length. All component networks are tagged with "s"

In BGP when a route is aggregated, the Atomic Aggregate attribute is attached to it to inform the neighbor AS that the originating router aggregated routes and the as-path

Aggregator attribute specifies IP address and AS number of the router that performed route aggregation. Shown in Useful for troubleshooting (does not influence best path selection)

The AS\_Path involved in aggregation is not kept in show output

| (config-router)# aggregate-address \[summary-only] \[as-set]       | Specify aggregated prefix in BGP process The aggregate will be announced if there is at least one network in the specified range in the BGP table Individual networks will still be announced in outgoing BGP updates summary-only: Advertise only the aggregate and not the individual networks, the sub-networks are marked as suppressed (s) as-set option adds a list of all AS, contained in suppressed individual networks - shown in the curly brackets. Useful for troubleshooting (does not influence best path selection) |
| ------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-router-af)# neighbor \[suppress \| unsuppress-map] \[NAME] | Should be used when only single prefixes need to be suppressed/unsuppressed                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| show bgp ipv4 unicast \<prefix/x> longer-prefixes                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

![](<../.gitbook/assets/Unknown image (170)>)

![](<../.gitbook/assets/Unknown image (171)>)

![](<../.gitbook/assets/Unknown image (172)>)

Another example, showing that the aggregated prefix point to null0 interface

![](<../.gitbook/assets/Unknown image (173)>)

![](<../.gitbook/assets/Unknown image (174)>)

![BGP | zartmann.dk](<../.gitbook/assets/Unknown image (175)>)

### BGP best path selection

BGP doesn't use a metric like other IGPs; instead, it uses path attributes, which are appended to the advertised routes (NLRI) conveyed in Update messages. These attributes describe the path characteristics used in the path selection process. Network Layer Reachability Information (NLRI) represents the specific routes or IP prefixes that are advertised between BGP peers

Once BGP routers reach the Established state, they exchange their NLRIs within BGP update messages. All BGP routes received from a neighbor are placed into the BGP table.

Through the BGP route selection process, each router installs only a single best path (by default) into the global routing table. This best path is then advertised to other BGP peers, regardless of the number of routes (NLRIs) in the BGP Loc-RIB table.

Each router compares the offered BGP routes against any other available paths to those networks. The best path—based on the longest prefix match or administrative distance (AD)—is installed in the global routing table (RIB) and used for forwarding.

All other paths remain in the BGP forwarding table in case the best path becomes unavailable. If the current best path is no longer accessible, a new best path is selected.

If a new route with the same prefix and AD is received, it is compared to the existing path; if there is a tie, BGP then compares path attributes to determine the best path. The preferences for these attributes are outlined in the Path Attributes (PA) section.

EBGP routes (BGP routes that are learned from an external AS) have an administrative distance of 20

IBGP routes (BGP routes that are learned from within the AS) have an administrative distance of 200

Whenever identical routes are received from IBGP and EBGP peers, the route from the EBGP peer is preferred.

BGP route is installed in the IP routing table of a router only if the IP address in the next-hop attribute is reachable according to the information already in the routing table

After configuring a BGP network statement, the BGP process searches the global RIB for an exact network prefix match

After verifying that the network statement matches a prefix in the global RIB, the prefix is installed into the BGP Loc-RIB table, then the following BGP PAs are set

Connected network The next-hop BGP attribute is set to 0.0.0.0, the BGP origin attribute is set to i (IGP), and the BGP weight is set to 32,768

Static route or routing protocol The next-hop BGP attribute is set to the next-hop IP address in the RIB, the BGP origin attribute is set to i (IGP), and weight set to 32,768

BGP table is comprised of three tables

**Adj-RIB-In**: Contains the NLRIs in original form before inbound policies

**Loc-RIB (BGP database)**: Contains all the NLRIs that originated locally or from other BGP peers

**Adj-RIB-Out**: Contains the NLRIs after outbound route policies have been processed

![](<../.gitbook/assets/Unknown image (176)>)

BGP process periodically scans the IP forwarding table and inserts or revokes routes from BGP routing table based on their presence in the forwarding table

BGP re-calculates the best path for a prefix upon four possible events:

BGP next-hop reachability change

Failure of an interface connected to an eBGP peer

Redistribution change

Reception of new or removed paths for a route

Advertising a path and replacing it with a new path is called an implicit withdraw.

### Path attributes (PAs)

Attribute propagation to neighbors depends on BGP session type (eBGP or iBGP) = not everything is propagated outside of AS

Well-known attributes must be recognized by all compliant implementations

Optional attributes are only recognized by some implementations (could be private), expected not to be recognized by everyone

Well-known mandatory must be supported and present in all update messages

Origin, AS\_Path, Next\_Hop

Well-known discretionary must be supported; they could be present in update messages

Local preference, Atomic Aggregate

Optional non-transitive are discarded, if unsupported. Recognized optional attributes are propagated to other neighbors based on their meaning

Multi-exit discriminator (MED), CLUSTER\_LIST, Originator-ID

Optional transitive Propagated to other neighbors, if not recognized, Partial bit set to indicate that the attribute was not recognized

Aggregator, Communities , AS4\_AGGREGATOR & AS4\_PATH

BGP table tags

> Best selected path mark. For a path to be selected as “best”, its next-hop must be in the routing table and reachable via recursive routing

An "**s**," for suppressed, indicates that the specified routes are suppressed (usually because routes have been summarized and only the summary route is being sent).

A "**d**," for dampening, indicates that the route is being dampened (penalized) for going up and down too often. Although the route might be up right now, it is not advertised until the penalty has expired.

An "**h**," for history, indicates that the route is unavailable and is probably down; historic information about the route exists, but a best route does not exist.

An "**r**," for RIB failure, Indicate that the router is not using given route for forwarding decisions (using an existing with lower AD) - example can be redistributing into BGP P2P subnet on one of the BGP routers, the other BGP neighbor will tag it as rib failure since it uses the connected entry in its RIB

![](<../.gitbook/assets/Unknown image (177)>)

![](<../.gitbook/assets/Unknown image (178)>)

An "**S**," for stale, indicates that the route is stale (this symbol is used in the nonstop forwarding-aware router)

#### Best path selection order

1. Exclude unsynchronized routes or routes with inaccessible next-hop. All BGP prefixes must pass the route validity check, and the next-hop IP address must be resolvable for the route to be eligible as a best path. Some vendors and publications consider this the first step.
2. Prefer the path with the highest weight
3. Prefer the path with the highest local preference (within AS - iBGP)
4. Prefer routes that the router originated (network, aggregate)
5. Prefer the path with the shortest AS path length
6. Prefer the best origin code. (IGP < EGP < INCOMPLETE)
7. Prefer the path with the lowest MED
8. Prefer eBGPover iBGP (indicated by i)
9. Prefer the path with the lowest IGP metric to the BGP next hop

9 Determine if multiple paths require installation in the routing table for BGP Multipath

10. Prefer the oldest route for eBGP paths
11. Prefer the path with the lowest neighbor BGP RID (11 - 8 on the picture)

If a path contains route reflector (RR) attributes, the originator ID is substituted for the router ID in the path selection process.

12. If the originator or router ID is the same for multiple paths, prefer the path with the minimum cluster list length
13. Prefer the path with the lowest neighbor IP address

![](<../.gitbook/assets/Unknown image (179)>)

#### Weight

Is a Cisco-defined locally significant attribute used to influence outbound routing decisions - local to a single router only. The weight value is never propagated by the BGP protocol

So it is often used to influence path selection of a single router in an BGP cluster, that may be that to prioritize directly connected router rather than accepting path selection from RR

{% hint style="info" %}
Weight is local-only, but it can impact the whole AS if set on a route-reflector (RR).
{% endhint %}

32,768 indicates that the prefix is locally originated. Default is 0 and up to 65535

Using Local Preference over Weight for influencing BGP outbound traffic is strongly recommended and is the industry standard practice. Weight should be used only as a last-resort, localized tool.

If you rely on Weight, you must configure the exact same policy on every single PE router where the decision needs to be made.

Risk: If you have 20 PEs and a new circuit requires an adjustment, you must touch 20 different configuration files. A single missed PE will result in inconsistent routing, creating suboptimal or asymmetric paths and potential black holes or routing loops if not managed perfectly.

Contrast with Local Preference: You change the Local Preference value on the one Egress router receiving the prefix, and iBGP instantly and reliably propagates that policy to all 20 PEs. The policy is centralized and scalable.

![](<../.gitbook/assets/Unknown image (180)>)

| XE                                                      | XR                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| neighbor weight <>                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| route-map \[NAME] permit (config-route-map)# set weight | RP/0/RP0/CPU0:PE1(config)# route-policy AdjustWeight RP/0/RP0/CPU0:PE1(config-rpl)# if destination in (172.16.1.0/24) then RP/0/RP0/CPU0:PE1(config-rpl-if)# set weight 5000 RP/0/RP0/CPU0:PE1(config-rpl-if)# endif RP/0/RP0/CPU0:PE1(config-rpl)# pass RP/0/RP0/CPU0:PE1(config-rpl)# end-policy RP/0/RP0/CPU0:PE1(config)# router bgp 64500 RP/0/RP0/CPU0:PE1(config-bgp)# neighbor 192.168.101.11 RP/0/RP0/CPU0:PE1(config-bgp-nbr)# address-family ipv4 unicast RP/0/RP0/CPU0:PE1(config-bgp-nbr-af)# route-policy AdjustWeight in RP/0/RP0/CPU0:PE1(config-bgp-nbr-af)# commit |

#### Local preference

Used mainly to influence outbound routing decisions. Policy is applied to inbound direction e.g. to apply the local preference to the routes received from the neighbor

Communicated only within a single AS - between iBGP peers and stripped in outbound EBGP updates, except in the EBGP updates with confederation peers. Default is 100

Using Local Preference over Weight for influencing BGP outbound traffic is strongly recommended and is the industry standard practice. Weight should be used only as a last-resort, localized tool.

![](<../.gitbook/assets/Unknown image (181)>)

| XE                                                                                      | XR                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Router(config)# route-map \[NAME] permit Router(config-route-map)# set local-preference | RP/0/RP0/CPU0:PE2# configure RP/0/RP0/CPU0:PE2(config)# route-policy AdjustLPref RP/0/RP0/CPU0:PE2(config-rpl)# if destination in (172.16.1.0/24) then RP/0/RP0/CPU0:PE2(config-rpl-if)# set local-preference 200 RP/0/RP0/CPU0:PE2(config-rpl-if)# endif RP/0/RP0/CPU0:PE2(config-rpl)# pass RP/0/RP0/CPU0:PE2(config-rpl)# end-policy RP/0/RP0/CPU0:PE2(config)# RP/0/RP0/CPU0:PE2(config)# router bgp 64500 RP/0/RP0/CPU0:PE2(config-bgp)# neighbor 192.168.102.21 RP/0/RP0/CPU0:PE2(config-bgp-nbr)# address-family ipv4 unicast RP/0/RP0/CPU0:PE2(config-bgp-nbr-af)# route-policy AdjustLPref in RP/0/RP0/CPU0:PE2(config-bgp-nbr-af)# commit |
| bgp default local-preference <>                                                         | change the default local preference value that is applied to all updates The specified value is applied to all routes that don’t have local preference set (EBGP routes)                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

Originate (3) (Next-Hop)

refers to the next hop for a given prefix. Local paths sourced by the network or redistribute commands (tagged in table as 0.0.0.0) are preferred over local aggregate-address command or learned externally from BGP peer

![](<../.gitbook/assets/Unknown image (182)>)

Accumulated Interior Gateway Protocol (AIGP) (not common _if configured_)

allows BGP to choose the shortest path from end to end via multiple IGPs with unique routing policies and the one with AIGP wins. Optional non-transitive

#### AS\_PATH

describes a sequence of AS numbers through which the network is accessible. The sender’s AS number is prepended to the AS-path attribute when the routing update crosses AS boundary.

As a BGP route advertisement passes through different ASes, each AS that receives the advertisement prepends its own AS number to the AS\_Path attribute

This creates a complete list of ASes that the route has traversed

If a BGP router receives a prefix advertisement with its own AS listed in the AS\_Path attribute in the BGP packet, it discards the prefix to prevent loop

![](<../.gitbook/assets/Unknown image (183)>)

iBGP peerings normally requires full mesh because iBGP isn’t allowed to advertise routes learned from an iBGP peer to another iBGP peer

R2 has to form iBGP also with R4 to prevent that R4 would discard the NLRI to prevent loop

BGP multihop is a feature that enables BGP sessions to be established between two routers that are not directly connected (Transit connectivity). It also allows load balancing over parallel link if the peering is established over loopbacks

![](<../.gitbook/assets/Unknown image (184)>)

**BGP AS Override**

the BGP loop prevention mechanism can cause issues in the network (especially for a big enterprise spanning across multiple locations) where a customer has multiple sites spread geographically, connected by some ISP and using the same AS number, so when site A sends prefix advertisement to site B via ISP MPLS network, the site B rejects it as it has the same ASN

Adding the as-override keyword, the PE router override's the site A's AS with it's own, so site B will accept it. This feature is used on the sending side

Difference to allowas-in: as-override overrides the AS\_PATH attributes of the originating AS with his own AS number.

| Router(config-router-af)# neighbor as-override | Configured on PE. Recommended to be used together with the SoO (Site of Origin) feature |
| ---------------------------------------------- | --------------------------------------------------------------------------------------- |

![](<../.gitbook/assets/Unknown image (185)>)

**BGP Allowas-in**

Disable the AS\_PATH check on the router. The allowas-in command permits multiple occurrences of the same AS number (in this case, the AS number of the service provider) as the AS number of the BGP speaker in the AS path without BGP denying the route.

This feature is used on the receiving side, where a BGP router is configured to accept BGP updates even if its own AS number is present in the AS-Path

| Router(config-router-af)# neighbor allowas-in      | Router will refuse an update if its own AS number appears in the AS\_PATH more times than specified in configuration Limit – (Optional) Specifies the number of times to allow the advertisement of a PE router's ASN. Valid values are from 1 to 10. If no number is supplied, the default value of 3 times is used |
| -------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RP/0/RP0/CPU0:router(config-bgp-nbr-af)#allowas-in | IOS XR                                                                                                                                                                                                                                                                                                               |

![](<../.gitbook/assets/Unknown image (186)>)

{% hint style="info" %}
On IOS XR (PE), you may need to disable the default behavior of filtering updates that contain the local AS in the AS-path: `as-path-loopcheck out disable`.
{% endhint %}

**Dual AS Configuration for Network AS Migrations (Local-AS)**

The BGP Support for Dual AS Configuration for Network AS Migrations feature allows you to merge a secondary AS under a primary AS without disrupting existing peering sessions.

The configuration of this feature is transparent to customer networks. This feature allows a router to appear to external peers as a member of secondary AS during the AS migration.

It also allows the network operator to merge the AS and then later migrate customers to new configurations during normal service windows without disrupting existing peering arrangements.

| neighbor local-as \[as-number \[no-prepend \[replace-as \[dual-as]]]] | no-prepend (Optional) Does not prepend the local AS number to any routes received from the EBGP neighbor. replace-as (Optional) Prepends only the local AS number to the AS path attribute. The AS number from the local BGP routing process is not prepended. dual-as (Optional) Configures the EBGP neighbor to establish a peering session using the real AS number from the local BGP routing process or by using the AS number from local-as |
| --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

The local-AS feature is useful if ISP-A purchases ISP-B, but ISP-B customers do not want to modify any peering arrangements or configurations

The local-AS feature allows routers in ISP-B to become members of ISP-A AS. At the same time, these routers appear to their customers to retain their ISP-B AS number.

ISP-B belongs to AS 100, and ISP-C to AS 300. When peering with ISP-C, ISP-B uses AS 200 as its AS number with the use of the neighbor ISP-C local-as 200 command

In updates sent from ISP-B to ISP-C, the AS\_SEQUENCE in the AS\_PATH attribute contains "200 100". The "200" is prepended by ISP-B due to the local-as 200 command configured for ISP-C

![](<../.gitbook/assets/Unknown image (187)>)

| hostname ISP-B ! router bgp 100 neighbor 192.168.1.2 remote-as 300 neighbor 192.168.1.2 local-as 200 | This is the AS number of ISP-A, which is now used by all routers in ISP-B after its acquisition by ISP-A. This command makes the remote router in ISP-C to see this router as belonging to AS 200 instead of AS 100. This also make this router to prepend AS 200 in all updates to ISP-C. |
| ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

Another Example

![](<../.gitbook/assets/Unknown image (188)>)

| PE1(config)# router bgp 100 PE1(config-router)# neighbor 172.16.33.3 remote-as 1 PE1(config-router)# neighbor 172.16.33.3 local-as 200 no-prepend replace-as dual-as |                                                                                                                                                                      |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CE1(config-router)# neighbor 172.16.33.34 remote-as 100                                                                                                              | After the transition is complete, the configuration on CE1 can be updated to peer with AS 100 during a normal maintenance window or during other scheduled downtime. |

The following example strips private AS 65100 from outbound routing updates for the 172.16.33.3 neighbor and replaces it with public AS 100:

| PE1(config)# router bgp 65100 PE1(config-router)# neighbor 172.16.33.3 local-as 100 no-prepend replace-as |   |
| --------------------------------------------------------------------------------------------------------- | - |

**AS path prepend**

Manual manipulation of AS-path length is called AS-path prepending

AS-Path should be extended with multiple copies of the sender’s AS-number

AS-Path prepending is used to Ensure proper return path selection

Distribute the return traffic load for multi-homed customers

Both local and remote AS\_PATH prepending can be used to influence mainly inbound (return) traffic to the AS by making the AS routes appear less attractive

To "make a route look bad" you add your own AS to the AS-Path and make it longer, prepending other AS is suboptimal as it may cause the EBGP peer to drop the BGP update

AS path prepending potentially allows the customer to influence the route selection of its service providers

![](<../.gitbook/assets/Unknown image (189)>)

| route-map name permit sequence match condition set as-path prepend as-number \[ as-number … ]                                                                                                                                                                                                               | Prepends the specified AS-number sequence to the routes matched by the route-map entry AS-numbers are prepended to the AS-path from the BGP table, the sender’s AS-number is always prepended to the end result                                                                                                                                                                                                                                                                        |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| neighbor address route-map name out                                                                                                                                                                                                                                                                         | Applies the route-map to outgoing updates sent to the specified BGP neighbor                                                                                                                                                                                                                                                                                                                                                                                                           |
| Filtering based on predefined AS-PATH ACL - IOS-XE Router(config)# ip as-path access-list <> {deny \| permit} \[regex-query] Router(config)# route-map \[NAME] permit Router(config-route-map)# match as-path < AS-PATH-ACL>Router(config-route-map)# set as-path \[prepend] \[asn1 asn2 asn... \| last-as] |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| XR route-policy Prepend prepend as-path 123 5 pass end-policy ! router bgp 123 address-family ipv4 unicast network 10.0.0.0/8 ! neighbor 1.0.0.2 remote-as 387 address-family ipv4 unicast route-policy PASS in route-policy Prepend out                                                                    | XR example to filter out specific AS from one neighbor RP/0/RP0/CPU0:PE1(config)# route-policy P2\_Incoming RP/0/RP0/CPU0:PE1(config-rpl)# if as-path originates-from '64503' then drop else pass endif RP/0/RP0/CPU0:PE1(config-rpl)# end-policy RP/0/RP0/CPU0:PE1(config)# router bgp 64501 RP/0/RP0/CPU0:PE1(config-bgp)# neighbor 192.168.121.12 RP/0/RP0/CPU0:PE1(config-bgp-nbr)# address-family ipv4 unicast RP/0/RP0/CPU0:PE1(config-bgp-nbr-af)# route-policy P2\_Incoming in |

How long should the prepending be?

The backup AS-path should be very long to ensure that the primary AS-path will always be shorter

Experiment with various AS path lengths until the backup link is idle.

Add a few more AS numbers for improved security (unexpected changes in the Internet)

Caveat: Long backup AS-path consumes memory on every Internet router

There is no exact mechanism to calculate the required prepended AS-path length

Start with short prepended AS-path, monitor link utilization and extend the prepended path length as needed

Continuously monitor the link utilization and change the prepended AS-path length if required

**Limit AS Path Length**

Some updates on Internet can have very long AS path due to wrong in implementation, bug in SW or and attack

| bgp maxas-limit | Limits the length of the AS path segment to a specified number |
| --------------- | -------------------------------------------------------------- |

Note: Results of AS-path prepending can be observed on the receiving router

**AS-Path Filtering**

Service Provider must filter AS-path prepending, so that only AS of the customer is prepended to the incoming updates.

SP's can no longer use unified AS-path filter for all customers, a dedicated filter is required for each customer

| ip as-path access-list 10 permit ^123(\_123)\*$ | where 123 is the customer AS. Or it is possible to use universal AS-Path ^(\[0-9]+)(\_\1)\*$ |
| ----------------------------------------------- | -------------------------------------------------------------------------------------------- |

| no bgp enforce-first-as | By default, Cisco routers deny any updates recived from an eBGP peer that does not list it's AS number in the path of an incoming update. "no bgp enforce-first-as" will diable that |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

#### Origin

it is historical attribute- it indicates how the route was injected into BGP:

IGP (shows up as i)

EGP (shows up as e)

Incomplete (shows up as ?)

You will see IGP when you use the network command for BGP. It means you advertised the network yourself in BGP. EGP is historical, and you won’t see it in the BGP table anymore. EGP is an old routing protocol. We don’t use it anymore. Incomplete means you have redistributed something into BGP.

#### MED (Multiple-Exit Discriminator)

used to influence path selection in neighbor autonomous systems. The MED attribute is useful only when there are multiple entry points into an AS

It is is exchanged between ASes and is set and advertised by routers in one AS to influence the BGP Path Selection decisions of routers in another AS.

Those routers propagate the MED within their AS, and the routers within the AS use the MED, but do not pass it on to the next AS.

When the same update is passed on to another AS, the metric is set back to the default of 0

MED is called metric in IOS. A lower value of the MED attribute indicates a more preferred path

![](<../.gitbook/assets/Unknown image (190)>)

| (config-router)# default-metric                                                                                                                                                                                                                                                                                                                      | sets the default MED value to all redistributed networks from IGP                                                                                                                                                                                                                                                                                                                                    |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| XE                                                                                                                                                                                                                                                                                                                                                   | XR                                                                                                                                                                                                                                                                                                                                                                                                   |
| CE2(config)# route-map MED permit 10 CE2(config-route-map)# set metric 1000 CE2(config-route-map)# route-map MED permit 20 CE2(config-route-map)# exit CE2(config)# router bgp 64512 CE2(config-router)# address-family ipv4 unicast CE2(config-router-af)# neighbor 192.168.102.2 route-map MED out CE2(config-router-af)# end CE2# clear ip bgp \* | Router(config)# route-policy \[NAME] Router(config-route-map)# set med <32-bit value>                                                                                                                                                                                                                                                                                                                |
| Advanced MED Configuration                                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                      |
| bgp always-compare-med bgp bestpath med always // XR                                                                                                                                                                                                                                                                                                 | By default, BGP routers consider the MED value when selecting the best path for a particular destination prefix, but only if the routes come from the same AS. If there are multiple paths from the same AS, BGP will compare their MED values to determine the best path. #bgp always-compare-med ensures that MED values are always compared, regardless of the AS from which the routes originate |
| bgp deterministic-med                                                                                                                                                                                                                                                                                                                                | When you enable a deterministic MED comparison, you allow a router to compare MED values before it considers BGP route type (external or internal) and IGP metric to the next-hop address. The router will compare MED values immediately after the AS path length.                                                                                                                                  |
| bgp bestpath med missing-as-worst (not recommended) bgp bestpath missing-as-worst // xr                                                                                                                                                                                                                                                              | If the MED attribute is missing for a route, it's assigned with a default value 0, making the routes most preferred In the IETF specification assigns the MED to infinity, to make it less preferred This command configure the router to conform to the IETF standard                                                                                                                               |
| bgp bestpath med confed                                                                                                                                                                                                                                                                                                                              | By default, MED is only considered when selecting routes from the same autonomous system which does not include intra-confederation AS Use this command to allow routers to compare paths learned from confederation peers                                                                                                                                                                           |

Advanced MED Configuration Example

The following example demonstrates how the bgp deterministic-med and bgp always-compare-med commands can influence MED-based path selection. Consider the following BGP routes for the network 172.16.0.0/16, listed in the order that they are received:

entry 1: AS(PATH) 65500, med 150, external, rid 192.168.13.1

entry 2: AS(PATH) 65100, med 200, external, rid 1.1.1.1

entry 3: AS(PATH) 65500, med 100, internal, rid 192.168.8.4

{% hint style="info" %}
BGP compares routes to a destination in pairs, starting with the newest entry. The winner is then compared to the next entry, and so on.
{% endhint %}

When both commands are disabled, BGP compares entry 1 and entry 2. Entry 2 is chosen as the better one because it has a lower router ID. The MED is not checked because the paths are from a different neighbor AS. Next, entry 2 is compared to entry 3. BGP chooses entry 2 as the best path because it is external.

When the bgp deterministic-med command has been disabled and the bgp always-compare-med command has been enabled, BGP compares entry 1 to entry 2. These entries are from different autonomous systems, but because the bgp always-compare-med command is enabled, the MED is used in the comparison. Entry 1 is the better of these two entries because it has a lower MED value. Next, BGP compares entry 1 to entry 3. The MED is checked again because the entries are now from the same AS. BGP chooses entry 3 as the best path.

In the case where the bgp deterministic-med command has been enabled and the bgp always-compare-med command has been disabled, BGP groups routes from the same AS. Then it compares the best entries of each group. The BGP table looks like the following:

entry 1: AS(PATH) 65100, med 200, external, rid 1.1.1.1

entry 2: AS(PATH) 65500, med 100, internal, rid 192.168.8.4

entry 3: AS(PATH) 65500, med 150, external, rid 192.168.13.1

There is a group for AS 65100 and a group for AS 65500. BGP compares the best entries for each group. Entry 1 is the best of its group because it is the only route from AS 100. BGP compares entry 1 to the best of group AS 65500, entry 2 (because it has the lowest MED). Because the two entries are not from the same neighbor AS, the MED is not considered in the comparison. The EBGP route wins over the IBGP route, making entry 1 the best route.

If the bgp always-compare-med command is also enabled, BGP takes the MED into account for the last comparison and selects entry 2 as the best path.

Examtopics question:

Consider the tie breaker with bgp best path selection. In this question, relevant attributes are External vs Internal learned routes, router-id, and MED. See [https://www.cisco.com/c/en/us/support/docs/ip/border-gateway-protocol-bgp/13753-25.html](https://www.cisco.com/c/en/us/support/docs/ip/border-gateway-protocol-bgp/13753-25.html) as it is a crucial point in bgp. (MED > external vs Internal > router-Id).

With bgp deterministic-med command, routes are grouped per AS. Group 1 = AS51 route, Group2 = AS321. Out of Group2, since they are in the same AS, MED is compared by default. Lowest MED wins, so route with MED300 is chosen. Then, route from AS51 (med500) vs route with med300 are compared. Since these routes are coming from different AS, the MED is NOT compared. Therefore, we move on to next attribute which is external vs internal. Since we prefer external, AS51 with med500 is chosen.

![](<../.gitbook/assets/Unknown image (191)>)

### BGP communities

Community is one or more BGP prefix that share a common property such as all should have specific MED or Local Preference value etc

BGP communities is an optional transitive attribute that is used to tag BGP routes to ensure consistent filtering or route-selection policy

Any BGP router can tag routes in incoming and outgoing routing updates or when doing redistribution

Based on the community tags a BGP router can filter routes in incoming or outgoing updates or select preferred routes by using RPL or route-maps

Note Routers that do not support communities pass them along unchanged.

There are three types of BGP communities:

**Standard communities**

**Extended communities**

**Large communities**

BGP Standard Communities

BGP standard community are commonly used to signal to the ISP, what attribute they should adjust. For standard community a 32-bit numerical value is used (0–4,294,967,295), which can be written also in a new-format as two 16-bit numbers - enabled with ip bgp-community new-format

**Private**

High-order 16 bits contain the AS number of the AS that defines the community meaning

Low-order 16 bits have local significance

Example BGP router in AS 65000 want to signal to the BGP neighbor to append a local preference of 200 to the received route, would send community as 65000:200

**Well-Known**

All routers capable of sending or receiving BGP communities must implement well-known communities defining 4 main non-numerical options:

**no-advertise**: Do not advertise routes to any peer (iBGP or eBGP)

**no-export**: Do not advertise routes to EBGP peers. Router will not propagate that update to any external neighbors, except iBGP peers and intraconfederation external neighbors

This is the most widely used predefined community attribute

**local-as**: Do not advertise routes to any EBGP peers (similar to no-export, but it keeps a route within the local AS (or member AS within the confederation))

The route is not sent to external BGP neighbors or to intraconfederation external neighbors

**internet**: Advertise this route to the Internet community

Configuration Steps - IOS-XE

Configure route tagging with BGP communities

| route-map name match set community <> \[additive]                                                                                                                 | Setting a community is done directly in the route-map Matching a community is done with a route-map linked to an ip community-list Within a RR cluster all clients need to have the send-community enabled or else the communities get stripped additive keyword all other communities will get preserved, otherwise they will be overwritten by the set community none keyword deletes all communities from a route |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-route-map)# set comm-list delete \[NAME]                                                                                                                  | ## Deleting a subset of communities of a route (must be linked to an ip community-list)                                                                                                                                                                                                                                                                                                                              |
| (config)# router bgp (config-router)# address-family \[ipv4 \| ipv6 \| vpnv4 \| vpnv6] (config-router-af)# neighbor \[IP \| peer-group] route-map <> \[in \| out] | You have to apply a route map to inbound or outbound BGP updates. Or you can apply a route map to redistributed routes                                                                                                                                                                                                                                                                                               |

Configure BGP community propagation

| (config-router-af)# neighbor \[IP \| peer-group] send-community \<standard\|expanded\|both> | By default, communities are stripped in outgoing BGP updates For IOS-XE Community propagation to BGP neighbors has to be manually configured |
| ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |

Define BGP community access-lists (community-lists) to match BGP communities

| (config)# ip community-list \[1–99] \[permit \| deny] value                                     | Standard community lists are defined in range 1-99 Community lists are similar to access lists - they are evaluated sequentially, line by line All values listed in one line must match for the line to match and permit or deny a route Keyword internet can be used to match any community                                                                                                                               |
| ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config)# ip community-list \[100–199] \[permit \| deny] regexp                                 | Expanded community lists are defined in range 100-199 Expanded community lists are like standard community lists, but they match based on regular expressions Communities attached to a route are ordered, converted to string and matched with regexp Use .\* to match any community value                                                                                                                                |
| (config)# ip community-list \[standard \| expanded] list-name \[permit\|deny] \[value\| regexp] | Named community lists This feature allows you to assign meaningful names to community lists, and it increases the number of community lists that can be configured. It can be configured with regular expressions and with numbered community lists. It increases the number of community lists that a network operator can configure—there is no limitation to the number of named community lists that can be configured |

Example:

The original list of communities in an update:

"10.0.0.0/24, NH=1.1.1.1, origin=I, AS-path=20 30 40, community=10:101, community=10:201, community=10:105, community=10:205"

A string of characters containing an ordered list of community values:

"_10:101\_10:105\_10:201\_10\_205_" ("\_" represents a space)

A regular expression:

"permit _10:.0\[1-5]_" ("\_" represents an underscore that matches spaces)

The result:

This regular expression permits the route because it permits all routes with communities where the first 16 bits carry the AS number 10 and the second 16 bits contain 0 as the second digit and a number between 1 and 5 as the third digit; the first digit can be anything (as indicated by the ".").

Use the regular expression ".\*" to permit any community.

Configure route-maps that match on community-lists and filter routes or set other BGP attributes

| route-map name permit \| deny match community <> \[exact-match] set | Community lists are used in match conditions in route-maps to match on communities attached to BGP routes A route-map with community list matches a route if at least some communities attached to the route match the community list With the exact-match option, all communities attached to the route have to match the community-list Route-maps can be used to filter routes or set other BGP attributes based on communities attached to routes |
| ------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Apply route-maps to incoming or outgoing updates

| (config)# router bgp (config-router)# address-family \[ipv4 \| ipv6 \| vpnv4 \| vpnv6] (config-router-af)# neighbor \[IP \| peer-group] route-map <> \[in \| out] | You have to apply a route map to inbound or outbound BGP updates. Or you can apply a route map to redistributed routes |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |

Example

| router bgp 213 neighbor 1.1.1.1 neighbor 1.1.1.1 route-map outfilter out ! route-map outfilter permit 10 match community 1 ! ip community-list 1 deny 213:5 ip community-list 1 permit internet | Do not pass routes with community 213:5 to EBGP peers |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |

Verification

| show bgp community \[all]  | Displays all routes in a BGP table that have at least one community attached |
| -------------------------- | ---------------------------------------------------------------------------- |
| show bgp community-list <> | Displays all routes in BGP table that match community list clist             |

IOS-XR

| RP/0/RP0/CPU0:PE1(config)# route-policy PassWithCommunity RP/0/RP0/CPU0:PE1(config-rpl)# set community ( 64500:100 , no-export ) RP/0/RP0/CPU0:PE1(config-rpl)# pass RP/0/RP0/CPU0:PE1(config-rpl)# end-policy RP/0/RP0/CPU0:PE1(config)# router bgp 64500 RP/0/RP0/CPU0:PE1(config-bgp)# neighbor 192.168.101.11 RP/0/RP0/CPU0:PE1(config-bgp-nbr)# address-family ipv4 unicast RP/0/RP0/CPU0:PE1(config-bgp-nbr-af)# send-community-ebgp RP/0/RP0/CPU0:PE1(config-bgp-nbr-af)# route-policy PassWithCommunity out RP/0/RP0/CPU0:PE1(config-bgp-nbr-af)# commit RP/0/RP0/CPU0:PE1(config-bgp-nbr-af)# end RP/0/RP0/CPU0:PE1# clear ip bgp \* |   |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

![](<../.gitbook/assets/Unknown image (192)>)

This table lists the goals and the community values.

All customers of the service provider should know this list so that they can use the BGP communities without having to discuss their use with the service provider.

![](<../.gitbook/assets/Unknown image (193)>)

#### Extended communities

define a 64bit tag numerical value in (Type:AS:Membership) format

The first 16 bits are used to encode a type that defines a specific purpose for an extended community, extended community type numbers are assigned by IANA

The remaining 48 bits can be used by operators to implement the required policy, given the purpose of the extended community

MPLS VPN are an example where the Route Target (RT) extended community use to control the exporting and importing of VPN routes.

Simply, Route Target (RT) used in MP-BGP with MPLS L3 VPN , its indicates to PE routers if a route should be imported into VRF

There are many other types of extended communities, such as to encode the Site of Origin (SOO), Ethernet VPN (EVPN), OSPF Domain Identifier.

Site of Origin (SoO)

SOO uniquely identifies the site that originates a route. It is added as a tag to a route as an BGP extended community that prevents routing loops or suboptimal routing, specifically when a back door is present between VPN sites. SOO provides loop prevention in networks with dual-homed sites (sites that are connected to two or more PE routers)

If a route is received with an associated SOO value that matches the SOO value that is configured on the receiving interface, the route is filtered out because it was learned from another PE router or from a backdoor link. This behavior is designed to prevent routing loops.

If a route is received with an associated SOO value that does not match the SOO value that is configured on the receiving interface, the route is accepted into the EIGRP topology table so that it can be redistributed into BGP.

If a route is received without an SOO value, the route is accepted into the EIGRP topology table. The SOO value from the interface that is used to reach the next-hop CE router is appended to the route before it is redistributed into BGP.

| Router(config-router-af)# neighbor send-community both Router(config-router-af)# neighbor soo | Configuration of SoO must be done on the PE routers towards the CE routers (since the inbound prefix is to be tagged)         |
| --------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| IOS\_XR(config-router-afi)# site-of-origin                                                    | The SOO value is set under address family IPv4 VRF configuration mode either directly for a neighbor or for a BGP peer group. |

For other routing protocols, the SOO attribute can be applied to routes learned through a particular VRF interface during the redistribution into BGP - Used for non-BGP SOO use.

| Router(config-if)# ip vrf sitemap route-map                                                                                         | Applies a route map that sets the SOO extended community attribute to inbound routing updates received from this interface. |
| ----------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Router(config)# route-map name permit seq Router(config-route-map)# match conditions Router(config-route-map)# set extcommunity soo | Creates a route map that sets the SOO attribute                                                                             |

![](<../.gitbook/assets/Unknown image (194)>)

#### Large communities

written as numeric 96-bit tags in (Source AS:Action: Target AS) format split into three 32-bit values which can accommodate more identification data including 4-byte AS numbers

#### Cost community

BGP cost community is a nontransitive extended community attribute that is passed to IBGP and confederation peers but not to EBGP peers.

The BGP Cost Community feature allows you to customize the BGP best-path selection process for a local AS or confederation by assigning cost values to specific routes.

Applied to internal routes by configuring the set extcommunity cost command in a route map.

| set extcommunity cost \[igp] | The cost extended community attribute is propagated to IBGP peers when an extended community exchange is enabled with the neighbor send-community command. |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |

The path with the lowest cost community number is preferred. Paths that are not specifically configured with the cost community attribute are assigned a default cost number value of 2,147,483,647 (the midpoint between 0 and 4,294,967,295) and evaluated by the best-path selection process accordingly.

When two paths have been configured with the same cost number value, the path selection process prefers the path with the lowest cost community ID.

The cost community attribute influences the BGP best-path selection process at the Point of insertion (POI)

By default, the POI follows the IGP metric comparison. When BGP receives multiple paths to the same destination, it uses the best-path selection process to determine which path is best. BGP automatically makes the decision and installs the best path into the routing table. The POI allows you to assign a preference to a specific path when multiple equal-cost paths are available. If the POI is not valid for local best-path selection, the cost community attribute is silently ignored.

Multiple paths can be configured with the cost community attribute for the same POI. The path with the lowest cost community ID is considered first. In other words, all the cost community paths for a specific POI are considered, starting with the one with the lowest cost community ID. Paths that do not contain the cost community (for the POI and community ID being evaluated) are assigned the default community cost value (2,147,483,647). If the cost community values are equal, then cost community comparison proceeds to the next-lowest community ID for this POI.

Applying the cost community attribute at the POI allows you to assign a value to a path originated or learned by a peer in any part of the local AS or confederation. The cost community can be used as a tie breaker during the best-path selection process. Multiple instances of the cost community can be configured for separate equal-cost paths within the same AS or confederation. For example, a lower cost community value can be applied to a specific exit path in a network with multiple equal-cost exit points. Then the BGP best-path selection process will prefer the specific exit path.

BGP Cost Community Example

| Router(config)# router bgp 50000 Router(config-router)# neighbor 10.0.0.1 remote-as 50000 Router(config-router)# neighbor 10.0.0.1 update-source Loopback 0 Router(config-router)# address-family ipv4 Router(config-router-af)# neighbor 10.0.0.1 activate Router(config-router-af)# neighbor 10.0.0.1 route-map COST1 in Router(config-router-af)# neighbor 10.0.0.1 send-community both Router(config)# route-map COST1 permit 10 Router(config-route-map)# match ip-address 1 Router(config-route-map)# set extcommunity cost 1 100 |   |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

### Load balancing

BGP always selects only the best path = nexthop (> best), which is then propagated to its neighbors and offered to the routing table and CEF

Load balancing of data traffic over several paths/interfaces:

Single BGP nexthop is reachable via several paths, based on IGP/static routing

Internal BGP – depends on IGP and its configuration inside AS

External BGP – > ebgp-multihop configuration.

Change of BGP default behavior = bgp multipath

| neighbor ebgp-multihop                                                                                  | Used when parallel links between routers exist. Loopback interfaces must be used for BGP peering. Static or dynamic routing must be in use to provide both routers with information about how to reach the loopback interfaces of each other. Otherwise their EBGP session does not complete establishment Depending on the switching mode in use, load sharing is done per packet, per destination, or per source and destination pair. |
| ------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-if)#ip load-sharing ? per-destination Deterministic distribution per-packet Random distribution |                                                                                                                                                                                                                                                                                                                                                                                                                                          |

![](<../.gitbook/assets/Unknown image (195)>)

![](<../.gitbook/assets/Unknown image (196)>)

#### Multipath

is a feature feature can be used to install multiple eBGP/iBGP paths to the same destination in the RIB to ensure load-balancing - but the BGP still advertise only the best route to the neighbors.

For BGP Multipath to work, the paths must have the same attributes, including the same AS path. Default BGP behavior can be changed only globally (Not per neighbor)

Note

1. eBGP multihop can be helpful and recommended if your both links are from single ISP router and terminating on Single router at your end.
2. Multipath used when you have connection from two different routers of same ISP terminating on single router at your end.

| maximum-paths \[ibgp\|eibgp] <1-32> | If configured, a BGP router can select up to six identical BGP routes as the best routes. without any parameter it sets multipath for ebgp only, use ibgp for iBGP-multipath and eibgp for both |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### Add-Path

feature used to advertise multiple paths (instead of only the best path) to the same destination to iBGP neighbors. Unlike Multipath, these paths don't necessarily have to be equal in terms of attributes

Normally used in RR designs to advertise more than the best path to the RR clients/non-clients

A unique path ID (comparable to a RD for VPNv4) is added to the prefix

| Router(config-router-af)# bgp additional-paths select \[all \| backup \| best \| best-external \| group-best] \[options] | ## Configuring BGP with additional paths to select group-best keyword selects the n best paths for the same AS |
| ------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------- |
| Router(config-router-af)# bgp additional-paths \[send \| receive]                                                        | ## Configuring BGP to send and/or receive additional paths globally                                            |
| Router(config-router-af)# neighbor additional-paths \[send \| receive \| disable]                                        | ## Configuring BGP to send and/or receive additional paths on a per-neighbor basis                             |
| Router(config-router-af)# neighbor advertise additional-paths \[all \| best \| group best] \[options]                    | ## Configuring BGP which additional paths to advertise on a per-neighbor basis                                 |
| Router(config-router-af)# bgp additional-paths install                                                                   | ## Configuring BGP to install additional paths in the BGP table                                                |

#### Link bandwidth feature

The BGP Link Bandwidth is an extended community 4-byte attribute used to enable multipath load balancing for external links with unequal bandwidth capacity.

This feature supports IBGP, EBGP multipath load balancing, and EIGRP multipath load balancing in MPLS VPNs.

When this feature is enabled, routes that are learned from directly connected external neighbors are propagated through the IBGP network with the bandwidth of the source external link.

The BGP Link Bandwidth feature allows BGP to be configured to send traffic over multiple IBGP or EBGP learned paths.

The traffic that is sent on those paths is proportional to the bandwidth of the links that are used to exit the AS.

Unequal-cost load balancing over links with unequal bandwidth was not possible in BGP before the BGP Link Bandwidth feature was introduced.

Two paths are designated as equal for load balancing if the weight, local preference, AS path length, MED, and IGP costs are the same.

| bgp dmzlink-bw OR neighbor dmzlink-bw | You enable the BGP Link Bandwidth feature under an IPv4 or VPNv4 address family session Note send-community must be configured aswell enabled per neighbor |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |

**BGP Link Bandwidth Example**

![](<../.gitbook/assets/Unknown image (197)>)

First, R1 is configured to support IBGP multipath load balancing and to exchange the BGP extended community attribute with IBGP neighbors: R2 at the 10.0.1.2 IP address and R3 at the 10.0.1.3 IP address.

| R1(config)# router bgp 100 R1(config-router)# neighbor 10.0.1.2 remote-as 100 R1(config-router)# neighbor 10.0.1.2 update-source Loopback 0 R1(config-router)# neighbor 10.0.1.3 remote-as 100 R1(config-router)# neighbor 10.0.1.3 update-source Loopback 0 R1(config-router)# address-family ipv4 R1(config-router)# bgp dmzlink-bw R1(config-router-af)# neighbor 10.0.1.2 activate R1(config-router-af)# neighbor 10.0.1.2 send-community both R1(config-router-af)# neighbor 10.0.1.3 activate R1(config-router-af)# neighbor 10.0.1.3 send-community both R1(config-router-af)# maximum-paths ibgp 6 |   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

R2 is configured to support multipath load balancing, to distribute R4 (172.16.100.1) and R5 (172.16.200.2) link traffic proportionally to the bandwidth of each link, and to advertise the bandwidth of these links to IBGP neighbors as an extended community:

| R2(config)# router bgp 100 R2(config-router)# neighbor 10.0.1.1 remote-as 100 R2(config-router)# neighbor 10.0.1.1 update-source Loopback 0 R2(config-router)# neighbor 10.0.1.3 remote-as 100 R2(config-router)# neighbor 10.0.1.3 update-source Loopback 0 R2(config-router)# neighbor 172.16.100.1 remote-as 200 R2(config-router)# neighbor 172.16.100.1 ebgp-multihop 1 R2(config-router)# neighbor 172.16.200.2 remote-as 200 R2(config-router)# neighbor 172.16.200.2 ebgp-multihop 1 R2(config-router)# address-family ipv4 R2(config-router-af)# bgp dmzlink-bw R2(config-router-af)# neighbor 10.0.1.1 activate R2(config-router-af)# neighbor 10.0.1.1 next-hop-self R2(config-router-af)# neighbor 10.0.1.1 send-community both R2(config-router-af)# neighbor 10.0.1.3 activate R2(config-router-af)# neighbor 10.0.1.3 next-hop-self R2(config-router-af)# neighbor 10.0.1.3 send-community both R2(config-router-af)# neighbor 172.16.100.1 activate R2(config-router-af)# neighbor 172.16.100.1 dmzlink-bw R2(config-router-af)# neighbor 172.16.200.2 activate R2(config-router-af)# neighbor 172.16.200.2 dmzlink-bw R2(config-router-af)# maximum-paths ibgp 6 R2(config-router-af)# maximum-paths 6 |   |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

Also R3 is configured to support multipath load balancing and to advertise the bandwidth of the link with R5 (172.16.300.3) to IBGP neighbors as an extended community:

| R3(config)# router bgp 100 R3(config-router)# neighbor 10.0.1.1 remote-as 100 R3(config-router)# neighbor 10.0.1.1 update-source Loopback 0 R3(config-router)# neighbor 10.0.1.2 remote-as 100 R3(config-router)# neighbor 10.0.1.2 update-source Loopback 0 R3(config-router)# neighbor 172.16.300.3 remote-as 200 R3(config-router)# neighbor 172.16.300.3 ebgp-multihop 1 R3(config-router)# address-family ipv4 R3(config-router-af)# bgp dmzlink-bw R3(config-router-af)# neighbor 10.0.1.1 activate R3(config-router-af)# neighbor 10.0.1.1 send-community both R3(config-router-af)# neighbor 10.0.1.1 next-hop-self R3(config-router-af)# neighbor 10.0.1.2 activate R3(config-router-af)# neighbor 10.0.1.2 send-community both R3(config-router-af)# neighbor 10.0.1.2 next-hop-self R3(config-router-af)# neighbor 172.16.300.3 activate R3(config-router-af)# neighbor 172.16.300.3 dmzlink-bw R3(config-router-af)# maximum-paths ibgp 6 R3(config-router-af)# maximum-paths 6 |   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

Examtopics

![](<../.gitbook/assets/Unknown image (198)>)

Local preference is applied within an AS, affecting the routing decisions of internal BGP (iBGP) routers. It is not advertised to external BGP (eBGP) peers.

Admin has only access to his own AS 64502. A only work if we have access to ASN 64501 and modify LP between South\_A and South\_B routers not North\_A and South\_A routers

### Multiprotocol BGP (MP-BGP)

MP-BGP is an BGP4 extension that allows BGP to transport any kind of control plane information in a single BGP process such as:

IPv4, IPv6, VRF, Layer 2 and Layer 3 VPN routes and Ethernet VPN (EVPN)

Configured as address family

Backward compatibility with systems that do not support these extensions is ensured (support is negotiated when setting up a BGP session)

This control plane information is distinguished in a separate new optional non-transitive BGP attributes:

Multiprotocol reachable NLRI - MP\_REACH\_NLRI (attribute code: 14) attribute describes IP route information

Multiprotocol unreachable NLRI - MP\_UNREACH\_NLRI (attribute code: 15) attribute withdraws the route from service

Each MP\_REACH\_NLRI contains Address Family Identifier (AFI) and Subsequent Address Family Identifier (SAFI) to identify carried network layer protocols, the next-hop information and the advertised prefix (NLRI). MP-BGP can carry VPNv4 or VPNv6 address families. Route Distinguishers (RDs) are used to differentiate VPN routes belonging to different customers and Route Targets (RTs) to control their distribution. Each customer's VPN routes are segregated within their own VRF instance

MP-BGP is commonly used in MPLS Layer 3 VPN deployments to exchange VPN routing information between Provider Edge (PE) routers

BGP4 extension codes:

AFI = 1 (IPv4) or 2 (IPv6)

Sub-AFI = 1 Unicast

Sub-AFI = 2 Multicast for RPF check

Sub-AFI = 3 For both unicast and multicast

Sub-AFI = 4 Labeled Unicast

Examples:

AFI=1, SAFI=1, IPv4 unicast

AFI=1, SAFI=2, IPv4 multicast

AFI=1, SAFI=128, L3VPN IPv4 unicast

AFI=1, SAFI=129, L3VPN IPv4 multicast

AFI=2, SAFI=1, IPv6 unicast

AFI=2, SAFI=2, IPv6 multicast

AFI=25, SAFI=65, BGP-VPLS/BGP-L2VPN

AFI=25, SAFI=70, EVPN

AFI=2, SAFI=128, L3VPN IPv6 unicast

AFI=2, SAFI=129, L3VPN IPv6 multicast

AFI=1, SAFI=132, RT-Constrain

AFI=1, SAFI=133, Flow-spec

AFI=1, SAFI=134, Flow-spec

AFI=3, SAFI=128, CLNS VPN

AFI=1, SAFI=5, NG-MVPN IPv4

AFI=2, SAFI=5, NG-MVPN IPv6

AFI=1, SAFI=66, MDT-SAFI

AFI=1, SAFI=4, labeled IPv4

AFI=2, SAFI=4, labeled IPv6 (6PE)

#### Configuration (MP-BGP)

An address family is activated within BGP using the address-family command in BGP routing protocol configuration (router configuration mode)

Afterwards, an IPv6 neighbor needs to be activated within that address family using the neighbor activate command

MP-BGP Configuration Structure

![](<../.gitbook/assets/Unknown image (199)>)

| bgp upgrade-cli | used to migrate to the address family format |
| --------------- | -------------------------------------------- |

IPvX routes over IPvX transport

BGP session can be established to global or link-local IPv6 address

BGP-4 is carried on top of TCP

Session can be established either over IPv4 or IPv6 protocol

IPvX routes can can be sent over IPvX session (IPv6 routes can be sent over IPv4 session)

Two methods:

IPv6 and IPv4 routes can be sent over IPv4 session (single TCP session)

IPv6 routes sent over IPv6 session and IPv4 routes sent over IPv4 session (independent transport - separate TCP session for each AFI)

next-hop must correspond to transfering address family (IPv4 for IPv4 prefix and IPv6 for IPv6 prefix)

Next-hop for IPv6 prefix over an IPv4 session is automatically set to the IPv4 mapped IPv6 address

Otherwise this must be corrected manually by configuring and attaching a route map to the neighbor configuration statement

The route map should set an IPv6 next hop address to IPv6 prefixes, and this next hop IPv6 address must be reachable either globally and configured on the link, or reachable by the underlying IGP

Shared:

Using a single TCP session

Reduces number of neighbors

IPv6 over IPv4 session requires modification of next hop attribute

Only one session must be reset every time filters are updated

Independent:

Using separate TCP sessions

Complete independence of IPv4 and IPv6

Additional neighbor configuration required

Next hop can be taken from neighbor address

It’s recommended to use the native IPvX transport to avoid unwanted complexity

IPv6 Payload over IPv4 Transport

![](<../.gitbook/assets/Unknown image (200)>)

![](<../.gitbook/assets/Unknown image (201)>)

![](<../.gitbook/assets/Unknown image (202)>)

PCAP

![](<../.gitbook/assets/Unknown image (203)>)

![](<../.gitbook/assets/Unknown image (204)>)

Independent Transport

![](<../.gitbook/assets/Unknown image (205)>)

![](<../.gitbook/assets/Unknown image (206)>)

![](<../.gitbook/assets/Unknown image (207)>)

| Router(config-router)# no bgp default ipv4-unicast                            | disables the default behavior of automatically enabling IPv4 unicast address-family, as it might cause conflicts with the IPv6 configuration |
| ----------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Router(config-router)# address-family \[ipv4\| ipv6 \| vpnv4 \| vpnv6 \| vrf] | enters the AFI                                                                                                                               |
| Router(config-router-af)# neighbor activate                                   | when configuring MP-BGP, the neighbor/group has to be explicitly activated within address family config                                      |
| Router(config-router-af)# neighbor send-community extended                    | sends NLRI with community extended (to send RT and label in case of L3 VPN)                                                                  |

![udPzWDxIDzx1umfvlhRxII9lqAaZVC1sYoDzrXP0jsNSpFzt0laSwDzFSxRteYdxS1ow3SqbzgXAcasnNr0KRBNPyOmflPMTwPcgWUdBIeyz4KxGhqNdQBpTCJLEeU-cNJD83EJh](<../.gitbook/assets/Unknown image (208)>)

### Link-local BGP peering (IPv6)

Link-local IPv6 address can be used instead of global address

When link-local addresses are used for peering with a neighbor BGP router, these link-local IPv6 addresses are used as next hop IP addresses for the routes carried by BGP

If prefixes are forwarded to iBGP peers, the next-hop must be changed to the global unicast address (for example of the router’s GUA on loopback interface)

Using link-local addresses for BGP peering is most commonly seen at interexchange points. These points are where ISPs and other large organizations meet at a collocation facility, and each puts a router on a common Layer 2 subnet. In this case, using link-local addresses is advantageous because no global-scope addresses need to be used on the peering subnet. Given the large address space in IPv6, prefix conservation is not the main motivator here, rather it is an effort by two parties to have a "neutral meet," with a clean demarcation between their routable address spaces.

Disadvantages

BGP peering using the link-local address may be easier, since no GUA or ULA address allocation is required, however this introduce risk if the address is not manually assigned to an interface.

For example a hardware failure resulting in its replacement or cable move will change the MAC address, resulting in a new link-local address.

This will cause the session to fail because the stateless address autoconfiguration will generate a new IP address

BGP peering to GUA address should be used whenever possible (assuming that the BGP should be established towards public Internet providers or entities) For private peering use ULA (FC)

Example

router bgp 65000

bgp router-id 1.1.1.1

bgp log-neighbor-changes

no bgp default ipv4-unicast

neighbor FE80::1%GigabitEthernet1 remote-as 65002

!

address-family ipv4

exit-address-family

!

address-family ipv6

network 2001:DB8:1::/48

neighbor FE80::1%GigabitEthernet1 activate

| R1(config-router)# neighbor FE80::X%GigabitEthernet1 remote-as AS       | When specifying a link-local address for peering, you must identify the interface associated with that link-local address The router has no mechanism to know which link-local address to use if more than one IPv6 interface is configured. Note that the full interface name must be specified – case-sensitive |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| router(config-route-map)# set ipv6 next-hop ipv6-address                | Specifies the new NEXT\_HOP attribute in a route map                                                                                                                                                                                                                                                              |
| router(config-router-af)# neighbor ipv6-address route-map route-map out | Applies the route map to the neighbor inside address family configuration                                                                                                                                                                                                                                         |

### Improving BGP convergence

As the number of routes in the Internet routing table grows, service providers and large enterprise customers are experiencing a dramatic increase in the time that BGP takes to converge. Networks that once converged in 10 or 15 minutes may now take up to 1 hour, and even longer in extreme situations. In general, convergence is defined as the process of bringing all routing tables to a state of consistency.

As the number of routes in the Internet routing table grows, the time required for BGP to converge increases.

The Internet currently contains more than 900k prefixes.

Network convergence times can range from 10 minutes to more than 1 hour.

BGP is considered converged when:

All routes have been accepted.

All routes have been installed in the routing table.

The input queue and output queue for all peers is 0.

The table version for all peers equals the table version of the BGP table.

![](<../.gitbook/assets/Unknown image (209)>)

Convergence time is a direct measurement of how long the BGP router process runs on the CPU, not the total time that the process is actually running

| show process cpu \| include BGP | to display the volume of CPU resources that are consumed (utilization) because of running BGP processes |
| ------------------------------- | ------------------------------------------------------------------------------------------------------- |

To reduce BGP convergence time and the high CPU utilization that a running BGP process causes, use the following performance improvement features:

Kepalive and Hold-timer,

BFD,

PIC,

BGP Peer groups,

BGP NSF

Increase interface input queues:

Input queue on an interface specifies how many packets can be queued before the packets are dropped.

BGP routers with several peers may experience packet drops on an interface due to many TCP ACK segments

Each interface owns an input queue into which incoming packets are placed to await processing by the router. The rate at which incoming packets are placed in the input queue frequently exceeds the rate at which the router can process the packets. Each input queue has a size that indicates the maximum number of packets that can be placed in the queue. After the input queue becomes full, the interface drops any new incoming packets.

If BGP is advertising thousands of routes to many neighbors, TCP must transmit thousands of packets. BGP peers receive these packets and send TCP ACKs to the advertising BGP speaker, causing the BGP speaker to receive a flood of TCP ACKs in a short time. If the ACKs arrive at a rate that is too high for the router CPU, packets back up in inbound interface queues. By default, router interfaces use an input queue size of 75 packets. In addition, special control packets such as BGP updates use a special queue with SPD that holds 100 packets. During BGP convergence, TCP ACKs can quickly fill the 175 spots of input buffering, causing newly arriving packets to be dropped. On routers with 15 or more BGP peers that also exchange the full Internet routing table, more than 10,000 drops per interface per minute may be seen. You can increase the interface input queue depth to help reduce the number of dropped TCP ACKs, thus reducing the amount of work that BGP must do to converge.

| (config-if)# hold-queue length \[in \| out] | This command limits the size of the IP queue on an interface. The default input hold-queue limit is 75 packets, configurable from 0 to 65,535 packets. A length of 1000 normally will resolve problems that are caused by input queue drops of TCP ACKs. in Specifies the input queue. The default is 75 packets. For asynchronous interfaces, the default is 10 packets. These limits prevent a malfunctioning interface from consuming an excessive amount of memory. out Specifies the output queue. The default is 40 packets. For asynchronous interfaces, the default is 10 packets. These limits prevent a malfunctioning interface from consuming an excessive amount of memory. |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### Path MTU discovery (PMTUD)

The default TCP MSS for BGP traffic is 536 bytes. If you enable PMTU discovery, and thus use a higher MSS for BGP traffic, you can substantially improve BGP convergence because fewer packets are required to send BGP updates e.g prevent fragmentation

| router(config)# ip tcp path-mtu-discovery \[age-timer {minutes \| infinite}] | The command enables the PMTU discovery feature for all new TCP connections from the router. The age timer is a time interval for how often TCP re-estimates the path MTU with a larger MSS The default value of the age timer is 10 minutes, but it can be manually configured up to 30 minutes or disabled (set to infinite) (default is 10 minutes) |
| ---------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### Scan time

The BGP scanner process walks (scans) the BGP table and confirms the reachability of next hops. A change of this status triggers a new BGP route selection for the network. The router then propagates the changes to established BGP neighbors. Increasing the BGP scanner process frequency will make the router find a changed status more quickly, but it will also consume more CPU resources

The BGP scanner process is also responsible for some advanced BGP features. For example, it checks the conditional advertisement to determine whether BGP should advertise conditional prefixes or perform route dampening.

Scan interval is defined per BGP router process and address family.

| (config-router)# bgp scan-time | Defines how often the BGP scanner process scans the BGP table. If lowered, it can improve convergence. import (Optional) Configures import processing of VPNv4 unicast routing information from BGP routers into routing tables. |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### Next Hop Tracking (NHT)

BGP next-hop tracking is an event-driven system that provides faster convergence in on-demand fashion rather than periodically based on the scan-timer. It monitors the RIB for next-hop-related changes for both EBGP and IBGP prefixes. It reports changes to the BGP routing process.

This optimization improves overall BGP convergence by reducing the response time to next-hop changes for routes that are installed in the RIB. When a best-path calculation is run in between BGP scanner cycles, only next-hop changes are tracked and processed.

When a route to a next-hop used within a BGP route is going down, the BGP process will be notified avoiding the default delay timer to expire (60s) and instead is notified by NHT after 5s

| bgp nexthop trigger {delay seconds \| enable} | Enabled by default. bgp scan-time command is ignored if your router has BGP next-hop tracking enabled for the address family. |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |

#### Advertisement interval

The advertisement interval timer controls the rate at which successive advertisements are sent to a BGP neighbor. A BGP-speaking router that sends a route update to a neighbor for a specific destination is not allowed to send another update to the neighbor about the same destination until a time equal to the advertisement interval has elapsed. The advertisement interval timer acts as a form of rate limiting on a per-destination basis, even though the value of the advertisement interval is configured for each neighbor.

The default values are different for IBGP and EBGP peers. For IBGP peers, the interval is set to 0 seconds. For EBGP peers, the interval is set to 30 seconds. For EBGP peers that are configured in a VRF instance, the interval is also set to 0 seconds.

If the time is lowered, convergence can improve.

| (config-router)# neighbor \[ip-address \| peer-group-name] advertisement-interval seconds | Changes the default time interval in the sending of BGP routing updates for a specific neighbor |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |

#### Distributed BGP (IOS XR)

Distributed BGP is an optimization technique available on Cisco IOS XR Software platforms.

Distributed BGP also allows more CPU capacity for receiving, computing, and sending BGP routing updates. When the router is in distributed BGP mode, you can control the number of distributed speakers that are enabled and which neighbors are assigned to each speaker. If no distributed speakers are enabled, BGP operates in standalone mode. If at least one distributed speaker is enabled, BGP operates in distributed mode.

Distributed BGP is used to reduce the impact that a fault in one address family has on another address family. For example, you can have the following scenario:

One speaker with only IPv6 neighbors (peering to IPv6 addresses)

A separate speaker with only IPv4 neighbors (peering to IPv4 addresses)

Another speaker with only VPNv4 PE or CE neighbors (peering to IPv4 addresses distinct from the non-VPN neighbors)

Distributed BGP characteristics:

Supported on Cisco IOS XR Software only

Splits BGP functionality into three process types with several instances:

BGP process manager (one instance)

BGP RIB process (one instance per address family)

BGP speaker process (up to 15 instances)

Used to reduce the impact that a fault in one address family has on another address family

Must be enabled

BGP process manager: Responsible for verifying configuration changes and calculating and publishing the distribution of neighbors among BGP speaker processes. There is a single instance of this process.

bRIB process: Responsible for performing the best-path calculation of routes (receives partial best paths from the speaker). The best route is installed into the bRIB and is advertised back to all speakers. The bRIB process is also responsible for installing routes in the routing table, and for managing routes that are redistributed from the routing table. To accommodate route leaking from one routing table to another, bRIB may register for redistribution from multiple routing table routes into a single route in the bRIB process. There is a single instance of this process for each address family.

BGP speaker process: Responsible for processing all BGP connections to peers. The speaker stores received paths in the RIB and performs a partial best-path calculation, advertising the partial best paths to the bRIB (limited best-path calculation). Speakers perform a limited best-path calculation because, in order to compare MEDs, paths need to be compared from the same AS but may not be received on the same speaker. Because BGP speakers do not have access to the entire BGP local routing table, they can only perform a limited best-path calculation. Only the best paths are advertised to the bRIB to reduce speaker/bRIB IPC, and to reduce the number of paths to be processed in the bRIB. BGP speakers can mark a path as active only after learning the result of the full best-path calculation from the bRIB. Neighbor import and export policies are imposed by the speaker. There are multiple instances of this process in which each instance is responsible for a subset of BGP peer connections. Up to a total 15 speakers for all address families and one bRIB for each address family (IPv4, IPv6, and VPNv4) are supported.

![](<../.gitbook/assets/Unknown image (210)>)

#### BGP Prefix Independent Convergence (PIC)

With BGP PIC, CEF stores an alternate path per prefix. PIC installs next (backup) best route as a repair/backup path in the FIB

Once a failure is detected, BGP will immediately switchover to the repair/backup path, allowing for BGP sub-second convergence - Normally implemented on the BGP Edge devices

| bgp additional-paths install |   |
| ---------------------------- | - |

![](<../.gitbook/assets/Unknown image (211)>)

![](<../.gitbook/assets/Unknown image (212)>)

#### Route dampening

BGP is the only routing protocol that is designed for large internetworks with the specific intention of carrying a large number of prefixes. Several mechanisms are built into BGP that ensure maximum router stability.

BGP router does not forward BGP routing updates immediately after receiving them. Every time a BGP router sends an update, it starts a 5-second timer for internal neighbors and a 30-second timer for external neighbors. No new updates can be sent until that timer expires.

Routers that are external to the source of the flap are not forced to recalculate the best path every second but at most every 30 seconds.

A better approach is to remove the update about the route until the destination can be guaranteed as being more stable.

Route flap dampening, was created to reduce route update processing requirements by suppressing unstable route

Route dampening is a BGP feature designed to minimize the propagation of flapping routes across an internetwork. A route is considered to be flapping when its availability alternates repeatedly.

\#bgp dampening is a mechanism that tracks all routes and issue a penalty score to a route each time it flaps, and removes it from the BGP table until the route becomes stable again

It can be adjusted either within the bgp configuration or further adjusted in RPL

The default behavior of dampening results in stopping propagation of routes that consecutively flap three or more times in a short period, for a certain period.

The penalty score is increased by 1000points for each flap event, and if the score exceeds a predefined Suppress Limit threshold, the route is considered unstable

If a cumulative penalty exceeds the suppress limit (2000 points by default), the route is dampened

It is stored in the BGP table, but is not evaluated in the best-path selection.

Therefore, it is not installed into the routing table nor forwarded to any neighbor. The penalty is remembered by routers when the route is not reachable by storing it as a "history" entry.

After route dampening is enabled, the router never removes a route from the BGP table. A route that a BGP neighbor has withdrawn can still be seen in the BGP table and is marked with an "h" (history state).

The penalty is gradually decreased. The penalty reduction is determined by the half-life, which is 15 minutes by default.

Halflife is a configurable parameter to gradually reduce the penalty score of a suppressed route over time. The route is readvertised when penalty score falls below the reuse limit (750 by default) or when the route has been dampened for more than the maximum suppress time (1 hour by default)

When the penalty drops below one half of the reuse limit, all flap history and penalty is forgotten

Note A penalty is always applied to a path and not to a prefix. A flapping path does not mean that the destination is flapping.

![](<../.gitbook/assets/Unknown image (213)>)

Conditional BGP Dampening

Small prefixes (/25 to /32) are assumed to be more likely to flap, and are hence more aggressively punished if they flap several times

XR

![](<../.gitbook/assets/Unknown image (214)>)

XE

| router bgp as-number address-family \[ipv4 \| ipv6] mvpn vrf vrf-name bgp dampening \[half-life reuse suppress max-suppress-time] |   |
| --------------------------------------------------------------------------------------------------------------------------------- | - |

Monitoring

| Show ip bgp dampening dampened-paths                                                                                 | Displays the dampened routes                                                                                                                                                 |
| -------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Show ip bgp flap-statistics \[regexp regexp \| filter-list access-list \| ip-address mask \[longer-prefix]]          | Displays flap statistics for all routes with dampening history Can match routes against regular expressions, AS path access lists, a specific route, or more specific routes |
| debug ip bgp dampening                                                                                               | Displays the BGP dampening events                                                                                                                                            |
| router# clear ip bgp ip-address flap-statistics \[regexp regexp \| filter-list list-name \| ip-address network-mask] | Clears the flap statistics but does not release dampened routes                                                                                                              |
| router# clear ip bgp dampening \[ip-address network-mask]                                                            | Releases all the dampened routes or just the specified network Flap statistics or dampened routes are also cleared when the BGP session with the neighbor is lost.           |

Cisco / IETF Recommended Best Practice

Per RFC 7196 – Deprecation of BGP Route Flap Damping

Do not enable dampening globally with old aggressive defaults (Suppress = 2000, Reuse = 750, Half-life = 15 min, Max-suppress = 60 min).

If used, apply it only to customer / unstable edge routes or conditionally (e.g. small prefixes /25–/32)

Use milder parameters to avoid unnecessarily penalizing stable prefixes.

Suggested Parameters (if you deploy

Cisco IOS/XR “milder” recommended profile

Half-life: 15 minutes (default, OK)

Reuse limit: 750 (default, OK)

Suppress limit: 2000 (too strict → raise to 6000)

Max suppress time: 30 minutes (instead of 60)

So in config form:

router bgp

bgp dampening 15 750 4000 40

Every flap adds 1000 penalty points.

Example: 4 flaps → 4000 penalty.

Route is suppressed when penalty ≥ suppress-limit (4000).

In your case, it takes 4 flaps in a short period to push a route into dampening

Route is re-advertised when penalty < reuse-limit (750).

IOS exponentially decays the penalty based on half-life (15 min here).

With half-life 15, penalty halves every 15 min (4000 → 2000 → 1000 → 500).

So a route that just crossed 4000 will be suppressed for 40min before dropping below 750 and being reused.

Max suppress-time 40 acts as a cap.

Even if math kept the penalty above 750, IOS will force reuse after 30 minutes.

Conclusion:

The increase of routers’ processing power has made the practice of using BGP dampening to prevent CPU consumption obsolete. Modern routers have enough CPU power to support BGP updates without negatively affecting performance, mostly when they hold only a subset of a full BGP table. BGP Flap dampening is not considered a good practice any longer. There is a number of negative effects associated with the use of technology described in the RIPE Routing Working Group Recommendations On Route-flap Damping. It makes complete sense for this feature to be disabled by default on Cisco routers. Nevertheless it can still be used between the eBGP peers on private links. For instance, it can prevent traffic oscillation between the primary and the backup paths.

#### Prefix suppression

It is important to mention that BGP still advertises networks in RIB-Failure state on Cisco Routers that runs Cisco IOS.

The Suppress BGP Advertisements for Inactive Routes features allows you to configure the suppression of advertisements for routes that are not installed in the Routing Information Base (RIB).

A route that is not installed into the RIB is an inactive route. Inactive route advertisement can occur, for example, when routes are advertised through common route aggregation

[https://www.cisco.com/c/en/us/support/docs/ip/border-gateway-protocol-bgp/213286-understand-bgp-rib-failure-and-the-bgp-s.html](https://www.cisco.com/c/en/us/support/docs/ip/border-gateway-protocol-bgp/213286-understand-bgp-rib-failure-and-the-bgp-s.html)

| (config-router)# bgp suppress-inactive | modifies this behavior to stop the advertisement of the prefixes that are in RIB-Failure state Note: Only the networks in RIB-Failure condition which have a different next-hop in BGP than its same entry in Routing Table are suppressed with the bgp suppress-inactive command. |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### Conditional route injection

BGP routes are commonly aggregated to minimize the number of routes that are used and reduce the size of global routing tables

BGP Conditional Route Injection allow you to improve the accuracy of common route aggregation by conditionally injecting or replacing less specific prefixes with more specific prefixes (the opposite of aggregation)

Route Injection use two route-maps “inject-map” and “exist-map” to install specific IP prefix (or more of them) to the BGP table

| (config-router)# bgp inject-map map1 exist-map map2 \[copy-attributes] | Route Injection use two route-maps “inject-map” and “exist-map” to install specific IP prefix (or more of them) to the BGP table exist-map – this is the condition that monitor presence of IP prefixes in the BGP table exist-map must contain two match statements match ip address prefix-list of the aggregated route match ip route-source prefix-list of the BGP neighbor (/32) inject-map – IP prefixes injected (added) to the local BGP table copy-attributes keyword allows more-specific routes to inherit the attributes of the aggregated route; otherwise they are treated as locally originated routes |
| ---------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show bgp injected-paths                                                | To verify disaggregated routes routes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |

![](<../.gitbook/assets/Unknown image (215)>)

#### Conditional advertisement

BGP conditional advertisement feature provides additional control of route advertisement, depending on the existence of other prefixes in the BGP table

For example: If route A exists in the local BGP table, then DO advertise route B else don't advertise

MANDATORY: An advertise-map (= route-map) which defines the prefixes to be advertised

EITHER: An exist-map (= route-map) which defines the routes to be tracked if they’re existing

OR: A non-exist-map (= route-map) which defines the routes to be tracked if they’re not existing

| Router(config-router-af)# neighbor \[IP \| peer-group] advertise-map \[ROUTE-MAP-NAME] \[nonexist-map \| exist-map] \[ROUTE-MAP-NAME] |   |
| ------------------------------------------------------------------------------------------------------------------------------------- | - |

Example

If 192.168.50.0/24 exists in R2's BGP table, then do not advertise the 128.16.16.0/24 network to R1

![](<../.gitbook/assets/Unknown image (216)>)

![](<../.gitbook/assets/Unknown image (217)>)

#### Attribute filtering and enhanced error handling

BGP Attribute Filter feature allows to discard BGP updates containing specific BGP attributes (prefixes to remove/not insert into BGP table = "treat-as-withdraw")

These prefixes are also removed from the routing table

Furthermore, the function allows you to remove specific BGP attributes from an update and process it further

The main reason is to prevent BGP session flapping due to a "malformed update“

Malformed BGP attributes caused many Internet outages in the past, due to that the BGP modules in the routers couldn’t process these unknown attributes

[https://www.iana.org/assignments/bgp-parameters/bgp-parameters.xhtml](https://www.iana.org/assignments/bgp-parameters/bgp-parameters.xhtml)

Note in other words, this feature is used to discard and withdraw the BGP path atribute value for a given code. For example according to the IANA assignments the AS\_PATH has value 2, so this shouldn't be filtered, instead according to the RFC8093, the recommended values to filter are 30, 31, 129, 241, 242, and 243

| neighbor path-attribute treat-as-withdraw | Discards the update, and if it was for a prefix that is in the routing table, removes it from the routing table                           |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| neighbor path-attribute discard           | The whole attribute is removed from the update message and not propagated further to other peers; valid attributes are processed normally |
| bgp enhanced-error                        | Turned on by default = malformed update is processed as “tread-as-withdraw”                                                               |

Examples

| router bgp 65600 neighbor 2001:DB8:1::2 path-attribute treat-as-withdraw 100 in neighbor 2001:DB8:1::2 path-attribute treat-as-withdraw 128 in | treat-as-withdraw any Update messages received from BGP neighbor that contained attribute 100 or 128                  |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| router bgp 65600 neighbor 2001:DB8:1::2 path-attribute treat-as-withdraw 21 255 in                                                             | treat-as-withdraw any Update messages received from BGP neighbor that contained attribute within range from 21 to 255 |
| router bgp 65600 neighbor 2001:DB8:1::1 path-attribute discard 100 in neighbor 2001:DB8:1::1 path-attribute discard 128 in                     | discards attributes 100 and 128, everything else is processed                                                         |
| router bgp 65600 neighbor 2001:DB8:1::1 path-attribute discard 17 255 in                                                                       | discards attributes in range from 17 to 255, everything else is processed                                             |

In IOS XR:

Basic BGP error handling of less severe errors is enabled by default

Extended BGP error handling of rare errors is disabled by default

Recommend: Enable extended BGP error handling

| router bgp 65530 update in error-handling extended ebgp update in error-handling extended ibgp |   |
| ---------------------------------------------------------------------------------------------- | - |

BGP Support for Sequenced Entries in Extended Community Lists

The BGP Sequenced Entries in Extended Community Lists feature allows you automatic sequencing of individual entries in BGP extended community lists

It also allows you to remove or resequence extended community list entries without deleting the entire existing extended community list.

| ip extcommunity-list expanded-list-number \| expanded list-name {permit\| deny} \[regular-expression] \| standard-list-number \| standard list-name {permit\| deny} \[rt extcom-value] \[soo extcom-value] |   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

Sequenced and Resequenced Extended Community List Entry Example

| Router(config)# ip extcommunity-list standard NAMED\_LIST Router(config-extcom-list)# 1 permit rt 64512:10 Router(config-extcom-list)# 2 permit rt 65000:20 Router(config-extcom-list)# 3 permit rt 64535:30 Router(config-extcom-list)# 4 permit soo 65535:40 | The following example creates and configures a named extended community list that will permit routes only from RT 64512:10, 65000:20, 64535:30, and SOO 65535:40. All other routes are implicitly denied. |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

| Router(config)# ip extcommunity-list standard NAMED\_LIST Router(config-extcom-list)# resequence 50 100 Router(config-extcom-list)# end | This example resequences the extended community list entries in the named community list that is configured. The first entry is resequenced to the number 50 and the range for each subsequent entry that follows increases by 100 (for example, 150, 250, 350, and so on). |
| --------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

| Router> show extcommunity-list Standard extended community-list NAMED\_LIST 50 permit RT:64512:10 150 permit RT:65000:20 250 permit RT:64535:30 350 permit SoO:65535:40 | To display the routes that the named extended community list permits, use the show ip extcommunity-list EXEC command. The output shows the configuration from the first example after it has been resequenced with user-defined values. |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Best practices

Static configuration of router ID using loopback address to prevent changes to the router ID and consequent flapping of BGP sessions

Enable TCP Path MTU discovery to enable use of the largest packet size that does not require fragmentation anywhere along the path between two BGP peers

E-BGP route policies to restrict routes accepted from and advertised to E-BGP neighbors (e.g., bogons, more specifics, infrastructure routes)

Delete inbound communities and extended communities, especially if doing VRF peering; some vendors may accept routes with an RT set from an eBGP neighbor

BGP session authentication using TCP Authentication Option (AO) for session integrity

E-BGP TTL security (i.e., RFC 3682 GTSM) to help protect against remote BGP attacks

AS-PATH limits to filter prefixes with an AS-PATH length greater than a specific value (e.g., 50)

eBGP Route Flap Damping (RFD) can be considered to suppress Internet BGP churn

BGP Best External to advertise the best–external path to I-BGP peers, when a locally selected best path is from an I-BGP peer – may enable faster restoration of connectivity (i.e., BGP PIC Edge)

IETF BCP 194 (RFC 7454: BGP Operations and Security) if providing Internet service

BGP Flowspec for rapid, intra-domain, distributed attack mitigation

BGP Route Target Constrain (RTC) filter changes can cause a lot of churn, so use RTC only where it can dramatically reduce the number of L3VPN routes to update and store

Also, RRs should advertise RT-filter default to clients while RR clients send specific RT-filters to the RRs, not the other way around

BGP update wait-install to postpone advertising UPDATES until the RIB confirms that BGP routes have been installed

With the utilization of BGP Next-Hop Tracking (NHT) and Bidirectional Fault Detection (BFD), the BGP timers can be kept at the default values (keepalive interval is 60 seconds and hold down timer is 180 seconds) for iBGP sessions.

Failure detection should happen with BFD instead of BGP timers whenever available.

This statement is related to BGP for PE-CE, CE-customer network. There is no plan to use BFD for iBGP (generally not recommended).
