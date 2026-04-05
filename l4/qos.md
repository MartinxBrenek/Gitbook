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

# QoS

Before networks converged, network engineering was mainly focused on connectivity. However, the rates at which data came onto the network resulted in bursty data flows. Data, arriving in packets, tried to grab as much bandwidth as it could at any given time. Access was on a first-come, first-served basis. The data rate available to any one user varied, depending on the number of users accessing the network at any given time.

The protocols that have been developed have adapted to the bursty nature of data networks, and brief outages are survivable. For example, when you retrieve email, a delay of a few seconds is generally not noticeable. A delay of minutes is annoying, but not serious.

In converged network, voice, video, and data traffic use the same network facilities. Merging these different traffic streams with dramatically differing requirements can lead to a various problems and congestion, which occurs any time an interface is presented with more traffic than it is able to transmit.

Speed mismatch is a common cause of congestion in a network. Speed mismatches can occur when traffic moves from a high-speed LAN environment (1000 Mbps or higher) to lower-speed WAN links or in a LAN-to-LAN environment when, for example, a 10-Gbps link feeds into a 1-Gbps link.

Other typical places of congestion are aggregation points. In a LAN environment, congestion resulting from aggregation often occurs at the distribution layer of networks, where the different access layer devices feed traffic to the distribution-level devices.

![](<../.gitbook/assets/Unknown image (1208)>)

### Causes of network congestion

Lack of bandwidth in the internal network, causing overutilization. High CPU utilization caused by outdated hardware, Poor network design with causing bottlenecks or Too many devices in the broadcast domain

The maximum available bandwidth is equal to the narrowest (slowest link) point along the route

Multiple data streams compete for the same bandwidth, resulting in limited bandwidth for individual applications

![](<../.gitbook/assets/Unknown image (1209)>)

You can follow these approaches to prevent drops in sensitive applications:

Increase link capacity to ease or prevent congestion.

Guarantee enough bandwidth and increase buffer space to accommodate the bursts of fragile applications. There are several QoS mechanisms available in Cisco IOS and Cisco IOS XR Software that can guarantee bandwidth and provide prioritized forwarding to drop-sensitive applications. These mechanisms are listed:

Class-based weighted fair queuing (CBWFQ)

Low latency queuing (LLQ)

Prevent congestion by randomly dropping packets before congestion occurs. You can use WRED to selectively drop lower-priority traffic first, before congestion occurs.

### QoS fundamentals

**QoS** is a technology enabling to classify sensitive traffic in order to prioritize its transmission during the network congestion

QoS is performed in 3 processes. The router firstly classifies, whether the packet requires QoS to be implemented or not. Packets requiring QoS are marked by its defined priority in configuration. Packet marking is used in the second process called queueing, which sorts packets by its marked priority into queues that are buffered until the last process called shaping/policing allows them to be sent over the link. Shaping and policing enforce the bandwidth paid by customer

To provide functional QoS, it must be consistently configured on all network devices in the network

There are three basic steps that are involved in implementing QoS on a network.

Identify the traffic on the network and its requirements. Study the network to determine the type of traffic that is running on the network and then determine the QoS requirements for the different types of traffic.

Group the traffic into classes with similar QoS requirements. For example, three classes of traffic can be defined: voice and video (premium class), high priority (gold class) and best effort.

Define QoS policies that will meet the QoS requirements for each traffic class.

Voice traffic has extremely stringent QoS requirements. Voice traffic usually generates a smooth demand on bandwidth and has minimal impact on other traffic as long as the voice traffic is managed. While voice packets are typically small, they cannot tolerate delay or drops. The result of delays and drops are poor, and often unacceptable, voice quality. Because drops cannot be tolerated, UDP is used to package voice packets, because TCP retransmit capabilities have no value. Voice packets can tolerate no more than a 150-ms delay (one-way requirement) and require jitter of less than 30 msec and less than 1 percent packet loss. The network itself should be designed not to lose voice packets because lost voice packets result in reduced voice quality.

Video conferencing applications also have stringent QoS requirements very similar to voice requirements. But video conferencing traffic is often bursty and greedy in nature and, as a result, can impact other traffic. Therefore, it is important to understand the video conferencing requirements for a network and to provision carefully for it. The minimum bandwidth for a video conferencing stream would require the actual bandwidth of the stream (dependent upon the type of video conferencing codec being used) plus some overhead. For example, a 384-kbps video stream would actually require a total of 460 kbps of priority bandwidth.

High-definition video conferencing systems such as Cisco TelePresence combine rich audio, high-definition video, and collaboration technologies to provide real-time, face-to-face interactions between individuals in remote locations. These systems have higher service-level requirements than standard video conferencing systems and bandwidth requirements as high as 15 Mbps. Due to presence of multiple types of traffic flows, high-bandwidth requirements, and low tolerance for jitter, latency, and packet loss, careful network capacity and QoS planning are required when deploying Cisco TelePresence Systems.

Data traffic QoS requirements vary greatly. Different applications may make very different demands on the network (for example, a human resources application versus an automated teller machine application). Even different versions of the same application may have varying network traffic characteristics. In enterprise networks, important (business-critical) applications are usually easy to identify. Most applications can be identified based on TCP or UDP port numbers. Some applications use dynamic port numbers that, to some extent, make classifications more difficult.

Using the three previously defined traffic classes, you can determine QoS policies:

Voice and video (premium class): Minimum bandwidth: 160 kbps. Use QoS marking to mark voice packets as a high priority; use priority queue to minimize delay.

Business applications (gold class): Minimum bandwidth: 80 kbps. Use QoS marking to mark critical data packets as medium-high priority; use medium-priority queue.

Web traffic (best effort): Use QoS marking to mark these data packets as a low priority. Use queuing mechanism to prioritize best-effort traffic flows that are below the premium and gold classes.

Service quality offered by service provider can be measured with statistics such as throughput, usage, percentage of loss, and uptime.

Service providers formalize these expectations within SLAs, which clearly state the acceptable bounds of network performance.

