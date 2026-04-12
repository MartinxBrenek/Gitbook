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

# Traffic engineering

### Why traffic engineering

An unpredictable growth and alternations of traffic

A lack of links

Non-existing infrastructure

Long-terms of realization

Costs

Failure scenarios



By default, routing is a destination-based logic. With policy-based or TE routing, the default behavior of a router can be influenced

**Policy-based routing (PBR)** is a technique used to influence the outgoing or incoming path route filtering controls what prefixes can or cannot be accepted

The conditions can be made based on many parameters, such as next hop, the source of route advertisement, protocol type, time of the day, etc..

A routing policy is always implemented by using route maps in the Cisco IOS or IOS XE Software or routing policies in Cisco IOS XR Software

Implementing policies that are consistent across the entire AS, that is, implementing policies on edge routers is recommended

{% hint style="info" %}
If you use PBR with `set ip default next-hop` and a packet’s destination is **not** in the RIB, the router forwards it to the PBR next-hop anyway.
{% endhint %}

This is because PBR allows the router to apply forwarding policies that are independent of the routing table lookup process

**Inbound manipulation affects outbound traffic.** When you perform inbound traffic manipulation, you are influencing the way the router or network device receives and processes incoming traffic. This can affect the selection of routes and impact outbound traffic because it determines the paths the router will choose for sending traffic out to other networks.

**Outbound manipulation affects inbound traffic.** When you perform outbound traffic manipulation, you are influencing the way the router or network device advertises routes or policies to its neighbors or the wider network. This can affect the routes that other routers use to send traffic into your network, influencing inbound traffic.

{% hint style="info" %}
Inject prefixes first (`network` or `redistribute`). Then apply route-maps / prefix-lists / distribute-lists / RPL for filtering. You cannot “inject” routes by only applying filters.
{% endhint %}

### Bogon filtering (BGP)

