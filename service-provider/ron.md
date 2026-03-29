# RON

### Challenges of legacy transport

Having different layers with their own management and control planes brings complexity and challenges to planning, building, and operating. Duplication of tasks between IP and optical teams, along with the complexity of network protection and restoration across layers, adds to operational challenges. Furthermore, interoperability issues and the need for hardware conversions between layers, such as OTN and DWDM, result in inefficiencies. For instance, the conversion of OTN and IP network traffic into wavelength signals for DWDM networks often necessitates dedicated external hardware, adding complexity and costs.

### Traditional network design

![](<../.gitbook/assets/Unknown image (820)>)

![](<../.gitbook/assets/Unknown image (821)>)

Despite a shared Fiber topology, a traditional architecture has two (2) distinct topologies for the IP and Optical layers given its complex mesh of wavelengths

![](<../.gitbook/assets/Unknown image (822)>)

### Traditional optical protection schemes

Optical transport networks (OTN, DWDM) use hardware-based redundancy to protect against fiber cuts or node failures. The main optical protection schemes are:

1+1 Protection: This approach provides route diversity by ensuring two dedicated paths for transmission. However, if a fiber cut occurs, it reduces capacity by 50% because traffic is split across the two paths.

1+1+R (Restoration): This method enhances reliability by adding an additional restoration path. When a fiber cut happens, traffic is dynamically rerouted via the restoration path, maintaining full capacity and improving resilience.

![](<../.gitbook/assets/Unknown image (823)>)

When a fiber cut occurs (e.g., at node 7 in the diagram), the network automatically reroutes traffic using the predefined restoration path (green path).

This allows traffic to continue without manual intervention, minimizing downtime.

No Coordination with the IP Layer

Unlike traditional protection mechanisms that may require higher-layer (IP/MPLS) rerouting, Optical Restoration works at the optical layer and can restore connectivity within minutes.

This makes the restoration process faster and transparent to the IP layer, preventing network-wide disruptions.

Challenges with Partial Path Overlap

The restoration path (green) shares some links with the original primary path (purple).

Once the failed link is repaired, the system must revert to the original path (purple) to maintain intended routing.

This raises an operational question: how do you track and manage the home path versus the restored path?

Optical Reversion is Hard

Automatic Reversion: The network can be configured to automatically revert to the primary path after a set time (Wait-to-Restore, WTR), but this can cause issues if not well-coordinated.

Manual Reversion: A more controlled approach is to schedule reversion events in coordination with the IP layer, ensuring minimal traffic disruption.

Multiple Circuits Issue: If several optical circuits revert at once, network instability may occur due to sudden traffic shifts.

![](<../.gitbook/assets/Unknown image (824)>)

### Routed optical networking (RON)

**Routed optical networking (RON)** (also called IP over DWDM, **IPoDWDM**) integrates the IP and optical layers into a single, cost-effective, and easily manageable infrastructure. It collapses the control plane and switching layer

RON architecture aims at making optical and IP topologies congruent which enables:

Optimal traffic forwarding for applications, content and Internet peers

Higher utilization of network assets, wavelengths and higher bit-rate wavelengths given their shorter distances

In routed optical networking, data packets are routed directly over optical wavelengths, bypassing optical layer. This direct routing reduces latency, enhances bandwidth usage, and simplifies the network, leading to improved performance and lower operational costs

