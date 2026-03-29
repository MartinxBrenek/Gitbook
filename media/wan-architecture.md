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

# WAN Architecture

#### Wide area network (WAN)

**Wide area network (WAN)** interconnects LANs or MANs that can represent Cities, Regions or countries into large network called internet

WAN is typically managed by large Internet service providers (ISP's), that provides the means to connect all customers in the region they manage with other ISP's to form an Internet

WAN primarily differs from the LAN in terms of the distance, where the signal must be preserved over longer distances, thus the mediums must be crafted accordingly. However nowadays the WAN technologies and mediums are so advanced, that it has bandwidth and transmission speeds equal to the LAN technologies over shorter distances - this wasn't true in older WAN transport mediums

#### Metropolitan area network (MAN)

**Metropolitan Area Network (MAN)** connects network devices over a larger geographical area than a LAN but smaller than a WAN

MAN links are commonly used to interconnect various sites within a city or region, providing faster and more reliable communication compared to wide area networks

A MAN link could use various technologies, including Metro Ethernet, leased lines, fiber optics, or even wireless connections

![](<../.gitbook/assets/Unknown image (242)>)

![](<../.gitbook/assets/Unknown image (243)>)

### Switching models

#### Circuit-switched networks

These networks transmit frames at regular intervals, with each frame marked with a different time slot. Traffic from individual clients is mapped onto specific time slots

And since each port and time slots are mapped/dedicated/fixed for each customer, the time slots keep sending empty frames if one of the customer's doesn't send any traffic at any given moment , which wastes the bandwidth as It could have been used for other customers

Dedicated communication path is established between two endpoints before the transmission of data begins, and resources, such as bandwidth, are reserved for the duration of the communication session, regardless of whether data is being transmitted or not

**Dial-on-Demand Routing (DDR)** is a Cisco routing technique that initiates and closes circuit-switched data sessions on an automated and as-needed basis.

DDR uses external terminal adapters that enable wide area network (WAN) routing connections via (PSTN) or (ISDN)

![Circuit Switching vs. Packet Switching | by Okan Özşahin | Medium](<../.gitbook/assets/Unknown image (244)>)

![](<../.gitbook/assets/Unknown image (245)>)

#### Packet-switched networks

data is broken into packets, and sent dynamically across the network - using OSI model.

Each packet is labeled with an IP address and sequencing information, so that not entire time slot is reserved for one customer, remaining available bandwidth to be used, which improves flexibility and efficiency. So single flow from one customer can utilize the entire bandwidth of the link at any given time, (in case that no other customer is sending traffic)

![](<../.gitbook/assets/Unknown image (246)>)

### WAN access technologies

![](<../.gitbook/assets/Unknown image (247)>)

#### Public switched telephone network (PSTN)

**Public switched telephone network (PSTN)** is the traditional analog telephone network that provides basic voice communication services over copper wires

PSTN supports voice calls, fax, and low-speed data transmission using analog signals

#### Integrated services digital network (ISDN)

**Integrated services digital network (ISDN)** is a set of communication standards for simultaneous digital transmission of voice, video, data, and other network services over the PSTN

#### Frame Relay

was a WAN packet switched technology, that utilized virtual circuits for logical connections. Replaced by newer WAN technologies like MPLS and broadband internet

Instead of using a physical ‘circuit’, Frame Relay uses ‘virtual circuits’ which may be Permanent Virtual Circuits (PVCs) or Switched Virtual Circuits (SVCs)

#### Asynchronous Transfer Mode (ATM)

ATM was a WAN technology used in 1990s. ATM breaks data into small, fixed-length cells, allowing for more efficient switching and transmission compared to variable-length packets used in technologies like IP. ATM established virtual circuits between endpoints, similar to circuit-switched networks

This allowed for guaranteed quality of service (QoS) and predictable performance for real-time applications like voice and video

ATM eventually declined in popularity as IP-based technologies, such as Ethernet and MPLS, became more dominant for wide-area networking

**Circuit emulation service (CES)** is typically used to transfer voice or video traffic across an ATM network by employing virtual circuits

#### Digital Subscriber Line (DSL)

**Digital Subscriber Line (DSL)** is a broadband technology that uses existing copper telephone lines to deliver high-speed internet access and digital communication services over existing PSTN

It uses a different frequency than the telephone, so that you can use the internet while making a call

DSL uses RJ11 connectors for connecting modems or routers to telephone lines. They look similar to RJ-45, but are smaller with fewer pins - 4 or 6

![](<../.gitbook/assets/Unknown image (248)>)

![](<../.gitbook/assets/Unknown image (249)>)

DSL operates on the same physical infrastructure as PSTN and ISDN but utilizes digital modulation techniques to achieve higher data transfer rates

DSL is an older concept that provides a typical speed of around 6mbps. The good thing in DSL is the bandwidth is not shared and provides a constant speed

DSL offers ADSL (Asymmetric DSL) and SDSL (Symmetric DSL), providing different upload and download speeds to meet various user requirements

ADSL:Download: Up to 8 Mbps, Upload: Up to 1 Mbps

ADSL2: Download: Up to 24 Mbps, Upload: Up to 2 Mbps

ADSL2+: Download: Up to 100 Mbps, Upload: Up to 10 Mbps

VDSL2 (Very high-speed Digital Subscriber Line 2): Download: Up to 200 Mbps, Upload: Up to 50 Mbps

VDSL2: Download: Up to 300 Mbps, Upload: Up to 50 Mbps

G.SHDSL (Single-pair High-speed Digital Subscriber Line) is a digital subscriber line technology that enables high-speed data transmission over a single twisted pair of copper wires

Digital Subscriber Line Access Multiplexer

is a networking device that bridge the gap between traditional copper telephone lines and the high-speed internet backbone

Annexes define specific parameters such as frequency bands, modulation techniques, and line-coding methods tailored to different regional standards or requirements

Annex A: Used mainly in North America and parts of Asia, it specifies parameters optimized for these regions' telephone networks.

Annex B: Commonly used in Europe and some other regions, it defines parameters suited for European telephone networks.

Annex C: Primarily used in Japan, it specifies parameters specific to Japanese telephone networks.

![How DSL works](<../.gitbook/assets/Unknown image (250)>)

#### Serial leased line (legacy)

Serial links were older design for WAN point-to-point connections, established over telephone leased line, reaching speed up to 64 kbit/sec, 2M bit/sec, 34 Mbit/sec

In telecommunication and data transmission, serial communication is the process of sending data one bit at a time, sequentially

Supports loopback testing, allowing administrators to verify the functionality of the link by redirecting transmitted data back to the originating device for testing and troubleshooting

T1 (DS1) and T2 (DS3) and E1 and E3 are standards for serial digital telecommunications transmission - voice, data and video providing leased line, which is a P2P connection from the customer to his service provider

Leased line is a dedicated P2P link with fixed-bandwidth connection, providing reliable and secure connection, however leased lines can be expensive and not scalable as it is a permanent physical connection.

T1 and T3 lines were standard set for US. They utilize Time-Division Multiplexing (TDM) to combine multiple voice or data signals into a single transmission line T1 providing data rate of 1.544 Mbps (24 channels) and T3 44.736 Mbps (672 channels)

E1 and E3 lines were standards for Europe and other countries

They also utilize the same TDM technology. E1 provides a data rate of 2.048 Mbps (32 channels each operatig at 64Kbps) and E3 provides data rate of 34.368 Mbps (16 channels)

TDM over IP (TDMoIP) is a transport technology that is optimized for trunking T1 or E1, T3 or E3, and multiline voice and serial data across packet-switched networks

TDMoIP uses a permanent virtual circuit (PVC) concept in which a PVC is set up to carry multiple voice and data channels in the same trunk

With PVCs, the source and destination addresses are statically assigned for multiple channels and, therefore, only a single IP header is needed

G.703 specifies the parameters for transmitting voice and data signals over E1 circuits, including details such as voltage levels, line impedance, framing formats, and synchronization

Physical components

Data Terminal Equipment (DTE) device in a serial connection that receives the clocking and timing signals from the DCE

Data Circuit-terminating Equipment (DCE) device in a serial connection that provides clocking and timing signals to control the data transmission

In a typical scenario, a DCE device is responsible for generating clock signals, ensuring that data is transmitted at the correct speed and timing

The DCE device is often associated with a modem in networking setups on an ISP side.

CSU (Channel Service Unit) is responsible for maintaining the integrity and quality of the digital signal as it traverses the transmission medium. It handles tasks such as signal conditioning, line coding, and synchronization. CSUs ensure that the data transmission adheres to specific standards and can be compatible with the receiving end.

DSU (Data Service Unit) focuses on the framing and formatting of data, converting it between the network's data format and the format suitable for the serial link

It handles tasks like packetization, error detection, and flow control. DSUs enable the serial link to efficiently carry data from the connected networking devices.

Clock Rate specifies how many bits can be transmitted at a certain period = essentially configuration of the serial link speed

![](<../.gitbook/assets/Unknown image (251)>)

![](<../.gitbook/assets/Unknown image (252)>)

![DTE and DCE connector?](<../.gitbook/assets/Unknown image (253)>)

#### Metro Ethernet (Carrier Ethernet)

Ethernet was originally designed for short-range data transmission within the LAN and not for the WAN, due to the distance limitation of metallic mediums

After the invention of fiber optics it is now possible to deliver Ethernet over longer distance

Since Ethernet is integrated in most of the networking devices, it is also cheap to implement, so that is why it is nowadays used by the carriers as WAN service to connect multiple customer sites over metropolitan areas (MAN).

From the customer’s perspective, it’s like your sites are connected with a regular transparent switch

**Metro Ethernet Forum (MEF)** defines Carrier Ethernet (also called Metro Ethernet - MetroE), that can be used as a WAN transport.

#### Fiber to the X (FTTx)

Fiber to the home (FTTH): Directly brings fiber to individual residences. It offers the highest speed potential, delivering broadband services such as high-speed internet, television, and telephone.

Fiber to the building (FTTB): Fiber reaches the building (such as an apartment block or office building) and is then distributed via copper or other types of cabling.

Fiber to the curb (FTTC): Here, fiber is extended to a point near the user, often a street cabinet, and the final connection to the user's premises is made using copper wires.

Fiber to the node (FTTN): Like FTTC, but the fiber extends to a neighborhood node, and connections to individual premises are completed with copper or coaxial cables.

Dark Fiber refers to fiber connection, where only single wavelength is leveraged to transmit data (e.g not DWDM), it can also refer to unused or unlit optical fiber infrastructure that has been laid underground or installed on utility poles for the purpose of data transmission. Unlike "lit" fiber optic cables, which are actively transmitting data signals, dark fiber is essentially dormant and not currently being used by any organization or service provider for data transmission

Data Center Interconnect (DCI) facilitate the transfer of data across different data centers. This interconnectivity is essential for a range of functions, including load balancing, disaster recovery, and distributed storage systems. DCI is not just about physical connectivity; it also involves network design, virtualization technologies, and protocols that ensure efficient data transfer and synchronization.

![](<../.gitbook/assets/Unknown image (254)>)

#### SONET and SDH (legacy optical transport)

are older multiplexing standards for optical telecommunications transport developed to provide high-speed, reliable, and synchronous communication over fiber optic networks

SONET, developed in the U.S., and SDH, its international equivalent. SONET is based on time-division multiplexing (TDM).

SDH/SONET+ATM were replaced by Ethernet and now the SP core transport is built on top of IP+MPLS

This is because the Ethernet is cheaper, the packet switching like MPLS is cheaper than circuit switching

![](<../.gitbook/assets/Unknown image (255)>)

### Internet and ISP concepts

#### Internet and autonomous systems

The internet on a global scale is composed of a large number of autonomous systems. An autonomous system, also known as a routing domain, is usually a large collection of routers under a common administration. Typical examples are the internal network of a company and a network of Internet service providers (ISPs). Many entities can have multiple autonomous systems under their common management.

It is made up of a vast collection of private, public, business, academic, and government networks that are connected to each other using standardized communication protocols.

Companies and service providers that participate in Internet routing use public Autonomous System (AS) numbers to peer with their neighboring ASs.

Autonomous system (AS) is a network that is under common network administration = network devices share common routing policies = one organization

These ASs are assigned by the Internet Assigned Numbers Authority (IANA) and are used to identify and exchange routing information between different networks

Each autonomous system has the public IP address space that it controls. Each autonomous system has its neighboring autonomous systems to which it connects. To exchange autonomous system information over the internet, BGP is used to run between autonomous systems.

Each autonomous system consists of several networks interconnected with routers. The routers are crucial in the process of packet forwarding to the right destination. Inside a single autonomous system, there is usually an interior dynamic routing protocol that is running to exchange reachability information between routers.

![](<../.gitbook/assets/Unknown image (256)>)

#### Internet access models

Standard Internet Access:

Typically provided by local or regional ISPs to individual customers and businesses.

Enterprise internet connectivity is established through a provider-assigned IP address from the ISP, which can be either statically assigned for consistent public access or dynamically assigned using DHCP. Static IPs are manually configured on the router, while DHCP allows automatic IP assignment along with gateway information.

Wholesale Internet Access is a service where large network providers sell bulk internet connectivity to smaller ISPs or businesses. These providers own the infrastructure, like fiber-optic or MPLS networks, and allow others to use it for reselling or internal operations. This model is cost-effective, scalable, and ensures ISPs can offer internet services without building their own networks. Common buyers include ISPs, mobile operators, and enterprises needing dedicated bandwidth.

It is design for companies requiring granular connectivity through multiple upstream ISPs

![](<../.gitbook/assets/Unknown image (257)>)

#### Cable Internet (DOCSIS over coax)

One way to provide broadband internet connection to SOHO networks is by using cable internet from a local cable TV or ISP provider

Cable internet, often referred to as cable broadband, utilizes the same coaxial cables that are traditionally used for transmitting cable television signals

These coaxial cables are part of the cable television infrastructure and are already installed in many homes and neighborhoods.

With cable internet, the internet service provider (ISP) utilizes the existing cable television infrastructure to deliver high-speed internet access to subscribers

The coaxial cables are used as a last-mile connecting customer's modems to carry both television signals and internet data simultaneously, allowing customer's to access both services using the same connection.

This coaxial cable is extended to the nearest Point of Presence (POP) of the ISP which is then connected using Fiber connection with their high-speed core transport network inter-connecting with other ISP's to maintain Internet

Cable internet operates on a shared infrastructure, where multiple users in the same geographic area (segment) share the available bandwidth, where the ISP allocates the bandwidth for each customer based on subscription plan

Cable internet offers high-speed internet access, typically faster than traditional dial-up or DSL connections, making it a popular choice for residential and business users. It is known for its reliability and consistency, especially in areas where fiber optic internet is not yet available.

Internet users most of the time downloading data, rather than uploading, thus most ISP's provide Cable Internet with Asymmetric access, where download speed is the main service customers pay for, reaching speed up to the purchased speed, while upload speed is considerably lower

Cable Internet is "always-on", so that customers can access it anytime without the need to dial-in.

Customers may use VoIP services since they have access to the packet-swithched internet

Point of Presence (POP) refers to a location where a network service provider or telecommunications company has equipment and facilities to provide services to their customers

Last-mile refers to the physical connection, typically a cable or line, that directly connects the end user's modem or router to the service provider's network

Those are the latest coaxial cable technologies providing last mile connection

DOCSIS 3.0: This widely deployed standard supports theoretical download speeds of up to 1 Gigabits per second (Gbps).

DOCSIS 3.1: The latest iteration offers even faster theoretical download speeds of up to 10 Gbps.

![](<../.gitbook/assets/Unknown image (258)>)

Dial-up internet access was prevalent in the early days of the internet but has largely been replaced by faster broadband technologies such as DSL, cable, fiber-optic, and wireless connections. It used PSTN networks to establish a connection to an ISP by dialing a telephone number on a conventional telephone line. Dial-up connections use modems to decode audio signals into data to send to a router or computer, and to encode signals from the latter two devices to send to another modem at the ISP.

#### POP, last-mile, and demarcation

**Point of Presence (POP)** is a facility or datacenter where service providers have their networking infrastructure devices that terminate connections from their customers and connecting them to their backbone networks to provide Internet or WAN access (essentially connections to other Internet Service providers)

**Demarcation point** is a boundary that separates a service provider's network from a customer's network

It marks the end of the provider's responsibility for maintaining the network and the beginning of the customer's responsibility

Device that is on the customer's location is called CE (customer edge) and the device that sits in the ISP network and connects the CE to the WAN is called PE

**UNI (User-Network Interface)** refers to the interface or connection point between the service provider front-haul device and end-user device

**NNI (Network-Network Interface)** refers to the interface or connection point between two network domains or service provider networks

![](<../.gitbook/assets/Unknown image (259)>)

#### ISP hierarchy (Tier 1/2/3)

ISP Tier 1: The backbone of the internet. Owns global infrastructure (cables, data centers). Peers freely with other Tier 1s (no need to buy transit). Provides traffic to all other ISPs, not directly to end users. Global reach, highest connectivity. Examples: Verizon, AT\&T, NTT.

ISP Tier 2: Regional or national presence. Buys transit from Tier 1 ISPs and provides transit to Tier 3 ISPs. Serves larger regional businesses with substantial bandwidth and reliable connectivity.

ISP Tier 3: Localized, customized experience for end-users. Buys transit from Tier 1 or Tier 2 ISPs for broader internet access. Owns limited infrastructure. The ISP you contact for local internet issues

Domestic Internet Service Providers (ISPs) Provide Internet access in a particular country, state or region.

Global ISPs Have presence in several countries and also connect their domestics ISPs to the rest of the world.

Content and cloud providers provide other types of services to users around the world such as multimedia, hosting cloud services

Transit providers These are typically large Tier-1 networks that comprise the Internet backbone

Their customers are other ISPs. If two SPs are not directly connected, they typically reach one another through one or more transit providers.

B2B IP Peering

An often-overlooked form of peering is B2B peering between different networks. There are many examples of B2B peering, the most visible today are those connecting datacenter colocation providers to cloud service providers. Additionally, B2B peering is used for linear video content providers to send video to end providers, carry voice services over IP instead of PSTN, and interconnect various service owners to consumers.

#### Internet connectivity redundancy options

Single Homed one connection to a single ISP. No redundancy

![](<../.gitbook/assets/Unknown image (260)>)

Dual Homed still only connected to a single ISP, but you use two links instead of one

![](<../.gitbook/assets/Unknown image (261)>)

Single Multi-homed connected to at least two different ISPs

![](<../.gitbook/assets/Unknown image (262)>)

Dual multihomed connected to two different ISPs with redundant links

![](<../.gitbook/assets/Unknown image (263)>)

### Service provider network design

#### Point-to-point WAN links

is a link between two points, the customer edge (CE) and the provider edge (PE)

![Point to Point (P2P) WAN Connectivity Made Easy](<../.gitbook/assets/Unknown image (264)>)

#### Point-to-multipoint / NBMA (hub-and-spoke)

one central location communicates with multiple remote locations (Star-like topology), efficiently connecting a central point, like a headquarters, to multiple branches or remote sites

This topology is also known as Hub and Spoke

Broadcast traffic is not forwarded or supported as it would be in broadcast networks like Ethernet, since routers are designed to route packets based on specific addresses and do not propagate broadcast messages to all connected devices

![Multicast PIM NBMA Mode](<../.gitbook/assets/Unknown image (265)>)

#### Service provider architecture

Ideally, service provider networks should be uniform and monolithic

**Terminology**

P device: Service provider device that provides only data transport across the service provider backbone, and has no customers that are attached to it. In an MPLS implementation, such devices would be LSRs.

PE device: Service provider devices to which customer devices are attached are called provider edge (PE) devices.

CE device: The customer router that connects the customer site to the service provider network is called a customer edge (CE) router, or CE device. This device can also be called customer premises equipment (CPE).

PE-CE link: A link between a PE router and a CE router

Aggregation routers are designed to terminate connections from multiple sources/customer, and perform forwarding of high volumes of data traffic

Multi-Layer control plane has the advantage of being able to coordinate restoration efforts across different layers. For example, if a Layer 0 link fails, the control plane can not only reroute the wavelength at the optical layer but also inform the higher layers (like Layer 3) about the change in topology, allowing them to adjust routing protocols accordingly. This coordinated approach leads to faster and more efficient recovery

![](<../.gitbook/assets/Unknown image (266)>)

![](<../.gitbook/assets/Unknown image (267)>)

### Next-generation service provider architecture

#### Cisco IP NGN

In earlier days, service providers were specialized for different types of services, such as telephony, data transport, and internet service. The popularity of the internet, through telecommunications convergence, has evolved into the usage of the internet for all types of services. Development of interactive mobile applications, increasing video and broadcasting traffic, and the adoption of IP version 6 (IPv6) have pushed service providers to adopt new architecture to support new services on the reliable IP infrastructure with a good level of performance and quality.

Cisco IP Next-Generation Network (IP NGN) is the next-generation service provider architecture for providing voice, video, mobile, and cloud or managed services to users. The general idea of Cisco IP NGN is to provide all-IP transport for all services and applications, regardless of access type. IP infrastructure, service, and application layers are separated in next-generation networks (NGNs), enabling addition of new services and applications without any changes in the transport network.

The newest broadband connection that provides the highest transmission speed service over fiber optic cables, consisting of strands of glass or plastic fibers that transmit data as pulses of light

#### End-to-end principle

Idea: the core network should be as simple as possible and focus just on delivering data from the source to destination

Extra features like reliability, security, or error correction should be handled by the communicating endpoint devices, so that the network and their intermediary devices focuses solely on data forwarding rather than ensuring reliability or retransmission for the endpoints

This approach aims to keep the network infrastructure simple and efficient

End-to-End communication refers to the direct communication between the source and destination without intermediary network devices altering the data transmitted between them

This is however violated by technologies like NAT or proxy

### Wireless WAN and mobile networks

#### Radio access network (RAN)

A cellular network, or a mobile network, is a radio network that is distributed over land areas that are called cells, each served by at least one fixed-location transceiver, which is known as a cell site or a base station.

When joined, these cells provide radio coverage over a wide geographic area. This configuration enables many portable transceivers (for example, mobile phones and pagers) to communicate with each other

Although originally intended for mobile phone voice service, with the development of smart phones, cellular telephone networks routinely carry data in addition to voice.

Commonly known connection types for wireless WAN are cellular technologies 3G, 4G, LTE, and 5G offered as a service by local ISP to provide wireless internet access to mobile devices, using specific licensed frequencies to ensure wider coverage and stronger signal to customers

![](<../.gitbook/assets/Unknown image (268)>)

RAN connects individual user equipment (UE) to the internet through a radio link

Radio access network technologies have primarily evolved in two main tracks. These tracks are 3GPP and Third Generation Partnership Project 2 (3GPP2).

**3GPP / 3GPP2 terminology**

The following explains in more details the 3GPP, 3GPP2, and related RAN terminology:

Global System for Mobile Communications (GSM): A standard developed by the European Telecommunications Standards Institute (ETSI) to describe the protocols for second-generation (2G) digital cellular networks

Enhanced Data rates for GSM Evolution (EDGE): Digital mobile technology that allows improved data transmission rates as a backward-compatible extension of GSM.

GSM EDGE Radio Access Network (GERAN): The radio part of GSM/EDGE (2G) together with the network that connects the base stations.

Universal Mobile Telecommunications System (UMTS): Third generation mobile cellular system (3G) for networks based on the GSM standard. UMTS was developed and is maintained by the 3GPP.

UMTS Terrestrial Radio Access Network (UTRAN): The radio part of the UMTS. It contains the base stations, which are called Node B's and Radio Network Controllers (RNCs) which make up the UMTS RAN

Long Term Evolution (LTE): The standard for wireless broadband communication for mobile devices and data terminals, based on the GSM/EDGE and UMTS technologies. LTE was developed by 3GPP as the evolution of existing standards, published in 3GPP Release 8, and is maintained by the 3GPP.

Evolved UMTS Terrestrial Radio Access Network (E-UTRAN): The radio part of LTE. E-UTRAN features high data transfer speeds and low latency radio access.

Code-division multiple access 2000 (CDMA2000): A set of 3G standards based on the earlier Code Division Multiple Access One (CDMAOne) 2G CDMA technology. CDMA2000 is the 3G evolution of CDMAOne, a GSM competing technology, and was used predominately in North America. 3GPP2 is the standardization body working on CDMA2000.

Evolved High Rate Packet Data (eHRPD): The evolutionary path to LTE for CDMA operators as it allows the mobile operators to upgrade their existing HRPD packet core network using elements of the Evolved Packet Core (EPC) architecture.

#### RAN components and transport (fronthaul/backhaul)

Base Station (BS): Also known as a cell site, it's the physical infrastructure that houses the radio equipment. A base station typically consists of:

Radio Units (RUs): Handle the actual radio signal processing, including transmission and reception.

Baseband Units (BBUs): Process the digital data signals, performing tasks like modulation, coding, and encryption.

Antennas: Transmit and receive radio signals to and from user equipment

Fronthaul: Short-distance, high-speed connection between radio towers (RRU) and processing units (BBU) carrying user data traffic

Backhaul: Longer-distance connection between cell sites (BBU) and core network, they transport aggregated data traffic from multiple users or devices to the main network backbone, enabling communication and service delivery.

Midhaul: In some network architectures, a midhaul connection might exist between geographically dispersed BBUs to handle aggregation and distribution of data traffic before reaching the core network.

X-Haul (general term): Encompasses all network connections within a cellular network, including fronthaul, backhaul, and sometimes midhaul (data aggregation between BBUs)

#### C-RAN (Centralized RAN)

Remote Radio Unit (RRU) sits at a Cell Site (either atop the antenna or inside an Outdoor Cabinet) and far away from the Baseband Unit (BBU) which is placed at a Centralized Location

The RRU and the BBU are connected via the CPRI Interface which carries digitized IQ (or User plane) samples back and forth to be able to deliver 4G/LTE service to the end-user

{% hint style="info" %}
Fronthaul is the high-speed, low-latency connection between the central BBU and the remote radio units (RRU) in a C‑RAN architecture.
{% endhint %}

![](<../.gitbook/assets/Unknown image (269)>)

#### Common Public Radio Interface (CPRI)

It is a key specification in wireless telecommunications that defines the interface between two main components of a cellular base station:

Radio Equipment Control (REC): Typically the Baseband Unit (BBU), which performs the digital signal processing.

Radio Equipment (RE): The Remote Radio Head (RRH) or Remote Radio Unit (RRU), which handles the analog radio frequency (RF) functions (e.g., filtering, conversion, amplification) near the antenna.

Function:

CPRI is the protocol used for the Fronthaul network, which is the fiber-optic link connecting the central BBU on the ground to the remote RRH up on the tower.

It transports the digitized radio signals (I/Q data—In-phase and Quadrature data), along with control, management, and synchronization information, over this high-speed digital link.

Relevance (Why it matters for the product you showed):

The device you are looking at (N540-FH-CSR-SYS and N540-FH-AGG-SYS) is a router/aggregator designed for the mobile network, specifically a fronthaul component. The presence of CPRI ports on the specification means the device is built to directly connect to and handle traffic from the Remote Radio Heads (RRHs) in a cellular network.

The catch: CPRI is an older, bandwidth-hungry protocol (used mainly in 4G LTE). For 5G, it is largely being superseded by eCPRI (enhanced CPRI), which is designed to be more bandwidth-efficient and run over standard Ethernet/IP networks, providing greater flexibility and lower cost. Any contemporary equipment still relying heavily on classic CPRI is either legacy-focused or limited in its 5G applicability without the eCPRI feature.

CPRI or CPRI is based on constant bit rate of traffic, but in order to carry CPRI traffic over a packet based network an industry defined technique is needed to transmit all radio data types over a traditional Ethernet based Fronthaul network. IEEE 1914.3 Radio over Ethernet is a standard specification that defines encapsulation and mapping techniques of radio protocols, for transport over Ethernet frames

Operation

Mobile Device: Transmits and receives radio signals.

Remote Radio Unit (RRU): Converts the radio signals to an intermediate format and sends it to the BBU.

Baseband Processing Unit (BBU): Performs further processing, converts the signal to digital data packets, and sends them to the fronthaul network.

Fronthaul Router: Forwards these data packets efficiently between the BBU and the RRUs.

RRU: Receives the data packets, converts them back to an appropriate format, and transmits radio signals to mobile devices.

#### Cisco RAN infrastructure (examples)

With the increase in bandwidth needs, mobile networks are expanding, with multiple levels of aggregation and pre-aggregation added, as necessary to segment and manage traffic.

![](<../.gitbook/assets/Unknown image (270)>)

Common platforms deployed at cell sites include the smaller capacity routers such as Cisco ASR 920 and NCS 540 Routers. Preaggregation devices include the higher capacity models from the Cisco ASR 900 and NCS 500 Series Routers, and at the aggregation and mobile transport gateway layers, Cisco ASR 9000 Series Routers and NCS 5500 Series Routers are typically used.

![](<../.gitbook/assets/Unknown image (271)>)

eNodeB (evolved NodeB) is a base station in 4G LTE mobile networks that wirelessly communicates with mobile devices (UE) and connects the E-UTRAN network to the core network. It acts as an interface that manages radio resources, manages user mobility, processes data packets, provides authentication and security, and ensures efficient data transmission using technologies such as OFDMA and MIMO. Unlike older 3G networks, eNodeB integrates features previously managed by a standalone Radio Network Controller (RNC), simplifying architecture and reducing latency.

also performs the radio bearer control, radio admission control, and scheduling of uplink and downlink radio resources for individual UE. Encryption of the user data plane and compression of the IP header are also controlled by the eNodeB.

Mobility Management Entity (MME) performs the function of selecting the SGW for a UE at initial attach and even during the handover. By interacting with a user database, the MME is responsible for authenticating the end user, and during the roaming the MME enforces any roaming restrictions that the UE may have. MME provides the control plane functionality for mobility between the LTE and 2G/3G access networks. The MME acts as the terminating point in the network for the security of signaling, handling the ciphering protection and management of security keys.

Serving Gateway Key device that routes and forwards user data packets for UE, also responsible for the following:

Acting as the mobility anchor during inter-eNodeB handovers

Terminating the downlink data path and triggering paging when data arrives for UE

Lawful interception

QoS marking in the uplink and downlink

PDN Gateway acts as an anchor point for sessions toward the external packet data networks. In its role as gateway, the PGW may perform packet inspection or packet filtering on a per-user basis. The PGW also performs service-level gating control and rate enforcement through rate policing and shaping. From a quality of service (QoS) perspective, the PGW also marks the uplink and downlink packets with the differentiated services code point (DSCP). In the case of mobility between 3GPP and non-3GPP technologies such as Wi-Fi or WiMax, the PGW serves as the anchor point.

Home Subscriber Server (HSS) stores all the user subscription information, including user identification and addressing, which includes the International Mobile Subscriber Identity (IMSI) and Mobile Subscriber ISDN Number (MSISDN) or mobile telephone number. It also stores user profile information, including service subscription states and the QoS that the user has subscribed to.

The HSS is also in charge of generating security information from user identity keys. Security is mainly used for mutual network terminal authentication, radio path ciphering, and integrity protection to make sure the data that is transferred between the network and the terminal is not eavesdropped upon or altered. For this function, the HSS is interrogated as the user attempts to register to the network to check the user subscription rights.

![](<../.gitbook/assets/Unknown image (272)>)

#### LTE core overview (EPC / EPS)

is the name for all components that are associated with the 3GPP committee-defined 4G mobile technology. Evolved packet core is the IP packet core part of the system, which routes both voice and data information encapsulated in standard IP packets.

![](<../.gitbook/assets/Unknown image (273)>)

#### 5G networks

The fifth generation of cellular systems (5G) is the next generation of 3GPP technology that comes after 4G LTE and is defined for wireless mobile voice and data communication. Starting with 3GPP Release 15 onward, the working groups within 3GPP define the standards for the fifth generation of mobile networks.

[https://www.cisco.com/c/en/us/solutions/service-provider/mobile-internet/5g-transport/converged-5g-xhaul-transport.html#](https://www.cisco.com/c/en/us/solutions/service-provider/mobile-internet/5g-transport/converged-5g-xhaul-transport.html)

Improvements

Very high throughput (1-20 Gbps)

Move to a new radio: 5G New Radio (5GNR, or NR) with OFDM

Increasing antenna count: massive MIMO antenna technology

Wider bands: 10/20 MHz to 100/200 MHz in millimeter spectrum

Support for ultra reliable sub 1-ms low-latency services

RANs becoming more centralized and increasingly virtualized

The new 5G packet core network (5GC, or 5GCN) offers new applications

Local hosting of services and Mobility Edge Computing (MEC)

Unified access control

Support for 3GPP and non-3GPP access (Wi-Fi)

The 5G System (5GS) is composed of the User Equipment, the 5G Access Network ("New Radio" or NR) and the Core Network (5GC or 5GCN).

Use cases

Fixed wireless access (FWA): Providing internet access to homes via 5G, is the simplest form of providing 5G internet access to the customers. Its lack of mobility requirements makes it simple to deploy and maintain.

Mobile internet: Offering mobility with voice and data services to the subscribers at higher speeds and lower latency than previous generations

Vehicle-to-everything (V2X): Passing of information from a vehicle to any entity that may affect the vehicle, or vice versa, in a highly reliable way with low latency. Vehicles will have to support the standard and some automotive vendors already include the support for V2X in their products

Robotics and virtual reality: Use cases where limited mobility is required but a high data rate with low latency is a must.

5G System Architecture

The 5G core (5GC) architecture uses the SBA framework, where the architecture elements are defined in terms of network functions (NFs) rather than by network entities. The network functions in the new 5G core are broken down into smaller entities such as the Session Management Function (SMF) and User Plane Function (UPF), which can be used on a per-service basis. Gone are the days of huge network boxes; welcome to services that automatically register and configure themselves over the service-based architecture, which is built with the new functions such as the Network Repository Function (NRF), which borrows their capabilities from cloud native technologies

![](<../.gitbook/assets/Unknown image (274)>)

Network slicing is a new concept to allow differentiated treatment depending on each customer requirement. With slicing, it is possible for Mobile Network Operators (MNO) to consider customers as belonging to different tenant types with each having different service requirements that govern in terms of what slice types each tenant is eligible to use based on service level agreement (SLA) and subscriptions.

#### WiMAX (IEEE 802.16)

is a collection of wireless broadband communication standards based on the IEEE 802.16 set of standards comparable to Wi-Fi technology and so is nicknamed as "WiFi on steroids."

WiMax is a standards-based technology that enables the delivery of last-mile wireless broadband connectivity as an alternative to cable and DSL.

IEEE 802.16m, also known as WirelessMAN-Advanced, was a competitor for 4G

However, WiMax provides much higher data rates, is used for outdoor networks, and uses IEEE 802.16 standards in contrast to IEEE 802.11 standards of Wi-Fi.

The primary difference between WiMax and Wi-fi is that WiMax technology is not primarily intended for end users. It is used by ISP’s to create a Backhaul networks connecting subscribers to the ISP’s POP

Backhaul Network is the backbone part of the network designed to handle high volumes of data traffic from multiple smaller subscriber edge networks

It operates in the frequency band of 2 GHz to 11 GHz. The bandwidth is dynamically allocated as per user requirements.

However WiMAX can operate on the same unlicensed frequency ranges of 2.4 and 5 GHz used by Wi-Fi

WiMax initially provided data rates of 30 to 40 Mbps. The updated version that came in 2011 provides up to 1 Gbps data rates for fixed stations.

WiMax is a long-range communication technology with range up to 90km

Unlike Wi-Fi's use for WLANs, the WiMax 802.16 standard is used in wireless metropolitan area networks (WMANs)

WMANs are fixed broadband wireless access links that connect a service provider's subscriber networks to a base station at a wireless carrier's point of presence

![Wimax Network : How Does It Work](<../.gitbook/assets/Unknown image (275)>)

WiMAX supports both fixed and mobile applications. Fixed WiMAX is primarily used for providing high-speed internet access to fixed locations, such as homes and businesses. Mobile WiMAX extends this capability to support mobile devices, such as smartphones and tablets

At the time, WiMax was expected to become the next Wi-Fi, but its adoption was hindered by higher costs, proprietary vendor technology and lack of carrier interest.