**BGP bogon filtering** removes prohibited or unwanted prefixes from BGP. Bogon examples and configuration in [security](onenote:Security.one#Control%20Plane%20Security\&section-id={0B7736F7-88AD-4CBC-BEE3-A076F5EBB1FC}\&page-id={F6392C8D-F9D2-4208-BEC6-545E8E29E3A7}\&object-id={99FB041B-EE9D-01D5-1234-9C398BD6D893}&4F\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE)

#### Common bogon prefixes

0.0.0.0/8

10.0.0.0/8

127.0.0.0/8

172.16.0.0/12

169.254.0.0/16

192.168.0.0/16

224.0.0.0/3

### Filtering with access lists (ACLs)

Traditional IP prefix filters were implemented with IP access-lists and applied with distribute-list command

IP access-lists serve mainly to permit or deny access or communications

IP access-lists used as route filters have several drawbacks:

They are not designed to filter prefixes

Standard access-list cannot match subnet mask

Only extended access-list can match subnet mask, since it’s structure is changed when applied with distribute list: \[network] \[network wildcard] \[subnet mask] \[subnet mask wildcard] (from the default \[source] \[wildcard] \[destination] \[wildcard])

Access-list is evaluated sequentially meaning the router checks each entry in the order they are defined. This can be inefficient, especially if the access list is long because it must check every entry for each route

More complex to implement

One advantage ACL have, that they can show the matches count

![](<../.gitbook/assets/Unknown image (1838)>)

Extended ACL Filtering can be used to match a specific octet for the matching network and subnet mask (0 bits has to match 1+ doesn't have to match bits)

When you use IP as the protocol, the extended access-list have it's native logical structure:

![Extended access-list filtering](<../.gitbook/assets/Unknown image (1839)>)

When the extended ACL is used for filtering - with distribute list - it is evaluated differently and changes the logical structure:

![extended access list bgp filtering](<../.gitbook/assets/Unknown image (1840)>)

The first field is for the network address, for example 10.0.0.0.

The second field is used to define what part of the network address to check.

For example, when we specify 10.0.0.0 then we use wildcard bits to tell the router if we want to look for 10.0.0.0, 10.0.0.x, 10.0.x.x or 10.x.x.x.

The subnet mask and its wildcard bits are used to define the prefix length, we can use this to tell the router to look for /24, /25, /26 or a range like /24 to /32

#### ACL filtering examples

| access-list 100 permit ip 20.0.0.0 0.0.0.0 255.0.0.0 0.255.255.255  | exact match for 20.0.0.0 for prefixes that have /8 or higher subnet mask |
| ------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| access-list 100 permit ip 172.16.0.0 0.0.0.0 255.255.255.0 0.0.0.0  | exact match for “172.16.0.0” and /24 mask                                |
| access-list 100 permit ip 192.168.1.0 0.0.0.0 255.255.255.0 0.0.0.0 | exact match for “192.168.1.0 and /24 mask                                |

Result: We aplied as distribute list inbound on the R2

In BGP table, you can see R2 is unable to install these prefixes because of a RIB-failure. To solve it add statement to access-list permit ip host any

![](<../.gitbook/assets/Unknown image (1841)>)

#### More ACL filtering examples

| access-list 101 permit ip 192.168.0.0 0.0.255.0 255.255.255.0 0.0.0.0     | Filter all 192.168.x.0 networks with a /24 prefix length                           |
| ------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| access-list 102 permit ip 10.0.0.0 0.255.255.0 255.255.255.0 0.0.0.0      | Filter all 10.x.x.0 networks with a /24 prefix length                              |
| access-list 103 permit ip 10.0.0.0 0.255.255.255 255.255.255.128 0.0.0.0  | Filter all 10.x.x.x networks with a /25 prefix length                              |
| access-list 104 permit ip 192.168.7.0 0.0.0.255 255.255.255.0 0.0.0.255   | Filter all 192.168.7.x networks with any prefix length                             |
| access-list 105 permit ip 0.0.0.0 255.255.255.255 255.255.255.0 0.0.0.255 | This access list permits any network prefix with a mask that ranges from 24 to 32. |

### Distribute list

**Distribute list** filters the contents of incoming or outgoing routing updates by referencing either ACL or prefix-list. IPv6 is compatible only with prefix-lists

Implicit deny statement at the bottom ensures that the packets or prefixes are denied by default

Inbound filters incoming routes, preventing them from being installed to the local routing table or advertised to neighboring routers

Outbound filters routes from being advertised to neighboring routers

| (config-router)# distribute-list \<ACL\_name \| route-map \| prefix> \<route-map\_name \| prefix-list\_name> < in \| out> \[ospf \| eigrp \| static \| rip \| ] \[routing-process-id] | Filters routes redistributed from the specified routing protocol can be specified as last parameter command with out filters routes being redistributed out of a source routing protocol based on a prefix list |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Prefix lists

**Prefix lists** are an improved version of ACL designed specifically for route filtering.

Prefix lists are organized in a tree structure, allowing the router to evaluate routes much more efficiently

When a prefix list is evaluated, the router uses the most significant bits of the prefix to traverse this tree structure, which allows for faster lookups compared to the sequential evaluation of access lists

The sequence numbers that can be configured and seen in the output of the show ip prefix-list command in Cisco IOS are used for organizational and management purposes

Individual entries in prefix lists can be inserted or deleted

Can match on subnet masks

Key access list features remain preserved:

Filtering using “permit” or “deny”

First match wins/is executed

Security-focused: implicit deny at the end of each prefix-list

The matching mechanism has changed:

Matches routes in a part of the address space with a subnet mask longer or shorter than a set number

Prefix-list are identified by using a case-sensitive name

Prefix-list can be applied using prefix-list command. It can be applied also with distribute list or route-map, which then enforces the filtering rules in the network

**High-Order Bit** defines which bits should match the high-order bit pattern

| le | An entry with le operator matches any route within the address space of address/prefix with prefix less or equal (<=) to the le value    |
| -- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| ge | An entry with ge operator matches any route within the address space of address/prefix with prefix greater or equal (>=) to the ge value |

{% hint style="info" %}
If you omit both `ge` and `le`, only an **exact** prefix-length match is permitted.
{% endhint %}

![](<../.gitbook/assets/Unknown image (1842)>)

#### Prefix-list matching example

The 10.168.0.0/13 prefix does not meet the matching length parameter because the prefix length is less than 24 bits

The 10.168.0.0/24 and 10.173.1.0/28 prefixes qualifies because the first 13 bits match the high-order bit pattern, and the prefix length is within the matching greater or equal parameter

10.104.0.0/24 prefix does not qualify because the high-order bit pattern does not match within the high-order bit count

| \[ip \| ipv6] prefix-list seq <> \<permit \| deny> \<x.x.x.x/xx> \[ge <>] \[le <>] \[description]                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ip prefix-list Host\_Routes deny 0.0.0.0/0 ge 32                                                                                                                                            | /0 - not interested in any bit in prefix that must be greater or equal to 32 Host routes and routes with prefix higher than /24 are often filtered out to minimize the size of the routing table use permit to match and accept all /32 routes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ip prefix-list Default\_Route permit 0.0.0.0/0                                                                                                                                              | To match a default route Omitted operator indicates that the prefix length should be the same as the number of bits in the prefix that it should be matched with Single-homed customers running BGP or multihomed customers that do not require full Internet routing should receive only the default route.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ip prefix-list <> seq 5 permit 0.0.0.0/0 le 32                                                                                                                                              | Matches and permits everything 10.0.0.0/0 le 32 → is unusual: IOS interprets /0 as the network mask, so it becomes the same as 0.0.0.0/0 le 32. In practice, it will still match everything, not just 10.x.x.x.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ip prefix-list Private\_pfx permit 10.0.0.0/8 le 32                                                                                                                                         | match all class A networks with any prefix length Private networks and CGNAT 100.64.0.0/10 are always filtered out when sending updates to other autonomous systems.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ipv6 prefix-list permit ::/0 le 128                                                                                                                                                         | permits all IPv6 prefixes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Application                                                                                                                                                                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| (config-router)# neighbor ip-address prefix-list list \[in \| out]                                                                                                                          | Filters inbound or outbound BGP routing updates from the neighbor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| (config-router)# distribute-list prefix-list <> out                                                                                                                                         | Filters routes redistributed from specified routing process into BGP                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ip prefix-list TEST seq 5 permit 123.123.123.123/32                                                                                                                                         | If you want to permit only that single host /32, you don’t need ge/le at all. Just write:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Verification                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| show \[ ip \| ipv6 ] prefix-list \[detail]                                                                                                                                                  | Refcount: When a prefix is created, its reference is set to 1. And the last deletion/insertion prefix list's reference is incremented by 1. If a prefix list is referred by a routing process like "distribute-list prefix pl1 out", its reference is incremented by 1. The reference counter is a common concept of IOS. It shows how many objects uses the memory block. You can imagine that objects are functions like show commands and data structures like configurations. Each entry's refcount is more complicated. Some entries refers other entries. This is because prefix-list entries form a kind of tree data structure and some of entries cannot be deleted without changing links from child entries. Hits: This counter tracks the number of times the prefix list was matched (or "hit") during routing decisions or filtering actions. |
| XR                                                                                                                                                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| <p>ipv4 prefix-list Default_Route<br>permit 0.0.0.0/0<br>!<br>ipv4 prefix-list All_Prefixes<br>permit 0.0.0.0/0 le 32<br>!<br>ipv4 prefix-list Small_Prefixes<br>permit 0.0.0.0/0 ge 25</p> |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

{% hint style="info" %}
A prefix-list does not originate routes. You still need `network` or `redistribute` to advertise routes first, then you can filter what gets sent.
{% endhint %}

![](<../.gitbook/assets/Unknown image (1843)>)

### AS-path access lists

**AS-path access lists** are used to match prefixes based on the BGP AS path attribute

| (config)# ip as-path access-list number \[permit \| deny] regexp (config-router-af)# neighbor ip-address filter-list as-path-acl-number \[in \| out] | define access list                                                             |
| ---------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| (config-router-af)# neighbor filter-list \<as-path-acl-number \[in \| out]                                                                           | then apply it to the neighbor                                                  |
| (config)#route-map permit 10 (config-route-map)#match as-path (config-router-af)# neighbor route-map <> \<in\|out>                                   | or you can match the as path ACL in the route map and attach it as s route map |
| show ip as-path-access-list                                                                                                                          |                                                                                |

### Route maps

**Route maps** are a language used to define and apply complex parameters for filtering and influencing path selection of a routing protocol

Route maps are uniquely identified by a case-sensitive name and consist of one or more statements.

Route maps process a route in the order defined by sequence numbers. Sequence numbers are used for insertion and deletion-specific statements in the route map

Implicit deny statement at the bottom ensures that the packets or prefixes are dropped/filtered out by default

Default statement action is ‘permit’

First matching statement permits or denies the route

Route-maps can also change BGP attributes in incoming or outgoing updates

match statements define the conditions that packets must meet to be considered for modification of set statement.

A route-map can be used for filtering only with just match statement without set statement

set statements specify the actions to take on the packets that match the match statement

deny statement effectively excludes packets from any actions specified in the set statements, so packets will be sent back to default forwarding channels for destination-based routing

permit statement applies the set actions to packets that match the statement's match criteria

When using a route map for BGP (inbound or outbound), an implicit deny all exists at the end. If your goal is modification of specific routes, you must add a final sequence number with the action permit and no match criteria. Failure to do so will drop all routes not explicitly matched by a preceding permit statement.

**Prefix-List/ACL vs. Route Map Action:** You must treat these as two separate layers of logic: 1. Prefix-List/ACL (match criteria): A permit here means the route is a candidate for this route map statement. A deny here means the route is not a match for this statement and proceeds to the next route map sequence. 2. Route Map Action (permit or deny): If the route matches the criteria: a route-map permit processes the route and applies any set commands; a route-map deny discards the route entirely (no further route map processing).

For PBR, there is no need for a default explicit "permit" statement at the end of the route map. This is because route maps have an implicit deny statement at the end, which means that packets that do not match any of the preceding match criteria in the route map will be excluded and forwarded normally based on the RIB

| route-map PBR permit 10                                                                                                                         | show route-map <>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| description <>                                                                                                                                  | set description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| match ip address \[ ACL\_name \| Prefix\_list\_name ] match ip next-hop \[prefix-list list-name] match ip route-source \[prefix-list list-name] | Permit any’ is achieved by specifying permit without ‘match’ clause Multiple match conditions in one statement = OR Multiple match conditions in one route-map = AND Note match policy list can match another route-map                                                                                                                                                                                                                                                                              |
| set \[ metric \| local preference ]                                                                                                             | set action example                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| set tag 444                                                                                                                                     | Route Tags are numeric value that are advertised with all parameters and are used for differentiation and as a match criterion for further distribution control for tagged prefix. Tags does not work with RIP version 1 or IGRP.                                                                                                                                                                                                                                                                    |
| set ip next-hop \[IP] \[verify-availability track <>]                                                                                           | withdraws next hop from RIB, when its state in tracking changes to down, normal forwarding (RIB) is then used                                                                                                                                                                                                                                                                                                                                                                                        |
| set ip next-hop recursive 10.2.2.2                                                                                                              | used if specified next hop is not directly connected and is reachable through another router, so the router needs to perform a recursive lookup to find the intended next hop                                                                                                                                                                                                                                                                                                                        |
| continue \<seq\_num>                                                                                                                            | Provides more flexible configuration and programming of routing and filtering policies Provides the ability to perform any additional entries in route-map after a successful match and set conditions Allows multiple modular configuration and organization of routing and filtering policies and thereby reduce or optimize recurring entries within the same route-maps It is used to jump to another statement instead of exiting (for example jump from 10 to 30 if 20 is meant to be skipped) |
| (config-if)# ip policy route-map PBR                                                                                                            | applies route-map PBR to an interface                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| (config-router)# neighbor ip-address route-map name in \| out                                                                                   | Applies a route-map to incoming or outgoing BGP updates                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| router bgp redistribute route-map intoBGP ! route-map intoBGP permit match ip address set origin igp ! access-list permit                       | since the routes are redistributed, they would have a attribute of ? - incomplete, we can change it to set the origin to IGP, if we want to indicate that the routes are originated by an IGP Note you can also use route-map to set the origin without matching any networks - thus it will be applied to all redistributed routes                                                                                                                                                                  |
| show route-map \[map-name]                                                                                                                      | Displays route-map configuration                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

### Route Policy Language (RPL, IOS XR)

**Route Policy Language (RPL, IOS XR)** is a more powerful and flexible version of the route maps that are available in Cisco IOS XR Software

Like route maps or many other objects, routing policies are identified by using a case-sensitive name

Each routing policy is a single object (no sequence numbers or multiple lines or statements). The RPL begins with route-policy and ends with end-policy

Unlike IOS route-maps operate as a series of statements which are executed sequentially, Route-policies not only operate sequentially but provide the ability to invoke other route-policies much like a ‘C’-program is able to call separately defined functions.

Routing policies attach points:

Redistribution between any pair of routing protocols

Received or sent updates, depending on the limitations of routing protocols (for example, ABRs in OSPF)

Origination of routes in BGP by using network statements or summarization

Injecting routes into the routing table from BGP

Using show commands in BGP to filter the output or test the effect of the routing policy

{% hint style="info" %}
RPL validity is checked either when you configure the policy or at `commit`. An invalid policy causes the commit to fail.
{% endhint %}

**pass action** permits all routes

**drop action** drops all routes. An empty policy implicitly denies all routes.

The RPL also use set statement like route-maps, where implicit pass action is taken.

Last set wins when multiple sets are evaluated for a unique parameter

If the additive keyword is used, the new communities will be added to the existing BGP communities. If omitted, the existing BGP communities will be overwritten

delete condition to strip specific communities; set extcommunity condition to apply extended community

{% hint style="info" %}
If a route-map sequence has no `match`, it matches all routes. In RPL, if you don’t specify a condition, the action applies to all routes.
{% endhint %}

**done** is a control action that immediately terminates the processing of the route policy.

When done is encountered in a policy sequence (after a match), no further policy statements are evaluated, and the route is accepted or rejected based on the last action taken (e.g., pass, drop, or default behavior).

| prepend as-path                             | command can be used to prepend an arbitrary number to the AS path several times.                                                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| replace as-path                             | command can be used to replace specified AS numbers with the local AS number.                                                                                                                                                                                                                                                                                                                                      |
| set metric-type <>                          | to set OSPF metric-type                                                                                                                                                                                                                                                                                                                                                                                            |
| set metric-type {external \| internal}      | to set IS-IS metric-type                                                                                                                                                                                                                                                                                                                                                                                           |
| set level {level-1 \| level-2 \| level-1-2} | to set IS-IS level                                                                                                                                                                                                                                                                                                                                                                                                 |
| set next-hop discard                        | sets the route to Null0 - stopping forwarding any traffic for a network                                                                                                                                                                                                                                                                                                                                            |
| suppress-route unsuppress-route             | Policies can be used with summarization (aggregation) to set various parameters to the summary, but also to specify which individual routes are suppressed or unsuppressed.                                                                                                                                                                                                                                        |
| edit route-policy <>                        | to edit an RPL use [GNU Nano (the default editor since Cisco IOS XR Release 3.6), Micro Emacs, and VIM](onenote:CMD.one#CLI%20Editors\&section-id={8B4407B0-FFD6-4610-BCD3-F1E15ACDDC17}\&page-id={BA9436C0-6DF0-4803-BB9C-717F6B95CC51}\&end\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE) Note jumping to the RPL in CLI as you would to the ACL will result in the policy being rewritten |
| show rpl route-policy \[detail]             | show rpl route-policy <> attach points                                                                                                                                                                                                                                                                                                                                                                             |
| show bgp route-policy                       | used to test the performance of a newly configured policy or to limit the display of a large BGP table for troubleshooting purposes                                                                                                                                                                                                                                                                                |

![](<../.gitbook/assets/Unknown image (1844)>)

RPL uses the conditional statement syntax that is found in many programming languages

if condition then operation1 else operation2 endif

#### Boolean operators

**and** operator if two or more conditions must match.

**or** operator if at least one of two or more conditions must match.

**not** operator to negate a condition.

{% hint style="info" %}
Use parentheses to control operator precedence. Precedence is:

not is always evaluated first

and is evaluated second

or is evaluated last
{% endhint %}

{% hint style="info" %}
Attribute changes are applied only when the policy completes. You can’t match on a value you just modified earlier in the same policy.
{% endhint %}

e.g matching local preference, setting it to value and then matching the value in next if condition - not possible

#### Operators

**eq**: An attribute numerically equal to a specified value

**le**: An attribute numerically lower than or equal to a specified value

**ge**: An attribute numerically greater than or equal to a specified value

**is**: An attribute equal to a specified value (used for non-numerical values)

**in**: An attribute contained in a value sets. Each set can contain multiple values. The in operator can be used for the existence of a value in the set

#### Value sets

**Value sets** are objects that are used to modularize routing policies. Various types of sets exist for different types of parameters and attributes, such as as-path-set community-set extcommunity-set prefix-set and rd-set

![](<../.gitbook/assets/Unknown image (1845)>)

**AS Path Sets**

Use one or more comma-separated ios-regex commands to define the regular expressions that define set membership.

![](<../.gitbook/assets/Unknown image (1846)>)

{% hint style="info" %}
Instead of regex, you can use built-in AS-path conditions:
{% endhint %}

| is-local               | Matches any prefix with an empty AS path attribute (equals regular expression '^$)       |
| ---------------------- | ---------------------------------------------------------------------------------------- |
| neighbor-is _path_     | Matches based on first ASN in the AS path attribute (equals regular expression '^path\_) |
| originates-from _path_ | Matches based on last ASN in the AS path attribute (equals regular expression '\_path$') |
| passes-through _ASN_   | Matches based on ASN anywhere in the AS path (equals regular expression '_path_)         |
| length _len_           | Matches AS paths that are based on number of ASNs in the path                            |
| unique-length _len_    | Matches AS paths that are based on number of unique ASNs in the path                     |

![](<../.gitbook/assets/Unknown image (1847)>)

**Standard community Sets**

can be matched either with RegEx, numbered or named matching

matches-any operator should be used to match routes that have at least one community in the community set.

matches-every operator should be used to match routes that have all communities that are listed in the community set.

![](<../.gitbook/assets/Unknown image (1848)>)

| AS:num"       | is used to match a specific community.      |
| ------------- | ------------------------------------------- |
| "AS:\[range]" | is used to match a range of values.         |
| "AS:\*"       | is used to match all values for a given AS. |

![](<../.gitbook/assets/Unknown image (1849)>)

| internet     | : Match all communities.                                      |
| ------------ | ------------------------------------------------------------- |
| local-as     | : Keep tagged prefixes in the local AS.                       |
| no-advertise | : Prevent tagged prefixes from being advertised to any peer.  |
| no-export    | : Prevent tagged prefixes from being announced to EBGP peers. |

![](<../.gitbook/assets/Unknown image (1850)>)

**Prefix Sets**

A prefix set is used to match routes based on prefix-list-like criteria

![](<../.gitbook/assets/Unknown image (1851)>)

Example

| router bgp address-family ipv4 unicast redistribute route-policy intoBGP ! route-policy intoBGP set origin igp if destination in then pass else drop endif end-policy ! prefix-set , end-set ! | prefix-set is matched in if condition by using attribute destination since the routes are redistributed, they would have a attribute of ? - incomplete, we can change it to set the origin to IGP, if we want to indicate that the routes are originated by an IGP |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

#### Hierarchical (nested) RPL

Large and complex routing policies should preferably be optimized by using modularization as much as possible.

apply command is used within a route-policy to call another route-policy

![](<../.gitbook/assets/Unknown image (1852)>)

#### RPL global parameters

To make policies modular and reusable, parameters can be used in place of fixed values when calling nested policies.

![](<../.gitbook/assets/Unknown image (1853)>)

#### RPL passed parameters

matching based on the AS path is always done using regular expressions, which must be enclosed within single quotes

In below example the right RPL referenced policy on the right. The first apply $med is referenced as 50 and $med1 is referenced to 100, the second is 150 for $med and 200 for $med1

The $AS can be defined in a seprarate RPL like shown in global parameters for example

![](<../.gitbook/assets/Unknown image (1854)>)

#### RPL examples

| RP/0/RP0/CPU0:P1(config)# route-policy setMEDandLP($MED, $LP) RP/0/RP0/CPU0:P1(config-rpl)# set local-preference $LP RP/0/RP0/CPU0:P1(config-rpl)# set med $MED | create a route policy that is named setMEDandLP, which will take two parameters, $MED and $LP. In this route policy you will set the MED and local preference to the values of the parameters |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

| RP/0/RP0/CPU0:P1(config)# route-policy setASandComm($AS, $N) RP/0/RP0/CPU0:P1(config-rpl)# prepend as-path $AS $N RP/0/RP0/CPU0:P1(config-rpl)# set community (64501:511) additive | create a route policy that is named setASandComm, which will take two parameters, $AS and $N. In this route policy you will prepend AS given by the first parameter, N number of times for a given prefix. In addition to that you will also add one additional community, 64501:511 to the prefix |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

| RP/0/RP0/CPU0:P1(config)# route-policy from\_Customer RP/0/RP0/CPU0:P1(config-rpl)# if community matches-any (64511:101) then RP/0/RP0/CPU0:P1(config-rpl-if)# apply setASandComm(64501, 1) | Prepend AS-path 64501 once and add community 64501:511 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| RP/0/RP0/CPU0:P1(config-rpl)# if community matches-any (64511:201) then RP/0/RP0/CPU0:P1(config-rpl-if)# apply setMEDandLP(200, 200)                                                        |                                                        |

| RP/0/RP0/CPU0:P1(config)# router bgp 64511 RP/0/RP0/CPU0:P1(config-bgp)# neighbor 192.168.111.1 RP/0/RP0/CPU0:P1(config-bgp-nbr)# address-family ipv4 unicast RP/0/RP0/CPU0:P1(config-bgp-nbr-af)# route-policy from\_Customer in |   |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

### Regular expressions (regex)

**Regular expressions (regex)** are patterns used to describe and match specific sets of strings. They are commonly used in programming and text processing to search, manipulate, and validate text based on certain criteria. With Regex, you can define rules and patterns that enable you to effectively filter and extract information from large amounts of data, such as the filtering of ASNs in [AS-Path ACL](https://onenote/#BGP%20Cont\&section-id={6EC9C333-3E04-41C1-8C01-0E8D85C128A8}\&page-id={45262C8F-C420-416A-B9D3-D13ED59C5C01}\&object-id={8C92153C-87F7-0D97-240F-F29E59D082D5}&1F\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE/L3.one)

| Character | Description                                        |
| --------- | -------------------------------------------------- |
| ^         | Matches the start of the AS path                   |
| $         | Matches the end of the AS path                     |
| \_        | Matches any delimiter (start, end, or space)       |
| .         | Matches any single character                       |
| \*        | Matches the preceding character zero or more times |
| +         | Matches the preceding character one or more times  |
| ?         | Matches the preceding character zero or one time   |

| \* | None or more characters |
| -- | ----------------------- |
| +  | One or more characters  |
| ?  | None or one character   |

{% hint style="info" %}
Regex special characters must be escaped with `\` if you want literal matching.
{% endhint %}

If you want to match a string that includes the character sequence 3.14, you would write: 3.14

| **Character** | **Description**                                                                             |
| ------------- | ------------------------------------------------------------------------------------------- |
| \|            | logical OR operator (e.g. "\_100\|_200_")                                                   |
| ()            | groups characters for precedence or to capture matched values into \n (e.g. "_100_(200      |
| \[range]      | matches a single character from the defined range of characters (e.g. "\[0-9]", "\[13579]") |
| \n            | matches again what was found within the n-th pair of parentheses (e.g. "(\[0-9]+)(\1)\*")   |
| \X            | removes the special meaning of character X (e.g. ""or"" or "")                              |

| **String**            | **Matches**                                                                  |
| --------------------- | ---------------------------------------------------------------------------- |
| .\*                   | Anything.                                                                    |
| ^$                    | Locally originated routes.                                                   |
| ^10\_                 | Routes learned from AS 10.                                                   |
| _10_                  | Routes transiting through AS 10.                                             |
| ^\[0-9]+$             | Directly connected AS                                                        |
| \_10$                 | Routes originated in AS 10                                                   |
| ^(\[0-9]+)\_10        | Routes from AS 10 where AS 10 is behind one of our directly connected AS'es. |
| ^10\_.                | Networks behind AS 10                                                        |
| .                     | Matches nonlocal prefixes (e.g. all except empty AS path)                    |
| ^number$              | Matches prefixes originating in the specified neighboring AS                 |
| ^(\[0-9]+)(\_.\_1)\*$ | Matches prefixes originating in any neighboring AS and allowing prepending   |
| \[13579]$             | Matches routes originating in odd-numbered autonomous systems                |
| \[02468]$             | Matches routes originating in even-numbered autonomous systems               |

#### Regex example

| ip as-path access-list 1 permit ^$ | The ^$ in the AS path access list matches routes with an empty AS path (i.e., locally originated routes). This means only local routes will be advertised to the neighbor |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

| show ip bgp regexp <> | Displays all routes in BGP table matching regular expression |
| --------------------- | ------------------------------------------------------------ |

Regex is also used to filter output in the CLI

| show interfaces \| include (errors \| collisions) | show only the interfaces that have errors or collisions.                                                                   |
| ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| show interfaces status \| include ^Fa\|^Gi.\*up   | matches lines that start with "Fa" or "Gi" and have the word "up" in them. This will show only the interfaces that are up. |
| show ip int \| include ^Eth.\*up$\|errors\|       |                                                                                                                            |
