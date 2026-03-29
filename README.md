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

# Fundamentals

### What is a computer network, and what is it used for?

A **computer network** is a structure of interconnection of several devices, such as computers, printers or servers, used to exchange data and files

These networks can be physically connected via cables or wirelessly. Computer networks are divided into different types, such as **local area networks (LANs)**, which connect devices in a single space or building, and **wide area networks (WANs)**, which interconnects individual LAN structures

**Internet of Things (IoT)** describes smart appliances with processing ability, sensors and ability to independently connect to the network/internet and exchange data between other devices

### Understanding what happens during communication

Let us consider a scenario in which two users, Alice and Bob, wish to communicate with each other, for instance, by chatting over their laptops. When users try to communicate with each other by using a computer, they do so by using an application. If Alice wishes to chat with Bob, she uses a chat application. The application acts as a go-between between the user and the computer network. The user does not have to know anything about how their message travels the network. The application accepts inputs from the user and transforms them. This transformation must happen because the rules of communication require that. The transformation helps ensure that the user’s input is digitalized. Digitalization means that information is represented by a series of ones and zeros or bits in a specific order. The series of bits can be very long. Each digit is 1 bit of data. So, if you have a series of 1 gigabit, this means that there are 1 billion digits in the series.

the application and computer’s operating system breaks down the large series of bits into smaller groups and prepare them for transmission over the computer network. This preparation can include adding different tags, such as priority tags, which indicate special handling requirements. Once they are correctly prepared, the user’s computer transmits them as a digital signal.

On the path to their destination, the groups of bits encounter different network devices that help them steer and navigate through the network. Most commonly, the first device is a network switch

Each group of bits can take its own path. How these paths are chosen depends on many different characteristics, such as the quality of the path, how congested it is, are there rules that determine which group is allowed where

Once the groups of bits reach the desired destination, there is one last step to reassemble the queue in the same order as it was before crossing the computer network. The reason for this is so protocols can convert bits back to original data, that is displayed by the application.

### History

Remote computer-to-computer communications were first developed for semi-automatic military radar systems and their counterparts in automatic airline reservation systems in the late 1950s. Major development took place at universities (MIT, Stanford..) in the 1960s and 1970s

In 1969, the first military computer network called **ARPANET** was launched, which was the seed of what we understand today as the Internet

Originally, it was only an experimental network that was supposed to ensure the connection of US radar stations

**NCP (Network Control Program)** was the protocol used at the time for intercommunication between the main computers of the ARPANET, allowed machines to transfer files and emails, replaced by **TCP/IP**. In 1983, a United States military network called **MILNET** was created to separate military network from the civilian - public part of the ARPANET

### Network components

![](<.gitbook/assets/Unknown image (421)>)

**Endpoints:** In the context of a network, endpoints are called end-user devices and include PCs, laptops, tablets, mobile phones, game consoles, and television sets. Endpoints are also file servers, printers, sensors, cameras, manufacturing robots, smart home components, and so on. All end devices were physical hardware units years ago. Today, many end devices are virtualized, meaning that they do not exist as separate hardware units anymore. In virtualization, one physical device is used to emulate multiple end devices—for example, all the hardware components that one end device would require. The emulated computer system operates as a separate physical unit and has its own operating system and other required software. In a way, it behaves like a tenant living inside a host physical device, using its resources (processor power, memory, and network interface capabilities) to perform its functions. Virtualization is commonly applied to servers to optimize resource utilization, because server resources are often underutilized when they are implemented as separate physical units.

**Intermediary devices:** These devices interconnect end devices or interconnect networks. In doing so, they perform different functions, which include regenerating and retransmitting signals, choosing the best paths between networks, classifying and forwarding data according to priorities, filtering traffic to allow or deny it based on security settings, and so on. As endpoints can be virtualized, so can intermediary devices or even entire networks. The concept is the same as in the endpoint virtualization—the virtualized element uses a subset of resources available at the physical host system. Intermediary devices that are commonly found in enterprise networks are:

**Switches:** These devices enable multiple endpoints such as PCs, file servers, printers, sensors, cameras, and manufacturing robots to connect to the network. Switches are used to allow devices to communicate on the same network. In general, a switch or group of interconnected switches attempt to forward messages from the sender so it is only received by the destination device. Usually, all the devices that connect to a single switch or a group of interconnected switches belong to a common network and can therefore communicate directly with each other. If an end device wants to communicate with a device that is on a different network, then it requires "services" of a device that is known as a router, which connects different networks together.

**Routers:** These devices connect networks and intelligently choose the best paths between networks. Their main function is to route traffic from one network to another. For example, you need a router to connect your office network to the internet. An analogy that may help you understand the basic function of switches and routers is to imagine a network as a neighborhood. A switch is a street that connects the houses, and routers are the crossroads of those streets. The crossroads contain helpful information such as road signs to help you in finding a destination address. Sometimes, you might need the destination after just one crossroad, but other times you might need to cross several. The same is true in networking. Data sometimes "stops" at several routers before it is delivered to the final recipient. Certain switches combine functionalities of routers and switches, and they are called **Layer 3 switches**.

**APs (access points):** APs are nodes on a wireless network that allows other wireless devices to connect to a wired network. An AP usually connects to a switch as a standalone device, but it also can be an integral component of the router itself.

**WLCs (Wireless LAN Controllers):** These centralized network devices are used by network administrators or network operations centers to facilitate the management of many APs. The WLC automatically manages the configuration of wireless APs.

**Cisco Secure Firewalls:** Firewalls are network security systems that monitor and control the incoming and outgoing network traffic based on predetermined security rules. A firewall typically establishes a barrier between a trusted, secure internal network and another outside network, such as the internet, that is assumed not to be secure or trusted.

**Intrusion Protection System (IPS):** An IPS is a system that performs a deep analysis of network traffic while searching for signs that behavior is suspicious or malicious. If the IPS detects such behavior, it can take protective action immediately. An IPS and a firewall can work in conjunction to defend a network.

**Management Services:** A modern management service offers centralized management that facilitates designing, provisioning, and applying policies across a network. It includes features for discovery and management of network inventory, management of software images, device configuration automation, network diagnostics, and policy configuration. It provides end-to-end network visibility and uses network insights to optimize the network. An example of a centralized management service is **Cisco Catalyst Center**.

