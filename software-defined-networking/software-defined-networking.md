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

# SDN

## Software-Defined Networking (SDN)

Traditional networks comprise several devices (for example, routers, switches, and WLCs) that are equipped with software and networking functionality:

The **data (or forwarding) plane** is responsible for the forwarding of data through a network device.

The **control plane** is responsible for controlling the forwarding tables that the data plane uses.

The **management plane** is integrated into the control plane.

Each device has a control and data plane. This means that all devices are equally smart and can make decisions on their own, since the control plane exists. Of course, the data plane is what is responsible for the actual packet forwarding. This network is now referred to as the traditional network, and is still the dominant network type deployed.

The data plane acts on the forwarding decisions.

The control and management planes learn/compute forwarding decisions.

The following trends in traditional networks present significant challenges:

There are more users and endpoints, therefore, more VLANs and subnets. It becomes more difficult to keep track of all those groups and segment them.

There are so many different types of users coming into the network that it is becoming more complex to configure. Multiple steps are required to give users credentials and support connectivity choices.

As users and devices move around the network, policy is not consistent. As a result, it is difficult to find users when they move around and troubleshoot issues.

With SDN, the network changes:

The control (and management) plane becomes centralized.

Physical devices retain data plane functions only.



**Software-Defined Networking (SDN)** refers to the set of techniques that are used to manage and change a network’s behavior through an open interface rather than closed-box methods

SDN centralizes control and management plane function into application called **Controller**, which runs on a dedicated device or as a virtual software on a server and acts as the brain of the network

The **controller** is a single pane of glass centralized management platform that serves as unified interface or dashabord to provide comprehensive visibility and control over various systems, applications, or network elements from a single location and interact programmatically with the infrastructure using APIs

The **data plane** are the individual network edge devices on each site, that perform forwarding and WAN connectivity to the individual site

This method decreases complexity, human error, and the time it takes to deliver a new service

With Software-Defined Networking (SDN), you can reduce the complexity of your network by using a standardized network topology and by building an abstract overlay network on top. In this way, you move from a single device view of the network (box-oriented) to a global, high-level view (network-oriented). This high-level view enables you to use abstractions and simplifications when provisioning new services. For example, the network operator configuring a virtual private network (VPN) for a remote office environment is not concerned (and should not be) with the physical layout of the network. The only requirement of the remote site and operator is that the network spans all geographic regions required for the VPN (for example, the Main Campus and Remote Office). The controller will figure out what needs to be provisioned. The prerequisite to this is that the controller is the central point of management and the "source of truth" for the configuration.

Using abstractions when managing your network also enables you to use standardized components. Software-Defined Networking (SDN) implementations typically define a standard architecture and Application Programming Interfaces (APIs) that the network devices use. To a limited degree, you can also swap a network device for a different model or a different vendor altogether. The controller will take care of configuration, but the high-level view of the network for the operators and customers will stay the same.

Simplification of configuration and automated management also directly results in operating expenses (OPEX) savings. Typically, the total cost of ownership (TCO) for a network in a 5-year span comprises about 30 percent capital expenditure (CAPEX) and about 70 percent OPEX. Manual service configuration and activation represent a significant chunk of OPEX.

Controller-based networking makes centralized policy easy to achieve. Networkwide policy can be easily defined and distributed consistently to the devices connected to the controller. For example, instead of attempting to manage access control lists across many individual devices, a flow rule can be defined on the central controller and pushed down to all the forwarding devices as part of the normal operations.

Compared to traditional networking, controller-based networking makes it easy to define special treatment for specific network traffic. Instead of adding complexity to the network through advanced mechanisms like policy-based routing, a traffic flow rule can be defined on the controller and pushed down to all the forwarding devices as part of normal operations. The largest benefit here is that there is a device, a controller, that has a unified view of the network in one location.

This single point of administration addresses the scalability problem where administrators are no longer required to touch each individual device to make changes to the environment. This concept is also not new as controllers have also been around for many years and used for campus wireless networking. Similar to the behavior between a Cisco Wireless LAN Controller (WLC) and its managed access points (APs), the controller provides a single point to define business intent or policy, reducing overall complexity through the consistent application of intent or policy to all devices that fall within the management domain of the controllers. For example, think about how easy it is to enable authentication, authorization, and accounting (AAA) for wireless clients using a WLC, compared to enabling AAA for wired clients (where you would need AAA changes on every switch if you are not using a controller).

With automated processes, the time to provision a new service or implement a change request is drastically reduced. What would previously take days or weeks to implement can be automated to run in hours, along with testing and verification. Lifecycle management is another important step in the automation process—from design and installation of the infrastructure components (day 0) to service enablement (day 1) to management and operations (day 2). Also, after the customer no longer needs the service, the resources that are used must be deallocated and the configuration of the devices must be cleaned up. Even with proper change management procedures, this process is tedious at best if performed manually. If the process is fully automated, you can make sure that the same configuration changes that were applied when provisioning the new service will be removed when it is deprovisioned.

The hybrid SDN option is a combination of the best of both schemas. In a hybrid SDN, the controller becomes an active part of the distributed network control plane, rather than a means to configure the network control plane behavior in devices. This solution also offers a centralized view of the network, giving an SDN controller the ability to act as the brain of the network.

### SDN layers

**Infrastructure layer:** Contains network elements (any physical or virtual device that deals with traffic).

**Control layer:** Represents the core layer of the SDN architecture. It contains SDN controllers, which provide centralized control of the devices in the data plane.

**Application layer:** Contains the SDN applications, which communicate network requirements towards the controller.

![](<../.gitbook/assets/Unknown image (994)>)

#### Northbound API (NBI)

**Northbound API (NBI)** is used to communicate between SDN controller and its management software (Network management system (NMS)) via REST APIs to provide an abstracted network view to upstream applications in the application layer.

#### Southbound API (SBI)

**Southbound API (SBI)** is used to communicate between the SDN controller and network devices via the NETCONF and RESTCONF or Openflow or APIs to control individual devices in the infrastructure layer

![](<../.gitbook/assets/Unknown image (995)>)

![](<../.gitbook/assets/Unknown image (996)>)

#### Integration API (westbound)

