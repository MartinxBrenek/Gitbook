---
description: L3
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

# Routing

### Router

**Router** is a device that connect multiple IP networks and routes packets between them, while ensuring that the packets are sent over the best path

Routers enable internetwork communication by connecting interfaces in multiple networks. For example, the router in the figure above has one interface connected to the 192.168.1.0/24 network and another interface connected to the 192.168.2.0/24 network. The router uses a routing table to route traffic between the two networks.

Networks to which the router is attached are called local or directly connected networks. All other networks—networks that a router is not directly attached to—are called remote networks. There are two ways for a router to learn about remote networks. Either the route is entered manually in the routing table by using something called a static route, or the router learns about it automatically by using a dynamic routing protocol.

Every network not accessible over the directly attached interface is considered a remote network.

{% hint style="info" %}
Router interfaces must have an IP address. This lets them learn connected networks. It also lets them act as a default gateway for hosts.

Same for router-to-router P2P links. Each side needs an IP for routing protocols and forwarding.
{% endhint %}

If no IP address is configured, even if the interface is in the "up/up" state, the router will not attempt to send and receive IP packets on the interface. To attain proper operation, for every interface that a router should use for forwarding IPv4 packets, the router needs an IPv4 address.

For the point-to-point link the /30 subnet mask is usually used since only 2 usable addresses are needed on the link between the routers

![](<../.gitbook/assets/Unknown image (933)>)

**CPU**: A CPU, or processor, is the chip installed on the motherboard that carries out the instructions of a computer program. For example, it processes all the information gathered from other routers or sent to other routers.

**Motherboard**: The motherboard is the central circuit board, which holds critical electronic components of the system. The motherboard provides connections to other peripherals and interfaces.

**Ports (also referred to as interfaces)**: Ports are used to connect routers to other devices in the network. Routers can have these types of ports:

**Management ports**: Routers have a console port that can be used to attach to a terminal used for management, configuration, and control. High-end routers may also have a dedicated Ethernet port that can be used only for management. An IP address can be assigned to the Ethernet port, and the router can be accessed from a management subnet. The auxiliary (AUX) interface on a router is used for remote management of the router. Typically, a modem is connected to the AUX interface for dial-in access. From a security standpoint, enabling the option to connect remotely to a network device carries with it the responsibility of vigilant device security.

**Network ports**: The router has many network ports, including various LAN or WAN media ports, which may be copper or fiber cable. IP addresses are assigned to network ports.

![](<../.gitbook/assets/Unknown image (934)>)

A router must perform the following actions to route data:

Identify the destination of the packet: Determine the destination network address of the packet that needs to be routed by using the subnet mask. It uses Layer 3 header information to determine, which interface to use to forward a packet to it's destination

Identify the sources of routing information: Determine from which sources a router can learn paths to network destinations. A source can be a directly connected interface, a dynamic routing protocol, or a statically configured route.

Identify routes: A router may know the same route from multiple sources and choose the preferred route based on certain criteria.

Select routes: Select the best path to the intended destination.

Maintain and verify routing information: Update known routes and the selected route according to network conditions.

Router maintains a routing table, similar to the CAM table in a switch, where it stores all known networks by which it selects the interface through which to send a packet in the direction of its destination

When a router receives an incoming packet, it examines the destination IP address in the packet and searches for the best match between the destination address and the network addresses in the routing table. A matching entry may indicate that the destination is directly connected to the router or that it can be reached via another router. This router is called the next-hop router and is on the path to the final destination. If there is no matching entry, the router sends the packet to the default route. If there is no default route, the router drops the packet.

![](<../.gitbook/assets/Unknown image (935)>)

Cisco routers can be categorized in:

Branch Routers

Service Provider Routers

Small Business Routers

Virtual routers are used for public, private, or provider-hosted clouds.

### Multilayer (Layer 3) switch

**Multilayer (Layer 3) switch** is a switch capable of routing

| #ip routing    | To enable routing on a L3 switch:                                                                                                            |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| #no switchport | to enable IP routing on an port. The switchport turns into routed port, thus no STP or other L2 mechanisms would be operational on that port |

### Firewall (in routing context)

**Firewall** is a network security device similar to router that is able to examine Layer 4 information, allowing it to filter incoming and outgoing packets. It performs stateful inspection of network traffic to allow or block communications based on administratively configured rules such as source/destination IP, ports and protocols, helping to protect the network from potential attacks

Firewall establishes a barrier between trusted internal networks and untrusted outside networks such as the Internet

Firewall is placed on the edge perimeter of the network to serve as the secure gateway or barrier between inside and outside network

### IP forwarding / routing process

L3 forwarding is destination-based - if I want to get to destination X, I must go to Y

Upon receiving an Ethernet frame, the router examines the Ethernet header to verify, whether the destination MAC address matches his own/or local broadcast or multicast MAC that his features are subscribing for, while verifying the frame's integrity with FCS (if the FCS fails the router drops the frame) - remember Layer 2 and 3 does not have any error recovery/retransmission, it is performed by the Layer 4

If the destination MAC address matches his own it proceeds to examine the EtherType in the Ethernet header to verify whether it understand and supports the encapsulated protocol.

If the protocol is not supported by the router it drops the frame

If the router supports the upper layer protocol such as IPv4,v6 or ARP it strips the Ethernet header and proceeds to examine the Layer 3 header

The first thing he checks in the Layer 3 header is the checksum, if the checksum is incorrect it drops the frame

If the checksum is ok, he proceeds to checks the destination IP and examine it's routing table to see if there is a best-matching record of where to send the packet with the given IP

If no record is found, the packet is dropped. If there is a record in a routing table, meaning that there is a next hop router address where he can send it.

It will now do a second routing table lookup to see if it can reach the next-hop - this is called recursive routing, where the router repeatedly performs routing table lookup until it finds the outoing interface to send the packet. After the router finds the outgoing interface in the routing table it proceeds to check it's arp table to determine the MAC address of the next hop (sends ARP request if there is no entry). The router proceeds to decrement the TTL in the IP header and recalculates the checksum to reflect the modifications in the IP packet. Now it will create new Layer 2 header, where he inserts the destination MAC as the MAC address of the next hop router and the source MAC address as his outgoing interface. The router also recalculate the FCS and update the ethernet trailer and encapsulates the entire IP packet into new data link frame and sends it out of the outgoing interface to the next hop router.

{% hint style="info" %}
Source and destination IP addresses are not modified by a router. Same idea as a switch not rewriting L2 source/dest MACs.
{% endhint %}

(there are only exceptions, where the router is configured to do so)

{% hint style="info" %}
Routers normally forward based on destination IP only. They can also match source IP using ACLs / PBR.
{% endhint %}

![IP Routing Topic Notes - The Bit-Bucket](<../.gitbook/assets/Unknown image (936)>)

Example

![](<../.gitbook/assets/Unknown image (937)>)

### IPv4 interface configuration

Ethernet interfaces: The term Ethernet interface refers to any type of Ethernet interface. For example, some Cisco routers have an Ethernet interface that is capable of only 10 Mbps, so to configure this type of interface, you would use the interface Ethernet interface-identifier configuration command. However, other routers have interfaces that are capable of operating up to 100 Mbps. These interfaces are referred to as Fast Ethernet ports. You use the interface FastEthernet interface-identifier command to configure these types of ports. Similarly, the interfaces that are capable of Gigabit Ethernet speeds are referenced with the interface GigabitEthernet interface-identifier command. The interfaces that are capable of operating up to 10 Gbps, 25 Gbps, 40 Gbps, and 100 Gbps Ethernet speed are referenced with the interface TenGigabitEthernet interface-identifier, interface TwentyFiveGigE interface-identifier, interface FortyGigabitEthernet interface-identifier, and interface HundredGigE interface-identifier commands, respectively.

Serial interfaces: Serial interfaces are the second major type of physical interfaces on Cisco routers. To support point-to-point leased lines and Frame Relay access-link standards, Cisco routers use serial interfaces. You can then choose which data link layer protocol to use, such as High-Level Data Link Control (HDLC) or PPP for leased lines or Frame Relay for Frame Relay connections, and configure the router to use the correct data link layer protocol. Use the interface serial interface-identifier command when configuring these types of interfaces.

Loopback Interface is a virtual configurable interface, that is always in up state. It is an interface like any other and can be assigned with any IP address.

Unlike physical network interfaces that can experience downtime due to hardware issues or disconnections, the loopback interface is always "up" and reachable as long as the device is in operation. This inherent reliability ensures that it remains available for internal communication and network tasks regardless of the status of physical interfaces

It is used by routing protocols to determine roles in the network topology or most commonly just as an IP device identifier that is used for management purpose - ssh/telnet

An IPv4 address with a mask of 255.255.255.255 (prefix /32, all bits set to binary 1) is called the host IPv4 address. The host IPv4 address indicates that only one IPv4 address is used in the subnet and is often used to address loopback interfaces.

![](<../.gitbook/assets/Unknown image (938)>)

Routers use interface identifiers to distinguish between interfaces of the same type. Depending on the model of the router, the interface-identifier may be:

An interface number, for example, interface Ethernet 1

A slot/interface number, for example, interface FastEthernet 0/1

A module/slot/interface number, for example, interface Serial 1/0/1

The router interface characteristics include, but are not limited to, the interface description, the IP address of the interface, the data link encapsulation method, the media type, the bandwidth, and the clock rate. You can enable many features on a per-interface basis.

