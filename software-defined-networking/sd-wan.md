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

# SD WAN

### Software-Defined WAN

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
