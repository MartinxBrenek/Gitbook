# PON

#### Passive Optical Network (PON)

**Passive Optical Network (PON)** is an cost-effective fiber-optic technology for delivering broadband network access to end-customers or as an alternative to Ethernet switching in enterprise networking

PONs are widely used in telecommunications as a means to implement fiber to the x (FTTX) solutions, including fiber to the home (FTTH), fiber to the building (FTTB), and others

Its architecture implements a point-to-multipoint topology in which a single mode optical fiber serves multiple endpoints by using unpowered (passive) fiber optic splitters to divide the fiber bandwidth among multiple subscribers, making it very scalable and cost-effective. It is deployed in a last mile between an ISP and its customers.

The primary advantages of PONs include their cost-effectiveness, scalability, and lower energy consumption compared to active networks. The lack of active components in the field reduces maintenance costs and increases reliability

### Active Optical Network (AON)

An Active Optical Network (AON) uses powered devices like switches to manage the flow of data between the network and its users. Unlike a Passive Optical Network (PON), which relies on non-powered splitters in a point-to-multipoint topology, AON operates on a point-to-point topology where each user gets a dedicated connection. This allows AON to optimize bandwidth by directing data specifically to the intended recipient, reducing congestion and increasing efficiency. The active devices in AON provide better control and network intelligence, enabling dynamic bandwidth allocation and easier troubleshooting. However, these devices require electricity and cooling, often needing placement in specialized telecom closets, which can complicate deployment and increase costs compared to PON’s simpler infrastructure

![](<../.gitbook/assets/Unknown image (800)>)

![Possive Optical Network](<../.gitbook/assets/Unknown image (801)>)

### Optical Distribution Network (ODN)

**Optical Distribution Network (ODN)** is the last mile network composed of passive optical components, such as optical fibers, and passive optical splitters and amplifiers. It provides optical channels between the OLT and ONTs.

Architectures like PON falls into ODN

![](<../.gitbook/assets/Unknown image (802)>)

ODN is the important part of the PON network which provides optical transmission between

OLTs and ONU/ONTs

Ideal distance is 20 km or sometimes more

Feeder optical fibers, optical distribution point, distribution fibers, optical access point, and drop fibers

form integral parts of ODN

Feeder fibre usually connects the central office to the distribution points

The distribution fibre extends the distribution points to access points

The drop fibers connect the individual ONTs from the access points

ODN is the very path essential to PON data transmission and its quality directly affects the performance,

reliability, and scalability of the PON system

### PON types

Asynchronous Transfer Mode passive optical network (APON) contains an electrical layer built on ATM

Broadband PON (BPON) is the improved successor of APON and maintains the ATM structure. BPON has a transmission rate of up to 622 Mb/s, with downstream capabilities of 155 Mb/s to 622 Mb/s. BPON can be converted to EPON or GPON over time, as ATM bandwidth is not ideal for video.

Ethernet PON (EPON) uses ethernet packets rather than the ATM cells used in APON and BPON. EPON is popular for its 1 Gb/s bandwidth for modern networks.

Gigabit PON (GPON) provides a very high bandwidth of 2.5 Gb/s. GPON uses IP and ATM or GPON encapsulation method (GEM) for encoding.

XG-PON (10G-PON or XG-PON1) Offers enhanced data rates up to 10 Gbps downstream and 2.5 Gbps upstream, providing higher bandwidth for demanding applications such as ultra-high-definition video streaming and cloud services.

XGS-PON (10G-EPON or XG-PON2) Extends the capabilities of XG-PON by supporting symmetrical 10 Gbps data rates both downstream and upstream, enabling symmetric gigabit services for businesses and residential users. XGS PON stands for 10 Gigabit Symmetrical PON

### PON components

![](<../.gitbook/assets/Unknown image (803)>)

#### Optical Line Terminal (OLT) (PE)

**Optical Line Terminal (OLT)** is a device that is placed at the head end of the ISP network