In user homes, you can often find one device that provides connectivity for wired devices, connectivity for wireless devices, and provides access to the internet. You may be wondering which kind of device it is. This device has characteristics of a switch because it offers physical ports to plug local devices, a router, that enables users to access other networks and the internet, and a WLAN AP, allowing wireless devices to connect to it. It is all three of these devices in a single package. This device is often called a **wireless router**.

Another example of a network device is a **file server**, which is an end device. A file server runs software that implements standardized protocols to support file transfer from one device to another over a network. This service can be implemented by either **FTP** or **TFTP**. Having an FTP or TFTP server in a network allows uploads and downloads of files over the network. An FTP or TFTP server is often used to store backup copies of files that are important to network operation, such as operating system images and configuration files. Having those files in one place makes file management and maintenance easier.

Media are the physical elements that connect network devices. Media carry electromagnetic signals that represent data. Depending on the medium, electromagnetic signals can be guided in wires and fiber-optic cables or propagated through wireless transmissions, such as Wi-Fi, mobile, and satellite. Different media have different characteristics and selecting the most appropriate medium depends on the circumstances, such as the environment in which the media is used, distances that need to be covered, availability of financial resources, and so on. For instance, a satellite connection (air medium) might be the only available option for a filming crew working in a desert.

Connecting wired media to network devices is considerably eased by the use of **connectors**. A connector is a plug, which is attached to each end of the cable. The most common type of connector on a LAN is the plug that looks like an analog phone connector. It is called an **RJ-45 connector**.

To connect the media, which connects a device to a network, devices use **network interface cards (NICs)**. The media "plugs" directly into the NIC. NICs translate the data created by the device into a format that can be transmitted over the media. NICs used on LANs are also called LAN adapters. End devices used in LANs usually come with several types of NICs installed, such as wireless NICs and Ethernet NICs. NICs on a LAN are uniquely identified by a **MAC address**. The MAC address is hardcoded or "burned in" by the NIC manufacturer. NICs used to interface with WANs are called **WAN interface cards (WICs)**, and they use serial links to connect to a WAN network.

### Network management

It includes tasks like:

* Planning and designing the network architecture to meet organizational requirements and goals.
* Setting up and configuring routers, switches, and firewalls to ensure performance and security.
* Implementing monitoring to track traffic, health, and performance.
* Managing resources like bandwidth, IP addresses, and server capacity.
* Maintaining documentation of network configurations, topology, policies, and procedures.

### Common network applications

* World Wide Web (WWW): websites accessed on the internet via browsers.
* E-mail: Outlook, Gmail, etc.
* Data storage: cloud services.
* Instant messaging and conferencing: Webex, Teams, Messenger.
* Voice over IP (VoIP).

Fax "Facsimile" is a technology that allows the transmission of printed or handwritten documents over a telephone line. It converts these documents into electronic signals that can be sent to another fax machine, where they are then printed out as a physical copy. Fax machines have been used for many years to transmit documents over long distances, and they were especially popular before the widespread use of email and digital document transmission. While faxing is less common today, it is still used in some industries where physical signatures or paper documents are required

#### Real-world questions this stuff answers

* How does Netflix stream movies so smoothly without buffering, even during peak hours?
* How do submarines communicate underwater without traditional signals?
* How does your phone stay connected at 300 km/h on a train?
* How do Tesla cars ‘see’ the world in real-time and make instant decisions?
* How do we communicate with Mars across millions of kilometers?
* How do F1 racing teams analyze car performance in real-time at breakneck speeds?
* How do trading platforms like Robinhood display real-time stock prices?

Networking isn’t just about routers, switches, firewalls, load balancers and cables – it’s the backbone of modern technology and touches every corner of our lives

### Network services

Services in a network comprise software and processes that implement common network applications, such as email and web, including the less obvious processes implemented across the network. These generate data and determine how data is moved through the network.

Companies typically centralize business-critical data and applications into central locations called **data centers**. These data centers can include routers, switches, firewalls, storage systems, servers, and application delivery controllers. Similar to data center centralization, computing resources can also be centralized off-premises in the form of a **cloud**. Clouds can be **private**, **public**, or **hybrid**, and they aggregate the computing, storage, network, and application resources in central locations. Cloud computing resources are configurable and shared among many end users. The resources are transparently available, regardless of the user's point of entry (a personal computer at home, an office computer at work, a smartphone or tablet, or a computer on a school campus). Data stored by the user is available whenever the user is connected to the cloud.

### Key network characteristics

When you purchase a mobile phone or a PC, the specifications list tells you the important characteristics of the device, just as specific characteristics of a network help describe its performance and structure. When you understand what each characteristic of a network means, you can better understand how the network is designed, how it performs, and which aspects you may need to adjust to meet user expectations.

You can describe the qualities and features of a network by considering these characteristics:

* **Topology:** A network topology is the arrangement of its elements. Topologies give insight into physical connections and data flows among devices. In a carefully designed network, data flows are optimized, and the network performs as desired.
* **Bitrate (bandwidth):** Bitrate measures the data rate in bits per second (bps) of a given link in the network. This measure is often referred to as bandwidth or speed in device configurations, which is sometimes thought of as speed. However, it is not about how fast 1 bit is transmitted over a link—which is determined by the physical properties of the medium that propagates the signal—it is about the number of bits transmitted in a second. Link bitrates commonly encountered today are 1 and 10 gigabits per second (1 or 10 billion bits per second). 100-Gbps links are also not uncommon.
* **Availability:** Availability indicates how much time a network is accessible and operational. Availability is expressed in terms of the percentage of time the network is operational. This percentage is calculated as a ratio of the time in minutes that the network is available and the total number of minutes over an agreed period, multiplied by 100. In other words, availability is the ratio of uptime and total time, expressed in percentage. To help ensure high availability, networks should be designed to limit the impact of failures and to allow quick recovery when a failure does occur. High availability design usually incorporates redundancy. The redundant design includes extra elements, which serve as backups to the primary elements and take over the functionality if the primary element fails. Examples include redundant links, components, and devices.
* **Reliability:** Reliability indicates how well the network operates. It considers the ability of a network to operate without failures and with the intended performance for a specified time period. In other words, it tells you how much you can count on the network to operate as you expect it to. For a network to be reliable, the reliability of all its components should be considered. Highly reliable networks are highly available, but a highly available network might not be highly reliable—its components might operate at lower performance levels. A standard measure of reliability is the mean time between failures (MTBF), which is calculated as the ratio between the total time in service and the number of failures. Not meeting the required performance level is considered a failure. Choosing highly reliable redundant components in the network design increases both availability and reliability.

