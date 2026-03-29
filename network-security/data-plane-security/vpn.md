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

# VPN

### Virtual Private Network (VPN)

The easiest and cheapest way to achieve mutual communication and data exchange today is to use the Internet

Even some companies use the Internet to communicate between their branches or offices spread all over the country, because it is much less expensive than to bring your own cable across the republic, or the whole country, to connect company offices

But the connection via the Internet is very dangerous, because anyone can connect through it and by various ways of attack can influence or eavesdrop on the flow of data, which can cause companies great financial damage, or gain your private data

This is why various technologies have been invented to encrypt data and establish secure connections, thus preventing almost any threat to the Internet.

One of them is the concept of **Virtual Private Network (VPN)**, which allows private networks to communicate with each other.

**Virtual Private Network (VPN)** is an overlay network that allows private networks to communicate with each other across a shared or untrusted network such as the Internet.

It aims to provide the same policies and services as a private network, even though they won't own the network and won't control it as much

Without VPN, the MITM attack can spoof the website that is accessed even when HTTPS is used

With a VPN, internet traffic is encrypted and routed through the VPN server, preventing others on the network from discerning the websites being visited, even if they don't use HTTPs

![diagram](<../../.gitbook/assets/Unknown image (1742)>)

### Remote-access VPN

**Remote-access VPN** is designed to establish a secure connection between an individual user's device (such as a laptop) and the corporate network.

This type of VPN is commonly used by mobile workers or remote employees who need to access corporate resources from outside the office.

VPN client is a software application or device that enables users to establish a secure connection with a remote Network Access Server (NAS)/VPN Server to access private network over the Internet

VPN server's public IP address is configured in the VPN client application as well as authentication details (username/password,certificate), and encryption settings

The VPN client initiates a handshake with the VPN server, exchanging credentials and negotiating encryption algorithms.

Upon successful authentication, secure tunnel is established between client and server, using tunneling protocols such as IPsec, SSL/TLS , or OpenVPN, granting authorization to the client to access the VPN. The VPN server assigns a private IP from the corporate IP space to the VPN client, so that it can be identified within the VPN

The VPN client software dynamically creates a Virtual Adapter/NIC on the user's device, acting like a regular network interface but routes traffic through the VPN tunnel

The VPN client encrypts all outgoing data using algorithms like AES and sends it through Virtual NIC. Same applies for receiving traffic - the VPN client decrypts it and processes it

Cisco AnyConnect SSL VPN: Allows remote users to access corporate networks from anywhere on the Internet through an SSL VPN client

Clientless Cisco SSL VPN: Allows the user to securely access the corporate LAN from anywhere using a web browser

Once the user is authenticated, the VPN gateway establishes a secure SSL/TLS connection with the user's web browser - other advantage is that the SSL/TLS port is usually open on any kind of firewalls

VPN Concetrator is a dedicated hardware used by large companies that can manage thousands of concurrent VPN connections, and perform authentication and tunnel encryption

![](<../../.gitbook/assets/Unknown image (1743)>)

Connection established from SOHO 192.168.0.0/24 to VPN 10.32.0.0/24. The VPN server public IP is 193.239.0.2

![](<../../.gitbook/assets/Unknown image (1744)>)

![](<../../.gitbook/assets/Unknown image (1745)>)

![](<../../.gitbook/assets/Unknown image (1746)>)

![](<../../.gitbook/assets/Unknown image (1747)>)

### Site-to-site VPN

for site-to-site VPN there must be a router on each site, that can use technology such as GRE with IPsec to connect two sites via Internet

#### Generic Routing Encapsulation (GRE)

**Generic Routing Encapsulation (GRE)** is a tunneling protocol that creates a logical connection on top of a physical one.

It is used to allow to connect remote sites over an transport underlay network such as Internet, as if they were directly connected by P2P link

Can be used to create VPNs, tunnel traffic through a firewall/ACL or to connect discontiguous networks.

GRE encapsulation is identified in the IP header as IP protocol 47, thus it does not use TCP or UDP

Original IP packet is encapsulated into new GRE, that adds it's new header, which contains the remote endpoint IP address as the destination

The GRE tunnel is up as long as there is underlay IP connectivity between the remote sites

