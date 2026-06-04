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
  actions:
    visible: true
---

# High Availability

### Bidirectional Forwarding Detection (BFD)

**Bidirectional Forwarding Detection (BFD)** is a lightweight (low-overhead) probing feature that measures link parameters and rapidly detects failure in the forwarding path between two adjacent routers, including the interfaces, data links, and forwarding planes across all media types and encapsulations

BFD uses small, significantly faster (down to microseconds) control packets to monitor the liveliness of a path, which imposes minimal processing and bandwidth overhead on network devices compared to the potentially larger hello packets

BFD operates independently of any specific routing protocol and can be integrated in multiple processes of routing protocols to enhance their native failure detection mechanisms

It notifies routing protocols to initiate the routing table recalculation process when failures occur between peers

The Fundamental difference between the BFD Hellos and the Protocol Hellos (OSPF, ISIS, BGP and so on) is that BFD adjacencies do not go down on Control-Plane restarts (e.g. RSP failover) since the goal of BFD is to detect only the forwarding plane failures

The BFD protocol has no discovery mechanisms to detect neighbors; it is designed solely as an agent for other applications requiring fast failure detection.

Whenever a routing protocol that is configured to use BFD detects a new neighbor, it requests availability tracking from BFD.

When a routing protocol (BGP, for example) discovers a neighbor, it sends a request to the local BFD process to initiate a BFD neighbor session with the BGP neighbor router.

Then the BFD neighbor session with the BGP neighbor router is established. If there is a failure on the link between neighbors, the BFD neighbor session with the BGP neighbor router is torn down, then BFD notifies the local BGP process that the BFD neighbor is no longer reachable and eventually the local BGP process tears down the BGP neighbor relationship. If an alternative path is available, the routers will immediately start converging on it.

SD-WAN uses BFD primarily to detect the state of the path and its parameters (loss/latency/jitter and IPsec tunnel MTU))

**BFD Asynchronous mode** establishes and maintains BFD session by exchanging BFD control packets between peers at consistent rate; must be configured on both peers.

Control packet streams are independent of each other and do not work in a request/response model

If a specified number of packets in a row are not received by the other system, the session is declared down.

UDP port 3784 is used for the Control plane of BFD

UDP port 3785 is used for the Asynchronous mode of BFD

**Demand Mode** sessions are established on-demand, triggered by initiating traffic between routers, which provides additional processing reduction

Once a BFD session is established, such a system may ask the other system to stop sending BFD Control packets, except when the system feels the need to verify connectivity explicitly, in which case a short sequence of BFD Control packets is exchanged, and then the far system quiesces. Demand mode may operate independently in each direction, or simultaneously.

**BFD Echo mode (default)** is an extension to both modes. A stream of BFD Echo packets is transmitted in such a way as to have the other system loop them back through its forwarding path. If a number of packets of the echoed data stream are not received, the session is declared to be down. The Echo function may be used with either Asynchronous or Demand mode. Since the Echo function is handling the task of detection, the rate of periodic transmission of Control packets may be reduced (in the case of Asynchronous mode) or eliminated completely (in the case of Demand mode). Simply said remote router does not process BFD packets, instead echoes them back to the sender, which minimizes CPU processing load for the remote router

Echo packets are IP packets that are addressed to the router itself but sent to the Layer 2 address of the next-hop node

BFD echo packets are looped back through the forwarding path only of the BFD peer and are not processed by any protocol stack. So, packets sent by BFD router "Peer A" can be sent with both the source and destination address of Peer A. BFD echo packets are sent in addition to BFD control packets.

{% hint style="info" %}
Cisco IOS XR supports **asynchronous mode** only. Echo is enabled by default.
{% endhint %}

![](<../.gitbook/assets/Unknown image (1071)>)

**BFD control packet payload**

**Desired Min TX Interval:** Represents the minimum time interval between sending BFD control packets from the local system to its neighbor

**Detect Mult (Detection Time Multiplier):** Multiplier value applied to the "Desired Min TX Interval" to calculate the detection time that a system will wait for a BFD packet from its peer before declaring the path as down

**Required Min RX Interval:** minimum time interval within which the local system expects to receive BFD control packets from its peer, the local system will consider the neighbor as unreachable, if interval is not met

**Required Min Echo RX Interval:** specific to BFD echo mode for echo packet reception timing

**BFD Template:** instead of indvidual BFD configurations, one BFD template can be defined and used for multiple protocols

Differences in BFD in Cisco IOS XR Software and Cisco IOS Software

In Cisco IOS XR software, BFD is an application that is configured under a dynamic routing protocol, such as an OSPF or BGP instance. This is not the case for BFD in Cisco IOS software, where BFD is only configured on an interface.

Demand mode is not supported in Cisco IOS XR software.

Instead of using a dynamic routing protocol to establish a BFD neighbor, you can establish a specific BFD peer or neighbor for BFD responses in Cisco IOS XR software using a method of static routing to define that path. In fact, you must configure a static route for BFD if you do not configure BFD under a dynamic routing protocol in Cisco IOS XR software

However, this standard echo failure detection does not address latency between transmission and receipt of any specific echo packet, which can build beyond (I x M) over the course of the BFD session. In this case, BFD will not declare a neighbor down as long as any echo packet continues to be received within the multiplier window and resets the counter to zero.