**Integration API (westbound)** in the context of Cisco DNA Center refers to an API that enables the system to publish network data, events, and notifications to external systems, as well as to consume information from connected third-party applications. In contrast, a Northbound API is typically used to allow external applications to retrieve network insights and interact with Cisco DNA Center

### Intent-based networking

**Intent-based networking** transforms a hardware-centric, manual network into a controller-led network that captures business intent and translates it into policies that can be automated and applied consistently across the network to help assure desired business outcomes

SDN is a foundational building block of intent-based networking. The good news for SDN practitioners is that intent-based networking addresses shortfalls of SDN. For example, the automated translation of business policies to IT (security and compliance) policies, automated deployment of these policies, and the assurance that if the network is not providing the requested policies, they will receive proactive notification. Intent-based networking adds context, learning, and assurance capabilities, by tightly coupling policy with intent. The "intent" enables the expression of both business purpose and network context through abstractions, which are then translated to achieve the desired outcome for network management. SDN is purposely focused on instantiating change in network functions.

The translation element enables the operator to focus on what they want to accomplish, and not how they want to accomplish it. The translation element takes the desired intent and translates it to associated network policies and security policies. Before applying these new policies, the system checks if these policies are consistent with the already deployed policies or if they will cause any inconsistencies.

Once approved, the new policies are then activated (automatically deployed across the network).

With assurance, an intent-based network performs continuous verification that the network is operating as intended. Any discrepancies are identified; a root-cause analysis can recommend fixes to the network operator. The operator can then "accept" the recommended fixes to be automatically applied, before another cycle of verification. The assurance does not occur at discrete times in an intent-based network. Continuous verification is essential since the state of the network is constantly changing. Continuous verification assures network performance and reliability.

Software-Defined Networking (SDN) allows network engineers to provision, manage, and program networks more rapidly, as it greatly simplifies automation tasks by providing a single point of administration for the programming of the infrastructure.

## Software-Defined WAN

**Software-Defined WAN (SD-WAN)** is a solution for Enterprise and DC networks that was developed to address new requirement for WAN

The traditional company network traffic flowed directly between company locations and data centers, which allowed the company to have all network traffic under complete management

Today, enterprises rely on external cloud services such as software as a service, platform as a service or infrastructure as a service to reduce the cost of maintaining the applications and resources needed for business operations

However, this is changing the structure of traffic, so that most business traffic is going directly to public clouds and the Internet, which they cannot control.

These changes create new requirements for security, application performance, cloud connectivity and overall traffic that traditional WAN solutions were not designed to handle.

SD-WAN solutions address these challenges by offering features such as dynamic path selection, Application-aware routing (AAR), secure cloud connectivity and centralized management

* Cisco SD-WAN architecture applied the principles of SDN networks to the WAN by separating the production traffic (data plane) and the control traffic (control plane) and the remote management of branch routers (management plane)

The control plane is responsible for running and configuring remote routers that provide WAN connectivity for individual locations.

Control and management plane vManage, vSmart and vBond can run as virtual machines (VMs) on a server or as dedicated hardware appliances, depending on the deployment model chosen by the organization

The server can then be managed directly at the main location of the organization (on-prem), or it can be managed from a cloud environment provided by a cloud provider, so the company does not have to maintain the server at its own premises

* SD-WAN solution offers Transport independence, creating an overlay network that is built on top of any type of circuit or transport
* Applications are steered across available transport circuits to ensure their SLA needs as well as load-balancing
* Traffic segmentation is delivered by leveraging VPNs that are in SD-WAN synonymous to VRFs
* Companies can leverage Direct Internet Access (DIA), which is a premium internet service that provides businesses with a private connection to the internet or cloud

This is in contrast to the shared internet access among all subscribers such as typical SOHO networks

This allows a branch to send internet traffic destined to cloud applications directly to the Internet or directly to the cloud service providers without having to route the traffic via their private Hub or Data center, which would introduce latency or jitter to the employees and consume bandwidth of more expensive transports such as MPLS circuits, which should be used to route traffic destined only to the business internal resources

* SDN also includes Zero-Touch provisioning (ZTP), where no manual intervention is required to connect a new network element to the SDN infrastructure

The provisioning of the new device is done automatically without intervention - hence zero touch

There is also assumption that the WAN provider assigns the CE IP to the connected device via DHCP

Alternatively, the IP address can only be set for the interface connected to the ISP device, which mediates the connection to the WAN

* The orchestration plane assists in the automatic onboarding of the SD-WAN routers into the SD-WAN overlay.
* The management plane is responsible for centralized configuration and monitoring.
* The control plane builds and maintains the network topology and makes decisions on where traffic flows.
* The data plane is responsible for forwarding packets based on decisions from the control plane.

SD-WAN represents an evolution of networking from an older, hardware-based model to a secure, software-based, virtual IP fabric. The overlay network forms a software overlay that runs over standard network transport services, including the public internet, MPLS, and broadband. The overlay network also supports next-generation software services, thereby accelerating the shift to cloud networking.

![](<../.gitbook/assets/Unknown image (1551)>)

### Cisco SD-WAN (Viptela)

Provide secure connectivity to remote offices, branch offices, campus networks, data centers, and the cloud over any type of IP-based underlay transport network (Internet, 3G/4G LTE, and MPLS). Meant for organizations requiring solution with segmentation, advanced routing, security, and complex topologies while connecting to cloud instances

![](<../.gitbook/assets/Unknown image (1552)>)

![](<../.gitbook/assets/Unknown image (1553)>)

### SD-WAN planes and components

#### Orchestration plane (vBond / validator)

This software-based component performs the initial authentication of WAN Edge devices and orchestrates vSmart and WAN Edge connectivity. It also has an important role in enabling the communication of devices that sit behind Network Address Translation (NAT).

Creates temporary DTSL (UDP port 12346) tunnel with SDWAN router to authenticate them with certificates and informs vSmart and vManage about their request to join the network and providing connectivity information about vSmart and vManage to routers. Once control plane connectivity is up to vSmart and vManage, the connection to the vBond is torn down

It is the only device that must have a public IP address so that all SD-WAN devices in network can connect to it

Does have control plane connection over a DTLS tunnel with each vSmart controller

