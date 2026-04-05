# LISP

### Location/ID Separation Protocol (LISP)

**Location/ID Separation Protocol (LISP)** is an Cisco's alternative architecture to traditional routing, designed to address challenges of modern networking (mobility, multi-homing, IOT)

In traditional routing architectures, an endpoint IP represents the endpoint’s identity and location.

If the location of the endpoint changes, its IP also changes. LISP separates IP into EIDs and RLOCs.

This way, endpoints can roam from site to site, and the only thing that changes is their RLOC while the EID remains the same

The host retains its EID and updates its RLOC whenever it changes networks. The LISP Mapping System ensures that communication is routed correctly, even if the host moves to a subnet with a completely different address range (e.g., 10.10.20.0/24

![](<../.gitbook/assets/Unknown image (1628)>)

### Components

**Endpoint identifier (EID)** IP address of an endpoint within a LISP site

**LISP site** name of a site where LISP routers and EIDs reside

**Ingress tunnel router (ITR)** LISP-encapsulate packets from EIDs destined to outside the LISP site

**Egress tunnel router (ETR)** de-encapsulate LISP-encapsulated packets coming from outside destined to EID. Publishes EID-to-RLOC mappings for a site

**Tunnel router (xTR)** performs ITR and ETR functions

**Proxy ITR (PITR)** like ITR but for non-LISP sites that send traffic to EID destinations

**Proxy ETR (PETR)** like ETR for non-LISP sites

**Proxy xTR (PxTR)** performs PITR and PETR functions. Router can be PxTR and xTR at the same time

**Routing locator (RLOC)** WAN IP address of an ETR

**Map server (MS)** **Map resolver (MR)** serves as DNS for LISP-encapsulated queries for destination EID location

It tracks EID-to-prefix information recieved from an ETR and stores them in a local EID-to-RLOC database

It then resolves requests from ITR to the corresponding ETR RLOC of the destination EID

### LISP control plane

**LISP control plane** operates as DNS, resolving an EID into an RLOC by sending map requests to the MS/MR.

Efficient and scalable on-demand routing protocol as it is based on a pull model, where only the routing information that is necessary is requested to be resolved by MS/MR (as opposed to the push model of traditional routing prot (BGP or OSPF) that pushes all routes to routers (including unnecessary ones)

**Database:** Contains the locally attached EIDs. Reachability of EID space depends on the RIB entry therefore if there’s no RIB entry for a EID Space it will be shown as unreachable

**Map-Cache:** Used to build the LISP data plane. Populated through MAP-REQUESTS

### LISP data plane

ITRs LISP-encapsulate IP packets received from EIDs in an outer IP UDP header with source and destination addresses in the RLOC space. Reverse process for receiving ETR

#### LISP header (encapsulation)

LISP adds an additional 36 byte (20 byte outer IPv4 + 8 byte UDP + 8 byte LISP header) or 56 byte (IPv6) (40 byte outer IPv6 + 8 byte UDP + 8 byte LISP) header

![](<../.gitbook/assets/Unknown image (1629)>)

Outer LISP IP header Contains the source and destination RLOC IP addresses needed to route the packet from the ITR to ETR

Outer LISP UDP header contains a source port that is selected thoughtfully by an ITR. If the source port in the Outer LISP UDP header is the same for every encapsulated packet from a specific site, it could lead to Polarization, which occurs when packets from one site follow exactly the same path, even though there are multiple equally good paths available (ECMP), thus not utilizing load balancing

Destination UDP port 4341 is used for LISP data plane, for control plane it uses UDP port 4342

Instance ID (24-bit) enable VRF and VPNs for virtualization and segmentation

Original IP header This is the IP header as received by an EID

![Cisco SDA Part II - basic LISP configuration and operation - The ASCII Construct](<../.gitbook/assets/Unknown image (1630)>)

### LISP operation

MAP-REGISTRATION and MAP-NOTIFY messages

ETR sends a map register message to the MS to register its associated EID prefix and RLOC IP

The MS sends a map notify message to the ETR to confirm that the map register has been received and processed using UDP port 4342

MAP-REQUEST and MAP-REPLY messages

1. The endpoint in LISP Site 1 (host1) sends a DNS request to resolve the IP of the endpoint in LISP Site 2 (host2.cisco.com). The DNS server replies with the IP 10.1.2.2, which is the

destination EID. host1 sends IP packets with destination IP 10.1.2.2 to its default gateway

2. The ITR receives the packets from host1 destined to 10.1.2.2. It performs a FIB lookup and evaluates the following forwarding rules:

Did the packet match a default route because there was no route found for 10.1.2.2 in the routing table? If yes, continue to next step. If no, forward the packet natively to matched route

Is the source IP a registered EID prefix in the local map cache? If yes, continue to next step and If no, forward the packet natively

3. ITR sends an encapsulated map request to the MR for 10.1.2.2. UDP destination port 4342, and the source port is chosen by the ITR
4. MR/MS on the same device > the MS mapping database system forwards the map request to the appropriate ETR. If the MR/MS functions were on different devices, the MR would forward the encapsulated map request packet to the MS as received from the ITR, and the MS would then forward the map request packet to the ETR
5. ETR sends to ITR a map reply message that includes an EID-to-RLOC mapping 10.1.2.2 → 100.64.2.2.

The map reply message uses the UDP source port 4342 and the destination port is the one chosen by the ITR in the map request message

An ETR may also request that the MS answer map requests on its behalf by setting the proxy map reply flag (P-bit) in the map register message

6. The ITR installs the EID-to-RLOC mapping in its local map cache and programs the FIB; then forwards LISP traffic

![](<../.gitbook/assets/Unknown image (1631)>)

P-bit = Proxy-map-reply bit. If set, the MS replies directly to MAP-REQUEST messages.

M-bit = Want-map-notify bit. If set, the MS is required to send a MAP-NOTIFY message.

S-bit = Solicit-map-request bit. Set by the “receiving xTR” to inform that a specific EID is no longer “behind” it. Tells the “sender xTR” that it should send a MAP-REQUEST to the MR/MS to find out where the specific EID is now located.

#### LISP data path

1. ITR receives a packet from EID host1 (10.1.1.1) destined to host2 (10.2.2.2).
2. The ITR performs a FIB lookup and finds a match. It encapsulates the EID packet and adds an outer header with the RLOC IP from the ITR as the source IP and the RLOC IP of the ETR

as the destination IP. The packet is then forwarded using UDP destination port 4341 with a tactically selected source port in case ECMP load balancing is necessary

3. ETR receives the encapsulated packet and de-encapsulates it to forward it to host2.

![](<../.gitbook/assets/Unknown image (1632)>)

### Non-LISP site operation

#### Proxy ETR (PETR)

is a router connected to a non-LISP site (such as a data center or the Internet) that is used when a LISP site needs to communicate to a non-LISP site. Since the PETR is connected to non-LISP sites, a PETR does not register any EID addresses with the mapping database system. When an ITR sends a map request and the EID is not registered in the mapping database system, the mapping database system sends a negative map reply to

the ITR. When the ITR receives a negative map reply, it forwards the LISP-encapsulated traffic to the PETR. For this to happen, the ITR must be configured to send traffic to the PETR’s

RLOC for any destinations for which a negative map reply is received.

When the mapping database system receives a map request for a non-LISP destination, it calculates the shortest prefix that matches the requested destination but that does not match

any LISP EIDs. The calculated non-LISP prefix is included in the negative map reply so that the ITR can add this prefix to its map cache and FIB. From that point forward, the ITR can

send traffic that matches that non-LISP prefix directly to the PETR.

Step 1. host1 perform a DNS lookup for [www.cisco.com](http://www.cisco.com/). It gets a response form the DNS server with IP 100.64.254.254 and forward packet to the ITR with DIP 100.64.254.254

Step 2. The ITR sends a map request to the MR for 100.64.254.254

Step 3. The mapping database system responds with a negative map reply that includes a calculated non-LISP prefix for the ITR to add it to its mapping cache and FIB

Step 4. The ITR can now start sending LISP-encapsulated packets to the PETR

Step 5. The PETR de-encapsulates the traffic and sends it to [www.cisco.com](http://www.cisco.com/)

![](<../.gitbook/assets/Unknown image (1633)>)

#### Proxy ITR (PITR)

receive traffic destined to LISP EIDs from non-LISP sites. PITRs behave in the same way as ITRs: They resolve the mapping for the destination EID and encapsulate and forward the traffic to the destination RLOC. PITRs send map request messages to the MR even when the source of the traffic is coming from a non-LISP site (that is, when the traffic is not originating on an EID). In this situation, an ITR behaves differently because an ITR checks whether the source is registered in the local map cache as an EID before sending a map request message to the MR. If the source isn’t registered as an EID, the traffic is not eligible for LISP encapsulation, and traditional forwarding rules apply.

![](<../.gitbook/assets/Unknown image (1634)>)

### Deployment notes

Before you can configure LISP, you will need to determine the type of LISP deployment you intend to deploy. The LISP deployment defines the necessary functionality of LISP devices, which, in turn, determines the hardware, software, and additional support from LISP mapping services and proxy services that are required to complete the deployment.

LISP configuration requires the datak9 license

By default, the outer header uses the IP address of the xTR egress interface as source address, this can be changed by issuing the ip lisp source-locator command under the xTR egress interface configuration
