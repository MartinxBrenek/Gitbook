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

# L3 Gateway Redundancy

To ensure **High Availability (HA)** for gateway of the network for key locations, it is important to have redundant paths with two or more routers to avoid **Single Points of Failure (SPOFs)**, which is to the reliance on a single forwarding device whose failure could disconnect the entire network. Each router can provide a WAN connectivity via a different service providers, which creates a backup pathway for data flow

However, most client computers, servers, printers, and so on, do not support dynamic routing protocols and whenever they need to communicate with a host that is located in a different subnet, they must relay packets through the default gateway. Therefore, the availability of this gateway is extremely important.

The end device is configured with a single default gateway IPv4 address, which does not dynamically update when the network topology changes. If the default gateway fails, the local device is unable to send packets out of the local network segment. As a result, the host is isolated from the rest of the network. Even if a redundant router that could serve as a default gateway for that segment exists, there is no dynamic method by which these devices can determine the address of a new default gateway.

![](<../.gitbook/assets/Unknown image (1065)>)

### First-hop redundancy protocols (FHRP)

**First-hop redundancy protocols (FHRP)** provide automatic failover (taking over services/forwarding in case of failure) and load balancing mechanism in a redundant router setups.

It is achieved by having redundant routers to share one **VIP (Virtual IP address)** and **Virtual MAC address** and the currently active router assumes it to serve as L3 gateway for the end hosts

FHRP does not require configuration of dynamic routing or router discovery protocols on every end host - they simply initiate an ARP for the virtual MAC

In the even of the failure of the active router, the failover is initiated dynamically, the backup router takes over the VIP and updates the end hosts to use it as the new default gateway

The host devices send traffic to the address of the virtual router. The actual (physical) router that forwards this traffic is transparent to the end stations.

Note Routing protocol with FHRP on one interface is not recommended and doesn't work

![](<../.gitbook/assets/Unknown image (1066)>)

### Hot Standby Router Protocol (HSRP)

**Hot Standby Router Protocol (HSRP)** is FHRP protocol designed by Cisco, where a set of routers on a LAN segment are designated as members of a virtual standby group

This standby group creates a virtual router configured with a unique virtual IP and virtual MAC address.

The virtual router does not physically exist but represents the common target for two or more routers that are configured to provide backup to each other.

The routers manage this virtual gateway address, communicating among themselves to determine which router is responsible for forwarding traffic sent to the virtual IP address.

Router with the highest HSRP interface priority assume Active role with the VIP and VMAC address and performs IP forwarding

Active router responds to ARP requests from end hosts trying to resolve the owner of the VMAC address so that they can send the traffic to the current active router

Standby router serves as instant backup ready to take over the role of Active router, in case the Active router failure

If the Active router fails, the Standby router sends Gratuitous ARP to update the ARP table of all downstream switches and hosts in the subnet, so that switches learns the VMAC from the port, where standby is connected

Active and Standby routers send periodic hello messages every 3 seconds to the well-known multicast IP 224.0.0.2 on UDP port 1985 and MAC address 0000.0C07.ACxx (where xx is the group number in hex) to negotiate roles and to detect mutual reachability

The Hold time is 3 times of Hello timer - e.g if no Hello packet is received in 10 seconds, the router assumes that the other HSRP has router failed and takes over the active role

Also, more than two routers can participate in HSRP and back up both Active and Standby, in case they both fail. Those routers remain in LISTEN state

![](<../.gitbook/assets/Unknown image (1067)>)

#### HSRP states

**DISABLED** Similar to STP disabled port state.

**INIT** Default state when HSRP is enabled on the interface, or when the interface is turned on

**LISTEN** The router knows the virtual IP, listens to Hello messages from other routers in the group - other HSRP routers remain in this state

**SPEAK** The router sends HELLO packets and participates in the active/standby election process.

**STANDBY** The router with the second highest priority is elected as standby router

It knows the virtual IP, listens to Hello messages, sends Hello messages, and is ready to take an active role if necessary

**ACTIVE** The router is elected as active router and actively forwards traffic.

It knows the virtual IP, listens to Hello messages, sends Hello messages, performs the function of the default gateway and processes all messages that are sent to the virtual MAC address