GRE was originally created to provide transport for non-routable legacy protocols such as Internetwork Packet Exchange (IPX) across an IP network

Standalone GRE lacks inherent security features, which is why it is commonly utilized in conjunction with IPsec to enhance the security of encapsulated traffic

![](<../../.gitbook/assets/Unknown image (1748)>)

![](<../../.gitbook/assets/Unknown image (1749)>)

![Anatomy Of GRE Tunnels | Packet Pushers](<../../.gitbook/assets/Unknown image (1750)>)

![](<../../.gitbook/assets/Unknown image (1751)>)

| Configuration                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| interface Tunnel0                        | Also referred to as Virtual Tunnel Interface (VTI), is routable interface used to terminate tunnels (including IPsec)                                                                                                                                                                                                                                                                                                                                                                                   |
| tunnel source 100.64.1.1                 | or we can specify the WAN source interface Gi0/1                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| tunnel destination 100.64.2.2            | WAN IP of the destination device                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ip address 192.168.100.1 255.255.255.255 | This is the IP that we want to use to establish connection to the remove device                                                                                                                                                                                                                                                                                                                                                                                                                         |
| tunnel mode gre \[ip\|ipv6]              | this is the default mode of tunnels - not required to be configured                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| bandwidth \[1-10000000]                  | optional; for best-path calculation or QoS                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| keepalive \[seconds \[retries]]          | default is 10sec with 3 retries                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ip mtu mtu <>                            | optional; GRE tunnel additional IP header, so MTU have to be adjusted accordingly to avoid fragmentation that worsens network performance                                                                                                                                                                                                                                                                                                                                                               |
| tunnel ttl <1-255>                       | Traceroute does not display all the hops in the underlay In the same fashion, the packet’s time-to-live (TTL) is encapsulated as part of the payload The original TTL decreases by only one for the GRE tunnel, regardless of the number of hops in the transport network                                                                                                                                                                                                                               |
| tunnel path-mtu-discovery                | When you enable the PMTUD on a GRE tunnel, the GRE packets are sent with the DF bit set and the router responds to the incoming ICMP destination unreachable messages with the reduction of the tunnel MTU size. The decreased MTU can only be inspected with the show interface command. Regardless of the tunnel PMTUD algorithm, the router fragments or rejects the tunneled packets if their size exceeds the IP MTU size configured on the tunnel interface with the ip mtu configuration command |

#### Recursive routing issue

the tunnel source interface IP address should not be advertised into a GRE tunnel, because when the router learns the destination IP address for the tunnel interface through the tunnel itself, it removes the previous entry for the tunnel destination IP address from the routing table, making the tunnel’s destination unreachable and triggers Midchain syslog

\*Jul 9 07:59:50.611: %ADJ-5-PARENT: Midchain parent maintenance for IP midchain out of Tunnel11 - looped chain attempting to stack

\*Jul 9 07:59:54.151: %TUN-5-RECURDOWN: Tunnel11 temporarily disabled due to recursive routing

![GRE Tunnel and Exposed Underlay Routing](<../../.gitbook/assets/Unknown image (1752)>)

![Running Routing (EIGRP) On GRE Tunnel Overlay](<../../.gitbook/assets/Unknown image (1753)>)

Foxtrot13 is now learning the IP address (10.4.14.2) used as its destination IP address for the tunnel via EIGRP over the tunnel

![Steps for understanding why tunnel went down due to recursive routing](<../../.gitbook/assets/Unknown image (1754)>)

Note An VPN Tunnel within specific VRF can use tunnel source IP/interface and tunnel destination of an interface that is not directly associated with the VRF

Example: This tunnel in vrf CZ-DC1-VRF001 use tunnel source IP from an interface in global vrf (to further explain this scenario, this tunnel is to establish P2P connection with a tunnel destination to establish eBGP)

![](<../../.gitbook/assets/Unknown image (1755)>)

### FlexVPN

Interoperable with non-Cisco implementations and therefor works with 3rd party devices

Unified CLI for configuring different VPN types:

Site-to-Site

Hub-to-Spoke

Spoke-to-Spoke

Remote Access

### Group Encrypted Transport VPN (GET VPN)

Designed for any-to-any tunnel-less VPNs using the original IP header for routing encrypted traffic across ISP MPLS or private WANs

