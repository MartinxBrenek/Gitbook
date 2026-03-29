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

# Link Aggregation

**EtherChannel** is a link aggregation method that bundles several physical links into a single logical **Port-channel (Po)**

Etherchannel enables packets to be sent over several physical interfaces as if over a single interface

The logical Po interface is created automatically as soon as two or more physical links are assigned to the group. Each bundle has a single MAC, a single IP address, and a single configuration set such as ACLs or QoS. Port channel configuration is reflected to all member physical ports

EtherChannel always creates one-to-one logical links. You cannot send traffic to two different switches through the same EtherChannel logical link. One EtherChannel logical link always connects only two devices.

You can also configure multiple EtherChannel links between two devices. However, when several logical EtherChannel links exist between two switches, STP detects loops.

To avoid loops, STP will make only one logical link operational. When STP blocks the redundant links, it blocks one entire EtherChannel, thus blocking all the ports belonging to that EtherChannel link.

### Benefits

STP sees links in Ethechannel as a single link, as a result STP does not consider a single link to be loop and put all port channel interfaces in the forwarding state. The broadcast received on one of the etherchannel ports wouldn't cause broadcast storm, since all ports within the etherchannel acts as one port, thus the switch won't send the broadcast back from the port, where the broadcast was received from.

Ensures redundancy - If a link fails, EtherChannel redirects traffic from the failed link to the remaining links in the channel without downtime

Load balances network traffic across all links/ports. Frames belonging to the same flow always traverse the same physical link (per-flow). Flows cannot exceed BW of an individual link

Etherchanel is up as long as at least one physical link is active

It can bundle L3 routed ports (ports that do not run DTP,STP) - can be assigned an IP address to isolate broadcast domain

Can be used on L2/access or L2/trunk port as well as on L3 ports - also between switches and servers

![](<../.gitbook/assets/Unknown image (536)>)

### Bundle member requirements

The individual links must match on several parameters:

Interface types cannot be mixed, for instance FastEthernet or Gigabit Ethernet cannot be bundled into a single EtherChannel.

Speed and duplex settings must be the same on all the participating links.

Switchport mode and VLAN information must match. Access ports must be assigned to the same VLAN. Trunk ports must have the same allowed range of VLANs. The native VLAN

must be the same on all the participating links.

Routed L3 etherchannels must have matching duplex mode and bandwidth

![](<../.gitbook/assets/Unknown image (537)>)

![](<../.gitbook/assets/Unknown image (538)>)

### Static EtherChannel

Manual static configuration places the interface in an EtherChannel manually, without any negotiation. No negotiation between the two switches means that there is no checking to make

sure that all the ports have consistent settings.

With static configuration, you define a mode for a port. There is only one static mode, the on mode. When static on mode is configured, the interface does not negotiate—it does not exchange any control packets. It immediately becomes part of the aggregated logical link, even if the port on the other side is disabled

| (config-if)# channel-group mode on     | enables default etherchannel without negotiations - when a device does not support LACP or Pagp                   |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| port-channel channel-number persistent | Converts the auto created EtherChannel into a manual one and allows you to add configuration on the EtherChannel. |
| show etherchannel \[summary \| detail] |                                                                                                                   |
| show interface port-channel            |                                                                                                                   |

### LACP (Link Aggregation Control Protocol)

**LACP (Link Aggregation Control Protocol)** is an open (IEEE) EtherChannel protocol

With LACP, you can control link aggregation (for example the maximum number of bundled ports allowed). LACP is superior to static port channels with its automatic failover where

traffic from a failed link within EtherChannel is sent over remaining working links in the EtherChannel.

LACP controls the bundling of physical interfaces to form a single logical interface.

When you configure LACP, LACP packets are sent between LACP enabled ports to negotiate the forming of a channel. When LACP identifies matched Ethernet links, it groups the matching links into a logical EtherChannel link.

Supports up to 16 links in a channel, but only 8 can be in operation and all other as hot-standby

