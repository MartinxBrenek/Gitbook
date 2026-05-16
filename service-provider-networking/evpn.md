---
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
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

# EVPN

### Limitations of traditional L2VPN

Layer 2 VPN (L2VPN) technologies have evolved over time to meet scalability and operational needs. First, traditional Ethernet was improved with Institute of Electrical and Electronic Engineers (IEEE) 802.1Q or VLAN Tagging, allowing multiple bridged networks to transparently share the same physical network link. Then the IEEE 802.1ad standard further advanced Layer 2 bridging technologies by allowing the nesting of an extra virtual LAN (VLAN) tag on a packet, permitting service providers to use a single VLAN to support customers who have multiple VLANs. Further, the 802.1ah standard increased service instance scalability and MAC address scalability by encapsulating the end user’s traffic inside the service provider’s MAC header, and the 802.1aq and 802.1Qbp standards allow for multipathing.

![](<../.gitbook/assets/Unknown image (1884)>)

In terms of Internet Engineering Task Force (IETF) L2VPN technologies evolution, Virtual Private LAN Service (VPLS) was a widely adopted technology. Then, to increase the scalability of pseudowires (PW), hierarchical VPLS was standardized, followed by provider backbone bridge (PBB)-VPLS to better address the MAC address scale.

Although native Layer 2 bridging and L2VPN technologies have evolved over time, they still prove insufficient in supporting some of today’s networks. EVPN addresses the major shortcomings of the earlier technologies. EVPN can provide interdomain WAN connectivity, it works with IP and MPLS fabrics, it is widely supported and multivendor, and it allows for all-active multihoming and multipathing.

![](<../.gitbook/assets/Unknown image (1885)>)

MPLS Layer 2 VPNs based on PW have been widely deployed in service providers and enterprise networks. L2VPN applications range from Ethernet business services to fixed and mobile convergence and enterprise campus Layer 2 transport.

Recently, Data Center Interconnect (DCI) has become a leading application for Ethernet multipoint L2VPNs. As customers deploy virtualization to consolidate servers and become more flexible in their data centers, they demand higher agility, lower cost, and resource optimization among Data Center (DC) sites. With that in mind, a DCI solution must support the following:

Cloud bursting

Disaster recovery and business continuity

Workload (virtual machine \[VM]) and data (storage) mobility

In the case of Data Center Interconnect (DCI), VPLS cannot address solely several requirements.

First, to keep the data center always on and to use the resources and links as efficiently as possible, data centers require per-flow load balancing between data center switches and DCI routers.

Second, highly virtualized multitenant service provider and large-enterprise data centers require solutions that can address a high number of VLANs and MAC addresses.

Finally, fast convergence is crucial to reduce downtime and packet loss due to any network topology changes

Existing VPLS PE dual-homing solutions provide limited load-balancing support, including active-standby or active-active per-VLAN scenarios. If the traffic is heavier on one particular VLAN, administrative intervention is necessary to reassign VLANs to individual PEs manually.

In cases of per-VLAN load balancing, manual administration is necessary to compensate for the lack of access to autodiscovery and automatic service-carving mechanisms. VPLS cannot address the challenges associated with dual-homing and per-flow load balancing.

In addition, VPLS scalability, regarding the number of PEs, is limited by the maximum number of pseudowires that an implementation allows on a given VPLS VFI. (This number is typically in the low hundreds.)

![](<../.gitbook/assets/Unknown image (1886)>)

**Packet Reflection:** When both redundant Provider Edge (PE) routers are active at the originating site, packets can be reflected back into the same segment, causing unnecessary duplication and potential loops.

**Duplicate Frame Reception:** If the originating site has both redundant PEs active, the remote site may receive duplicate frames from the core network due to simultaneous transmission across active pseudowires.

**MAC Address Flapping:** MAC address flip-flopping can occur when frames are received from both redundant PEs over the pseudowire. This results in inconsistent MAC learning, as the same MAC address appears to be reachable via different PE routers in rapid succession

![](<../.gitbook/assets/Unknown image (1887)>)

### Ethernet VPN (EVPN)

**Ethernet VPN (EVPN)** is a standardized, multi-vendor technology used to provide Ethernet point-to-point and multipoint L2 services as well as traditional L3 services.

The core idea of EVPN is the separation of network control from actual data forwarding, which allows the same operating model to be used over different transport technologies such as MPLS or VXLAN.

In traditional Layer 2 VPN solutions so far MAC address learning and forwarding was performed by data plane - A PE router learns about customer MAC addresses by observing actual data traffic.

The BGP EVPN is similar as L3 VPN in terms of the control plane - it advertises both IP and MAC address as L3 VPN advertises a prefix

PE routers participating in EVPN instances learn customer MAC routes in the control plane and exchange them with other PE routers using the MP-BGP protocol. Control plane MAC learning brings several benefits allowing EVPN to address VPLS shortcomings, including support for multihoming with per-flow redundancy and load balancing.

When EVPN is used in conjunction with an MPLS data-plane, the BGP EVPN routes also signal the MPLS labels associated with MAC addresses and Ethernet segments.

No use of PWs - EVPN uses MP2P tunnels for unicast and multidestination frame delivery via ingress replication (via MP2P tunnels) or LSM for multicast