### Dynamic Multipoint VPN (DMVPN)

**Dynamic Multipoint VPN (DMVPN)** is a hub-and-spoke overlay routing architecture used to build a secure and scalable dynamic VPN over public or private networks such as the Internet.

Each spoke router establish only a single tunnel to the hub router, that enables spokes to create on-demand tunnels for direct communication between them

When new spokes are added to the network, they can simply establish a dynamic tunnel to the hub router without any additional configuration on the hub router

DMVPN is Non-broadcast Multiple Access (NBMA) network, where NMBA address refers to the public IP of a DMVPN router

![](<../../.gitbook/assets/Unknown image (1756)>)

#### Components

GRE is used to encapsulate tunnel's private IP inside public IP address. Used only for point-to-point (pure hub-to-spoke)

Multipoint GRE (mGRE) destination IP address for a tunnel interface is not specified as it is being learned dynamically with NHRP, allowing to establish full mesh between routers

(Optional) Routing Protocol an IGP is chosen to exchange routes between Hub and Spoke

(Optional) IPsec to encrypt the GRE-encapsulated traffic

Next Hop Resolution Protocol (NHRP) allows Hub to as an ARP server, that resolves the next-hop address for spoke routers

Next Hop Client (NHC) = Spoke, Register themselves to the server and report their own NBMA address (public IP address)

Next Hop Server (NHS ) = Hub, keeps track of all NBMA addresses (public IP addresses) in its NHRP cache

![DMVPN | zartmann.dk](<../../.gitbook/assets/Unknown image (1757)>)

We can use DMVPN in three different ways. They are called phases

#### Phase 1 (obsolete)

was the original implementation of DMVPN. It’s based only around the Hub and spoke model

VPN tunnels are created only between spoke and Hub sites. Traffic between spokes must traverse the hub to reach any other spoke, so the Hub is still in the data plane

Summarization can be performed on the Hub and advertised to spokes, to conserve space in their RIB

Problems

When traffic arrives at the hub, it needs to be decapsulated. It will then be encapsulated again and sent to destination spoke = huge overhead

Split-horizon has to be disabled on the hub tunnel interface to make route advertisements between spokes work

![](<../../.gitbook/assets/Unknown image (1758)>)

#### Phase 2 (obsolete)

adds mGRE to hub and spoke routers, so they can communicate directly without having traffic traversing via Hub, so the Hub is now only in the control plane

When a spoke router wants to communicate with another spoke router in the network, it sends an NHRP request to the hub router. The Hub examines it's NHRP mapping table to find the NBMA address for a destination spoke. The Hub will forward the Resolution request to the destination spoke router. Destination spoke router caches the information in the request, and sends an NHRP Resolution Response directly to the source spoke router. Response contains the NBMA address of the destination spoke router, so spokes can find themselves to form dynamic tunnel between them and torn it down when no longer needed. Each spoke maintains permanent static tunnel with the Hub

![](<../../.gitbook/assets/Unknown image (1759)>)

First traceroute flows through the hub to terminate dynamic tunnel to destination spoke

![](<../../.gitbook/assets/Unknown image (1760)>)

Second traceroute flows directly to the spoke via dynamic tunel

![](<../../.gitbook/assets/Unknown image (1761)>)

Limitation of Phase 2

For example Spoke1 will receive route 10.2.0.0/24 with the next hop of 10.0.0.4 even though the route has been “relayed” via the hub. This installs a special CEF entry for 10.0.4.0/24 with the next hop set to 10.0.0.4 and marked as “invalid”. At the same time, the CEF adjacency for 10.0.0.4 is marked as “glean” meaning it needs L3 to L2 lookup to be performed, which is performed by NHRP, after an initial packet is being sent to 10.0.4.0/24

![](<../../.gitbook/assets/Unknown image (1762)>)

This invalid CEF entry makes the initial NHRP request packet to be routed using process switching to the Hub, which in turn responds and completes the CEF entry for the spoke

The second main problem with the DMVPN Phase 2 is that when we summarize on the Hub, and since Spoke1 has the summary route 10.0.0.0/8 in the RIB, it will not send NHRP request anymore as it already knows that it should pass any traffic to the Hub, so the traffic to other spoke (Spoke2) will always pass through the Hub