For instance, let’s consider a networking device that reboots every hour. The reboot takes 5 minutes, after which the device works as expected. The figure shows the calculations of availability and reliability.

The availability percentage for one day can be calculated as follows:

![](<.gitbook/assets/Unknown image (422)>)

* **Scalability:** Scalability indicates how easily the network can accommodate more users and data transmission requirements without affecting current network performance. If you design and optimize a network only for the current conditions, it can be costly and difficult to meet new needs when the network grows.
* **Security:** Security tells you how well the network is defended from potential threats. Both network infrastructure and the information that is transmitted over the network should be secured. The subject of security is important, and defense techniques and practices are constantly evolving. You should consider security whenever you take actions that affect the network.
* **Quality of Service (QoS):** QoS includes tools, mechanisms, and architectures, which allow you to control how and when applications use network resources. QoS is essential for prioritizing traffic when the network is congested.
* **Cost:** Cost indicates the general expense for the initial purchase of the network components and any costs associated with installing and maintaining these components.
* **Virtualization:** Traditionally, network services and functions have only been provided via hardware. Network virtualization creates a software solution that emulates network services and functions. Virtualization solves many of the networking challenges in today’s networks, helping organizations centrally automate and provision the network from a central management point.

### How applications impact the network

Furthermore, when we talk about larger networks managed by various multinational companies or service providers, the price of individual network elements is also important

To classify applications, their traffic, and performance the requirements are described in terms of these characteristics:

**Interactivity:** Applications can be interactive or noninteractive. Interactivity presumes that a response is expected for the normal functioning of the application for a given request. For interactive applications, it is important to evaluate how sensitive they are to delays—some might tolerate larger delays up to practical limits, but some might not.

**Real-time responsiveness:** Real-time applications expect a timely data serving, and they are not necessarily interactive. An example of a real-time application is like a football game video streaming (live streaming) or video conferencing. Real-time applications are sensitive to delay. Delay is sometimes used interchangeably with the term **latency**. Latency refers to the total amount of time from the source sending data to the destination receiving it. Latency accounts for the propagation delay of signals through media, the time required for data processing on devices it crosses along the path, etc. Because of the changing network conditions, latency might vary during data exchange: some data might arrive with less latency than other data. The variation in latency is called **jitter**.

**Amount of data generated:** Some applications produce a low quantity of data, such as voice applications. These applications do not require much bandwidth, and they are usually referred to as benign bandwidth applications. Video streaming applications produce a significant amount of traffic and are termed bandwidth greedy.

**Burstiness:** Applications that always generate a consistent amount of data are referred to as smooth or nonbursty applications. On the other hand, bursty applications at times create a small amount of data, but they can change behavior for shorter periods. An example, is web browsing. If you open a page in a browser that contains a lot of texts, a small amount of data is transferred. But, if you start downloading a huge file, the amount of data will increase during the download.

**Drop sensitivity:** Packet loss is losing packets along the data path, which can severely degrade the application performance. Some real-time applications (such as video on demand) are sensitive to the perceived packet loss when using the network resources. You can say that such applications are drop-sensitive.

**Criticality to business:** This aspect of an application is "subjective" in that it depends on someone's estimate of how valuable and important the application is to a business. For instance, an enterprise that relies on video surveillance to secure its premises might consider video traffic as a top priority, while another enterprise might consider it totally irrelevant.

**Batch applications:** Applications such as FTP and TFTP are considered batch applications. Both are used to send and receive files. Typically, a user selects a group of files that need to be retrieved and then starts the transfer. Once the download starts, no additional human interaction is required. The amount of available bandwidth determines the speed at which the download occurs. While bandwidth is important for batch applications, it is not critical. Even with low bandwidth, the download is completed eventually. Their principal characteristics are:

Typically do not require direct human interaction.

Bandwidth is important but not critical.

Examples: FTP, TFTP, inventory updates.

**Interactive applications:** Applications in which the user waits for a response to their action are interactive. Think of online shopping applications, which are offered by many retail businesses today. Interactive applications require human interaction, and their response times are more important than for batch applications. However, strict response times or bandwidth guarantees might not be required. If the appropriate amount of bandwidth is not available, then the transaction may take longer, but it will eventually complete. The main characteristics of the interactive applications are:

Typically support human-to-machine interaction.

Acceptable response times have different values depending on how important the application is for the business.

Examples: database inquiry, stock exchange transaction

**Real-time applications:** Are applications such as voice and video that may also involve human interaction. Because of the amount of information that is transmitted, bandwidth is critical. In addition, because these applications are time-critical, a delay on the network can cause a problem. Timely delivery of the data is crucial. It is also important that data is not lost during transmission because real-time applications, unlike other applications, do not retransmit lost data. Therefore, sufficient bandwidth is mandatory, and the quality of the transmission must be ensured by implementing **QoS**. QoS is a way of granting higher priority to certain types of data, such as **VoIP**. The main characteristics of the real-time applications are:

Typically support human-to-human interaction.

End-to-end latency is critical.

Examples: Voice applications, video conferencing, and online streaming such as live sports.

Applications may be required to manage different types of communications. One such application is the factory-automation application. Factory-automation applications deal with plant process-related data, such as readings from sensors and alarms, which require guaranteed delivery times and typically require feedback within a prescribed response time. On the other hand, the same factory-automation application must also handle certain device configurations and commercial data, which is not time-critical.

### Standards bodies and organizations

#### IEEE (Institute of Electrical and Electronics Engineers)

is a leading global organization focused on advancing technology and innovation in various fields, including electrical engineering, electronics, telecommunications, and computer science, through standards and protocols development