#### HSRP version 2

**HSRP version 2** adds support for both IPv4 and IPv6; not compatible with v1

The expanded group number range was changed to allow the group number to match the VLAN number on subinterfaces - range from 0-4095

Virtual MAC address is 0000.0C9F.Fxxx (xxx = group number in hex)

Hello packets are sent to Multicast address 224.0.0.102 on UDP port 2029 for IPv4

Hello timer can be set to milliseconds (default is same as in HSRPv1 - Hello every 3sec; Hold time 10sec)

HSRP supports BFD, which can provide failure time detection at the microsecond level

However the current hello time mechanism is sufficient enough to ensure the failover mechanism and BFD can be used in scenarios where it can be defined as a template that is applied as failure detection for multiple protocols, such as OSPF or BGP

Note The shorter the advertisement interval, the shorter the black hole period, though at the expense of more traffic in the network

For IPv6

Hello packets are sent to Multicast address FF02::66

MAC address is 0005.73A0.0xxx (xx = group number in hex)

Virtual IPv6 link-local address is, by default, derived from the HSRP virtual MAC address. Both MAC and Virtual IPv6 can be set to preferred value

#### Operation

When HSRP is configured on an interface, it sets the RA lifetime to 0 for all announced prefixes

Once the Active router is elected, it will start sending RA with the default lifetime again to configure itself as the Active gateway for the hosts

Virtual MAC contained inside RA

Sent from virtual IPv6 address

Active and standby routers then continue to send periodic HSRP Hello messages from link-local address to FF02::66 to detect mutual reachability

#### Restrictions (SVI / VLAN gotchas)

When SVI is configured for a VLAN, it automatically act as the L3 interface gateway for that VLAN and terminate HSRP that is configured on the uplink routers via switch trunk link

E.g if you have HSRP for sub-interface .10 with encapsulation for vlan 10 and you create Vlan10 on the switch, it terminates the HSRP between the routers on the uplink trunk link

It is also important to note that you should move any switched ports to separate VLAN to avoid that the default interface Vlan 1 would prevent HSRP and other features to be established for uplink devices

| (config-if)#standby version 2                                   | must be explicitly configured to run IPv6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-if)#standby ip                                          | IPv4 config The basic HSRP configuration only requires you to select the HSRP number of the group and set the IP address of the virtual router                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| (config-if)#standby ipv6 autoconfig OR (config-if)#standby ipv6 | IPv6 config it is recommended to use automatically configured HSRP VIP, as it autogenerates link-local VIP from the HSRP reserved range with EUI and group However manual link-local can be configured aswell Also configure GUA and link-local address on the interface to not assume volatile and less secure EUI64                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| (config-if)#standby priority <>                                 | range of 0 – 255; default is 100                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| (config-if)#standby preempt                                     | HSRP Preempt is a mechanism that ensures that the previously active router with a highest priority will become the Active again when it comes back online (not enabled by default) can be fine tuned to ensure stability: #standby 2 preempt delay minimum 30 reload 30                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| (config-if)#standby track                                       | Object tracking FHRPs rely only on monitoring the status of devices and status of FHRP-enabled interfaces This approach has a blind spot: it cannot detect problems outside the local network, such as uplink failure or IP reachability to the external networks such as Internet To ensure that when the uplink of Active router fails or if it loses IP reachability to the WAN, it fails over the active role to the standby router, the [IP SLA with object tracking](https://onenote/#Probing%20Mechanisms\&section-id={6EC9C333-3E04-41C1-8C01-0E8D85C128A8}\&page-id={E4D987C5-DB44-4A7B-B03D-9C29A9CB7501}\&object-id={A5995C43-A2A5-0106-363D-26E52E3DF133}&39\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Notes/L3.one) must be implemented Interface tracking monitor the status of specific interface on the device Object tracking can monitor the state of an object like the IP SLA tracking IP reachability of specific IP address The state change of either of the two can trigger a specific action such as dynamically adjusting the FHRP router priority If a tracked object goes down, the priority decrements. If a tracked object comes back up, the priority increments - the default decrement/increment is 10 |
| (config-if)#standby authentication \<text \| md5> key-string <> | HSRP can be authenticated to prevent an unauthorized router from joining a group Authentication can be done in two ways: Plaintext or MD5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| (config-if)# standby timers                                     | on HSRP negotiation the active router will override the standby timers. Best practise                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| (config-if)# bfd interval <> min\_rx <> multiplier <>           | Enables BFD on the interface.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| (config-if)# standby bfd                                        | (Optional) Enables HSRPsupport for BFD on                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| show standby \[brief]                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

