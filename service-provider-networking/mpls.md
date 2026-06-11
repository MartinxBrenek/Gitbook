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

# MPLS

### Overview

**Multiprotocol Label Switching (MPLS)** is an IETF standard forwarding method.

It encodes an extra header called a _label_.

The label identifies membership in a **Forwarding Equivalence Class (FEC)**. This enables forwarding based on label lookups, not IP lookups.

The label is a locally significant identifier that indexes a forwarding entry corresponding to a specific forwarding equivalence class, often associated with a prefix.

MPLS is used in service provider networks on top of their own IP infrastructure.

From the customer point of view, MPLS is transport infrastructure.

Key advantage: one backbone can serve multiple customers and services.

When a labeled packet arrives, the router uses the incoming label as a lookup key in the LFIB. The LFIB returns the forwarding action (swap/pop/push + outgoing interface).

This label is imposed between layer 2 and layer 3 header, which allows the MPLS to transport of traffic for multiple logically separated networks - VPNs, over a shared infrastructure while reducing forwarding complexity and improving network performance.

MPLS relies on signaling protocols such as the Label Distribution Protocol (LDP) to dynamically allocate and exchange labels between routers

When a packet enters an MPLS domain, it is assigned a label that identifies the path to its destination - the packet is SWITCHED - not routed. Core routers (Label Switch Routers, LSRs) then switch the packet based solely on this label, without needing to examine the full packet header (unlike IPv4-based routing, where each router examines multiple headers and fields (such as source IP, destination IP, protocol, and options) to forward packets). This reduces forwarding overhead and allows core routers to maintain only neighbor LSRs and internal IGP prefixes, while Provider Edge (PE) routers retain knowledge of customer prefixes.

### Forwarding Equivalence Class (FEC)

**Forwarding Equivalence Class (FEC)** is a concept introduced by MPLS to describe the meaning to which an MPLS label is bound in the control plane. MPLS is a multiprotocol transport technology, so an FEC does not describe packets themselves, but rather an abstract forwarding target or service that a label represents. Most commonly this identifier is an IP prefix, for example 10.10.10.0/24, but it can also represent a Layer 2 circuit, a VPN group or site, a Virtual Switch Instance, or a Traffic Engineering tunnel.

The FEC type code identifies what the label is signaling.

#### Common MPLS FEC types

* **Type 1 (Address Prefix):** Basic FEC. Used by LDP to signal an LSP to an IP prefix.
* **Type 127 (Generalized LSP):** Used by RSVP-TE for Traffic Engineered LSPs (tunnels).
* **Type 128 (Pseudowire ID):** Used by LDP to signal an L2 pseudowire (VPWS/VPLS).
* **Type 129 (Generalized PW):** Alternative to Type 128. Often used for VPLS auto-discovery.
* **Type 32 (MPLS-TP PW Path ID):** MPLS-TP static provisioning of a pseudowire path.

#### IP forwarding vs MPLS forwarding (mental model)

FECs are nothing new. Every router performing generic IP forwarding determines the next hop to which the packet is to be forwarded, the interface out which the packet is sent to get to that next hop, and how to queue the packet for that interface. But we don’t often hear those very basic procedures presented as “determining what FEC a packet belongs to.”

First let’s look at how a packet is forwarded across a path toward a destination using regular IP processes. A packet arrives at R1, and its destination IP address is examined. A lookup is performed, the packet’s FEC (again: next-hop, outgoing interface, and forwarding treatment) is determined, and using that information the packet is forwarded to the next hop router R2. R2 then repeats the process: The FEC is determined and the packet is forwarded to R3. R3 again determines the packet’s FEC and forwards it to R4.

In other words, the packet’s FEC is determined hop-by-hop at every router along the forwarding path toward the destination.

Now let’s suppose the four routers MPLS LSRs, and there is a Label Switching Path (an LSP, which is simply an MPLS virtual circuit) between R1 and R4; the termination point of the LSP is R4’s loopback interface, 192.168.255.1. As before, a packet arrives at R1 and the packet’s FEC is determined. In this case, however, the next hop for the FEC is the termination point of the LSP at R4. The packet is encapsulated with an MPLS header – that is, an MPLS header is PUSHed onto the packet – and the packet is forwarded out the interface to R2. At the remaining hops to R4, the packet is simply switched from its incoming interface to the outgoing interface designated by the MPLS switching table, with the MPLS label being SWAPed at each hop. At no point along the LSP does a router again determine the packet’s FEC until the packet exits the LSP (the MPLS header is POPed) at R4.

And that’s the key point: In an MPLS network the FEC is determined only once, at the ingress to an LSP, rather than at every router hop along the path. In our example, even though the LSP actually traverses R2 and R3, R1 “sees” the LSP as a single link to R4 and therefore chooses it as a better path than the hop-by-hop IP routed path through R2 and R3.

Conceptually, then, you can think of MPLS as a technology that pushes the “intelligence” to the edge of the network, leaving the core to do simple switching.

In other words, the network control plane is located at the edge and the forwarding plane is in the center. This separation of control and forwarding planes is very much in keeping with broad trends in networking, such as in high-performance routers that have long implemented the control and forwarding planes as separate physical components

### MPLS header

**MPLS header** is 32bit long containing:

* **Label (20-bit):** Identifier representing a destination (prefix) or service (VPN/QoS).\
  Label space is $$2^{20}=1{,}048{,}576$$ values.
* **EXP / Traffic Class (3 bits):** QoS (CoS/IP Precedence).
* **S / BoS (1 bit):** Bottom-of-stack. If set, this label is last in stack.
* **TTL (8 bits):** Same purpose as IP TTL. Copied from IP TTL at the LER.

#### EtherType

MPLS is recognized by EtherType:

* `0x8847` MPLS unicast
* `0x8848` MPLS multicast

![](<../.gitbook/assets/Unknown image (1387)>)

### Label Distribution Protocol (LDP) (IETF standard)

**Label Distribution Protocol (LDP)** is responsible for allocation, assignment and the exchange of label binding information between MPLS routers (Automatically enabled when enabling MPLS on an interface)

#### Prerequisites

MPLS control plane requires an already established IGP, typically OSPF or IS-IS, which propagates IP reachability between all routers in an MPLS domain

Each router determines its own shortest path to a destination, based on the calculated shortest path tree on each node

LDP then installs the next-hop of each best path to each network in the LFIB (data-plane) and allocates and propagates labels for those networks

The LDP is not responsible for finding a shortest, loop-free path to destinations. Instead, the LDP relies on routing protocols to find the best path to destinations. If, however, a loop does occur, a TTL field in the MPLS label prevents the packet from looping indefinitely.

#### LSR ID (LDP router-id)

Each LSR must have its own unique LSR ID (router-id). It is typically inherited from a loopback IP.

If not configured, the highest loopback IP or highest operational IP is chosen. Best practice is to use the IGP loopback IP.

To setup LDP neighborship the LSR RID must be reachable (eg. advertised via the IGP, static route, …)

#### TDP vs LDP (historical)

Cisco TDP (Tag Distribution Protocol) is legacy. LDP is the IETF standard and is widely used. TDP and LDP are incompatible, but can co-exist in the same network.

