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

# SD Access

### Challenges with traditional networks

A slow-to-deploy network impedes the ability of many organizations to innovate rapidly and adopt new technologies such as video, collaboration, and connected workspaces. The ability of a company to adopt any of these is impeded if the network is slow to change and adapt. In addition, one of the major challenges with wireless deployment today is that it does not easily utilize network segmentation. While wireless can leverage multiple service set identifiers (SSIDs) for traffic separation over the air, these are limited in the number that can be deployed and are ultimately mapped back into VLANs at the wireless LAN controller (WLC). The WLC itself has no concept of virtual routing and forwarding (VRF) or Layer 3 segmentation, making deployment of a true wired and wireless network virtualization solution very challenging.

Policy is one of those abstract words that can mean many different things to different people. However, in the context of networking, every organization has multiple policies that they implement. Use of security access control lists (ACLs) on a switch, or security rulesets on a firewall, is security policy. Using quality of service (QoS) to sort traffic into different classes, and using queues on network devices to prioritize one application versus another, is QoS policy. Placing devices into separate VLANs based on their role is device-level access control policy. The traditional methods used today for policy administration (large and complex ACLs on devices and ̀firewalls) are difficult to implement and maintain. Also, most organizations want to establish user and device identity for end-to-end policy. In addition, most organizations lack comprehensive visibility into network operation, limiting their ability to proactively respond to changes. All these issues influence how long it takes for a new network service to be deployed. A more comprehensive, end-to-end approach is needed, one that allows insights to be drawn from the mass of data that potentially can be reported from the underlying infrastructure.

### Software-Defined Access (SDA)

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

![](/broken/files/a0cc62778bfc93cf67d40d7e86b054772fd17b0f)

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

![](/broken/files/1337eb0bda70981a8fe1db4e2b13011cb7b2ccb5)

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