Acts as a Session Traversal Utilities for NAT (STUN) server, which allows other controllers and SD-WAN routers to discover their own mapped/translated IP addresses and port numbers

#### Control plane (vSmart controller)

This software-based component is responsible for the centralized control plane of the SD-WAN network. It establishes a secure connection to each WAN Edge router and distributes routes and policy information via the Overlay Management Protocol (OMP). It also orchestrates the secure data plane connectivity between the WAN Edge routers by distributing crypto key information.

Overlay Management Protocol (OMP) is used to influence control and data plane operations

Configuration and policies are created on vM and pushed to WAN edges via NETCONF

Handles the security and encryption of the fabric by providing key management

Can handle up to 5,400 connections per vSmart server with up to 20 vSmarts in a single deployment

After authentication, each vSmart controller establishes a permanent DTLS tunnel to each SD-WAN router and uses these tunnels to establish OMP neighborship

![](<../.gitbook/assets/Unknown image (1554)>)

![](<../.gitbook/assets/Unknown image (1555)>)

#### Management plane (vManage)

Centralized network management system provides a GUI interface to monitor, configure, and maintain all Cisco SD-WAN devices and links in the underlay and overlay network.

Network administrators can simulate traffic flows to show data paths, troubleshoot WAN impairment, and access the configuration and routing tables of all devices

Onboards SD-WAN routers into the SD-WAN overlay by pushing configuration to them

Onboarding refers to the commissioning of the device and its introduction into the network infrastructure

Each WAN Edge will form a single management plane connection to vManage. If device has multiple transports available, only one will be used for management plane connectivity to vManage

Programmatic APIs (REST): Programmatic control over all aspects of SD-WAN Manager administration.

Analytics (SD-WAN Analytics):

optional analytics and assurance service, requires additional licensing and isn’t on by default. vM is for real-time data, whilst vAnalytics is used to review the historical performance of network

If a branch office is experiencing latency or loss on its MPLS link, vAnalytics detects this, and it compares that loss or latency with information on other organizations in the area that it is also monitoring to see, if they are also having that same loss and latency in their circuits. If they are, vAnalytics can then report the issue with confidence to the SPs

Help predict how much bandwidth is truly required for any location (useful in deciding whether a circuit can be downgraded to a lower bandwidth to reduce costs)

#### Data plane (WAN Edge / cEdge / vEdge)

This device, available as either a hardware appliance or software-based router, sits at a physical site or in the cloud and provides secure data plane connectivity among the sites over one or more WAN transports. It is responsible for traffic forwarding, security, encryption, QoS, routing protocols such as BGP and OSPF, and more.

end nodes at the boundary of a site establishing a data plane between other sites and the control site

At each site, WAN Edge routers are used to directly connect to the available transports. Colors are used to identify an individual WAN transport, as different WAN transports are assigned different colors, such as mpls, private1, biz-internet, metro-ethernet, lte, and so on. The topology uses one color for the biz-internet transport and a different one for the public-internet transport.

The WAN Edge routers form a Datagram Transport Layer Security (DTLS) or Transport Layer Security (TLS) control connection to the SD-WAN Controllers and connect to both of the SD-WAN Controllers over each transport. The WAN Edge routers securely connect to WAN Edge routers at other sites with IPsec tunnels over each transport. The Bidirectional Forwarding Detection (BFD) protocol is enabled by default and will run over each of these tunnels, detecting loss, latency, jitter, and path failures.

Policies are an important part of the Cisco Catalyst SD-WAN solution and are used to influence the flow of data traffic among the WAN Edge routers in the overlay network. Policies apply either to control plane or data plane traffic and are configured either centrally on SD-WAN Controllers (centralized policy) or locally (localized policy) on WAN Edge routers.

Establishes IPsec sessions with other SD-WAN routers in the fabric to convey multiple VPNs for traffic segmentation

Have local intelligence to make site-local decisions regarding routing, high availability (HA), interfaces, ARP, and ACLs

vEdge The original Viptela platforms running Viptela software. Available as hardware, software, cloud, or virtualized routers

cEdge Viptela software integrated with Cisco IOS-XE. Has couple of more security features embedded than vEdge

Centralized control policies operate on the routing and transport location (TLOC) information and allow for customizing routing decisions and determining routing paths through the overlay network. These policies can be used in configuring traffic engineering, path affinity, service insertion, and different types of VPN topologies (full-mesh, hub-and-spoke, regional mesh, and so on). Another centralized control policy is application-aware routing, which selects the optimal path based on real-time path performance characteristics for different traffic types. Localized control policies allow you to affect routing policy at a local site.

Data policies influence the flow of data traffic through the network based on fields in the IP packet headers and VPN membership. Centralized data policies can be used in configuring application firewalls, service chaining, traffic engineering, and QoS. Localized data policies allow you to configure how data traffic is handled at a specific site, such as ACLs, QoS, mirroring, and policing. Some centralized data policy may affect handling on the WAN Edge itself, as in the case of app-route policies or a QoS classification policy. In these cases, the configuration is still downloaded directly to the SD-WAN Controllers, but any policy information that needs to be conveyed to the WAN Edge routers is communicated through Overlay Management Protocol (OMP).

### Multi-tenancy options

**Dedicated tenancy** each tenant has dedicated components and the data plane is segmented as well

**Multi-tenancy** (multiple customers are running on same infrastucture) v prekladu najem

**VPN tenancy** segments only the data plane of the VPN topology and allows you to define read-only users who can view and monitor their VPN within vManage. VPN tenancy still shares the same SD-WAN components

**Enterprise tenancy** the orchestration and management planes are operating in multi-tenancy mode, but the control plane requires dedicated, per-tenant appliances. Because the control plane is dedicated, it can be deployed as a container or nested virtual machine to decrease scalability concerns

![](<../.gitbook/assets/Unknown image (1556)>)

### Overlay Management Protocol (OMP)

**Overlay Management Protocol (OMP)** is proprietary routing protocol similar to BGP that perform best-path selection routing policy advertisement, data plane security information distribution including encryption keys.

Admin Distance 250 for Viptela OS (251 for XE)

OMG Advertisements

OMP Routes (vRoutes) prefixes learned from vEdge from it's connected interfaces, static routes, and underlay dynamic routing protocols