[https://www.ciscolive.com/c/dam/r/ciscolive/emea/docs/2023/pdf/BRKSPG-2473.pdf](https://www.ciscolive.com/c/dam/r/ciscolive/emea/docs/2023/pdf/BRKSPG-2473.pdf)

![](<../.gitbook/assets/Unknown image (1888)>)

### E-VPN Service Types

EVPN can provide a wide variety of service types. Besides E-LAN and E-LINE there is also an E-TREE Layer 2 VPN service type, Datacenter Fabric, EVPN Integrated Routing and Bridging (EVPN IRB), Datacenter Interconnect (DCI), and IP-VPN Layer 3 VPN services. For each traditional Layer 2 VPN technology there is an EVPN service type that provides identical or similar functionality.

For example, if you want to provide a LAN extension over the provider network you can use VPLS (as a traditional technology) or the the E-LAN service in the form of Provider Backbone Bridging EVPN (PBB-EVPN).

A similar example is if you want to provide a point-to-point Layer-2 VPN you can use pseudowires, or the the E-LINE service by using EVPN Virtual Private Wire Service (EVPN-VPWS).

In the case of the Network Virtualization Overlay, three data plane ecapsulations can be used: VXLAN, NVGRE and MPLS over GRE.

EVPN employs the established Cisco EVC to allow the configuration of the L2 or L3 VPN service

![](<../.gitbook/assets/Unknown image (1889)>)

#### EVPN IRB (integrated routing and bridging)

EVPN IRB integration of MPLS L3VPNs and L2VPNs

#### EVPN Provider Backbone Bridging (PBB-EVPN)

This solution improves scalability by introducing a further layer where B-MACs (backbone MAC addresses) represent a provider network bridge that represents local Ethernet segments connected to that PE device. The C-MACs (customer MAC addresses) are in turn learned from within the data plane mechanisms, unlike the non-PBB solution where they are learned via MP-BGP.

#### EVPN E-LAN

Distributed Layer-2 domain across the MPLS fabric

Acts as a single virtual Ethernet bridge interconnecting sites.

Integrated MAC learning and distribution via BGP EVPN

Elimination of manual pseudowire configuration

MAC addresses distributed via EVPN

![](<../.gitbook/assets/Unknown image (1890)>)

#### EVPN E-Tree

This solution implements the E-Tree service that provides efficient filtering. When traffic originates from a leaf and is destined to a leaf, it immediately drops at the ingress PE. It also provides flexible support of leaf or root site connectivity where a root or leaf designation can be an attachment circuit or per MAC address..

Consider L2, L3, and L4 as leaf ACs, and L1 as root AC. Root ACs can communicate with all other ACs. Leaf ACs can communicate with root ACs but not with other leaf ACs with either L2 unicast or L2 BUM traffic. If a PE is not configured as E-Tree leaf, it is considered as root by default. This feature only supports leaf or root sites per PE.

Root and leaf exports or imports single Routed Targets (RTs)

![](<../.gitbook/assets/Unknown image (1891)>)

#### EVPN E-Tree with IRB (ETREE-IRB)

Root to leaf

Leaf to leaf inter-subnet

Leaf to leaf intra-subnet prohibited

![](<../.gitbook/assets/Unknown image (1892)>)

### EVPN Advantages

Integrated Services

Integrated Layer 2 and Layer 3 VPN services

L3VPN-like principles and operational experience for scalability and control

BGP integrates services with programmable SR transport

Services Control Plane is BGP with different AF / SAFI

Single Service Control Plane is easy to manage and troubleshoot

No technical benefit to replace them with EVPN L3!!

![](<../.gitbook/assets/Unknown image (1893)>)

#### Network Efficiency

All-active Multi-homing & PE load-balancing (ECMP)

Fast convergence (link, node, MAC moves)

Control-plane (BGP) learning. PWs are no longer used.

Optimized Broadcast, Unknown-unicast, Multicast traffic delivery

#### Multihoming

Prior to EVPN, mostly proprietary dual-homing solutions were possible and the amount of PE routers to which a CE device could connect was limited to two. These two PE routers needed to have an inter-chassis link to enable Layer 2 loop prevention mechanisms or support multilink aggregation solutions. Examples of these two would be an inter-chassis Layer 2 trunk link to support the Spanning Tree Protocol, or a Virtual PortChannel (vPC) peer link that is required to bring up the vPC itself.

With EVPN you can have n-way redundancy - there is no longer a limit to connect your CE device to only two PE routers. There is also no need for interchassis links.

#### Load Balancing

EVPN provides per-flow load balancing among egress PEs by using BGP multipathing. Per-flow load balancing between ingress and egress PEs is provided with the use of IGP ECMP.

EVPN can efficiently use all available bandwidth. Solutions before EVPN generally had the following limitations:

MPLS/IP fabrics, data, and control plane scale issues exist: a full mesh of pseudowires, learning of all MACs over a pseudowire, and a full mesh of targeted LDP sessions

You can't impose a MAC-based policy e.g. to route traffic for a specific MAC address over a specific path

Single-active multihoming support only allowing you to use only a single link out of two available

In terms of efficient fabric bandwidth utilization, EVPN has the following capabilities:

MAC learning in BGP makes it possible for removing PWs and targeted LDP sessions - this simplifies management of a service provider network

Support for policy pertaining to individual MACs and ingress filtering. You can apply fine-grained policies (per EVI, per VLAN, per VRF).

Simplifying operation via autosensing and autoconfiguration of Ethernet segments

Service Flexibility

Choice of MPLS, VxLAN or SRv6 data plane encapsulation

Support existing and new services types (E-LAN, E-Line, E-TREE)

Peer PE auto-discovery. Redundancy group auto-sensing

Fully support IPv4 and IPv6 in the data plane and control plane

Investment Protection

Open-Standard and Multi-vendor support

### EVPN BGP Control Plane

Control-plane learning offers greater control over the MAC learning process, such as restricting who learns what, and the ability to apply policies.

This provides flexibility and the ability to preserve the "virtualization" or isolation of groups of interacting agents (hosts, servers, virtual machines) from each other.

In EVPN, PEs advertise the MAC addresses learned from the CEs that are connected to them, along with an MPLS label, to other PEs in the control plane using MP-BGP.

Control-plane learning enables load balancing of traffic to and from CEs that are multihomed to multiple PEs- this is in addition to load balancing across the MPLS core via multiple LSPs between the same pair of PEs.

Learning between PEs and CEs is done by the method best suited to the CE: data-plane learning, IEEE 802.1x, the Link Layer Discovery Protocol (LLDP), IEEE 802.1aq, Address Resolution Protocol (ARP), management plane, or other protocols.

It is a local decision as to whether the Layer 2 forwarding table on a PE is populated with all the MAC destination addresses known to the control plane, or whether the PE implements a cache-based scheme. For instance, the MAC forwarding table may be populated only with the MAC destinations of the active flows transiting a specific PE.

![](<../.gitbook/assets/Unknown image (1894)>)

![](<../.gitbook/assets/Unknown image (1895)>)

### EVPN BGP Data Plane

In the data plane, EVPN supports multiple protocols, such as SR/SRv6/MPLS/PBB+MPLS. Also supported are network virtualization overlays such as VXLAN, Network Virtualization Using Generic Routing Encapsulation (NVGRE), and MPLS over Generic Routing Encapsulation (MPLSoGRE).

L2VPN Services Overlay Encapsulation

![](<../.gitbook/assets/Unknown image (1896)>)

L3VPN Services Overlay Encapsulation

![](<../.gitbook/assets/Unknown image (1897)>)

![](<../.gitbook/assets/Unknown image (1898)>)

### EVPN terminology and concepts

![](<../.gitbook/assets/Unknown image (1899)>)

#### EVPN instance (EVI)

**EVPN instance (EVI)** represents a VPN on a PE router. It serves the same role as an IP virtual routing and forwarding instance (VRF) - EVIs are assigned import/export route targets (RTs).

E-VPN Instance (EVI) spanning the PE devices participating in the EVI

An EVI consists of one or more broadcast domains. Depending on the service multiplexing behaviors at the user-to-network interface (UNI), all traffic on a port (all-to-one bundling), or traffic on a VLAN (one-to-one mapping), or traffic on a list or range of VLANs (selective bundling) can be mapped to a bridge domain (BD). The BD is then associated with an EVI for forwarding in the MPLS core.

#### Ethernet Tag ID

**Ethernet Tag ID** is a 12-bit or 24-bit identifier that identifies a particular broadcast domain (e.g., a VLAN) in an EVPN instance.

An EVPN instance consists of one or more broadcast domains (one or more VLANs). VLANs are assigned to a given EVPN instance by the provider of the EVPN service.

A given VLAN can itself be represented by multiple VIDs. In such cases, the PEs participating in that VLAN for a given EVPN instance are responsible for performing VLAN ID translation to/from locally attached CE devices.

![](<../.gitbook/assets/Unknown image (1900)>)

#### Service interface models

**VLAN-based service interface**

Each VLAN is associated to one bridge domain and one EVI.

The Route Distinguishers and Router Targets, which are used to share routes between different VRFs, are autogenerated to ensure unique Route Distinguisher numbers across EVIs.

1:1 mapping between VLAN ID and EVI (MAC-VRF)

Single bridge domain for each EVI

This enables VID translation, because each VLAN is tracked independently inside the EVI.

Ethernet tag in EVPN route is set to 0

![](<../.gitbook/assets/Unknown image (1901)>)

**VLAN bundling service interface**

Multiple VLANs share the same bridge table

Each EVPN instance corresponds to multiple broadcast domains maintained in a single bridge table per MAC-VRF.

MAC addresses must be unique across all VLANs for an EVI

Multiple broadcasts domains (VLANs)

N:1 mapping between VLAN ID and EVI (MAC-VRF)

Single bridge domain for each EVI

VLAN translation is not allowed

VID translation is NOT permitted - since multiple VLANs are treated as a single “bundle”, and if VLAN IDs were allowed to differ across sites, the PE wouldn’t know how to map them consistently since all VLANs collapse into one MAC-VRF, each VLAN must be represented by the same VID everywhere

Ethernet tag in route is set to 0

![](<../.gitbook/assets/Unknown image (1902)>)

**Port-based service interface**

All VLANs on the port are part of the same service and map to the same bundle.

All VLANs on a port share one bridge table.

VLAN translation is not allowed - Since they are all collapsed together, EVPN cannot distinguish them.

Ethernet tag in route is set to 0

![](<../.gitbook/assets/Unknown image (1903)>)

**VLAN-aware bundling service interface**

For VLAN-aware Bundle Service Interface, each VLAN is associated with one bridge domain, but there can be multiple bridge domains associated with one EVI.

An EVPN instance consists of multiple broadcast domains where each VLAN has one bridge table. Multiple bridge tables (one per VLAN) are maintained by a single MAC-VRF that corresponds to the EVPN instance.

Multiple broadcast domains (VLANs)

N:1 mapping between VLAN ID and EVI (MAC-VRF)

This enables VID translation, because each VLAN is tracked independently inside the EVI.

Ethernet Tag ID set to VLAN ID automatically or manually

![](<../.gitbook/assets/Unknown image (1904)>)

**Port-based VLAN-aware bundle service interface**

All VLANs on the port are part of the same service and are mapped to a single bundle

Each VLAN on the port still has its own bridge table (per-VLAN separation).

VLAN translation is not allowed, because VLANs are tracked independently, translation could theoretically work, but RFC restricts this case to no VID translation

Ethernet Tag ID set to VLAN ID automatically or manually

![](<../.gitbook/assets/Unknown image (1905)>)

#### Ethernet segment (ES) and ESI

**Ethernet segment (ES)** represent a site associated with the access-facing interfaces (physical or logical Ethernet bundles)

Ethernet segment can be a single device (i.e., CE router) or an entire network

Devices and networks can be single-homed (SHD, SHN) or multihomed (MHD, MHN)

Multi-Homed includes Dual-Homed but means “really MULTI” = 2,3,4,5, … (however there is HW/implementation limitation)

For a multihomed site, each ES is identified by a unique nonzero 10-byte identifier called an **Ethernet Segment Identifier (ESI)**

An Ethernet segment should have a nonreserved ESI that is unique across the whole network (i.e., across all EVPN instances on all the PEs)

This is required to enable auto-discovery of Ethernet segments and Designated Forwarder (DF) election.

They help EVPN PEs identify shared segments and enable advanced features like All-Active Multihoming and Loop Avoidance.

An ESI can be autodiscovered with the use of LACP or MST.

![](<../.gitbook/assets/Unknown image (1906)>)

The following values are reserved:

**ESI 0:** single-homed site

**ESI all 1 ({0xFF} (repeated 10 times) ):** MAX-ESI

The last byte always identifies the ESI Type (e.g., 0x01 for Type 1, etc.)

ESI = 9 octets of value + 1 octet of Type.

ESI Types determine how the 9-octet ESI is built, depending on your topology and multi-homing method. They help EVPN PEs identify shared segments and enable advanced features like All-Active Multihoming and Loop Avoidance.

T (ESI Type) is a 1-octet field (most significant octet) that specifies the format of the remaining 9 octets (ESI Value). The following six ESI types can be used:

| **T (ESI Type)**    | **Description**                                                                                                                               | **ESI Value**                                                                                                                                                                                |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Type 0 (T=0x00)** | Arbitrary value                                                                                                                               | Arbitrary 9-octet ESI, which is managed and configured by the operator Recommended by cisco The 10th octet is prepended as double zero to the 9octet ESI to indicate manually configured ESI |
| **Type 1 (T=0x01)** | IEEE 802.1AX LACP is used between PEs and CEs (typically MHD). Autogenerated and determined from LACP                                         | (CE LACP System MAC address \[6 octets]) + (CE LACP Port Key \[2 octets]) + (0x00)                                                                                                           |
| **Type 2 (T=0x02)** | Hosts are connected via a bridged LAN between CEs and PEs (typically MHN). Autogenerated and determined based on Layer 2 bridge protocol BPDU | (Root Bridge MAC address \[6 octets]) + (Root Bridge Priority \[2 octets]) + (0x00)                                                                                                          |
| **Type 3 (T=0x03)** | MAC-based ESI value that can be autogenerated or configured by the operator                                                                   | (PE System MAC address \[6 octets]) + (Local Discriminator value \[3 octets])                                                                                                                |
| **Type 4 (T=0x04)** | Router-ID ESI value that can be autogenerated or configured by the operator                                                                   | (Router ID \[4 octets]) + (Local Discriminator \[4 octets]) + (0x00)                                                                                                                         |
| **Type 5 (T=0x05)** | Autonomous system (AS)-based ESI value that can be autogenerated or configured by the operator                                                | (AS number \[4 octets]) + (Local Discriminator \[4 octets]) + (0x00)                                                                                                                         |

#### Ethernet Segment – ESI Autosense

![](<../.gitbook/assets/Unknown image (1907)>)

### BGP EVPN routes

The EVPN NLRI is carried in BGP with the use of multiprotocol extensions with an address family identifier (AFI) of 25 (AFI for L2VPN information) and a new subsequent address family identifier (SAFI) of 70 (EVPN)

BGP capabilities advertisements are used to ensure that two speakers support EVPN NLRI. The EVPN NLRI includes a route type, length, and route type–specific fields.

![](<../.gitbook/assets/Unknown image (1908)>)

BGP EVPN Routes serve control plane purposes, including:

MAC address reachability

MAC mass withdrawal

Split-horizon label advertisement

Aliasing (load-balancing)

Multicast endpoint discovery

Redundancy group discovery

Designated forwarder election

IP address reachability

#### BGP EVPN route types

| Route Type | Name                               | Purpose                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ---------- | ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0x1        | Ethernet Autodiscovery (A-D) Route | <p>Carries list of EVIs that belong to an Ethernet segment, and is used for MAC Mass-Withdraw; Aliasing (Ethernet A-D per EVI route); Back-up path; Split-horizon filtering (encodes ESI label) Tagged with ESI label Extended community<img src="../.gitbook/assets/Unknown image (1909)" alt=""> Constructing the Eth A-D Route per Eth Segment The value of RD field comprises an IP address of the PE (typically, the loopback address) followed by 0. The Ethernet Segment Identifier MUST be a ten-octet entity (10 octets) The Ethernet Tag ID MUST be set to 0. The "ESI MPLS Label Extended Community" MUST be included in the route The MPLS label in this Extended Community is referred to as an "ESI label". This label MUST be a downstream assigned MPLS label if the advertising PE is using ingress replication for receiving multicast, broadcast or unknown unicast traffic from other PEs. If the advertising PE is using P2MP MPLS LSPs for sending multicast, broadcast or unknown unicast traffic, then this label MUST be an upstream assigned MPLS label.</p><p>The Ethernet A-D route MUST carry one or more RTs. These RTs MUST be the set of RTs associated with all the EVIs to which the Ethernet Segment belongs. Since each ethernet segment with it's ESI value can be provisioned with multiple EVI's - there are two types of Ethernet Auto-Discovery routes advertised by the PE's connecting multihomed segment: - Per ESI and Per EVI:<img src="../.gitbook/assets/Unknown image (1910)" alt=""> Per ESI RT1: This route handles mass withdrawal and split-horizon filtering. So for a given ESI, there is a single per-ESI RT1, regardless of how many EVIs are present. The Ethernet Tag ID: Set to MAX-ET (0xFFFFFFFF) to indicate it is a Per-ESI route. Per EVI RT1: The significance of this route is to advertise the aliasing label, which is used by the other PE in order to load balance the traffic across multiple PE owning the ESI. </p><p></p><p>Downstream-assigned The receiver (downstream PE) assigns the label and advertises it upstream to the sender. </p><p></p><p>Upstream-assigned:</p><p>The sender (upstream PE) assigns the label and advertises it downstream to the receiver</p> |
| 0x2        | MAC/IP Advertisement Route         | Advertise MAC address reachability Advertise IP/MAC bindings for ARP broadcast suppression Tagged with MAC Mobility Extended Community Note MAC addresses originated on PE share the same MPLS label — there is no per-MAC label allocation Note The label is locally significant – thus it can happen PE's connected to the multi-homed segment advertise same label for the EVI![](<../.gitbook/assets/Unknown image (1911)>)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| 0x3        | Inclusive Multicast Route          | Multicast Tunnel Endpoint Discovery Handling of Multi-destination Traffic (Broadcast/Unknown Unicast/Multicast - BUM) Indicates Interest of BUM traffic for attached L2 segments![](<../.gitbook/assets/Unknown image (1912)>)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 0x4        | Ethernet Segment Route             | Autodiscovery of multihomed Ethernet segment—i.e., Redundancy Group Discovery; Designated forwarder (DF) election Tagged with ES-import Extended Community![](<../.gitbook/assets/Unknown image (1913)>) Constructing the Ethernet Segment Route The value field comprises of the Route-Distinguisher (RD) and loopback IP of the PE followed by 0’s. The Ethernet Segment Identifier MUST be set to the ten octet ESI identifier. The BGP advertisement that advertises the Ethernet Segment route MUST also carry an ES-Import extended community attribute The Ethernet Segment Route filtering MUST be done such that the Ethernet Segment Type 4 Route is imported only by the PEs that are multi-homed to the same Ethernet Segment. To that end, each PE that is connected to a particular Ethernet segment constructs an import filtering rule to import a route that carries the ES-Import extended community, constructed from the ESI. As a result the Ethernet Segment route is imported only by the PEs that are connected to the same Ethernet segment.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| 0x5        | IP Prefix Advertisement Route      | Advertise IP prefixes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

#### Official IANA allocation

![](<../.gitbook/assets/Unknown image (1914)>)

![](<../.gitbook/assets/Unknown image (1915)>)

#### Route Distinguisher (RD) and Route Targets

RD MUST be assigned for a given MAC-VRF on a PE.

This RD MUST be unique across all MAC-VRFs on a PE.

On cisco devices the value is autogenerated on PE. Value comprises of IP address of the PE BGP router-id followed by a value of EVI ID.

It is recommended to configure RD manually in real deployments

![](<../.gitbook/assets/Unknown image (1916)>)

The EVPN route may carry one or more Route Target (RT) attributes.

RTs may be configured (as in IP VPNs) or may be derived automatically.

The use of RT Constraints allows each EVPN route to reach only those PEs that are configured to import at least one RT from the set of RTs carried in the EVPN route.

#### BGP EVPN extended communities

Extended communities are prepended to signal additional attributes:

<table><thead><tr><th>Attribute</th><th>Purpose</th></tr></thead><tbody><tr><td>ESI MPLS Label Extended Community</td><td><p>Encode Split-Horizon Label for Ethernet Segment and Indicate Redundancy Mode (Active/Standby vs. All-Active) transitive Extended Community having a Type field value of 0x06 and the Sub-Type 0x01 The ESI Label field represents an ES by the advertising PE, and it is used in split-horizon filtering by other PEs that are connected to the same multihomed Ethernet segment The low-order bit of the Flags octet is defined as the "Single-Active" bit. A value of 0 means that the multihomed site is operating in All-Active redundancy mode, and a value of 1 means that the multihomed site is operating in Single-Active redundancy mode The second low order bit of the flags octet is defined as the "Root-Leaf". A value of 0 means that this label is associated with a Root site; whereas, a value of 1 means that this label is associate with a Leaf site. The other bits must be set to 0.</p><p><img src="../.gitbook/assets/Unknown image (1917)" alt=""></p></td></tr><tr><td>ES-Import Extended Community</td><td><p>Transitive Route Target extended community carried with the Ethernet Segment route having a Type field value of 0x06 and the Sub-Type 0x02 Nodes which share same ESI import this ethernet segment routes The value is derived automatically for the ESI Types 1, 2, and 3, by encoding the high-order 6-octet portion of the 9-octet ESI Value, which corresponds to a MAC address, in the ES-Import Route Target. In IOS-XR configured in the evpn context </p><pre><code> evi 500
  bgp
   route-target import 1:500
   route-target export 1:500
</code></pre><p><img src="../.gitbook/assets/Unknown image (1918)" alt=""></p></td></tr><tr><td>MAC Mobility Extended Community</td><td>Indicate that a MAC address has moved from one segment to another across Leafs transitive Extended Community having a Type field value of 0x06 and the Sub-Type 0x00 It may be advertised along with MAC/IP Advertisement routes. The low-order bit of the Flags octet is defined as the "Sticky/static" flag and may be set to 1. A value of 1 means that the MAC address is static and cannot move. The sequence number is used to ensure that PEs retain the correct MAC/IP Advertisement route when multiple updates occur for the same MAC address.<img src="../.gitbook/assets/Unknown image (1919)" alt=""></td></tr><tr><td>Default Gateway Extended Community</td><td>Indicate the MAC/IP bindings of a gateway MAC/IP Advertisement Route</td></tr></tbody></table>

### Multihoming and suppression mechanisms in EVPN

Multihome-d Ethernet segment autodiscovery: PEs that connect to the same Ethernet segment can automatically discover each other through the exchange of BGP Ethernet segment Type 4 routes Ethernet segment routes must carry an ES-Import RT that is constructed from the ESI. As such, the ES-Import RTs are identical on PEs that connect to the same Ethernet segments, and the Ethernet segment route imports only between the PEs that are multihomed to the same Ethernet segment as part of the Ethernet segment route filtering.

#### MAC mass-withdraw

Challenge: How to inform remote PEs of a failure affecting many MAC addresses quickly while the control-plane re-converges?

Since BGP is a stateful the convergence e.g the BGP withdrawal of each MAC address would depend on the number of the MAC addresses for that segment. This convergence would poses very high overhead and blackhauled traffic - since the PE's would still send the traffic to the destination PE that is still converging

E-VPN defines a mechanism to efficiently and quickly signal, to remote PE nodes, the need to update their forwarding tables upon the occurrence of a failure in connectivity to an Ethernet segment.

This is done by having each PE advertise an Ethernet A-D Route per Ethernet segment for each locally attached segment.

Upon a failure in connectivity to the attached segment, the PE withdraws the corresponding Ethernet A-D route. This triggers all PEs that receive the withdrawal to update their next-hop adjacencies for all MAC addresses associated with the Ethernet segment in question. If no other PE had advertised an Ethernet A-D route for the same segment, then the PE that received the withdrawal simply invalidates the MAC entries for that segment. Otherwise, the PE updates the next-hop adjacencies to point to the backup PE(s).

#### Split horizon for Ethernet segments in EVPN

Challenge: How to prevent flooded traffic from echoing back to a multi-homed Ethernet Segment?

PE advertises in BGP a split-horizon label (ESI MPLS Label) associated with each multi-homed Ethernet Segment.

Split-horizon label is only used for multi-destination frames (Unknown Unicast, Multicast & Broadcast).

Split-horizon in EVPN is implemented using both:

An extended community in BGP (ESI MPLS Label Extended Community), and an additional MPLS label inserted into the label stack for multi-destination traffic.

This is not a forwarding label, just a signaling mechanism to tell other PEs what label to expect for split-horizon.

When an ingress PE sends BUM (Broadcast, Unknown Unicast, Multicast) traffic, it pushes the advertised split-horizon label into the MPLS label stack (typically below the EVPN service label).

This label tells the remote PE that the traffic came from a specific ES, so it should not forward it back to that same ES (i.e., avoid echo).

![](<../.gitbook/assets/Unknown image (1920)>)

There may be a scenario, when the L1 would have to send traffic to L2 from the C1 ESI to the C11 ESI. Since the L2 is also connected to the C1 ESI it must not send the traffic back to the ESI C1 – this is where the SH label comes in – otherwise the L1 would not send the frame to L2 if it was destined to a different PE

{% hint style="info" %}
Route Type 1 that conveys SH label is recognized by specific parameter in the route number \[4294967295]
{% endhint %}

![](<../.gitbook/assets/Unknown image (1921)>)

RP/0/RP0/CPU0:PE011-NCS540-1#sh bgp l2vpn evpn rd 1.1.1.11:501 \[1]\[0010.1010.1010.1010.1010]\[4294967295]/120

Sun Oct 19 12:54:10.849 UTC

BGP routing table entry for \[1]\[0010.1010.1010.1010.1010]\[4294967295]/120, Route Distinguisher: 1.1.1.11:501

Versions:

Process bRIB/RIB SendTblVer

Speaker 527 527

Last Modified: Oct 19 11:37:31.780 for 01:16:39

Paths: (1 available, best #1)

Not advertised to any peer

Path #1: Received by speaker 0

Not advertised to any peer

Local

1.1.1.12 (metric 200) from 172.18.0.152 (1.1.1.12)

Received Label 0

Origin IGP, localpref 100, valid, internal, best, group-best, import-candidate, imported, rib-install

Received Path ID 0, Local Path ID 1, version 527

Extended community: EVPN ESI Label:0x00:128003 RT:1:201 RT:1:401 RT:1:501

Originator: 1.1.1.12, Cluster list: 172.18.0.152

Source AFI: L2VPN EVPN, Source VRF: default, Source Route Distinguisher: 1.1.1.12:1

#### Split horizon for Ethernet segments in PBB-EVPN

There is no split-horizon label. Instead, split horizon is enforced using B-MAC addresses.

it is implied and enforced via local knowledge and consistent configuration

PBB encapsulation wraps the original Ethernet frame in a new MAC header with source B-MAC and destination B-MAC

In multi-homing, all PEs attached to the same Ethernet Segment (ESI) use the same B-MAC.

This B-MAC is tied to the ESI (1:1 mapping between B-MAC and ESI).

When PE sends BUM traffic (Broadcast, Unknown Unicast, Multicast), it encapsulates it using its B-MAC as the source in the PBB header.

At the receiving PE, it checks:

"Is this B-MAC associated with one of my local Ethernet Segments?"

If yes, it means the frame came from a PE that shares the same Ethernet Segment (e.g., All-Active MH).

In that case, the egress PE discards the frame on that segment — i.e., split horizon is enforced based on B-MAC match, not label.

#### Split horizon for core tunnels

Challenge: How to prevent flooded traffic from looping back over the core?

Same as in VPLS - Traffic received from an MPLS tunnel over the core is never forwarded back to the MPLS core.

#### Aliasing (BGP multipath)

Challenge: How to load-balance traffic towards a multihomed device across multiple PEs when MAC addresses are learnt by only a single PE?

In an EVPN all-active multihoming setup, a Customer Edge (CE) device connects to multiple Provider Edge (PE) routers via a single Ethernet Segment (ES).

Each PE advertises the ES using an Ethernet Segment Identifier (ESI) and participates in Designated Forwarder (DF) election per VLAN to control BUM traffic.

Now, when a PE learns a MAC address from that CE, it advertises a Type-2 MAC/IP route that includes the MAC, its own IP (the next hop), and the associated ESI.

If EVPN stopped there, remote PEs would only see that single advertising PE as the next hop for that MAC.

This means that unicast traffic from remote PEs toward the CE would always go through the PE that originally advertised the MAC — preventing load balancing and full utilization of all-active uplinks.

EVPN introduces the aliasing mechanism to ensure that remote PEs can load-balance unicast traffic toward a multihomed CE, even if only one PE actually learned the MAC at first or due to the load balancing algorithm of CE

All PEs connected to the same Ethernet Segment advertise a Type-1 Ethernet A-D route that carries the shared Ethernet Segment Identifier (ESI).

These routes indicate that the PEs are part of the same all-active segment.

The non-owner PEs (the other PEs attached to the same ES that did not learn that MAC) still advertise an "alias" for that ESI — effectively saying “I can also reach that ESI,” even if they didn’t learn the MAC locally.

For unicast traffic, any PE on the ES can receive packets and deliver them correctly to the CE.

For BUM and unknown-unicast traffic, the DF election per VLAN ensures that only one PE forwards such traffic to the CE, avoiding duplication.

![](<../.gitbook/assets/Unknown image (1922)>)

The key point is that aliasing is only required when a MAC is learned by a single PE on an all-active Ethernet Segment. EVPN does not forbid multiple PEs from advertising the same MAC/IP as Type-2 routes; it only defines how remote PEs should behave when that does or does not happen. Your original notes correctly describe the worst-case scenario that aliasing was designed to solve, not a mandatory steady state.

In an all-active multihomed ES, if the CE uses a hashing algorithm that sends traffic for that host consistently across both uplinks, or if the host itself emits frames that arrive on both PEs, then both PEs will independently learn the same MAC and IP from the data plane. When that happens, each PE legitimately originates a Type-2 MAC/IP route for that MAC, with its own RD and next hop, exactly as you observed. EVPN treats these as equivalent advertisements for the same endpoint, and remote PEs install them as equal-cost paths via normal BGP multipath. In this situation, aliasing is effectively unnecessary because the control plane already has multiple Type-2 paths.

Aliasing only activates conceptually when there is asymmetry in learning. If the CE hashes all traffic for that host to a single uplink, only one PE learns the MAC. The other PE still advertises the Type-1 Ethernet A-D route for the ESI, which allows remote PEs to infer that additional PEs can forward traffic for that MAC even though they did not originate a Type-2 for it. That is the specific gap aliasing fills. When both PEs advertise Type-2 routes, there is no gap to fill.

#### Backup path

PEs advertise in BGP the ESIs of local multi-homed Ethernet Segments.

Active/Standby Redundancy Mode is indicated

When PE learns a MAC address on its AC, it advertises the MAC in BGP along with the ESI of the Ethernet Segment from which the MAC was learnt.

Remote PEs will install:

active path to the PE that advertised both MAC Address & ESI

backup path to the PE that advertised ESI only

![](<../.gitbook/assets/Unknown image (1923)>)

#### Designated Forwarder (DF)

Challenge: How to prevent duplicate copies of flooded traffic from being delivered to a multi-homed Ethernet Segment (BUM traffic)?

PEs that connect to a multihomed Ethernet segment discover each other via BGP. These PEs then elect a designated forwarder responsible for forwarding flooded multidestination frames (BUM traffic) to the multihomed segment. The role of the designated forwarder is to decapsulate and forward BUM traffic that originates from the remote segments to the destination local segment for which the device is the designated forwarder. The default procedure for designated forwarder election is referred to as service carving. With service carving, it is possible to elect multiple designated forwarders per Ethernet segment (one per VLAN or VLAN bundle) to load balance multidestination traffic destined for a given segment. The load-balancing procedures carve up the VLAN space per Ethernet segment among the PE nodes evenly so that every PE is the designated forwarder for a disjoint set of VLANs or VLAN bundles for that Ethernet segment

DF Election granularity can be:

Per Ethernet Segment (Single PE is the DF)

Per EVI (E-VPN) or I-SID (PBB-EVPN) on Ethernet Segment (Multiple DFs for loadbalancing) – Service Carving

![](<../.gitbook/assets/Unknown image (1924)>)

**Service carving** refers to the algorithm used to elect a Designated Forwarder (DF) among PE (Provider Edge) routers that are dual-homed to the same Ethernet Segment (ES). It determines which PE is responsible for forwarding broadcast, unknown unicast, and multicast (BUM) traffic for a given VLAN (a.k.a. EVI – Ethernet Virtual Instance).

#### Default (Modulo-Based) DF Election = “Service Carving”

Defined in RFC 7432, the default DF election is based on a modulo algorithm:

DF index = VLAN\_ID % N

VLAN\_ID = the VLAN or service ID (EVI),

N = number of PEs connected to the Ethernet Segment,

PEs are sorted by IP address and indexed from 0.

This results in different VLANs being served by different PEs (if the IDs are diverse), allowing load balancing between them.

Process:

1. When a PE discovers the ESI of the attached Ethernet segment, it advertises an Ethernet Segment route with the associated ES-Import extended community attribute.
2. The PE then starts a timer (default value = 3 seconds) to allow the reception of Ethernet Segment routes from other PE nodes connected to the same Ethernet segment. This timer value should be the same across all PEs connected to the same Ethernet segment.
3. When the timer expires, each PE builds an ordered list of the IP addresses of all the PE nodes connected to the Ethernet segment (including itself), in increasing numeric value. Every PE is then given an ordinal indicating its position in the ordered list, starting with 0 as the ordinal for the PE with the numerically lowest IP address. The ordinals are used to determine which PE node will be the DF for a given EVPN instance on the Ethernet segment, using the following rule:

(V mod N) = i (where V is EVI/VLAN N is the number of PE nodes in a group and i is the ordinal of each DF)

Modulo (nebo modulární aritmetika) je matematická operace, která vrací zbytek po dělení.

Example:

PE1: rid 1.1.1.1 position 0

PE2: rid 2.2.2.2 position 1

PE3: rid 3.3.3.3 position 2

EVI modulo Number of Nodes = position

EVI 10 modulo 3 = 1 ==> DF PE2

(10%3 = 1 → 10 ÷ 3 = 3 \* 3 = 9, remainder is 1)

EVI 11 modulo 3 = 2 ==> DF PE3

(11%3 = 2 → 11 ÷ 3 = 3 \* 3 = 9, remainder is 2)

EVI 12 modulo 3 = 0 ==> DF PE1

(12%3 = 0 → 12 ÷ 3 = 3 \* 4 = 12, remainder is 0)

4. The PE that is elected as a DF for a given VLAN will unblock multi-destination traffic for that VLAN or VLAN bundle on the corresponding ES. Note that the DF PE unblocks multi-destination traffic in the egress direction towards the segment. All non-DF PEs continue to drop multi-destination traffic in the egress direction towards CE

In the case of link or port failure, the affected PE withdraws its Ethernet Segment route. This will trigger the service carving procedures on all the PEs in the redundancy group.

Manual load balancing per EVI

Default : even on one PE, odd on other PE

!! Manual carving config should match on both PEs (NOT advertised)

![](<../.gitbook/assets/Unknown image (1925)>)

![](<../.gitbook/assets/Unknown image (1926)>)

#### Limitations of Modulo-Based DF Election

Imbalanced distribution:

If all VLANs are even-numbered (e.g., 100, 102, 104), they all elect the same PE, defeating load balancing.

No awareness of interface/link status:

A PE might be elected DF even if its local access link (AC) is down, causing black-holing of traffic.

These limitations are addressed in RFC 8584 and other enhancements.

Enhanced DF Election Methods (RFC 8584)

RFC 8584 introduces advanced service carving mechanisms:

HRW (Highest Random Weight) Algorithm

Uses hashing of ESI, Ethernet Tag, and PE IPs to evenly and predictably distribute DF roles.

More balanced and deterministic than modulo-based.

Avoids re-election when PEs flap.

AC-DF Capability

Ensures a PE can only be elected DF if its Attachment Circuit (AC) is up.

Prevents DF election to non-operational devices.

Service Carving Time (SCT) – RFC 9722

When a new PE comes up, DF roles should not switch instantly.

SCT provides a graceful and synchronized time for all PEs to perform DF election.

Prevents traffic disruption and unnecessary failover.

Cisco Implementation (IOS-XR, Catalyst, etc.)

You can configure the DF election as preference-based or access driven.

In a preference-based DF election mechanism, the weight decides which PE is the DF at any given time. You can use this method for topologies where interface failures are revertive. However, for topologies where an access-PE is directly connected to the core PE, use the access-driven DF election mechanism.

When access PEs are configured in a non-revertive mode, the access-driven DF election mechanism allows the access-PE to choose which PE is the DF.

Consider an interface in an access network that connects PE nodes with the EVPN PE in the core network. When this interface fails, there may be a traffic loss for a longer duration. The delay in convergence is because the backup PE is not chosen before failure occurs.

The EVPN DF Election feature allows the EVPN PE to preprogram a backup PE even before the failure of the interface. In the event of failure, the PE node will be aware of the next PE that will take over, thereby reducing the convergence time. Use the preference df weight option for an Ethernet segment identifier (ESI) to set the backup path. By configuring the weight for a PE, you can control the DF election, thus define the backup path.

service-carving auto — Default modulo-based election

service-carving preference — Manual DF priority configuration

non-revertive — Maintains current DF unless manual re-election is triggered

HRW-based election on platforms that support RFC 8584

#### ARP broadcast suppression

Challenge: How to reduce ARP broadcasts over the MPLS/IP network, especially in large scale virtualized server deployments

Construct ARP caches on the E-VPN PEs and synchronize them either via BGP or data-plane snooping.

Initial ARP requests broadcast to all the sites. Subsequent ARP requests are suppressed, and the PEs act as ARP proxies for locally attached hosts, which prevents repeated ARP broadcasts over the MPLS/IP network.

Implementovano na VXLAN based EVPN (Nexus 9000), na IOS-XR pravdepodobne neni

![](<../.gitbook/assets/Unknown image (1927)>)

#### Unknown unicast suppression

happen when a device sends traffic to a destination MAC address that is not in the local PE’s MAC table.

Normally in plain Ethernet switching, this packet is flooded within the broadcast domain.

By default, a PE receiving unknown unicast traffic from a CE will flood it to all remote PEs in the same EVI (EVPN Instance) using:

Ingress Replication (IR) or

P2MP LSPs

EVPN improves this by reducing or eliminating unknown unicast flooding, thanks to its control-plane learning:

PEs advertise all locally learned MACs and IPs using EVPN Route Type 2 (MAC/IP Advertisement Route).

Because of this, remote PEs already know the MAC/IP mappings, so when traffic for that destination arrives, they can forward it directly without flooding.

All PEs must participate in control-plane learning (MAC/IP learning via BGP EVPN).

End hosts (CEs) must signal their presence when they connect (sending ARP, GARP, or first packet).

If some MAC/IPs are not advertised (e.g., silent hosts), suppression won’t work, and you must allow flooding.

The unknown-unicast-suppress command

This configuration tells the PE not to flood unknown unicast traffic into the EVPN/MPLS core.

Instead, unknown unicast packets are dropped if no matching MAC is found in the EVPN control-plane database.

The assumption is that the control plane (BGP EVPN) is distributing all the MAC/IPs, so flooding is unnecessary.

### EVPN load-balancing modes

**All-active load balancing (AAPF)** Supports multi-homed devices with per-flow load balancing. This mode allows the access device to connect via a “single” Ethernet bundle to multiple PEs and to send receive traffic of the “same” VLAN from all the PEs in the same Ethernet segment.

**Single-active load balancing (AAPS)** Supports multi-homed devices with per-vlan load balancing. In this mode,the access device connects via “separate” Ethernet bundles to multiple PEs. PE routers in turn automatically perform service carving in order to divide VLAN forwarding responsibilities across the PEs in the Ethernet segment. The access device learns via the data-plane which Ethernet bundle to use for a given VLAN.

The reason for the separate port-channel link between CE and the two PE in a Single-active setup is to ensure that the CE floods and learns the addresses via proper PE, since the second PE, that is not the DF for the VLAN will discard all flood and unicasted frames coming from the access and the core for a given vlan.

![](<../.gitbook/assets/Unknown image (1928)>)

<div align="left"><figure><img src="../.gitbook/assets/image (1).png" alt="" width="233"><figcaption></figcaption></figure></div>

{% hint style="info" %}
Note: there can also be physical connection between CE and the two PE without LAG between them – and if you want to retain the logical bundle-ether interface on the PE you can simply set it to mode ON
{% endhint %}

### EVPN startup process

1. RT4 with ES-import Extended Community is always exchanged between routers to avoid loops for their ethernet segments first. It helps them recognize what other routers are connected to the same ethernet segment, so that they can elect DF

![](<../.gitbook/assets/Unknown image (1930)>)

2. RT1 is then advertised to convey the Split horizon label as well as the attached ESI's with the RD and RT for import

![](<../.gitbook/assets/Unknown image (1931)>)

3. RT3 is advertised to signal the multidestination label

![](<../.gitbook/assets/Unknown image (1932)>)

4. RT2 to advertise the MAC address

The R37 updates its forwarding table aswell as soon as the R36 learns H1 MAC and sends it via EVPN BGP as a route

![](<../.gitbook/assets/Unknown image (1933)>)

![](<../.gitbook/assets/Unknown image (1934)>)

![](<../.gitbook/assets/Unknown image (1935)>)

The IOS-XR output from a SP6:

The BGP extended community in a Route Type 2 (MAC/IP Advertisement) indicates:

SoO: marks the originating site.

RT: controls VPN membership (which EVIs/BDs import the route).

0x060e… (EVPN AC EC): signals that this EVPN route is associated with an Attachment Circuit, and the trailing value (0000.0000.0015) is an identifier used to distinguish the AC (vendor-specific usage, in Cisco’s case often tied to the Bridge Domain/Service ID).

Ref [https://www.iana.org/assignments/bgp-extended-communities/bgp-extended-communities.xhtml?utm\_source=chatgpt.com](https://www.iana.org/assignments/bgp-extended-communities/bgp-extended-communities.xhtml?utm_source=chatgpt.com)

![](<../.gitbook/assets/Unknown image (1936)>)

5. RT1 per EVI is now exchanged to signal the aliasing label

![](<../.gitbook/assets/Unknown image>)

![](<../.gitbook/assets/Unknown image (1)>)

### Traffic-forwarding operation in EVPN

Unicast traffic forwarding: After the advertisements of MAC routes, unicast traffic can forward from a PE by imposing the appropriate multipoint-to-point VPN and MPLS labels and forwarding the traffic to the destination PE. It is always forwarded to single destination PE - not both, since it is load balanced per flow

![](<../.gitbook/assets/Unknown image (2)>)

![](<../.gitbook/assets/Unknown image (3)>)

### EVPN native

Native EVPN provides L2 connectivity (MAC learning and distribution via BGP) - it is the basic deployment of EVPN with it's core principles

Traffic comes in one port in the bridge domain.

The source MAC address (AA) is learned on the PE and is stored as a dynamic MAC entry.

The MAC address (AA) is converted into a type-2 BGP route and is sent over BGP to all the remote PEs in the same EVI.

The MAC address (AA) is updated on the PE as a remote MAC address.

![](<../.gitbook/assets/Unknown image (4)>)

### BUM forwarding (multi-destination traffic)

The PEs in a particular EVPN instance can use the following to send BUM traffic to other PEs:

Ingress replication

Point-to-multipoint LSPs

Each PE must advertise an Inclusive Multicast Ethernet Tag route to enable BUM traffic forwarding over the MPLS network.

When an unknown unicast (or BUM) MAC is received on the PE, it is advertised as EVPN Route Type 3 to other PEs

![](<../.gitbook/assets/Unknown image (5)>)

#### **BUM ingress replication**

A multicast flow can transmit only to PEs with receivers that are interested in the multicast flow.

Two service labels per EVPN instance

BUM Label – to forward Broadcast, Unknown Unicast and Multicast

Unicast Label – to forward Unicast - Unicast forwarding in EVPN uses a single MPLS service label per EVI, not per MAC. All MAC addresses learned on that EVI share the same MPLS label when forwarding unicast traffic.

![](<../.gitbook/assets/Unknown image (6)>)

![](<../.gitbook/assets/Unknown image (7)>)

PE1 must send BUM traffic to PE2 even though it knows that they have a segment in common because PE2 could host another CE on SHD or another segment that is part of the same EVI.

The multicast and ESI MPLS label are downstream-assigned when using ingress replication.

![](<../.gitbook/assets/Unknown image (8)>)

![](<../.gitbook/assets/Unknown image (9)>)

#### **Point-to-multipoint inclusive tree**

You can create point-to-multipoint LSPs with Resource Reservation Protocol Traffic Engineering (RSVP-TE) or Multicast Label Distribution Protocol (MLDP) for inclusive P-multicast trees.

You can set up a particular P-multicast tree to carry the traffic that originates from sites in a single EVPN instance or in several EVPN instances (aggregate inclusive P-multicast tree). A PE can receive BUM traffic even if it has no relevant receivers.

The procedure for aggregation is the same as the ones that are described in RFC 7117, but it uses an EVPN Inclusive Multicast Ethernet Tag (IMET) route.

![](<../.gitbook/assets/Unknown image (10)>)

![](<../.gitbook/assets/Unknown image (11)>)

### Segment and PE failures

When a PE detects a failure of one of its attached Ethernet segments, it withdraws the per-ESI auto discovery route for the failed segment. It then withdraws the Ethernet segment route. Notification is sent to all remote PEs associated to the same VPN. Remote PEs remove local PE (originating the notification) from the path list for all MAC addresses of failed Ethernet segments.

If a PE router fails, the other PEs detect the BGP session timeout and invalidate routes from the failed PE. The router that connects to the same segment as the failed PE router then becomes the designated forwarder for all EVIs that are on the segment.

### MAC mobility

A PE that is advertising a MAC address with its corresponding segment identifier for the first time, advertises it with MAC mobility extended community and with a particular sequence number (higher sequence number denote freshness of the information).

If the MAC moves to another segment (for example, virtual machine mobility), when the new PE discovers it, it will begin advertising the same MAC with a different ESI. This action increases the sequence number and the MAC mobility extended community.

A PE that is receiving a MAC/IP advertisement route for a MAC address with a different Ethernet segment identifier and a higher sequence number than what it had been previously advertised, withdraws its MAC/IP advertisement route and then install the new record.

MAC address duplication issues can appear if two or more PEs at different locations learn the same MAC address due to misconfiguration or continued MAC moves.

The PE detecting the duplication problem sets a timer (180 seconds by default), and if the MAC moves again before the timer expires, that PE will notify the operator and stop processing advertisements routes for that MAC address until corrected.

SEQ incremented each time MAC advertised from a different ESI

When number of moves exceeds threshold (def: 5 moves per 180 sec) à duplicate

If a duplicate for a MAC is seen 3x, the MAC is frozen

Need to unfreeze manually (clear l2route evpn frozen-mac frozen-flag evi x)

![](<../.gitbook/assets/Unknown image (12)>)

![](<../.gitbook/assets/Unknown image (13)>)

EVPN RFC mentions auto-generating the ESI - static configuration is recommended, since auto-generating can poses some issues

The evi 100 in the l2vpn configuration on the right side ensures the auto-prepending of the RT to the advertised BGP EVPN routes

![](<../.gitbook/assets/Unknown image (14)>)

### Configuring and verifying EVPN native

#### Prerequisites

Before you configure EVPN, some underlying items must be configured, including interface IP addressing, a configuration of an Interior Gateway Protocol (IGP), a configuration of MPLS, and basic configurations of BGP.

#### SHD and SHN configuration

The figure shows an example of how to configure EVPN Native with the Software MAC Learning feature in a single-home device (SHD) or a single-home network (SHN).

![](<../.gitbook/assets/Unknown image (15)>)

#### Verification

![](<../.gitbook/assets/Unknown image (16)>)

![](<../.gitbook/assets/Unknown image (17)>)

![](<../.gitbook/assets/Unknown image (18)>)

#### Dual-home device: AApF

All-active load-balancing is known also as Active-Active per Flow (AApF).

![](<../.gitbook/assets/Unknown image (19)>)

![](<../.gitbook/assets/Unknown image (20)>)

#### Dual-home device: AApS

Single-active load-balancing is also known as Active-Active per Service (AApS).

![](<../.gitbook/assets/Unknown image (21)>)

Identical ESIs are configured on both EVPN PEs. In the CE, separate bundles or independent physical interfaces are configured toward two EVPN PEs. In this mode, both PE1 and PE2 store the MAC address that they learn. Only one PE can forward traffic within the EVI at a given time per VLAN. The load-balancing mode is set to single-active.

A full Single-Active Multi-Homing configuration example can be found here: [https://xrdocs.io/ncs5500/tutorials/bgp-evpn-based-single-active-multi-homing/](https://xrdocs.io/ncs5500/tutorials/bgp-evpn-based-single-active-multi-homing/)

![](<../.gitbook/assets/Unknown image (22)>)

### Configuring and verifying EVPN VPWS

The EVPN VPWS configuration requires the EVPN instance (EVI), the local **Attachment Circuit (AC)** identifier that identifies the local end of the emulated service, and the remote **AC identifier** that identifies the remote end of the emulated service.

use show l2vpn xconnect to verify

![](<../.gitbook/assets/Unknown image (23)>)

![](<../.gitbook/assets/Unknown image (24)>)

#### EVPN VPWS multihomed configuration

![](<../.gitbook/assets/Unknown image (25)>)

![](<../.gitbook/assets/Unknown image (26)>)

### EVPN Features

All EVPN Features for IOS-XR [https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5500/vpn/25xx/configuration/guide/b-l2vpn-cg-ncs5500-25xx/evpn-features.html#concept\_F1990874980146D6AEF5E30A7D4F6825](https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5500/vpn/25xx/configuration/guide/b-l2vpn-cg-ncs5500-25xx/evpn-features.html#concept_F1990874980146D6AEF5E30A7D4F6825)

#### EVPN core isolation protection

When a core link failure is detected in the provider edge (PE) device, EVPN brings down the PE's Ethernet Segment (ES), which is associated with access interface attached to the customer edge (CE) device. EVPN replaces ICCP in detecting the core isolation. This new feature eliminates the use of ICCP in the EVPN environment.

Consider a topology where CE is connected to PE1 and PE2. PE1, PE2, and PE3 are running EVPN over the MPLS core network. The core interfaces can be Gigabit Ethernet or bundle interface.

When the core links of PE1 go down, the EVPN detects the link failure and isolates PE1 node from the core network by bringing down the access network. This prevents CE from sending any traffic to PE1. Since BGP session also goes down, the BGP invalidates all the routes that were advertised by the failed PE. This causes the remote PE2 and PE3 to update their next-hop path-list and the MAC routes in the L2FIB. PE2 becomes the forwarder for all the traffic, thus isolating PE1 from the core network.

When all the core interfaces and BGP sessions come up, PE1 advertises Ethernet A-D Ethernet Segment (ES-EAD) routes again, triggers the service carving and becomes part of the core network.

![](<../.gitbook/assets/Unknown image (27)>)

Configure core interfaces under EVPN group and associate that group to the Ethernet Segment which is an attachment circuit (AC) attached to the CE. When all the core interfaces go down, EVPN brings down the associated access interfaces which prevents the CE device from using those links within their bundles. All interfaces that are part of a group go down, EVPN brings down the bundle and withdraws the ES-EAD route.

Starting from Cisco IOS-XR software version 7.1.2, you can configure a sub-interface as an EVPN Core. With this enhancement, when using IOS-XR software versions 7.1.2 and above, EVPN core facing interfaces can be physical, bundle main, or sub-interfaces. For all Cisco IOS-XR software versions lower than 7.1.2, EVPN core facing interfaces must be physical or bundle main. Sub-interfaces are not supported.

Router# configure

Router(config)# evpn

Router(config-evpn)# group 42001

Router(config-evpn-group)# core interface GigabitEthernet0/2/0/1

Router(config-evpn-group)# core interface GigabitEthernet0/2/0/3

Router(config-evpn-group)#exit

!

Router(config-evpn)# group 43001

Router(config-evpn-group)# core interface GigabitEthernet0/2/0/2

Router(config-evpn-group)# core interface GigabitEthernet0/2/0/4

Router(config-evpn-group)#exit

!

Router# configure

Router(config)# evpn

Router(config-evpn)# interface bundle-Ether 42001

Router(config-evpn-ac)# core-isolation-group 42001

Router(config-evpn-ac)# exit

!

Router(config-evpn)# interface bundle-Ether 43001

Router(config-evpn-ac)# core-isolation-group 43001

Router(config-evpn-ac)# commit

\#show evpn group

#### Network convergence with core isolation protection

This feature reduces the duration of traffic drop by rapidly rerouting traffic to alternate paths. This feature uses object tracking to detect remote link failure and the failure of connected interfaces.

Tracking interfaces can detect failure of connected interfaces only. They cannot detect failure of a remote router interface that provides connectivity to the core. Tracking one or more BGP neighbor sessions, along with one or more of the neighbor’s address families, enables you to detect remote link failure.

Object tracking is a mechanism for tracking an object to take any client action on another object as configured by the client. The object on which the client action is performed may not have any relationship to the tracked objects. The client actions are performed based on changes to the properties of the object being tracked.

You can identify each tracked object by a unique name that is specified by the track command in configuration mode.

The tracking process receives the notification when the tracked object changes its state. The state of the tracked objects can be up or down.

You can also track multiple objects by a list. You can use a flexible method for combining objects with Boolean logic. This functionality includes the following:

Boolean AND function: When a tracked list has been assigned a Boolean AND function, each object that is defined within a subset must be in an up state. It allows the tracked object to also be in the up state.

Boolean OR function: When the tracked list has been assigned a Boolean OR function, at least one object that is defined within a subset must also be in an up state. It allows the tracked object to also be in the up state.

Consider a traffic flow from CE1 to PE1. The CE1 router can send the traffic either from Leaf1-1 or Leaf1-2. When Leaf1-1 loses the connectivity to both the local links and remote link, BGP sessions to both route reflectors are down; Leaf1-1 brings down Bundle-Ether14 connected to CE1. The CE1 router redirects the traffic from Leaf1-2 to PE1.

![](<../.gitbook/assets/Unknown image (28)>)

Example LAB config

track SvRR1-PE11

type bgp neighbor address-family state

address-family l2vpn evpn

neighbor 10.64.201.1

!

!

!

track SvRR2-PE22

type bgp neighbor address-family state

address-family l2vpn evpn

neighbor 10.64.202.2

!

!

!

track Uplink-BE1

type line-protocol state

interface Bundle-Ether1

!

!

track Uplink-BE2

type line-protocol state

interface Bundle-Ether2

!

!

track SvRR-any-up

type list boolean or

object SvRR1-PE11

object SvRR2-PE14

!

!

track Uplinks-any-up

type list boolean or

object Uplink-BE1

object Uplink-BE2

!

!

track SvRR-and-Uplinks-up

type list boolean and

object SvRR-any-up

object Uplinks-any-up

!

action

track-down error-disable interface Bundle-Ether10 auto-recover

### EVPN MPLS seamless integration with VPLS

EVPN MPLS Seamless Integration with VPLS allows you to upgrade the VPLS PE routers to EVPN one by one without any network service disruption.

The EVPN service can be introduced in the network one PE node at a time. The VPLS to EVPN migration starts on PE1 by enabling EVPN in a VPN instance of VPLS service. As soon as EVPN is enabled, PE1 starts advertising EVPN inclusive multicast route to other PE nodes. Since PE1 does not receive any inclusive multicast routes from other PE nodes, VPLS pseudo wires between PE1 and other PE nodes remain active. PE1 keeps forwarding traffic using VPLS pseudo wires. At the same time, PE1 advertises all MAC address learned from CE1 using EVPN route type-2. In the second step, EVPN is enabled in PE3. PE3 starts advertising inclusive multicast route to other PE nodes. Both PE1 and PE3 discover each other through EVPN routes. As a result, PE1 and PE3 shut down the pseudo wires between them. EVPN service replaces VPLS service between PE1 and PE3. At this stage, PE1 keeps running VPLS service with PE2 and PE4. It starts EVPN service with PE3 in the same VPN instance. This is called EVPN seamless integration with VPLS. The VPLS to EVPN migration then continues to remaining PE nodes. In the end, all four PE nodes are enabled with EVPN service. VPLS service is completely replaced with EVPN service in the network. All VPLS pseudo wires are shut down.

VFI1 is by default in Split Horizon Group 1 (SPHG1)

SHG1 protects loops in MPLS Core

Full Mesh of Pseudowires (PW) is required for Any-to-Any forwarding

EVI100 is also by default in Split Horizon Group -> R36 doesn’t forward data between VFI1 and EVI100

![](<../.gitbook/assets/Unknown image (29)>)

R36\&R38 run BGP EVPN -> PW\_R38 goes DOWN

-> Data Forwarding between R36 and R38 via EVI100

![](<../.gitbook/assets/Unknown image (30)>)

### VPWS & MPLS seamless migration

With EVPN-VPWS Seamless Integration feature, you can migrate the PE nodes from legacy VPWS service to EVPN-VPWS gradually and incrementally without any service disruption.

You can migrate an Attachment Circuit (AC) connected to a legacy VPWS pseudowire (PW) to an EVPN-VPWS PW either by using targeted-LDP signaling or BGP-AD signaling.

Instead of performing network-wide software upgrade at the same time on all PEs, this feature provides the flexibility to migrate one PE at a time. Thus allows the coexistence of legacy VPWS and EVPN-VPWS dual-stack in the core for a given L2 Attachment Circuit (AC) over the same MPLS network.

During migration, an EVPN-VPWS PE router performs either VPWS or EVPN-VPWS L2 cross-connect for a given AC. When both EVPN-VPWS and BGP-AD PWs are configured for the same AC, the EVPN-VPWS PE during migration advertises the BGP VPWS Auto-Discovery (AD) route as well as the BGP EVPN Auto-Discovery (EVI/EAD) route and gives preference to EVPN-VPWS Pseudowire (PW) over the BGP-AD VPWS PW.

![](<../.gitbook/assets/Unknown image (31)>)

![](<../.gitbook/assets/Unknown image (32)>)

![](<../.gitbook/assets/Unknown image (33)>)

![](<../.gitbook/assets/Unknown image (34)>)

### EVPN Integrated Routing and Bridging (IRB)

Native EVPN provides L2 connectivity (MAC learning and distribution via BGP) and IRB extends this by enabling L3 routing for the same hosts, without sending the traffic out to an external L3 device.

The combination allows integration of L2 and L3 VPNs, so a tenant can have both bridged and routed connectivity across the EVPN fabric.

This is done using a **BVI (Bridge Virtual Interface)** on each PE, which acts as the default gateway for the subnet

BVI is tied to a VRF for routing

To send packets to other subnets, hosts use the destination IP and local BVI MAC.

BVI in the local PE looks up the VRF table and routes the packet to the destination PE.

Supports Distributed Anycast Gateway (DAG) – a key IRB capability enabling seamless first-hop default gateway.

<div data-full-width="false"><img src="../.gitbook/assets/Unknown image (35)" alt="" width="375"></div>

IRB Configuration and all features:

{% embed url="https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5500/vpn/25xx/configuration/guide/b-l2vpn-cg-ncs5500-25xx/m-configure-evpn-irb-ncs5500.html#concept_3B72C94196324D56914438704046D88F" %}

> EVPN Route Type 5 (IP Prefix Route), defined in RFC 9136, is used to advertise IPv4/IPv6 prefixes in the EVPN control plane. It enables Layer-3 reachability distribution within EVPN fabrics, typically for inter-subnet routing in VXLAN EVPN networks. Type 5 routes advertise prefixes associated with a VRF and L3VNI and use the VTEP as the next-hop. They complement Type 2 MAC/IP routes and allow scalable prefix-based routing instead of advertising individual host routes.

In **DCI (Data Center Interconnect)** deployments there is a feature where a device can **translate routes between EVPN and MPLS L3VPN control planes**.

This is typically called:

* **EVPN–L3VPN Stitching**
* **EVPN ↔ VPNv4/VPNv6 Interworking**
* **EVPN Gateway / EVPN DCI Gateway**

The device performs **route conversion between address families**.

[https://www.cisco.com/c/en/us/td/docs/routers/asr9000/software/asr9k-r6-3/lxvpn/configuration/guide/b-l3vpn-cg-asr9000-63x/b-l3vpn-cg-asr9000-63x\_chapter\_0111.html#task\_6B3D13C152BB4614A62374AB5F9EE181](https://www.cisco.com/c/en/us/td/docs/routers/asr9000/software/asr9k-r6-3/lxvpn/configuration/guide/b-l3vpn-cg-asr9000-63x/b-l3vpn-cg-asr9000-63x_chapter_0111.html#task_6B3D13C152BB4614A62374AB5F9EE181)

[https://www.ciscolive.com/c/dam/r/ciscolive/emea/docs/2019/pdf/BRKSPG-3965.pdf](https://www.ciscolive.com/c/dam/r/ciscolive/emea/docs/2019/pdf/BRKSPG-3965.pdf)

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

#### BVI-coupled mode

Normally, the BVI state depends on the availability of the local attachment circuits (ACs) — i.e. if all access ports in that BD go down, the BVI also goes Down.

This is the “default” behavior: if nothing is attached locally, the gateway should also go down.

You may still have remote reachability to hosts in that BD (via EVPN), even if no local ports are up.

In such cases, shutting down the BVI just because local ACs are down is undersirable - the subnet may be stretched somewhere else in the fabric

In BVI-Coupled Mode, the BVI is made EVPN-aware:

It doesn’t go down just because local ACs are down.

It remains Up as long as there is EVPN connectivity (EAD/ES routes) for that BD.

This means routing adjacencies (OSPF, BGP, etc.) that depend on the BVI don’t keep flapping every time local ACs bounce

Without BVI-Coupled Mode:

If AC goes down → BVI goes Down → IGP adjacency resets → churn in control-plane and forwarding.

With BVI-Coupled Mode:

BVI stays Up as long as remote EVPN peers still advertise the subnet (via EAD/RTs).

The device still installs adjacencies in the FIB, but marks them as invalid if the local AC is down.

[https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5500/vpn/25xx/configuration/guide/b-l2vpn-cg-ncs5500-25xx/m-configure-evpn-irb-ncs5500.html#concept\_2905F121E0B64A2EB01CD64A640C539B:\~:text=interface%20is%20down.-,Configure%20BVI%2DCoupled%20Mode,-Perform%20this%20task](https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5500/vpn/25xx/configuration/guide/b-l2vpn-cg-ncs5500-25xx/m-configure-evpn-irb-ncs5500.html#concept_2905F121E0B64A2EB01CD64A640C539B)

## Distributed L3 anycast gateway

Hosts are configured with a single default gateway address for their local subnet. That single (anycast) gateway address is configured with a single (anycast) MAC address on all EVPN PE nodes locally supporting that subnet. This process is repeated for each locally defined subnet requires Anycast Gateway support.

The host-to-host Layer 3 traffic, similar to Layer 3 VPN PE-PE forwarding, is routed on the source EVPN PE to the destination EVPN PE next-hop over an IP or MPLS tunnel, where it is routed again to the directly connected host. Such forwarding is also known as Symmetric IRB because the Layer 3 flows are routed at both the source and destination EVPN PEs.

![](<../.gitbook/assets/Unknown image (36)>)

1. The two “modes” of EVPN IRB Anycast Gateway

These are essentially design choices:

EVPN IRB with All-Active Multi-Homing without Subnet Stretch (No Host Routing)

Subnet is local to a leaf pair (redundancy group).

Only prefixes (/subnet) are advertised to the rest of the fabric via RT5.

No /32 host routes exported fabric-wide.

Dual-homed hosts still need ARP/MAC sync between the multihomed leafs.

<img src="../.gitbook/assets/Unknown image (37)" alt="" width="375">

EVPN IRB with All-Active Multi-Homing with Subnet Stretch (Host Routing)

Subnet/BD is stretched across multiple leaf pairs.

Host MAC+IP (RT2) entries and sometimes host /32 IP routes are distributed across the fabric.

Remote PEs can install those /32s directly into their VRFs (similar to L3VPN imposition)

Allows true any-to-any host mobility and routing.

## Centralized L3 anycast gateway

### Flexible Cross-Connect (FXC)

The **Flexible Cross-Connect (FXC)** service feature enables aggregation of attachment circuits (ACs) across multiple endpoints in a single Ethernet VPN Virtual Private Wire Service (EVPN-VPWS) service instance, on the same Provider Edge (PE). ACs are represented either by a single VLAN tag or double VLAN tags. The associated AC with the same VLAN tag(s) on the remote PE is cross-connected. The VLAN tags define the matching criteria to be used in order to map the frames on an interface to the appropriate service instance. As a result, the VLAN rewrite value must be unique within the flexible cross-connect (FXC) instance to create the lookup table. The VLAN tags can be made unique using the rewrite configuration. The lookup table helps determine the path to be taken to forward the traffic to the corresponding destination AC. This feature reduces the number of tunnels by muxing VLANs across many interfaces. It also reduces the number of MPLS labels used by a router. This feature supports both single-homing and multi-homing.

![](<../.gitbook/assets/Unknown image (38)>)

### EVPN headend multi-homed (PWHE)

**Multi-homed EVPN Head End** allows the termination of Access pseuodwires or PWs (like EVPN-VPWS) into a Layer 3 \[virtual routing and forwarding (VRF) or global] domain. PWHE subinterface resides in customer VRFs allowing service providers to offer IP services such as DHCP, NTP, and Layer 3 VPN for internet connectivity.

Multi-homed EVPN Head End has the following advantages:

Decouples the customer-facing interface (CFI) of the service PE from the underlying physical transport media of the access or aggregation network.

Reduces capex in the access or aggregation network and service PE.

Distributes and scales the customer-facing Layer 2 UNI interface set.

Extends and expands service provider’s Layer 3 service footprints.

Allows provisioning features such as QoS and ACL, L3VPN on a per PWHE subinterface

The Multi-homed EVPN headend solution supports redundant Layer 3 gateway functionality over PW-Ether interface termination, residing on a pair of redundant PE routers. The PW-Ether subinterfaces offer redundancy in the Core and first-hop router towards the access on a per-customer-service basis.

Multi-homed EVPN is supported with:

Regular attachment circuits

Physical Ethernet ports

Bundle interfaces

PW-Ether interfaces

Multi-homed EVPN headend supports three load balancing modes.

Single-Active: also referred to as anycast single-active mode. That is, all-active in Layer 3 Core and single-active in Layer 2 Access, which is the default load-balancing mode for PWHE. For more information, see How EVPN headend multi-homed single active load balancing mode works.

All-Active: traffic is load balanced through both redundant PEs in both directions, that is, in the Layer 3 Core and in the Layer 2 Access. For more information, see How EVPN headend multi-homed all-active load balancing mode works.

Port-Active: PWHE interface is UP only on one PE, so all traffic flows through one PE in both directions. For more information, see How EVPN headend multi-homed port-active load balancing mode works.

![](<../.gitbook/assets/Unknown image (39)>)

![](<../.gitbook/assets/Unknown image (40)>)