Protocols are a detailed set of rules that govern successful network communication. The rules define various situations, methods, and behaviors that every communicating device should follow. The examples of specifications that protocols include are the voltage to use for an electrical signal, which messages are allowed in communication, what are the building blocks of the messages, what is to be done if a message is lost, and what should be done if the message contains an error.

The exchange of data within the internet follows the same well-defined protocol rules, designed specifically for internet communication. Protocols define how data is transmitted between devices in networks and how it allows the devices to communicate with each other. The analogy to communication between two devices would be two people talking the same language. Protocols are typically created according to the industry standard by various networking or information technology organizations. The internet is a base for various data exchange services, such as email or file transfers. It is a common global infrastructure, composed of many computer networks connected that follow communication rules standardized for the internet. A set of documents called RFCs defines the protocols and processes of the internet.

Request for Comments (RFC) is a document that is written by the technical and standards-setting bodies for the internet. In the following examples, you will browse through some of the RFCs that you can find at [https://www.ietf.org/standards/rfcs/](https://www.ietf.org/standards/rfcs/). For this task, you do not require any technical knowledge. The objective of the task is to browse through the RFC, get to know its structure, and search for information within it.

The number 802 represents the year and month when Ethernet was developed - 1980 Feb (February)

**Well-known network-related standards**

IEEE 802.1: Higher Layer LAN Protocols

802.1Q: Virtual LANs (VLAN)

802.1X: Port-based Network Access Control

IEEE 802.3: Ethernet

802.3: Ethernet standard

802.3ae: 10 Gigabit Ethernet

802.3af: Power over Ethernet (PoE)

802.3at: Power over Ethernet Plus (PoE+)

IEEE 802.11: Wireless LAN (Wi-Fi)

802.11a: High-speed Wireless LAN at 5 GHz

802.11b: Wireless LAN at 2.4 GHz

802.11g: Higher-speed Wireless LAN at 2.4 GHz

802.11n: Multiple Input Multiple Output (MIMO) Wireless LAN

802.11ac: Very High Throughput (VHT) Wireless LAN

802.11ax: High-Efficiency Wireless (HEW)

#### ISO (International Organization for Standardization)

* Brings together national standardization institutions (e.g. ANSI, ETSI, DIN)
* From the point of view of data networks, its most famous creation is the so-called OSI reference model
* ISO standards are not freely available (compared to e.g. RFC documents)

#### ITU (International Telecommunication Union)

International Telecommunication Union under the United Nations

* Issue operating procedures, technical specifications or manuals and guidelines
* Has several working groups (ITU-T, ITU-R, ITU-D)

#### IETF (The Internet Engineering Task Force)

An open international community dedicated to the development of Internet technologies

Proposals and standards are published as RFCs (Request for Comments)

### Why standards matter

Communication can be described as successful sharing or exchanging of information. It involves a source and a destination of the information. Information is represented in some form of message. In computer networks, the sources of messages are end devices, also called endpoints or hosts. The messages are created at the source, transferred over the network, and delivered to the destination. For communication to be successful, the message has to traverse one or more networks. A network interconnects a large number of devices produced by different hardware and software manufacturers over many different transmission media, each one having its specifics. All these parameters make the network very complex.

Computer networks were initially concerned only with the transfer of data. The term data referred to information in an electronic form that could be stored and processed by a computer. Additionally, different data transfer protocols required completely different network topologies, equipment, and interconnections. IP, AppleTalk, Token Ring, and FDDI are examples of data transfer communications protocols that required different hardware, topologies, and equipment to operate properly. In addition to data transfer, other communication networks existed in parallel. For example, telephone networks were built using separate equipment and implemented different protocols and standards. Over the years, computer networking evolved such that IP became a common data communications standard. The technology has also been extended to include other types of communication, such as voice conversations and video

The need to interconnect devices is not exclusive to computer networks. Industrial manufacturing companies used standards and protocols specifically designed to provide automation and control over the production process. The management and monitoring of the manufacturing plant were traditionally the task of the Operational Technology (OT) departments. IT departments, which manage business applications, and OT departments functioned independently. Today, thanks to the industrial Internet of Things (IoT), manufacturers are collecting more data from the plant floor than ever before. However, that data is only as valuable as the decisions it can support. OT and IT departments collaborate to make the data meaningful and accessible for use across the organization.

The result is another example of a converged network, called Factory Network, which connects factory automation and control systems with IT systems using standards-based networking. The Factory Network provides real-time access to mission-critical data at the plant level while sharing knowledge throughout the enterprise, helping operations leaders make decisions that can contribute to safety and operational effectiveness.

### OSI model (ISO/OSI)

ISO is designed as a reference model that divided network communication into several parts, which helped people understand the overall structure of network communication and troubleshoot issues. It was developed by the ISO (International Organization for Standardization) as a theoretical model of communication.

Standards-based, layered models provide several benefits:

Make complexity manageable by breaking communication tasks into smaller, simpler functional groups.

Define and specify communication tasks to provide the same basis for everyone to develop their own solutions.

Facilitate modular engineering, allowing different types of network hardware and software to communicate with one another.

Prevent changes in one layer from affecting the other layers.

Accelerate evolution, providing for effective updates and improvements to individual components without affecting other components or having to rewrite the entire protocol.

Simplify teaching and learning.

To address the issues with network interoperability, the ISO researched different communication systems. As a result of this research, the ISO created the ISO OSI model to serve as a framework on which a suite of protocols can be built. The vision was that this set of protocols would be used to develop an international network that would not depend on proprietary systems. In the computer industry, proprietary means that one company or a small group of companies uses their own interpretation of tasks and processes to implement networking. Usually, the interpretation is not shared with others, so their solutions are not compatible; hence they do not communicate. Meanwhile, the TCP/IP protocol suite was used in the first network implementations. It quickly became a standard, meaning that it was the protocol suite implemented in practice. Consequently, it was chosen over the OSI protocol suite and became the standard in network implementations today.

ISO, the International Organization for Standardization, is an independent, nongovernmental organization. It is the world's largest developer of voluntary international standards. Those standards help businesses to increase productivity while reducing errors and waste.

Each one of us subconsciously follows the OSI model when solving a connection failure - when a webpage that you try to access on the internet is not loading, you first try if the other applications load or not, then you proceed to check if the lights on the modem are on, if yes, then you proceed to check if the cables are connected, or you if your device has an IP address and can ping the destination web address. The same approach network engineers use when solving network issues

![](<.gitbook/assets/Unknown image (423)>)

Roughly, the model layers can be grouped into upper and lower layers. Layers 5 to 7, or upper layers, are concerned with user interaction and the information that is communicated, its presentation, and how the communication proceeds. Layers 1 to 4, the lower layers, are concerned with how this content is transferred over the network.

#### Layer 1: Physical

The physical layer defines electrical, mechanical, procedural, and functional specifications for activating, maintaining, and de-activating the physical link between devices. This layer deals with the electromagnetic representation of bits of data and their transmission. The physical layer specifications define line encoding, voltage levels, the timing of voltage changes, physical data rates, maximum transmission distances, physical connectors, and other attributes. This layer is the only layer implemented solely in hardware.

Data are transmitted in bits through medium in the form of a signal, either electrical or light, which is transmitted through a cable, or a wireless signal, which is sent through the air using electromagnetic waves

Equipment operating on this layer are Cables, Mechanical connections or HUB. L1 PDU: Bits (0 and 1 that are transmitted in the form of signal)

#### Layer 2: Data link

The data link layer defines how data is formatted for transmission and controlled access to physical media. This layer typically includes error detection and correction to help ensure reliable data delivery. The data link layer involves network interface controller to network interface controller (NIC-to-NIC) communication within the same network or subnet. This layer uses a physical address, sometimes called a MAC address, to identify hosts on the local network - mediates the connection between directly connected devices

Device operating on this layer is called switch. L2 PDU: Frames Example protocols: Ethernet, 802.11 (Wi-Fi)

#### Layer 3: Network

The network layer provides connectivity and path selection beyond the local segment, all the way from the source to the final destination. The network layer uses logical addressing to manage connectivity. In networking, the logical address is used to identify the sender and the recipient. The postal system is another common system that uses addressing to identify the sender and the recipient. Postal addresses follow the format that includes name, street name, and number, city, state, and country. Network logical addresses have a different format than postal addresses; they are determined by the network layer rules. Logical addressing helps ensure that a host has a unique address or that it can be uniquely identified in terms of network communication. Provides hierarchical network addressing, routing and forwarding of datagrams between networks based on the network address

Device operating on this layer is called Router; L3 PDU: Packets Example protocols: IP, ICMP

#### Layer 4: Transport

The transport layer defines the segmenting and reassembling of data belonging to multiple individual communications, defines the flow control, and defines the mechanisms for reliable transport if required. The transport layer serves the upper layers, which in turn interface with many user applications. To distinguish between these application processes, the transport layer uses its own addressing. This addressing is valid locally, within one host, unlike addressing at the network layer. The transport services can be reliable or unreliable. The selection of the appropriate service depends on application requirements. For instance, the file transfer may be reliable to guarantee that the file arrives intact and whole. On the other hand, a missing pixel when watching a video might go unnoticed. In networking, this is called an unreliable service.

Example protocols: TCP,UDP. L4 PDU: Segment

Device operating on this layer are for example Firewalls, as they can inspect the transport protocol used to determine how to process the packet

#### Layer 5: Session

A session is the period during which two devices exchange data

Session Layer controls dialog between applications and ensures the checkpoint, transaction sync and correct termination of files

Remote Procedure Call (RPC) protocol is used to provide session layer functionality. Also SIP and TCP can be considered session layer protocol

#### Layer 6: Presentation

translates data between a networking service and an application; including character encoding, data compression

The layer is concerned only with the structure of the data, but not with its meaning, which is known only to the application layer

Serialization of complex data structures into flat byte-strings using mechanisms such as TLV, XML or JSON can be thought of as the key functionality of the presentation layer.

Lightweight Presentation Protocol (LPP) Network Data Representation (NDR)

#### Layer 7: Application

provides an interface between software running on a computer and the network itself. and enables user-level communication and interaction with the network

The application layer does not define the application itself, but it defines services that application should follow in order to move data between systems

For example HTTP defines how web browsers can download web contents from a web server

For communication to be successful, the application layer protocols that are implemented on the source and target devices must be compatible with each other

Application protocols always use one of two basic transport layer services: TCP or UDP

### TCP/IP model

The TCP/IP model represents a protocol suite. It is similar to the ISO OSI model in that it uses layers to organize protocols and explain which functions they perform. TCP/IP protocols are actively used in actual networks today.

The TCP/IP model defines and describes requirements for the implementation of host systems. These include standard protocols that these systems should use. It does not specify how to implement the protocol functions but rather provides guidance for vendors, implementors, and users of what should be provided within the system.

The TCP/IP protocol suite has four layers and includes many protocols, although its name stands for only two: TCP, which stands for the Transmission Control Protocol, and IP, which stands for the Internet Protocol. The reason is that layers represented by these two protocols carry out functions crucial to successful network communication.

Since the TCP/IP protocols provided the functionality of several layers, it provided new simplified structure merged the top three OSI layers into one as well as the first two lower layers, since Ethernet ensured both physical and data-link layer functions

Because the functions of each OSI layer are clearly defined, the OSI layers are used when referring to devices and protocols.

Take, for example, a “Layer 2 switch,” which is a LAN switch. The “Layer 2” in this case refers to the OSI Layer 2, making it easy for people to know what is meant, as they associate the OSI Layer 2 with a clearly defined set of functions.

Similarly, it is often said that IP is a “network layer protocol” or a “Layer 3 protocol” as the TCP/IP’s internet layer can be matched to the OSI network layer.

It is very important to remember that the OSI model terminology and layer numbers are often used rather than the TCP/IP model terminology and layer numbers when referring to devices and protocols.

![](<.gitbook/assets/Unknown image (424)>)

#### Link layer

This layer is also known as the media access layer. It defines protocols used to interface the directly connected network. Tasks of the protocols at this layer are closely related to the characteristics of the physical medium and deal primarily with physical network details. The link layer is also referred to as a network interface, network access, or even data link layer. Because there are many different types of physical networks, there are many link layer protocols. An example of the TCP/IP link layer protocol is Ethernet. The link layer introduces physical addresses, sometimes called hardware addresses or MAC addresses, to identify devices sharing a particular physical network segment.

#### Internet layer

This layer routes data from the source to the destination, provides a means to obtain information on reaching other networks and deals with reporting errors. The Internet layer provides logical addressing. Logical addressing helps ensure that a host is uniquely identified. An Internet layer logical address, called an IP address, is used to identify a host. This address is valid globally and aims at uniquely identifying the host. End devices, such as laptops, mobile phones, and servers are configured with a logical address before connecting to the network. IP protocols—namely, IPv4 and the newer version, IPv6—reside in this layer. This layer serves the upper transport layer and passes information to the Link layer.

#### Transport layer

This layer is the core of the TCP/IP architecture and the Internet layer. It is placed between "data mover" protocols of the link and internet layers and software-oriented protocols of the application layer. There are two main protocols at this layer, TCP and UDP. These protocols serve many application-layer protocols. Transport services "prepare" application data for transfer over the network, follow the transfer process, and ensure that data from different applications is not mixed. To distinguish between the applications, the transport layer identifies each application with its own addressing. This addressing is valid locally, within one host, unlike addressing at the Internet layer, which is valid globally.

#### Application layer

The functions of this layer mainly deal with user interaction. It supports user applications by providing protocols and services that let you actually use the network. It also supports network application programming interfaces (APIs) that allow programs to access the network services, regardless of the operating system that they are running on. This layer accommodates protocols such as HTTP, HTTPS, Domain Name System (DNS), FTP, Simple Mail Transfer Protocol (SMTP), Secure Shell (SSH), and many more. These protocols facilitate applications for web browsing, file transfer, names to IP addresses resolution, sending of emails, remote access to devices, and many other functions that network users perform.

### Peer-to-peer communications

The term peer means the equal of a person or object. By analogy, **peer-to-peer** communication means communication between equals. This concept is at the core of layered modeling of a communication process. Although a layer deals with layers directly above and below it in performing its functions, the data it creates is intended for the corresponding layer at the receiving host. The concept is also called the horizontal communication.

The key idea is that even though your letter goes through many steps and different hands (layers) to get to the recipient, the message itself is only truly "understood" by you and the recipient.

In that context, **peer-to-peer (P2P)** means that all computers in a network are equal and can both request and provide services. Think of file-sharing networks where every computer can download and upload files.

**Client-server** is different: one computer (the client) requests services from another computer (the server), which provides them. Think of Browse a website, where your computer is the client and the website's computer is the server.

### Encapsulation and decapsulation process

to be able to forward data over the network as a signal of ones and zeros or high or low current, by stamping light or using electromagnetic waves, so it must be divided into bits, then each layer of the TCP/IP model attaches its information to the data and thus **encapsulates** it and forwards it to the lower layer, and so do all TCP/IP layers until the data is finally forwarded as a signal through the physical layer.

The reverse process is performed on the receiver, where the layers **decapsulate** the data together with information that stores information about the type of transfer and for whom and what application the data is intended

As each layer adds its information to the data, the team creates a data unit called a **PDU (Protocol Data Unit)**

For example, a PDU at the transport layer is composed of a header of transport layer information and data, forming a PDU called a **segment**

Next, the network layer adds its header with the information necessary for the transmission and thus creates a PDU called a **packet**

The link layer also adds its header with information to create a **frame**

And it is eventually sent over the physical medium which has a PDU called **bits**, since it only carries ones and zeros

#### Process

Step 1. Create and encapsulate the application data with any required application layer headers

For example, the HTTP OK message can be returned in an HTTP header, followed by part of the contents of a web page.

Step 2. Encapsulate the data supplied by the application layer inside a transport layer header to be transported

Step 3. Encapsulate the data supplied by the transport layer inside a network layer (IP) header. IP defines the IP addresses that uniquely identify each computer.

Step 4. Encapsulate the data supplied by the network layer inside a data-link layer header and trailer. This layer uses both a header and a trailer.

Step 5. Transmit the bits. The physical layer encodes a signal onto the medium to transmit the frame.

At the destination, each layer looks at the information in the header added by its counterpart layer at the source. Based on this information, each layer performs its functions and removes the header before passing it up the stack. This process is equivalent to unpacking a box. In networking, this process is called **de-encapsulation**.

The de-encapsulation process is like reading the address on a package to see if it is addressed to you and then, if you are the recipient, opening the package and removing the contents of the package.

The following is an example of how the destination device de-encapsulates a sequence of bits:

The link layer reads the whole frame and looks at both the frame header and the trailer to check if the data has any errors. Typically, if an error is detected, the frame is discarded, and other layers may ask for the data to be retransmitted. If the data has no errors, the link layer reads and interprets the information in the frame header. The frame header contains information relevant for further processing, such as the type of encapsulated protocol. If the frame header information indicates that the frame should be passed to upper layers, the link layer strips the frame header and trailer and then passes the remaining data up to the Internet layer to the appropriate protocol.

The internet layer examines the internet header in the packet received from the link layer. Based on the information it finds in the header, it decides either to process the packet at the same layer or to pass it up to the transport layer. Before the internet layer passes the message to the appropriate protocol on the transport layer, it first removes the packet header.

The transport layer examines the segment header of the received segment. The information included in the segment header indicates which application layer protocol should receive the data. The transport layer strips the segment header from the segment and hands over data to the appropriate application layer protocol.

The application layer protocol strips the data header. It uses the information in the header to process the data before passing it to the user application.

![](<.gitbook/assets/Unknown image (425)>)

Not all devices process PDUs at all layers. For instance, a switch might only process a PDU at the link layer, meaning that it will “read” only frame information that is contained in the frame header and trailer. Based on the information found in the frame header and trailer, the switch will either forward the frame unchanged out of a specific port, forward it out all ports except for the incoming port, or discard the frame if it detects errors. Routers might look "deeper" into the PDU. A router de-encapsulates the frame header and trailer and relies on the information contained in the packet header to make their forwarding decisions. If the router is filtering the packets, it may also look even deeper, into the information contained in the segment header before it decides on what to do with the packet.

A host performs encapsulation as it sends data and performs de-encapsulation as it receives it; it can perform both functions simultaneously as part of multiple communications it maintains.

PCs and all network devices such as routers and switches are capable of examining both the IP and Ethernet headers when they receive a packet.

When a packet is received by a network interface (NIC) on a PC, it goes through several layers of processing within the networking stack

These layers typically include the Data Link layer (where Ethernet headers are processed) and the Network layer (where IP headers are processed)

While a NIC does perform some functions similar to those of a router and switch (such as examining packet headers and making forwarding decisions), its scope and capabilities are more limited and focused on a communication within a single device

### Ethernet

**Ethernet** is a family of networking technologies used in LAN (Local Area Networks), originally developed in the 1970s by Xerox and later standardized by IEEE. It's known for its frame-based data transmission, MAC addressing, and support for various physical media (copper, fiber, etc.).

IEEE 802.3 – This is the official standard for Ethernet.

Not all IEEE 802 standards are Ethernet, but many are Ethernet-related extensions or complementary (e.g., VLANs in 802.1Q)

### Numbering systems

#### Positional

The key characteristic of **positional** systems is their foundation/base. This is usually a natural number greater than one.

The weights of each digit are then powers of this base. At the same time, the base determines the number of symbols for the digits used in the system.

Examples are Decimal, Binary, Hexadecimal or Roman numerals (V, IV VI, VII etc…)

#### Non-positional

**Non-positional** is an obsolete and unused way of representing numbers in which the value of a digit is not determined by its position in a given sequence of digits.

Examples are simple marks to indicate the number (IlII lll lll lllll llllll)

#### Decimal

**Decimal** is base-10 numbering system that we use in everyday life representing 10 digits (0, 1, 2, 3, 4, 5, 6, 7, 8 a 9 - zero included)

Each digit/position representing power of 10 (10^x) starting with 10^0 to represent units/base then next position is power of the base - 10^1 next position is 10^2 and so on..

For example, a decimal number 27398 represents the sum (2 x 10,000) + (7 x 1000) + (3 x 100) + (9 x 10) + (8 x 1). If you write this with exponents the sum would look like: (2 x 104) + (7 x 103) + (3 x 102) + (9 x 101) + (8 x 100).

For example, the number 123 in decimal represents (1 \* 10^2) + (2 \* 10^1) + (3 \* 10^0), which is 100 + 20 + 3.

#### Binary

**Binary** is base-2 numbering system used in IT systems, representing only two digits, 0 and 1. Each binary digit is called **bit** (more related is **byte** that consists of 8 bits)

Each digit's position represents a power of 2 (2^x). For example, the binary number 101 represents (1 \* 2^2) + (0 \* 2^1) + (1 \* 2^0), which is 4 + 0 + 1.

Note The binary representation and multiples of 2 are not the same thing. When multiplying, we start with 2, but in binary representation, counting starts from 1

Binary system is the most efficient way to represent information in digital devices, which operate with two basic states: on (1) and off (0), which is easily implemented using electronic circuits. This enables compact data storage and fast processing

Computers use binary to transmit and store data (letters and characters), because they rely on micro transistors that can be either on or off, and binary aligns with this physical limitation

The binary system eliminates ambiguity and errors in data representation. Each bit has a clearly defined value (0 or 1) that cannot be changed or interpreted differently.

This ensures the integrity and reliability of the stored information.

When all bits in a binary number are set to 1, it yields the maximum value for that number of bits

For example, after the sum of a 8-bit (1byte) binary number, when all bits are 1 (11111111) gives value of 255 (128+64+32+16+8+4+2+1)

**Binary-to-decimal conversion**

Start by making a table with all of the 2exponent values listed, for exponent values 0 through 7, as shown in the first row of the following table.

Add a row that lists the decimal value of each of these exponents, as shown in the second row; these are the positional or place values (and are also called placeholders).

Write out the given bit sequence in the table, as shown in the third row for the example binary number 10111001.

For each bit, multiply the place value by the bit value, as shown in the fourth row. Notice that where the bit value is 0, the answer is 0, and where the bit value is 1, the answer is the place value.

Finally, add all of these values together; the result is the decimal value of the binary number. In this example, the decimal value of the binary number 10111001 is 185.

Note In mathematics, the rule is that any number raised to the zeroth power is equal to one. This rule follows from the definition of power and from mathematical consistency

![](<.gitbook/assets/Unknown image (426)>)

**Decimal-to-binary conversion**

The process of converting a decimal number to a binary number can be simplified by using a table. The table method utilizes elementary mathematics like addition and subtraction. This process is simple and effective. With a bit of practice, you will learn it quickly.

When converting from decimal into binary, the idea is to find the right sequence of bits by marking placeholders as 1 or 0. All bits are represented, and each placeholder marked with 1 adds its value to the converted number, while 0s are ignored. For example, 255 is represented by marking all placeholders with 1, meaning that summing up each placeholder value produces the decimal number: 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 = 255.

The process of converting a decimal number into binary is done by marking the closest (lower) placeholder as 1 and subtracting the corresponding value from the decimal number until there is no remainder. Any unused or skipped placeholders are marked as 0. The binary representation of the decimal number is the 1 and 0 sequence that is produced.

Procedure for Converting a Decimal Number to a Binary Number

| Step | Action                                                                                                                                                                                                                                                               |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.   | Start by making a table with all of the 2exponent values listed, for exponent values 0 through 7, as shown in the first row of the table. Add a row that lists the decimal value of each of these exponents, as shown in the second row; these are the place values. |
| 2.   | Looking at the table columns, what is the greatest power of 2 that is less than or equal to 147? 128 is less than or equal to 147, so place a 1 in the 27 = 128 column.                                                                                              |
| 3.   | Calculate how much is left over by subtracting 128 from 147. The result is 19.                                                                                                                                                                                       |
| 4.   | Now look for the next greatest power of 2 that is less than or equal to 19. The next place value is 64, which is not less than or equal to 19, so place a 0 in that column.                                                                                          |
| 5.   | The next place value is 32, which again is not less than or equal to 19, so place a 0 in that column as well.                                                                                                                                                        |
| 6.   | The next place value is 16, which is less than or equal to 19. Place a 1 in that column. Calculate how much is left over by subtracting 16 from 19. The result is 3.                                                                                                 |
| 7.   | Now look for the next greatest power of 2 that is less than or equal to 3. The next place value is 8, which is not less than or equal to 3, so place a 0 in that column.                                                                                             |
| 8.   | The next place value is 4, which is not less than or equal to 3, so place a 0 in that column too.                                                                                                                                                                    |
| 9.   | The next place value is 2, which is less than or equal to 3. Place a 1 in that column. Calculate how much is left over by subtracting 2 from 3. The result is 1.                                                                                                     |
| 10.  | Now look for the next greatest power of 2 that is less than or equal to 1. The next place value (which is also the final place value) is 1, which is equal to 1, so place a 1 in the last column.                                                                    |
| 11.  | The binary equivalent of the decimal number 147 is 10010011.                                                                                                                                                                                                         |

![](<.gitbook/assets/Unknown image (427)>)

#### Hexadecimal

**Hexadecimal** is base-16 numbering system expressed in digits 0-9 followed by letter A to F

Each hexadecimal digit represents 4bits and is equal to 16 (16^x); pair of hexadecimal digits represents a byte (8 bits) of data

Used in networking field for layer two addressing of the device (MAC address) or in the field of color display, for example in the RGB model

Each color component red, green, blue (RGB) is represented by eight bits, which corresponds to two hexadecimal digits

For example the number 2A3 in hexadecimal is (3×16^0=3×1=3) + (10x16^1=160) + (2x16^2=2x256=512) = 675 in decimal

From Dec 60 to Hex: divide 60 by 16 = 3 and the remainder is 12 (since 60 = 3 \* 16 + 12)

Convert the remainder 12 to hexadecimal. In hexadecimal, 12 is represented as C

Combine the quotient and the remainder: 3C

From Dec 329 to Hex: divide 329 by 16 = 20 and the remainder is 9

20/16 = 1 with the remainder 4

1/16 = 0 with the remainder of 1

Now we convert the remainders into hex format: 149

Dec Hex Binary

0 0 0

1 1 1

2 2 10

3 3 11

4 4 100

5 5 101

6 6 110

7 7 111

8 8 1000

9 9 1001

10 A 1010

11 B 1011

12 C 1100

13 D 1101

14 E 1110

15 F 1111

16 10 1000

31 1F 11111

32 20 100000

255 FF 11111111

![](<.gitbook/assets/Unknown image (428)>)

### Character encoding

**Character encoding** is a method of representing characters as numerical values. One commonly used character encoding scheme is **ASCII**

**ASCII (American Standard Code for Information Interchange)**

assigns a unique numeric value to each character, including letters, numbers, and symbols. For example, the ASCII value for the uppercase letter "A" is 65.

Computers convert it into binary code using a process called binary representation. In binary representation, the decimal value is successively divided by 2, and the remainders are recorded in reverse order. The remainders form the binary representation of the decimal value. Each remainder is either 0 or 1, corresponding to the binary digits.

Converting the ASCII value 65 (represents the letter "A") into binary code:

Divide 65 by 2. The quotient is 32, and the remainder is 1.

Divide 32 (the quotient from the previous step) by 2. The new quotient is 16, and the remainder is 0.

Divide 16 by 2. The quotient is 8, and the remainder is 0.

Divide 8 by 2. The quotient is 4, and the remainder is 0.

Divide 4 by 2. The quotient is 2, and the remainder is 0.

Divide 2 by 2. The quotient is 1, and the remainder is 0.

Divide 1 by 2. The quotient is 0, and the remainder is 1.

The binary representation of 65 is the sequence of remainders in reverse order: 1000001. This is equivalent to the binary code for the letter "A".

In modern computers, characters are represented using **Unicode**, which defines the characters and their code points to each character.

**UTF-8** is commonly used to convert characters into binary code. It represents ASCII characters with one byte, and non-ASCII characters with multiple bytes based on their Unicode code point.

Computers check the character's Unicode code point and use UTF-8 encoding rules to convert it into the corresponding sequence of 0s and 1s.

Unicode and UTF-8 enable computers to handle characters from different languages (including popular emojis) and represent them in binary for storage and processing

Each decimal value in ASCII can be represented in binary using 7 bits (since the standard ASCII character set uses 7 bits for encoding). For example, the decimal value 65 is represented in binary as 01000001.

![](<.gitbook/assets/Unknown image (429)>)

A 32-bit system can address up to 4 gigabytes of memory, while a 64-bit system can address up to 16 exabytes, which is significantly larger.

In 32-bit system, the limited memory (4GB) may lead to data being stored on the slower hard drive, causing a slowdown as the CPU has to retrieve data from the hard drive.

64-bit system, with its ability to support much larger memory, allows more data to be stored in the faster RAM, leading to improved speed and overall performance.

### Voice over IP (VoIP)

Instead of using traditional phone lines, **VoIP** technology converts voice and other audio signals into digital data packets for transmission over the internet

Since VOIP uses the internet infrastructure, it can bypass traditional telephone networks and their associated costs for international calls

VOIP services are not tied to physical locations, allowing users to make and receive calls from any location with internet connectivity. This enhances mobility and flexibility for businesses and individuals

The protocols commonly used in VoIP communication include **SIP (Session Initiation Protocol)** for initiating and terminating sessions, and **RTP (Real-Time Transport Protocol)** for the transmission of audio and video.

Popular VOIP service providers include Skype, Zoom, Microsoft Teams, and various business-oriented services

The audio input, which is in analog form (sound waves), is first converted into digital form using an **Analog-to-Digital Converter (ADC)**

This process involves sampling the analog signal at regular intervals and quantizing the amplitude of each sample

The digital audio data is then encoded and compressed to reduce the amount of data that needs to be transmitted. Various audio **codecs (Coder-Decoder)** are used for this purpose. Popular codecs include **G.711**, **G.729**, and **Opus**

Collaboration in Cisco often encompasses various technologies and solutions related to voice, video, messaging, and conferencing, which are commonly associated with VOIP