A single fiber optic cable runs from the OLT to a nonpowered (passive) optical beam splitter, which multiplies the signal and relays it to many optical network terminals (ONTs). End-user devices such as PCs and telephones are connected to the ONTs.

Since the splitting function is a one-to-many broadcast of the same data stream, the ONTs are responsible for filtering packets meant for the various connected endpoint devices. Encryption ensures that each ONT reads only the contents addressed to the endpoints connected to it.

The OLT can be either switch like it is employed in Cisco Catalyst PON or as an service provider OLT chassis:

![](<../.gitbook/assets/Unknown image (804)>)

**OMCI (ONT Management and Control Interface)**

OMCI (ONT Management and Control Interface) is a standardized protocol used in Passive Optical Networks (PON) to facilitate communication between the Optical Line Terminal (OLT) and the Optical Network Unit (ONU)/Optical Network Terminal (ONT). It is defined in the ITU-T G.988 recommendation.

OMCI is responsible for provisioning, managing, and monitoring the ONU.

ONU Configuration & Service Provisioning – Setting up VLANs, Quality of Service (QoS), GEM ports, and multicast services.

Performance Monitoring – Collecting statistics on traffic, signal levels, and error rates.

Fault Management – Detecting and reporting failures like signal loss, hardware malfunctions, or link degradation.

Firmware Upgrades & ONU Software Management – Remotely updating ONU firmware to ensure compatibility and security.

Security Management – Handling authentication and encryption mechanisms for secure communication.

OMCI messages are encapsulated within G-PON Encapsulation Method (GEM) frames and transmitted over the PON link.

The OLT sends OMCI commands to configure or request status from the ONU.

The ONU responds with acknowledgments or requested information.

Enables centralized management of multiple ONUs from the OLT.

Supports multi-vendor interoperability since it is standardized under ITU-T G.988.

Reduces the need for on-site configuration by allowing remote provisioning and troubleshooting.

#### Passive Optical Splitter (POS)

Optical splitters take a single light source (a single fiber optic strand) and refract and duplicate it multiple times to "outbound" fibers. In its simplest form, an optical beam splitter splits a light source in two by using two back-to-back prisms.

Typical gigabit-capable passive optical network (GPON) deployments have used a splitting ratio of 1:32 or 1:64. Current GPON standards specify up to 128 splits on a single GPON port. Those same standards set the distance between active devices at 20 kilometers

POS is a passive networking component, meaning it require no power, no climate control, and no maintenance whatsoever

Because PON uses the same strand of fiber to send and receive data, the passive optical splitter also acts as an optical combiner receiving data traffic from the same connected end devices. To achieve this, PON takes advantage of two distinct types of long-established telephony multiplexing concepts: wavelength division and time division.

**Wavelength-division multiplexing (WDM)** allows bidirectional traffic across a single fiber by using a different wavelength for each direction of traffic: the 1490-nanometer (nm) wavelength for downstream traffic and the 1310-nm wavelength for upstream traffic. The 1550-nm wavelength is reserved for optional overlay services, typically RF (analog) video.

Future iterations of the PON standard will define separate wavelengths for backward compatibility.

**Time-division multiplexing (TDM)** allows multiple end devices to transmit and receive independent signals across a single fiber by reserving time slots in a stream of data. PON uses two such technologies: TDM for downstream traffic and **time-division multiple access (TDMA)** for upstream traffic.

Downstream (OLT → ONUs):

Uses TDM: The OLT broadcasts data to all ONUs, but each ONU reads only its own time-slotted data.

All ONUs share the same wavelength (1490 nm or 1577 nm for XGS-PON) and time is divided into slots for signal multiplexing.

Upstream (ONUs → OLT):

Uses TDMA: Each ONU is assigned specific time slots to transmit upstream to avoid collisions.

The OLT coordinates these time slots using a Dynamic Bandwidth Allocation (DBA) algorithm.

TDM is about combining signals on a shared medium.

TDMA is about allowing multiple users to access the medium without interference.