When you first configure an interface, except in the setup mode, you must administratively enable the interface before the router can use it to transmit and receive packets. Use the no shutdown command to enable the interface.

| int Gi0/0                                                                                                                                          | enters interface config-level                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| -------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ip address                                                                                                                                         | associate IP address for given interface                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ip address secondary                                                                                                                               | secondary IP addresses can be used to accommodate multiple IP networks without requiring additional physical interfaces or subinterfaces When we think about a physical interface (and not considering any possible sub-interfaces) we generally assume that the interface will connect to a single subnet/network and will have a single IP address configured. But sometimes we might want to change that assumption and have the physical interface connect to more than one subnet/network. As explained by my colleagues that might be the case if the original subnet is small (perhaps the connected subnet is 10.10.10.0 255.255.255.248 (which gives you 6 usable addresses)) and you have more than 6 devices. By configuring a secondary address you make more addresses available through this interface. Or it might be the case that the existing subnet is 10.10.10.0/24 and you want to change the subnet to 172.16.1.0/24. By configuring the secondary address you make the interface function for both subnets at the same time, and when the address conversion is complete then you remove the secondary address and make the new subnet address be the primary address.                                                                                                                                                                                                           |
| Router(config)# interface Gigabitethernet0/0 Router(config-if)# ip address 1.1.1.1 255.255.255.255                                                 | assigning an IPv4 address to interface                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Router(config)# interface Gigabitethernet0/0 Router(config-if)# ip unnumbered Loopback0 interface Loopback0 ip address 192.168.1.1 255.255.255.255 | Unnumbered interfaces are designed for point-to-point transit links, which serve as interconnections for routing data without hosting any end devices (i.e., sources or destinations). These links do not require unique IP addresses because their primary purpose is to forward traffic between endpoints located off the link. On Cisco devices, if an interface is not assigned an IP address, IP protocol processing is deactivated on that interface, rendering it incapable of sending or receiving IP packets. This limitation also affects routing protocols, which rely on a source IP address to exchange routing updates. To address this, Cisco allows interfaces to borrow an IP address from another interface, typically a loopback or primary interface. This approach enables IP protocol processing and routing protocol functionality without the need to assign a unique IP address to the transit interface. Unnumbered interfaces offer several advantages, including conservation of IP address space, especially in environments with limited IPv4 availability, and simplified configuration, as network operators can manage fewer IP addresses. Additionally, these interfaces maintain operational efficiency by behaving as they have an IP address for routing purposes without consuming an actual unique address Used manily for transit WAN interfaces or GRE tunnels |

![](<../.gitbook/assets/Unknown image (939)>)

| **Output Field** | **Description**                                                                          |
| ---------------- | ---------------------------------------------------------------------------------------- |
| Interface        | Type of specific interface                                                               |
| IP Address       | IPv4 address that is assigned to the interface.                                          |
| OK?              | "Yes" means that the IPv4 address is valid; "No" means that the IPv4 address is invalid. |
| Method           | Describes how the IPv4 address was obtained or configured.                               |
| Status           | Indicates the physical layer status of the interface.                                    |
| Protocol         | Indicates the data link layer status of the interface.                                   |

RouterX# show interfaces

GigabitEthernet0/0 is up, line protocol is up

Each of the command outputs shown in the previous examples lists two interface status codes. For a router to use an interface, the two interface status codes on the interface must be in the up state. The first status code refers to whether the physical layer (Layer 1) is working, and the second status code mainly (but not always) refers to whether the data link layer (Layer 2) protocol is working.

| **Output**                                                                               | **Description**                                                                                                                                                                                                                        |
| ---------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| GigabitEthernet...is {up \| down \| administratively down} Line protocol is {up \| down} | Indicates whether the interface hardware is currently active, down, or if an administrator has taken it down. Please refer to the Troubleshooting Status Codes table for an explanation of all the combinations of these two statuses. |
| Hardware                                                                                 | Displays the hardware type and MAC address.                                                                                                                                                                                            |
| Description                                                                              | Displays the configured interface description.                                                                                                                                                                                         |
| Internet address                                                                         | Displays the IPv4 address followed by the prefix length (subnet mask).                                                                                                                                                                 |
| MTU                                                                                      | Displays the maximum transmission unit (MTU) of the interface.                                                                                                                                                                         |
| BW                                                                                       | Shows the bandwidth of the interface in kilobits per second. The bandwidth parameter is used to compute routing protocol metrics and other calculations.                                                                               |
| DLY                                                                                      | Shows the delay of the interface in microseconds. This parameter is used to compute routing protocol metrics and other calculations.                                                                                                   |
| Rely                                                                                     | Displays the reliability of the interface as a fraction of 255 (255/255 is 100% reliability). This parameter is used to compute routing protocol metrics and other calculations.                                                       |
| Load                                                                                     | Displays the load on the interface as a fraction of 255 (255/255 is completely saturated). This parameter is used to compute routing protocol metrics and other calculations.                                                          |
| Encapsulation                                                                            | Shows the encapsulation method that is used on the interface.                                                                                                                                                                          |
| 5-minute input rate, 5-minute output rate                                                | Shows the average number of bits and packets that the interface transmitted per second in the last 5 minutes.                                                                                                                          |

### Routing table (RIB)

**Routing table (RIB)** contains a list of IP addresses that represents host or subnets/network prefixes, each associated with an outgoing interface (next hop)

Routing table entries are populated either with manual configuration (statically) or learned from routing protocol (dynamically), sorted sequentially according to the prefix lengths so that all networks with the longest prefix length are always at the top up until to the least specific such as default route (/32 first /0 last)

Bidirectional routing - refers to that both network devices connecting the source host and also device connecting the destination host must have a routing entry in their routing table to know how to reach other

The forwarding decision is a function of the FIB and results from the calculations performed in the RIB. The RIB is calculated from combination of AD and routing protocol metrics

There are four types of entries, depending on the source and role:

Directly connected networks

Dynamic routes

Static routes

Default routes

The entries contain the following information:

**Route source**: Identifies how the route was learned. Directly connected interfaces have two route source codes. "C" identifies a directly connected network. "L" identifies the local IPv4 address assigned to the router’s interface.

**Destination network**: For directly connected networks, the destination networks are local to the router. The destination network address is indicated with a network address and subnet mask in the form of the prefix. Note that "L" entries, which identify the local IPv4 address of the interface, have a prefix of /32.

**Outgoing interface**: Identifies the exit interface to use when forwarding packets to the destination network.

Directly connected route

All directly connected networks are added to the routing table automatically. A newly deployed router, without any configured interfaces, has an empty routing table. The directly connected routes are added after you assign a valid IP address to the router interface, enable it with the no shutdown command, and when it receives a carrier signal from another device (router, switch, end device, and so on). In other words, when the interface status is up/up, the network of that interface is added to the routing table as a directly connected network. If the hardware fails or is administratively shut down, the entry for that network is removed from the routing table. The following figure shows examples of routing table entries for directly connected networks. An active, properly configured, directly connected interface, creates two routing table entries. The following figure displays the IPv4 routing table entries on R1 for the directly connected network 10.1.1.0/24.

Directly-Connected route (C) to the network prefix of the interface (/xx) and a Local route (L) for the exact IP address of the interface, with a /32 netmask

{% hint style="info" %}
The router removes both routing entries if the interface goes down.
{% endhint %}

![](<../.gitbook/assets/Unknown image (940)>)

{% hint style="info" %}
In IPv6, routers do not create routing table entries when an interface is configured with only a link-local address.
{% endhint %}

| show \[ip\|ipv6] route | to display the RIB                                   |
| ---------------------- | ---------------------------------------------------- |
| show ip route vrf \*   | to perform RIB lookup in all VRF's for a specific IP |
| show ip route \| i 10. | to filter only 10.x.x.x networks in the RIB          |

### Static routes

Static routes are entries that you manually enter directly into the configuration of the router. Static routes are not automatically updated and must be manually reconfigured if the network topology changes. Static routes can be effective for small, simple networks that do not change frequently. The benefits of using static routes include improved security and resource efficiency. The main disadvantage of using static routes is the lack of automatic reconfiguration if the network topology changes. There are two common types of static routes in the routing table—static routes to a specific network and the default static route.

It is manually specified route appropriate when the software cannot dynamically build a route to the destination or we want a to achieve specific routing

Static route takes into account that you know what you are doing, so it has the lowest administrative distance of 1

By default, static routes are preferred to routes learned by routing protocols. You can configure an administrative distance with a static route if you want the static route to be overridden by dynamic routes.

![](<../.gitbook/assets/Unknown image (941)>)

Directly-Connected static route is a route in which you specify the exit interface, but not the next hop IP

The router treat this static route as directly connected network, instead of sending it to the next-hop router.

If the exit interface goes down, the route becomes invalid and will be withdrawn from the RIB

![](<../.gitbook/assets/Unknown image (942)>)

Fully specified static route is route with specified next hop IP and exit interface

![](<../.gitbook/assets/Unknown image (943)>)

| ip route                                                           | IOS-XE you can also use keyword name to add a description of purpose of the static route |
| ------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| ipv6 route ipv6 \<address/prefix-length> \[ \| ]                   |                                                                                          |
| ipv6 route 2001:1234:A:2::/64 GigabitEthernet 0/0 2001:1234:A:B::2 | Fully specified static route example                                                     |
| router static address-family ipv4 unicast 192.168.0.0/16           | IOS-XR static route                                                                      |