Beginning in Cisco IOS XR Release 4.0.1, you can configure the router to detect the actual latency between transmitted and received echo packets on non-bundle interfaces and also take down the session when the latency exceeds configured thresholds for that roundtrip latency

In addition, you can verify that the echo packet path is within specified latency tolerances before starting a BFD session. With echo startup validation, an echo packet is periodically transmitted on the link while it is down to verify successful transmission within the configured latency before allowing the BFD session to change state

Refer to the official config guide for the platform.

#### IOS XR: BFD over bundle interfaces

[https://xrdocs.io/ncs5500/tutorials/coexistence-between-bfd-over-bundle-and-bfd-over-logical-bundle](https://xrdocs.io/ncs5500/tutorials/coexistence-between-bfd-over-bundle-and-bfd-over-logical-bundle)

#### BFD over Bundle (BoB)

**BFD over Bundle (BoB)** runs a separate BFD session on each physical member inside the bundle.

This means each physical link is individually monitored.

If a member interface fails, BFD detects it fast because the session on that member goes down.

Multiple sessions exist — one for each member link.

But BoB does not check the logical bundle interface itself at L3 (the IP level).

#### BFD over Logical Bundle (BLB)

**BFD over Logical Bundle (BLB)** runs one BFD session for the entire bundle as a single logical interface.

This session looks at the bundle from a higher, IP-level perspective.

Only one member link is monitored at a time, selected by the bundle hashing.

Therefore if other member links fail that aren’t currently monitored, BLB won’t notice.

So:

BoB = many BFD sessions, one per actual link.

BLB = one BFD session for the logical interface itself.

#### Why the problem exists

Each method alone has limitations:

BoB alone

Doesn’t verify that the logical bundle interface (the L3 address on the bundle) is healthy.

Doesn’t work on subinterfaces.

Only lower-layer detection.

BLB alone

Only one member is monitored at a time.

If other member links fail that aren’t being monitored at that moment, BLB won’t detect it.

If the member currently monitored fails, BLB will mark the whole bundle down even if others are still good.

#### Why coexistence helps

Running both BoB and BLB at the same time gives you the best of both worlds:

BoB gives fast detection of individual link failures across all bundle members.

BLB gives a true logical/Layer-3 check on the bundle interface so routing protocols can react correctly to real IP-level reachability changes.

Together, they improve failure detection and convergence speed more than either alone.

#### Two modes of coexistence

When both are enabled, you have two ways they can operate:

#### **Inherit mode**

BLB doesn’t send any BFD packets itself; it borrows or “inherits” the state from BoB.

This means BLB will only be active based on the existing BoB sessions.

If BoB goes down, BLB goes down too.

#### **Logical mode**

BLB actually runs its own real BFD session in addition to BoB.

Even when BoB is active, BLB exchanges its own packets and independently checks the logical bundle path.

This gives independent monitoring for both physical and logical.

Exception: if the bundle interface itself has an IPv4 address, BLB may still inherit under some conditions.

BoB is recommended and provides faster detection versus BLB

Use both together to get both fast physical detection and correct logical reachability notification, especially when routing protocols depend on BFD.

### IP SLA

**SLA (Service Level Agreement)** is a contract between a customer and a service provider to provide a certain quality of service for IP connectivity, technical support response times or operational processes

IP SLA is an embedded probing feature used to test and monitor end-to-end L3 connectivity

Metrics like Delay , Jitter, Packet loss , Path Connectivity, voice quality scores and interface states can be measured

![](<../.gitbook/assets/Unknown image (1072)>)

| R1(config)# ip sla                                                          | show ip sla \[summary \| statistics \| configuration 1] sh run \| s sla |
| --------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| R1(config-ip-sla)# icmp-echo 192.168.14.100 source-interface Loopback0      |                                                                         |
| R1(config-ip-sla-echo)#frequency 300                                        | probe every 300 seconds                                                 |
| R1(config-ip-sla-echo)#threshold 1000                                       | interval to assume destination unresponsive in milliseconds             |
| R1(config-ip-sla-echo)#tos 184                                              | applying QoS tag                                                        |
| R1(config)# ip sla schedule 1 life forever start-time now                   | ip sla execution                                                        |
| R1(config)# ip sla 2                                                        |                                                                         |
| R1(config-ip-sla)# http get [http://192.168.14.100](http://192.168.14.100/) | tests HTTP get requests towards destination                             |
| R1(config-ip-sla-http)# frequency 90                                        |                                                                         |
| R1(config)# ip sla schedule 2 start-time now life forever                   |                                                                         |

#### UDP jitter operation

**TCP-connect** involves establishing a full connection whereas **UDP-echo** is used to measure response times and test end-to-end connectivity

Before you configure a UDP jitter operation on the source device, you must enable the IP SLAs responder on the target device (the operational target)

| ip sla responder ip sla responder udp-echo | Configuration on Destination device additional parameters can be specified, such as timeout, frequency, DSCP, etc. |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------ |
| ip sla udp-jitter endpoint-list <>         | Configuration on Source device                                                                                     |

#### TCP connect operation

**TCP connect operation** measures the response time taken to perform a TCP Connect operation between devices to test application availability

Server and application connection performance can be tested by simulating HTTP, Telnet, SQL, and other types of connection

| ip sla \<operation\_number>tcp-connect \<destination\_ip\_address> \<destination\_port> | \[control disable] use this option when target host is not running IP SLA |
| --------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |

![](<../.gitbook/assets/Unknown image (1073)>)

### IP SLA with object tracking

**Interface tracking** monitor the status of specific interface on the device

**Object tracking** can monitor the state of an object like the IP SLA tracking IP reachability of specific IP address

The state change of either of the two can trigger a specific action such as dynamically adjusting the FHRP router priority

If a tracked object goes down, the priority decrements

If a tracked object comes back up, the priority increments - the default decrement/increment is 10

It is also used in PBR, so that when the IP state changes, it can dynamically adjust the next-hop or remove static route from the routing table

![](<../.gitbook/assets/Unknown image (1074)>)

| ip sla 1                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| icmp-echo 8.8.8.8 source-interface Gi0/0      | we have to specify proper WAN interface/IP of the tracked primary circuit In case we wanted to apply tracking for the secondary circuit, we would have to create new separate IP sla probe and tracking object for the secondary circuit with it's source interface/WAN IP                                                                                                                                                                                                                                                                                                                |
| frequency 5                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| threshold 900                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ip sla schedule 1 start-time now life forever |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| track 1 ip sla 1 \[reachability\|state]       | track operation ID associates to the ip sla operation ID reachability Use case: When you want to determine if the IP SLA operation can successfully reach the target (e.g., ping or probe response). Behavior: The tracking object evaluates the success or failure of the IP SLA operation's reachability state Use case: When you want to monitor the operational status (state) of the IP SLA process itself, regardless of whether it is successful or not. Behavior: The tracking object evaluates whether the IP SLA process is in a valid running state (not stopped or inactive). |
| track 2 interface Ethernet0/0 line-protocol   | Interface Tracking                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| delay down 5 up 5                             | when line-protocol flaps, the up and down delay will keep tracking down for 5 seconds even when the link goes back up in case another link flap occurs, to prevent false negatives                                                                                                                                                                                                                                                                                                                                                                                                        |
| ip route 0.0.0.0 0.0.0.0 1.1.1.1 track 1      | associates a tracking object to the static route The route's availability will depend on the status of the tracked object If the tracking object reports a failure, the route will be removed from the routing table. With this approach, you can configure a second static route with a higher AD to the secondary ISP so that traffic can be redirected from the primary "tracked" path to the secondary path in the event of a probe failure                                                                                                                                           |
| ip route 0.0.0.0 0.0.0.0 2.2.2.2 66           | floating static route with increased AD for the backup circuit that will failover primary in case of failure                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| show track brief                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |

Tracking can be used as an object in EEM

To achieve automatic failover for NAT when the primary link goes down, you can combine Cisco's IP SLA, tracking, and policy-based NAT features

| ip sla 1 icmp-echo 8.8.8.8 source-interface GigabitEthernet0/0 frequency 5 ip sla schedule 1 life forever start-time now                                                                                                                                                                      |   |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| track 1 ip sla 1 reachability                                                                                                                                                                                                                                                                 |   |
| ip route 0.0.0.0 0.0.0.0 X.X.X.X track 1 name primary                                                                                                                                                                                                                                         |   |
| ip route 0.0.0.0 0.0.0.0 X.X.X.X 10 name secondary                                                                                                                                                                                                                                            |   |
| ip nat inside source static X.X.X.X X.X.X.X                                                                                                                                                                                                                                                   |   |
| event manager applet NAT\_Primary\_Up event track 1 state up action 1.0 cli command "enable" action 2.0 cli command "no ip nat inside source list 100 interface GigabitEthernet0/1 overload" action 3.0 cli command "ip nat inside source list 100 interface GigabitEthernet0/0 overload"     |   |
| event manager applet NAT\_Primary\_Down event track 1 state down action 1.0 cli command "enable" action 2.0 cli command "no ip nat inside source list 100 interface GigabitEthernet0/0 overload" action 3.0 cli command "ip nat inside source list 100 interface GigabitEthernet0/1 overload" |   |

### Hardware-based redundancy

**Route Processor (RP)** is a hot-swappable hardware component responsible for system operation

It handles control plane operations and tasks such as running routing protocols (e.g., OSPF, BGP), maintaining routing tables, and making forwarding decisions based on the control plane information and also system initialization

The RP is primarily concerned with control plane functions and does not directly participate in data forwarding or packet switching.

In a dual RSP system, one serves as primary that actively ensures control ooperations and the second operates on standby mode, ready to take over, if the primary RP fails e.g hardware reundancy

**Route Switch Processor (RSP)** is a more comprehensive component that combines the functionalities of both a Route Processor and a Switching Processor

In addition to control plane operations, the RSP also handles data plane functions, including packet forwarding and switching.

RSPs are typically found in high-end Cisco routers that require both advanced routing capabilities and high-speed packet forwarding.

R0 and R1 refers to the route processor

F0 and F1 refer to fabric cards

LC is line card

![](<../.gitbook/assets/Unknown image (1075)>)

### Software-based redundancy

On the Cisco 8500 Series Catalyst Edge Platform, Cisco IOS runs as one of the many processes. This architecture supports software redundancy opportunities. Specifically, a standby Cisco IOS process is available on the same Route Processor as the active Cisco IOS process. In the event of a Cisco IOS failure, the system switches to the standby Cisco IOS process.

On the Cisco 8500 Series Catalyst Edge Platform, Stateful Switchover (SSO) can be used to enable a second IOS process.

Stateful Switchover is particularly useful in conjunction with Nonstop Forwarding. SSO allows the dual IOS processes to maintain state at all times, and Nonstop Forwarding lets a switchover happen seamlessly when a switchover occurs

![](<../.gitbook/assets/Unknown image (1076)>)

### Stateful Switchover (SSO)

**Stateful Switchover (SSO)** allows a standby RP or supervisor module to take over the forwarding and control plane functions seamlessly in case of a failure or planned maintenance on the active RP or supervisor

With SSO, the standby device maintains synchronized state of control plane with the active device, ensuring a smooth transition without disrupting network traffic or dropping active sessions

Layer 2 forwarding is maintained during failover

Layer 3 protocol states are not synchronized, thus L3 protocol adjacencies go down during the failover, so the Layer 3 forwarding is interrupted

| Router(config)# redundancy      | to enable SSO                                                                                                                                                                                                                                                                                                                                                  |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Router(config)# mode sso        | #show redundancy #show platform                                                                                                                                                                                                                                                                                                                                |
| redundancy force-switchover     | initiates a switchover at the supervisor or control plane level. This reboots the active in the process                                                                                                                                                                                                                                                        |
| redundancy force-switchover fp: | initiates a switchover at the forwarding processor (FP) level It forces the device to switch packet forwarding responsibilities from the active forwarding processor to the standby forwarding processor This can be useful in situations where the forwarding processor is experiencing issues or to test failover mechanisms at the packet forwarding level. |
| hw-module slot R1 reload        | reloading specified module/RSP                                                                                                                                                                                                                                                                                                                                 |

![](<../.gitbook/assets/Unknown image (1077)>)

### Nonstop Forwarding (NSF, Cisco)

**Nonstop Forwarding (NSF, Cisco)** enables checkpointing of FIB from Active Route Processor to Standby RP, so linecards can continue to forward packets at L3 while the RP fails over, however routing adjecencies are interrupted

NSF does not require cooperation with neighboring device, however the routes from the failed device are dropped

NSF is enabled by default (on devices that support it) if SSO is enabled. Cisco support both GR and NSF

Cisco NSF is supported by four protocols for routing (BGP, EIGRP, OSPF, and IS-IS) and by Cisco Express Forwarding for forwarding

**NSF-capable** if configured to support NSF and can rebuild routing information from **NSF-aware** or NSF-capable neighbors

The routing protocols (for example, BGP) run only on the active RP, and they receive routing updates from their neighbor routers. Routing protocols do not run on the standby RP.

NSF awareness is enabled automatically in supported software images for Interior Gateway Protocols, such as EIGRP, IS-IS, and OSPF.

In BGP, global NSF awareness is not enabled automatically and must be started by issuing the bgp graceful-restart command in router configuration mode.

If Cisco NSF is not configured, a neighboring device will tear down the adjacency during a switchover. The result would be a routing flap that would spread across the network

Using Cisco NSF, the routing protocols on an NSF-capable device requests that the NSF-aware neighbor devices send routing information to help rebuild the routing tables.

The Cisco NSF-aware neighbor will not tear down the neighbor relationship.

It will re-establish the neighbor relationship with the Cisco NSF-capable device and will send routing information to synchronize the routing table on an NSF-capable device

Restrictions for Cisco Nonstop Forwarding with Stateful Switchover

NSF capability is supported for IPv4 routing protocols only

For NSF operation, you must have SSO configured on the device.

For IETF, all neighboring devices must be running an NSF-aware software image.

HSRP is not supported with NSF SSO

### Graceful Restart (IETF)

**Graceful Restart (IETF)**, with NSF, maintains L3 forwarding during an RP failover. It allows peers of a device performing an RP failover to maintain routes received from the failovering device and continue to forward traffic, even if the routing protocol adjacency has been lost. The length of time that the neighbors will continue forwarding traffic is called the grace period.

GR requires communication and cooperation with neighboring devices.

Downside to this is that even if the device is actually down (and not just doing an RP failover), the other devices will continue to forward traffic to the failed device, creating blackhole

#### NSF for BGP (sequence)

1. The BGP process of router R1 begins and establishes a peering relationship with router R2. It sends an Open message to R2. The Open message includes the graceful restart capability (code 64), Address Family of IPv4, and Subsequent Address Family ID of unicast. Because R2 supports graceful restart, it also sends an acknowledgment through its own Open Message, which contains GR=64 and AF=IPv4.
2. A route processor switchover occurs, and the router R1 BGP process restarts on the newly active route processor. R1 does not have a Routing Information Base (RIB) on this Route Processor and must reacquire it from its peer routers. R1 will continue to forward IP packets destined for (or through) peer routers (R2) using the last updated FIB and CEF table
3. When the receiving router (R2) detects that the TCP session between it and the restarting router is cleared, it immediately marks routes, learned from the restarting router, as stale. Router R2 also initializes a restart-timer for the restarting router. The default setting for this timer is 120 seconds. The restart-timer is the amount of time that a receiving router will wait for an Open message from the restarting router. A receiving router will remove all stale routes unless it receives an Open message from the restarting router within the specified restart-time. When R2 receives the R1 Open message, the restart-timer is reset. During this time, routers R1 and R2 continue to forward traffic using the last updated CEF table
4. The R1 BGP process has been initialized. It will now attempt to reestablish a BGP session with R2. It first establishes a new TCP session, and then sends an Open message (Restart State bit set, Restart Time = n, and Forwarding State = IPv4). By default, Restart-time is 120 seconds and it is configurable. When R2 receives this Open message, it resets its own restart-timer and starts a stalepath-timer. The stalepath-timer, by default, is 360 seconds and is also configurable - might be increased in large networks
5. Both routers successfully re-establish their session. At this point, if R2 recognizes that the Forwarding State in the R1 Open message is not set for IPv4 (Normally, the Forwarding State will be set for IPv4), it immediately removes any stale routes, which it had learned from the restarting router, and recomputes its routing database.
6. R2 will begin to send UPDATE messages to R1. These messages contain IP prefix information, and R1 will process them accordingly. R1 starts an update-delay timer and waits up to 120 seconds to receive end-of-RIB (EOR) e.g. end of record from all its NSF-peers. R1 will not start the BGP Route Selection Process until an EOR indication is received from all peers (or the BGP update-delay timer expires). A new routing information database is available after the Route Selection Process is finished, and the CEF information is updated accordingly.
7. When R1 receives EOR from all its peers, it will begin the BGP Route Selection Process.
8. When this process is complete, it will begin to send Update messages with prefix information to R2. R1 concludes this process by sending an EOR indication to R2 so that R2, in turn, can start its route selection process.
9. While R2 waits for an EOR, it also monitors stalepath-time. If the timer expires, all stale routes will be removed and "normal" BGP processes will be in effect. When R2 has completed its route selection process, then any stale entries in BGP will be refreshed with newer information or removed from the BGP RIB and FIB. The network is now converged.

![](<../.gitbook/assets/Unknown image (1078)>)

![](<../.gitbook/assets/Unknown image (1079)>)

#### NSF for OSPF

When OSPF is enabled on an Cisco NSF router with dual Route Processors, the routing process runs only on the active Route Processor. The standby Route Processor does not contain any OSPF-related routing information, no LSDB, nor does it maintain a neighbor data structure. When the switchover occurs, the neighbor relationships must be reestablished.

OSPF Hello protocol is responsible for establishing and maintaining neighbor relationships and ensuring that communication between neighbors is bidirectional. Bidirectional communication is indicated when the router sees itself listed in its neighbor's Hello Packet.

When switchover occurs, the restarting router tries to reestablish neighbor adjacency by sending out hello packets. Neighbor state information does not exist in the new active Route Processor, so the hello packet will not contain any neighbor information in the neighbor list of the hello packet. Without any additional protocol changes, a neighbor receiving this hello packet would fail the two-way check and then reset the existing neighbor adjacency with the restarting router. The neighbor router would simultaneously flood update LSAs to reflect the adjacency change, in turn causing routing disruption.

To avoid the neighbor adjacency flap, the Cisco implementation for OSPF Cisco NSF introduces a new bit, Restart Signal, into Hello protocol. A hello packet with the Restart Signal-bit set indicates that the router is undergoing a Route Processor switchover. Upon receiving this hello packet, a neighbor would follow the OSPF Cisco NSF procedures and would ignore the two-way connectivity check.

The Restart Signal-bit is stored in Extended Options TLV (EO-TLV) in the Link Local Signaling (LLS) data block of a hello packet. The existence of the LLS data block on a hello packet is indicated by an L-bit that is introduced in the IETF draft. The L-bit is set in the OSPF Options field. The value of the bit is 0x10.

Hello packets with Restart Signal-bit set during Cisco NSF procedures are sent out in two-second intervals. They are sent out this way to expedite the convergence time after a switchover. This two-second interval of Hello with Restart Signal-bit set is referred to as "Fast Hello." The Restart Signal-bit is cleared when the neighbor adjacency is resumed

#### LSDB resynchronization

Because OSPF Cisco NSF does not maintain OSPF state information on the standby Route Processor, the newly active Route Processor needs to synchronize its LSDB with its neighbors.

Cisco OSPF Cisco NSF addresses this issue by using out-of-band (OOB) LSDB resynchronization. The OOB-Resync mechanism enables full LSDB resynchronization after the neighbor relationship is established.

To announce this OOB-Resync capability, a new bitm LR-bit (LSDB Resynchronization) is defined. The LR-bit is set in the EO-TLV in the link local signaling (LLS) data block. This data block is included on all Hello and Database Description (DBD) packets.

In addition to the LR-bit, a new R-bit is also introduced in the DBD packet. The R-bit is used to indicate that the OOB-Resync procedure is active. This R-bit is set in the options field flag of DBD packets.

With the introduction of the LR-bit, an OSPF Cisco NSF router can discern whether an OSPF neighbor is capable of supporting its Cisco NSF procedures. When OSPF is operating and receiving hello packets with the presence of the LR-bit from its neighbors, it knows that the neighbor is NSF-aware and can execute the Cisco NSF procedures. With the introduction of the R-bit, a router can determine whether a normal LSDB synchronization or an OOB-Resync is taking place.

Note that the LSDB synchronization process using the OOB-Resync mechanism does not occur among all the adjacent neighbors. It occurs between routers in the same way as defined in the existing LSDB synchronization method in RFC 2328. For example, in a broadcast network, if the restarting router is not a Designated Router or Backup DR (BDR), it will do the OOB-Resync with the Designated Router only. If the restarting router has a point-to-point connection with its NSF-aware neighbor, it will do the OOB-Resync with that neighbor.

#### Operation

1. The restarting router (R1) marks routes in the FIB "stale." It also starts an Cisco NSF restart timer, which will trigger DR/BDR selection and OOB-Resync.
2. R1 multicasts out fast hello packets with RS-bit set, signaling the beginning of OSPF Cisco NSF procedures. The LR-bit is also set. The neighbor list in these hello packets is empty because there is no neighbor information that is retained after the switchover. Note that NSF-capable and NSF-aware neighbors always have their LR-bit set in the hello packets, regardless of Cisco NSF process status.
3. R2 receives the hello packets with RS-bit set from R1, and knows that R1 is undergoing an Cisco NSF restart procedure. The 2-Way check is, therefore, ignored. In the meantime, it keeps the neighbor's Finite State Machine (FSM) in FULL state. A timer, called Resync-Timeout, is started at this point. This timer limits the delay between the first seen hello packet with RS-bit set and initiation of the OOB-Resync.

Note

The OOB-Resync timer is set to the maximum value of either the dead-interval timer or 40 seconds by default. For example, if the dead-interval timer is set to a value lower than 40 seconds, the OOB-Resync timer will still be 40 seconds. Conversely, if the dead-interval timer is raised to some value greater than 40 seconds (for some reason specific to an individual network configuration), then the OOB-Resync timer will be set to the same value. The timer is set automatically, and requires no special configuration on the router. A CLI command allows explicit configuration of the OOB-Resync timer: ip ospf resync-timeout seconds. If desired, this command can be enabled on the NSF-aware peers of the restarting router. The command is enabled on a per-interface basi

1. R2 sends unicast hello packets back to R1. Instead of waiting for normal Hello timer, R2 immediately replies to those hello packets. Also, note that the hello packets from R2 do not have RS-bit set.
2. When R1 receives the fast Hello from R2, it moves the neighbor adjacency state to 2-Way. However, from an Cisco NSF perspective, the state is considered Full.
3. R1 waits until the Cisco NSF restart timer expires, which is 20 seconds. When this timer is expired, it starts DR/BDR election and OOB LSDB resynchronization. This "wait time" ensures that the restarting router can learn all its neighbors' states because there may be an NSF-unaware router on the segment. Also, the RS-bit is now cleared. After DR/BDR selection, R1 moves its neighbor adjacency state to exstart.
4. Note If the (OSPF) nsf \[enforce global] CLI option is configured, then as soon as any Hello is received from a peer without the LR-bit set, OSPF Cisco NSF is disabled and DR/BDR election proceeds immediately.
5. R1 begins to send DBD packets with R-bit set to R2.
6. When R2 receives the DBD with R-bit set from R1, R2 moves the neighbor Finite State Machine (FSM) (FULL) to exstart and starts LSDB synchronization. R2 cancels the resync timer.
7. R1 and R2 now perform LSDB synchronization in the same manner as normal LSDB synchronization described in RFC 2328. If R1 receives self-generated LSAs during the LSDB synchronization process, it will not prematurely flush out the LSAs. Instead, R1 stores the LSAs and marks them as "stale."
8. OOB-Resync is complete at this stage. R1 starts generating router LSAs and network LSAs. It does not send those LSAs to its neighbor unless they are different from the ones learned from its neighbor earlier. If they are same, it simply clears the "stale" status for those LSAs. At this stage, R1 also starts to update its RIB and FIB
9. R1 detects that the Cisco NSF flush timer has expired (the default Cisco NSF flush timer is 60 sec). It flushes all the LSAs still present in the database with a "stale" flag set.
10. OSPF Cisco NSF is now complete.

![](<../.gitbook/assets/Unknown image (1080)>)

![](<../.gitbook/assets/Unknown image (1081)>)

**Restarting Mode**--Also known as IETF NSF-restarting mode or graceful-restarting mode. In this mode, the OSPF router process is performing non-stop forwarding recovery because of an RP switchover; thisbmay result from an RP crash or a software upgrade on the active RP. The router uses NSF to handle RP switchover while allowing neighbor relationships to remain up.

**Helper Mode**--Also known as IETF NSF-awareness. In this mode, the neighboring router is restarting and helping in the NSF recovery.

The OSPF RFC 3623 Graceful Restart Helper Mode feature is enabled by default. Disabling this feature is

not recommended because the disabled neighbor will detect the lost adjacency and the graceful restart process

will be terminated on the restarting neighbor router.

The strict LSA checking feature allows a helper router to terminate the graceful restart process if it detects a

changed LSA that would cause flooding during the graceful restart process. Strict LSA checking is disabled

by default. You can enable strict LSA checking when there is a change to an LSA that would be flooded to

the restarting router. You can configure strict LSA checking on both NSF-aware and NSF-capable routers;

however, it becomes effective only when the router is in helper mode

| (config-router)# nsf ietf helper |   |
| -------------------------------- | - |

[https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute\_ospf/configuration/15-mt/iro-15-mt-book/iro-restart.pdf](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_ospf/configuration/15-mt/iro-15-mt-book/iro-restart.pdf)

#### NSF for IS-IS

IS-IS is similar to the OSPF and the same problems must be addressed to ensure NSF

There are two solutions to address this problem. One is a stateful routing solution that is Cisco-specific and the other is much like the previously described methods used for OSPF and BGP. The Cisco IOS Software-specific solution uses checkpoint facilities to back up the states of the IS-IS adjacencies and database on the standby RP. The second solution is based on IETF work and uses a new TLV in the IS-IS Hello PDU. Therefore, the second method requires a supportive neighbor to work.

IETF solution is stateless:

Does not checkpoint the content of the LSP database

Does not checkpoint the adjacency

Restarting router informs neighbors about restart:

Sends a hello message with the restart TLV having the RR bit set

All routers perform Complete Sequence Number PDU (CSNP) synchronizations

#### IETF operation (stateless)

1. R1 restarts.
2. R1 sends a hello message including TLV 211 with RR bit set and RA bit cleared indicating that it has restarted.
3. R2 receives R1's hello message.
4. R2, because it is IS-IS NSF-aware, responds with a hello message including TLV 211 with RR bit cleared and RA bit set: R2 indicates that it acknowledges the previous Hello received from R1.
5. R1 receives hello message from R2.
6. If the interface is a Point-to-Point interface, or if R2 has the highest router priority (with highest source MAC address breaking ties) among those routers whose IS-IS Hellos (IIHs) contain the restart TLV (excluding R1), R2 sends a complete set of CSNPs. When both this CSNP and the above hello message sent in 4 are received, the Adjacency timer is cancelled. If the adjacency timer expires, R1 resends the hello message with RR bit set.

![](<../.gitbook/assets/Unknown image (1082)>)

#### Cisco NSF (stateful, Cisco-specific)

Cisco implementation is stateful:

Full adjacency and LSP information saved, or checkpointed, to the standby RP

Following a switchover, newly active RP uses checkpointed data and quickly restores the routing information

Hellos are sent from the newly Active RP prior to peer hold timer expiration

Neighbors are unaware of the restart

1. R1 restarts.
2. R1 sends a hello message including TLV 211 with RR bit set and RA bit cleared indicating that it has restarted.
3. R2 receives hello message from R1.
4. R2, because it is NOT NSF-aware, responds with a hello message without TLV 211. The adjacency is dropped.
5. R1 receives the hello message without TLV 211, reinitializing the adjacency with R2. R1 sends a hello message with TLV 211 but RR and RA bits are cleared. The purpose is not necessarily to reinitialize the adjacency (because R2 has already done it), but to do the "normal" adjacency acquisition process.

![](<../.gitbook/assets/Unknown image (1083)>)

### LDP graceful restart

**LDP graceful restart** is comparable to EIGRP/OSPF/BGP graceful restart, which means the forwarding plane will stay active even if the control plane is going to be restarted

When you enable MPLS LDP graceful restart on a router that peers with an MPLS LDP SSO- or NSF-enabled router, the SSO- or NSF-enabled router can maintain its forwarding state when the LDP session between them is interrupted. While the SSO/NSF -enabled router recovers, the peer router forwards packets using stale information. This action enables the SSO- or NSF-enabled router to become operational more quickly

LDP uses SSO, NSF, and graceful restart to allow a Route Processor (RP) to recover from disruption in control plane service (specifically, the LDP component) without losing its MPLS forwarding state. LDP NSF works with LDP sessions between directly connected peers and with peers that are not directly connected (targeted sessions).

When an LDP graceful restart session is established and there is control plane failure, the peer LSR starts graceful restart procedures, initially keeps the forwarding state information pertaining to the restarting peer, and marks this state as stale. If the restarting peer does not reconnect within the reconnect timeout, the stale forwarding state is removed. If the restarting peer reconnects within the reconnect time period, it is provided recovery time to resynchronize with its peer. After this time, any unsynchronized state is removed.

An RP that is configured to perform LDP NSF includes the Fault Tolerant (FT) Type Length Value (TLV) in the LDP initialization message. The RP sends the LDP initialization message to a neighbor to establish an LDP session.

The FT session TLV includes the following information:

The Learn from Network (L) flag is set to 1, which indicates that the RP is configured to perform LDP Graceful Restart.

The Reconnect Timeout field shows the time (in milliseconds) that the neighbor should wait for a reconnection if the LDP session is lost. This field is set to 120 seconds and cannot be configured.

The Recovery Time field shows the time (in milliseconds) that the neighbor should retain the MPLS forwarding state during a recovery. If a neighbor did not preserve the MPLS forwarding state before the restart of the control plane, the neighbor sets the recovery time to 0.

| Router(config)# mpls ldp graceful-restart                                          | ## Configuring LDP graceful restart                                                            |
| ---------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| graceful-restart \[reconnect-timeout seconds \| forwarding-state-holdtime seconds] | to enable LDP NSR on Cisco IOS XR Software use the nsr command in mpls ldp configuration mode. |
| show mpls ldp graceful-restart                                                     |                                                                                                |
| show mpls ldp neighbor brief                                                       |                                                                                                |

### Nonstop Routing (NSR)

**Nonstop Routing (NSR)** allows an RP failover, process restart, or in-service upgrade to be invisible to peer routers and ensures that there is minimal performance or processing impact. Routing protocol interactions between routers are not impacted by NSR. NSR is built on the warm standby extensions. It alleviates the requirement for Cisco NSF and IETF graceful restart protocol extensions.

Other routers do not need to be NSF-capable or NSF-aware to benefit from NSR capabilities. When Nonstop Routing is used, peer networking devices have no knowledge of any event on the switching over router. All information needed to continue the routing protocol peering state is transferred to the standby processor, so it can continue immediately upon a switchover. Therefore, an obvious prerequisite for NSR is redundant route processors.

FIB, routing protocol state information is checkpointed also to the standby RP, thus no cooperation from neighbors is needed, the neighbors aren't aware that an RP failover is happening.

To maintain full forwarding information during an RP failover you need SSO + NSF + GR or NSR

After an active switch or RP change, the new active switch or RP sends an OSPF NSF signal to neighboring NSF-aware devices.

These devices recognize the signal and refrain from resetting the neighbor relationship with the stack, ensuring continuity of routing operations

NSR is a method to achieve High Availability (HA) of the routing protocols. TCP connections and the routing protocol sessions are migrated from the active RP to standby RP after the RP failover without letting the peers know about the failover. Currently, the sessions terminate and the protocols running on the standby RP reestablish the sessions after the standby RP goes active. Graceful Restart (GR) extensions are used in place of NSR to prevent traffic loss during an RP failover but GR has several drawbacks.

| Router#configure Router(config)#nsr process-failures switchover Router(config)#commit | Configuring Failover as a Recovery Action for NSR When the active TCP or the NSR client of the active TCP terminates or restarts, the TCP sessions go down. To continue to provide NSR, failover is configured as a recovery action. If failover is configured, a switchover is initiated if the active TCP or an active application (for example, LDP, OSPF, and so forth) restarts or terminates. [https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/ip-addresses/710x/b-ip-addresses-cg-8k-710x/configuring-transports.html](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/ip-addresses/710x/b-ip-addresses-cg-8k-710x/configuring-transports.html) |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

New syntax for OSPF and BGP shows that those are enabled by default

RP/0/RSP0/CPU0:router# \[no] nsr disable

Useful show commands to review the applied configuration:

RP/0/RSP0/CPU0:router# show ospf process-id

RP/0/RSP0/CPU0:router# show bgp nsr

RP/0/RSP0/CPU0:router# show redundancy

Use the show redundancy command to confirm that your IOS XR router has both Active and Standby RP cards.

NSR feature had to be enabled manually in initial Cisco IOS XR releases, with the single command configured on global protocol level:

router ospf 1

nsr

….

router bgp 1

nsr \[disable]

…

router isis 1

nsr \[disable]

What are two factors to consider when implementing NSR High Availability on an MPLS PE router? (Choose two.)

A. It requires all PE-CE sessions to support NSR

B. It requires routing protocol extensions

C. It consumes more memory and CPU resources than NSF

D. It operates normally without NSR support on the PE peers

E. It cannot sync state information across redundant RPs

## Gateway Redundancy

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

| (config-if)#standby version 2                                   | must be explicitly configured to run IPv6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| --------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-if)#standby ip                                          | IPv4 config The basic HSRP configuration only requires you to select the HSRP number of the group and set the IP address of the virtual router                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| (config-if)#standby ipv6 autoconfig OR (config-if)#standby ipv6 | IPv6 config it is recommended to use automatically configured HSRP VIP, as it autogenerates link-local VIP from the HSRP reserved range with EUI and group However manual link-local can be configured aswell Also configure GUA and link-local address on the interface to not assume volatile and less secure EUI64                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| (config-if)#standby priority <>                                 | range of 0 – 255; default is 100                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| (config-if)#standby preempt                                     | HSRP Preempt is a mechanism that ensures that the previously active router with a highest priority will become the Active again when it comes back online (not enabled by default) can be fine tuned to ensure stability: #standby 2 preempt delay minimum 30 reload 30                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| (config-if)#standby track                                       | Object tracking FHRPs rely only on monitoring the status of devices and status of FHRP-enabled interfaces This approach has a blind spot: it cannot detect problems outside the local network, such as uplink failure or IP reachability to the external networks such as Internet To ensure that when the uplink of Active router fails or if it loses IP reachability to the WAN, it fails over the active role to the standby router, the [IP SLA with object tracking](onenote:#Probing%20Mechanisms\&section-id={6EC9C333-3E04-41C1-8C01-0E8D85C128A8}\&page-id={E4D987C5-DB44-4A7B-B03D-9C29A9CB7501}\&object-id={A5995C43-A2A5-0106-363D-26E52E3DF133}&39\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Notes/L3.one) must be implemented Interface tracking monitor the status of specific interface on the device Object tracking can monitor the state of an object like the IP SLA tracking IP reachability of specific IP address The state change of either of the two can trigger a specific action such as dynamically adjusting the FHRP router priority If a tracked object goes down, the priority decrements. If a tracked object comes back up, the priority increments - the default decrement/increment is 10 |
| (config-if)#standby authentication \<text \| md5> key-string <> | HSRP can be authenticated to prevent an unauthorized router from joining a group Authentication can be done in two ways: Plaintext or MD5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| (config-if)# standby timers                                     | on HSRP negotiation the active router will override the standby timers. Best practise                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| (config-if)# bfd interval <> min\_rx <> multiplier <>           | Enables BFD on the interface.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| (config-if)# standby bfd                                        | (Optional) Enables HSRPsupport for BFD on                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| show standby \[brief]                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |

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