Measures are performed with the [IP SLA feature](onenote:Assurance.one#IP%20SLA\&section-id={9F30FF1D-D9FA-4657-8062-B8D62DED78FA}\&page-id={E4D987C5-DB44-4A7B-B03D-9C29A9CB7501}\&end\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE)

There are several points in the network where SLA measurements can take place:

Provider edge-to-provider edge (PE-to-PE) measurement: The most common points for measurements of SLA parameters

Customer edge-to-customer edge (CE-to-CE) measurement: Measurements of SLA parameters from the customer site, available from the service provider when the CE router is managed by the service provider

Measurements of SLA parameters for end-to-end applications, available only from the enterprise. Some types of applications have the ability to measure SLA parameters such as delay, jitter, and packet loss. For example, an IP telephony phone call between Cisco IP phones can provide these statistics on the display of the IP phone.

![](<../.gitbook/assets/Unknown image (1210)>)

### QoS models

**Best effort**

QoS is not enabled for this model. It is used for traffic that does not require any special treatment

#### IntServ

Applications signal the network to make a bandwidth reservation and to indicate that they require special QoS treatment

**IntServ** is similar to a concept known as "hard QoS." With hard QoS, traffic characteristics such as bandwidth, delay, and packet-loss rates are guaranteed end to end.

For real-time applications such as voice and video that require bandwidth, delay, and packet-loss guarantees to ensure both predictable and guaranteed service levels

Uses Resource Reservation Protocol (RSVP) to reserve resources throughout a network for a specific application and to provide call admission control (CAC) to guarantee that no other IP traffic can use the reserved bandwidth. The bandwidth reserved by an application that is not being used is wasted. RSVP uses RSVP messages to build flow path states (RESV req)

Drawback: To provide end-to-end QoS, all nodes, including the endpoints running the applications, need to support, build, and maintain RSVP path state for every single flow; Not scalable

RSVP is an IETF standard (RFC 2205) signaling protocol for allowing an application to dynamically reserve network bandwidth. RSVP enables applications to request a specific QoS for a data flow (shown in the figure). The Cisco implementation also allows RSVP to be initiated within the network, using a configured proxy RSVP. Network managers can take advantage of RSVP benefits in the network, even for non-RSVP-enabled applications and hosts.

Hosts and routers use RSVP to deliver QoS requests to the routers along the paths of the data stream. Hosts and routers also use RSVP to maintain the router and host state to provide the requested service, usually bandwidth, and latency. RSVP uses LLQ or weighted random early detection (WRED) QoS mechanisms, setting up the packet classification and scheduling that is required for the reserved flows.

The figure outlines how RSVP data flows are allocated when RSVP is configured on an interface. The maximum bandwidth available on any interface is 75 percent of the line speed; the rest is used for control plane traffic. When RSVP is configured on an interface, the option is to use the entire usable bandwidth or a certain configured amount of bandwidth. The default is for RSVP data flows to use up to 75 percent of the available bandwidth.

![](<../.gitbook/assets/Unknown image (1211)>)

#### DiffServ

**DiffServ** is a new model that supersedes—and is backward-compatible with—IP precedence. DiffServ redefines the ToS byte as the DiffServ field and uses six prioritization bits that permit classification of up to 64 values (0 to 63), of which 32 are commonly used. A DiffServ value is called a DSCP.

It was designed to overcome the limitations of both the best-effort and IntServ models. DiffServ can provide an "almost guaranteed" QoS, while still being cost-effective and scalable.

The network identifies classes that require special QoS treatment. End-to-end QoS guarantees cannot be enforced as QoS characteristics (such as bandwidth and delay) are managed on a hop-by-hop basis (called Per-Hop Behavior PHB) with QoS policies defined independently at each device in the network

DiffServ is similar to a concept known as "soft QoS." With soft QoS, QoS mechanisms are used without prior signaling.

The soft QoS approach is not considered an end-to-end QoS strategy because end-to-end guarantees cannot be enforced but it is a more scalable approach, because many (hundreds or potentially thousands) of applications can be mapped into a small set of classes upon which similar sets of QoS behaviors are implemented.

Divides IP traffic into classes and marks it based on their resource requirements, so that each of the classes can be assigned a different level of service with specified treatment

As IP traffic traverses a network, each of the network devices identifies and services the packet by its own class parameters

Traffic classification is performed closest to the source of traffic; classified packets are marked using the differentiated services code point (DSCP) field in IP packets.

With DiffServ, packet classification is used to categorize network traffic into multiple priority levels or classes of service. Packet classification uses the DSCP traffic descriptor to categorize a packet within a specific group to define that packet. After the packet has been defined (classified), the packet is then accessible for QoS handling on the network.

### Classification

**Classification** is the process of identifying traffic and categorizing traffic into different classes. In a QoS-enabled network, all traffic is classified at the input interface of every QoS-aware device.

Classification should take place at the network edge, typically in the wiring closet, in IP phones, or at network endpoints. Classification should occur as close to the source of the traffic as possible.

Traffic can be classed by traffic descriptors

MQC allows classification to be implemented separately from policy.

Packet classification should take place at the network edge, as close to the source of the traffic as possible

**Trust Boundary** defines where the network considers packets to be trusted.

The concept of trust is very important for deploying QoS. When an end device (such as a workstation or an IP phone) marks a packet with class of service (CoS) or differentiated services code point (DSCP), a switch or router has the option of accepting or not accepting the QoS marking values from the end device. If the switch or router chooses to accept the QoS marking values, the switch or router trusts the end device. If the switch or router trusts the end device, it does not need to do any reclassification of packets coming from that interface. If the switch or router does not trust the interface, it must perform a reclassification to determine the appropriate QoS value for the packets that are coming in from that interface. Switches and routers are generally set to not trust end devices, and must be specifically configured to trust packets coming from an interface.

Many endpoints (including user PCs) technically support the ability to mark traffic on their network interface cards (NICs), allowing a blanket trust of such markings could easily facilitate network abuse, as users could simply mark all their traffic with Expedited Forwarding, that would allow them to hijack network priority services for their traffic that is not real time, and thus ruin the service quality of real-time applications throughout the enterprise.

Packets should be marked by the endpoint or as close to the endpoint as possible (for example at the access layer) to avoid that the end devices manipulate priority.

The service provider trust boundary separates the enterprise and service provider QoS domains, with traffic classes or rates being (un)trusted at the boundary, and the trust location varying between unmanaged and managed services. It is usually performed on the PE routers

Identification of a traffic flow can be performed by using several methods within a router, such as matching traffic using access control lists (ACLs), using protocol match, or matching the IP precedence, IP DSCP, Multiprotocol Label Switching experimental bit (MPLS EXP bit), or CoS.

### Marking

**Marking**, also known as coloring, marks each packet as a member of a network class so that the packet class can be quickly recognized throughout the rest of the network.

The marking is done using traffic descriptors

Marking is performed as close to the network edge as possible, and is typically done using MQC.

QoS mechanisms set bits in the IP, MPLS, or Ethernet header according to the class of the packet. Other QoS mechanisms use these bits to determine how to treat the packets when they arrive. If the packets are marked as high-priority voice packets, the packets will generally not be dropped by congestion avoidance mechanisms, and will be given immediate preference by congestion management queuing mechanisms. However, if the packets are marked as low-priority file transfer packets, they will have a higher drop probability when congestion occurs, and will generally be moved to the end of the congestion management queues.

![](<../.gitbook/assets/Unknown image (1212)>)

![](<../.gitbook/assets/Unknown image (1213)>)

### Traffic descriptors

Internal: QoS groups (locally significant to a router) are used to mark packets as they are received and processed internally within the router and are automatically removed when packets egress the router. They are used only in special cases in which traffic descriptors marked or received on an ingress interface would not be visible for packet classification on egress interfaces due to encapsulation or de-encapsulation

Layer 1: Physical interface, subinterface, or port

Layer 2: MAC address and 802.1Q/p Class of Service (CoS) bits

Layer 2.5: MPLS Experimental (EXP) bits

Layer 3: Differentiated Services Code Points (DSCP), IP Precedence (IPP), and source/destination IP address

Layer 4: TCP or UDP ports

Layer 7: Next Generation Network-Based Application Recognition (NBAR2)

NBAR2 is a deep packet inspection engine that can classify and identify a wide variety of protocols and applications using Layer 3 to Layer 7 data, including difficult-to-classify apps that dynamically assign TCP or UDP port numbers. Can recognize more than 1000 apps, and monthly protocol packs are provided for recognition of new and emerging applications, without requiring an IOS upgrade or router reload. Protocol Discovery enables NBAR2 to discover and get real-time statistics on applications currently running in the network. These statistics are used for MQC configuration.

For example network traffic matching a specific network protocol such as Cisco Webex can be placed into one traffic class, while traffic that matches a different network protocol such as YouTube can be placed into another traffic class

#### Layer 2 marking (CoS / 802.1p)

802.1p is a standard for adding QoS to Ethernet frames and it is part of the .1q frame header

Class of Service (CoS) marks a Layer 2 Ethernet 3bit field Priority Code Point (PCP) with 8 (0-7) available levels of priority values.

Three bits allow for eight levels of classification, allowing a direct correspondence with IPv4 (IP precedence) type of service (ToS) values

Drop Eligible Indicator In case of congestion, could be set to 1 to allow second end to discard this frame

CoS field is part of the 802.1q header and therefore is not preserved/lost at the first L3 gateway

One disadvantage of using CoS markings is that frames will lose their CoS markings when transiting a non-802.1Q or non-802.1p link, including any type of non-Ethernet WAN link. Therefore, a more permanent marking should be used for network transit, such as Layer 3 IP DSCP marking. This marking is typically accomplished by translating a CoS marking into another marker or by simply using a different marking mechanism.

If the service provider network is an MPLS network, the IP precedence bits are copied into the MPLS experimental field at the edge of the network. However, the service provider might want to set an MPLS packet QoS to a different value that is determined by the service offering.

The MPLS experimental field allows the service provider to provide QoS without overwriting the value in the customer IP precedence field.

![](<../.gitbook/assets/Unknown image (1214)>)

CoS 7 (111): network

CoS 6 (110): Internet

CoS 5 (101): critical

CoS 4 (100): flash override

CoS 3 (011): flash

CoS 2 (010): immediate

CoS 1 (001): priority

CoS 0 (000): routine

![IEEE 802.1ad Access Network - Switching - NetworkLessons.com Community Forum](<../.gitbook/assets/Unknown image (1215)>)

Note

TID is a term that is used to describe a 4-bit field in the QoS control field of wireless frames (802.11 MAC frame). TID is used for wireless connections, and CoS is used for wired Ethernet connections.

#### Layer 3 marking (DSCP / IPP)

Layer 3 marking uses the L3 ToS field (1 byte field). Layer 3 marking (IP Precedence/DSCP) is retained end-to-end if it’s not modified/removed in between. CEF must be enabled

IP Precedence (IPP) was former version used for QoS marking, which had only 6 (3bit - 0 - 7) usable classes (6 and 7 was reserved for network use)

Differentiated Services Code Point (DSCP) is the newer QoS version field that allows for classification of up to 64 (6-bit) values (0 to 63)

and a 2-bit Explicit Congestion Notification (ECN) field which indicate that congestion was experienced in transit

Per-Hop Behavior (PHB) is forwarding treatment by each network node to the destination based on the DSCP value

Behavior Aggregates (BAs) is a collection of packets with the same DiffServ value crossing a link in a particular direction

![](<../.gitbook/assets/Unknown image (1216)>)

![](<../.gitbook/assets/Unknown image (1217)>)

Class Selector (CS) make DSCP backward compatible with IP Precedence which uses the same 3 bits to determine class

Bits 2 to 4 are ignored by non-DiffServ-compliant devices. Ranging from CS0 to CS7

![](<../.gitbook/assets/Unknown image (1218)>)

Default Forwarding (DF) = Best effort (BE) Use the DS value 000000. Also applied to packets that cannot be classified.

![](<../.gitbook/assets/Unknown image (1219)>)

Scavenger Class = (Worst effort) less than best-effort marked as CS1

Assured Forwarding (AF) used for guaranteed bandwidth service. Defines four classes of queueing together with a drop rate probability

AFxy

x = queueing class (1-4, bits 1-3, the higher the better)

y = drop probability (1-3, bits 4-5, the lower the better)

value aaadd0: aaa is the binary value of the AF class (bits 5 to 7), and dd (bits 2 to 4) is the drop probability where bit 2 is unused (0)

AF class number does not represent precedence; Each class should be treated independently and placed into different queues

![](<../.gitbook/assets/Unknown image (1220)>)

![](<../.gitbook/assets/Unknown image (1221)>)

Expedited Forwarding (EF) ensures a minimum departure rate. The EF PHB provides the lowest possible delay for delay-sensitive applications (decimal 46, binary 101110)

![](<../.gitbook/assets/Unknown image (1222)>)

IP Precedence

000 (0) Routine or Best Effort

001 (1) Priority

010 (2) Immediate

011 (3) Flash - mainly used for Voice Signaling or for Video.

100 (4) Flash Override

101 (5) Critical -mainly used for Voice RTP.

110 (6) Internet

111 (7) Network

![QoS Marking with Scapy - PacketLife.net](<../.gitbook/assets/Unknown image (1223)>)

[https://www.tucny.com/Home/dscp-tos](https://www.tucny.com/Home/dscp-tos)

| Traffic type          | RFC 4594 DSCP | Cisco DSCP | PHB                  | Latency (ms) | Jitter (ms) | Packet loss | Elasticity | AQM/WRED | Admission Control | Notes                                                                            |
| --------------------- | ------------- | ---------- | -------------------- | ------------ | ----------- | ----------- | ---------- | -------- | ----------------- | -------------------------------------------------------------------------------- |
| Network Control       | CS6           | CS6        | BW reservation       | Don’t care   | Don’t care  | Minimal     | Inelastic  | No       | No                | Routing protocols and other traffic holding the network together                 |
| Signaling             | CS5           | CS3        | BW reservation       | Don’t care   | Don’t care  | Minimal     | Inelastic  | No       | No                | Interactive voice/video signaling (call setup, forwarding, etc)                  |
| OAM                   | CS2           | CS2        | BW reservation       | Don’t care   | Don’t care  | Minimal     | Inelastic  | No       | No                | Network operations: SNMP, SSH, RADIUS, TACACS, Netflow                           |
| Voice                 | EF            | EF         | Low-latency/priority | < 150        | < 30        | < 1%        | Inelastic  | No       | Yes               | Very sensitive to latency, jitter, and loss                                      |
| Broadcast Video       | CS3           | CS5        | LL/priority (may)    | Don’t care   | Don’t care  | < 0.1%      | Inelastic  | No       | Yes               | Typically has application level buffering. Includes live video feeds, IPTV, CCTV |
| Real-time interactive | CS4           | CS4        | LL/priority (may)    | < 200        | < 50        | < 0.1%      | Inelastic  | No       | Yes               | Telepresence; similar SLA as VOIP with a little bit more latency but less loss   |
| Multimedia Conf       | AF4x          | AF4x       | AF + BW reservation  | < 200        | Don’t care  | < 1%        | Elastic    | Yes      | Yes               | Bidirectional software media, like webex                                         |
| Multimedia Stream     | AF3x          | AF3x       | AF + BW reservation  | < 400        | Don’t care  | < 1%        | Elastic    | Yes      | May               | Video on demand, youtube/training videos, unidirectional video viewing           |
| Transaction Data (LL) | AF2x          | AF2x       | AF + BW reservation  | Don’t care   | Don’t care  | Don’t care  | Elastic    | Yes      | No                | Interactive foreground applications; users are expecting a response (ERP, CRM)   |
| Bulk Data (HT)        | AF1x          | AF1x       | AF + BW reservation  | Don’t care   | Don’t care  | Don’t care  | Elastic    | Yes      | No                | Non-interactive background applications; database sync, backup jobs              |
| Best Effort           | DF            | DF         | DF + BW res (may)    | Don’t care   | Don’t care  | Don’t care  | Elastic    | May      | No                | Most applications fit here, anything unclassified (also DNS, DHCP)               |
| Scavenger             | CS1           | CS1        | Minimal BW           | Don’t care   | Don’t care  | Don’t care  | Elastic    | No       | No                | Explicit low priority, video games, peer to peer traffic                         |

#### Mapping Layer 2 to Layer 3 markings

IP headers are preserved end-to-end when IP packets are transported across a network, but data link layer headers are not preserved. This preservation means that the IP layer is the most logical place to mark packets for end-to-end QoS.

Service providers offering IP Services have a requirement to provide robust QoS solutions to their customers. The ability to map network layer QoS to data link layer CoS allows these providers to offer a complete end-to-end QoS solution that does not depend on any specific data link layer technology.

Compatibility between an MPLS transport layer QoS and network layer QoS is also achieved by mapping between MPLS EXP bits and the IP precedence or DSCP bits. A service provider can map the customer network layer QoS marking as is, or change it to fit an agreed-upon service level agreement (SLA). The information in the MPLS EXP bits can be carried end-to-end in the MPLS network, independent of the transport media. In addition, the network layer marking can remain unchanged so that when the packet leaves the service provider MPLS network, the original QoS markings remain intact. Thus, a service provider with an MPLS network can help provide a true end-to-end QoS solution.

Example: Application Service Classes

By default, a Cisco IP phone sends 802.1p-tagged packets with the Class of Service (CoS) field set to 5.

![](<../.gitbook/assets/Unknown image (1224)>)

### QoS implementation

Several years ago, the only way to implement QoS in a network was by using the CLI to individually configure QoS policies at each interface. This was a time-consuming, tiresome, and error-prone task that involved cutting and pasting configurations from one interface to another.

Methods for implementing QoS:

CLI can be used to individually configure QoS policy on each interface.

MQC allows the creation of modular QoS policies and attachment of these policies on the interface.

Cisco Prime Carrier Management is a tool that allows a network administrator to create, control, and monitor QoS policies.

#### Modular QoS CLI (MQC)

**Modular QoS CLI (MQC)** is a CLI structure that allows you to create policies and attach these policies to interfaces

A traffic policy contains a traffic class and one or more QoS features. A traffic class is used to classify traffic, whereas the QoS features in the traffic policy determine how to treat the classified traffic. Separates classification engine from the policy

Upon packet classification, the packet's field is assigned a specific value (e.g., marked), indicating its priority status over other packets and influencing its treatment within the network

Modification of the class: QoS traffic classes can be modified in different ways. You can create new classes, edit an existing class by adding more traffic into it, add conditional matching statements on existing classes, or remove classes that are no longer being used by any policy.

Modification of policy: Policy can be modified in many ways. You can apply a different policy on a traffic class, apply a child policy, or simply add a new PHB to an existing traffic class. Change of policy is immediately reflected on traffic that is passing through the interface.

Modification of attachment point or direction: The same policy can be applied on multiple interfaces. You can disable or enable policy on an interface by entering one command.

![](<../.gitbook/assets/Unknown image (1225)>)

#### Configuration (IOS / IOS-XE)

MQC QoS commands on Cisco IOS and Cisco IOS XE Software are identical. Each QoS technique has slightly different capabilities between Cisco IOS and IOS XE and Cisco IOS XR Software. Cisco IOS XR Software supports a DiffServ architecture, a multiple-service model that can satisfy different QoS requirements.

Steps:

Define access list for classifying traffic

access-list 101 permit udp any any eq 5060

access-list 102 permit ip any 10.0.0.0 0.255.255.255

access-list 103 permit tcp any any eq 443

1. Configure classification by using the class-map command.

| Router(config)# class-map \[match-all \| match-any] \[NAME] class-map VOICE match access-group 101 class-map VIDEO match access-group 102 class-map DATA match access-group 103 | Class-Maps are used to specify the traffic which should later be marked. Class-Maps can be created with two options: match-all: all defined matches must match in order to get marked (= AND operator) (default behavior) match-any: one of the defined matches must match in order to get marked (= OR operator) Matching can be done against ACL CoS value Source/Destination Address DSCP value Input Interface IP Precedence value Protocol type Classification based on NBAR is using the #match protocol |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Router(config-cmap)# match \[option] \[arguments]                                                                                                                               | ## Matching traffic based on various options                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

2. Configure traffic policy by associating the traffic class with one or more QoS features using the policy-map command.

| policy-map QOS class VOICE set dscp ef priority percent 30 class VIDEO bandwidth percent 40 set dscp af41 random-detect dscp 32 64 96 8 class DATA bandwidth percent 20 set dscp af21 class class-default set ip dscp default fair-queue random-detect dscp-based | // bandwidth allocation of 30% using the priority command, ensuring that voice traffic is given the EF treatment // random-detect configure the WRED, correspond to the DSCP values AF11, AF21, AF31, and CS1 respectively, last defines the minimum and maximum thresholds for packet drop probability for each DSCP value // default class represents all other traffic |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

3. Attach the traffic policy to inbound or outbound traffic on interfaces, subinterfaces, or VCs by using the service-policy command.

| interface GigabitEthernet0/0 |
| ---------------------------- |
| service-policy input QOS     |

| show policy-map int out                        |   |
| ---------------------------------------------- | - |
| show policy-map int <> det                     |   |
| show queueing interface <>                     |   |
| show policy-map type lan-queuing int <> in/out |   |

#### Configuration (IOS XR)

In Cisco IOS XR Software, features are generally disabled by default and must be explicitly enabled. There are some differences in the default syntax values used in MQC. For example, if you create a traffic class with the class-map command in Cisco IOS and Cisco IOS XR Software, Cisco IOS Software creates by default a traffic class that must match all statements under the service class that is defined. Cisco IOS XR Software creates a traffic class that matches any of the statements under the service class. Another difference is the available set of capabilities in different types of software

| class-map class-map-name-1 match match-criteria-1 class-map class-map-name-n match match-criteria-n            | The class-map command defines a named object representing a class of traffic, specifying the packet matching criteria that identify packets that belong to this class.                                                                                                                                                                                                                                                                                                                                                                                       |
| -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| policy-map policy-map-name class class-map-name-1 policy-1 policy-n class class-map-name-n policy-m policy-m+1 | The policy-map command defines a named object that represents a set of policies to be applied to a set of traffic classes. An example of such a policy is policing the traffic class to some maximum rate                                                                                                                                                                                                                                                                                                                                                    |
| interface Gig0/0/0/0 service-policy \[input\|output] \[name]                                                   | attaches a policy map and its associated policies to a target, a named interface. Rules for applying QoS policies on interfaces: One policy per direction can be used on an interface. One policy can be applied on multiple interfaces You can attach a policy to an interface (physical or logical), to a PVC, or to special points to control route processor traffic. Examples of logical interfaces include these: Port channel (Ethernet channel of interfaces) POS channel (POS/SDH channel of interfaces) Multilink (Multilink PPP) Virtual template |

[Qos-group](onenote:Architecture.one#MPLS%20QoS%20|%20MVPN\&section-id={DB9CE639-F13F-4E99-AA12-1A96859BBE4A}\&page-id={B1667746-1C5E-461D-A9FE-719243C1FF7D}\&object-id={21BED673-2109-0CDB-207D-F7B06D06702F}\&CA\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Notes)

![](<../.gitbook/assets/Unknown image (1226)>)

### Hierarchical QoS (HQF / HQoS)

**Hierarchical QoS (HQF / HQoS)** is an extension to MQC that allows you to specify QoS behavior at multiple policy levels of hierarchy, which provides a high degree of granularity in traffic management.

H-QoSis applied on the router interface using nested traffic policies. The first level of traffic policy, the parent traffic policy, is used for controlling the traffic at the main interface or sub-interface level. The second level of traffic policy, the child traffic policy, is used for more control over a specific traffic stream or class. The child traffic policy, is a previously defined traffic policy, that is referenced within the parent traffic policy using the service-policy command.

Hierarchical policies can be used to perform these functions:

Allow a parent class to shape multiple queues in a child policy. Shape multiple queues to a single rate.

Apply specific policy map actions on the aggregate traffic.

Apply class-specific policy map actions.

Restrict the maximum bandwidth of a virtual circuit (VC), while allowing policing and marking of traffic classes within the VC.

It is used where there is a need to combine multiple QoS policies at different levels, e.g. for broadband networks (ISP), MPLS VPN, mobile networks.

Consists of two policy maps

Child policy map: Marks all the classified traffic, assigning bandwidth, …

Parent policy map: Assigns overall bandwidth, …, and links to the child policy map

| class-map HTTP match protocol http policy-map QueueAll class HTTP bandwidth 1000 class-map AllTraffic match any policy-map ShapeAll class AllTraffic shape 2000000 service-policy QueueAll interface FastEthernet/0 service-policy output ShapeAll | In the example configuration, a child policy map QueueAll is created, which guarantees bandwidth of 1 Mb/s to web traffic. The QueueAll policy map is then nested within a parent policy map named ShapeAll. Finally, the parent policy map ShapeAll is applied to the FastEthernet interface. Traffic out of the FastEthernet interface will be shaped to 2 Mb/s, of which 1 Mb/s of the 2 Mb/s of shaped traffic will be guaranteed for HTTP traffic. |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Policing and shaping

Upon packet marking, the policing and shaping is deployed to throttle the traffic rate to enforce the amout of traffic that can be forwarded

Policing drops excess traffic to control traffic flow within specified rate limits. Traffic policing does not introduce any delay to traffic that conforms to traffic policies, but it can cause TCP retransmissions since it is more aggressive than shaping

The traffic policing feature limits the input or output transmission rate of a class of traffic based on user-defined criteria, and can mark down packets by setting values such as IP precedence, QoS group, or DSCP value. Policing mechanisms can be set to drop traffic classes that have lower QoS priority markings first.

Policing mechanisms can be used at either input or output interfaces. These mechanisms are typically used to control the flow into a network device from a high-speed link by dropping excess low-priority packets. A good example would be the use of policing by a service provider to slow down a high-speed inflow from a customer that was more than the service agreement. In a TCP environment, this policing would cause the sender to slow its packet transmission.

Shaping buffers or delays egress traffic rates that goes beyond a desired traffic rate instead of dropping them.

Shaping adds variable delay to traffic, possibly causing jitter.

In many networks, especially those with different upstream and downstream bandwidths, traffic can build up at the slower link (usually on the remote or access side).

By shaping outbound traffic, the network sends data at a controlled rate, preventing sudden bursts from overwhelming the slower link.

You can deploy traffic shaping at the central-site router to shape the traffic rate out of the central-site router to match the link speed of the remote site. For example, the central router can shape the outgoing traffic rate going to a specific remote site to match that remote-site ingress SLA

Shaping implies the existence of a queue and of sufficient memory to buffer delayed packets on the routers, while policing does not

Ensure that you have sufficient memory when you enable shaping. In addition, shaping requires a scheduling function for later transmission of any delayed packets

![](<../.gitbook/assets/Unknown image (1227)>)

Traffic shaping and policing are deployed in the access and IP edge layers. Traffic rate control implementations in the aggregation layer are rare, and exist mainly in situations where the aggregation layer is collapsed with the access or IP edge. Traffic policing and shaping are never used in the core because the main purpose of the core is high-speed forwarding through a highly available core infrastructure.

You can apply policing to either the inbound or outbound direction while shaping is typically applied in the outbound direction.

Sub-rate Ethernet Link The physical interface of the CE/PE router has a bandwidth of 1G. The provisioned circuit between the PE and CE is 250Mbps. On the PE side this is most commonly implemented via policing which drops all packets exceeding the CIR. On the CE side this should be implemented via shaping or else (massive) packet drops could occur when the traffic exceeds the CIR on the PE ingress interface.

#### Token bucket policing

The token bucket is a mathematical model that is used by routers and switches to regulate traffic flow.

Committed Information Rate (CIR): The policed traffic rate, in bits per second (bps)

Committed Access Rate (CAR) is a Cisco-specific implementation of traffic policing and shaping

![](<../.gitbook/assets/Unknown image (1228)>)

Committed Time Interval (Tc): The time interval, in milliseconds (ms), over which the committed burst (Bc) is sent. Tcs: How many Tc intervals fit into 1 second.

Committed Burst Size (Bc): The maximum size of the CIR token bucket, measured in bytes, and the maximum amount of traffic that can be sent within a Tc. Bc = CIR × (Tc / 1000)

Token: A single token represents 1 byte or 8 bits

Token bucket: A bucket that accumulates tokens until a maximum predefined number of tokens is reached (such as the Bc when using a single token bucket); these tokens are added into the bucket at a fixed rate (the CIR) Each packet is checked for conformance to the defined rate and takes tokens from the bucket equal to its packet size

For example, if the packet size is 1500 bytes, it takes 12,000 bits (1500 × 8) from the bucket. If there are not enough tokens in the token bucket to send the packet, then it buffer, drops or mark down packets

![](<../.gitbook/assets/Unknown image (1229)>)

![](<../.gitbook/assets/Unknown image (1230)>)

Lets assume that a 1 Gbps interface is configured with a policer defined with a CIR of 120 Mbps and a Bc of 12 Mb

The Tc value cannot be explicitly defined in IOS, but it can be calculated as follows:

Tc = (Bc \[bits] / CIR \[bps]) × 1000

Tc = (12 Mb / 120 Mbps) × 1000

Tc = (12,000,000 bits / 120,000,000 bps) × 1000 = 100 ms

Once the Tc value is known, the number of Tcs within a second can be calculated as follows:

Tcs per second = 1000 / Tc

Tcs per second = 1000 ms / 100 ms = 10 Tcs

If a continuous stream of 1500-byte (12,000-bit) packets is processed by the token algorithm, only a Bc of 12 Mb can be taken by the packets within each Tc (100 ms). The number of packets that conform to the traffic rate and are allowed to be transmitted can be calculated:

Number of packets that conform within each Tc = Bc / packet size in bits (rounded down)

Number of packets that conform within each Tc = 12,000,000 bits / 12,000 bits = 1000 packets

Any additional packets beyond 1000 will either be dropped or marked down.

To figure out how many packets would be sent in one second, the following formula can be used:

Packets per second = Number of packets that conform within each Tc × Tcs per second

Packets per second = 1000 packets × 10 intervals = 10,000 packets

To calculate the CIR for the 10,000, the following formula can be used:

CIR = Packets per second × Packet size in bits

CIR = 10,000 packets per second × 12,000 bits = 120,000,000 bps = 120 Mbps

To calculate the time interval it would take for the 1000 packets to be sent at interface line rate:

Time interval at line rate = (Bc \[bits] / Interface speed \[bps]) × 1000

Time interval at line rate = (12 Mb / 1 Gbps) × 1000

Time interval at line rate = (12,000,000 bits / 1000,000,000 bps) × 1000 = 12 ms

Note The recommended values for Tc range from 8 ms to 125 ms. Shorter Tcs, such as 8 ms to10 ms, are necessary to reduce interpacket delay for real-time traffic such as voice. Tcs longer than 125 ms are not recommended for most networks because the interpacket delay becomes too large

![](<../.gitbook/assets/Unknown image (1231)>)

#### Policer types

**Single-rate two-color**

either conforming to or exceeding the CIR

**Single-rate three-color (srTCM)**

two token buckets

If there are any tokens left over in the bucket after each time period due to low or no activity, instead of discarding the excess tokens, the algorithm places them in a second bucket called excess burst (Be) to be used later for temporary bursts that might exceed the CIR. Be is the maximum number of bits that can exceed the Bc

Parameters to meter the traffic streams including CIR and Bc

Excess Burst Size (Be) The maximum size of the excess token bucket, measured in bytes

Bc Bucket Token Count (Tc) The number of tokens in the Bc bucket (Not to be confused with the committed time interval Tc)

Be Bucket Token Count (Te) The number of tokens in the Be bucket

Incoming Packet Length (B) The packet length of the incoming packet, in bits

Conform Traffic under Bc is classified as conforming and green and transmitted

Exceed Traffic over Bc, but under Be is classified as exceeding and yellow. Exceeding traffic can be dropped or marked down and transmitted

Violate Traffic over Be is classified as violating and red. This type of traffic is usually dropped but can be optionally marked down and transmitted

Using a dual token bucket model allows traffic exceeding the normal burst rate (CIR) to be metered as exceeding, and traffic that exceeds the excess burst rate to be metered as violating traffic. Different actions can then be applied to the conforming, exceeding, and violating traffic.

![](<../.gitbook/assets/Unknown image (1232)>)

**Dual-rate three-color (trTCM)**

The two-rate three-color marker/policer is based on RFC 2698 and is similar to the single-rate three-color policer. The difference is that single-rate three-color policers rely on excess tokens from the Bc bucket, which introduces a certain level of variability and unpredictability in traffic flows; the two-rate three-color marker/policers address this issue

by using two distinct rates, the CIR and the Peak Information Rate (PIR). The two-rate three-color marker/policer allows for a sustained excess rate based on the PIR that allows for different actions for the traffic exceeding the different burst values; for example, violating traffic can be dropped at a defined rate, and this is something that is not possible with the single-rate three-color policer

Committed Information Rate (CIR): The policed traffic rate, in bits per second (bps)

Peak Information Rate (PIR) The maximum rate of traffic allowed. PIR should be equal to or greater than the CIR

Committed Burst Size (Bc) The maximum size of the second token bucket, measured in bytes

Peak Burst Size (Be) The maximum size of the PIR token bucket, measured in bytes

Bc Bucket Token Count (Tc) The number of tokens in the Bc bucket (Not to be confused with the committed time interval Tc)

Bp Bucket Token Count (Tp) The number of tokens in the Bp bucket

Incoming Packet Length (B) The packet length of the incoming packet, in bits

The two-rate three-color policer also uses two token buckets, but the logic varies from that of the single-rate three-color policer. Instead of transferring unused tokens from the

Bc bucket to the Be bucket, this policer has two separate buckets that are filled with two separate token rates. The Be bucket is filled with the PIR tokens, and the Bc bucket is filled

with the CIR tokens. In this model, the Be represents the peak limit of traffic that can be sent during a subsecond interval.

The logic varies further in that the initial check is to see whether the traffic is within the PIR. Only then is the traffic compared against the CIR. In other words, a violate condition is checked first, then an exceed condition, and finally a conform condition, which is the reverse of the logic of the single-rate three-color policer

Example

Tokens represent available bandwidth.

If there are enough tokens in the CIR bucket, the packet is sent, and tokens are deducted from both CIR and PIR buckets.

If CIR tokens are insufficient but PIR tokens are available, the packet is sent but marked as lower priority.

If no tokens are available in either bucket, the packet is dropped.

Dual-rate metering is often configured on interfaces at the edge of a network to police the rate of traffic entering or leaving the network. In the most common configurations, traffic that conforms is sent and traffic that exceeds is sent with a decreased priority, and traffic that violates is dropped. Users can change these configuration options to suit their network needs.

In addition to rate limiting, traffic policing using dual-rate metering allows marking of traffic according to whether the packet conforms, exceeds, or violates a specified rate.

![](<../.gitbook/assets/Unknown image (1233)>)

![](<../.gitbook/assets/Unknown image (1234)>)

| ## Configuring a single-rate, two/three color policer Router(config-pmap-c)# police cir bc be | ## Configuring a two-rate policer Router(config-pmap-c)# police cir bc pir be |
| --------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |

#### Token bucket shaping

Class-based traffic shaping uses the basic token bucket mechanism, in which Bc of tokens are added at every Tc time interval. The maximum size of the token bucket is Bc + Be. You can think of the traffic shaper operation like opening and closing of a transmit gate at every Tc interval. If the shaper gate is opened, the shaper checks to see if there are enough tokens in the token bucket to send the packet. If there are enough tokens, the packet is immediately forwarded. If there are not enough tokens, the packet is queued in the shaping queue until the next Tc interval. If the gate is closed, the packet is queued behind other packets in the shaping queue.

For example, on a 1 Gbps link, if the CIR is 100 Mbps, the Bc is 12.5 MB, the Be is 0, and the Tc = 0.125 seconds, then during each Tc (125 ms) interval, the traffic shaper gate opens and up to 12.5 MB can be sent. To send 12.5 Mb over a 1 Gbps line will only take 12.5 ms. Therefore the router will, on average, be sending at 10% of the transmission capacity (1 Gbps \* 10% = 100 Mbps, or 125 ms \* 10% = 12.5m).

Traffic shaping also includes the ability to send more than Bc of traffic in some time intervals after a period of inactivity. This extra number of bits in excess to the Bc is called Be.

![](<../.gitbook/assets/Unknown image (1235)>)

**Average shaper**

Shaping to the configured average rate: Shaping to the average rate forwards up to Bc of traffic at every Tc time interval, with additional bursting capability when enough tokens are accumulated in the bucket. Bc of tokens are added to the token bucket at every Tc time interval. After the token bucket is emptied, additional bursting cannot occur until tokens are allowed to accumulate, which can occur only during periods of silence or when the transmit rate is lower than the average rate. After a period of low traffic activity, up to Bc + Be of traffic can be sent.

Simplified:

Limits traffic to the configured average rate (CIR).

Allows bursts only when traffic was previously low, meaning tokens have accumulated.

Useful for steady and predictable traffic shaping.

**Peak shaper**

Shaping to the peak rate: Shaping to the peak rate forwards up to Bc + Be of traffic at every Tc time interval. Bc + Be of tokens are added to the token bucket at every Tc time interval. Shaping to the peak rate sends traffic at the peak rate, which is defined as the average rate multiplied by (1 + Be / Bc). Sending packets at the peak rate may result in dropping in the WAN cloud during network congestion. Shaping to the peak rate is recommended only when the network has additional available bandwidth beyond the CIR and applications can tolerate occasional packet drops.

Simplified: Peak Shaper allows consistent bursts up to PIR = CIR + excess burst (Be), meaning the customer can send traffic above CIR regularly.

If the provider's network is congested, the excess traffic may be dropped

| Router(config-pmap-c)#shape {average \| peak} average-bit-rate \[Bc] \[Be] | When using the shape peak command the token bucket has the size of BC + BE and will be fully refilled each interval |
| -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |

### Congestion management (queuing)

**Congestion management (queuing)** is a QoS rechnique that involves a combination of Packet Queuing and Packet scheduling on an an interface to manage network congestion

Congestion management mechanisms (queuing algorithms) use the marking on each packet to determine in which queue to place the packets. Different queues are given different treatment by the queuing algorithm, based on the class of packets in the queue. Generally, queues with high-priority packets receive preferential treatment.

Queuing (buffering) is the temporary storage of excess packets in router's buffer. Activated when output interface is experiencing congestion and deactivated when congestion clears

Congestion is detected by the queuing algorithm when a Layer 1 hardware queue present on physical interfaces, known as the transmit ring (Tx-ring or TxQ), is full or if the input interface is faster than output interface or if output interface is receiving packets from multiple input interfaces

Congestion management uses sophisticated queuing technologies, such as WFQ and LLQ, to ensure that time-sensitive packets such as voice are transmitted first.

Congestion management is implemented on all output interfaces in a QoS-enabled network by using queuing mechanisms to manage the outflow of traffic. Each queuing algorithm was designed to solve a specific network traffic problem, and each has a particular effect on network performance.

![](<../.gitbook/assets/Unknown image (1236)>)

| ## Configuring flow-based WFQ Router(config-pmap-c)# fair-queue ## Configuring class-based WFQ Router(config-pmap-c)# bandwidth \[arguments] | ## Configuring LLQ Router(config-pmap-c)# priority \[arguments] ## Enabling WRED Router(config-pmap-c)# random-detect \[arguments] |
| -------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |

#### Queueing / scheduling algorithms

**Active Queue Management (AQM)** refers to the family of algorithms that provide queue management:

**First-in, first-out queuing (FIFO)**

is a simple algorithm that forwards traffic in the order it was received - single queue and same class

**Round robin**

queues are serviced in sequence one after the other and each queue processes one packet only, so it does not include a mechanism to prioritize traffic

**Weighted round robin (WRR)**

allows a weight to be assigned to each queue, and based on that weight, each queue effectively receives a portion of the interface bandwidth

The limit or weight of the queue is configured in bytes. The accuracy of WRR queuing depends on the weight (byte count) and the MTU. If the ratio between the byte count and the MTU is too small, WRR queuing will not allocate bandwidth accurately. If the ratio between the byte count and the MTU is too large, WRR queuing will cause long delays.

**DRR Queuing**

is an implementation of the WRR algorithm developed to resolve the inaccurate bandwidth allocation problem with WRR.

DRR uses a deficit counter to track the number of “extra” bytes dispatched over the number of bytes that was to be configured to be dispatched each round. During the next round, the number of “extra” bytes—the deficit—is effectively subtracted from the configurable number of bytes that are dispatched.

Example:

Threshold of 3000

Packet sizes of 1500, 1499, and 1500

Total sent in round = 4499 B

Deficit = (4499 – 3000) = 1499 B

On the next round, send only the (threshold – deficit) = (3000 – 1499) = 1501 B

**Modified Deficit Round-Robin Queuing (MDRR)**

is a class-based composite scheduling mechanism that allows for queuing of up to eight traffic classes.

Differs from DRR by adding a special low-latency queue that can be serviced in one of two modes:

Strict priority mode: Low-latency queue is serviced whenever it is not empty, which may starve other queues

Alternate mode: MDRR alternately services the low-latency and other queues. Example: with three queues, the low-latency queue is serviced twice as often as the other queues, and it sends twice its weight per cycle.

Each queue within MDRR is defined by the following:

Quantum value: Average number of bytes served in each round.

Deficit counter: Number of bytes a queue has transmitted in each round.

Packets in a queue are served as long as the deficit counter is greater than zero. Each packet served decreases the deficit counter by a value equal to its length in bytes. A queue can no longer be served after the deficit counter becomes zero or negative. In each new round, the deficit counter for each nonempty queue is incremented by its quantum value. In general, the quantum size for a queue should not be smaller than the MTU of the interface to ensure that the scheduler always serves at least one packet from each nonempty queue.

Each MDRR queue can be given a relative weight, with one of the queues in the group defined as a priority queue. The weights assign relative bandwidth for each queue when the interface is congested. The MDRR algorithm dequeues data from each queue in a round-robin fashion if there is data in the queue to be sent. During each cycle, a queue can dequeue a quantum based on its configured weight.

**Custom queuing (CQ)** cisco implementation of WRR. Other queues are allowed to use unused BW of other queues. Set of 16 queues with round robin scheduler and FIFO queueing.

**Priority Queuing (PQ)** is a simple mechanism - set of 4 queues (High,medium,normal,low).

Packets are dispatched from a lower queue only when all higher-priority queues are empty. If a packet arrives for a higher queue, the packet from the higher queue is dispatched before any packets in lower-level queues. The problem with PQ is that packets in queues with a lower priority may never be dispatched if a steady stream of packets continues to arrive for a queue with a higher priority. These traffic flows will be "starved" for network bandwidth.

**Weighted fair queuing (WFQ)** automatically divides the interface bandwidth by the number of flows (weighted by IP Precedence) to allocate bandwidth fairly among all flows

**Class-Based Weighted Fair Queuing (CBWFQ)**

With CBWFQ, you define the traffic classes based on match criteria, including protocols, ACLs, and input interfaces. Packets satisfying the match criteria for a class constitute the traffic for that class. A queue is reserved for each class, and traffic belonging to a class is directed to that class queue.

Each queue is serviced based on the bandwidth assigned to configured class (up to 256 classes).

The queue limit for that class is the maximum number of packets allowed to be buffered in the class queue. Excess packets are dropped. Suitable for non-real-time data traffic

Each queue has a queue size:

Maximum number of packets that it can hold.

Maximum queue size is platform dependent.

Cisco IOS XR platforms use dynamic thresholds.

Tail drop is the default dropping scheme of CBWFQ. You can use WRED in combination with CBWFQ to prevent congestion of a class

Weights can be defined by specifying:

Bandwidth (in kbps, Mbps, Gbps)

Percentage of bandwidth (percentage of available interface bandwidth).

Percentage of remaining available bandwidth.

One service policy cannot have mixed types of weights.

Note

On egress, the actual bandwidth of the interface is determined to be the Layer 2 capacity excluding CRC

Theoretical capacity: 1 Gbps (1,000,000,000 bits per second)

However, due to Layer 2 overhead (headers, IFG, preamble, etc.), the actual usable bandwidth is slightly lower.

If mostly small packets (e.g., 64-byte frames) are sent, the effective throughput is even lower because overhead makes up a larger percentage of total bandwidth.

You can configure bandwidth guarantees by using one of these commands:

Note A single service policy cannot mix the fixed bandwidth (in bits per second), bandwidth percent, and bandwidth remaining commands in the same level.

| #bandwidth                   | command allocates a fixed amount of bandwidth by specifying the amount in kilobits, megabits, or gigabits per second.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| #bandwidth percent           | command to allocate a percentage of the default or available bandwidth of an interface. The default bandwidth usually equals the maximum speed of an interface. The default value can be replaced by using the bandwidth interface command. It is recommended that the bandwidth reflect the real speed of the link. The value configured with the bandwidth percent command is the minimum guaranteed bandwidth allocated to the traffic class                                                                                                                                                                                     |
| #bandwidth remaining percent | command to define how any unallocated bandwidth should be apportioned. It is typically used in conjunction with the bandwidth configuration at the parent level in hierarchical policy maps. In such a combination, if the minimum bandwidth guarantees are met, the remaining bandwidth is shared in the ratio defined by the bandwidth remaining command in the class configuration in the policy map. The available bandwidth is equally distributed among those queuing classes that do not have the remaining bandwidth explicitly configured. The bandwidth remaining command does not offer any reserved bandwidth capacity. |

**Low-latency queuing (LLQ)** is CBWFQ combined with PQ developed to guarantee the requirements of real-time traffic, such as voice. Real-time traffic is serviced by PQ and other queues as

CBWFQ. LLQ classes are policed to prevent the PQ from starving the CBWFQ non-priority classes. If a traffic class is not using the bandwidth assigned to it, it is shared among the other classes.

Classes to which the priority command is applied are considered priority classes. Within a policy map, you can give one or more classes priority status. When multiple classes within a single policy map are configured as priority classes, all traffic from these classes is enqueued to a single strict-priority queue

If LLQ is used within the CBWFQ system, it creates an extra priority queue in the CBWFQ system, which is serviced by a strict-priority scheduler

Policing of priority queues also prevents the priority scheduler from monopolizing the CBWFQ scheduler and starving nonpriority classes, as legacy PQ does

Cisco recommendation: Links with mixed traffic (voice, video, data) should never exceed 33% of the total interface bandwidth for all LLQ combined

![](<../.gitbook/assets/Unknown image (1237)>)

### Congestion avoidance (RED / WRED / ECN)

Congestion-avoidance techniques monitor network traffic flows to anticipate and avoid congestion at common network and internetwork bottlenecks, before problems occur by randomly dropping packets from selected queues when previously defined limits are reached.

Congestion avoidance mechanisms are typically implemented on output interfaces wherever a high-speed link (or set of links) feeds into a lower-speed link. These techniques are designed to provide preferential treatment for traffic (such as a video stream) that has been classified as real-time critical under congestion situations, while concurrently maximizing network throughput and capacity utilization and minimizing packet loss and delay. Cisco IOS XR Software supports the random early detection (RED), weighted random early detection (WRED), and tail-drop QoS congestion-avoidance features.

Another TCP-related phenomenon that reduces optimal throughput of network applications is TCP starvation. When multiple flows are established over a router, some of these flows may be much more aggressive than other flows. For instance, when a file transfer application TCP transmit window increases, the TCP session can send several large packets to its destination. The packets immediately fill the queue on the router, and other, less aggressive flows can be starved because there is no differentiated treatment indicating which packets should be dropped. As a result, these less aggressive flows are tail-dropped at the output interface.

**Tail Drop**

is the default congestion avoidance behavior. Does not distinguish between service types. When the output queue is full, packets are dropped until the traffic congestion is cleared

Tail drop drawbacks:

When congestion occurs, dropping affects most of the TCP sessions, which simultaneously back off and then restart again. This causes inefficient link utilization at the congestion point ([TCP global synchronization](https://onenote/#Transport%20Layer\&section-id={E7D8C7C5-105B-4198-A054-E267184A51AE}\&page-id={664269A1-2936-417B-91FA-B3708098457E}\&object-id={B5A6C804-7921-070E-3C60-AA96A7139ACB}&4A\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE/L4%20%5eM%20IP%20Services.one)).

TCP starvation, in which all buffers are temporarily seized by aggressive flows, and normal TCP flows experience buffer starvation.

There is no differentiated drop mechanism, and therefore premium traffic is dropped in the same way as best-effort traffic.

Based on the knowledge of TCP behavior during periods of congestion, you can conclude that tail drop is not the optimal mechanism for congestion avoidance and therefore should not be used. Instead, more intelligent congestion avoidance mechanisms should be used that slow down traffic before actual congestion occurs.

**Random Early Detection (RED)**

is a dropping mechanism that monitors the buffer and randomly drops packets before a queue is full. The dropping strategy is based primarily on the average queue length—that is, when the average size of the queue increases, RED will be more likely to drop an incoming packet than when the average queue length is shorter.

RED randomly drops packets without per-flow intelligence, but aggressive flows are more likely to lose packets, slowing them down and preventing congestion. This reduces TCP global synchronization, keeps queue sizes lower, and ensures smoother bandwidth utilization by spreading packet losses over time.

Thus Tail drop can be avoided if congestion is prevented = TCP sessions are desynchronized by random drops.

**Weighted Random Early Detection (WRED)**

performs differentiated packet dropping based on packet markers, such as DSCP.

As with RED, WRED monitors the average queue length in the router and determines when to begin discarding packets based on the length of the interface queue

When the average queue length is greater than the user-specified minimum threshold, WRED begins to randomly drop packets (both TCP and UDP packets) with a certain probability. If the average length of the queue becomes larger than the maximum threshold, WRED reverts to a tail-drop packet discard strategy.

WRED can selectively discard lower-priority traffic when the interface becomes congested, and can provide differentiated performance characteristics for different classes of service.

WRED is only useful when the bulk of the traffic is TCP traffic. With TCP, dropped packets indicate congestion, so the packet source reduces its transmission rate. With other protocols, packet sources might not respond or might resend dropped packets at the same rate, and so dropping packets might not decrease congestion.

WRED is more often used in the core than in the edge and the access network. Access and edge routers mark packets. WRED uses these assigned values to determine how to treat different types of traffic. WRED is not recommended for voice and video.

WRED can use multiple different RED profiles.

Drops less important packets more aggressively than important packets.

Packets with a higher weight (= higher IP Precedence or DSCP value) are less likely to be dropped

As the queue length grows longer, it becomes more aggressive, dropping even more random packets

Each profile is identified by:

Minimum threshold

Maximum threshold

Maximum drop probability (Cisco IOS and IOS XE Software only)

Low-priority traffic is dropped first (green line).

Medium-priority traffic is dropped later (red line).

High-priority traffic is dropped last (blue line).

![](<../.gitbook/assets/Unknown image (1365)>)

When a packet arrives at the output queue, a packet marker is used to select the correct WRED profile for the packet. The packet is then passed to WRED for processing. Based on the selected traffic profile and the average queue length, WRED calculates the probability for dropping the current packet and either drops the packet or passes it to the queue.

If the queue is already full, the packet is tail-dropped. Otherwise, the packet will eventually be transmitted. If the average queue length is greater than the minimum threshold but less than the maximum threshold, based on the drop probability, WRED will either queue the packet or perform a random drop.

![](<../.gitbook/assets/Unknown image (1238)>)

**ECN** is an extension to WRED that allows for signaling to be sent to ECN-enabled endpoints, instructing them to reduce their packet transmission rates

![](<../.gitbook/assets/Unknown image (1239)>)

### QoS in service provider environments

Service providers must provide QoS provisioning within their MPLS networks.

Different actions are based on the type of device—PE or P router.

All classification, marking, shaping, and policing should be done at the PE router.

Input policy (traffic classification, marking, and policing) are typically done at the PE router.

Output policy includes queuing, dropping, and shaping.

In the core the traditional Best-effort overprovisioning method is used - simply adding more bandwidth (overprovisioning) instead of actively managing traffic through Quality of Service (QoS) mechanisms. QoS policies on P routers are optional. Such policies are optional because some service providers overprovision their MPLS core networks, and therefore do not require any additional QoS policies within their backbones; however, other providers might implement simplified DiffServ policies within their cores, or might even deploy Cisco MPLS TE to manage congestion scenarios within their backbones. However P routers can employ packet queuing and dropping on their output interface if needed.

Congestion can occur in any layer, where there are points of speed mismatches - for example, a Gigabit Ethernet link feeding a Fast Ethernet link, aggregation - for example, multiple Gigabit Ethernet links feeding an upstream Gigabit Ethernet, or confluence - the flowing together of two or more traffic streams

Requirements for SP networks:

Support enterprises with diverse QoS policies. The QoS requirements on the CE and PE routers will differ, depending on whether the CE is managed by the service provider. For a managed CE, the service provider may also implement a QoS policy on the CE, or even push all of the ingress marking/policing to the Ethernet handoff, which is the new trust boundary.

Ensure contracted rates per SLAs.

Ensure loss, latency, and jitter, per class and per SLA.

Maintain QoS transparency for customers.

Measure and report SLA metrics.

Plan capacity that is based on SLA metric measurements.

### IPv6 QoS

Congestion management for IPv6 is similar to IPv4, and the commands used to configure queueing and traffic shaping features for IPv6 environments are the same commands as those used for IPv4. IPv6 also provides support for QoS marking via a field in the IPv6 header. Similar to the ToS (or differentiated services) field in the IPv4 header, the Traffic Class field (8 bits) is available for use by originating nodes and forwarding routers to identify and distinguish between different classes or priorities of IPv6 packets. The Traffic Class field can be used to set specific precedence or DSCP values, which are used the same way that they are used in IPv4.

IPv6 also has a 20-bit field that is known as the Flow Label field. The flow label enables per-flow processing for differentiation at the IP layer. It can be used for special sender requests and is set by the source node. The flow label must not be modified by an intermediate node. The main benefit of the flow label is that transit routers do not have to open the inner packet to identify the flow, which aids with identification of the flow when using encryption and in other scenarios

All of the QoS features available for IPv6 environments are managed from the modular QoS command-line interface (MQC). The MQC allows you to define traffic classes, create and configure traffic policies (policy maps), and then attach those traffic policies to interfaces.

To implement QoS in networks running IPv6, follow the same steps that you would follow to implement QoS in networks running only IPv4. At a very high level, the basic steps for implementing QoS are as follows:

Know which applications in your network need QoS.

Understand the characteristics of the applications so that you can make decisions about which QoS features would be appropriate.

Know your network topology, so that you know how link layer header sizes are affected by changes and forwarding.

Create classes based on the criteria you establish for your network. In particular, if the same network is also carrying IPv4 traffic along with IPv6, decide if you want to treat both of them the same way or treat them separately and specify match criteria accordingly. If you want to treat them the same, use match statements such as match precedence , match dscp . If you want to treat them separately, add match criteria such as match protocol ip and match protocol ipv6 in a match-all class map.

Create a policy to mark each class.

Work from the edge toward the core in applying QoS features.

Build the policy to treat the traffic.

Apply the policy.