{% hint style="info" %}
Always use a fully specified static route.
{% endhint %}

If the next-hop interface specified in the static route is not in an "up" state, the router will remove the static route from its RIB.

When you specify only the outbound interface in a static route, the router needs to perform additional an ARP lookup to determine the L2 MAC address associated with the next-hop interface, leading to forwarding failures if the remote end of the interface is unreachable

#### Floating static route

is a static route intentionally configured with a higher AD than the primary static route, serving as a backup in case the primary becomes unreachable

| ip route 1.1.1.1 FastEthernet0/0 192.168.0.2 210            |                                                                     |
| ----------------------------------------------------------- | ------------------------------------------------------------------- |
| ipv6 route 2001:A:2::/64 GigabitEthernet0/0 2001:A:B::2 210 | LLA can be used as a next-hop aswell, however it is not recommended |

### Dynamic routes

Routers use dynamic routing protocols to share information about the reachability and status of remote networks. A dynamic routing protocol allows routers to learn about remote networks from other routers automatically. These networks, and the best path to each, are added to the router's routing table and identified as a network learned by a specific dynamic routing protocol. Cisco routers can support a variety of dynamic IPv4 and IPv6 routing protocols, such as Border Gateway Protocol (BGP), Open Shortest Path First (OSPF), Enhanced Interior Gateway Routing Protocol (EIGRP), Intermediate System-to-Intermediate System (IS-IS), Routing Information Protocol (RIP), and so on. The routing information is updated when changes in the network occur. Larger networks require dynamic routing because there are usually many subnets and constant changes. These changes require updates to routing tables across all routers in the network to prevent connectivity loss. Dynamic routing protocols ensure that the routing table is automatically updated to reflect network changes. The following figure displays an IPv4 routing table entry on R1 for the route to remote network 172.16.1.0/24.

From the example entry, you can tell the following:

**Route source**: Identifies how the route was learned. "O" in the figure indicates that the source of the entry was the OSPF dynamic routing protocol.

**Destination network**: Identifies the address of the remote network. The router knows how to reach 172.16.1.0/24 network.

**Administrative distance**: Identifies the trustworthiness of the route source. Lower values indicate the preferred route source. OSPF has a default administrative distance value of 110.

**Metric**: Identifies the value assigned to reach the remote network. Lower values indicate preferred routes. This OSPF route has a metric of 2 for the destination network 172.16.1.0/24.

**Next-hop**: Identifies the IPv4 address of the next router to forward the packet to. The IPv4 address of the next-hop is 192.168.10.2.

**Route time stamp**: Identifies how much time has passed since the route was learned. The information in the example entry was learned 3 minutes and 23 seconds ago.

**Outgoing interface**: Identifies the exit interface to use to forward a packet toward the final destination. The packets destined to the 172.16.1.0/24 network will be forwarded out of the GigabitEthernet 0/1 interface.

![](<../.gitbook/assets/Unknown image (944)>)

#### Recursive routes

routing entry where the next hop IP address is not directly connected to the router but instead, it is reachable by recursively looking up to other entries in the routing table until a valid next hop is found

![](<../.gitbook/assets/Unknown image (945)>)

### Default route

A default route is an optional entry used by the router if a packet does not match any other, a more specific route in the routing table. A default route can be dynamically learned or statically configured. More than one source provides the default route, but the selected default route is presented in the routing table as Gateway of last resort.

it is either dynamically or statically created route, that serves as a default next-hop for all packet with a destination address that doesn't have any specific match entry in the RIB

Every default route is flagged as "\* - candidate default" - this is to indicate that the default route is considered as candidate and is used if no other "better" default route is present in RIB

Default-gateway (Gateway of last resort)

device that receives and forwards all default route traffic received from endpoints in one of it's connected networks

The AND result is not all 0s, so the source host knows the destination is in a different subnet. The source host will send the packet to its default gateway

![](<../.gitbook/assets/Unknown image (946)>)

| ip route 0.0.0.0 0.0.0.0       |                                                                                                                                                                                          |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ip default-network 192.168.1.0 | is a classful command that specifies the network that serves as default gateway for all default gateway traffic - involves recursive lookup to find the next hop for the default network |
| ip default-gateway 172.16.15.4 | differs from the other two commands as it must only be used when ip routing is disabled on the Cisco router - it acts as a host                                                          |
| ipv6 route ::/0 \[ \| ] \[]    |                                                                                                                                                                                          |

The letters are:

C: Indicates directly connected networks; the first and seventh entries are directly connected networks.

L: Indicates local interfaces within connected networks; the second and eighth entries are local interfaces.

R: Indicates RIP; the third entry is the RIP route.

O: Indicates OSPF; the fourth entry is an OSPF route.

D: Indicates EIGRP; the fifth entry is an EIGRP route. The letter D stands for Diffusing Update Algorithm (DUAL), which is the update algorithm that EIGRP uses. The code letter E was previously taken by the legacy Exterior Gateway Protocol (EGP).

S: Indicates static routes; the sixth and ninth entries are static routes.

Asterisk (\*): Indicates that this static route is a candidate for the default route.

RouterA#\*\* show ip route\*\*Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2 E1 - OSPF external type 1, E2 - OSPF external type 2 i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2 ia - IS-IS inter area, \* - candidate default, U - per-user static route o - ODR, P - periodic downloaded static route, + - replicated route

Gateway of last resort is 10.1.1.1 to network 0.0.0.0

C 10.1.1.0/24 is directly connected, GigabitEthernet0/0L 10.1.1.2/32 is directly connected, GigabitEthernet0/0R 172.16.0.0/16 \[120/1] via 192.168.10.2, 00:01:08, GigabitEthernet0/1O 172.16.1.0/24 \[110/2] via 192.168.10.2, 00:03:23, GigabitEthernet0/1D 192.168.20.0/24 \[90/156160] via 10.1.1.1, 00:01:23, GigabitEthernet0/0S 192.168.30.0/24 \[1/0] via 192.168.10.2C 192.168.10.0/24 is directly connected, GigabitEthernet0/1L 192.168.10.1/32 is directly connected, GigabitEthernet0/1S\* 0.0.0.0/0 \[1/0] via 10.1.1.1

### Path selection

Determining the best path involves evaluating multiple paths to the same destination network and selecting the optimum path to reach that network. When you statically configure a route, then you determine what the best path to the network is. But when dynamic routing protocols are used, the best path is selected by a routing protocol based on the quantitative value called a metric. A metric is a quantitative value used to measure how to get to a given network. A dynamic routing protocol's best path to a network is the path with the lowest metric.

Dynamic routing protocols typically use their own rules and metrics. The routing algorithm calculates a metric for each path to the destination network. Metrics can be based on either a single characteristic, such as bandwidth, or several characteristics of a path, such as bandwidth, delay, and reliability. Some routing protocols can base route selection on multiple metrics, combining them into a single metric.

#### Longest-prefix match (LPM)

this is the first criterium that has the highest influence on the path selection - routes with longest - e.g most specific mask/prefix length are always preferred and used for packet forwarding

{% hint style="info" %}
Routes to the same destination with different prefix lengths are still all installed into the RIB (for example `10.0.3.0/24`, `/25`, `/26`, `/27`).
{% endhint %}

If the there are two routes for the same network with same prefix length, the second tie-breaking criterium is considered, which is Administrative distance (AD)

If the AD matches as well, the third tie-breakers Metric of the routing protocol is used to determine the best path

#### Administrative Distance (AD)

is a value that is assigned to every route and each routing protocol, determining its priority/quality

It is used as a tie breaker, if router receives route to the network with same prefix, then the AD is compared and the routing entry with the lowest AD is installed into the RIB

Adjusting AD to influence routing decisions is not scalable/recommended technique.

AD can be specified and applied to the routing process or completely rewritten in global config or adjusted as part of route-map

Administrative distance can be adjusted under the (config-router)# level with a keyword #distance , where you can specify AD for entire protocol or for any specific routes

For example, the BGP command distance 44 55 66 sets the AD for eBGP routes to 44, the AD for iBGP routes to 55, and the AD for locally learned routes to 66

{% hint style="info" %}
Applies only to routes received after the command is entered (similar to filters).
{% endhint %}

Directly connected networks have an administrative distance of 0 and preempt all other entries for that destination network. Only a directly connected route can have an administrative distance of 0 and the administrative distance of 0 cannot be modified for directly connected networks.

| Route Source                                          | Default Administrative Distance        |
| ----------------------------------------------------- | -------------------------------------- |
| Connected interface (and static routes via interface) | 0                                      |
| Static route (via next hop address)                   | 1                                      |
| External Border Gateway Protocol (EBGP)               | 20                                     |
| EIGRP                                                 | 90                                     |
| OSPF                                                  | 110                                    |
| IS-IS                                                 | 115                                    |
| RIP                                                   | 120                                    |
| External EIGRP                                        | 170                                    |
| Internal Border Gateway Protocol (IBGP)               | 200                                    |
| Unreachable                                           | 255 (will not be used to pass traffic) |

![](<../.gitbook/assets/Unknown image (947)>)

If the AD of an existing route in the RIB is lower than the AD of a new received route , the new route is rejected

If the AD of the existing route is higher, the new route is accepted and the current source protocol is notified of the removal of the existing entry from the RIB

#### Metric

Each routing protocol use their metric to determine the best path to the destination. For OSPF its link cost, for RIP hop count, for EIGRP delay, BW, reliability, load, MTU