So in order to maintain Phase 2 ability to establish direct communication between the spokes, they have to preserve the next-hop and have unsummarized specific entries for their delegated networks, thus the spoke has to maintain all specific routes in it's RIB, consuming memory. This limits the scalability in large networks

![](<../../.gitbook/assets/Unknown image (1763)>)

![](<../../.gitbook/assets/Unknown image (1764)>)

#### Phase 3

allows summarization of routes at the Hub, reducing the size of routing table size and improves DMVPN scalability. This phase is also called NRHP Override (NHO)

A spoke will no longer start with an NHRP Resolution Request, instead, a spoke will simply start sending traffic to the hub

The Hub then sends the source spoke an NHRP Redirect message (similar to ICMP redirect message; it is also referred to as Traffic indication) indicating that i doesn't own the network and redirects the source spoke router to the NBMA of the destination spoke. The Redirect message also completes the CEF entry for the Spoke.

Then spoke proceeds to send the NHRP request directly to the destination spoke

The destination spoke receives the request, making him learn about the source spoke, and sends the NHRP resolution to the source spoke to form a dynamic tunnel

| #redirect | is configured on the hub                                                                                                                                                                                              |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| #shortcut | is configured on the spoke with summary route to destination spoke networks with hub as a next hop shortcut command enables the NHRP to dynamically insert the new route or overwrite existing route in routing table |

![](<../../.gitbook/assets/Unknown image (1765)>)

![](<../../.gitbook/assets/Unknown image (1766)>)

The RIB stays with the summary/default route, and the specific routes are now maintained in the FIB (this is the intentional DMVPN conflict between FIB and RIB)

However when that particular prefix is not summarized at the Hub, it is present in the RIB with % tag - next hop override

![](<../../.gitbook/assets/Unknown image (1767)>)

![](<../../.gitbook/assets/Unknown image (1768)>)

#### DMVPN and dynamic routing

Multicast can only travel between hub and spoke. This is because DMVPN is an NBMA network, that doesn’t support multicast or broadcast

The only reason we can get it working to the hub is because of the nhrp multicast command we add to the tunnel interface.

As there’s no multicast on spoke-to-spoke tunnels, there is no traditional IGP running between spokes. Routes are learned through the hub router

So, guideline number 1 is, don’t try to run a routing protocol between spokes. Leave it to NHRP to work out the routing for you

Generally avoid phase 2 with dynamic routing, causing the large RIB for each specific route and avoid OSPF since it is difficult to enforce the required hierarchy

![](<../../.gitbook/assets/Unknown image (1769)>)

**With OSPF**

as the hubs have a single tunnel interface, spokes that connect to it all have to be in the same area, which limits scalability. This is due to Phase 2 does not support VRF's that allows for the creation of separate routing domains, enabling the spokes to be associated with different OSPF areas

Network type point-to-multipoint is recommended because of no DR/BDR election

Network type broadcast can be used, but then DMVPN is limited to two hubs and the priority must be explicitly configured (255 on the hubs, 0 on the spokes) so that no spoke router becomes the DR/BDR, which woud cause entire DMVPN design to bring down (Since the Hub would lose his DR position and bring down all the tunnels)

with all other OSPF network types you would destroy purpose of dmvpn, since you would have to manualy map each spoke in a topology

**With EIGRP**

In DMVPN, the tunnel interface is used for everything, including learning routes and advertising them back through the same tunnel.

However, split-horizon would block this, and Spoke-2 will never get the route, so the simplest solution is to disable #no ip split-horizon eigrp {AS} on the tunnel interface on hub

Phase 1 and Phase 3 have been designed to bypass the split horizon limitation, enabling the advertisement of summary or default routes from the hub without being blocked

Since the split-horizon is bypassed, you will find that the hub updates the route with it’s own IP as the next-hop IP

This introduces a new problem, especially noticeable in phase 2. If the hub is the next hop for all routes, then all traffic will flow via the hub

This effectively disables spoke-to-spoke tunnels, which is the whole point of phase 2

To remedy this, use #no ip next-hop-self eigrp {AS} on the hub tunnel interface. This prevents the hub from making itself the next hop, allowing spoke-to-spoke tunnels