[https://www.cisco.com/c/en/us/products/collateral/routed-optical-networking/routed-optical-networking-wp.html](https://www.cisco.com/c/en/us/products/collateral/routed-optical-networking/routed-optical-networking-wp.html)

![](<../.gitbook/assets/Unknown image (825)>)

#### IP / MPLS network utilization (traditional)

Uses more wavelengths per fiber span

Uses less traffic aggregation

Results in lower IP port/wavelength utilization

Traditional architecture splits traffic onto dedicated IP ports and wavelengths toward distant routers (based on destinations) in lower IP port/wavelength bit rates (due to longer distances)

Leads to:

Underutilized network assets

Over-investment in the physical infrastructure

Higher cost per bit

#### IP / MPLS network utilization (RON)

[https://www.cisco.com/c/en/us/products/collateral/routed-optical-networking/routed-optical-networking-wp.html#Arepeopleskillsarealproblemforroutedopticalnetworkingadoption](https://www.cisco.com/c/en/us/products/collateral/routed-optical-networking/routed-optical-networking-wp.html#Arepeopleskillsarealproblemforroutedopticalnetworkingadoption)

Uses less wavelengths per fiber span

Uses more traffic aggregation

Results in higher IP port/wavelength utilization

Results in higher IP port/wavelength bit rates (due to shorter distances)

Leads to:

Aggregates traffic onto fewer IP ports/wavelengths on a given node

Drives higher utilization of network assets

Increases network efficiency which leads to a lower cost per bit

Traffic Engineering (TE) is not required

However, use of TE and a Centralized (SDN) Controller can drive utilization even higher

IP and optical integration is a fundamental part of routed optical networking given the significant economic benefits provided by the current 400-Gbps digital coherent optics pluggable transceiver technology. However, routed optical networking goes far beyond that, and when deployed with modern IP/MPLS technology, it brings network optimizations and efficiencies to another level.

More efficient use of scarce DWDM wavelengths through statistical multiplexing and traffic aggregation leveraging direct router-to-router designs that better align with fiber topology, plus optional centralized traffic engineering.

Statistical Multiplexing is a technique that dynamically shares network resources among multiple data streams based on actual usage rather than reserving fixed capacity for each stream.

Traditional optical bypass networks dedicate a fixed DWDM wavelength to each router connection, often leaving unused capacity and requires more IP ports.

Statistical Multiplexing in Routed Optical Networks allows multiple routers to share wavelengths dynamically, filling them efficiently with traffic and reducing the total number of wavelengths needed.

Full services convergence: IP/MPLS is the de facto multiservice technology and the only networking technology capable of supporting L1 (emulated TDM circuits), L2 (point-to-point and multipoint Ethernet), and L3 services (internet, IP VPNs). The addition of PLE technology enabled by routed optical networking expands the IP/MPLS capabilities further to support bit-transparent services for high-speed circuits, including OTN, Ethernet, and storage protocols as clients.

Optical transport simplification (fewer OTN transponders and intermediate protection layers).

End-to-end traffic engineering and resiliency at the IP/MPLS layer, replacing rigid optical protection schemes.

Faster failover and restoration compared to slow optical restoration mechanisms.

Fast Convergence: IP/MPLS Fast Reroute (FRR) restores traffic in sub-50ms, similar to 1+1 optical protection but with far greater flexibility.

Better Bandwidth Utilization: Unlike optical protection, IP/MPLS doesn’t require full duplication of fiber paths.

Traffic Engineering: MPLS-TE enables dynamic path computation, optimizing traffic flow and minimizing congestion.

End-to-End Protection: IP/MPLS protects not just the transport layer but also application and service layers.

Flexibility: Optical protection is hardware-dependent, whereas IP/MPLS is software-defined and adaptable to changing network conditions.

Typical transport networks switch traffic in the event of fiber, node, or link failures under 50 milliseconds, which is the golden standard for any network technology. That switching time is easily met by multiple solutions available for IP/MPLS networks today. For over a decade, classic IP/MPLS networks have supported RSVP-TE-based fast-reroute for link and node failures. More recently, IP/MPLS-based networks started providing sub-50-ms switching even without RSVP-TE, using loop-free alternate (LFA) fast-reroute and topology-independent loop-free alternate (TI-LFA). As the name implies, TI-LFA applies to any network topology, it’s very simple to configure, and it’s enabled by segment routing, with no need for additional protocols or stateful core tunnels that are operationally demanding to configure and maintain and consume costly router resources.

Finally, it goes without saying that no technology available today except IP/MPLS can provide an optimal transport solution to any L1, L2, or L3 service in terms of scale, efficiency, and flexibility.

RON architecture provides opportunity to converge all services on the RON infrastructure. Instead of having OTN as a separate service, via RON you can provide packet-based services as well as TDM type of services. The following figure presents evolution from traditional multilayer to full convergence approach:

Multi-layered: Traditional services via OTN, DWDM transponder and ROADM

Transponder Integration: Hybrid approach where you are integrating transponder inside the router while keeping OTN and wave services.

Service Convergence Private Line Emulation (PLE): Further simplification by emulating OTN over RON infrastructure via private line emulation, while keeping wave services for strategic customers,

Full Convergence with Circuit Style Segment Routing (CS-SR): Delivering all services via RON infrastructure. CS-SR provides the underlying TDM-like transport to support traditional private line Ethernet services without additional hardware and bit-transparent Ethernet, OTN, SONET/SDH, and Fiber Channel services using Private Line Emulation hardware.

![](<../.gitbook/assets/Unknown image (826)>)

RON allows two types of services:

Connection-less

Classic SR on transport underlay

Ethernet VPN (EVPN) Virtual Private Wire Service (VPWS) on service overlay

Connection-oriented (for example traditional OTN services)

CS-SR on transport underlay, ensuring bandwidth and end-to-end path protection

PLE on service overlay, ensuring bit transparency and various payloads

Note

Segment Routing (SR) is not mandatory for Routed Optical Networking. Classic IP/MPLS is supported as well.

However, Routed Optical Networking will provide the best benefits when deployed over Segment Routing

Both SR MPLS and SRv6 are supported

Each option has its own set of benefits

### Key technologies in the RON IP/MPLS layer

MP-BGP and BGP-LS

DiffServ QoS

YANG model-driven programmability & telemetry

Segment Routing (SR) and SR-TE (Traffic Engineering)

Centralized (SDN) Controller with PCE (Path Computation Engine)

PLE (Private Line Emulation)

### Private Line Emulation (PLE)

**Private Line Emulation (PLE)** is a pillar of the Routed Optical Networking solution. It enables service providers and enterprises to further collapse network layers, decreasing network complexity and increasing network efficiency. PLE enables private line services to be carried over the same MPLS or Segment Routing network for non-Ethernet type services such as SONET/SDH, OTN, and Fiber Channel. PLE also supports bit-transparent Ethernet services where required.

High revenue legacy private line services exist in the network infrastructure of most service providers, often carried over a dedicated inefficient TDM OTN layer. PLE enables service providers to carry SONET/SDH, OTN, Ethernet, and Fiber Channel over a circuit-style segment routed packet network while maintaining existing service SLAs. PLE utilizes Circuit Emulation (CEM) to transparently transfer PLE client frames over MPLS or SR networks without changing the characteristics of the original signal.

#### How PLE works

Ethernet, OTN, Fiber Channel, or SONET/SDH PLE client traffic is carried on an EVPN-VPWS single homed service that is created between PLE endpoints. EVPN-VPWS signalling information is carried using BGP between the PLE circuit endpoints either through direct BGP sessions or through a BGP services route-reflector. The EVPN-VPWS pseudowire channel is set up between the endpoints when the CEM (Circuit Emulation) client interfaces are configured on each endpoint router and end-to-end transport connectivity using MPLS or Segment Routing transport is enabled.

CEM is a method through which client data can be transmitted over MPLS or Segment Routing networks in a bit-transparent manner, retaining the client L1 frame between sender and receiver. CEM over a Packet Switched Network (PSN) places the client bit streams into packet payload with appropriate pseudowire emulation headers.

The PLE initiator encapsulates the PLE client traffic and carries it over the EVPN-VPWS service running on MPLS or Segment Routing transport. The PLE terminator node extracts the bit streams from the EVPN-VPWS packets and places them onto the PLE client interface as defined by the client attribute and CEM profile.

| **PLE Transport Type** | **Supported Payloads**             |
| ---------------------- | ---------------------------------- |
| Ethernet               | 1GE and 10GE                       |
| OTN                    | OTU2 and OTU2e                     |
| SONET                  | OC-48 and OC-192                   |
| SDH                    | STM-16 and STM64                   |
| Fiber Channel          | FC1, FC2, FC4, FC8, FC16, and FC32 |

More on [https://www.cisco.com/c/en/us/td/docs/optical/ron/3-0/solution/guide/b-ron-solution-30/m-ple-configuration.html](https://www.cisco.com/c/en/us/td/docs/optical/ron/3-0/solution/guide/b-ron-solution-30/m-ple-configuration.html)

![](<../.gitbook/assets/Unknown image (827)>)

### Digital coherent optics (DCO)

**Digital coherent optics (DCO)** is a pluggable transceiver that combines silicon photonics and Digital Signal Processors, operating at 400 Gbps speeds and using the Quad Small Form Factor Pluggable Double Density (QSFP-DD) form factor.

The following figure illustrates the benefits of replacing a DWDM transponder with DCO, showcasing reductions in costs, rack space, power consumption, and operational complexity.

![](<../.gitbook/assets/Unknown image (828)>)

![](<../.gitbook/assets/Unknown image (829)>)

DCO relies on several standards to ensure interoperability, efficiency, and reliability in optical communication systems. These standards play a crucial role in shaping the functionality and performance of DCO technology. Three industry optical standards have emerged to cover a variety of use cases:

OpenZR+

Open ROADM

Optical Internetworking Forum (OIF)

![](<../.gitbook/assets/Unknown image (830)>)

![](<../.gitbook/assets/Unknown image (831)>)

![](<../.gitbook/assets/Unknown image (832)>)

Also the OLS is integrated in the pluggable

The Fanout A/D cable in this diagram serves as an optical multiplexing/demultiplexing medium, connecting multiple individual QDD BZR+ modules (each handling a specific wavelength like Lambda 1 or Lambda 2) to a single aggregated optical port on the QDD OLS (Optical Line System). This means the cable branches out on one end to connect to the separate QDD BZR+ modules, and converges into a single connection on the other end that plugs into the QDD OLS, effectively allowing the OLS to "add" (transmit) or "drop" (receive) these multiple wavelengths over a single optical link

![](<../.gitbook/assets/Unknown image (833)>)