If you don't specify a cost metric, each routing protocol will use the default cost based on the bandwidth of the link

Cumulative cost refers to the total cost metric of a given route along a path in a routing protocol

Each routing entry displays the network followed by \[AD/Metric] and the next-hop IP, the lifetime of an entry and outgoing interface as shown on below screenshot

### Routing protocols (overview)

With the increasing size and complexity of networks, static configuration of routing tables has become impractical.

To force routers to automatically exchange routing data and add routes to each other's routing tables dynamically, we can use something called a routing protocol, which allows routers to tell each other what networks they are connected to or how to reach them through other routers.

Routing protocols calculate the best paths for data packets based on various factors such as link cost, bandwidth, delay, and congestion. By selecting the most efficient routes, routing protocols help optimize network performance and improve overall throughput

Routing protocols adapt to changes in network conditions, such as link failures or the addition of new devices

They dynamically update routing tables to reroute traffic along alternative paths, ensuring continuous network connectivity and minimizing downtime

Routing protocols support redundancy and fault tolerance by maintaining multiple paths to reach a destination. If one path becomes unavailable due to a link failure or congestion, routers can quickly switch to an alternate path without disrupting network operations

Networks can be injected into routing protocol either with network statement #network 192.168.0.0 or by enabling routing process directly under interface

{% hint style="info" %}
“Routed protocols” means the protocol being routed. For example IPv4 or IPv6.
{% endhint %}

![](<../.gitbook/assets/Unknown image (948)>)

![](<../.gitbook/assets/Unknown image (949)>)

Classless routing protocol: RIP version 2 (RIPv2), EIGRP, OSPF, IS-IS, and BGP are classless routing protocols and can be considered second-generation protocols because they are designed to address the limitations of classful routing protocols. A classless routing protocol is a protocol that advertises subnet mask information in the routing updates for the networks advertised to neighbors. As a result, this feature enables the protocols to support discontiguous networks (where subnets of the same major network are separated by a different major network) and Variable Length Subnet Masking (VLSM). This allows the routers to exchange routing information for subnets (such as 10.1.1.0/24) as well as for major networks (for example, 10.0.0.0/8). In the following figure, when routers R1 and R3 send routing advertisements to router R2, they include the subnet mask in the updates (10.1.1.0/24 and 10.2.2.0/24), so R2 learns about those specific subnets.

Classful routing protocol: Classful routing protocols such as RIP version 1 (RIPv1) and Interior Gateway Routing Protocol (IGRP) are legacy protocols and not used today. They do not advertise the subnet mask information within the routing updates. Therefore, only one subnet mask can be used within a major network. VLSM and discontiguous networks are not supported.

Classful routing protocols do not support manual route summarization and perform only autosummarization.

#### IGP (Interior Gateway Protocol)

exchange routing information within a single AS (RIP,RIPv2,OSPF,EIGRP,IS-IS or internal BGP)

Autonomous system (AS) is a network that is under common network administration = network devices share common routing policies = one organization

EGP simply allow us routing between Autonomous systems using their internal IGP.

EGP allows routers to include, or summarize, several networks that their IGP contains

Once a packet reaches a network with an IGP, it is then forwarded via more specific paths

#### EGP (Exterior Gateway Protocol)

exchange routing information between autonomous systems (BGP)

![](<../.gitbook/assets/Unknown image (950)>)

### Distance-vector routing (RIP/IGRP)

routers with distance vector protocol enabled periodically sending their entire routing table ONLY to their connected neighbors

Each router participating in a Distance Vector algorithm performs a routing calculation based on the current information received from the router’s neighbors

Distance vector knows about the other networks and their respective next-hop router, but they don't know the entire topology of the network

The router then sends the results (e.g modifies the update) of his calculation (which is typically the router’s current routing table) ONLY to all of its neighbors, causing them to redo their calculations based on this updated information

The neighbors in turn send their updated routing tables to their neighbors, and so on. This process then iterates until all routers converge on the best paths to the destination networks.

This is also referred to as "routing by rumor", since each router rely on the information received from their neighbors

The Distance is the next hop router and the Metric is the hop count, bandwidth of the link, delay, reliability or load of the link

The router maintains all known routes in a table in the form of ordered triples (N, G, D), where:

* N is the destination network
* G is the address of the next router (through which the data will be sent to the destination network)
* D is the distance to the destination network (a metric, e.g. hop count)

Since distance vector protocols rely on routers exchanging routing information with their neighbors

This information may take time to propagate through the network, leading to delays in detecting topology changes

Disadvantages: Susceptible to routing loops, Slow convergence, Broadcasts updates, Uses more bandwidth as they send periodic updates containing entire routing table

Advantages: Simpler to configure, Lower CPU and memory requirements as they don't maintain the entire network topology

Count to infinity problem of Distance Vector routing protocols

Routers A, B, and C are interconnected.

Router C has a direct connection to 10.4.0.0.

Each router maintains a routing table with the shortest hop count to reach each destination.

The 10.4.0.0 network becomes unreachable from Router C.

Router C removes the route to 10.4.0.0 from its table.

Router B, which had learned about 10.4.0.0 from Router C, still believes the network is reachable (via C).

Router B advertises that it can reach 10.4.0.0 with a hop count of 1.

Router C, upon receiving this advertisement, assumes that 10.4.0.0 is reachable through B and updates its table with a hop count of 2.

Router A learns from B that 10.4.0.0 is reachable via B, with a cost of 2.

Router B then updates its hop count to 3 (since it now sees 10.4.0.0 through A).

This process continues, with the hop count increasing at each step, creating a loop.

This process continues until the hop count reaches infinity (or the RIP maximum of 16 hops, at which point the route is declared unreachable).

To avoid the problem of infinite metric growth associated with routing loops, a maximum metric value is defined. In the case of the RIP protocol, the maximum number of routers along the path from a given node to the destination network is determined.

The Solution is integrated split horizon and other mechanisms are described below

![](<../.gitbook/assets/Unknown image (951)>)

![](<../.gitbook/assets/Unknown image (952)>)

#### Routing loop prevention mechanisms

#### **Split horizon**

is the main loop prevention mechanism built-in routing protocols to prevent a router from advertising a route back to the interface from which it learned that route

NBMA networks don’t support broadcast and multicast natively, which can cause issues for dynamic routing protocols like RIP, EIGRP, and OSPF.

By using point-to-point subinterfaces, each virtual link is treated as a separate interface, effectively eliminating Split Horizon issues and preventing loops.

Imagine you have a hub-and-spoke topology with a single physical interface on the hub router (R1) connecting to two spokes (R2, R3) via Frame Relay.

Problem Without Subinterfaces

Spokes (R2, R3) send updates to the hub (R1).

Due to Split Horizon, R1 does not forward R2’s route to R3 and vice versa.

R2 and R3 can’t communicate, even though they both connect to R1.

#### Route poisoning

is a method used to prevent routers from sending packets through a route that has been deemed invalid. Upon failure of a route, Distance Vector Protocols spread the update about the route failure by poisoning the route. In Route Poisoning, a special metric value called Infinity is used when advertising the route. Routers with a metric of Infinity are considered to have failed. The main disadvantage of this method is that it increases the sizes of routing announcements significantly in many common network topologies

Unreachable Message sent by a router to its neighboring routers to indicate that a particular route or network is no longer reachable

#### Poison Reverse

when a router detects that a route has become unreachable, it advertises the route back to its neighboring routers with an infinite (unreachable) metric value

The maximum metric value for RIP is 16, which indicates an unreachable network for EIGRP is 4,294,967,295 (2^32-1) for OSPF is 16,777,215 (2^24-1)

#### Hold-down Timer

When a router detects that a route is no longer reachable, it activates the hold-down timer for that route.

During this period, the router temporarily suppresses updates about the affected route, ignoring any alternative path advertisements unless they come from the original failed link.

This mechanism helps stabilize the network by preventing the router from prematurely accepting potentially unreliable updates from neighboring routers. Once the hold-down timer expires, the route is either reinstated if the original link is restored or removed from the routing table if it remains unreachable

Triggered update other routers are informed immediately about a change in routing information

#### Routing convergence

refers to the time it takes for all routers or devices in a network to come to a consistent understanding of the network's topology and routing information after a network change

When a link or a device fails, initially only the neighboring devices are aware of the failure. All other devices in the network are unaware of the nature and location of this failure until information about this failure is propagated through the routing protocol. The propagation of this information may take several hundred milliseconds. Meanwhile, packets affected by the network failure need to be steered to their destinations. A device adjacent to the failed link employs a set of repair paths for packets that would have used the failed link.

These repair paths are used from the time the router detects the failure until the routing transition is complete

By the time the routing transition is complete, all devices in the network revise their forwarding data and the failed link is eliminated from the routing computation

Per-link (link-based) computation used to determine the link protection status of individual links, whether they have backup/repair path available

Per-prefix (prefix-based) computation used to group links based on their prefix and determine the best backup path for each prefix

### Routing Information Protocol (RIP)

one of the first invented routing protocols, that is a simple and easy-to-use distance-vector protocol suitable for small networks

RIP uses a metric called hop count to determine the best path to a network. A hop count is the number of routers a packet must pass through to reach its destination

The metric value for the path is increased by 1, and the sender is indicated as the next hop

RIP is limited to a maximum of 15 hops, which means it can only be used effectively in small networks.