These prefixes are redistributed into OMP and advertised to the vSmart controller which operates similarly to a BGP route reflector in iBGP. The vSmart receives routing information from each WAN Edge and apply policies before advertising this information back out to other WAN Edges. OMP routes resolve their next-hop to a TLOC (next hop of the OMP route). OMP route is installed in the forwarding table only if the next-hop TLOC is known and there is a BFD session in UP state

Service routes advertise a specific service used for service chaining policies

Service chaining allows data traffic to be routed to a remote site through one or more services (firewalls,

IPS,IDS, load balancers, or an IDP) before being routed to the traffic’s original destination

Devices that provide services for the overlay must be Layer 2 adjacent for traffic to be redirected through them (Layer 2 adjacency can be achieved with IPsec or GRE tunnels)

**TLOC (Transport Locator)** identifier that ties an OMP route to a physical location

Represents the endpoint of the data plane tunnels and sends data plane instructions to vSmart

![](<../.gitbook/assets/Unknown image (1557)>)

TLOC attributes

System-IP unique identifier of the WAN edge device across the SD-WAN fabric (similar to the Router-ID)

It does not need to be routable or reachable across the fabric

Transport Color identify local WAN transports such as MPLS, Internet, LTE, 5G, etc

Encapsulation Type specifies the type of encapsulation (IPsec, GRE) used for the data plane tunnel

TLOC private address derived from the physical interface of the WAN Edge

TLOC public address publicly routable IP address assigned to the WAN Edge

If both public and private addresses match in a TLOC route, the device is considered to not be behind a NAT

Preference similar to OMP Preference, used to prefer one TLOC over another; higher preference

Site ID identifies the originator of this TLOC route and is used to control how data plane tunnels are built

Tag similar to OMP tags; can control how prefixes are exchanged and, ultimately, how traffic will flow

Weight path selection method. Similar to BGP Weight and is locally significant; higher preference

Above TLOC and following attributes can be used to influence routing decisions

Origin source of the route is inserted into update; contains an identifier

(BGP, OSPF, EIGRP, Connected, or Static), along with the protocol’s original metric

Originator system IP of the advertiser

Preference (OMP preference) similar to LOCAL\_PREF in BGP; higher preference

Service a service (firewall) is associated to this route

![](<../.gitbook/assets/Unknown image (1558)>)

Site ID is BGP ASN

Tag an optional, transitive attribute that an OMP peer can apply to the route (route tag in traditional routing)

VPN communicates what VPN/VRF this route was advertised from

![](<../.gitbook/assets/Unknown image (1559)>)

Clear text IPsec tunnel is used primarily for network management and control purposes

This clear text tunnel allows for real-time monitoring, analysis, and optimization of network traffic without the overhead of encryption and decryption.

It ensures that the SD-WAN controller has the necessary insights to dynamically route traffic based on application requirements, network conditions, and business priorities.

Encrypted IPsec tunnel is used to secure the actual user data traffic that flows between different locations in an SD-WAN deployment

In Cisco SD-WAN, key exchange and distribution have been moved to the vSmart. Each WAN Edge will compute its own keys per transport and distribute these to the vSmart. The vSmart will then distribute them to each WAN Edge, depending on defined policy. In addition, the vSmart is also responsible for rekeying of the IPsec Security Associations (SA) when they expire. By moving key exchange to a centralized location, we achieve greater scale as each WAN Edge doesn’t need to handle key negotiation or distribution

If there is a situation where control connectivity was established but, due to an outage, has been lost, then data plane connectivity will continue to flow. By default, WAN Edges will continue forwarding data plane traffic in the absence of control plane connectivity for 12 hours, utilizing the last-known state of the routing table, though this is configurable, depending on your requirements. When control plane connectivity is reestablished, WAN Edges will be updated with any policy changes that were made during the outage. When the control connection is restored, the route table is flushed and the newly received route table is installed. This will cause a brief outage to the data plane when this occurs

### Onboarding and provisioning

When the WAN Edge initially gets connected to the network, it first tries to reach out to a Plug and Play (PNP) or Zero Touch Provisioning (ZTP) server

There are two methods of auto-provisioning of WAN Edges: PNP and ZTP. PNP uses HTTPS to connect to Cisco PNP servers, and ZTP uses UDP port 12346 to connect. Cisco XE SD-WAN routers use PNP, while Cisco vEdges use ZTP for provisioning.

One remaining functionality that the vBond provides is network address translation (NAT) traversal. By default, the vBond operates as a STUN Server (RFC 5389). The WAN Edge operates as a STUN client. What this means is that the vBond can detect when WAN Edges are behind a NAT device such as a firewall. When the WAN Edge goes to establish its DTLS tunnel, the interface IP it knows about will be written into the outer IP header and noted within a payload of the message. When the vBond receives this information, it performs a XOR operation comparing the two values. If the two values are different, it can be inferred that NAT is in the transit path of the WAN Edge (since the outer IP header was changed to a NAT’d IP address and no longer matches the IP address noted in the payload of the packet). The vBond will communicate this back to the WAN Edge, and the WAN Edge can communicate this information to the rest of the overlay components—ultimately allowing data plane connectivity to be established through a NAT device

![](<../.gitbook/assets/Unknown image (1560)>)

![](<../.gitbook/assets/Unknown image (1561)>)

### Cloud OnRamp

**Cloud OnRamp** delivers the best application quality of experience (QoE) for SaaS applications by continuously monitoring SaaS performance across diverse paths and selecting the best-performing path based on performance metrics (jitter, loss, and delay)

Simplifies hybrid cloud and multicloud IaaS connectivity by extending the SD-WAN fabric to the public cloud while at the same time increasing high availability and scale

1st picture Can be configured on the vManage NMS and can become active on the remote site router. The router at the remote site starts sending small HTTP probes to the SaaS application through both DIA circuits to measure latency and loss. Based on the results, the router will know which circuit is performing better (in this case, ISP2) and sends the SaaS application traffic out that circuit. The process of probing continues, and if a change in performance characteristics of ISP2’s DIA circuit occurs (for example, due to loss or latency), the remote site router makes an appropriate forwarding decision.