| (config-if-range)# channel-group 1 mode { active \| passive } | bundles physical ports into port channel. One end must be active and the second either active or passive. Passive and Passive does not form etherchannel between devices Active sets the port to actively negotiate etherchannel with LACP packets Passive sets the port to listen for PagP frames to form an etherchannel |
| ------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-if)# port-channel min-links <>                        | Specifies the minimum number of member ports that must be in the link-up state and bundled in the EtherChannel for the port channel interface to transition to the link-up state                                                                                                                                           |
| (config-if)# lacp port-priority 32000                         | to enforce active state for port in an etherchannel                                                                                                                                                                                                                                                                        |
| (config)#port-channel auto                                    | Auto-LAG is LACP feature that enables to create port channel autmatically                                                                                                                                                                                                                                                  |

LACP system priority identifies which switch is the master switch for a port channel (when there are more member interfaces in a port channel than the maximum number, master choose which member interfaces are active, and this can be configured with system priority)

LACP Fast when fail occurs a link can be identified and removed in 3 seconds compared to the 90 seconds specified in the original LACP standard

{% hint style="info" %}
The `channel-group` identifier does not need to match on both sides. Use the same number anyway. It makes operations easier.
{% endhint %}

### PAgP (Port Aggregation Protocol)

Cisco proprietary etherchannel protocol. Up to 8links in a channel

| (config-if-range)# channel-group 1 mode { desirable \| auto } | bundles physical ports into port channel. One end has to be desirable and the second either desirable or auto. Auto and Auto does not form etherchannel between devices Desirable sets the port to actively negotiate etherchannel Auto sets the port to listen for PagP frames to form an etherchannel |
| ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| interface Port-channel1 switchport mode trunk                 | then configure port channel as trunk You can configure IP address aswell, if you want this etherchannel to be L3                                                                                                                                                                                        |
| (config-if)#pagp port-priority <>                             | to configure which port is always selected for packet transmission                                                                                                                                                                                                                                      |

{% hint style="info" %}
The EtherChannel number does not need to match on both sides. Use matching numbers anyway. It reduces confusion later.
{% endhint %}

### Load-balancing options

EtherChannel balances the traffic load across the links in a channel by reducing part of the binary pattern formed from the addresses in the frame to a numerical value that selects one of the links in the channel. You can specify one of several different load-balancing modes, including load distribution based on MAC addresses, IP addresses, source addresses, destination addresses, or both source and destination addresses. The selected mode applies to all EtherChannels configured on the switch.

| (config)#port-channel load-balance { dst-ip \| dst-mac \| src-dst-ip \| src-dst-mac \| src-ip \| src-mac } | to set the load-distribution method based on the source/dest MAC/IP address show etherchannel load-balance                                                                                                                                                                                                      |
| ---------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| port-channel load-defer                                                                                    | allows ports to be bundled into port channels, but prevents the assignment of group mask values to these ports. This prevents the traffic from being forwarded to new stack members that would introduce data loss, since the data path is not fully established for new member. #show platform pm group-masks. |

### IOS XR: link bundles

The Link Bundling feature allows you to group multiple point-to-point links together into one logical link and provide higher bidirectional bandwidth, redundancy, and load balancing between two routers. A virtual interface is assigned to the bundled link. The component links can be dynamically added and deleted from the virtual interface.

The virtual interface is treated as a single interface on which one can configure an IP address and other software features used by the link bundle. Packets sent to the link bundle are forwarded to one of the links in the bundle.

A link bundle is simply a group of ports that are bundled together and act as a single link. The advantages of link bundles are as follows:

Multiple links can span several line cards to form a single interface. Thus, the failure of a single link does not cause a loss of connectivity.

Bundled interfaces increase bandwidth availability, because traffic is forwarded over all available members of the bundle. Therefore, traffic can flow on the available links if one of the links within a bundle fails. Bandwidth can be added without interrupting packet flow.