It has mechanism such as Hold down, Split horizon, Poison reverse and Triggered update to prevent routing loops and announce unreachable networks to the neighbors ato prevent the spread of incorrect information that can cause routing loops or announcing unreachable networks, creating blackhole

RIP is able to originate a default route out of a given interface

As a routing loop prevention mechanism, RIP ignores all default routes received on any interface

Options for default route origination:

Originate only the default route and suppress all other routes

Originate "::/0" in addition to other routes

When announcing routes, RIP does not differentiate from internal and redistributed (external) routes.

To mark the routes and distinguish them one from another, we can use the route tag

Advantages: simple configuration, supported by most of the manufacturers

Disadvantages: transfer a lot of routing information, limited metric, slow convergence

RIPv1 routing updates (entire routing table) are sent as broadcast every 30 second encapsulated in UDP port 520

RIPv2 updates (entire routing table) are sent to multicast 224.0.0.9 on UDP port 520. It supports VLSM and authentication

If a device does not receive an update from another device for 180 seconds or more, the receiving device marks the routes served by the nonupdating device as unusable

If there is still no update after 240 seconds, the device removes all routing table entries for the nonupdating device

| (config)#router rip                              | Enable RIP routing process                                                                                                                                                                                                                                                                          |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-router)#version 2                        | Specify RIP version 2                                                                                                                                                                                                                                                                               |
| (config-router)#network 192.168.1.0              | Advertise subnet on Fa0/0                                                                                                                                                                                                                                                                           |
| (config-router)#network 10.0.0.0                 | Advertise subnet 10.0.0.0                                                                                                                                                                                                                                                                           |
| (config-router)#neighbor 10.10.10.1              | Specify neighbors for RIP updates exchange (limits updates to specific routers)                                                                                                                                                                                                                     |
| (config-router)#no auto-summary                  | eliminates default auto summary behavior                                                                                                                                                                                                                                                            |
| show ip rip \[neighbors \| database]             |                                                                                                                                                                                                                                                                                                     |
| (config-if)# ip rip authentication key-chain kal | requires preconfigured [key chain](https://onenote/#OSPv2%20Cont\&section-id={6EC9C333-3E04-41C1-8C01-0E8D85C128A8}\&page-id={475A8F40-BA6A-43D9-9C98-CC5A77AE93A2}\&object-id={708B7DF5-831E-05CD-1EC7-C4093996CAFC}&57\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE/L3.one) |

#### RIPng (IPv6)

An extension of RIPv2 for support of IPv6

RIPng sends updates on UDP port 521 using the multicast group FF02::9

RIPng supports load balancing across multiple paths

RIP can simultaneously use four paths to load-balance traffic

It relies on CEF to perform load balancing

RIPng supports up to 64 configurable paths (default is 4); on hardware-based platforms, limitations come from the hardware used

| (config)# ipv6 unicast-routing                          | enables IPv6 routing                                               |
| ------------------------------------------------------- | ------------------------------------------------------------------ |
| (config)# ipv6 router rip                               | enables RIPng routing process and moves to it's configuration mode |
| (config-if)# ipv6 rip enable                            | RIPng is enabled under interface - no network command              |
| (config-if)# ipv6 rip tag default-information originate | Originates the default route (::/0) from an interface              |
| show ipv6 rip \[database]                               | shows information about RIP                                        |
| show ipv6 route rip                                     | shows routers learned from RIP neighbors                           |

### Link-state routing (OSPF / IS-IS)

A router that runs a link-state routing protocol must first establish a neighbor adjacency with its neighboring routers. A router achieves this neighbor adjacency by exchanging hello packets with the neighboring routers. After neighbor adjacency is established, the neighbor is put into the neighbor database.

After a neighbor relationship is established between routers, the routers synchronize their LSDBs (also known as topology databases or topology tables) by reliably exchanging link-state advertisements (LSAs). An LSA describes a router and the networks that are connected to the router. LSAs are stored in the LSDB.

LSA's are FLOODED TO ALL routers in the link-state network

By exchanging all LSAs, routers learn the complete topology of the network. Each router will have the same topology database within an area, which is a logical collection of OSPF networks, routers, and links that have the same area identification within the autonomous system.

After the topology database is built, each router applies the SPF algorithm to the LSDB in that area. The SPF algorithm uses the Dijkstra algorithm to calculate the best (also called the shortest) path to each destination.

The best paths to destinations are then offered to the routing table. The routing table includes a destination network and the next-hop IP address. In the example, the routing table on router A states that a packet should be sent to router D to reach network X.

The routers don't modify the LSAs, each router makes decisions to use certain route from their own perspective (in contrast to distance vector protocols)

The division of networks into areas is a fundamental feature of OSPF, enabling greater scalability. In contrast, routing protocols like EIGRP treat networks as a "flat" structure, where any change in topology can eventually impact routing decisions across the entire network. This effect can be mitigated by manually summarizing routes, effectively segmenting the network to improve efficiency and stability

The state of the link is a description of that interface and of its relationship to its neighboring routers. A description of the interface would include, for example, the IP address of the interface, the subnet mask, the type of network to which it is connected, the routers that are connected to that network, and so on. The collection of all these link states forms an LSDB. All routers in the same area share the same LSDB. Routers in other OSPF areas will have different LSDBs.

Disadvantages: Higher CPU and memory requirements

Advantages:

They are scalable: Link-state protocols use a hierarchical design and can scale to very large networks, if properly designed.

Each router has a full map of the topology: Because each router contains full information about the routers and links in a network, each router is able to independently select a loop-free and efficient pathway, which is based on cost, to reach every neighbor in the network.

Updates are sent when a topology change occurs and are reflooded periodically: Link-state protocols send updates of a topology change by using triggered updates. Also, updates are sent periodically—by default every 30 minutes.

They respond quickly to topology changes: Link-state protocols establish neighbor relationships with the adjacent routers. The failure of a neighbor is detected quickly, and this failure is communicated by using triggered updates to all routers in the network. This immediate reporting generally leads to fast convergence times.

More information is communicated between routers: Routers that run a link-state protocol have a common view on the network. Each router has full information about other routers and links between them, including the metric on each link.

![](<../.gitbook/assets/Unknown image (953)>)

### Hybrid routing (EIGRP)

is a combination of distance-vector routing such as performing calculations based on the calculation from its neighbors, and link-state such as maintaining topology table

### Path-vector routing (BGP)

the EGP is for example BGP, which is similar to the distance vector. It doesn't select the best path according to the link properties, but instead it uses path attributes to provide greater control over the path selection

Guarantees loop-free paths by keeping a record of each AS that the routing advertisement traverses

| show ip protocols | to check active protocols running on a router, use ipv6 to check ipv6 |
| ----------------- | --------------------------------------------------------------------- |

### Type-Length-Value (TLV)

is a format which is used to encode optional information without changing the existing structure. It is employed in many other network protocols such as BGP, IS-IS,OSPF, SNMP,.1X to prepend additional information to allow a extended functionality

Receivers can quickly parse and skip unknown TLVs, improving efficiency

**Type (T)** Indicates the kind of information being encoded (fixed length 1-2bytes)

**Length (L)** Specifies the length of the Value field (fixed length 1-2bytes)

**Value (V)** Contains the actual data being transmitted. A variable-length field, as specified by the Length

![](<../.gitbook/assets/Unknown image (954)>)

### Load balancing

Depending on a routing protocol, if it identifies multiple paths as a best path, it installs both to it's routing table and load-balance traffic between them

**Difference between Load splitting and Load balancing**

**Load splitting** refers to the distribution of traffic or workload across multiple paths or resources without necessarily considering the current load or capacity of each path or resource

**Load balancing** is a specific type of load distribution that aims to optimize resource utilization by considering the current load, capacity, or performance of each path or resource

**Equal-Cost Load-balancing** the traffic is distributed equally among the paths with the equal metric

![](<../.gitbook/assets/Unknown image (877)>)

**Unequal-Cost Load Balancing** traffic is distributed across the multiple routes to the same destination based in ratio to the metric of each route (This type of load balancing is EIGRP-specific). The router sends more traffic through the path with a lower metric and proportionally less traffic through paths with higher metrics

![](<../.gitbook/assets/Unknown image (878)>)

![](<../.gitbook/assets/Unknown image (879)>)

First route forwards 120 packets and second 71

| show ip cef internal    | displays the load balancing flow pattern with the next hop and other details |
| ----------------------- | ---------------------------------------------------------------------------- |
| show ip cef exact-route | router perform theoretical traffic flow                                      |
| show ip route           | displays share count                                                         |

![](<../.gitbook/assets/Unknown image (880)>)

Load Balancing from traceroute perspective

![](<../.gitbook/assets/Unknown image (881)>)

![](<../.gitbook/assets/Unknown image (882)>)

Dangerous assumptions in networking #3: Multiple links are always better

It is easy to assume that two Internet links will make a transfer faster than one. More bandwidth should mean more performance.

Now, imagine a router using two upstream links and sending packets from the same TCP flow across both paths. At first glance, this looks efficient. Both links are active, both carry traffic, and the transfer appears to use more of the available capacity.

Now consider what happens on the receiver side.\
Packet #1 arrives first, so everything is normal. The receiver acknowledges it and waits for packet #2. But packet #2 takes the other path and is delayed, since ISP2 has a higher latency than ISP1. In the meantime, packet #3 arrives.

The receiver now has a gap in the byte stream. It has already received later data, but it is still missing the data carried by packet #2. So it cannot move its cumulative acknowledgment forward. Instead, it keeps sending the same ACK again, still indicating that packet #2 has not been received.