2nd picture Cloud OnRamp for SaaS also gets enabled on the regional hub SD-WAN router and is designated as the gateway node. Quality probing service via HTTP toward the cloud SaaS application of interest starts on both the remote site and the regional hub. BFD runs through the DTLS session between the remote site and the regional hub.

The regional hub router reports its HTTP connection loss and latency characteristics to the remote site router in OMP message exchange through the vSmart controllers. At this time, the remote site router can evaluate the performance characteristics of its local DIA circuit compared to the performance characteristics reported by the regional hub

It also takes into consideration the loss and latency incurred by traversing the SD-WAN fabric between the remote site and the hub site (calculated using BFD) and then makes an appropriate forwarding decision, sending application traffic down the best-performing path toward the cloud SaaS application of choice.

// Viptela Quality of Experience (vQoE) SaaS app score on a scale of 0 to 10, with 0 being the worst quality and 10 being the best. vQoE can be observed in the vManage GUI

![](<../.gitbook/assets/Unknown image (1562)>)

![](<../.gitbook/assets/Unknown image (1563)>)

![](<../.gitbook/assets/Unknown image (1564)>)

## Meraki SD-WAN

### Overview

Cisco Meraki represents a powerful shift in the way networks are managed, aligning with the broader trend of moving IT services to the cloud. This transition is driven by the need for simplicity, scalability, and the ability to manage systems from anywhere at any time. Cloud services offer significant cost savings, enhanced collaboration, and automatic updates, which are essential in today's fast-paced digital world.

Cisco Meraki brings these advantages to network management. When Cisco Meraki devices—like access points, switches, and routers—are powered on, they automatically connect to the Cisco Meraki cloud. Once connected, you are able to utilize the Cisco Meraki Dashboard GUI to monitor and configure your enterprise network from anywhere.

It is a Unified Threat Management (UTM) solution for organizations requiring all-in-one solution delivered in a single appliance including SD-WAN and Firewall functionalities (including IPS/IDS, Web content filtering and VPN)

### Benefits

Cisco Meraki offers these benefits:

Deployment: Many features can be deployed quickly and easily. Rolling out deployments is much easier.

Cloning configurations: When creating a new network, administrators can choose to clone the configuration for the new network from an existing network.

Configuration templates: An administrator can make one change that can be applied to many networks and the devices within those networks.

Zero-touch deployment: The cloud architecture allows you to configure devices without having the hardware. This approach is possible because configurations are stored and managed in the cloud, so administrators can stage configurations before they have the hardware.

Cisco Meraki solutions are very scalable and can extend to hundreds of thousands of devices. Scaling means simply adding more devices and licenses to the dashboard.

![](<../.gitbook/assets/unknown (1).png>)

The Cisco Meraki ecosystem encompasses a range of device families designed to create a seamless, cloud-managed network:

MR: Access Points supporting Wi-Fi 6/6E and WPA3.

MX: Security Appliances supporting up to 6Gbps firewall throughput.

MS: Switches supporting PoE+, multigigabit connectivity (nGig), and SFP+ uplinks.

SM: System Manager for mobile device management (MDM).

MV: Smart Cameras for indoor and outdoor, with up to 360-degree coverage.

MI: Insights for network visibility and traffic analytics.

MT: Sensors for temperature, humidity, water leak detection, air quality, and security.

![](<../.gitbook/assets/unknown (2).png>)

### Cisco Cloud Monitoring

The Cisco Cloud Monitoring feature provides a cohesive view of your network by displaying statistics, configurations, and offering troubleshooting tools for both Cisco Catalyst wireless and switch devices. It is important to note that Cloud Monitoring is primarily a visual and informational tool, meaning that it doesn't replace comprehensive management solutions for configuring wireless controllers and switches.

The Cisco Meraki product lineup has expanded to include Cisco Cloud Monitoring (CCM) tools. The product lineup offers you the capability to monitor and manage your existing Cisco Catalyst 9000 series devices through the Cisco Meraki Dashboard for a unified management experience.

Cisco Meraki Cloud Monitoring is currently supported on the following Cisco Catalyst hardware:

Cisco Catalyst 9200/L Series

Cisco Catalyst 9300/L/X Series

Cisco Catalyst 9500 Series

Catalyst 9800 Wireless LAN Controller

## Software-Defined Access (SDA)

### Challenges with traditional networks

A slow-to-deploy network impedes the ability of many organizations to innovate rapidly and adopt new technologies such as video, collaboration, and connected workspaces. The ability of a company to adopt any of these is impeded if the network is slow to change and adapt. In addition, one of the major challenges with wireless deployment today is that it does not easily utilize network segmentation. While wireless can leverage multiple service set identifiers (SSIDs) for traffic separation over the air, these are limited in the number that can be deployed and are ultimately mapped back into VLANs at the wireless LAN controller (WLC). The WLC itself has no concept of virtual routing and forwarding (VRF) or Layer 3 segmentation, making deployment of a true wired and wireless network virtualization solution very challenging.

Policy is one of those abstract words that can mean many different things to different people. However, in the context of networking, every organization has multiple policies that they implement. Use of security access control lists (ACLs) on a switch, or security rulesets on a firewall, is security policy. Using quality of service (QoS) to sort traffic into different classes, and using queues on network devices to prioritize one application versus another, is QoS policy. Placing devices into separate VLANs based on their role is device-level access control policy. The traditional methods used today for policy administration (large and complex ACLs on devices and ̀firewalls) are difficult to implement and maintain. Also, most organizations want to establish user and device identity for end-to-end policy. In addition, most organizations lack comprehensive visibility into network operation, limiting their ability to proactively respond to changes. All these issues influence how long it takes for a new network service to be deployed. A more comprehensive, end-to-end approach is needed, one that allows insights to be drawn from the mass of data that potentially can be reported from the underlying infrastructure.



**Software-Defined Access (SDA)** is a programmable network architecture that provides software-based policy and segmentation from the edge of the network to the applications. SD-Access is implemented via Cisco Catalyst Center, which provides design settings, policy definition, and automated provisioning of the network elements, as well as assurance analytics for an intelligent wired and wireless network.

In an enterprise architecture, the network may span multiple domains, locations, or sites such as main campuses and remote branches, each with multiple devices, services, and policies. The Cisco SD-Access solution offers an end-to-end architecture that ensures consistency in terms of connectivity, segmentation, and policy across different locations (sites).