Cisco IOS XR software supports the following method of forming bundles of Ethernet interfaces:

IEEE 802.3ad—Standard technology that employs a Link Aggregation Control Protocol (LACP) to ensure that all the member links in a bundle are compatible. Links that are incompatible or have failed are automatically removed from a bundle.

Any type of Ethernet interfaces can be bundled, with or without the use of LACP (Link Aggregation Control Protocol).

Bundle membership can span across several line cards that are installed in a single router.

A single bundle supports maximum of 64 physical links.

Different link speeds are allowed within a single bundle, with a maximum of four times the speed difference between the members of the bundle.

Physical layer and link layer configuration are performed on individual member links of a bundle.

Configuration of network layer protocols and higher layer applications is performed on the bundle itself.

A bundle can be administratively enabled or disabled.

Each individual link within a bundle can be administratively enabled or disabled.

Ethernet link bundles are created in the same way as Ethernet channels, where the user enters the same configuration on both end systems.

The MAC address that is set on the bundle becomes the MAC address of the links within that bundle.

When LACP configured, each link within a bundle can be configured to allow different keepalive periods on different members.

Load balancing (the distribution of data between member links) is done by flow instead of by packet. Data is distributed to a link in proportion to the bandwidth of the link in relation to its bundle.

QoS is supported and is applied proportionally on each bundle member.

Link layer protocols, such as CDP and HDLC keepalives, work independently on each link within a bundle.

Upper layer protocols, such as routing updates and hellos, are sent over any member link of an interface bundle.

Bundled interfaces are point to point.

A link must be in the up state before it can be in distributing state in a bundle.

All links within a single bundle must be configured either to run LACP or EtherChannel (non-LACP). Mixed links within a single bundle are not supported.

A bundle interface can contain physical links and VLAN subinterfaces only.

Access Control List (ACL) configuration on link bundles is identical to ACL configuration on regular interfaces.

Multicast traffic is load balanced over the members of a bundle. For a given flow, internal processes select the member link and all traffic for that flow is sent over that member.

| RP/0/RSP0/CPU0:Router(config)# interface gig0/2/0/3 RP/0/RSP0/CPU0:Router(config-if)# bundle id 100 mode on\|active\|passive RP/0/RSP0/CPU0:Router(config)# interface gig0/2/0/4 RP/0/RSP0/CPU0:Router(config-if)# bundle id 100 mode on\|active\|passive | The default number of active links allowed in a single bundle is 8. To add interface members on the bundle If no mode is specified it falls into on mode - no LAC enabled                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RP/0/RSP0/CPU0:Router(config)# interface Bundle-Ether 100.1 l2transport RP/0/RSP0/CPU0:Router(config-subif)# encapsulation dot1q 11 RP/0/RSP0/CPU0:Router(config-subif)# commit                                                                           | To create subinterfaces on the bundle, use these commands:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| bundle maximum-active links 1                                                                                                                                                                                                                             | designates one active link and one link in standby mode that can take over immediately for a bundle if the active link fails (1:1 protection). Member interfaces that are in standby are displayed in the collecting state Election of active and standby The standby port is determined based on the Port ID and system ID. The system ID is based on system priority and MAC address. Whichever is lower is treated as higher priority. The lower system ID (or high-priority system) device will decide which port is in standby. The highest Port ID will be put into standby state. Both port ID and system priority are user-configurable |
| show bundle                                                                                                                                                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |

### Convergence notes (IOS XR)

A bundle member port switchover won't cause TE/FRR switchover. Link bundle convergence is around 20 msec for Layer 3 service and 3-4 msec for Layer 2 service. In 3.7.x release, the ASR 9000 Series supports only hot standby mode. In the 3.9 release, it supports both hot standby and warm standby modes. Warm standby is the default configuration.

With hot standby mode, the standby port is in the collecting state. Potentially, it will save a couple of msec moving to the forwarding state compared with warm standby

![](<../.gitbook/assets/Unknown image (539)>)