From the sender side, these repeated ACKs look exactly like a sign of packet loss. The sender assumes the network is congested. To protect the network, it begins to reduce its TCP transmission window.\
The result? The sender slows down the transfer rate, even though the bandwidth is available and no packets were actually lost. The transfer crawls simply because the packets arrived out of order.

That is why most routers prefer per-flow load balancing instead of per-packet load balancing. It keeps all packets from the same session on the same path and preserves packet order within each flow.

Multiple links are necesssary for resilience and for increasing total capacity across many simultaneous sessions. They just do not automatically make a single transfer faster.

Generally, routers want to guarantee that packets belonging to a given TCP connection always travel the same path. Reordering the TCP packets would reduce TCP performance and increase CPU cycles if done in software. For this reason, routers use a hash function of some TCP connection identifiers ( source and destination IP address ) to choose among the multiple next hops. A TCP connection is identified by a 5-tuple, which refers to a set of five values that comprise a TCP/IP connection.

It includes a source IP address/port range, destination IP address/port number, and the protocol in use. A router can load on any of these. In addition, recent availing technologies let L2 load balance ( ECMP ), such as THRILL and Cisco FabricPath, allow you to build massive data center topologies with Layer 2 multipathing



### Route summarization (aggregation)

refers to the process of combining multiple smaller network prefixes into a single, larger summarized route. This technique improves network scalability and efficiency by reducing routing table size and reducing the number of updates exchanged between routers. By advertising a single summary instead of multiple specific prefixes, summarization decreases control-plane load and limits the propagation of routing changes beyond the summarization boundary—keeping network instability localized.

**All benefits**

Reduces memory usage by shrinking routing tables.

Decreases bandwidth consumption since fewer routes are advertised.

Limits the propagation of routing instability by suppressing unnecessary updates beyond the summarization boundary.

In EIGRP, reduces query scope since summarization can be configured on any router, unlike OSPF which summarizes only at area boundaries or ASBRs.

{% hint style="info" %}
The term aggregation is also used for: Aggregation routers represent a consolidation point between the access layer (where end users are connected) and the backbone network (CORE). Their primary function is to effectively collect thousands of individual data streams from many access devices (such as dslams or Olty) into a smaller number of high -speed connections. These routers thus simplify the topology of the network, perform basic routing and priority of operation (QoS) and ensure that expensive routers in the network core are burdened only by consolidated, organized operation
{% endhint %}

Example

In the event of a link flap on the 10.13.1.0/24 network, R3 removes all the AS 65100 routes learned directly from R1 and identifies the same network prefixes via R2

R3 has to advertise new routes to R4 because of these flaps, which is a waste of CPU cycles because R4 only receives connectivity from R3.

If R3 summarized the network prefix range to 10.13.0.0/12, R4 would execute the best-path algorithm only once for both available links via R2 and R1 received from R3

![](<../.gitbook/assets/Unknown image (883)>)

#### Possible Issues - Suboptimal or blackhole traffic

Summarization must be considered wisely to avoid summarizing subnets that the advertising router is not delegated to, otherwise routers may have send the traffic to him even when the router doesn't have a specific route to them

Process

The summary mask is determined by converting all subnets for summarization into binary and determine how many bits in the network part they have in common. Example:

The block of addresses from 172.16.8.0 through 172.16.15.0/24 can be summarized using 172.16.8.0/21 since the first subnet and the last subnet have first 21 bits in common

172.16.8.0 - 10101100 00010000 00001000 00000000

172.16.15.0 -10101100 00010000 00001111 00000000

![](<../.gitbook/assets/Unknown image (884)>)

Some routing protocols such as older classful ones (RIP IGRP etc..) have automatic summarization enabled by default, which aggregates routes based on the outdated classful addressing scheme.

For routing protocols supporting classless scheme you can disable it with the no auto summary command.

![](<../.gitbook/assets/Unknown image (885)>)

#### Null0 interface

is a pseudointerface that is always up and can never forward or receive traffic. Forwarding packets to Null0 is a common way to filter packets to a specific destination

A static route can be created to direct traffic to a non-existent interface called the Null 0. Most of the time it is created after route aggregation has been implemeted

Used as technique to prevent routing loops that can occur for many different reasons, but one of them is when a packet is forwarded based on a summary route that aggregates “extra” networks that do not exist or they are not part of the aggregated address. If the destination network does not exist, the next-hop router will not have a more specific route and it will use the default and if this default route points back to the router with the summary address, then these two devices create a routing loop and packets will be dropped when TTL expires

By pointing the summary address to Null0 interface, the packets to the aggregate subnet that does not exist are dropped there

This is because of route specificity – the longest prefix-length takes precedence. As the summary address (a /21) directs traffic to Null0 (null-routed), any packets received without a more specific route will be discarded, as a /21 is more specific than a default route (/0)

![](<../.gitbook/assets/Unknown image (886)>)

#### Variably subnetted route

In the above picture the "variably subnetted" message appears in the routing table when a major network (classful network) is subdivided into multiple subnets with different subnet masks. This message is mainly an indication that the router is dealing with subnets of the same major network but with different masks (Variable Length Subnet Masking - VLSM).

Even if you only have directly connected networks, this message will appear if the router sees multiple subnets of the same classful network. The router considers all learned or configured subnets under a major network and labels them as "variably subnetted" if they have different masks.

Network Resiliency using route summarization technique (with BGP) in a multihoming scenario

to ensure network resiliency, the setup of two circuits is required, one acts as primary and second as secondary

If primary ISP circuit fails, the traffic can be redirected to the secondary ISP circuit

To guarantee that paths to a company are selected deterministically outside the organization is to advertise a summary prefix (100.64.0.0/16) out both routers R1 and R2

We should advertise a longer matching prefix out the router for one prefix, and then advertise a longer matching prefix out the other router for the second prefix.

This allows for traffic to enter a network in a deterministic manner while still providing a backup path to the other network in the event that the first router fails

![](<../.gitbook/assets/Unknown image (887)>)

### Symmetric vs asymmetric routing

Symmetrical routing occurs when the path a packet takes from source to destination is identical (or effectively the same) as the path the return traffic takes back from destination to source.

In other words, forward and reverse paths are mirror images of each other across the network topology.

For many simple network setups, symmetric routing is the default or "normal" behavior.

Benefits:

Simplifies network management and troubleshooting because the path is predictable.

Required for certain network functions and security devices, such as stateful firewalls and some Quality of Experience (QoE) applications.

![](<../.gitbook/assets/Unknown image (888)>)

### Asymmetric Routing

Occurs when a packet traverses from a source to a destination in one path and takes a different path when it returns to the source (return traffic).

Asymmetric routing is not a problem by itself, but will cause problems when NAT or firewalls are used in the routed path. For example, in firewalls, state information is built when the packets flow from a higher security domain to a lower security domain. The firewall will be an exit point from one security domain to the other. If the return path passes through another firewall, the packet will not be allowed to traverse the firewall from the lower to higher security domain because the firewall in the return path will not have any state information. The state information exists in the first firewall.

![](<../.gitbook/assets/Unknown image (889)>)

![BGP and asymmetric routing | Noction](<../.gitbook/assets/Unknown image (890)>)

## Route redistribution

is a process where routes learned through one source (for example, statically configured routes, locally connected routes, or routes learned through a routing protocol) are injected into a routing protocol or between routing protocols

Redistribution occurs from the routing table into a routing protocol’s data structure (such as the EIGRP topology table or the OSPF LSDB)

This is a key concept for troubleshooting purposes because if the route is not in the routing table, it cannot be redistributed.

While it is desirable that you run a single routing protocol throughout your entire IP internetwork, multiprotocol routing is common for many reasons

These reasons include company mergers, multiple departments that are managed by multiple network administrators, and multivendor environments.

Because routing protocols use different metrics to determine the best path, path selection using the redistributed route information may be suboptimal

The metric information about a route cannot be translated exactly into a different protocol, so the path that a router chooses may not be the best.

Below example shows loss of the original metric during redistribution into OSPF, the OSPF will load balanc the traffic over both links, but the load balancing within the eigrp domain would be suboptimal due to the slower link since the redistribution to OSPF doesn't retain the native metric to the EIGRP

The issue can be solved by adding a different seed metrics

The metric assigned to a route being redistributed into another routing process is called a seed metric. The seed metric is needed to communicate relative levels of reachability between dissimilar routing protocols. A seed metric can be defined in one of three ways:

• Using the default-metric command

• Using the metric parameter with the redistribute command

• Applying a route map configuration to the redistribute command

If multiple seed metrics are defined with the commands, the order of preference is (1) metric defined in the route map that was applied to the redistribute command; (2) metric parameter defined with the redistribute command; (3) metric defined with the defaultmetric command. If a seed metric is not specified, a default seed metric is used

The below scenario is resolved by adding the metric/metric-type value with the redistribute command) on the boundary routers to ensure that a certain path is preferred because it has a lower overall metric

![](<../.gitbook/assets/Unknown image (891)>)

Redistribution Requirement: For a route to be redistributed, it must already be in the routing table of the router

If a route is withdrawn from the routing table (due to reasons like network changes or administrative actions), it will also be withdrawn from redistribution to other protocols.

This ensures that redistributed routes always reflect the current network topology