Cisco SD-Access comprises these elements:

Cisco Catalyst Center: Cisco SDN Controller for automation, policy, assurance, and integration infrastructure

SD-Access fabric: Physical and logical network-forwarding infrastructur

Cisco Catalyst Center provides a central management plane for building and operating an SD-Access fabric. The management plane is responsible for forwarding configuration and policy distribution, as well as device management and analytics.

SD-Access provides automated end-to-end services (such as segmentation, QoS, and analytics) for user, device, and application traffic. SD-Access automates user policy so organizations can ensure that the appropriate access control and application experience are set for any user or device to any application across the network. This is accomplished with a single network fabric across LAN and WLAN, which creates a consistent user experience, anywhere, without compromising on security.

### Architecture

![](<../.gitbook/assets/Unknown image (1546)>)

![](<../.gitbook/assets/Unknown image (1547)>)

#### Management layer

**Cisco Catalyst Center (CCC)**

is the user interface/user experience (UI/UX) layer, where all the information from the other layers is presented to the user in the form of a centralized management dashboard

Cisco Catalyst Center and Cisco ISE controller appliances provide mgmt, provisioning and monitoring for SDA

#### Physical layer

All devices participating in SD-Access must support ASICs and Field-Programmable Gate Arrays (FPGAs)

Cisco Catalyst Switches provide wired and wireless (embedded WLC) access to the fabric. Default MTU is 9100

Cisco ASR 1000, ISR, and CSR routers provide WAN access to the fabric

#### Network layer

Overlay involves all overlay control plane protocols, configuration and addressing. Fully automated

When it comes to the overlay, SD-Access supports both IPv6-only wired and wireless endpoints. That IPv6 traffic is encapsulated in IPv4 and VXLAN header within the SD-Access fabric until they reach the fabric border nodes. The fabric border nodes decapsulate the IPv4 and VXLAN header, which pursues the normal IPv6 unicast routing process from then.

Underlay IPv6 functionality for overlay depends on the underlay. The IPv6 overlay uses the IPv4 underlay IP addressing to create LISP control plane and VXLAN data plane tunnels. You can always enable the dual-stack for the underlay routing protocol. Only the SD-Access overlay LISP depends on the IPv4 routing. This requirement is for the current version of DNA-C (2.3.x) and is removed in later releases where the underlay can only be dual-stack or single IPv6 stack.

Configured manually via CLI, via API or with automated approach with Catalyst Center that can deploy unicast and multicast routing (By default, this is disabled)

If broadcast, Link local multicast and ARP flooding is required, it must be specifically enabled on a per-subnet basis using Layer 2 flooding feature

IS-IS is used as underlay routing protocol for it's simplicity

Control plane based on LISP

Data plane based on VXLAN-GPO

Policy plane based on Cisco TrustSec with ISE

ISE identifies users and devices connecting to the network and provides network access control and segmentation

Security Group Access Control Lists (SGACLs) provides simpler and more scalable form of policy enforcement based on identity instead of IP address

We can either manage policy in CCC with ISE in read-only mode for a simpler, policy-enforcement-focused approach, suitable for smaller networks or use ISE with its UI for granular policy control, advanced authentication, dynamic policy changes, and comprehensive reporting, ideal for larger or complex networks with stringent security needs

#### Fabric roles

**Control Plane Node** is a host database, that manage SGT mapping (assumes LISP MS/MR role)

All registrations are sent to control node, which then updates fabric edge nodes and border nodes with wired and wireless client mobility and RLOC information

**Border Node** gateway between the fabric and external networks (assumes LISP PxTRs). Translate reachability and policy information such as VRF and SGT from one domain to another

Internal border connects only to the known areas of the organization

Default border connects only to unknown areas outside the organization. Configured with a default route to reach external unknown networks such as the Internet or the public cloud

Advertises EID summary prefix to the outside eBGP neigbor and imports external prefixes to the LISP domain

**Edge Node** It is a LISP tunnel router (xTR) that provides onboarding and mobility services for wired endpoints and perform en/de-capsulation of host traffic to and from its connected endpoints. Authenticate endpoints using .1x and places them in a host pool (SVI and VRF instance) and scalable group, then registers EID host address (MAC, /32 IPv4, or /128 IPv6) with the control plane node. Single L3 anycast SVI gateway with the same IP on all fabric edge nodes is implemented

**WLC Node** fabric edge for wireless clients. The control plane node maps the host EID to the current AP and edge node location the AP is attached to

Fabric APs are part of overlay as they establish a VXLAN tunnel to the fabric edge to transport wireless client data traffic through the VXLAN tunnel instead of the CAPWAP tunnel, which improves performance. SD-Access Embedded Wireless is a feature that enables wireless controller functionality on Catalyst 9000 Series switches without a hardware WLC

**Intermediate Nodes** device covering L3 underlay network that connects the border nodes and edge nodes. It provides IP reachability between devices that operate in a fabric function

**Fusion device** enables VRF leaking across SD-Access fabric domains and enables host connectivity to shared services

**Extended node** device that extends the fabric overlay and segmentation to non-fabric devices (such as IoT), acting as a bridge between the traditional routed network and the SDA fabric

Policy extended nodes

![](<../.gitbook/assets/Unknown image (1548)>)

#### Controller layer

Cisco ISE and the Catalyst Center (NCP and NDP) integrate with each other to share contextual info between via APIs

**Cisco Network Control Platform (NCP)**

subsystem integrated directly into Cisco Catalyst Center that ensures the underlay and fabric automation and orchestration services using NETCONF/YANG, SNMP, SSH/Telnet

**Cisco Network Data Platform (NDP)**

is a data collection and analytics and assurance subsystem integrated directly into Catalyst Center, providing data to NCP and ISE

NDP analyzes and correlates various network events through multiple sources (NetFlow, SPAN)

**Cisco Identity Services Engine (ISE)**

provides network access control and identity services for dynamic endpoint-to-group mapping and policy definition in a variety of ways, including using 802.1x, MAB, and WebAuth.

ISE then places the profiled endpoints into the correct scalable group and host pool; also collects and uses the contextual info shared from NDP and NCP

![](<../.gitbook/assets/Unknown image (1549)>)