#### HSRP router replacement procedure

1. Replace the standby router first:

* Ensure the new standby router successfully negotiates its standby state with the existing active router.
* Monitor HSRP logs to verify that the virtual IP and MAC addresses are functioning correctly.
* Confirm that the new standby router transitions into the 'standby' state and is ready to take over if needed.

2. Replace the active router:

* After verifying that the standby router is functioning properly, replace the active router.
* The standby router should automatically take over as the 'active' router without interrupting traffic.
* Once the new active router is in place, it should negotiate its active role based on HSRP priority and preemption settings.

3. Check HSRP Preemption and Priority Settings:

* Ensure preemption is configured on both routers so the router with the higher priority can take over the active role.
* Verify that HSRP priorities are set correctly, with the intended active router having the highest priority.

### Multigroup HSRP (MHSRP)

**Multigroup HSRP (MHSRP)** enables load-sharing by using multiple HSRP groups (because in a single HSRP group, only one router is active at a time).

Load balancing can be implemented based on the VLAN by creating multiple HSRP groups on individual subinterfaces of a single interface on a router

Routers can simultaneously provide redundant backup and perform load-sharing across different IP subnets.

For each standby group, an IP address and a single well-known MAC address with a unique group identifier is allocated to the group.

The IP address of a group is in the range of addresses belonging to the subnet in use on the LAN.

However, the IP address of the group must differ from the addresses allocated as interface addresses on all routers and hosts on the LAN, including virtual IP addresses assigned to other HSRP groups.

Switch1 is active for vlan 10 and standby for vlan 20, switch2 is active for vlan 20 and standby for vlan 10

![](<../.gitbook/assets/Unknown image (1068)>)

#### HSRP and STP

If L3 switches are used as the default gateway, ensure the active HSRP router for a VLAN is also the STP root for that VLAN.

Otherwise, network traffic will be suboptimal

![](<../.gitbook/assets/Unknown image (1069)>)

### Virtual Router Redundancy Protocol (VRRP)

**Virtual Router Redundancy Protocol (VRRP)** is an industry open standard FHRP suitable for multi-vendor environments

The active router with the highest configurable priority is called Master and the standby is called Backup. Priority range from 1-254

Hello messages are sent every 1 second to the multicast address 224.0.0.18 with a reserved IP protocol 112 on a reserved MAC 0000.5e00.01xx, where the xx is the group ID in hex

This address is used by only one master router at a time, and it will reply with this MAC address when an ARP request is sent for the virtual router's IP address

Only the Master send Hello packets to the multicast address FF02::12, to report its state

The Backup router(s) are only supposed to send multicast packets during an election process

Failure to receive a multicast packet from the primary router for more than three times the hello timer causes the secondary routers to assume the primary router is dead and assume the role of Master. The same is true if the Master starts announcing its priority which is lower than the priority of the current backup router, which can happen if the priority is lowered based on the changed state of the monitored object, so that the backup router takes over the role of Master.

The virtual router can manage multiple IP addresses, including secondary IP addresses

Therefore, if you have multiple subnets configured on an Ethernet interface, you can configure VRRP on each subnet.

VRRP supports BFD, which can provide failure time detection at the microsecond level

However the current hello time mechanism is sufficient enough to ensure the failover mechanism and BFD can be used in scenarios where it can be defined as a template that is applied as failure detection for multiple protocols, such as OSPF or BGP

#### VRRPv3

Support IPv4 and IPv6

When VRRPv3 is in use, VRRPv2 is unavailable

No Authentication available for VRRPv3 (VRRPv2 support clear text authentication)