| redistribute < connected \| static \| eigrp \| ospf \| bgp > \<Process\_ID/AS> \<route-map \| metric> <> \[include-connected] | include-connected keyword is related to redistribution in IPv6 - it will redistribute local interfaces participating in the routing process being redistributed into the IPv6 routing protocol. Without include-connected option, local interfaces are treated as “connected” and are not redistributed as in case of IPv4. |
| ----------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Highly available network designs remove single points of failure through redundancy.

When redistributing routes between protocols, there must be at least two points of redistribution in the network to ensure that connectivity is maintained during a failure.

When performing multipoint redistribution between two protocols, the following issues may aris Suboptimal routing, Routing loops, which can lead to loss of connectivity or slow connectivity for the end users

Redistribution loop can occur when routes are continually redistributed between protocols, creating a loop of route advertisements that cause instability of the network

Rule: Never perform mutual redistribution e.g don't advertise prefixes from routing protocol X into Y and then back into X, and always prefer your “internal” routes over “external” routes for networks within internal domain

There are multiple scenarios of route redistribution

Either one redistrubtion point where single router connects two domains or multiple redistribution points where two redundant routers connects two domain

One-way redistribution from one routing protocol can be implemented or two-way redistribution, where the redistribution is performed for both routing domains.

With each scenario the complexity and the possibility of suboptiomal routing or routing loop is increased

![9.8 Types of Redistribution – Stuck-in-Active: Journal of an IT-Network Administrator](<../.gitbook/assets/Unknown image (892)>)

#### Redistribution at multiple points

There might not be an issue, if redistribution is performed on two or more points between EIGRP and OSPF, because if we redistribute routes between ospf and eigrp and eigrp to ospf on both EGW1 and EGW2 routers, no loop occurs, since EIGRP is redistributed as external with the AD higher than the OSPF default AD, so that the EGW1 will still prefer the OSPF route to the NCO1 instead of going through the worse route over ECO1

However this approach can differ when we redistribute between the same routing protocols or between RIP and OSPF. The router may overwrite the RIP route with the OSPF route, so it won't pass directly in the native RIP domain, but will traverse through the OSPF and the router may send it back to the router via OSPF domain - creating loop

![](<../.gitbook/assets/Unknown image (893)>)

Redistribution loop or routing table instability may arise, if external prefix is injected from OSPF to OSPF or EIGRP to EIGRP (they both will have the same metric)

When mutual redistribution is implemented on two points, the route will be redistributed mutually and the external prefix will be bouncing in the routing table, since the route will be continuously updated and overwritten by both paths. This implementation should be completely avoided or there should be implemented fixes shown after the below debug

![](<../.gitbook/assets/Unknown image (894)>)

Debug showing the issue

EGW1#debug ip ospf rib

OSPF RIB (Routing Information Base) debugging is on

OSPF Local RIB (Routing Information Base) debugging is on

OSPF Global RIB (Routing Information Base) debugging is on

OSPF Redistribution debugging is on

EGW1#

\*Jun 25 12:40:30.927: OSPF-200 LRIB : Updating route 172.16.0.0/24

\*Jun 25 12:40:30.927: OSPF-200 LRIB : Add path area dummy area, type Ext2, dist 20, forward 1, tag 0x0, via 10.1.1.1 GigabitEthernet1, route flags (PartialSPF), path flags (none), source 172.16.0.1, spf 64, list-type route\_type\_list, src rtr 172.16.0.1

\*Jun 25 12:40:30.927: OSPF-200 LRIB : Updating route 172.16.0.0/24

\*Jun 25 12:40:30.927: OSPF-200 LRIB : Add path area dummy area, type Ext2, dist 20, forward 2, tag 0x1, via 10.1.1.1 GigabitEthernet1, route flags (PartialSPF), path flags (none), source 10.1.2.2, spf 64, list-type route\_type\_list, src rtr 10.1.2.2

\*Jun 25 12:40:30.927: OSPF-200 LRIB : Sync'ed 172.16.0.0/24 type Ext2 - change (0x0): added 0 paths, deleted 0 paths, spf 64, route instance 64, pdb spf instance 64

\*Jun 25 12:40:30.927: OSPF-200 LRIB : Updating route 66.66.66.0/24

\*Jun 25 12:40:30.927: OSPF-200 LRIB : Add path area dummy area, type Ext2, dist 20, forward 1, tag 0x0, via 10.1.1.1 GigabitEthernet1, route flags (PartialSPF), path flags (none), source 172.16.0.1, spf 64, list-type route\_type\_list, src rtr 172.16.0.1

\*Jun 25 12:40:30.927: OSPF-200 LRIB : Updating route 66.66.66.0/24

\*Jun 25 12:40:30.927: OSPF-200 LRIB : Add path area dummy area, type Ext2, dist 20, forward 2, tag 0x1, via 10.1.1.1 GigabitEthernet1, route flags (PartialSPF), path flags (none), source 10.1.2.2, spf 64, list-type route\_type\_list, src rtr 10.1.2.2

\*Jun 25 12:40:30.927: OSPF-200 LRIB : Sync'ed 66.66.66.0/24 type Ext2 - change (0x0): added 0 paths, deleted 0 paths, spf 64, route instance 64, pdb spf instance 64

\*Jun 25 12:40:30.955: OSPF-100 LRIB : Creating route 172.16.0.0/24

\*Jun 25 12:40:30.955: OSPF-100 LRIB : Add path area dummy area, type Ext2, dist 20, forward 2, tag 0x1, via 20.1.1.1 GigabitEthernet2, route flags (PartialSPF), path flags (none), source 20.1.2.2, spf 86, list-type route\_type\_list, src rtr 20.1.2.2

\*Jun 25 12:40:30.956: OSPF-200 GRIB : Route 172.16.0.0/24 type 'Ext2' has been replaced

\*Jun 25 12:40:30.956: OSPF-100 GRIB : IP route replace of 1 next hops succeeded for 172.16.0.0/24 (flags 0x0, type Ext2, tag 0x1), retcode 0

\*Jun 25 12:40:30.956: OSPF-100 GRIB : Next hop via 20.1.1.1 on GigabitEthernet2 (distance 20, source 20.1.2.2, label 1048578) installed

\*Jun 25 12:40:30.956: OSPF-100 LRIB : Sync'ed 172.16.0.0/24 type Ext2 - change (Change, PathChange, HigherCost, ForcedSync): added 1 paths, deleted 0 paths, spf 86, route instance 86, pdb spf instance 86

\*Jun 25 12:40:30.956: OSPF-100 LRIB : Creating route 66.66.66.0/24

\*Jun 25 12:40:30.956: OSPF-100 LRIB : Add path area dummy area, type Ext2, dist 20, forward 2, tag 0x1, via 20.1.1.1 GigabitEthernet2, route flags (PartialSPF), path flags (none), source 20.1.2.2, spf 86, list-type route\_type\_list, src rtr 20.1.2.2

\*Jun 25 12:40:30.956: OSPF-200 GRIB : Route 66.66.66.0/24 type 'Ext2' has been replaced

\*Jun 25 12:40:30.956: OSPF-100 GRIB : IP route replace of 1 next hops succeeded for 66.66.66.0/24 (flags 0x0, type Ext2, tag 0x1), retcode 0

\*Jun 25 12:40:30.956: OSPF-100 GRIB : Next hop via 20.1.1.1 on GigabitEthernet2 (distance 20, source 20.1.2.2, label 1048578) installed

\*Jun 25 12:40:30.956: OSPF-100 LRIB : Sync'ed 66.66.66.0/24 type Ext2 - change (Change, PathChange, HigherCost, ForcedSync): added 1 paths, deleted 0 paths, spf 86, route instance 86, pdb spf instance 86

\*Jun 25 12:40:30.958: OSPF-100 REDIS: Notification to redistribute 172.16.0.0/24

\*Jun 25 12:40:30.958: OSPF-200 REDIS: Notification to redistribute 172.16.0.0/24

\*Jun 25 12:40:30.958: OSPF-100 REDIS: Notification to redistribute 66.66.66.0/24

\*Jun 25 12:40:30.958: OSPF-200 REDIS: Notification to redistribute 66.66.66.0/24

EGW1#

\*Jun 25 12:40:35.955: OSPF-200 GRIB : Add backup request 172.16.0.0/24, type Ext2

\*Jun 25 12:40:35.955: OSPF-100 GRIB : Route 172.16.0.0/24: delete primary path via 20.1.1.1 on GigabitEthernet2, source 20.1.2.2 succeeded

\*Jun 25 12:40:35.955: OSPF-100 LRIB : Sync'ed 172.16.0.0/24 type Ext2 - change (RtDelete, RthDelete, PathChange): added 0 paths, deleted 1 paths, spf 87, route instance 86, pdb spf instance 87

\*Jun 25 12:40:35.955: OSPF-100 REDIS: Translate 172.16.0.0/24 into Type-7 LSA, metric 16777215, metric-type 0, forw addr 0.0.0.0, tag 0x0

\*Jun 25 12:40:35.955: OSPF-200 GRIB : Add backup request 66.66.66.0/24, type Ext2

\*Jun 25 12:40:35.955: OSPF-100 GRIB : Route 66.66.66.0/24: delete primary path via 20.1.1.1 on GigabitEthernet2, source 20.1.2.2 succeeded

\*Jun 25 12:40:35.955: OSPF-100 LRIB : Sync'ed 66.66.66.0/24 type Ext2 - change (RtDelete, RthDelete, PathChange): added 0 paths, deleted 1 paths, spf 87, route instance 86, pdb spf instance 87