### SD-Access fabric (underlay vs overlay)

Part of the complexity in a network comes from the fact that policies are tied to network constructs such as IP addresses, VLANs, ACLs, and so on. The concept of fabric changes that. With a fabric, an enterprise network is thought of as being divided into two different layers, each for different objectives. One layer is dedicated to the physical devices and forwarding of traffic (known as an underlay), and the other entirely virtual layer (known as an overlay) is where wired and wireless users and devices are logically connected together, and services and policies are applied. This provides a clear separation of responsibilities and maximizes the capabilities of each sublayer while dramatically simplifying deployment and operations since a change of policy would only affect the overlay and the underlay would not be touched.

The concepts of overlay and fabric are not new in the networking industry. Existing technologies such as Multiprotocol Label Switching (MPLS), Generic Routing Encapsulation (GRE), Locator/ID Separation Protocol (LISP), and Overlay Transport Virtualization (OTV) are all examples of network tunneling technologies that implement an overlay. Another common example is Cisco Unified Wireless Network (Cisco UWN), which uses Control and Provisioning of Wireless Access Points (CAPWAP) to create an overlay network for wireless traffic.

The Cisco SD-Access architecture is supported by a fabric technology implemented for the campus, enabling the use of virtual networks (overlay networks) running on a physical network (underlay network) creating alternative topologies to connect devices.

Cisco SD-Access network underlay (or simply, underlay) is comprised of the physical network devices, such as routers, switches, and WLCs, plus a traditional Layer 3 routing protocol. This provides a simple, scalable, and resilient foundation for communication between the network devices. The network underlay is not used for client traffic (client traffic uses the fabric overlay).

All network elements of the underlay must establish IPv4 connectivity between each other. This means an existing IPv4 network can be leveraged as the network underlay. Although any topology and routing protocol could be used in the underlay, the implementation of a well-designed Layer 3 access topology (that is, a routed access topology) is highly recommended. Using a routed access topology (leveraging routing all of the way down to the access layer) eliminates the need for Spanning Tree Protocol (STP), VLAN Trunk Protocol (VTP), Hot Standby Router Protocol (HSRP), Virtual Router Redundancy Protocol (VRRP), and other similar protocols in the network underlay, simplifying the network and at the same time increasing resiliency and improving fault tolerance.

Cisco Catalyst Center provides a prescriptive LAN automation service to automatically discover, provision, and deploy network devices according to Cisco design best practices. Once discovered, the automated underlay provisioning leverages plug-and-play (PnP) to apply the required IP address and routing protocol configurations.

Cisco SD-Access fabric overlay (or simply, overlay) is the logical, virtualized topology built on top of the physical underlay. An overlay network is created on top of the underlay to create a virtualized network. In the SD-Access fabric, the overlay networks are used for transporting user traffic within the fabric. The fabric encapsulation also carries scalable group information used for traffic segmentation inside the overlay. The data plane traffic and control plane signaling are contained within each virtualized network, maintaining isolation among the networks as well as independence from the underlay network. The SD-Access fabric implements virtualization by encapsulating user traffic in overlay networks using IP packets that are sourced and terminated at the boundaries of the fabric. The fabric boundaries include borders for ingress and egress to a fabric, fabric edge switches for wired clients, and fabric APs for wireless clients. Overlay networks can run across all or a subset of the underlay network devices. Multiple overlay networks can run across the same underlay network to support multitenancy through virtualization.

### Core overlay constructs

#### Virtual network (VN)

**Virtual network (VN)** provides virtualization at the device level, using VRF instances to create multiple L3 routing tables.

In the control plane, LISP instance IDs are used to maintain separate VRF instances. In the data plane, edge nodes add a VXLAN VNID to the fabric encapsulation

#### Host pool

**Host pool** group of endpoints assigned to an IP pool subnet. Fabric edge nodes have SVI for each host pool that is used by endpoints as their default gateway

The SD-Access fabric uses EID mappings to advertise each host pool (per instance ID), which allows host-specific (/32, /128, or MAC) advertisement and mobility.

Host pools can be assigned dynamically (802.1x) and/or statically per port

#### Scalable group (SGT)

**Scalable group (SGT)** group of endpoints with similar policies. The SD-Access policy plane assigns every endpoint to a scalable group using TrustSec SGT tags. Assignment to a scalable group can be either static per fabric edge port or using dynamic authentication through AAA or RADIUS using Cisco ISE. The same scalable group is configured on all fabric edge and border nodes

Scalable groups can be defined in Cisco Catalyst Center and/or Cisco ISE and are advertised through Cisco TrustSec

The fabric edge and border nodes include the SGT tag ID in each VXLAN header, which is carried across the fabric data plane

#### Anycast gateway

**Anycast gateway** provides Layer 3 default gateway where the same SVI is provisioned on every edge node with the same SVI IP and MAC add. This allows an IP subnet to be stretched across the SD-Access fabric. For example, if the subnet 10.1.0.0/24 is provisioned on an SD-Access fabric, this subnet will be deployed across all of the edge nodes in the fabric, and an endpoint located in that subnet can be moved to any edge node within the fabric without a change to its IP address or default gateway. Simplifying the IP address assignment and allowing fewer but larger IP subnets to be deployed. In essence, the fabric behaves like a logical switch that spans multiple buildings

#### Transit and peer networks

**Transit and peer networks** connect multiple fabric sites together or between a fabric site and the external world. They are configured on the border nodes of the fabric sites.

SD-Access transit uses a native SD-Access fabric with a domain-wide control plane node

IP-based transit uses a traditional IP-based network with VRF and SGT remapping

#### Transit Control Plane Node

**Transit Control Plane Node** construct that operates as a domain-wide control plane node for inter-site communication. It is only required when using SD-Access transits

It is part of the underlay network and needs to have reachability to the border nodes and Cisco DNA Center. It helps to exchange LISP mapping information between fabric sites

#### Fabric domain

**Fabric domain** hierarchical representation of fabric sites managed by Cisco DNA Center. A fabric domain can consist of multiple fabric sites and each site has its own devices that provide scale, resiliency and survivability. A fabric site is a logical grouping of devices that share the same control plane, data plane and policy plane.

#### Fabric in a box