![VPN vs MPLS: What Are They and What's the Difference? | FS Community](<../.gitbook/assets/Unknown image (1388)>)

### LDP components

#### Core functions

Label switching routers generally perform these functions:

* Routing information exchange
* Label exchange
* Label allocation

They forward packets/frames (LSR and edge LSR).

LSR has a different function based on its position within MPLS domain

The differences among different LSR types are only in the architecture which means that a single device can perform more functions at the same time.

#### Ingress Label Switching Router (LSR) (LER / PE)

**Ingress Label Switching Router (LSR) (LER / PE)** is router that performs the L3 lookup and translates the destination IP address to label and attaches it to the outgoing packet. Egress LSR is also the PE router that removes labels for the egress packets coming from the MPLS domain destined to the IP network.

They run LDP to establish neighborship with LSR's and MP-BGP for peering with the remote PE's

PE router usually have eBGP peering with CE routers to exchane customer routes. CE router doesn't have MPLS enabled

#### Intermediate / transit Label Switching Router (LSR) (P)

**Intermediate / transit Label Switching Router (LSR) (P)** is the transit provider (P) router that runs an IGP with LDP to establish MPLS domain with other P and PE routers. It exchanges MPLS labels, build LIB with LFIB and perform label swapping based on the MPLS label

### Label-Switched Path (LSP)

**Label-Switched Path (LSP)** is an unidirectional path describing the sequence of LSRs that forwards labeled packets for particular FEC and swaps the top label. - The IP is also unidirectional

It is emphasized here to make a contrast to the older circuit-switching technologies like ISDN and ATM are bidirectional, since they established a permanent circuit - path over which the both ends communicated, essentially utilizing one path for communication

A LSP can be seen as one virtual circuit between the ingress LSR and egress LSR. Can be defined manually or setup completely automatically

Each LSP is created over the shortest path, selected by the IGP, toward the destination network. The return traffic is usually the reverse path because most routing protocols provide symetrical routing, and this can be influenced by MPLS TE

### LDP packet/message types

#### Hello (discovery)

Hello discovery messages are sent periodically on all-routers multicast IP 224.0.0.2 encapsulated in UDP port 646 to discover other LSR's to form adjacency and exchange label information

They contain LSR ID, the label pool range used by each LSR, which is dynamically or manually allocated

Upon reception of the hello packet, the LSR with the highest IP address initiates a TCP session on port 646 (One will be active and the second passive)

Then the keepalives are sent every 60 second between neighbors

**Basic discovery**

Directly connected neighbor discovery (224.0.0.2)

**Extended discovery (Targeted Hello)**

Extended discovery (LDP Targeted Hello) is used to configure LSR to direct the hello message to allow nonadjacent LDP neighbors to form neighborship

The LSR sends periodic hello as unicast to the specified neighbor

For example, when you use the MPLS traffic engineering tunnel interface, a label distribution session is established between the tunnel headend and the tunnel tail end routers. To establish this not-directly-connected MPLS LDP session, the transmission of targeted LDP hello messages is used. It is also used by LDP Session Protection described in this section

![](<../.gitbook/assets/Unknown image (1389)>)

![](<../.gitbook/assets/Unknown image (1390)>)

#### Notification

Notification message used for error signaling

#### Session initialization

Session Initialization message

Initiate, maintain and close LDP sessions. Active sends Initialization message, Passive is waiting

![](<../.gitbook/assets/Unknown image (1391)>)

#### Keepalive

Keepalive message

![](<../.gitbook/assets/Unknown image (1392)>)

#### Label mapping

Label Mapping message

Create, change and clean label mappings

![](<../.gitbook/assets/Unknown image (1393)>)

### MPLS frame mode

**MPLS frame mode** refers to how MPLS operates at the data plane, specifically the encapsulation method used to transmit labeled packets over a network.

Frame mode is the default mode of MPLS operation on most interfaces, such as Ethernet, Frame Relay, or PPP links.

In frame mode, MPLS labels are inserted between the Layer 2 header (e.g., Ethernet header) and the Layer 3 header (e.g., IP header) of a packet. This is often called a "shim header."

The packet is encapsulated as a Layer 2 frame (hence "frame mode"), and the MPLS label is used to forward the packet across the network.

Each LSR assigns a label to every destination in the IP routing table

LSRs announce their own assigned labels to all other LSRs. Each LSR builds its own LIB, LFIB, and FIB data structures based on received labels.

Frame Mode vs. Cell Mode: Frame mode is contrasted with cell mode, which is used on ATM (Asynchronous Transfer Mode) networks. In cell mode, MPLS labels are encoded into the VPI/VCI fields of ATM cells, and the packet is broken into fixed-size cells for transmission.

Frame mode is more common today because most modern networks use Ethernet or similar frame-based technologies, not ATM.

### Label distribution

![](<../.gitbook/assets/Unknown image (1394)>)

{% hint style="info" %}
The allocated label is advertised to all neighbor LSRs. This happens regardless of whether the neighbors are upstream or downstream LSRs for the destination.
{% endhint %}

![](<../.gitbook/assets/Unknown image (1395)>)

The router A receives the IP packet on the IP port, so it will check the next-hop for the destination in the FIB first

If the next-hop kpoint's to the MPLS neighbor it will show the label that the router should label the packet

![](<../.gitbook/assets/Unknown image (1396)>)

The outgoing label is inserted in the LFIB after the label is received from the next-hop LSR.

![](<../.gitbook/assets/Unknown image (1397)>)

### Label Information Base (LIB)

Similar to RIB. Maintains locally allocated and learned labels from LDP peers. LSRs store labels and related information inside a data structure called a LIB. The FIB and LFIB contain labels only for the currently used best LSP segment, while the LIB contains all labels known to the LSR, whether the label is currently used for forwarding or not. The LIB in the control plane is the database that is used by LDP

Each IP prefix in the RIB is assigned a locally significant label, which is mapped to a next-hop label learned from a downstream neighbor.

A label only has meaning to the router that assigns it (LSR) and the next-hop router that receives it.

Let’s say you have 3 routers in a line:

R1 -- R2 -- R3

R1 assigns label 100 for prefix 10.1.1.0/24 and sends that to R2.

R2 receives label 100 from R1 and might assign label 200 to the same prefix and send it to R3.

R2 might also assign label 100 to some other prefix it advertises to R1.

R1 will only ever care about the labels it receives from R2.

R2 keeps separate Label Forwarding Information Base (LFIB) entries for each incoming label per interface.

### Label Forwarding Information Base (LFIB)

(Similar to FIB.) Maintains the best path for each prefix with a locally assigned and outgoing labels received from the next hop.

### Label Switching Database (LSD)

Responsible for allocating and storing the pool of labels.

### Forwarding Information Base (FIB)

Used by the LER to forward unlabeled IP packets or to label packets if the next-hop is MPLS-enabled.

![Fib Lfib Cisco](<../.gitbook/assets/Unknown image (1398)>)

![](<../.gitbook/assets/Unknown image (1399)>)

### Label distribution parameters

#### Label space

**Platform-wide label space (per-platform)**

Router uses a single pool of labels for all interfaces.

LFIB doesn't store incoming interfaces because the same label is valid across all interfaces

The same label can be used across all interfaces for a particular destination - If multiple links exist between two routers, a single label (e.g., 100) can represent a specific destination (e.g., 10.1.1.0/24). No matter which interface the packet arrives on, the router identifies the destination using the same label

This Reduces the overall number of labels required, conserving CPU and memory resources.

**Interface-specific label space (per-interface)**

each interface on the router has its own pool of labels for forwarding packets

LFIB entries are tied to specific interfaces, enabling per-interface label control.

Offers enhanced security by limiting which interfaces can use specific labels, reducing the chance of unauthorized traffic.

Higher resource usage due to increased label count and administrative complexity

**Label space ID format**

4 bytes: router ID

2 bytes: unique (within platform) ID;

Platform-wide = 0

e.g., 172.12.0.55:0

Interface-specific > 0

e.g., 127.14.3.22:5

#### Label distribution mode

LDP exchanges subnet/label bindings using one of two methods: downstream unsolicited distribution or downstream-on-demand distribution. Both LSRs must agree as to which mode to use.

**Downstream unsolicited distribution**

Downstream unsolicited LSR distributes labels to its upstream neighbors without being explicitly requested. This is a proactive approach where the downstream router advertises labels as soon as it assigns them to the prefix. Fast response time to path changes

**Downstream-on-demand distribution**

Downstream-on-demand LSR distributes labels only when requested by its upstream neighbors. This is a reactive approach, where a downstream router waits for an upstream router to request a label mapping. LSR has to co-operate with other LSRs during a path change.

For each route in its route table, the LSR identifies the next hop for that route.

It then issues a request (via LDP) to the next hop for a label binding for that route.

When the next hop receives the request, it allocates a label, creates an entry in its LFIB with the incoming label set to the allocated label, and then returns the binding between the (incoming) label and the route to the LSR that sent the original request.

#### LSP control mode

**Ordered**

The router waits for the next-hop router to give it a label first.

Only then does it assign and advertise a label upstream.

Slower to build the label path, but ensures the best possible path.

Follows hierarchical structure

**Independent**

Each LSR can assign labels for a prefix independently without waiting for downstream routers. This allows for quicker label assignment, but can lead to less optimized paths during the convergence

#### Label retention

**Liberal label retention mode**

Each LSR keeps all labels received from LDP peers, even if they are not the downstream peers (the next-hop) for reaching network X.

If a prefix becomes unreachable, the LSR can almost immediately start forwarding labeled packets after IGP convergence, which increases the convergence speed for LDP, but the numbers of labels maintained for a particular destination will be larger and thus will consume more memory

**Conservative label retention mode**

LSR retain only those labels that are currently being used for forwarding. This saves memory but may lead to slower adaptation to network changes as new labels must be requested

#### Label persistence note

When a router is rebooted, it may assign a different label for the same destination than it had before. This behavior occurs because labels in MPLS are locally significant and dynamically assigned from the router's label pool during the control plane's convergence process. If persistent labels are required (e.g., for specific applications or troubleshooting), manual label assignments or additional mechanisms like configured label space or Segment Routing (SR) may be used to provide consistent behavior.

### Label operations

* **PUSH:** Add an MPLS label between the Layer 2 and Layer 3 headers. Typically done by ingress PE when traffic enters the MPLS domain.
* **SWAP:** Replace an MPLS label with a new one. Typically done by transit P routers.
* **POP:** Remove the MPLS label. Typically done by egress PE (or via PHP).
* **Penultimate Hop Popping (PHP):** Penultimate LSR pops the top label instead of the egress PE.

This way the LER only receives the packet with the VPN label to determine the appropriate VRF and perform the lookup for the destination IP in the packet to send it to the appropriate CE.

This reduces the processing load on the last hop LER which has to perform IP lookup for multiple customers

**Imposition (packet entering a MPLS):** PE adds the necessary headers (e.g. MPLS labels)

**Disposition (packet exiting a MPLS):** PE removes the MPLS headers

### Label types

Labels 0 through 15 are reserved labels. They signal/indicate that a special operation must be performed.

#### Reserved labels (common)

**Implicit Null (3)**

When a frame with label 3 is received from a neighbor LSR, the label is popped. Used by PHP.

{% hint style="info" %}
If you see `imp null` for both the peer and local, it’s a prefix for the P2P link between the LSRs.
{% endhint %}

**Explicit Null (0 for IPv4, 2 for IPv6)**

The egress PE requests the penultimate hop not to pop the label. Instead, it swaps to explicit-null to preserve EXP/TC end-to-end.

Purpose: preserve QoS/EXP marking end-to-end while still indicating “this is the final hop”.

**Entropy label**

Used to improve ECMP hashing by adding entropy to the label stack.

**Label stacking**

Encapsulation of an MPLS packet inside another MPLS packet. Used to tunnel service labels (VPN/VC) inside transport labels.

**Bottom-of-Stack (BoS)**

Indicates the bottom-most label in the MPLS label stack.

**Traffic Engineering (TE) label**

Used when forwarding packets through an RSVP-TE tunnel. Represents a TE LSP.

![](<../.gitbook/assets/Unknown image (1400)>)

**No Label / Unlabelled** - there is no label for the destination from the next hop or label switching is not enabled on the outgoing interface.

**Pop Label** - the next hop advertised an implicit NULL label for the destination, signalling that this LSR must pop the label before sending it to the neighbor LSR

**\[V]** indicates that this prefix is par of the VPN

**Aggregate Per-VRF Aggr\[V]** indicates that this label describes the VPN

For locally connected interfaces and prefixes behind PE you will see "Unlabelled", "Aggregate" or "No label" in LFIB

#### IOS-XR

![](<../.gitbook/assets/Unknown image (1401)>)

#### IOS-XE

Mind the difference in local label allocation (first column). On the IOS-XR, all labels are above SRGB (default 24000 and above), for IOS-XE dynamic range by default begins by label 16.

R21# show mpls label range

Downstream Generic label region: Min/Max label: 16/1048575

R21#

For IOS-XR

RP/0/RP0/CPU0:R01#show mpls label range

Range for dynamic labels: Min/Max: 24000/1048575

RP/0/RP0/CPU0:R01#

![](<../.gitbook/assets/Unknown image (1402)>)

Default Cisco IOS packet router setting

Platform-wide label space

Unsolicited distribution

Independent control

Liberal label retention

IOS XE

<table><thead><tr><th width="328.20001220703125">(config-if)# mpls ip</th><th>Enables label switching on a frame-mode interface</th></tr></thead><tbody><tr><td>(config-if)# tag-switching ip</td><td></td></tr><tr><td>(config-if)# mpls label protocol ldp</td><td>enables LDP</td></tr><tr><td>mpls ldp router-id Loopback0 [force]</td><td>set the router ID for LDP to the IP address of the Loopback0 interface force - Alters the behavior of the mpls ldp router-id command to force the use of the named interface as the LDP router ID . (Changes router-id immediately, ldp sessions will restart)</td></tr><tr><td>(config)# mpls label range minimum maximum [static min-static-label max-static-label] Example: mpls label range 1000 1999</td><td>specifies the range for the labels for a router; locally significant; used as distinguisher (R2: 2000 2999; R3 3000 3999). Usually doesnt need to be configured, if no deterministic label allocation is required. Reload required for this configuration to take an effect</td></tr><tr><td>(config)# mpls static binding ipv4 prefix mask [input | output nexthop] label</td><td>IOS XE Static mpls label for prefix. Static record in the LFIB #show mpls static crossconnect</td></tr><tr><td>(config)# mpls static address-family ipv4 unicast local-label &#x3C;> allocate per-prefix prefix</td><td>IOS XR sh mpls static local-label all</td></tr><tr><td><p>Router(config)# mpls ldp neighbor password </p><p>Router(config)# mpls ldp password required</p></td><td>## Configuring LDP authentication</td></tr><tr><td>## Configuring a LDP neighbor manually (targeted session) Router(config)# mpls ldp neighbor targeted Router(config)# mpls ldp discovery targeted-hello accept from [ACL]</td><td></td></tr><tr><td>show mpls ldp [discovery | neighbor | parameters]</td><td></td></tr><tr><td>show mpls [neighbor | interfaces]</td><td>Show MPLS status of a particular interfaces</td></tr><tr><td>show mpls ldp bindings</td><td>LIB - The output displays local labels for each destination network, and the labels that are received from all LDP neighbors</td></tr><tr><td>show mpls forwarding-table labels &#x3C;> - &#x3C;></td><td>LFIB</td></tr><tr><td>debug mpls [ldp | lfib | packets]</td><td></td></tr></tbody></table>

IOS-XR

| RP/0/RP0/CPU0:router(config)#mpls ldp RP/0/RP0/CPU0:AS01-PE04(config)# mpls ldp router-id x.x.x.x RP/0/RP0/CPU0:router(config-ldp)#commit RP/0/RP0/CPU0:router(config-ldp)#interface GigabitEthernet0/0/0/0 RP/0/RP0/CPU0:router(config-ldp-if)#commit | Enables label switching on a frame-mode interface, TDP protocol is not supported Starts up LDP on an interface Never run mpls with your customers: interface Gi0/0/0/0 ip access-group NoLDP ingress mpls ldp interface GigabitEthernet0/0/0/0 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

{% hint style="info" %}
Never run MPLS with your customers. Use access lists to prevent customers from starting TDP/LDP
{% endhint %}

| ip access-list NoLDP deny tcp any any eq 646 |   |
| -------------------------------------------- | - |
| ip access-list NoLDP permit ip any any       |   |

### MTU in MPLS

MPLS forwarding is based on label switching, where transit LSRs only process the MPLS label stack and do not inspect or make forwarding decisions based on the inner IP header. Because of this encapsulation, fragmentation behavior differs from native IP forwarding. If an ingress PE sends an MPLS-encapsulated packet (e.g., 9000+ bytes including payload and labels) and an intermediate link has a lower MTU (e.g., 1500 bytes), the transit LSR cannot perform IP-layer fragmentation since the original IP header is not visible in a way that allows normal fragmentation processing. Instead, the labeled packet must already comply with the MTU constraints of every link along the LSP, including MPLS label overhead (stack, control word if used, etc.). If the packet exceeds the outgoing interface MTU, it is typically dropped at the ingress of that link, and depending on platform behavior, an ICMP “Fragmentation Needed” / “Packet Too Big” message may be generated back towards the source. However, MPLS forwarding itself does not perform fragmentation of labeled packets in the transit path, so correct operation requires consistent end-to-end MTU alignment across all MPLS-enabled interfaces, including transport and service encapsulation overhead (LDP, SR-MPLS, VPN labels).



Label switching increases the demands on the maximum MTU of an interface – caused by additional MPLS header. MPLS MTU is increased to 1512 Bytes to support 1500-Bytes IP packets and MPLS stack of a depth up to the third level. (3\*4Bytes = 12Bytes) The Ethernet standard says that a frame can be as large as 1518 bytes. 18 of those will be Ethernet headers, leaving 1500 bytes as the IP MTU. So, if we have a 1500 byte packet, two labels, and Ethernet headers, the frame size is now 1526 bytes. This is just over the 1518 byte limit, so it’s called a Baby Giant. Strictly speaking, the Ethernet standard says that this should be dropped, as it’s too large. However, most modern routers and switches turn a blind eye and allow Baby Giants. It is possible that your LSR, or another device in the path, does not support baby giants. This would mean that the frame size cannot go over 1518 bytes. So what do you do now? To account for this, the MPLS MTU (that is, the maximum size for the packet plus the MPLS labels) can be lowered to 1500 bytes, preventing it from going over the limit. That means your maximum IP MTU, and MSS if you’re adjusting it, will also need to be lowered.

XE(config)# mpls mtu mtu-size&#x20;

RP/0/RP0/CPU0:PE-003-XR(config-if)#mpls mtu

### MPLS forwarding operation

![](<../.gitbook/assets/Unknown image (1403)>)

Router A performs a FIB lookup. The FIB for that destination states that the packet should be labeled using label 25 and sent to router B.

Router A adds a label 25 and the packet is sent out the interface that connects to router B.

Router B receives an IP packet that is labeled with label 25. Router B performs an LFIB lookup, which states that label 25 should be swapped with label 34.

The label is swapped and the packet is sent to router C.

Router C receives an IP packet that is labeled with label 34. Router C performs an LFIB lookup, which states that label 34 should be removed \[penultimate hop popping (PHP)], and the unlabeled IP packet should be sent out the interface that connects to router D. POP is often used as a label value that indicates that a label should be removed.

The label is removed and the unlabeled IP packet is sent out the interface that connects to router D. Router will display a value of IMP-NULL instead of POP. An implicit null label means that the label should be removed. An IMP-NULL label uses the value 3 from a reserved range of labels.

Finally, router D receives an IP packet. Router D performs a FIB lookup, which states that the destination network is directly connected.

The IP packet is sent out the directly connected interface.

### Load balancing labeled packets

If multiple equal-cost paths exist for an IPv4 prefix, Cisco IOS XR Software can load-balance labeled packets. When labeled packets are load-balanced, they can have the same or different outgoing labels. The outgoing labels are the same if the two links are between a pair of routers and both links belong to the platform label space. If multiple next-hop LSRs exist, the outgoing label for each path is usually different, because the next-hop LSRs assign labels independently. The label is the same since the labes are by default assigned per prefix, not per link or anything else.

[https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/mpls/25xx/configuration/guide/b-mpls-cg-cisco8000-25xx/implementing-mpls-static-labeling.html](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/mpls/25xx/configuration/guide/b-mpls-cg-cisco8000-25xx/implementing-mpls-static-labeling.html)

Confirmed in lab that the cef ensures load balancing automatically for two links between same ospf/ldp neighbors:

IOS-XE#show ip cef 3.3.3.3/32 detail

3.3.3.3/32, epoch 0, per-destination sharing

dflt local label info: global/19 \[0x0]

nexthop 10.10.10.2 GigabitEthernet0/0 label 17-(local:19)

nexthop 20.20.20.2 GigabitEthernet0/1 label 17-(local:19)

IOS-XE#show ip cef 3.3.3.3/32 internal

3.3.3.3/32, epoch 0, RIB\[I], refcnt 5, per-destination sharing

sources: RIB, LTE

feature space:

IPRM: 0x00028000

LFD: 3.3.3.3/32 1 local label

dflt local label info: global/19 \[0x0]

contains path extension list

dflt disposition chain 0xD2EDD1C

loadinfo 0D2EDD1C, per-session, 2 choices, flags 009B, 7 locks

dflt label switch chain 0xD2EDD1C

loadinfo 0D2EDD1C, per-session, 2 choices, flags 009B, 7 locks

ifnums:

GigabitEthernet0/0(2): 10.10.10.2

GigabitEthernet0/1(3): 20.20.20.2

path list 0E47A0B4, 9 locks, per-destination, flags 0x49 \[shble, rif, hwcn]

path 0E47A4C4, share 1/1, type attached nexthop, for IPv4

MPLS short path extensions: \[none] MOI flags = 0x0 label 17

nexthop 10.10.10.2 GigabitEthernet0/0 label 17-(local:19), IP adj out of GigabitEthernet0/0, addr 10.10.10.2 0FA03DC8

path 0E47A530, share 1/1, type attached nexthop, for IPv4

MPLS short path extensions: \[none] MOI flags = 0x0 label 17

nexthop 20.20.20.2 GigabitEthernet0/1 label 17-(local:19), IP adj out of GigabitEthernet0/1, addr 20.20.20.2 0DB40928

output chain:

loadinfo 0D2EDD1C, per-session, 2 choices, flags 009B, 7 locks

flags \[Per-session, for-rx-IPv4, for-mpls-at-eos, for-mpls-not-at-eos, 2buckets]

2 hash buckets

< 0 > label 17-(local:19)

TAG adj out of GigabitEthernet0/0, addr 10.10.10.2 0FA03C98

< 1 > label 17-(local:19)

TAG adj out of GigabitEthernet0/1, addr 20.20.20.2 0DB407F8

Subblocks:

### Loop detection and prevention in MPLS

LDP relies on loop detection mechanisms that are built into the IGPs. If, however, a loop is generated (that is, misconfiguration with static routes), the TTL field in the label header is used to prevent the loops.

**Prevention** is the process of loop removing

Routing protocols ensure a topology without loops within an IP environment.

MPLS uses information provided by the routing table while creating LSP ---> MPLS utilizes the same mechanism to prevent loops as IP

Loop prevention is Control Plane task

**Detection** is the process of loop existence checking

MPLS forwarding/Data plane is responsible for loop detection

The same method as the one used by IP protocol

TTL checked at each L3 hop

Each hop decreases the TTL value by 1

If the TTL value reaches zero (0) the packet is dropped

#### TTL propagation in MPLS

By default, IP TTL is copied to the MPLS header during the label imposition and afterwards, MPLS TTL is copied back to the IP TTL during the removal of the label (= ip ttl propagation is enabled by default)

This results in a full disclosure of the MPLS core network if a traceroute is performed on the CE router

When IP TTL propagation disabled, the TTL value of the IP header won’t be copied into the MPLS header anymore, but instead the MPLS header uses a fixed TTL start value of 255 which doesn’t get copied back into the IP header once it leaves the LSP

{% hint style="info" %}
Ensure TTL propagation is either enabled on all routers or disabled on all routers. If TTL is enabled on some routers and disabled on others, packets can leave the MPLS domain with a higher TTL than when they entered.
{% endhint %}

| Router(config)# no mpls ip propagate-ttl \[forwarded \| local] /IOS RP/0/RP0/CPU0:router(config)#mpls ip-ttl-propagate disable ? forwarded Disable IP TTL propagation for only forwarded MPLS packets local Disable IP TTL propagation for only locally generated MPLS /XR | This command discard IP TTL and label TTL propagation. A TTL value of 255 is inserted into the label header. The TTL propagation has to be disabled on both the ingress and egress edge LSR. Forwarded (traceroute does not work for transit traffic which is labeled by that router) Local (traceroute does not work from that router but works for transit traffic which is labeled by that router) |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

![](<../.gitbook/assets/Unknown image (1404)>)

There is possible fine tune MPLS to:

Optimize LIB/FIB/LFIB usage (label allocation and filtering)

Speed up convergence (LDP IGP synchronization, LDP session protection, NSF)

To utilize traffic engineering/fast reroute capabilities (FRR)

### LDP session protection

When a link comes up, IP converges earlier and much faster than MPLS LDP and may result in MPLS traffic loss until MPLS convergence. If a link flaps, the LDP session will also flap due to loss of link discovery. LDP Session Protection minimizes traffic loss, provides faster convergence, and protects existing LDP (link) sessions by using a "parallel" source of targeted discovery hello.

An LDP session is kept alive and neighbor label bindings are maintained when links are down. Upon re-establishment of primary link adjacencies, MPLS convergence is expedited because LDP does not need to relearn the neighbor label bindings.

LDP Session Protection lets you configure LDP to automatically protect sessions with all or a given set of peers. When it is configured, LDP initiates backup targeted hellos automatically for neighbors for which primary link adjacencies already exist. These backup targeted hellos maintain LDP sessions when primary link adjacencies go down

MPLS LDP Session Protection protects an LDP session between DIRECTLY CONNECTED neighbors or an LDP session established for a traffic engineering (TE) tunnel.

By default, Session Protection is disabled. When it is enabled without peer ACL and duration, Session Protection is provided for all LDP peers, and continues for 24 hours after a link-discovery loss. This LDP Session Protection feature allows you to enable the automatic setup of targeted hello adjacencies with all or a set of peers, and specify the duration for which the session needs to be maintained using targeted hellos, after loss of link discovery. LDP supports only IPv4 standard access lists.

{% hint style="info" %}
Recent IOS versions establish the LDP session to the **LDP router-id**. If session protection is not configured, an interface down event immediately terminates the LDP session.
{% endhint %}

Both routers must have LDP session protection configured with consistent timers to avoid session timeouts during failures. This involves setting the same LDP session protection timer value on both the PE and P routers to ensure proper coordination

![](<../.gitbook/assets/Unknown image (1405)>)

| IOS-XE(config)# mpls ldp session protection \[vrf vrf-name] \[for acl] \[duration {infinite \| seconds}] | for acl - (Optional) Specifies a standard IP access control list that contains the prefixes that are to be protected. duration - (Optional) Specifies the time that the LDP Targeted Hello Adjacency should be retained after a link is lost. (default = without “duration” = 86400 seconds = 24h) vrf vpn-name (Optional) Specifies a VPN routing and forwarding instance (vpn-name) for accepting labels. This keyword is available when the router has at least one VRF configured. It is also sufficient to enable mpls ldp session protein globally and it is automatically enabled on all vrfs for acl (Optional) Specifies a standard IP access control list that contains the prefixes that are to be protected. duration (Optional) Specifies the time that the LDP Targeted Hello Adjacency should be retained after a link is lost. Note If you use this keyword, you must select either the infinite keyword or the seconds argument. infinite Specifies that the LDP Targeted Hello Adjacency should be retained forever after a link is lost. seconds Specifies the time in seconds that the LDP Targeted Hello Adjacency should be retained after a link is lost. The valid range of values is 30 to 2,147,483 seconds. |
| -------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IOS-XR(config)# mpls ldp session protection                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| # session protection duration <30-2147483> \| infinite)                                                  | you need to enable LDP session protection and specify a duration for how long the session should be protected after a link failure. The duration must be the same on both routers (PE and P) to ensure consistent behavior.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

### LDP-IGP synchronization

Packet loss can occur because the actions of the Interior Gateway Protocol (IGP) and the Label Distribution Protocol (LDP) are not synchronized. Packet loss can occur in the following situations:

When an IGP adjacency is established, the device begins forwarding packets using the new adjacency before the LDP label exchange completes between the peers on that link.

If an LDP session closes, the device continues to forward traffic using the link associated with the LDP peer rather than an alternate pathway with a fully synchronized LDP session.

LDP IGP synchronization synchronizes LDP and IGP so that IGP advertises links with regular metrics only when MPLS LDP is converged on that link.

LDP-IGP synchronization forces the IGP to advertise the link with a maximum metric (highest cost) whenever LDP is not fully operational on that interface, and have IGP to reroute traffic to avoid the affected neighbor (assuming that and alternate path is available)

This prevents the IGP from selecting the link as the preferred route until the LDP session is fully established and labels have been successfully exchanged

The LDP IGP synchronization feature is only supported for OSPF or IS-IS

| IOS-XE(config-router)# mpls ldp sync                                                                                                                                                                                                                    | ## Enabling LDP-IGP synchronization                                                                                                                                                |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IOS-XR(config-router)# mpls ldp sync                                                                                                                                                                                                                    | #show mpls ldp igp sync                                                                                                                                                            |
| Router(config-if)# mpls ldp igp sync delay seconds Router(config)# mpls ldp igp sync holddown msec                                                                                                                                                      | Adding fixed delay (per interface) after LDP convergence is finished. Max. waiting time for LDP (avoid waiting forever …) Without this command the “default delay” is INFINITY !!! |
| <p>RP/0/RP0/CPU0:AS01-PE04(config)#mpls ldp igp sync delay on-session-up ? &#x3C;5-300> Interface sync-up delay (seconds)<br>RP/0/RP0/CPU0:AS01-PE04(config)#mpls ldp igp sync delay on-proc-restart ? &#x3C;60-600> Global sync-up delay (seconds)</p> | Timers are optional tuning; the **prerequisite is using a supported IGP**, i.e., OSPF or IS-IS.                                                                                    |

### IGP LDP autoconfiguration

automatically configure LDP on all interfaces that are associated with a specified OSPF or IS-IS instance

| (config-router)# mpls ldp autoconfig area area\_id           | IOS-XE; applies for both OSPF and IS-IS |
| ------------------------------------------------------------ | --------------------------------------- |
| RP/0/RP0/CPU0:AS01-PE04(config-ospf-ar)#mpls ldp auto-config | IOS-XR                                  |

![](<../.gitbook/assets/Unknown image (1406)>)

### MPLS OAM (Operations, Administration, and Maintenance)

**MPLS OAM (Operations, Administration, and Maintenance)** encompasses the tools and protocols used by service providers to monitor label-switched paths (LSPs) and quickly isolate MPLS forwarding problems

The basic ping between PEs identify only IP reachability - the IGP, but not the actual LSP, which should consist of the correct label sequence

On Cisco devices in an MPLS network (like with VRFs), both traditional IP ping/traceroute and MPLS LSP ping/traceroute test connectivity and trace paths. Cisco’s traceroute is enhanced to show MPLS labels when packets travel over an MPLS LSP, making it look similar to MPLS LSP Traceroute.

| (config)# mpls oam | recommended to enable mpls oam, for IOS-XE and IOS-XR |
| ------------------ | ----------------------------------------------------- |

#### MPLS LSP ping

MPLS LSP ping uses MPLS echo request and reply packets to validate an LSP.

MPLS ping uses UDP on port 3503 to encode two types of messages:

**MPLS echo request:** is sent to a target device through the use of the appropriate label stack associated with the LSP to be validated. Use of the label stack causes the packet to be switched inband of the LSP (that is, forwarded over the LSP itself).

The MPLS echo request will use the outgoing interface IP address as the source.

The destination IP address of the MPLS echo request packet is different from the address used to select the label stack. The destination address of the UDP packet is defined as a 127.x .y .z /8 address. This prevents the IP packet from being IP switched to its destination if the LSP is broken. An LSP is not required for the reply to reach the ingress MPLS router

The TTL in MPLS ping is set to 255. Using the 127/8 address in the IP header destination address field will cause the packet to not be forwarded by any routers using the IP header, if the LSP is broken somewhere inside the MPLS domain. The default reply mode uses IPv4 and MPLS to return the reply.

The MPLS echo request message includes the information about the tested FEC (prefix) which is encoded as one of the TLVs. Additional TLVs can be used to request additional information in replies. The downstream mapping TLV can be used to request information about additional information such as downstream router and interface, MTU, and multipath information from the router where the request is processed.

**MPLS echo reply:** The MPLS echo reply uses the same packet format as the request, except that it may include additional TLVs to encode the information. The basic reply is, however, encoded in the reply code field.

An MPLS echo reply is sent in response to an MPLS echo request. It is sent as an IP packet and forwarded using IP, MPLS, or a combination of both types of switching. The source address of the MPLS echo reply packet is an address from the device generating the echo reply. The destination address is the source address of the device in the MPLS echo request packet

| ping mpls \[OPTIONS]   | fec-type generic (default) in an MPLS LSP ping means you are testing a basic LSP using an LDP FEC for an IPv4 prefix. If you wanted to test other FEC types (e.g., MPLS TE tunnels or VPN routes), you would use different options (e.g., fec-type traffic-eng) verbose displays more details about each hop dsmap option can be used to retrieve the details for a given hop![](<../.gitbook/assets/Unknown image (1407)>) |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show mpls oam counters |                                                                                                                                                                                                                                                                                                                                                                                                                             |

#### MPLS LSP traceroute

The MPLS LSP Traceroute feature is used to isolate the failure point of an LSP. It is used for hop-by-hop fault localization and path tracing. The MPLS LSP Traceroute feature relies on the expiration of the Time to Live (TTL) value of the packet that carries the echo request. When the MPLS echo request message hits a transit node, it checks the TTL value and if it is expired, the packet is passed to the control plane, else the message is forwarded. If the echo message is passed to the control plane, a reply message is generated based on the contents of the request message.

R1 sends a label-switched packet with a label of 47 and a TTL of 1 to R2. The TTL field of the IP packet is copied onto the TTL field of the label header.

R2 sees that it is not the intended recipient and the TTL is 1. It drops the packet and creates a TTL-expired ICMP message, as it would for a regular IP packet. In this case, the ICMP message packet is generated per ICMP extensions for MPLS.

R2 appends the label 47 (the incoming label that expired) to the ICMP message. It does not send the packet to R1 directly. Instead, it consults its LFIB and finds it should use a label of 45 for packets received with a label of 47. It puts a label of 45 on the packet and sends the TTL-expired ICMP message to R3.

R3 pops the label and sends it to R4. R4 sees that the destination is R1, gives a label of 28 to the message, and sends it through R3 and R2 to R1.

The ICMP error message travels all the way to the other end before it is sent back to R1.

R1 receives the ICMP message and sends another packet with a label of 47 and a TTL of 2 to R2. R2 swaps labels, decrements TTL (from 2 to 1) and forwards to R3. As in Step 2, R3 sends a TTL-expired ICMP message appended with the incoming label that expired to R4, and R4 then sends it back to R1.

Upon receipt of the ICMP message, R1 sends another packet with a label of 47 and a TTL of 3. On its way, R2 and R3 decrement the TTL and forward the packet to R4. R4 notes that it is the intended recipient and finds the UDP datagram port unreachable. It sends an ICMP port unreachable message to R1 through R3 and R2.

| traceroute mpls \[OPTIONS] | Best practice is to traceroute from one PE to another PE using the loopback interfaces as destination/source |
| -------------------------- | ------------------------------------------------------------------------------------------------------------ |

![](<../.gitbook/assets/Unknown image (1408)>)

| **Output code** | **Echo Return Code** | **Meaning**                          |
| --------------- | -------------------- | ------------------------------------ |
| x               | 0                    | No return code.                      |
| M               | 1                    | Malformed echo request               |
| m               | 2                    | Unsupported TLV                      |
| !               | 3                    | Success                              |
| F               | 4                    | No FEC mapping                       |
| D               | 5                    | DS map mismatch                      |
| I               | 6                    | Unknown upstream interface index     |
| U               | 7                    | Reserved                             |
| L               | 8                    | Labeled output interface             |
| B               | 9                    | Unlabeled output interface           |
| f               | 10                   | FEC mismatch                         |
| N               | 11                   | No label entry                       |
| P               | 12                   | No receive interface label protocol. |
| p               | 13                   | Premature termination of the LSP.    |
| X               | unknown              | Undefined return code.               |

| traceroute mpls multipath | command is used in MPLS networks to trace the Label Switched Path (LSP) and provide details about multiple paths taken by packets in an MPLS network. It shows the specific paths through the network when multiple paths exist, typically due to Equal-Cost Multi-Path (ECMP) routing or MPLS TE (Traffic Engineering). |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

![](<../.gitbook/assets/Unknown image (1409)>)

The fundamental operation of MPLS LSP Ping/Traceroute is to force transit nodes to process the diagnostic packet, which is achieved precisely by using one of the alert mechanisms. The RFC defines below three labels that were allocated for this specific purpose, however I confirmed in lab that the router alert is integrated within the IP header in options section

| Label Value | Name                                   | Primary Function for OAM/MPLS                                                                                                                                                                                                                    |
| ----------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1           | Router Alert Label                     | Forces the receiving router to forward the packet to the local CPU for deep inspection and processing, even though it's a labeled packet. This is used by LSP Ping and Traceroute to process the diagnostic request/reply at the correct hop.    |
| 13          | Generic Associated Channel Label (GAL) | Used in MPLS-TP (Transport Profile) and Pseudowire (PW) OAM (VCCV). It signals the presence of an ACH (Associated Channel Header) immediately following it, which contains the specific OAM message (e.g., Continuity Check, Delay Measurement). |
| 14          | OAM Alert Label                        | Defined primarily by ITU-T (Y.1711). Its purpose is to explicitly tag a packet as an OAM packet. In practice, its use is less universal than the Router Alert Label (1), and many vendors still rely on Label 1 for on-demand diagnostics.       |

![](<../.gitbook/assets/Unknown image (1410)>)

### Security for LDP

Authentication with MD5 option

| neighbor ip-address password \[encryption] password | enable LDP session authentication with MD5 option |
| --------------------------------------------------- | ------------------------------------------------- |

### Label advertisement control (outbound filtering / conditional distribution)

LDP by default advertises labels for all prefixes to all neighbors. For scalability and security, LDP outbound label filtering can be configured to control local label advertisement to specific peers or prefixes. Use show mpls ldp bindings to verify LIB

|                                                                                                               |                                                                                                                                                                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| mpls ldp advertise-labels \[for prefix-access-list \[to peer-access-list]]                                    | XE                                                                                                                                                                                                                                                                                            |
| mpls ldp address-family ipv4 label local advertise {disable \| for prefix-acl \[to peer-acl] \| interface <>} | XR for prefix-access-list: Specifies which destinations will have labels advertised. to peer-access-list: Specifies which LSR neighbors will receive the advertisement (identified by router ID). interface: Specifies an interface for label allocation and advertisement of its IP address. |

Example

Customer (SP/Enterprise) already has a functional IP infrastructure.

He needs MPLS only for the support of MPLS VPN services.

Labels are necessary only for a loopback interface (BGP next hops) on every routers

Every loopback interfaces share a common continuous address block (192.168.254.0/24)

| no mpls ldp advertise-labels ! ! Configure conditional advertisments: ! mpls ldp advertise-labels for 90 to 91 ! access-list 90 remark Loopback IP space access-list 90 permit 192.168.254.0 0.0.0.255 ! access-list 91 remark MPLS VPN LDP neighbors (ALL) access-list 91 permit any |   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

### Label acceptance control (inbound filtering)

By default, LDP accepts labels (as remote bindings) for all prefixes from all peers. LDP operates in liberal label retention mode, that instructs LDP to keep remote bindings from all peers for a given prefix. For security reasons, or to conserve memory, you can override this behavior by configuring label binding acceptance for a set of prefixes from a given peer.

In a simple MPLS VPN environment, the PE routers may require LSPs only to their peer PE routers (that is, they do not need LSPs to core routers). Inbound label binding filtering enables a PE router to accept labels only from other PE routers.

Restrictions Inbound label binding filtering does not support extended ACLs; it only supports standard ACLs.

| XR(config)# label accept for prefix-acl from A.B.C.D                        | Accepts and retains remote bindings for prefixes that are permitted by the prefix access list prefix-acl. Defines neighbor IP address from which router can accept inbound labels. |
| --------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| XE(config)# mpls ldp neighbor \[vrf vrf-name] nbr-address labels accept acl |                                                                                                                                                                                    |

The result is that no label is allocated in the LFIB

![](<../.gitbook/assets/Unknown image (1411)>)

### Local label allocation filtering

LDP default behavior is to allocate local labels for all non-BGP prefixes.

Labels are sent to neighbors even for prefixes that are irrelevant for MPLS.

A prefix-list is used to control which routes in the global routing table get labels.

Only the routes matching the prefix-list or host routes get labels allocated.

Labels are no longer assigned to unwanted prefixes, optimizing label usage

For BGP routes where BGP assigns local labels (e.g., Inter-AS Option C scenarios), LDP doesn’t interfere with label allocation

Commonly implemented on the core routers, so that they allocate label only for loopback prefixes of PE/P routers in the MPLS domain - to conserve their memory

| Router(config)# mpls ldp label Router(config-ldp-lbl)# allocate global {prefix-list {list-name \| list-number} \| host-routes} | RP/0/RP0/CPU0:PE14(config-ldp)#mpls ldp address-family ipv4 label local allocate for host-routes |
| ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ |

![](<../.gitbook/assets/Unknown image (1412)>)

### MPLS troubleshooting

Before you start in-depth MPLS VPN troubleshooting, you should ask the following standard MPLS troubleshooting questions:

Is CEF enabled on all routers in the transit path between the provider edge (PE) routers?

Are labels for BGP next hops generated and propagated?

Are there any MTU issues in the transit path (for example, LAN switches not supporting a jumbo Ethernet frame)

Verify control plane (Route propagation) Direction: from data destination to source

Verify data plane Direction: from data source to destination

To resolve MPLS VPN issues, you should verify the routing information flow.

Verify the routing information flow:

1. Are CE routes received by a PE router?

Verify with the #show ip route vrf vrf-name command on PE1.

1. Are routes redistributed into MP-BGP with proper extended communities?

Verify with the #show ip bgp vpnv4 vrf vrf-name ip-prefix command on PE1. #sh ip bgp vpnv4 unicast vrf vrf-name ip-prefix

Troubleshoot with debug ip bgp commands.

1. Are VPNv4 routes propagated to other PE routers?

Verify with the #show ip bgp vpnv4 all ip-prefix/length command. #sh bgp vpnv4 unicast ip-prefix/length

1. Is the BGP route selection process working correctly?

Verify with the #show ip bgp vpnv4 vrf vrf-name ip-prefix command.

Change local preference or weight settings if needed.

Do not change MED if you are using IGP-BGP redistribution - since MED is used to reflect the originating IGP original metric like in OSPF

1. Are VPNv4 routes inserted into VRFs on other PE routers?

Verify with the #show ip route vrf command.

Troubleshoot with the# show ip bgp ip-prefix and #show ip vrf detail commands.

1. Are VPNv4 routes redistributed from BGP into the PE-CE routing protocol?

Verify redistribution configuration—is the IGP metric specified?

1. Are IPv4 routes propagated to other CE routers?

Verify with the #show ip route command on CE.

Alternatively, do CE2B have a default route toward PE2?

You should verify routing information flow systematically, by starting at the ingress CE router and then moving to the egress CE router.

After you have verified proper route exchange, start MPLS VPN data flow troubleshooting using the checks that are listed below.

Verify proper data flow:

1. Is CEF enabled on the ingress PE router interface? CEF is the only switching method that can perform per-VRF lookup and thus support MPLS VPN architecture

Verify with the show cef interface command

1. Is the CEF entry correct on the ingress PE router?

Display the CEF entry with the #show ip cef vrf vrf-name ip-prefix/length detail command.

Verify the label stack in the CEF entry

1. Is there an end-to-end label switched path tunnel (LSP tunnel) between PE routers?

Check summarization issues—BGP next hop should be reachable as host route.

Quick check—if TTL propagation is disabled, the trace from PE2 to PE1 should contain only one hop

If needed, check LFIB values hop by hop.

Check for MTU issues on the path—MPLS VPN requires a larger label header than pure MPLS.

The quickest way to diagnose summarization problems is to disable IP TTL propagation into the MPLS label header, using the no mpls ip ttl-propagate configuration command on the P router and PE routers. The traceroute command from the ingress PE router toward the BGP next hop should display no intermediate hops when TTL propagation is disabled. If intermediate hops are displayed, the LSP tunnel between PE routers is broken at those hops and the VPN traffic cannot flow

1. Is the LFIB entry on the egress PE router correct?

Find out the second label in the label stack on PE2 with the show ip cef vrf vrf-name ip-prefix detail command.

Verify correctness of LFIB entry on PE1 with the show mpls forwarding vrf vrf-name value detail command.

As a last troubleshooting measure (usually not needed), you can verify the contents of the LFIB on the egress PE router and compare them with the second label in the label stack on the ingress PE router. A mismatch indicates an internal Cisco IOS Software error that you will need to report to the Cisco Technical Assistance Center (TAC).

![](<../.gitbook/assets/Unknown image (1413)>)

### Load balancing in MPLS networks (advanced)

By default and depending on the platform capability, standard L3VPN

provides good hashing results, except with services where the vast majority

of fields are the same and the customer-specific information is hidden too

deeply in the packet. GRE over L3VPN over BGP-LU over LDP is such a

problematic service, where an overlay using GRE is spanned between two

CE nodes over an L3VPN. With a multitude of customer services being

transported through a single GRE tunnel, it would require a deeper packet

inspection to be able to extract all customer-specific information. Lacking

this inspection would result in poor hashing as there would be a single hash

for all customers.

RFC 6790 defines the concept of an Entropy label, which is applicable to

both L2VPN and L3VPN services and eliminates the need for deep

inspection on transit routers. Instead, the ingress PE device extracts the

relevant field of the service before the MPLS encapsulation takes place;

then the device computes the hash and pushes the result as an additional

label onto the stack. Transit routers no longer need to guess the underlying

packet structure and can instead effectively load balance traffic by relying

on the packet’s MPLS label stack. For this to work, all devices in the path

must support the Entropy label.

The inspection of L2VPN services is more challenging than the inspection

of L3VPN services. As presented in the beginning of this chapter, the

MPLS header does not specify the payload that follows after popping the

Bottom of Stack (BoS) label. Instead, the router has to make an educated

guess. In order not to sacrifice too much performance or too many network

processor cycles, it is common to inspect the first nibble that follows the

BoS label.

![](<../.gitbook/assets/Unknown image (1414)>)

{% hint style="info" %}
The first nibble after the BoS label is often inspected:

* `0100` → IPv4 header
* `0110` → IPv6 header
* Anything else → treated as Layer 2 payload

This pragmatic approach breaks if a MAC address starts with `0x4` or `0x6`. The packet can be misread as IP. Load-balancing becomes nondeterministic. User experience can suffer.
{% endhint %}

#### Ethernet control word (RFC 4385)

RFC 4385 defines the Ethernet control word, which solves the

misinterpretation issue by inserting a 4-byte control word where the first

nibble is 0000. In essence, ensures that transit routers do not falsely

interpret the L2VPN pseudowire payload as IPv4 or IPv6 traffic and

possibly extract source and destination IP addresses. For this to work, the

ingress and egress PE devices have to agree on the usage of the control

word.

The third improvement relates to multiple flows going over a single

pseudowire. In this case, there may be one or more transport labels and a

pseudowire label present (refer to Figure 1-13). Transit routers may not be

able to inspect the Layer 2 information to distinguish the different flows.

Instead, the P nodes compute the same hash for all flows, preventing proper

load balancing. This problem description may sound familiar to the GRE

use case introduced earlier. It can be solved by using the Entropy label as

well.

There is yet another L2VPN-exclusive solution to this problem. RFC 6391

introduces an additional label in the MPLS label stack called the Flow label.

The Flow label is imposed by the ingress PE node based on the relevant

fields of the flow, which for an L2VPN could be source/destination

addresses of Layer 2 and Layer 3 (if present). It is important to note that all

packets belonging to the same flow must be transported using the same

Flow label to guarantee that the same path is taken across the network for

all packets belonging to the same flow. For this to work, ingress and egress

PE devices have to agree on the usage of the Flow label.

The Entropy label is applicable to both L2VPN and L3VPN services

and must be supported on all nodes in the path, whereas the Flow

label is limited to L2VPN services but requires support on the PE

nodes only.

#### Flow-Aware Transport pseudowire (FAT PW / Flow label) (RFC 6391)

P routers forward packets based on the topmost label in the MPLS label stack, which is the transport label (also called the tunnel label). This label identifies the Label Switched Path (LSP) to the egress Provider Edge (PE) router.

When multiple Equal Cost Multipath (ECMP) paths or Link Aggregation Group (LAG) members are available, P routers use a hashing algorithm to select a specific path or link. The hash is computed over a set of packet fields or labels to distribute traffic across available paths

P routers hash based on the entire label stack (transport and VC labels) or, in some implementations, only the bottom-most label (VC label). Since the VC label is the same for all packets in a pseudowire, all traffic for that pseudowire often takes the same path, leading to poor load balancing.

The flow, in this context, refers to a sequence of packets that have the same source and destination pair.

![](<../.gitbook/assets/Unknown image (1415)>)

The FAT label (flow label) is introduced to improve load balancing by adding a bottom-most label that varies per flow within the pseudowire, providing greater entropy (variability) for the hashing algorithm.

_Physics / Thermodynamics_

_Entropy measures the disorder of a system._

_In a closed system, entropy naturally increases (Second Law of Thermodynamics)._

_Example: Ice melts into water → molecules become less ordered → entropy increases._

_Information theory (Claude Shannon)_

_Entropy is the average amount of information (or uncertainty) in a message source._

_Higher entropy means less predictability._

_Networking / Cryptography_

_Entropy refers to the randomness used for secure key generation or hashing._

_In MPLS or load balancing, entropy labels are used to introduce variation in packet headers so that hashing functions can better distribute traffic across multiple equal-cost paths (ECMP)._

Flow Aware Transport (RFC6391) enables the ingress PE device to push an extra MPLS label (called the flow label) onto the bottom of the stack for a L2 VPN which is used as an entropy label by P nodes to hash on. (The FAT PW configuration enables the flow label)

The PE router will have visibility into the payload of the MPLS VPN so the flow label is based upon the flow details of the L2 VPN payload - IP+payload. This means the PE will consistently push the same FAT label for the same flow and prevent packet reordering in the core when ECMP occurs.

**Transport Label:** Identifies the LSP to the egress PE.

**VC Label:** Identifies the pseudowire.

**Flow Label:** Identifies a specific flow within the pseudowire, generated based on flow characteristics (e.g., source/destination IP or MAC addresses).

Transport Label: 1000

VC Label: 24000

Flow Label: 50001 (for Flow A) or 50002 (for Flow B)

The flow label is marked with the End of Stack (EOS) bit = 1 and TTL = 0, indicating it’s the bottom-most label and not used for forwarding.

P routers continue to perform forwarding based on the top Transport Label. However, when selecting an ECMP path or LAG member, the P router's hashing algorithm now includes the variable Flow Label. Since each flow has a different Flow Label, the traffic is efficiently distributed across all available paths

The egress PE discards the flow label such that no decisions are taken based on that label.

{% hint style="info" %}
The flow label is never signaled between PE routers. The value is generated locally and pushed at the bottom of the stack. Only the _presence_ is negotiated (sub-TLV `0x17`), not the exact label value.
{% endhint %}

For load balancing to work based on a flow-label configuration, a version of LDP that supports signaling extensions to use the flow label with pseudowires must be enabled on all routers. The LDP-signaling configuration is identical for VPLS and VPWS pseudowires.

As per the RFC, the flow label capable PE router must signal the capability using a new Pseudowire Interface Parameter TLV called the Flow Label sub-TLV (FL sub-TLV) in the LDP (more specifically targeted-LDP). Figure 2 shows the structure of FL sub-TLV.

Type is set to 0x17 for FL sub-TLV as assigned by IANA.

Length is set to 4 bytes for FL sub-TLV.

T is set to 1 if a PE router wishes to send the flow label, otherwise, it is set to 0.

R is set to 1 if a PE router is willing to receive the flow label, otherwise, it is set to 0.

Reserved is set to 0.

It is recommended that both ingress and egress PE must agree on the support of flow label, hence ideally, both T and R should be set to 1.

Since the FAT label is only used for Pseudowires, it's tied directly to the PW setup. The capability is negotiated between the PE routers using the Flow Label sub-TLV (T and R bits) during the LDP PW signaling

Packet encapsulation with Flow label:

S-bit = Bottom of the stack

![](<../.gitbook/assets/Unknown image (1416)>)

| IOS-XE# pseudowire-class EoMPLS encapsulation mpls flow-label enable ! Enables flow-label support on Pseudowire load-balance flow ! Enables load balancing on ECMPs | IOS-XR-Example# interface GigabitEthernet0/0/0/20.100 l2transport encapsulation dot1q 100 rewrite ingress tag pop 1 symmetric mtu 1518 ! l2vpn router-id 1.2.3.4 pw-class PW-ETH-CW-FAT encapsulation mpls load-balancing flow-label both ! xconnect group CEx p2p PW interface GigabitEthernet0/0/0/20.100 neighbor ipv4 5.6.7.8 pw-id 2210 pw-class PW-ETH-CW-FAT |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### Entropy label (EL) + Entropy Label Indicator (ELI) (RFC 6790)

addresses the same core problem as the FAT label: poor load balancing when routers hash based on a limited set of MPLS labels, which lack sufficient variability (entropy) to distribute traffic evenly. Unlike the FAT label, which is specific to L2VPN pseudowires, the entropy label is a general-purpose solution applicable to various MPLS applications, including L2VPN, L3VPN, and IP traffic over MPLS

The entropy label introduces two key components:

**Entropy Label Indicator (ELI):** A reserved MPLS label (value 7) that signals the presence of an entropy label in the stack.

**Entropy Label (EL):** A label that carries a value derived from packet flow characteristics (e.g., source/destination IP addresses, ports, or MAC addresses), providing high entropy for load balancing.

The entropy label is inserted into the MPLS label stack by the ingress Label Edge Router (LER) and used by transit Label Switch Routers (LSRs, equivalent to P routers) for load balancing

Depending on the implementation, the LSRs (Label Switching Router or P) will use the Entropy Label for hashing or the entire stack label (except the ELI Label).

LDP: Uses the Entropy Label Capability TLV (IANA type 0x0206) to signal whether an LER or LSR supports entropy labels. Both ingress and egress LERs must agree to use entropy labels.

BGP: For BGP-signaled VPNs (e.g., L3VPN, EVPN), entropy label capability is signaled via extended communities or attributes.

{% hint style="info" %}
The entropy label value is never signaled between PE routers. It is generated locally and pushed at the bottom of the stack. The ELI is signaled end-to-end.
{% endhint %}

[https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/mpls/b-mpls/m\_mp-ldp-entropy-label-support.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/mpls/b-mpls/m_mp-ldp-entropy-label-support.html)

#### Operation

1.

The ingress LER (PE router) encapsulates customer traffic into MPLS packets, assigns labels, and forwards them into the MPLS network.

It analyzes packet headers to identify flows (e.g., 5-tuple for IP: source/destination IP, protocol, source/destination ports; or MAC addresses for Layer 2).

Computes a unique entropy label value using a hash function based on flow characteristics. The value is typically a 20-bit label (non-reserved, >15) to ensure high variability.

Inserts the entropy label into the stack, preceded by the ELI:

Transport Label: For the Label Switched Path (LSP).

Service Label: For the VPN or pseudowire (e.g., L3VPN label, L2VPN VC label).

ELI (Label 7): Indicates the next label is an entropy label.

Entropy Label: Carries the flow-specific value.

Signals entropy label capability using the LDP Entropy Label Capability TLV (for LDP-signaled LSPs) or BGP attributes (for BGP-signaled VPNs). The ingress LER only inserts the entropy label if the egress LER signals support (via the R bit).

Forwards the packet to the first LSR.

Example: For an L3VPN packet, the ingress LER might push:

Transport Label: 1000

VPN Label: 20000

ELI: 7

Entropy Label: 60001 (hashed from IP 5-tuple)

2.

Transit routers that forward MPLS packets based on the transport label and perform load balancing across ECMP paths or LAGs.

Uses the topmost transport label to determine the next hop.

Swaps the transport label but leaves the ELI and entropy label unchanged.

3.

Egress Label Edge Router (LER) Recognizes the ELI (label 7) and pops both the ELI and entropy label. The entropy label’s TTL is typically 0, ensuring it’s not used for forwarding.

Processes the service label (e.g., VPN or VC label) to forward the packet to the correct CE.

Signals its ability to receive entropy labels (R bit = 1) via LDP or BGP.

![](<../.gitbook/assets/Unknown image (1417)>)

#### Ethernet control word: operational notes

Added between label(s) and the Layer 2 PDU

The primary purpose of the control word was to carry signaling and status information (reassembly of fragmented packets) between the two PE routers that terminate the pseudowire for older L2 protocols like ATM or Frame relay

The control word is optional and must be explicitly configured – however it is enabled by default on cisco devices

Cisco platforms by default attempts to load balance packets with the use of the IPv4/IPv6 header information encapsulated in the payload – for non-IP traffic this behavior is undesirable, and the control word can work like padding and thus shifting the field version (in IP header) by 32bits - which prevents IPv4/IPv6-like load balancing

![](<../.gitbook/assets/Unknown image (1418)>)

### MPLS QoS models

#### Point-to-cloud connection

Per-VPN QoS policies at the edge.

Same MPLS QoS policies for packets of all VPNs in the core.

QoS can be implemented with point-to-network guarantees.

QoS can also be implemented with point-to-point.

![](<../.gitbook/assets/Unknown image (1419)>)

#### Point-to-point connection

For the more stringent applications, where the customer desires a point-to-point guarantee, a virtual data pipe needs to be constructed to deliver the highly critical traffic.

DS-TE is required to offer hard point-to-point guarantees.

![](<../.gitbook/assets/Unknown image (1420)>)

### DiffServ over MPLS (QoS)

is an extension to unicast IP routing that provides differentiated services. Extensions to LDP are used to propagate different labels for different classes. The FEC is a combination of a destination network and a class of service

Differentiated QoS is achieved by using MPLS experimental bits or by creating separate LSP tunnels for different classes. Extensions to LDP are used to create multiple LSP tunnels for the same destination (one for each class)

Setting IP precedence on the edge

The 3 most significant bits from the DSCP bits in the IP header are inserted to the Traffic class section in the MPLS packet header (previously called EXP)

WRED precedence and CB-WFQ class within the core

Modular QoS CLI can classify labeled packets based on their MPLS experimental bits

![](<../.gitbook/assets/Unknown image (1421)>)

When an MPLS packet is in transit, it carries two (or more) QoS markings:

IP QoS marking (DSCP – Differentiated Services Code Point) in the original IP header

MPLS QoS marking (EXP bits) in the MPLS label

The default behavior of the DSCP MPLS EXP bits as a packet travels from one CE router to another CE router across an MPLS core is as follows:

**Imposition of the label (IP to label):**

The IP precedence of the incoming IP packet is copied to the MPLS EXP bits of all pushed label(s).

The first three bits of the DSCP bit are copied to the MPLS EXP bits of all pushed labels.

This technique is also known as ToS reflection.

**MPLS forwarding (label to label):**

The EXP is copied to the new labels that are swapped and pushed during forwarding or imposition

At label imposition, the underlying labels are not modified with the value of the new label that is being added to the current label stack.

**Disposition of the label (label to IP):**

At label disposition, the EXP bits are not copied to the IP precedence or DSCP field of the newly exposed IP packet.

{% hint style="info" %}
Ingress committed rate (ICR) is the inbound rate the provider commits to treat.

Egress committed rate (ECR) is the outbound committed rate toward the customer.

As long as traffic stays within ICR/ECR, you get the intended SLA treatment.

Providers often keep their own QoS policies. They avoid rewriting customer DSCP/IP precedence.
{% endhint %}

MPLS DiffServ Tunneling is designed to support Differentiated Services (DiffServ) over MPLS networks, enabling class-based QoS (Quality of Service) treatment across MPLS Label Switched Paths (LSPs). One of the key features is:

Per-Hop Behavior (PHB) layer management, which allows the mapping of DiffServ code points (DSCP) into MPLS EXP bits, ensuring consistent QoS treatment as packets traverse the MPLS backbone

#### Pipe mode

The service provider uses its own EXP values for QoS classification and queuing inside the MPLS core, including on the PE-CE (egress) link.

However, subscriber DSCP/ToS values remain unchanged across the entire MPLS domain.

The original DSCP is preserved and delivered intact to the destination CE.

This allows QoS transparency for customer traffic.

Defined in RFC 3270 as a mandatory model for MPLS networks supporting DiffServ.

Similar to short-pipe mode in that customer and provider are in different DiffServ domains, but:

Egress PE applies provider QoS policy for queuing and scheduling (e.g., WRED or WFQ) based on EXP, before label pop.

Reduces operational complexity by avoiding per-customer QoS config on egress PE.

#### Short-pipe mode

The provider modifies EXP values inside the core for internal QoS, just like in pipe mode.

At the egress PE-CE link, the original DSCP/ToS values are preserved and restored.

Ensures transparency of customer QoS markings at the CE.

The key difference: the PHB (Per-Hop Behavior) is inferred after label pop, using the inner DSCP instead of relying on EXP bits.

Suitable for networks using PHP (Penultimate Hop Popping) or non-PHP.

Enables separation of provider and customer QoS domains, with accurate QoS enforcement at the edge

In Short-pipe mode, the Egress PE is required to classify traffic for the final outbound link based on the inner IP DSCP (the customer's marking). This inherently requires the Egress PE to know:

a. Who the customer is (the VPN).

b. What QoS policy that specific customer is allowed to use.

c. The classification rules based on the customer's DSCP.

This requires per-customer (or per-VPN) QoS configuration on the Egress PE, increasing complexity.

#### Uniform mode

The service provider modifies EXP values inside the MPLS core.

These EXP values are propagated back into the IP header at the egress PE (when labels are removed).

This alters the original DSCP/ToS, so the subscriber must restore original markings on the CE.

Used when customer and provider are in the same DiffServ domain.

#### Classification based on QoS group

QoS group is the internal label used by the router or switch to identify packets as a member of specific class.

This label is not part of the packet header and is local to the router or switch.

The label provides a way to tag a packet for subsequent QoS action.

The QoS group label is identified at ingress and used at egress.

QoS groups can be used to aggregate multiple input streams across input classes and policy maps to have the same QoS treatment on the egress port.

Assign the same QoS group number in the input policy map to all streams that require the same egress treatment, and match the QoS group number in the output policy map to specify the required queuing and scheduling actions.

Class map:

| class-map acl match access-group name acl exit | (Cisco IOS and IOS XE configuration is similar) |
| ---------------------------------------------- | ----------------------------------------------- |

Input policy map:

| policy-map set-qos-group class acl set qos-group 5 exit | The set qos-group command is used only in an input policy. The assigned QoS group identification is then used in an output policy with no mark or change to the packet. The command match qos-group is used in the output policy. The command match qos-group cannot be used for an input policy map. |
| ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Output policy map:

| policy-map shape class qos-group 5 shape average 10 mbps exit |                                                                                                                                                                                                |
| ------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show policy-map interface                                     | Congestion is observed through matched, transmitted, and dropped packets. Queuing and congestion avoidance mechanisms prevent high priority packets from being dropped when congestion occurs. |

Configuring on PE example

![](<../.gitbook/assets/Unknown image (1422)>)

The P configuration example - the P router then can match the EXP bit set by the PE router and adjust the QoS accordingly

![](<../.gitbook/assets/Unknown image (1423)>)

| ## Modifying the MPLS imposition/topmost label Router(config)# policy-map \[NAME] Router(config-pmap)# class \[CLASS-MAP-NAME] Router(config-pmap-c)# set mpls experimental \[imposition \| topmost] | topmost keyword only affects the current topmost MPLS label at the current point of the interface (ingress/egress) imposition keyword affects all MPLS label at the current point of the interface (ingress/egress) |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
