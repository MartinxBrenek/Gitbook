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

# Network Architecture

## Local area network (LAN)

**Local area network (LAN)** is a small structure of a multiple devices connected within a same floor or a building - geographically in the same location

LANs can vary widely in size. A LAN may consist of only two computers in a home office or small business, or it may include hundreds of computers in a large corporate office or multiple buildings. A LAN is typically a network within your own premises (your organization's campus, building, office suite, or even your home). Organizations or individuals typically build and own the whole infrastructure, all the way down to the physical cabling.

#### SOHO

**Small office/home office (SOHO)** refers to the household LAN

#### Storage area network (SAN)

**Storage Area Network (SAN)** is a network of storage devices providing a shared pool of storage space for a LAN machines

### Network topologies

#### Physical vs logical topology

LAN Physical Network Topologies

refers to the physical or logical structure of devices and connections in a computer network.

During the development of the network, several types of topologies were invented to connect devices the most efficiently

**Physical topology** is determined by the physical connection of devices

**Logical topology** is the path which data travels from one point in the network to another. The diagram depicts the logical topology between PC A and the Server. In this example, data does not follow the shortest physical path, which would go through two switches. The logical topology requires data to also travel through the router for the two devices to communicate. The same could be true for all other end devices. Logical topology would then be a star, where the router is a central device.

The logical topology is determined by the intermediary devices and the protocols chosen to implement the network. The intermediary devices and network protocols both determine how end devices access the media and how they exchange data.

A physical star topology in which a switch is the central device is by far the most common in implementations of LANs today. When using a switch to interconnect the devices, both the physical and the logical topologies are star topologies.

The logical and physical topology of a network can be of the same type. However, physical and logical topologies often differ

![](<../.gitbook/assets/Unknown image (430)>)

#### Point-to-point (P2P)

**Point-to-point (P2P)** is the simplest topology, with a direct link between two devices

![](<../.gitbook/assets/Unknown image (431)>)

#### Bus topology

**Bus topology** is where all devices are attached to a long communication medium/line such as coaxial cable.

A bus topology makes detecting network failures difficult. If the main cable is corrupted, the network goes down.

Every additional node slows down the speed of data transmission in the network.

Data can be sent only in one direction and is half-duplex. When one station sends a packet to a target station, the packet is sent to all stations (broadcast communication)

Since this one link is shared by all connected devices they implement CSMA/CD to prevent collisions

At each end of the coaxial cable, terminators are used to absorb the signal and prevent it from bouncing back and causing interference

Token Bus was technology that created a logical token ring in networks built using the bus topology. A token is passed from one station to another in a defined sequence that represents the logical ring in the clockwise or counter-clockwise direction. Only the token holder (the station having the token) can transmit frames in the network.

![](<../.gitbook/assets/Unknown image (432)>)

![](<../.gitbook/assets/Unknown image (433)>)

#### Ring topology

**Ring topology** is a modification of a bus topology, connecting devices in a circular arrangement, forming a closed loop, where data travels in only sequentially in one direction, hence, the network works in half-duplex mode.

Each device in the ring acts as a repeater, regenerating and retransmitting the data, ensuring that it continues to flow around the ring

With each device added, the latency increases as each station has to process and send the frame to the next station

If a device in the ring fails, it can disrupt the entire network. In such cases, the data transmission is interrupted, and the network becomes inaccessible

However with Dualring topology we can create a redundant pathway, allowing data to flow in the opposite direction if one ring is broken.

![](<../.gitbook/assets/Unknown image (434)>)

Dual Ring allows for full duplex mode, where sending occur in one direction and receiving in a second

The optical ring in modern networks uses the ring network topology. This network topology is primarily used by ISPs to create connections in wide area networks.

![](<../.gitbook/assets/Unknown image (435)>)

Token ring and Fiber distributed data interface (FDDI) are examples of network technologies that use a ring topology

These technologies are not as prevalent today, as other topologies like Ethernet have become more popular

A special three-byte data frame called a "token" circulates around the network ring. Only the device holding the token is allowed to transmit data onto the network.

When a device wants to transmit data, it captures the token, attaches its data to the token frame, and then releases the token back onto the network.

The token continues to circulate around the ring, allowing each device to have a turn to transmit data when it holds the token.

After a device sends its data, it releases the token, allowing the next device in the ring to capture it and transmit its own data.

This token-passing mechanism ensures that only one device can transmit data at a time, preventing collisions and ensuring fair access to the network medium.

FDDI uses a dual-ring topology for redundancy and fault tolerance

#### Star topology

**Star topology** is the most common network topology used nowadays, it connects all devices to a centralized unit called switch/hub

If two stations interact with each other in the network, a frame leaves the network adapter of the sender and is sent to the switch, and then a switch retranslates the frame to the network card of the destination station

![](<../.gitbook/assets/Unknown image (436)>)

#### Extended star / tree topology

**Extended star / tree topology** is an hierarchical extension of the star topology and is widely used nowadays

Multiple star networks representing different sites are connected to the one main site or multiple switches in one building are connected to the main switch or router in the same building

![](<../.gitbook/assets/Unknown image (437)>)

![](<../.gitbook/assets/Unknown image (438)>)

![](<../.gitbook/assets/Unknown image (439)>)

#### Full mesh and partial mesh

**Full mesh** is where every node or device is directly connected to every other node, often used in critical communication scenarios where reliability and redundancy is priority

The number of connections for a full mesh is calculated with the formula Nc=N(N-1)/2 links, where N is the number of nodes in the network - not much scalable and costly

**Partial mesh** a compromise between price and redundancy, where only the main nodes of the network are connected to each other

![](<../.gitbook/assets/Unknown image (440)>)

![Network Topologies](<../.gitbook/assets/Unknown image (441)>)

#### Broadcast network

**Broadcast network** is a segment of multiple devices within the same IP subnet. Primarily used in LAN network topologies like star

![CCNA Complete Course: OSPF DR BDR Election Process | Types of OSPF networks](<../.gitbook/assets/Unknown image (442)>)

### Enterprise campus design

#### Enterprise hierarchical LAN model

By breaking up the design into modular layers, you can have each layer to implement specific functions, which simplifies the network design and provides an easy way to scale the network. In a modular layer design, network components can be placed or taken out of service with little to no impact to the rest of the network, facilitating network management

![](<../.gitbook/assets/Unknown image (443)>)

**Access layer**

Access Layer connects endpoints to the network and cover security with QoS policies

Provides high-bandwidth device connectivity using wired and wireless access technologies

**Distribution layer**

Distribution Layer acts as L3 default-gateway/FHRP for access Layer hosts and summarizes networks to Core

It is recommended to implement any processing-intensive features such as ACL's, Inter-vlan routing or traffic filtering on the distribution layer, so that the core can focus to handle large amout of incoming and outgoing traffic from the distribution layer or from the outside of the domain

**Core layer**

Core Layer high-speed central part/backbone, which ensures traffic forwarding between distribution layers

![](<../.gitbook/assets/Unknown image (444)>)

#### Two-tier design (collapsed core)

**Two-tier design (collapsed core)** is where the core and distribution layer is collapsed. Used in small campus networks

#### Three-tier design

**Three-tier design** is recommended when more than two pairs of distribution switches are required

#### Layer 3 access (routed access)

**Layer 3 access (routed access)** is when L3 is extended to access layer switches, which excludes need for FHRP and STP, providing fast convergence

Facilitates troubleshooting as all hops are visible in traceroute which make tshoot easier

![](<../.gitbook/assets/Unknown image (445)>)

#### Simplified campus design

Campus is a group of one or more buildings, and surrounding grounds, where people and their belongings work together

It utilizes switch clustering with VSS; StackWise. This simplifies design > there are fewer boxes to manage

Etherchannel avoids the need for FHRP, STP and increases BW, providing sub-second convergence

![](<../.gitbook/assets/Unknown image (446)>)

### Overlay networks

#### Overlay vs underlay

**Overlay network** is a logical network built on top of an existing physical transport network called an **underlay** or transport network

Overlay networks are used to overcome shortcomings of traditional networks by enabling network virtualization, segmentation, and security to make traditional networks more manageable, flexible, secure and scalable. Example of overlay can be all VPN types or SDN technologies like VXLAN

{% hint style="info" %}
An overlay tunnel can be built over another overlay tunnel.
{% endhint %}

![](<../.gitbook/assets/Unknown image (447)>)

### Data center architecture

#### Spine-leaf

**Spine-leaf** is designed for data centers (DCs), providing better scalability and high availability than traditional network architectures

Spine switches serve as the core of the network and are connected to all the leaf switches

Leaf switches connect directly to endpoints such as servers or storage devices.

The spine switches and leaf switches are connected in a full mesh topology, allowing for multiple parallel paths between any two endpoints

New leaf switches can be easily added to the network without affecting existing traffic flows

The full mesh topology provides redundancy, so if a link or switch fails, traffic can be automatically rerouted through another path

![](<../.gitbook/assets/Unknown image (448)>)

#### Top-of-rack (ToR)

**Top-of-rack (ToR)** is a switch or router that acts as a central connection point for servers in a rack and ensures the connection to the aggregation device in a large-scale Data centers

![Top of Rack and End of Row: What's the Difference? | FS Community](<../.gitbook/assets/Unknown image (449)>)

### Storage networking

#### Fibre Channel (FC)

**Fibre Channel (FC)** is a high-speed network technology primarily used for storage networking. As an integral part of modern data center infrastructures, it facilitates the construction of large-scale storage area networks (SANs).

Fibre Channel operates on both optical and electrical interfaces, capable of handling data rates from 1 Gbps to 128 Gbps and beyond.

Speed: Ranges from 1 Gbps up to 128 Gbps (e.g., FC-1, FC-2, FC-16, FC-32, FC-64, FC-128).

Its architecture allows for several topologies, such as point-to-point, arbitrated loop, and switched fabric. The switched fabric topology, being the most scalable, is widely used in enterprise environments, allowing thousands of ports to communicate simultaneously.

Fibre Channel uses its own protocol stack, completely separate from the TCP/IP or OSI model used by Ethernet/IP networks.

Fibre Channel Protocol (FCP) Fibre Channel frames encapsulate multiple upper-layer protocols (ULPs), including IP and Small Computer Systems Interface (SCSI), facilitating both network and storage communication

By default: Fibre Channel runs on its own physical layer.

However, there are two technologies where Fibre Channel is encapsulated over Ethernet:

**FCoE (Fibre Channel over Ethernet)**

FCoE (Fibre Channel over Ethernet):

FC frames are encapsulated into Ethernet frames.

Runs over Data Center Bridging (DCB) Ethernet.

Used to consolidate LAN and SAN on a single fabric.

Requires special support on switches (e.g., Cisco Nexus with FCoE support).

**FCIP (Fibre Channel over IP)**

FCIP (Fibre Channel over IP):

FC is encapsulated over IP for remote SAN extension.

Typically used between data centers over WAN links.

### Documenting networks

#### Network diagrams

Network diagrams are visual aids in understanding how a network is designed and to show how it operates. In essence, they are maps of the network. They illustrate physical and logical devices and their interconnections. Depending on the amount of information you wish to present, you can have multiple diagrams for a network. The most common diagrams are physical and logical diagrams.

Both physical and logical diagrams use icons to represent devices and media. Usually, there is additional information about devices, such as device names and models.

Physical diagrams focus on how physical interconnections are laid out and include device interface labels (to indicate the physical ports to which media is connected) and location identifiers (to indicate where devices can be found physically). Logical network diagrams also include encircling symbols (ovals, circles, and rectangles), which indicate how devices or cables are grouped. These symbols further include device and network logical identifiers, such as addresses. These symbols also indicate which networking processes are configured, such as routing protocols, and provide their basic parameters.

The network diagram must be documented and continuously updated, so that we are able to find what the network looks like in case we would have to troubleshoot it in the future

#### Physical diagrams

Depending on the device type and device model, the numbering convention usually refers to the following:

slot# / port#

for example, Te1/4. This is the fourth port in slot 1.

slot# / sub-slot# / port#

for example, G1/2/1. This is the first port in slot 1, sub-slot 2.

A slot is typically an opening in a router or switch that allows you to install a module for extra functionality. Some fixed-port switches don’t have modular slots. Instead, all ports are assigned to the built-in default slot, slot 0. Some modules can also include several smaller slots called sub-slots.

![](<../.gitbook/assets/Unknown image (450)>)

#### Logical diagrams

Logical Diagrams depicts how devices are using network mediums to communicate - or how traffic flows through the network

Both Logical and Physical diagrams are usually documented in one file

![What is a Logical Network Diagram?](<../.gitbook/assets/Unknown image (451)>)

#### Uplink vs downlink

**Uplink** is the connection that allows data to be sent from the network device to a higher-level network or the internet

**Downlink** is the connection that allows data to be received by the network device from a higher-level network or the internet

![](<../.gitbook/assets/Unknown image (452)>)

#### HLD (high-level design)

**HLD (high-level design)** involves creating an overview or blueprint of the entire network. It focuses on defining the network's architecture, topological design and high-level requirements. At the HLD stage, you're concerned with designing the overall network structure, including the placement of routers, switches, firewalls and other network devices. You also consider factors like network segmentation, redundancy and scalability. The result of HLD will be documents like physical and logical topologies and high-level descriptions of network components and their interconnections.

#### Environments (PROD / UAT)

PROD refer to the production environment

UAT refers to the test environment

#### CAPEX vs OPEX

**Capital expenditure (CAPEX)**

refers to the funds that a company invests in long-term assets or capital projects to acquire, upgrade, or maintain physical assets. These assets typically have a useful life of more than one year and are essential for the company's operations or future growth.

**Operating expenditure (OPEX)**

refers to the day-to-day expenses that a company incurs to maintain its business operations and generate revenue

Examples of OPEX include costs related to salaries, wages, utilities, rent, insurance, office supplies, marketing, and administrative expenses. It also includes expenses associated with providing services, such as customer support, maintenance, and repair.

OPEX is typically recorded on the income statement as expenses and deducted from revenue to calculate the company's operating profit or loss. Unlike CAPEX, which represents investments in long-term assets, OPEX reflects the ongoing costs of running the business and generating revenue

Most of modern deployments consist of:

* Internet edge: A segment representing the edge of a network that serves as entry and exit points to the network for a given location, and also mostly for Internet access
* Data center (DC): A centralized site that provides services and stores data for an organization
* LAN: A local area network that connects devices within a single location
* WAN: Broadband network that connects individual customer sites

#### LLD (low-level design)

**LLD (low-level design)** provides detailed instructions for configuration and implementation of each individual network elements, specifying IP addressing schemes, access control policies, routing protocols, and other configuration details. This includes hardware configurations for routers, switches, firewalls, etc. LLD is much closer to the actual implementation and configuration

#### Deployment types (greenfield / brownfield)

**Greenfield deployment** refers to the implementation of a new system, network, or infrastructure from scratch, typically in a clean and untouched environment

**Brownfield deployment** involves making changes, upgrades, or additions to an existing system, network, or infrastructure that has already been deployed

#### Proof of concept (PoC)

**Proof of concept (PoC)** is a methodology used in networking design to demonstrate the feasibility of a proposed solution.

It is a preliminary stage in the development process that aims to validate the concept and ensure its business value

#### North-south vs east-west traffic flow

**North-south traffic** This term refers to the data traffic that flows between the internal network of an organization and external networks

**East-west traffic** refers to the data traffic that moves within the internal network of an organization, typically between devices in the same data center or between different departments

If you have multiple networks that serve the same purpose for an example usernetworks from different buildings / floors then you can just terminate them on the switch and create p2p link to the firewall.

The firewall would be transit box between zones and security segments that's the way I prefer doing it. East west traffic does not hit the firewall north south does.

#### Disaster recovery plan (DRP)

**Disaster recovery plan (DRP)** refers to planning and implementing measures to ensure that a network can quickly and effectively recover from a disruptive event such as natural disasters, cyber-attacks, equipment failures, human errors, and so on. The goal of a DRP is to minimize downtime, data loss, and service interruption, allowing the network to resume normal operations as quickly as possible.

Business Continuity Planning and Disaster Recovery (BCP/DR) is comprehensive process that focuses on keeping all aspects of a business running after a disaster

Risk assessment and planning: identify potential risks and vulnerabilities that could affect the network.

Redundancy: to ensure a reliable network, it's important for organizations to design their network architecture with redundancies to avoid Single Points of Failure (SPOFs), which could potentially cut off the entire site from the network. By having backup pathways, alternative routes and failover mechanisms, the network remains resilient even if a critical component fails

Backup and restore: regularly backup critical data and configurations to offsite locations.

Testing and training: regularly test the disaster recovery plan through simulated scenarios to identify and address potential weaknesses.

#### RTO and RPO

**Recovery time objective (RTO):** define the maximum acceptable duration of time within which a business process or service must be restored after a disruption.

**Recovery point objective (RPO):** define the maximum amount of data loss that is acceptable in the event of a disruption or disaster.

## Wide area network (WAN)

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

## WLAN

### Wireless personal area network (WPAN)

A personal area network (PAN) is a network that exists within a relatively small area and connects electronic devices such as desktop computers, printers, scanners, fax machines, and notebook computers. In the past, connecting these devices required extensive cabling, connectors, and adapters. Now, these devices typically use Bluetooth to connect to the WPAN.

Typical applications for WPANs are in the office environment. WPAN there enables communication between devices in proximity (within several meters of each other) to communicate as if they were connected by a cable.

It is a wireless network established by personal devices within the range of an individual person, such as a smartphone, tablet, or laptop connected via Bluetooth, AirPlay or other short-range wireless technologies.

#### Wi‑Fi Direct

**Wi-Fi Direct** is used to connect wireless devices for printing, sharing, syncing, and display.

Not everyone has (or wants) access to a Wi-Fi AP or hotspot. However, users often carry content and applications that they want to share, print, display, or synchronize. Wi-Fi Direct is a certification by the Wi-Fi Alliance. The intent is the creation of peer-to-peer Wi-Fi connections between devices, without the need for an AP. It is another example of a WPAN.

This connection, which can operate without a dedicated AP, does not operate in IBSS mode. Wi-Fi Direct is an innovation that operates as an extension to the infrastructure mode of operation. With the technology that underlies Wi-Fi Direct, a device can maintain a peer-to-peer connection to another device inside an infrastructure network—an impossible task in ad hoc mode.

Wi-Fi Direct devices include Wi-Fi Protected Setup (WPS), which makes it easy to set up a connection and enable security protections. Often, these processes are as simple as pushing a button on each device.

Devices can operate one-to-one or one-to-many for connectivity.

Miracast connections over Wi-Fi Direct allow a device to display photos, files, and videos on an external monitor or television.

Wi-Fi Direct for Digital Living Network Alliance (DLNA) lets devices stream music and video between each other.

Wi-Fi Direct Print gives users the ability to print documents directly from a smart phone, tablet, or PC.

### Wireless local area network (WLAN)

In contrast to WPANs, WLANs provide more robust wireless network connectivity over a local area, up to approximately 100 m (328 feet) between an AP and associated clients. The goal is not to connect one device to another but to connect end devices to the backbone network (typically a wired LAN) without the need for cables. WLANs today are based on the IEEE 802.11 standard and are referred to as Wi-Fi networks.

#### Wi‑Fi (IEEE 802.11)

**Wi‑Fi (IEEE 802.11)** is a combination of wireless network protocols based on the IEEE 802.11 family of standards used to allow devices in the wireless LAN access the Internet

WiFi specifies how to physically create a wireless network using approaches similar to the Ethernet standard

WiFi is built into most of today's computers and mobile devices, such as smartphones and IoT devices

WiFi networks transmits have signal at the 2.4 GHz and 5 GHz radio frequency range reaching to about 100 meters, therefore, they are best for indoor use

#### Wi‑Fi generations

| **Wi-Fi Name** | **IEEE Standard**   | **Max Speed** | **Frequency Bands** | **Channel Width** | **Year Released** | **Key Features**                              |
| -------------- | ------------------- | ------------- | ------------------- | ----------------- | ----------------- | --------------------------------------------- |
| Wi-Fi 1        | 802.11b             | 11 Mbps       | 2.4 GHz             | 20 MHz            | 1999              | DSSS, legacy standard                         |
| Wi-Fi 2        | 802.11a             | 54 Mbps       | 5 GHz               | 20 MHz            | 1999              | OFDM, higher speed than 11b                   |
| Wi-Fi 3        | 802.11g             | 54 Mbps       | 2.4 GHz             | 20 MHz            | 2003              | Backward compatible with 11b                  |
| Wi-Fi 4        | 802.11n             | 600 Mbps      | 2.4 & 5 GHz         | 20/40 MHz         | 2009              | MIMO (up to 4x4), frame aggregation           |
| Wi-Fi 5        | 802.11ac            | 6.9 Gbps      | 5 GHz               | 20/40/80/160 MHz  | 2014              | MU-MIMO (DL), Beamforming, 256-QAM            |
| Wi-Fi 6        | 802.11ax            | 9.6 Gbps      | 2.4 & 5 GHz         | 20–160 MHz        | 2019              | OFDMA, MU-MIMO (DL/UL), BSS coloring          |
| Wi-Fi 6E       | 802.11ax (extended) | 9.6 Gbps      | 2.4, 5 & **6 GHz**  | 20–160 MHz        | 2021              | Adds 6 GHz band support                       |
| Wi-Fi 7        | 802.11be            | 46 Gbps       | 2.4, 5 & 6 GHz      | 20–320 MHz        | 2024–2025         | 4K-QAM, MLO, 16x MU-MIMO, preamble puncturing |

**Wi‑Fi 7 highlights**

* Faster speeds (up to 46 Gbps)

Theoretical max: \~46 Gbps using 16 spatial streams.

Practical use: 2–5x faster than Wi-Fi 6.

* 320 MHz channel width

Doubles the 160 MHz max from Wi-Fi 6E.

Only available in the 6 GHz band.

* 4096‑QAM (4K‑QAM)

Increases data rate by 20% compared to 1024-QAM (Wi-Fi 6).

Requires very clean signal conditions.

* Multi-Link Operation (MLO)

Simultaneous use of multiple bands (2.4, 5, 6 GHz).

Increases reliability, throughput, and reduces latency.

* Lower latency

Better for real-time apps (AR/VR, gaming, video conferencing).

* Enhanced MU‑MIMO

Up to 16 spatial streams (was 8 in Wi-Fi 6).

Supports more simultaneous users/devices.

* Preamble puncturing

Allows using clean parts of partially overlapping channels.

Improves spectrum efficiency.

* Improved OFDMA scheduling

More efficient resource allocation, especially in dense environments.

### Wireless metropolitan area network (WMAN)

**Wireless metropolitan area network (WMAN)** is a wireless communications network that covers a large geographic area, such as a city or a suburb. In this type of area, the wireless signal often provides a point-to-point or point-to-multipoint backbone. Wireless can be used to create links at a low cost: organizations need only two end devices that point at each other instead of a complex and costly wired infrastructure.

![](<../.gitbook/assets/Unknown image (453)>)

### WLAN building blocks

#### Access point (AP)

**Access points (APs)** are Layer 2 devices whose primary function is to bridge 802.11 WLAN traffic to 802.3 Ethernet traffic. APs can have internal (integrated) or external antennas to radiate the wireless signal and provide coverage with the wireless network.

#### Basic Service Area (BSA) and Basic Service Set (BSS)

**Basic Service Area (BSA)** physical range of AP signal

The central device in the BSA or wireless cell is an AP, which is close in concept to an Ethernet hub in relaying communication. But, as in an ad hoc network, all devices share the same frequency. Only one device can communicate at a given time, sending its frame to the AP, which then relays the frame to its final destination—this is half-duplex communication.

Although the system might be more complex than a simple peer-to-peer network, an AP is usually better equipped to manage congestion. An AP can also connect one client to another in the same Wi-Fi space or to the wired network—a crucial capability.

The comparison to a hub is made because of the half-duplex aspect of the WLAN client communication. However, APs have some functions that a wired hub simply does not possess. For example, an AP can address and direct Wi-Fi traffic. Managed switches maintain dynamic MAC address tables that can direct packets to ports that are based on the destination MAC address of the frame. Similarly, an AP directs traffic to the network backbone or back into the wireless medium, based on MAC addresses. The IEEE 802.11 header of a wireless frame typically has three MAC addresses but can have as many as four in certain situations. The receiver is identified by MAC Address 1, and the transmitter is identified by MAC Address 2. The receiver uses MAC Address 3 for filtering purposes, and MAC Address 4 is only present in specific designs in a mesh network. The AP uses the specific Layer 2 addressing scheme of the wireless frames to forward the upper-layer information to the network backbone or back to the wireless space toward another wireless client.

**Basic service set (BSS)** closed group of wireless devices connected to one AP within it's BSA

AP is dedicated to centralizing the communication between clients. This AP defines the frequency and wireless workgroup values. The clients need to connect to the AP in order to communicate with the other clients in the group and to access other network devices and resources.

#### SSID and BSSID

**Service Set Identifier (SSID)** To roam between different APs within a network, the APs must share the same network name. This network name is called the Service Set Identifier (SSID), which has as many as 32 ASCII characters and is configured on both the AP and the client stations that wish to join (associate) with this AP. However, the SSID may also require some type of authorization to determine which station has the right to connect. The term WLAN is often used to define both the SSID and the associated parameters (VLAN, security, quality of service \[QoS], and so on). Human-readable name of the AP (doesn't have to be unique)

**BSS identifier (BSSID)** MAC address, usually derived from the radio MAC address and associated with an SSID, - unique

Because this BSSID is a MAC address that is derived from the radio MAC address, APs can often generate several values. This ability allows the AP to support several SSIDs in a single cell.

An administrator can create several SSIDs on the same AP (for example, a guest SSID and an internal SSID). The criteria by which a station is allowed on one or the other SSID will be different, but the AP will be the same. This configuration is an example of Multiple Basic SSIDs (MBSSIDs).

MBSSIDs are basically virtual APs. All of the configured SSIDs share the same physical device, which has a half-duplex radio. As a result, if two users of two SSIDs on the same AP try to send a frame at the same time, the frames will collide. Even if the SSIDs are different, the Wi-Fi space is the same. Using MBSSIDs is only a way of differentiating the traffic that reaches the AP, not a way to increase the capacity of the AP.

SSIDs can be either broadcast (or advertised) or not broadcast (or hidden) by the APs. A hidden network is still detectable. APs periodically send out special frames called "beacon frames" over the air. These frames contain information about the network, including the SSID. Also, when a device wants to connect to a Wi-Fi network, it sends out a "probe request" looking for networks with a specific SSID. Access Points that have the requested SSID respond with a "probe response," providing details about the network and inviting the device to connect.

![](<../.gitbook/assets/Unknown image (454)>)

#### Mapping SSIDs to VLANs

VLANs provide an ideal way of separating users on different WLAN SSIDs when they access the wired side of the network. By associating each SSID to a different VLAN, you can group users on the Ethernet segment the same way that they were grouped in the WLAN. You can also isolate groups from each other, in the same way that they were isolated on the WLAN.

When the frames are in different SSIDs in the wireless space, they are isolated from each other. Different authentication and encryption mechanisms per SSID and subnet isolate them, even though they share the same wireless space.

When frames come from the wireless space and reach the Cisco WLC, they contain the SSID information in the 802.11 encapsulated header. The Cisco WLC uses the information to determine which SSID the client was on.

When configuring the Cisco WLC, the administrator associates each SSID to a VLAN ID. As a result, the Cisco WLC changes the 802.11 header into an 802.3 header, and adds the VLAN ID that is associated with the SSID. The frame is then sent on the wired trunk link with that VLAN ID.

**Distribution system (DS)** interconnect BSS and wired network; it is connection from AP to the switch

WLCs and APs are usually connected to switches. The switch interfaces must be configured appropriately, and the switch must be configured with the appropriate VLANs. The configuration on switches regarding the VLANs is the same as usual. The configuration differs on interfaces though, depending on if the deployment is centralized (using a WLC) or autonomous (without a WLC).

The following types of VLANs are required with WLANs:

Management VLAN

AP VLAN

Data VLAN

The management VLAN is for the WLC management interface configured on the WLC. The APs that register to the WLC can use the same VLAN as the WLC management VLAN, or they can use a separate VLAN. The APs can use this VLAN to obtain IP addresses through DHCP and send their discovery request to the WLC management interface using those IP addresses. To support wireless clients, you will need a VLAN (or VLANs) with which to map the client SSIDs to the WLC. You may also want to use the DHCP server for the clients

Layer 3 mode is the dominant mode today, where the AP interfaces are on a different subnet than the WLC management interface.

Second, you will need either the Layer 3 switch or a router to perform inter-VLAN routing. Usually, inter-VLAN routing is configured along with the VLAN creation.

![](<../.gitbook/assets/Unknown image (455)>)

### WLAN operating modes and topologies

#### Independent BSS (IBSS / ad hoc)

**Independent BSS (IBSS / ad hoc)** is when 2+ wireless devices connect directly without AP. Also called ad hoc network. Examples: Airdrop/Airplay

Because the computers in an ad hoc network communicate without other devices (AP, switch, and so on), the BSS in an ad hoc network is called an IBSS. Computer-to-computer wireless communication is most commonly referred to as an ad hoc network, IBSS, or peer-to-peer (wireless) network.

![](<../.gitbook/assets/Unknown image (456)>)

#### Extended Service Set (ESS)

When the distribution system links two APs, or two cells, the group is called an Extended Service Set (ESS). This scenario is common in most Wi-Fi networks because it allows Wi-Fi stations in two separate areas of the network to communicate and, with the proper design, also permits roaming.

In a Wi-Fi network, roaming occurs when a station moves. It leaves the coverage area of the AP to which it was originally connected and arrives at the BSA of another AP. In a proper design scenario, a station detects the signal of the second AP and jumps to it before losing the signal of the first AP.

Because an overlap exists between the cells, it is better to ensure that the APs do not work on the same frequency (also called a channel). Otherwise, any client that stays in the overlapping area affects the communication of both cells. This problem occurs because Wi-Fi is half duplex. The problem is called co-channel interference and must be avoided by making sure that neighbor APs are set on frequencies that do not overlap.

![](<../.gitbook/assets/Unknown image (457)>)

#### Mesh BSS (MBSS)

**Mesh BSS (MBSS)** is used in situation where it's difficult to run an Ethernet connection to every AP

Multiple AP's form a mesh network which is used to bridge traffic from AP to AP connected to DS

Using one radio, each mesh AP can provide wireless coverage for client devices within its area, while backhauling traffic through the second radio. Usually, network access to users is delivered over the 2.4-GHz frequency and the 5-GHz band is used to backhaul traffic.

![](<../.gitbook/assets/Unknown image (458)>)

#### Additional AP operational modes

Repeater extends range of BSS by re-transmitting any signal from AP Two radios recommended to receive on one channel and retransmit on another channel

![](<../.gitbook/assets/Unknown image (459)>)

Universal workgroup bridge (uWGB) A single wired device can be bridged to a wireless network

Workgroup bridge (WGB) is an AP that is configured to bridge between its Ethernet and wireless interfaces.

To enable the WGB to communicate with the lightweight access point, create a WLAN and make sure that Aironet IE is enabled

is an AP that is configured to bridge between its Ethernet and wireless interfaces.

![](<../.gitbook/assets/Unknown image (460)>)

Outdoor Bridge used to connect networks over long distances without physical cable

![](<../.gitbook/assets/Unknown image (461)>)

### 802.11 frames and association

#### Association process (client ↔ AP)

**Association process** for endpoint to be able to joint the network and send traffic through AP, it must be associated with AP

Active scanning the endpoint probe requests and listens for a probe response from an AP

Passive scanning endpoint listens for beacon messages from AP which are sent periodically by APs to advertise BSS

![](<../.gitbook/assets/Unknown image (462)>)

#### Message types

**Management** used to manage the BSS. Beacon,probe,auth,assoc

**Control** used to control access to the medium. Assist with delivery of management and data frames. RTS,CTS,ACK

**Data** used to send actual data packets

#### 802.11 frame format

![](<../.gitbook/assets/Unknown image (463)>)

Frame Control provides information such as the message type and subtype

Duration/ID depending on the message type it can indicate: the time the channel will be dedicated for transmission of the frame; identifier for the association

Addresses up to 4 add can be present in .11 frame. Destination: the final recipient of the fame; Source: original sender; Receiver immediate recipient; Transmitter immediate sender

Sequence control used to reassemble fragments and eliminate duplicate frames

QoS used to prioritize certain traffic

High throughput control added in .11n to enable HT operations. .11ac is known as Very high throughput (VHT) Wi-Fi

### WLAN architectures

#### Autonomous access points

**Autonomous access points** are self-contained APs, each offering one or more standalone BSS

configured with management IP address to enable remote management

not scalable as each AP have to be configured manually and separately (or could use Cisco Prime Infrastructure)

AP has a single wired Ethernet interface that has to be trunk in case multiple WLANs are operating on the AP

AP inherently trunks 802.11 frames by marking them with the BSSID of the associated WLAN

![](<../.gitbook/assets/Unknown image (464)>)

#### Centralized (controller-based) wireless

The centralized, or lightweight, architecture allows the splitting of 802.11 functions between the controller-based AP, which processes real-time portions of the protocol, and the WLC (Wireless LAN Controller) , which manages items that are not time-sensitive. This model is also called split MAC. Split MAC is an architecture for the Control and Provisioning of Wireless Access Points (CAPWAP) protocol

AP must associate with a WLC (Wireless LAN Controller) to become fully functional

AP handles most of the real-time processes and the WLC performs the management functions

AP needs only a single IP address for the tunnel

AP also has as a single wired Ethernet interface, however WLANs are mapped within the tunnel to WLC so switchport has to be configured as access

#### CAPWAP (Control and Provisioning of Wireless Access Points)

**CAPWAP (Control and Provisioning of Wireless Access Points)** is an open protocol that enables a WLC to manage a collection of wireless APs. CAPWAP control messages are exchanged between the WLC and AP across an encrypted tunnel. CAPWAP includes the WLC discovery and join process, AP configuration and firmware push from the WLC, and statistics gathering and wireless security enforcement.

After the AP discovers the WLC, a CAPWAP tunnel is formed between the WLC and AP. This CAPWAP tunnel can be IPv4 or IPv6. CAPWAP supports only Layer 3 WLC discovery.

Once an AP joins a WLC, the APs will download any new software or configuration changes. For CAPWAP operations, any firewalls should allow the control plane (UDP port 5246) and the data plane (UDP port 5247).

Both data and control connections are part of one CAPWAP tunnel

![](<../.gitbook/assets/Unknown image (465)>)

![](<../.gitbook/assets/Unknown image (466)>)

All MAC functionality that is not real time is processed by the Cisco WLC. The APs handle only real-time MAC functionality, which includes the following:

Frame exchange handshake between client and AP when connecting to a wireless network

Frame exchange handshake between client and AP when transferring a frame

Transmission of beacon frames, which advertise all the nonhidden SSIDs

Buffering and transmission of frames for clients in a power-save operation

Providing real-time signal quality information to WLC with every received frame

Monitoring all radio channels for noise, interference, and other WLANs, and monitoring for the presence of other APs

Wireless encryption and decryption of 802.11 frames

All remaining functionality is managed in Cisco WLC, where time sensitivity is not a concern and WLC-wide visibility is required. Some of the MAC functions that are provided in the Cisco WLC are as follows:

802.11 authentication

802.11 association and reassociation (roaming)

802.11 frame translation and bridging to non-802.11 networks, such as 802.3

Radio frequency (RF) management

Security management

QoS management

#### AP operational modes (controller-based)

Local the default operational mode of APs when connected to the Cisco WLC. When an AP is operating in local mode, all user traffic is tunneled to the WLC, where VLANs are defined.. Can scan rogue APs

Monitor The AP does not transmit at all, but its receiver is enabled to act as a dedicated sensor. The AP checks for IDS events, detects rogue access points

FlexConnect Cisco wireless solution for branch and remote office deployments, to eliminate the need for WLC on each location. In FlexConnect mode, client traffic may be switched locally on the AP instead of tunneled to the WLC. Can scan rogue APs

Sniffer An AP dedicates its radios to receiving 802.11 traffic from other sources, much like a sniffer or packet capture device

Rogue detector AP detecting rogue devices that causes interference by correlating MAC addresses heard on the wired network with those heard over the air

Bridge An AP becomes a dedicated bridge (P2P or point-to-multipoint) between two networks. Two APs in bridge mode can be used to link two locations separated by a distance

Flex+Bridge FlexConnect operation is enabled on a mesh AP

SE-Connect The AP dedicates its radios to spectrum analysis on all wireless channels

Sensor mode the AP can function much like a WLAN client would associating and identifying client connectivity issues within the network in real time without requiring an IT to be on site

#### Controller placement models

Centralized WLC is usually placed in data center (DC) and Unified WLC is placed at higher hierarchy level of a site

Round-trip time (RTT) time it takes for traffic to reach WLC via CAPWAP tunnel, where is decapsulated, examined and sent to destination. Latency should be less than 100 ms, so communication can be maintained in real time. AP may disconnect and look for another more reliable WLC, if any.

![](<../.gitbook/assets/Unknown image (467)>)

Embedded WLC is co-located with access layer switch

![](<../.gitbook/assets/Unknown image (468)>)

Mobility Express WLC just one embedded WLC to manage all AP's

![](<../.gitbook/assets/Unknown image (469)>)

#### AP join state machine

Every AP and WLC must also authenticate each other with digital certificates. Cisco lightweight APs are designed to be “touch free,” and don't have to be pre-configured. You have to configure the switch port, where the AP connects, with the correct access VLAN, access mode, and inline power settings

AP states

1. AP boots: Once an AP receives power, it boots on a small IOS image so that it can work through the remaining states and communicate over its network connection.The AP must also receive an IP address from either a DHCP server or a static configuration so that it can communicate over the network
2. WLC discovery: The AP goes through a series of steps to find one or more controllers that it might join
3. CAPWAP tunnel: The AP attempts to build a CAPWAP tunnel with one or more controllers. The tunnel will provide a secure Datagram Transport Layer Security (DTLS) channel for subsequent AP-WLC control messages. The AP and WLC authenticate each other through an exchange of digital certificates.
4. WLC join: The AP selects a WLC from a list of candidates and then sends a CAPWAP Join Request message to it. The WLC replies with a CAPWAP Join Response message.
5. Download image: The WLC informs the AP of its software release. If the AP’s own software is a different release, the AP downloads a matching image from the controller, reboots to apply the new image, and then returns to step 1. If the two are running identical releases, no download is needed
6. Download config: The AP pulls configuration parameters down from the WLC. Settings include RF, SSID, security, and quality of service (QoS) parameters
7. Run state: Once the AP is fully initialized, the WLC places it in the “run” state. The AP and WLC then begin providing a BSS and begin accepting wireless clients
8. Reset: If an AP is reset by WLC, it tears down existing client associations and any CAPWAP tunnels to WLCs. The AP then reboots and starts through the entire state machine again

You can predownload the new release to the controller’s APs. APs will download the new image but will keep running the previous release > reboot to apply new release during MW

#### Discovering a WLC

1. AP sends a unicast CAPWAP Discovery Request to a controller’s IP over UDP port 5246 or a broadcast to the local subnet > WLC returns a CAPWAP Discovery Response to AP
2. AP attempts to contact as many controllers as possible to build a list of candidates
3. DHCP server can also send DHCP option 43 to suggest a list of WLC addresses to the AP // specified as a hex string. The hex string represents the IP address of the WLC
4. AP attempts to resolve the name CISCO-CAPWAP-CONTROLLER.localdomain with a DNS request (where localdomain is the domain name learned from DHCP). If the name resolves to an IP address, the AP attempts to contact a WLC at that address
5. If none of the steps has been successful, the AP resets itself and starts the discovery process all over again

#### Selecting a WLC

1. If the AP has previously joined a controller and has been configured or “primed” with a primary, secondary, and tertiary controller, it tries to join those controllers in succession
2. If the AP does not know of any candidate controller, it tries to discover one
3. The AP attempts to join the least-loaded WLC, in an effort to load balance APs (During the discovery phase, each

controller reports its load—the ratio of the number of currently joined APs to the total AP capacity)

Once WLC reaches the maximum number (defined by license/platform), it rejects an AP with the lowest priority to make room for a new one that has a higher priority (can be configured)

When there is a master controller enabled, all newly added access points with no primary, secondary, or tertiary controllers assigned associate with the master controller on the same subnet

#### Maintaining WLC availability

Keepalives are sent from the WLC to APs every 30 seconds. If a keepalive is not answered, an AP escalates the test by sending four more keepalives at 3-second intervals. If still not answered, the AP moves quickly to find a successor controller to join. WLCs also support high availability (HA) with stateful switchover (SSO) redundancy.

{% hint style="info" %}
You can configure the local router to relay broadcast CAPWAP discovery (UDP/5246) to specific controller addresses.
{% endhint %}

| router(config)# ip forward-protocol udp 5246 router(config)# interface vlan n router (config-int)# ip helper-address WLC1-MGMT-ADDR router(config-int)# ip helper-address WLC2-MGMT-ADDR |   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

#### WLC activities (RRM)

Dynamic channel assignment (DCA) The WLC can automatically choose and configure the RF channel used by AP, based on other active access points in the area

When a radio changes from one channel to another, the user gets disconnected shortly. Increase DCA to more than 10 mins (default DCA) to reduce the number of times users are disconnected

Transmit power optimization The WLC can automatically set the transmit power of each AP based on the coverage area needed

Self-healing wireless coverage If an AP radio dies, the coverage hole can be “healed” by turning up the transmit power of surrounding APs automatically

Flexible client roaming Clients can roam between APs with very fast roaming times

Dynamic client load balancing If two or more APs are positioned to cover the same geographic area, the WLC can associate clients with the least used AP

RF monitoring Remotely gather information about RF interference, noise, signals from neighboring APs, and signals from rogue APs

Security management The WLC can authenticate clients from a central service and can require wireless clients to obtain an IP address from a trusted DHCP server

### Cloud-managed WLAN (Meraki)

AP management function is managed by Internet cloud. AP management is provided through the Meraki cloud website dashboard

The Cisco Meraki cloud also adds the intelligence needed to automatically instruct each AP on which channel and transmit power level to use. It can also collect information from all of the APs about things such as RF interference, rogue or unexpected wireless devices that were overheard, and wireless usage statistics

Control plane Traffic used to control, configure, manage, and monitor the AP itself

Data plane End-user traffic passing through the AP

![](<../.gitbook/assets/Unknown image (470)>)

![](<../.gitbook/assets/Unknown image (471)>)

![](<../.gitbook/assets/Unknown image (472)>)

### Roaming

**Roaming** occurs when a wireless client device moves outside the usable range of one access point (AP) and connects to a different AP within the range

Client actively scans channels and sends probe requests to discover candidate APs, and then the client selects one and tries to reassociate with it

If AP 1 still has any leftover wireless frames destined for the client after the roam, it forwards them to AP 2 over the wired infrastructure

Association Requests are used to form a new association and join BSS

Reassociation Requests are used to roam from one AP to another, preserving the client’s original association status

![](<../.gitbook/assets/Unknown image (473)>)

#### Layer 2 roaming

no need to tunnel client data between WLCs since the clients IP subnet doesn't change

Intra-controller Roam managed by WLC (10ms)

![](<../.gitbook/assets/Unknown image (474)>)

Inter-controller Roam between WLC's

![](<../.gitbook/assets/Unknown image (475)>)

#### Layer 3 roaming

when both WLC's are using different VLAN ID and range

CAPWAP is also formed between WLCs in such situation. The first or statically preconfigured WLC1 will become an anchor for client, if he roams to WLC2 with different VLAN ID and subnet range, WLC2 will take on the foreign role and same IP from WLC1 VLAN range would still be left for the client

Mobility groups is used to cover larger area. WLC's trusts and maintaining relationship only with WLC's in the same group; Roaming is possible, but CCKM and key caching do not work

![](<../.gitbook/assets/Unknown image (476)>)

Mobility MAC cluster should be configured to set up HA in WLC mobility group

![](<../.gitbook/assets/Unknown image (477)>)

To achieve efficient roaming, both of these processes should be streamlined as much as possible

DHCP The client may be programmed to renew the DHCP lease on its IP address or to request a new address

Client authentication The controller might be configured to use an 802.1x method to authenticate each client on a WLAN

Cisco controllers offer three techniques to minimize the time and effort spent on key exchanges during roams

#### Fast roaming techniques

Cisco Centralized Key Management (CCKM) One controller maintains a database of clients and keys on behalf of its APs and provides them to other controllers and their APs as needed during client roams. CCKM requires Cisco Compatible Extensions (CCX) support from clients

Key caching Each client maintains a list of keys used with prior AP associations and presents them as it roams

802.11r This 802.11 amendment addresses fast roaming or fast BSS transition; a client can cache a portion of the authentication server’s key and present that to future APs as it roams

Adaptive R feature under .11r allows the legacy clients to connect while still allowing other clients to use fast transition

### Location services (RTLS)

AP can use the received signal strength (RSS) of a client device as a measure of the distance between the two. The components of a wireless network can be coupled with additional resources to provide real-time location services (RTLS)(important part of tracking assets in a business; eg tracking hosts around shoppping park)

As long as Wi-Fi is enabled on the device, it will probably probe for available APs on all channels that would locate not even connected device

![](<../.gitbook/assets/Unknown image (478)>)

### Multicast over WLAN

The client station sends a directed (Layer 2 unicast) frame, which requires an acknowledgement, containing multicast data (an IGMP Join) to the access point.

The access point responds with an 802.11 acknowledgement frame.

• If the acknowledgement is not received the client station resends the frame.

• • The access point forwards the IGMP Join across the network.

• • When the access point receives the requested multicast, it forwards the packets on 802.11 group (broadcast or multicast) frames which require no acknowledgement and can be consumed by all client stations.

• • When the client station is done with the multicast it sends an IGMP Leave in a directed frame.

• The access point responds with an 802.11 acknowledgement frame.

• The access point forwards the IGMP leave across the network.

• The Ethernet network IGMP process determines if multicast continues to other clients.

### Building and operating a WLAN

#### Connecting a Cisco AP

To configure and manage Cisco APs, you can connect a serial console cable from your PC to the console port on the AP. Once the AP is operational and has an IP address, you can also use Telnet or SSH to connect to its CLI over the wired network. Autonomous APs support browser-based management sessions via HTTP and HTTPS. You can manage lightweight APs from a browser session to the WLC

#### Accessing a Cisco WLC

To connect and configure a WLC, you will need to open a web browser to the WLC’s management address with either HTTP or HTTPS. This can be done only after the WLC has an initial configuration and a management IP address assigned to its management interface. Users can be authenticated against an internal list of local usernames or against AS.

When you are successfully logged in, the WLC will display a monitoring GUI dashboard. You will not be able to make any configuration changes there, so you must click on the Advanced link in the upper-right corner. This will bring up the full WLC GUI.

{% code title="Common WLC show commands" %}
```
show sysinfo
show wlan summary
show ap [uptime | summary]
show ap dot11 24ghz { summary | load-info }
show ap dot11 5ghz load-info
show wireless client vlan summary
show wireless stats ap [discovery|history|join summary]
```
{% endcode %}

#### Switch port connected to WLC (high level)

#### WLC ports

Service port: Used for out-of-band management, system recovery, and initial boot functions; always connects to a switch port in access mode

Distribution system port: Used for all normal AP and management traffic; usually connects to a switch port in 802.1Q trunk mode

Console port: Used for out-of-band management, system recovery, and initial boot functions; asynchronous connection to a terminal emulator (9600 baud, 8 data bits, 1 stop bit)

Redundancy port: Used to connect to a peer controller for high availability (HA) operation

#### WLC interface types

Management interface: Used for normal management traffic, such as RADIUS user authentication, WLC-to-WLC communication, web-based and SSH sessions, SNMP, NTP, syslog, and so on. The management interface is also used to terminate CAPWAP tunnels between the controller and its APs

Redundancy management: The management IP address of a redundant WLC that is part of a high availability pair of controllers. The active WLC uses the management interface address, while the standby WLC uses the redundancy management address

Virtual interface: IP address facing wireless clients when the controller is relaying client DHCP requests, performing client web authentication, and supporting client mobility RFC defined following address blocks: 192.0.2.0/24, 198.51.100.0/24, 203.0.113.0/24

Service port interface: Bound to the service port and used for out-of-band management. Is assigned to a management VLAN so that you can access the controller with SSH or a web browser to perform initial configuration or for maintenance Supports only a single VLAN, so the corresponding switch port must be configured for access mode only

Dynamic interface: map WLANs to VLANs. One Dynamic interface for each WLAN

RADIUS Server Overwrite interface check box to enable the per-WLAN RADIUS source support:

1.When enabled, the controller uses the interface specified on the WLAN configuration as identity and source for all RADIUS related traffic on that WLAN.

2. When disabled, the controller uses the management interface

![](<../.gitbook/assets/Unknown image (479)>)

Port towards the WLC must be trunk, port towards to access point is in access vlan for access points

LAG (Etherchannel equivalent) is configured between DS and WLC to get most out of a switchport. LACP or PaGP are not supported

![](<../.gitbook/assets/Unknown image (480)>)

Based on the switch port configuration, the AP is connected to the switch on an access port (the VLAN for AP to get DHCP). The WLC is connected to the switch on a trunk port, allowing VLANs for WLC management (VLAN 11), AP (VLAN 12), and the wireless clients (VLAN 14).

The AP and WLC create a CAPWAP tunnel.

The client associates to the AP with an SSID of "CORP."

The AP sends the client data that is marked with SSID "CORP" through the CAPWAP tunnel to the WLC.

The WLC decapsulates the CAPWAP traffic.

The SSID of "CORP" is mapped in the WLC to VLAN ID 14.

The WLC tags the data with VLAN 14 before sending it back on the trunk port (where VLAN 14 is allowed) to the switch.

The switch sends it on to the network (based on the destination in the packet).

![](<../.gitbook/assets/Unknown image (481)>)

Autonomous AP connects to a trunk port. On the trunk, a native (untagged) VLAN is required for management of the AP. By default, all VLANs are allowed over the trunk link. To enhance security, you should specify which VLANs are permitted over the trunk link, which should include the AP management VLAN.

Based on the switch port configuration, the AP is connected to the switch as a trunk port, allowing VLANs for AP management (VLAN 12) and the wireless clients (VLAN 14).

The client associates to the AP with an SSID of "CORP."

The SSID of "CORP" is mapped in the AP to VLAN ID 14.

The AP tags the data with VLAN 14 before sending it on the trunk port (where VLAN 14 is allowed) to the switch.

The switch may send it on to the network (based on the destination in packet).

![](<../.gitbook/assets/Unknown image (482)>)

### Configuring a WLAN (SSID → VLAN)

To complete the path between the SSID and the VLAN, you must first define a WLAN on the controller

Every AP must broadcast beacon management frames at regular intervals to advertise the existence of a BSS. Because each WLAN is bound to a BSS, each WLAN must be advertised with its own beacons. Beacons are normally sent 10 times per second, or once every 100 ms, at the lowest mandatory data rate. The more WLANs you have created, the more beacons you will need to announce. If you create too many WLANs, a channel can be starved of any usable airtime. Clients will have a hard time transmitting their own data because the channel is overly busy with beacon transmissions coming from the AP. As a rule of thumb, always limit the number of WLANs to five or fewer; a maximum of three WLANs is best

#### Step 1. Configure a RADIUS server

Security > AAA > RADIUS > Authentication

#### Step 2. Create a dynamic interface

Controller > Interfaces

#### Step 3. Create a new WLAN

WLAN ID is only locally significant and is not passed between controllers

#### Security

#### QoS profiles

* Platinum (voice)
* Gold (video)
* Silver (best effort)
* Bronze (background)

{% hint style="info" %}
By default, a controller blocks management sessions initiated from a WLAN.\
This prevents access to the WLC GUI/CLI from wireless clients.\
You can change this globally via **Mgmt Via Wireless**.
{% endhint %}

#### Client isolation (peer-to-peer blocking)

Peer 2 Peer blocking is a feature that blocks direct communication between wireless

clients that present on the WLAN on the same Wireless LAN Controller

#### AAA override and dynamic VLAN assignment (ISE)

Enable AAA Override and Cisco Identity Services Engine (ISE) Assign VLANs features are

often used together

Enable AAA Override is a feature that allows the AAA server to override the VLAN assignment of a user's device. This allows the AAA server, such as Cisco ISE, to assign a specific VLAN to a user's device based on the user's credentials and the policies configured on the AAA server

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

## Cloud

Cloud service providers, such as Microsoft Azure and Amazon Web Services, offer companies and individuals convenient availability of various software, platform or infrastructure services on-demand for a fee, depending on the level of usage of the service (pay-as-you-go basis), saving expenses to individuals and opex for businesses

With cloud computing, companies can leverage resources like applications, server storage and cloud computing power, so that they don't have to maintain their own hardware infrastructure

Applications running in the cloud, as well as data, are distributed across many servers in a different regions or countries, ensuring high redundancy and disaster recovery

Cloud providers have an extensive set of security policies and monitoring practices, which in general makes them more secure than many traditional data centers.

Clouds provide unlimited resources compared to a traditional data center. Therefore, it is easy to scale up and down your cloud instances and services to increase their performance.

which streamlines the overall business maintenance and provides great flexibility and scalability

### Cloud deployment models

Cloud deployment models are based on the ownership model of the cloud infrastructure:

![AWS or Private Cloud or both, what's your strategy? - Cisco Blogs](<../.gitbook/assets/AWS or Private Cloud or both, what's your strategy - Cisco Blogs>)

**Private cloud (also called Virtual Private Cloud VPC)** cloud infrastructure is completely dedicated to a particular organization; there is no public access. Usually, it is located on the organization's premises, although it may be outsourced. This allows the organization to have full control over the infrastructure, helping ensure security, privacy, and customization according to their specific needs and requirements.

**Public cloud:** A third-party provider offers cloud infrastructure that is accessible to the general public. This means that individuals or organizations can access and use the services and resources provided by the cloud provider. Typically, there may be a fee associated with using public cloud services, which can vary based on the usage and specific offerings of the provider.

**Hybrid cloud:** The cloud infrastructure is a mix of at least two (usually private and public) cloud models. In this deployment model, the public cloud is often used to supplement the resources of the private cloud. This allows organizations to leverage the scalability and cost-effectiveness of the public cloud while maintaining control over sensitive data and applications in the private cloud.

**Multi-cloud:** Multi-cloud generally refers to the consumption of cloud services from two or more public cloud providers. It also often describes specific architectures where an app uses the same service model across multiple cloud providers, in some cases including on-premises data centers and colocation facilities.

### Cloud service models

#### Infrastructure-as-a-Service (IaaS)

Instead of purchasing all necessary equipment, clients can purchase IaaS, which offers virtualized cloud-based solution, including all equipment such as storage servers, computing power or networking hardware on a pay-as-you-go basis

It provides the clients control over physical hardware and software resources by allowing them to modify storage, CPU and RAM as well as configuring network resources within the cloud platform.

**Terraform** is an Infrastructure as Code (IaC) tool developed by HashiCorp that allows you to define, provision, and manage infrastructure using a declarative configuration language called HCL (HashiCorp Configuration Language)

Infrastructure as Code – Define infrastructure in code form, making it repeatable and version-controlled.

Cloud-Agnostic – Works with AWS, Azure, Google Cloud, VMware, Kubernetes, and even on-prem infrastructure.

Declarative – You describe the desired state, and Terraform figures out how to achieve it.

State Management – Keeps track of the infrastructure state in a Terraform state file (terraform.tfstate).

Modular & Scalable – Supports modules for reusable configurations, making large-scale deployments easier.

**Terraform workflow**

Write Configuration – Define infrastructure using HCL (.tf files).

Initialize (terraform init) – Prepares Terraform by downloading necessary provider plugins.

Plan (terraform plan) – Shows a preview of changes Terraform will apply.

Apply (terraform apply) – Deploys the infrastructure.

Destroy (terraform destroy) – Removes all managed resources.

Unlike Ansible, Puppet, or Chef, which focus on configuring and maintaining routers,switches,servers, Terraform is designed primarily for provisioning infrastructure, making it a better choice for managing cloud environments.

![](<../.gitbook/assets/unknown (3).png>)

#### Platform-as-a-Service (PaaS)

**Platform-as-a-Service (PaaS)** offers customers to directly access prepared platform to develop, deploy, and manage applications without the complexity of building and maintaining the underlying infrastructure

**DaaS (Desktop as a service)** delivers cloud-based virtual desktop infrastructure (VDI) to end-users, allowing them to access working virtualized desktop with their personal device over the internet. The most popular provider is Citrix

![](<../.gitbook/assets/unknown (4).png>)

#### Software-as-a-Service (SaaS)

**Software-as-a-Service (SaaS)** cloud-based software or application is offered on a per-client or per-group subscription basis

The consumer is only provided with the application’s user interface, and applications are not installed on the user’s device

One common example of SaaS applications is Google Workplace and Microsoft Office 365 applications, in which users can access the applications like emails and Google Docs using a web browser.

![](<../.gitbook/assets/unknown (5).png>)

#### Network-as-a-Service (NaaS)

**Network-as-a-Service (NaaS)** works by applying a service-based model only to network equipment. The service provider owns, installs, and operates the hardware used by their customers, and the customer pays a monthly subscription fee for access.

NaaS can also be deployed as a cloud-based service, where routing switching and entire network infrastructure such as routers, switches and firewalls can be deployed as a virtual machines in a cloud, so that enterprise locations can have just one harware that is capable to access the internet, where their core routing and switching politics are enforced instead of having to maintain their own core site with all necessarry hardware

### How IoT devices are controlled remotely (NAT + cloud)

1\. Connecting the vacuum cleaner to the mains for the first time

After turning it on and connecting to Wi-Fi, the vacuum cleaner gets a private IP address (e.g. 192.168.1.x).

The vacuum cleaner establishes an outbound connection to the manufacturer's server (cloud), e.g. via MQTT or WebSocket.

This creates a so-called persistent connection (a long-term open socket) to the Internet.

Thanks to NAT, the router allows outgoing connections, and because the connection remains open, the server can respond back through this "channel".

2\. Mobile applications (e.g. from a mobile phone via LTE or other Wi-Fi)

The app does not communicate directly with the vacuum cleaner, because it has a private IP.

It communicates with the manufacturer's cloud, typically via HTTPS REST API.

For example, the app sends a request: "Start cleaning at 18:00".

3\. The cloud as an intermediary

The cloud server processes the request and sends it to the vacuum cleaner via an open connection.

This way, the vacuum cleaner will receive the command even if you're completely away from your home network.

If the connection drops (e.g. after restarting the router), the vacuum cleaner will re-establish it.

#### Technologies used

**MQTT (Message Queue Telemetry Transport):** a lightweight protocol common in IoT devices, ideal for push notifications.

**WebSockets:** A persistent bidirectional TCP connection.

**NAT traversal over an outbound connection:** since the connection starts from the inside, NAT allows it.

**TLS/SSL encryption:** communication security.