**With BGP**

iBGP - not optimal. Usable only if all the spokes are part of your organization. Hub as Router Reflector

In phase 1 or phase 3, the hub routers will need to be the next-hop. This is simply configured with the next-hop-self command

In phase 3, even though the hub is the next hop, NHRP shortcuts still enable spoke to spoke communication

eBGP suitable for multi-tenancy deployment with hubs as part of core AS

Hub in one AS and all other spokes in same AS > phase 1 or 3 have to be used to avoid spokes to drop route advertisements from other spokes (BGP loop prevention mechanism to drop advertisements from same AS).

As the hub originates the route, the spoke ASN, is not in the path, and therefore the route is not dropped. Or we can have spokes in different AS to avoid this issue

![](<../../.gitbook/assets/Unknown image (1770)>)

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

### Dual / Multi-Hub setup

* The second hub must be registered to the first hub as spoke.
* For each hub, there must be a separate ip nhrp nhs command configured on the spoke(s).

### DMVPN with IPsec

* IPsec should be applied to the tunnel interface to encrypt traffic passing through the tunnel.

### Per-Tunnel QoS

Used to apply QoS on a per hub-to-spoke-tunnel basis. It cannot be applied on dynamic spoke-to-spoke tunnels. Tunnels must be flapped to apply QoS config.

Example commands:

```
Router(config-if)# nhrp map group <group-name> service-policy output <policy-name>  # Per-Tunnel QoS on hub
Router(config-if)# nhrp group <group-name>                                    # Per-Tunnel QoS on spoke
Router# show policy-map multipoint
```

### MPLS over DMVPN

* Used for traffic segmentation (e.g. overlapping address space).
* Based on NHRP instead of LDP (because LDP uses keep-alives and spoke-to-spoke tunnels would never be terminated).
* Configure mpls nhrp on all tunnel interfaces which belong to the DMVPN solution (hub and spokes).
* Configure iBGP VPNv4 between spokes and hub.
* Hub only: summarize routes for each VRF.

Example command:

```
Router(config-if)# mpls nhrp   # Enabling MPLS NHRP on all DMVPN interfaces
```

### ODR (On-Demand Routing)

* Cisco proprietary.
* With ODR enabled in DMVPN, spokes can learn routes dynamically from hub without every spoke learning all other spokes — reduces control traffic.
* Enable ODR on the hub and on each spoke router.
* Biggest concern: CDP timing. CDP frames are sent every 60 seconds. Dropped routes are marked invalid after three CDP intervals (180s) and removed after 4 intervals (240s) — these timers may need adjustment to avoid poor convergence.

### DMVPN with IPv6

* IPv4 only: use only ip nhrp commands.
* IPv4 over IPv6: the ip nhrp nhs must be mapped to the IPv6 NBMA address.
* IPv6 only: use only ipv6 nhrp commands.
* IPv6 over IPv4: the ipv6 nhrp nhs must be mapped to the IPv4 NBMA address.
* For IPv4 over IPv6, configure the tunnel mode:

```
tunnel mode gre multipoint ipv6
```

***

### NHRP Flags

<details>

<summary>NHRP Flags and meanings</summary>

* Authoritative: NHRP information was obtained from a NHS.
* Implicit: NHRP information was obtained/learned from a NHRP resolution request or packet.
* Local: NHRP mapping entry for a network that is local to this router.
* NAT: Remote device supports NHRP NAT extensions (allows dynamic spoke-to-spoke tunnels to/from spokes behind a NAT router).
* Negative: The requested NBMA address for a NHRP mapping could not be obtained.
* (no socket): Router won’t set up IPsec encryption for this mapping because there’s no data traffic that uses this tunnel.
* Registered: NHRP mapping entry was created from a received NHRP registration request.
* Router: NHRP mapping entries for a remote router itself to access a network/host behind it.
* Unique: NHRP mapping entry can’t be overwritten by a different NHRP mapping entry with the same tunnel address but different NBMA address.
* Used: Data packets are being process-switched for the NHRP mapping.

</details>

***

### Configuration Phases (Phase 1 → Phase 2 → Phase 3)

### Phase 1 — Point-to-Point GRE (Hub + Spokes)