**Fabric in a box** construct where the border node, control plane node, and edge node are running on the same fabric node

This may be a single switch, a switch with hardware stacking, or a StackWise Virtual deployment. The Fabric in a Box Site Reference Model should target less than 200 endpoints.

Collocated Design border and control node is on the same device

Distributed Design border and control plane node are on different devices. Additional configuration and iBGP peering required

The Cisco SD-Access and Cisco SD-WAN technology domains are integrated to enable communication between Cisco SD-Access sites across the Cisco SD-WAN fabric.

![](<../.gitbook/assets/Unknown image (1550)>)

### IPv6 support

Control plane node: The control plane node is configured to allow all IPv6 host subnets and the /128 host routes within the subnet ranges to be registered in its mapping database.

Border nodes: On the border nodes, IPv6 BGP peering with fusion devices is enabled. The border node decapsulates the IPv4 header from the fabric egress traffic while the ingress IPv6 traffic is encapsulated with the IPv4 header by the border nodes as well.

Fabric edge: All the switch virtual interfaces (SVIs) configured in Fabric Edge must be IPv6. This configuration is pushed by the Center Catalyst Center (DNA Center).

Cisco Catalyst Center (DNA Center): The Cisco Catalyst Center (DNA Center) physical interfaces do not currently support dual-stack. It can deploy only in a single stack with either IPv4 or IPv6 only in the management and or enterprise interfaces of the Cisco Catalyst Center (DNA Center).

Clients: Cisco SD-Access supports dual-stack (IPv4 and IPv6) or single stack either IPv4 or IPv6. However, when you deploy an IPv6 single stack, Cisco Catalyst Center (DNA Center) still requires creating a dual-stack pool to support an IPv6-only client. The IPv4 in the dual-stack pool is a dummy address only, as the IPv6 the client is expected to disable the IPv4 address.

### Underlay deployment

#### Manual (custom)

Advantages of manual is that it can be customized to fit the own requirements/compliance rules, suitable for for brownfield and greenfield deployments

Disadvantages are that the problems must be fixed by the administrators themselves (eg. MTU issues, improper routing/addressing, fragmentation, excessive latency, …)

Administrator must provide a fully functional IP-reachable underlay network (physical/logical interface configurations, control-plane protocols, address configurations) between all fabric-enabled devices and Cisco DNAC/ISE. Configuration can be either done fully manually on the devices themselves or using device templates defined in DNAC

The Loopback0 interface IP address (= “Fabric Node Router ID”) must be explicitly advertised in the IGP because it is the destination of packets destined from/to the device (eg. LISP Source Locator, …)

#### Automated (LAN Automation)

DNAC provisions a fully functional IP-reachable underlay network (autmatic discovery, physical/logical interface configurations, control-plane protocols, address configurations) between all fabric-enabled devices and Cisco DNAC/ISE. Meant for greenfield deployments. IS-IS is the only possible underlay routing protocol when using LAN Automation

Advantages: Eliminates misconfiguration and complexity and heavily simplifies and speeds up the building of the underlay network

Disadvantages: Cannot be customized to fit the own requirements/compliance rules because standardized design will be used for every DNAC/SDA deployment

Process

! In order for LAN Automation to work, the downstream devices need to be fully wiped to factory settings (everything including any certificates must be removed) !

1. Seed device/s (at least one, ideally two) must be added to DNAC either manually or via the Device Discovery feature and assigned to a site

Connection reachability has to be established between seed devices to allow DNAC for auto discovery and LAN automation

2. Global IP address Pool must be created and from the Global pool, we have to create Reserved IP address pool for each site
3. When LAN Automation is started, DNAC will push out a standardized (best practice) configuration to the seed device/s including IS-IS configuration, a temporary DHCP server (will be used for LAN Automation only and is deleted after it is complete) and puts the selected device ports in VLAN1
4. DNAC starts to discover all attached downstream network devices which are attached to the seed device and configures them

DNAC also discovers all network devices which are attached to already discovered network devices and/or are more than 1 CDP hop away

5. When everything is discovered and the status shows “Completed”, LAN Automation must be stopped and DNAC will remove some configuration (eg. DHCP server from seed device) and modify some configuration (eg. peer links will be configured to L3 instead of L2), then the LAN Automation is completed and the Routed Underlay network is ready to use

#### Plug and Play (PnP) onboarding

IPv4 is still required as of DNAC v1.3 (IPv6-only underlay is not possible)

A system MTU of 9100 is recommended to prevent fragmentation

Switches need “ip routing” enabled to participate in the fabric

A Loopback0 interface with a /32 mask, which is used as “Fabric Node Router ID”, is required

**Day-0 template / network profile**

Before using the Lan Automation process, a Day-0 template “Onboarding Template” has to be created, which is an initial device configuration applied to the device when claiming it, it can includes parameters like the hostname, loopback interface, …

Network Profile must be created which links to the Day-0 Template Network. It is attached to the fabric site the device gets assigned to

Normal PnP process for onboarding

Device (router, switches, APs) boots up and tries to acquire an IP address via DHCP on every port

The DHCP server provides the device not only with an IP address but also with the IP address of DNAC, either via…

Option A: DHCP Option 43 (if available)

Option B: Contact DNS server and ask for a name resolution of “pnpserver.localdomain” (localdomain = Domain provided via DHCP)

Option C: Cisco Cloud will be contacted and Daddress will be acquired form the Smart Account

Device will appear under Provision -> Devices -> Plug and Play as unclaimed device

Device must be claimed and a Day-0 template (“Onboarding Template”) can be applied to it which merges with the running-config

Pre-provisioned PnP process for onboarding

Device must be added to DNAC (Serial number, etc.) and added to a site so that the device will be automatically claimed when it gets added

Device (router, switches, APs) boots up and tries to acquire an IP address via DHCP on every port

The DHCP server provides the device not only with an IP address but also with the IP address of DNAC, either via…

Option A: DHCP Option 43 (if available)

Option B: Contact DNS server and ask for a name resolution of “pnpserver.localdomain” (localdomain = Domain provided via DHCP)

Option C: Cisco Cloud will be contacted and DNAC address will be acquired form the Smart Account

Device will appear under Provision -> Devices -> Plug and Play as claimed device