\*Jun 25 12:40:35.955: OSPF-100 REDIS: Translate 66.66.66.0/24 into Type-7 LSA, metric 16777215, metric-type 0, forw addr 0.0.0.0, tag 0x0

\*Jun 25 12:40:35.958: OSPF-200 REDIS: Notification to redistribute 172.16.0.0/24

\*Jun 25 12:40:35.958: OSPF-200 REDIS: Notification to redistribute 66.66.66.0/24

\*Jun 25 12:40:35.958: OSPF-200 GRIB : IP route replace of 1 next hops succeeded for 66.66.66.0/24 (flags 0x0, type Ext2, tag 0x0), retcode 0

\*Jun 25 12:40:35.958: OSPF-200 GRIB : Next hop via 10.1.1.1 on GigabitEthernet1 (distance 20, source 172.16.0.1, label 1048578) installed

\*Jun 25 12:40:35.958: OSPF-200 LRIB : Sync'ed 66.66.66.0/24 type Ext2 - change (PathChange): added 1 paths, deleted 0 paths, spf 64, route instance 64, pdb spf instance 64

\*Jun 25 12:40:35.958: OSPF-200 GRIB : IP route replace of 1 next hops succeeded for 172.16.0.0/24 (flags 0x0, type Ext2, tag 0x0), retcode 0

\*Jun 25 12:40:35.958: OSPF-200 GRIB : Next hop via 10.1.1.1 on GigabitEthernet1 (distance 20, source 172.16.0.1, label 1048578) installed

\*Jun 25 12:40:35.958: OSPF-200 LRIB : Sync'ed 172.16.0.0/24 type Ext2 - change (PathChange): added 1 paths, deleted 0 paths, spf 64, route instance 64, pdb spf instance 64

\*Jun 25 12:40:35.960: OSPF-100 REDIS: Notification to redistribute 66.66.66.0/24

\*Jun 25 12:40:35.960: OSPF-100 REDIS: Notification to redistribute 172.16.0.0/24

\*Jun 25 12:40:35.976: OSPF-200 LRIB : Updating route 172.16.0.0/24

\*Jun 25 12:40:35.976: OSPF-200 LRIB : Add path area dummy area, type Ext2, dist 20, forward 1, tag 0x0, via 10.1.1.1 GigabitEthernet1, route flags (PartialSPF), path flags (none), source 172.16.0.1, spf 65, list-type route\_type\_list, src rtr 172.16.0.1

\*Jun 25 12:40:35.976: OSPF-200 LRIB : Sync'ed 172.16.0.0/24 type Ext2 - change (0x0): added 0 paths, deleted 0 paths, spf 65, route instance 65, pdb spf instance 65

\*Jun 25 12:40:35.976: OSPF-200 LRIB : Updating route 66.66.66.0/24

\*Jun 25 12:40:35.976: OSPF-200 LRIB : Add path area dummy area, type Ext2, dist 20, forward 1, tag 0x0, via 10.1.1.1 GigabitEthernet1, route flags (PartialSPF), path flags (none), source 172.16.0.1, spf 65, list-type route\_type\_list, src rtr 172.16.0.1

\*Jun 25 12:40:35.976: OSPF-200 LRIB : Sync'ed 66.66.66.0/24 type Ext2 - change (0x0): added 0 paths, deleted 0 paths, spf 65, route instance 65, pdb spf instance 65

EGW1#show ip route profile

IP routing table change statistics:

Frequency of changes in a 5 second sampling interval

***

Change/ Fwd-path Prefix Nexthop Pathcount Prefix

interval change add change change refresh

***

0 1548 1564 1586 1565 1586

1 4 4 0 5 0

2 29 16 0 14 0

3 2 1 0 0 0

4 1 1 0 2 0

5 2 0 0 0 0

10 0 0 0 0 0

15 0 0 0 0 0

20 0 0 0 0 0

25 0 0 0 0 0

30 0 0 0 0 0

55 0 0 0 0 0

80 0 0 0 0 0

105 0 0 0 0 0

130 0 0 0 0 0

155 0 0 0 0 0

***

Change/ Fwd-path Prefix Nexthop Pathcount Prefix

interval change add change change refresh

***

280 0 0 0 0 0

405 0 0 0 0 0

530 0 0 0 0 0

655 0 0 0 0 0

780 0 0 0 0 0

1405 0 0 0 0 0

2030 0 0 0 0 0

2655 0 0 0 0 0

3280 0 0 0 0 0

3905 0 0 0 0 0

7030 0 0 0 0 0

10155 0 0 0 0 0

13280 0 0 0 0 0

Overflow 0 0 0 0 0

{% hint style="info" %}
the route eventually gets stable, but it was bouncing a while, however the path via NCO1 instead of the originating router ECO1 has been chosen - suboptimal routing
{% endhint %}

![](<../.gitbook/assets/Unknown image (895)>)

![](<../.gitbook/assets/Unknown image (896)>)

![](<../.gitbook/assets/Unknown image (897)>)

Solutions to deal with redistributions on edge points

Filtering based on route tags

| router ospf 1 redistribute ospf 2 subnets tag 2 distribute-list route-map DENY\_TAG\_2 in ! route-map DENY\_TAG\_2 deny 10 match tag 2 route-map DENY\_TAG\_2 permit 20 ! router ospf 2 redistribute ospf 1 subnets tag 1 distribute-list route-map DENY\_TAG\_1 in ! route-map DENY\_TAG\_1 deny 10 match tag 1 route-map DENY\_TAG\_1 permit 20                                                           | To prevent the redistribution of routes from one domain/protocol back into the same domain, we can set a tag using the redistribute command toward a domain and deny inbound in the other domain by a matching the tag on both redistribution points - R1 and R2 Downside of this approach is that since the prefixes are denied from the routing table, the domains can not back up each other![](<../.gitbook/assets/Unknown image (898)>) |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Filtering based on admin distance and ACL or prefix list                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| router ospf 1 redistribute ospf 2 subnets route-map OSPF\_DOMAIN\_2 distance ospf external 222 ! router ospf 2 redistribute ospf 1 subnets route-map OSPF\_DOMAIN\_1 distance ospf external 222 ! route-map OSPF\_DOMAIN\_2 permit 10 match ip address 2 ! route-map OSPF\_DOMAIN\_1 permit 10 match ip address 1 ! access-list 1 # all routes from OSPF Pid-1 ! access-list 2 # all routes from OSPF Pid-2 | Another approach is to increase the administrative distance in order to prefer one process over another, despite available redundancy, it is not recommended approach as you have to maintain long ACL with prefixes from each domain                                                                                                                                                                                                        |
| (config-route-map)# match internal                                                                                                                                                                                                                                                                                                                                                                          | redistributes only the internal routes that belong to one domain into another domain This prevents the redistribution of prefixes that are already external in one domain back into the same domain. Downside: If there are already external prefixes in either of the domains (such as external prefixes that were redistributed from another domain), then those prefixes will not be redistributed to other domains                       |

**Routing Loop scenarios**

![](<../.gitbook/assets/Unknown image (899)>)

The route to 192.168.200.0 is flapping between R1 and R2. Which set of configuration changes resolves the flapping route?

D is right. R1 just redistribute RIP in EIGRP. R2 learn 192.168.200.0 route from EIGRP and R2 redistribute EIGRP in OSPF, then R2 advertise 192.168.200.0 to R1. R1 learns 192.168.200.0 from R2 via OSPF. OSPF has AD 90 and RIP 120, so OSPF route become better than RIP route, but, for a route redistribution there is a rule that says the route must be in routing table and the source of redistribution must be the source of this route in route table, in this case, the RIP. When R1 learn this route in OSPF, the OSPF route replace the RIP route in route table, so the rule is broken and the redistribution stop working, then R1 stops redistributing 192.168.200.0 to EIGRP and R2 stop receiving this route and R2 stops redistribute this route in OSPF so R1 won't receive this route from OSPF anymore, then OSPF route is removed from LSDB and RIB, so RIP route go to the route table and the redistribution to EIGRP starts again and the problem starts over and over

If you redistribute RIP in OSPF in R1, R2 is gonna have this route as the best route from OSPF, so it does not matter if R2 learns it from EIGRP or NOT, because OSPF has AD 90 and External EIGRP 170.

![](<../.gitbook/assets/Unknown image (900)>)

## Route profiling

built-in feature that inspects the routing table with a 5 second interval. This can be used to troubleshoot routing inconsistencies or loops

**Change/interval**: This is the time in seconds since the router started monitoring.

**Fwd-path change**: This is the most critical metric. It counts how many times the number of routes in the forwarding path has changed. If this number is consistently high, it suggests instability.

**Fwd-path add**: This shows the number of new prefixes (network addresses) that have been added to the routing table.

**Nexthop change**: This counts how many times the "next hop" for a prefix has changed.

**Pathcount change**: This refers to the number of paths to a destination that have changed.

**Prefix refresh**: This shows how often a network's standard routing table maintenance process has been refreshed. A high count here is normal and indicates the router is doing its job.

For the first 5-second interval (row 0), there was 1 change in Fwd-path, Fwd-path add, Nexthop, and Pathcount (row 1). This suggests that one new route was added and that it caused a change in the forwarding path and the next hop information.

After that initial change, the rest of the report shows zeros across all columns for all subsequent intervals (5, 10, 15, and so on). The report continues to display zero changes for a very long time, all the way down to 11280 seconds.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>