As a passive device, the splitter acts as distribution point, with the single feed of downstream data broadcast to all connected ONT endpoints. The ONT accepts packets assigned to its TDM channel (frame time slot). It filters and discards packets meant for other ONTs.

TDMA enables multiple transmitters to be connected to one receiver. For PON, TDMA is used to recombine the multiple upstream feeds at the coupler. A splitter and a coupler are often found in one device.

Single-mode fiber is used

![Fiber Optic PLC Splitter SC/PC - 8x SC/PC](<../.gitbook/assets/Unknown image (805)>)

#### Optical Network Terminals (ONTs) (CE)

ONT is ITU-T terminology

ONU is IEEE terminology

Both refer to the user equipment in the PON system

They are generally on customer premises

is a simple switch or AP placed and powered at the customer premise that converts the optical signals from the fiber into the electrical signals, to be processed by the electiricity-powered circuit and send over metallic cable or wirelessly, if the ONT has Wireless capabilities

As a passive device, the splitter acts as distribution point, with the single feed of downstream data broadcast to all connected ONT endpoints

The ONT accepts packets assigned to its TDM channel (frame time slot). It filters and discards packets meant for other ONTs.

![Cisco Catalyst PON Series - Cisco](<../.gitbook/assets/Unknown image (806)>)

### Cisco Routed PON (RPON)

Limitations of Traditional PON

The traditional setup requires separate multiple OLTs, ONTs, and BNG/vBNGs, increasing hardware complexity and management overhead for service providers.

Adding new users might involve deploying additional hardware at both the OLT location and potentially the BNG location, making it less scalable

RPON addresses these limitations by collapsing the functionality of the OLT into a pluggable SFP+ form factor

This will make PON network a direct part of the access layer.

![](<../.gitbook/assets/Unknown image (807)>)

![](<../.gitbook/assets/Unknown image (808)>)

Notes

If specific features are required the customer must let us know, so that we can coordinate with the vendor to create proper service profiles and downstream QoS map that can be understood by the vendor's ONU

The failover from the singleVM to the distributed VM will be possibly supported in the next release

upgrade the routed pon to 25.2.1

cfg file pointing to the tftp server doesn't work even after specifying vrf in the tftp client configuration - Alex will follow up with the engineering team

kdyz pridam tag do policka pro "s" - servisni vlana, tak mi to nefunguje - zminit ze je to neco co se da s cisco engineering tymem doplnit podle potreb implementace

Routed PON reseni je nejoptimalnejsi pro SP, kteri maji optickou infrastrukturu zakonecnou centralizovane, proto aby se daly NCS hostujici OLT do stejnych prostoru a nahradily stavajici OLTcka

Na meetingu s Vantivou a KAONEM Zeptat se jestli zapinaji wifi a nastavujou vic veci na ONTcku z OLTcka, nebo to nechavaji na koncovem zakaznikovy ktery ma moznost se lognout do GUI

Cisco RPON deep dive:

<\<PON\_technical\_deep\_dive0524.pptx>>

<\<BRKSP-1169.pdf>>

XGS-PON: A shared-fiber system for broadband access, using multiple layers (Physical, TC, Path) and wavelength management to serve many users over one fiber with TDMA/TDM.

Ethernet: A point-to-point or switched system with a simpler Layer 2 frame structure, designed for LANs/WANs without shared medium scheduling or wavelength considerations.

![](<../.gitbook/assets/Unknown image (809)>)

![](<../.gitbook/assets/Unknown image (810)>)

### MTU in PON

the failure above 9176 bytes is caused by L2/PON encapsulation overhead (VLAN tag, GEM/PON headers, etc.) that reduces the effective path MTU for the IP packet. Below I show the math and how it matches your observed thresholds, then what to check.

Facts / definitions

An ICMP echo on IOS “size” for extended ping is the size of the entire IP datagram (i.e. it includes the IP header + ICMP header + ICMP data).

IP packet on the wire = IP header (20) + ICMP header (8) + ICMP payload (if IPv4, no options). The Ethernet payload carries that IP packet.

