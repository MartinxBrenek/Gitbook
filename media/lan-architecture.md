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

# LAN Architecture

#### Local area network (LAN)

**Local area network (LAN)** is a small structure of a multiple devices connected within a same floor or a building - geographically in the same location

LANs can vary widely in size. A LAN may consist of only two computers in a home office or small business, or it may include hundreds of computers in a large corporate office or multiple buildings. A LAN is typically a network within your own premises (your organization's campus, building, office suite, or even your home). Organizations or individuals typically build and own the whole infrastructure, all the way down to the physical cabling.

#### Wide Area Network (WAN)

**WAN** is a network that provides access to other networks over a large geographical area. WANs use facilities that an ISP or carriers, such as a telephone or cable company, provides. The provider connects locations of an organization to each other, to locations of other organizations, to external services, and remote users. WANs carry various traffic types such as voice, data, and video.

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