IPv6 Hello messages are sent to FF02::12 multicast address on IP protocol number 112

Virtual MAC address range from 0000.5E00.0200 through 0000.5E00.02FF

Only VRRP master router sends RA and responds to ND for VIP

Virtual MAC contained inside RA

Sent from virtual IPv6 address

| (config-if)# vrrp ip                                                                                                            |                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| (config-if)# vrrp priority                                                                                                      | Default Priority: 100                                                   |
| (config)# fhrp version vrrp v3                                                                                                  | enable version 3 to implement IPv6                                      |
| (config-if)# vrrp address-family ipv6                                                                                           | Address-family must be specified to get into the sub-configuration mode |
| (config-if-vrrp)# address fe80::1 primary                                                                                       | to specify link local address as a primary address for that group       |
| (config-if-vrrp)# priority                                                                                                      |                                                                         |
| (config-if)# vrrp authentication (config-if)# vrrp authentication md5 key-string (config-if)# vrrp authentication md5 key-chain |                                                                         |
| (config)# track interface line-protocol (config-if)# vrrp track decrement                                                       | ## VRRP interface tracking configuration                                |
| (config-vrrp-if)# vrrp 100 version 3 bfd fast-detect peer ipv4 <>                                                               | Enables BFD fast detection on the VRRP interface                        |
| R1(config-if)# vrrp 1 timers advertise \<sec\|msec value>                                                                       | adjusting timers                                                        |
| show \[fhrp\|vrrp] \[brief]                                                                                                     |                                                                         |

### Gateway Load Balancing Protocol (GLBP)

**Gateway Load Balancing Protocol (GLBP)** is a Cisco protocol used to overcome the limitations of HSRP and VRRP by adding load-sharing functionality to utilize all available bandwidth

GLBP group allows up to four virtual MAC addresses per group

GLBP supports up to 1024 virtual routers (GLBP groups) on each physical interface of a router - 4 AVFs and 1 AVG per GLBP group, where GLBP routers are in ACTIVE/ACTIVE state

Same model maintained as in IPv4: one virtual IPv6 address, multiple MAC addresses

Gateway Load Balancing Protocol provides load balancing over multiple routers using a single virtual IPv6 address and multiple virtual MAC addresses

The forwarding load is shared among all routers in a GLBP group

#### Active Virtual Gateway (AVG)

**Active Virtual Gateway (AVG)**: Members of a GLBP group elect one gateway to be the AVG for that group. The AVG router with the highest priority (or IP) responsible for operation of the protocol

The AVG is responsible for assigning the virtual MAC addresses to each member of the group. Other group members request a virtual MAC address after they discover the AVG through hello messages

Standby virtual gateway (SVG) is elected to ensure backup for the AVG when the AVG becomes unavailable

All other routers in a group are placed in a LISTEN state

The AVG assigns a virtual MAC address to each member of the GLBP group.

Each Active virtual forwarder (AVF) assumes responsibility for forwarding packets that are sent to the virtual MAC address, which is assigned to the AVF by the AVG

Similarly, if the AVF fails, Secondary virtual forwarder (SVF) assumes responsibility for the virtual MAC address

The AVG is responsible for answering ARP requests for the virtual IP address.

For example, when a local PC sends an ARP request (in IPv4) or NS (in IPv6) for the VIP, the AVG is responsible for replying to the ARP/NS request with the virtual MAC address of it's dedicated AVF

The AVG achieves load sharing by replying to the ARP/NS requests with different virtual MAC addresses

A router can be an AVG and an AVF at the same time and there can be multiple AVFs that are active at the same time but only one AVG

Load balancing is not based on traffic load, but rather on the number of hosts that will use each gateway router.

#### GLBP gateway priority

**GLBP gateway priority** controls the election of AVG and SVG – configurable value from 1 through 255

Determines if a GLBP router functions as a backup virtual gateway and the order of ascendancy to becoming an AVG if the current AVG fails

AVG preemption is disabled by default, enabled with command (config-if)#glbp preempt

#### GLBP gateway weight

**GLBP gateway weight** determine the forwarding capacity of each router in the GLBP group

Below a certain threshold, AVF stops forwarding packets