802.1Q VLAN tag = 4 bytes (affects L2 frame size). GPON/EPON (GEM) adds its own variable encapsulation overhead when mapping Ethernet into PON.

MTU configured = 9198.

Successful pings up to size 9176.

9177 (and 9180) fail.

Working the math (using Cisco ping semantics)

Using the Cisco semantics (ping size = total IP datagram length):

If size = 9176, then IP datagram length = 9,176 bytes.

That fits your path (ping succeeds).

If size = 9177, IP datagram = 9,177 bytes → fails on the path.

So the effective path MTU for IP datagrams appears to be 9,176 bytes (anything larger is dropped/fragmented and DF causes failure). That is 22 bytes less than your configured MTU of 9,198:

Edit

9198 (configured MTU)

* 9176 (max IP datagram that succeeds)

\= 22 bytes overhead

Where do those 22 bytes come from?

They come from L2 / PON encapsulation that is applied in addition to the IP datagram. Potential contributors:

802.1Q VLAN tag: 4 bytes.

Wikipedia

GPON/EPON GEM or PON encapsulation overhead: typically a few bytes per GEM header and possibly additional bytes for mapping/fragmentation — vendor/implementation dependent (GPON/EPON add small fixed headers and possible alignment padding). Practical GPON documents / vendor notes show per-frame encapsulation bytes (varying between \~4–8+ bytes depending on mode).

Any additional provider tags (Q-in-Q, service tags) or PON-specific TLV/preamble handling could add the remaining bytes.

Example decomposition that matches \~22 bytes (illustrative):

802.1Q tag = 4

GPON/GEM minimal encapsulation/padding = \~5–8

Other tags / alignment / management = \~10–13

Sum ≈ 22 (matching observed difference).

Because the encapsulation is inserted on the wire outside the IP datagram, an IP datagram of 9,198 bytes could become >9,198 on the wire once encapsulation is added — so the device downstream rejects or fragments it, explaining your observed failure at 9177/9180.

### Broadband Remote Access Server (BRAS) / Broadband Network Gateway (BNG)

**Broadband Remote Access Server (BRAS) / Broadband Network Gateway (BNG)** is a broadband service provider component at the edge of the SP network that aggregates and authenticates subscriber sessions and routes traffic between the access network and the ISP's backbone (core) network.

BRAS is deployed by the service provider and is present at the first aggregation point in the network, such as the edge router.

It established and manages subscriber sessions coming in from access networks such as DSL, fiber, or wireless. It be be also virtualized - Virtual BNG (vBNG)

<\<BNG\_PPOE.pdf>>

Main Functions of BRAS:

User Authentication & Authorization:

Communicates with RADIUS or AAA servers.

Verifies user credentials (usually via PPPoE, PPPoA, or DHCP).

IP Address Management:

Assigns IP addresses to clients dynamically (via DHCP or IPCP).

Interacting with the DHCP server to provide IP address to clients.

Policy Enforcement:

Applies Quality of Service (QoS), bandwidth profiles, and filtering rules per user.

Traffic Aggregation:

Collects and aggregates traffic from multiple DSLAMs (DSL Access Multiplexers) or access nodes.

Routing & Forwarding:

Directs user traffic into the ISP’s IP backbone.

Acts as a Layer 3 gateway for subscriber traffic.

Accounting:

Collects usage data for billing, auditing, or network planning.

![](<../.gitbook/assets/Unknown image (811)>)

#### Establishing subscriber sessions

• Each subscriber (or more specifically, an application running on the CPE) connects to the network by a logical session. Based on the protocol used, subscriber sessions are classified into two types:

PPPoE subscriber session: The PPP over

Ethernet (PPPoE) subscriber session is established using the point-to-point(PPP) protocol that runs between the CPE and BNG.

IPoE subscriber session: The IP over Ethernet (IPoE) subscriber session is established using IP protocol that runs between the CPE and BNG; IP addressing is done using the DHCP protocol.