Hub (example Tunnel0):

```
interface Tunnel0
 ip address 10.0.0.1 255.255.255.0
 tunnel source 22.22.22.1             ! WAN interface public IP (NBMA)
 ip nhrp network-id 1                 ! Domain ID; must match across routers
 tunnel mode gre multipoint
 ip nhrp map multicast dynamic
 ip mtu 1400
 ip tcp adjust-mss 1360
 ip nhrp authentication cisco
 tunnel key 100
```

Spoke (example Tunnel0):

```
interface Tunnel0
 ip address 10.0.0.2 255.255.255.0
 tunnel source 21.21.21.1
 tunnel destination 22.22.22.1       ! Phase 1 uses p2p GRE; destination is hub NBMA
 ip nhrp network-id 1
 ip nhrp nhs 10.0.0.1 nbma 22.22.22.1 multicast
 ip nhrp map multicast 22.22.22.1
 ip mtu 1400
 ip tcp adjust-mss 1360
 ip nhrp authentication cisco
 tunnel key 100
```

Notes:

* MTU/MSS adjustments listed above are recommended (IPv4: MTU 1400, MSS 1360; IPv6 MSS 1340).
* NHRP authentication must match on hub and spokes for mutual authentication.
* Tunnel key is optional but must match across routers if used.

````
{% endstep %}

{% step %}
## Phase 2 — mGRE (Spoke: remove tunnel destination)

On Spoke:
```text
interface Tunnel0
 no tunnel destination 22.22.22.1
 tunnel mode gre multipoint
````

This enables dynamic spoke-to-spoke tunnels (mGRE). \{% endstep %\}

\{% step %\}

### Phase 3 — Optimizations (redirect/shortcut)

On Hub:

```
interface Tunnel0
 ip nhrp redirect
```

On Spoke:

```
interface Tunnel0
 ip nhrp shortcut
```

Useful commands:

```
show dmvpn [detail]
show ip nhrp [shortcut | redirect]
debug NHRP
debug dmvpn detail crypto
```

\{% endstep %\} \{% endstepper %\}

***

### Other NHRP commands

* ip nhrp registration no-unique\
  Used to allow NHRP registration without requiring a unique registration for each source IP address. Useful when multiple devices behind NAT share a common public IP.

***

### IPsec + Routing Example (Hub)

Example configuration (Hub) that demonstrates applying IPsec protection to the tunnel and EIGRP routing.

```
! Hub - Interfaces
int Loopback0
 ip address 1.1.1.1 255.255.255.255

interface GigabitEthernet1
 ip address 10.10.255.254 255.255.255.0
 no shut

interface Tunnel0
 ip address 192.168.1.254 255.255.255.0
 no ip redirects
 ip mtu 1400
 no ip next-hop-self eigrp 100
 no ip split-horizon eigrp 100
 ip nhrp authentication ccnp123
 ip nhrp network-id 1
 ip tcp adjust-mss 1360
 tunnel source GigabitEthernet1
 tunnel mode gre multipoint
 tunnel key 100
 tunnel protection ipsec profile PROFILE

! IKE / ISAKMP
crypto isakmp policy 1
 encryption aes
 hash sha512
 authentication pre-share
 group 20

crypto isakmp key ccnp123 address 0.0.0.0 0.0.0.0

! IPsec transform/profile
crypto ipsec transform-set IPSEC esp-aes esp-sha512-hmac
 mode transport

crypto ipsec profile PROFILE
 set transform-set IPSEC
 set pfs group24

! EIGRP
router eigrp 100
 network 192.168.1.0 0.0.0.255
 network 1.1.1.1 0.0.0.0
```

***

### Useful "show" and debug commands

* show dmvpn \[detail]
* show ip nhrp \[shortcut | redirect]
* show policy-map multipoint
* debug NHRP
* debug dmvpn detail crypto

***

### Other example snippets / notes

* Use ip nhrp map group with service-policy on a per-tunnel QoS basis:

```
Router(config-if)# nhrp map group <group-name> service-policy output <policy-name>
```

* To enable MPLS over DMVPN interfaces:

```
Router(config-if)# mpls nhrp
```

* Example: ip nhrp registration no-unique

```
ip nhrp registration no-unique
```

***

End of document.