Pre-emption enabled by default

Can be disabled with (config-if)#no glbp forwarder preempt

Delay is by default 30 second – adjusted with (config-if)# glbp forwarder preempt delay

HELLO packets are sent to the multicast address 224.0.0.102 on UDP port 3222

VMAC address format is 0007.B400.XXYY

XX stands for the GLBP group number

YY stands for the AVF number

#### Load balancing modes

**Round robin** Uses each AVF MAC address to sequentially reply for the VIP (This is default)

**Weighted** works in conjunction with the interface weight value (100 is default) defining weights to each device in the GLBP group to determine the ratio of load balancing between the devices. This allows prioritizing one router to handle more traffic in a group

**Host dependent** every host will always receive the ARP reply by the same virtual MAC address and traffic gets always forwarded by the same AVF.

| (config-if)# glbp ip                                                                                                                 |                                                                                                                                                                                                                                                                                                     |
| ------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-if)# glbp priority                                                                                                           |                                                                                                                                                                                                                                                                                                     |
| (config-if)# glbp preempt                                                                                                            | AVG preemption is disabled by default, enabled with command (AVF preempt is enabled by default)                                                                                                                                                                                                     |
| (config-if)# glbp name                                                                                                               |                                                                                                                                                                                                                                                                                                     |
| (config-if)# glbp 1 load-balancing                                                                                                   | ## GLBP load-balancing method configuration                                                                                                                                                                                                                                                         |
| (config)# track interface line-protocol (config-if)# glbp weighting lower upper (config-if)# glbp weighting track decrement          | ## GLBP interface tracking configuration                                                                                                                                                                                                                                                            |
| (config-if)# glbp authentication text (config-if)# glbp authentication md5 key-string (config-if)# glbp authentication md5 key-chain | ## GLBP authentication configuration                                                                                                                                                                                                                                                                |
| show glbp \[brief]                                                                                                                   | When running the show glbp command the router who took the AVF over will be shown as secondary the local router always shows as ACTIVE while the others show as LISTEN, but all are actually working When running the show glbp command the router who took the AVF over will be shown as secondary |

### Other HA techniques

#### Anycast IP

**Anycast IP** can be used on multiple devices to enhance high availability by routing to the nearest node.

#### Same IP with different mask (not recommended)

It is possible to configure the same IP address with different mask on two routers (primary/secondary).

However, it is not a recommended approach as it can lead to several issues and complexities, such as routing conflicts, asymmetric routing, and packet loss.

![](<../.gitbook/assets/Unknown image (1070)>)

#### Redundancy with IPv6 ND (RS/RA)

IPv6 uses robust Neighbor Discovery and Router Advertisement procedures to provide address and default-gateway configuration to the hosts

Pseudo failover mechanism can be achieved by adjusting Router Advertisement parameters with default router-preference value set to high on the primary router

Additionally the RA lifetime and RA advertisement interval should be adjusted aswell to continuously update hosts about it's presence and to instruct hosts for how long they should use the router as the default gateway.

To ensure fast failover mechanism, the RA lifetime should be set to the lowest value which is 1 second, and the RA interval can be set to also to 1 sec to ensure consistency (despite it can be set to msec value). When the active gateway fails, it will stop sending any RA's, while the backup router will continue to send them. And as soon as the RA lifetime expires, the hosts will receive RA from the backup and will use it as their new gateway, thus achieving fault-tolerant GW. The host also can discover that the default gateway failed from the [NUD](onenote:L3.one#NDP\&section-id={6EC9C333-3E04-41C1-8C01-0E8D85C128A8}\&page-id={8CE84C36-003A-4FE8-8638-3BED6F709A3C}\&object-id={26910DE5-C591-06D1-2C4F-BAD0DB7C543A}\&E\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE)

This technique however saturates the segment with router advertisements as well as and adds additional load on the host CPU, as it must each second refresh it's default gateway

FHRP should be used whenever possible as they provide sub-second failover mechanism without additional overhead

Moreover additional IP SLA tracking should be implemented to avoid the issue that hosts will use the active router that is up, but lost the connectivity to the WAN, thus dropping the traffic and not letting the backup router to take over