PPPoE was designed for managing how data is

transmitted over Ethernet networks, and it allows a single

server connection to be divided between multiple clients,

using Ethernet. As a result, multiple clients in shared

network can connect to the same server from the Internet

Service Provider and get access to the internet, at the

same time, in parallel. To simplify, PPPoE is a modern

version of the old dial-up connections, which were popular

in the 80s and the 90s.

• P2P protocol over ethernet encapsulating PPP frames in

Ethernet frames (Src MAC, Dst MAC).

• Old days used mainly with ADSL services ( most common

PPPOE over ATM)

• Offers standard PPP features such as authentication,

encryption, and compression

• PPPoE has two distinct stages as defined in RFC 2516:

* Discovery stage
* PPP session stage

• IPoE is essentially DHCP-triggered subscriber interfaces.

• Users are "authenticated" through the use of DHCPv4/v6

Option-82 inserting their Circuit-ID into their initial DHCP

Discovery - this identifies the physical location of the user based

on the tail that they are connected to (this would be done at an

aggregation switch between the xPON network and whatever

backhaul gets them to their ISP of choice).

• The ISP will then service the DHCP request (if the Circuit-ID can

be mapped to a valid user via RADIUS), provide an IP (and

hopefully prefix-delegation if they're offering IPv6) and then

create a logical interface representing that subscriber that you

they apply their filtering/rate-shaping to and start grabbing stats

from.

• Session lifecycle based on DHCP Lease Tracking and Split Lease

• Authentication methods

* DHCP Option82
* DHCP Option 60

-Vlan Encap

BNG relies on an external Remote Authentication Dial-In User Service (RADIUS)

server to provide subscriber Authentication, Authorization, and Accounting (AAA)

functions. During the AAA process, BNG uses RADIUS to:

•authenticate a subscriber before establishing a subscriber session

•authorize the subscriber to access specific network services or resources

•track usage of broadband services for accounting or billing

• The RADIUS server contains a complete database of all subscribers of a service

provider, and provides subscriber data updates to the BNG in the form of attributes

within RADIUS messages. BNG, on the other hand, provides session usage

(accounting) information to the RADIUS server.

• BNG supports connections with more than one RADIUS server to have fail over

redundancy in the AAA process. For example, if RADIUS server A is active, then BNG

directs all messages to the RADIUS server A. If the communication with RADIUS

server A is lost, BNG redirects all messages to RADIUS server B.

• During interactions between the BNG and RADIUS servers, BNG performs load

balancing in a round-robin manner. During the load balancing process, BNG sends

AAA processing requests to RADIUS server A only if it has the bandwidth to do the

processing. Else, the request is send to RADIUS server B.

#### Cloud Native Broadband Network Gateway

**Cloud Native Broadband Gateway (cnBNG)** represents a fundamental shift in how providers build converged access networks by separating the subscriber BNG control-plane functions from user-plane functions. **CUPS (Control/User-Plane Separation)** allows the use of scale-out x86 compute for subscriber control-plane functions, allowing providers to place these network functions at an optimal place in the network, and also allows simplification of user-plane elements. This simplification enables providers to distribute user-plane elements closer to end users, optimizing traffic efficiency to and from subscribers.

**cnBNG control plane**

The cloud native BNG control plane is a highly resilient scale out architecture. Traditional physical BNGs embedded in router software often scale poorly, require complex HA mechnaisms for resiliency, and are relatively painful to upgrade. Moving these network functions to a modern Kubernetes based cloud-native infrastructure reduces operator complexity providing native scale-out capacity growth, in-service software upgrades, and faster feature delivery.

**cnBNG user plane**

The cnBNG user plane in Agile Metro 1.1 is provided by Cisco ASR 9000 routers. The routers are responsible for terminating subscriber sessions (IPoE/PPPoE), communicating with the cnBNG control plane for user authentication and policy, applying subscriber policy elements such as QoS and security policies, and performs subscriber routing. Fixed and modular platforms are supported
