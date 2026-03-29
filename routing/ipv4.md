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

# IPv4

### Why do we need IP addressing?

Since each network device, more precisely, it's network identification card (NIC) has its own burnt-in MAC addresses assigned from its manufacturer

If we were to build a network with a devices purchased from a different vendors, which is the usual scenario, we would miss the hierarchical structure that wouldn't allow us to separate devices in our network from devices in other networks or unify and expose our network with a common network identifier.

Since MAC addresses of all devices wouldn't be separated and grouped in networks, the switches participating in forwarding the traffic would have to maintain MAC address for every single device in the world, which would consume tremendous amount of memory and wouldn't be possible

Moreover if the NIC of a device or server break down, we would have to buy a new device with new NIC with a new MAC address and thus every switch in the Internet would have to update it's MAC table, which would cause very frequent updates and also wouldn't be possible

With IP address, we can use a hierarchical type of addressing, allowing us separate devices that belong to one network between devices belonging to different network

With IP we can summarize and indentify our network and dynamically assign an IP address to a device regardless of its physical address assigned by the manufacturer

This time, the mailing service (router) doesn't has to know the exact location of an address, but just a part of it - and this is exactly what happens with IP: Routers have route summaries

Now why do we need MAC address, when we have IP addresses?

Network cards, including wifi adapters are operating on Layer 2 and are designed for direct communication on the link or a shared segment such as LAN and the do not respond to IP addresses (that is a function of the IP stack in the OS). They respond to MAC addresses on Layer 2, since it gets converted to a MAC address to be transmitted over a link layer

MAC is what gets the message from one hop to another, while IP keeps track of the original source and destination

IP alone does not provide a mechanism for device-to-device delivery on the same local segment.

You’d have to invent a new method for local delivery (e.g., treating IP like a Layer 2 identifier), which breaks the OSI model and would not be compatible with legacy hardware.

Hardware is physically locatable, so if a hardware address were used on the Internet, an attacker could know where to shoot to attack a particular node

The bandwidth of the original ARPAnet relied on dial-up links that were as slow as 300 bits per second, and specified a maximum packet length of 576 characters, so 32-bit IPv4 address consumed less overhead in compare to 48-bit MAC address

### Internet Protocol (IP)

**Internet Protocol (IP)** ensures packet delivery between different networks and provides logical addressing for identifying systems in a network.

IP uses hierarchical addressing, in which the network identification is the equivalent of a street, and the host ID is the equivalent of a house or an office building on that street.

IP provides service on a best-effort basis and does not guarantee packet delivery. A packet can be misdirected, duplicated, or lost on the way to its destination.

IP does not provide any special features that recover corrupted packets. Instead, the end systems of the network provide these services.

There are two types of IP addresses, IPv4 and IPv6, the latter becoming increasingly important in modern networks.

The logical address, called an IP address, can be assigned to each device (in it's operating system) or to an interface of a Layer 3 network device, such as a router, to uniquely identify the interface or system within the network.

Routers use these addresses to route packets between networks, ensuring delivery via the optimal path.

The IP address of the sender device, known as the Source IP, is included in the packet alongside the Destination IP, forming an IP datagram (or IP packet).

At the destination, packets are reassembled into the original message and passed to the transport layer, which further reassembles the data stream for delivery to the application

IP is Connectionless meaning that it does not establish an end-to-end connection before sending data. Each packet of data is treated independently and can take different paths to reach the destination. There is no guarantee that packets will reach their destination, nor is there any built-in mechanism for retransmission or error correction - this is handled by Layer 4

Routers and devices do not retain session state information between packets

### Management authorities of Internet addressing

Internet Assigned Numbers Authority (IANA) was originally the sole authority managing the global IPv4 address space

As the internet grew, the need for a more structured and efficient resource management system emerged

In 1992, the Internet Engineering Task Force (IETF) introduced the concept of Regional Internet Registries (RIRs).

Regional Internet Registries (RIR) are organizations responsible for managing internet number resources within specific geographical regions.

Today, there are five RIRs operating globally: AFRINIC, APNIC, ARIN, LACNIC, and RIPE NCC.

IANA remains at the top of the hierarchy, allocating large blocks of IPv4 addresses to RIRs.

RIRs then distribute IPv4 address space to regional ISPs and end-users.

AFRINIC – Responsible for regions: African continent

APNIC – Responsible for regions: East, South and Southeast Asia as well as Oceania

ARIN – Responsible for regions: Antarctica, Canada, parts of the Caribbean and the United States

LACNIC – Responsible for regions: Caribbean, Mexico and South America

RIPE NCC – Responsible for regions: Europe, Russia, West and Central Asia

Autonomous system numbers (ASNs) and the IPv4 and IPv6 address spaces are distributed by RIRs to the regional ISPs and end-users

Local Internet Registry (LIR) are usually Internet Service Providers (ISPs) or large organizations that receive IP address allocations from an RIRNational Internet Registry (NIR) manage IP address allocations within a specific country, under the guidance of an RIR. They operate similarly to LIRs but at a national level

{% hint style="info" %}
Currently, the wait for public IPv4 allocation is several months or even years
{% endhint %}

![](<../.gitbook/assets/Unknown image (1037)>)

### IP version 4 (IPv4)

An IPv4 address is a 32-bit (4-byte) number written in dotted decimal format, giving 4.2 billion possible combinations, is hierarchical

The minimum value of an 8-bit binary number is 00000000, which in decimal equals 0. The maximum value of an 8-bit binary number is 11111111, which in decimal equals 255. If you have a number that is larger than 255, it cannot be written with 8 bits. For each of the decimal numbers an IPv4 address must be a number between 0 and 255.

Representing IPv4 address in raw binary format (e.g., 00001000.00001000.00001000.00001000 for 8.8.8.8) is cumbersome for humans to read and manage.

The human-readable format, also known as dotted decimal notation, separates the 32-bit binary number into four octets (8-bit groups) and converts each octet to its decimal equivalent, which can range from 0 to 255 (when all bits are set to 1). These decimal values are separated by periods, making it easier to remember and interpret IP addresses like 8.8.8.8.

{% hint style="info" %}
If we convert raw IPv4 format of 8.8.8.8 without separating each 8bits, it equals to 134744072 (2^27+2^19+2^11+2^3), so if you run the command ping 134744072 on Windows or Linux you will see that they generate ping packets to 8.8.8.8
{% endhint %}

{% hint style="info" %}
The term "octet" is commonly used in computer networking and informatics to mean a group of eight bits. "Octo" comes from the Greek word for "eight" or "eighth", referring to the fact that an octet contains exactly eight bits
{% endhint %}

![](<../.gitbook/assets/Unknown image (1038)>)

IP address consists of two parts:

* **Network address portion (network ID):** Network ID is the portion of an IPv4 address that uniquely identifies the network in which the device with this IPv4 address resides. The network ID is important because most hosts on a network can communicate only with devices in the same network. If the hosts need to communicate with devices with interfaces assigned to some other network ID, a network device—a router or a multilayer switch—can route data between the networks.
* **Host address portion (host ID):** Host ID is the portion of an IPv4 address that uniquely identifies a device on a given IPv4 network. Host IDs are assigned to individual devices, both hosts or endpoints and intermediary devices.

![](<../.gitbook/assets/Unknown image (1039)>)

### Network address

Each device belongs to a network of several other devices, in a LAN network for example, and thus for the correct delivery of a packet, the network itself, which identifies and separates a group of devices, must be also uniquely identified. Then, in a given network, each device is identified by an assigned IP address.

Network address identify different segments or subnetworks within a larger network infrastructure, so that the router can identify all networks connected to it's interfaces

You can imagine this from the point of view of a router that receives a packet from a sender that wants to send a packet to another device.

The router looks at the destination IP address to determine the network, where the device resides, and if this network is in it's routing table, it sends it to the next-hop router that advertised him knowledge of this network. Then the router that has the destination network connected to it's interface, proceeds to send the packet to that particular IP of the device

The network address is always the first address of the network - all bits in host part are set to be all zeros - for example 172.16.0.0

### Broadcast address

**Broadcast address** is a special address (all bits in host part are set to be all ones) that belongs to all devices in the IP subnet. Any message sent to this address reaches all devices on the subnet

A broadcast is a destination-only address. It is never used in the source address field of data packets.

A Layer 2 broadcast domain is a domain in which all devices see each other's Layer 2 broadcast frames, while a Layer 3 broadcast domain is a domain in which all devices see each other's Layer 3 broadcast packets

{% hint style="info" %}
Routers, by default, are configured to not forward broadcast packets out of the network in order to isolate each network segment and to prevent network congestion and security risks
{% endhint %}

**Limited broadcast (255.255.255.255):** Broadcast address of the zero network (`0.0.0.0`). DHCP clients use it to discover servers.

![](<../.gitbook/assets/Unknown image (1040)>)

**Directed broadcast:** Uses the broadcast address of a specific subnet that is not the local subnet. It can be routed over an intranet or the Internet.

For example when host wants to send packet to a broadcast IP address 192.168.1.255/24 to the foreign network

Routers in the path will forward these directed broadcast packets as normal packets, because, they don't know that the message is destined for a broadcast address, because the IP header only includes a destination IP address, not a destination subnet mask. To them, the packet looks like a unicast packet.

When the router connected to the destination subnet receives the message, it will know that the destination is a broadcast address (because it knows the subnet mask of the destination network)

{% hint style="info" %}
Subnet/directed broadcast IP is not much used in modern networks
{% endhint %}

| R1 (config-if)# ip directed-broadcast | By default, router will drop the message. Forwarding of directed broadcast messages can be enabled with the following command (on the interface the message will be broadcast out of) |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Directed broadcasts were invented at the dawn of computer networking, when the Internet was a much friendlier place

Back then it was simple enough to simply trust the other users on the Internet not to abuse the Directed broadcast.

This capability can be restored with the ip directed-broadcast command in the global configuration mode. It is a best practice to leave directed broadcasts disabled unless you have a specific use case. Routers began using the no ip directed-broadcast command as a platform default, starting with Cisco IOS Release 12.0.

![](<../.gitbook/assets/Unknown image (1041)>)

**Unicast:** Standard communication to a single destination.

**Multicast:** Special address used to communicate with devices that subscribed to receive the traffic.

Used mainly by routing protocols to subscribe for reception of route advertisements, or group of systems communicating with each other frequently - such as video streaming

![What is Multicast? — Techslang](<../.gitbook/assets/Unknown image (1042)>)

**Anycast:**

is a single address configured on multiple devices to allow one-to-nearest , so that when a packet is destined to anycast address it is routed to the nearest device among the group of devices sharing the same anycast IP

Anycast addresses are used by Content Delivery Networks (CDNs) to define a service that can be hosted by multiple nodes dispersed across the network

Each node advertising the anycast address provides the same service, and the routing infrastructure directs traffic to the nearest instance of the service, this ensures load balancing and redundancy

A typical example of anycast address use is DNS service. When authoritative DNS server needs to find out the translate FQDN to an address, it sends a query to the well-known anycast address of the DNS service. The query is directed by the network to the nearest node of the network. When unavailable, network delivers query to another node

Anycast does not create conflicts because routers use the routing protocol to direct traffic to the nearest device advertising the address. The concept of “nearest” ensures that only one route is active at a time, avoiding ambiguity.

![](<../.gitbook/assets/Unknown image (1043)>)

### IPv4 header

Before you can send an IP packet, there needs to be a format that all IP devices agree on to route a packet from the source to the destination. All that information is contained in the IP header. The IPv4 header is a container for values that are required to achieve host-to-host communications. Some fields (such as the IP version) are static, and others, such as Time to Live (TTL), are modified continually in transit.

IPv4 header is 20 bytes long, divided into five rows, containing four octets each. In fact only 40% of the header contain addressing info

20 bytes is the minimum length of the overall header; when some additional information are needed, then option fields are added which prolongs the overall length of the header.

![](<../.gitbook/assets/Unknown image (1044)>)

| Version: 4 bits              | specifies whether packet is IPv4 or IPv6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Internet Header Length (IHL) | 4 bit field showing length of the IP header in 32 bit increments. The minimum length of an IP header is 20 bytes (160 bits), so with 32 bit increments the value would be always >=5 (32\*5). The maximum value we can create with 4 bits is 15, so with 32 bit increments, that would be a header length of 480 bits (60bytes)                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Service type: 8 bits         | serves for packet classification to prioritize it over other packets (QoS). This field had name as ToS and now it is renamed to DSCP (Differentiate code point)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Packet Length: 16 bits       | indicates entire size of the header and data in bytes. The minimum size is 20 bytes, with no data; the maximum value we can create with 16-bit is 65535 The minimum size datagram that any host is required to be able to handle is 576 bytes, but most modern hosts handle much larger packets                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Identification (ID): 16 bits | If the IP packet is fragmented then each fragmented packet will use the same 16 bit identification number to identify to which IP packet they belong to The receiver reassembles the data from fragments with the same ID using both the fragment offset and the more fragments flag                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Flags: 3 bits                | The first bit is always set to 0 The second bit is called the DF (Don’t Fragment) bit and indicates that this packet should not be fragmented. True = 1 Non true = 0 If the DF flag is set, and fragmentation is required to route the packet, then the packet is dropped The third bit is called the MF (More Fragments) bit and is set on all fragmented packets except the last one. True = 1 Non true = 0                                                                                                                                                                                                                                                                                                                                           |
| Fragment Offset: 13 bits     | specifies the position of the fragment in the original fragmented IP packet The individual fragments are assembled into the original datagram by the end recipient The time for assembling all fragments into the original datagram is monitored by a timer If all fragments do not arrive within a certain time, all fragments are discarded and the entire original datagram must be sent - RFC 815                                                                                                                                                                                                                                                                                                                                                   |
| Time to Live (TTL): 8 bits   | each packet is set with certain TTL value that represents the counter of how many routers had to route the packet to get to the destination Each router decrements this value by one and sends it to the next router. This "stamp" acts as a loop prevention in case the packet circulate endlessly in the network and the packet cannot be delivered to the destination, which can happen when there is a routing loop Once TTL count hits 0 the router will drop the packet and sends an ICMP time exceeded message to the sender The IP specification also states that TTL should be decremented if a packet is queued for more than a certain amount of time, but this is not in use nowadays                                                       |
| Protocol: 8 bits             | defines which protocol is encapsulated in the IP packet, to indicate to the router that this packet contain certain protocol that he has or hasn't have to process ICMP has value of 1; TCP has value 6 and UDP has value 17, OSPF 89 and EIGRP 88                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Header Checksum: 16 bits     | leveraged for error checking of the IP header. When a packet arrives at a router or its destination, the network device calculates the checksum of all other parts in the header including the checksum field and compares it to the value in the checksum field, if a different result is obtained, the device discards the packet Checksum calculation involves adding 16-bit words, performing a one's complement, and inserting this value in the checksum field. The receiver does the same checksum calculation, and if the result is all 1s, it indicates no errors. Errors in the data portion of the packet are handled separately by the encapsulated protocol - such as TCP or UDp which have separate checksums that they apply to the data |
| Source Address: 32 bits      | (4bytes) source IP address defining the sender device                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Destination Address: 32b     | (4bytes) destination IP address defining the receiver device                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Options (variable length)    | not used in Internet communication and IP packets including some of the IP options that must dropped as per IPv4 security assessment RFC6274, since they can expose the network topology or network details Most of the IP options include specifications how many or which intermediate devices the packet should pass. The size of this field is a multiple of 32 bits. If an option is not 32 bits in the length, it uses Padding in the remaining bits to make the header an integral number of 4-byte blocks                                                                                                                                                                                                                                       |
| Data (Payload)               | Its contents are interpreted based on the value of the Protocol header field (65515 bytes)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |

![](<../.gitbook/assets/Unknown image (1045)>)

{% hint style="info" %}
Whenever an IPv4 header is modified (for example TTL decrement or NAT), the header checksum must be recalculated.
{% endhint %}

### Classful IP addressing (legacy)

In the early days of the internet before the introduction of address classes, the only address blocks available were large /8 blocks, known later as A block (range 0-255.X.X.X)

/8 block were allocated by the owner of IPv4 space - Internet Assigned Numbers Authority (IANA) to the IPv4 contributors

As a result, some organizations involved in the early development of the Internet received address space allocations far larger than they would ever need (16,777,216 IP addresses each)

It became clear that it would be a critical scalability limitation, if the number of networks will continue to increase

So classes were defined: A, B, C, D, and E, each had a fixed range of IP addresses and a default subnet mask

Each class was designed based on first 4 bits of the IP address, providing a specific purpose and accommodates different range of available addresses, dictating the number of devices you can have on your network

If, for instance, you only needed 300 IP addresses, a Class C would not suffice, so you would end up with a Class B and nearly 60,000 IP addresses would be wasted, so this solution also led to a lof of wasted IP addresses. Moreover only one fixed network had to be used, so that all hosts fall into one network

| **Class** | **Address Range**                                            | **Subnet Mask** | **Number of Hosts per Network** | **Reserved Ranges**                                                                                                     |
| --------- | ------------------------------------------------------------ | --------------- | ------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Class A   | 0.0.0.0 - 127.255.255.255 First 8 bits 00000000 - 011111111  | 255.0.0.0       | 16,777,214 (2^24 - 2 )          | Local Network 0.0.0.0/8 Private 1918 10.0.0.0/8 Reserved for CGNAT 100.64.0.0/10 Reserved for Loopback 127.0.0.0/8      |
| Class B   | 128.0.0.0 - 191.255.255.255 First 8 bits 10000000 - 10111111 | 255.255.0.0     | 65,534 (2^16 - 2)               | APIPA 169.254.0.0/16 Private 1918 172.16.0.0/12                                                                         |
| Class C   | 192.0.0.0 - 223.255.255.0 First 8 bits 1100 0000 - 1101 1111 | 255.255.255.0   | 254 (2^8 - 2)                   | Private 1918 192.168.0.0/16 Used for benchamark 198.18.0.0/15 Reserved for documentation 198.51.100.0/24 203.0.113.0/24 |
| Class D   | 224.0.0.0 - 239.255.255.255 First 8 bits 11100000 - 11101111 | N/A             | N/A                             | Reserved for Multicast (never used a unicast)                                                                           |
| Class E   | 240.0.0.0 - 255.255.255.255 First 8 bits 11110000 - 11111111 | N/A             | N/A                             | Reserved for future use Limited Broadcast 255.255.255.255/32                                                            |

n indicates a bit used for the network ID assigned by IANA

H indicates a bit used for the host ID assigned by the owner of the address space

![](<../.gitbook/assets/Unknown image (1046)>)

{% hint style="info" %}
In the modern networking slang the Class C refers to use the /24 prefix and class A /8 prefix, so it is now rather expression which mask is being used which directly correlate to this older approach, where first class A had /8 and Class C /24 mask
{% endhint %}

### Reserved IPv4 address space

Technically addresses from 1.0.0.0/8 to 223.255.255.255/32 can be used for unicast from an /8 address block

This account for approximately 3.5 billion of usable unicast addresses, which is about 7/8 of the total IPv4 address space (223×2^24=3,757,363,712)

In practice, due to the hierarchical nature of IP addressing and the reservation of certain address ranges for special purposes (such as private addresses, multicast addresses, and network infrastructure), not all 32-bit IPv4 addresses are available for use as unicast addresses

Therefore, the actual number of usable unicast addresses may be less than the theoretical maximum of 2^32

#### Private IP range (RFC 1918)

This was the next countermeasure to conserve IPv4 address space with increasing Internet connection demand

It is IP block reserved for routing only in private networks and ignored by the public routers routing internet traffic

This address space can be used in each private network and therefore can be used by several devices that are part of a different location or organization.

If a device assigned with private address wants to communicate with deivce outside of it's private network over the internet, it needs router that performs NAT for that private network and translate the private IP of the host to the public IP of the organization

For example IKEA internal network can use 10.0.0.0/8 while Tesco aswell, but they operate separately and their IP's are not leaked out of their network

Another example can be your SOHO network, where your home subnet has the same IP address space as your neighbor, but they operate separately

![](<../.gitbook/assets/Unknown image (1047)>)

#### Public IP range

IP's reserved for public addressing/routable on the internet

Classful network is an obsolete network addressing architecture used in the Internet from 1981 until the introduction of Classless Inter-Domain Routing (CIDR) in 1993. The method divides the IP address space for Internet Protocol version 4 (IPv4) into five address classes based on the leading four address bits. Classes A, B, and C provide unicast addresses for networks of three different network sizes. Class D is for multicast networking and the class E address range is reserved for future or experimental purposes

| IPv4 Address Class | Public IPv4 Address Range                                                                                      |
| ------------------ | -------------------------------------------------------------------------------------------------------------- |
| A                  | <ul><li><p></p><ul><li>1.0.0.0 to 9.255.255.255</li><li>11.0.0.0 to 126.255.255.255</li></ul></li></ul>        |
| B                  | <ul><li><p></p><ul><li>128.0.0.0 to 172.15.255.255</li><li>172.32.0.0 to 191.255.255.255</li></ul></li></ul>   |
| C                  | <ul><li><p></p><ul><li>192.0.0.0 to 192.167.255.255</li><li>192.169.0.0 to 223.255.255.255</li></ul></li></ul> |

0.0.0.0/8

reserved for local network operations such as specifying default route 0.0.0.0/32

127.0.0.0/8 - Loopback address space

is reserved for internal communication within a device. It enables network applications to communicate with themselves, facilitating local testing and diagnostics without using external network interfaces.

Localhost IP 127.0 0.1

managed entirely by the your operating system, allowing to communicate with itself and remains on the local network

That is why localhost is also referred to as the loopback address - it loops you back to the machine you are logged into - ping 127.0.0.1

Link-local address range

169.254.0.0-169.254.255.255 - reserved for Microsoft Automatic Private IP Addressing (APIPA)

assigned automatically to network interfaces when they cannot obtain a valid IP address from a DHCP server or they haven't been configured manually with the IP address

This basically provides the startup IP for the station, so it is able to communicate with the devices on the local network - assuming that all other devices fallen into assigned APIPA

Only valid and significant within a specific local network segment or link. It cannot be used for communication beyond the local network segment

Multicast reserved addresses

| Range                       | Description                                                                                      |
| --------------------------- | ------------------------------------------------------------------------------------------------ |
| 224.0.0.0 - 224.0.0.255     | Link-local multicast addresses (Reserved for automatic configuration on a local network segment) |
| 224.0.1.0 - 224.0.1.255     | All Systems on this Subnet                                                                       |
| 224.0.2.0 - 224.0.2.255     | All Routers on this Subnet                                                                       |
| 224.0.255.0 - 224.0.255.255 | Multicast DNS                                                                                    |
| 224.0.0.0/24                | Null-0 (Discard route)                                                                           |
| 224.0.0.9                   | RIP Version 2                                                                                    |
| 224.0.0.10                  | EIGRP (Enhanced Interior Gateway Routing Protocol)                                               |
| 224.0.0.18                  | VRRP (Virtual Router Redundancy Protocol)                                                        |
| 224.0.0.22                  | IGMP (Internet Group Management Protocol)                                                        |
| 224.0.0.39                  | OSPFv2 (Open Shortest Path First version 2)                                                      |
| 224.0.0.40                  | OSPFv3 (Open Shortest Path First version 3)                                                      |
| 224.0.0.50                  | DVMRP (Distance Vector Multicast Routing Protocol)                                               |
| 224.0.0.56                  | PIM (Protocol Independent Multicast)                                                             |
| 224.0.0.61                  | All PIM Routers                                                                                  |
| 224.0.0.62                  | All IGMPv3-capable routers                                                                       |
| 224.0.0.2 and 224.0.0.102   | HSRPv1 and HSRPv2                                                                                |
| 224.0.0.107                 | EIGRPv6 (Enhanced Interior Gateway Routing Protocol version 6)                                   |
| 224.0.0.109                 | EIGRP IPv6 Router                                                                                |
| 224.0.0.110                 | EIGRP IPv6 Multicast                                                                             |
| 224.0.0.251                 | mDNS (Multicast DNS)                                                                             |
| 224.0.0.252                 | LLMNR (Link-Local Multicast Name Resolution)                                                     |
| 224.0.0.253                 | Teredo Tunneling                                                                                 |
| 224.0.0.254                 | SSDP (Simple Service Discovery Protocol)                                                         |

### Classless Inter-Domain Routing (CIDR)

Networks allocated using CIDR can be arbitrarily large and do not have to match exactly the traditional classes in the classful network (A, B, C), allowing flexibility in address allocation

The whole unicast range (any IP address with a first octet of 0 – 223) can be allocated in any size block

With CIDR, IP addresses are expressed in a format called "CIDR notation," which includes the IP address followed by a slash (/) and a number indicating the subnet mask length.

If you need 300 IP addresses … You get a /23

If you need 1000 IP addresses … You get a /22

If you need 25,000 IP addresses … You get a /17

### Subnetting

Subnets allow us to logically divide one allocated network into smaller sub-networks called Subnets

The single subnet design (flat design) is inadequate for the company's needs, leading to inefficient IP address usage and network congestion.

Security: Because the network is not segmented, you can not apply security policies adapted to individual segments. If one device is compromised, it can quickly affect the whole network.

Troubleshooting: Isolation of network faults is more challenging, especially in bigger flat networks, because there is no logical separation or hierarchy.

Address space utilization: In a large flat network, you can end up with a lot of wasted IP addresses. You cannot use addresses from this network anywhere else.

Scalability and speed: A flat network represents a single Layer 2 broadcast domain. If there is a large amount of broadcast traffic, this can impose considerable pressure on the available resources. A single broadcast domain typically should not include more than a couple of hundred devices.

Understanding subnetting is essential for any network engineer. Properly subnetting a network ensures that it can scale to meet growing demands while maintaining security and efficient resource use. This knowledge allows you to segment a network logically, enhancing overall performance and making it easier to manage and protect.

You can more easily apply network security measures at the interconnections between subnets than within a single large network.

Subnetting refers to the process of dividing the IP address space into subnets, where we use additional bits from the host part to be used for network part identification e.g. we are extending the network ID part

For example 172.16.0.0 - is class B by default, with mask of /16 - indicating that the first two octet are fixed and used for this one network leaving other two for host part

With subnetting we can extend the network part by using one octet from host part to create 255 (8bit) subnets - 172.16.(0-255).0 leaving the last octet to be used for hosts - 255, so each of the 255 networks can have 255 hosts

Subnetting increases routing efficiency, enhances the security of the network and reduces the size of the broadcast domain

By creating subnets, the class rules telling how many bits are part of the network ID and how many bits belong to the station ID no longer apply

Therefore, a new parameter must be introduced that defines the length of the network identifier (or the length of the subnet ID - subnet mask)

Subnet mask is a 32-bit number that describes which portion of an IPv4 address refers to the network ID and which part refers to the host ID.

The subnet mask is referred to as Prefix length in advanced routing terminology

The mask is a four byte number - The same length as the IP address. If the mask bit is equal to 1, then the bit of the IP address that is in the same position belongs to the network ID

Written in xxxx.xxxx.xxxx.xxxx (255.255.255.0 - /24)

![](<../.gitbook/assets/Unknown image (1048)>)

{% hint style="info" %}
Although there are 256 (2^8) possible combinations of bits in an 8-bit subnet mask, one of those combinations represents the network address, and another represents the broadcast address. Therefore, the usable host addresses within the subnet range from 1 to 254, 0 is for network and 255 for broadcast, totaling 256 addresses
{% endhint %}

When you count all bits set to 1 = 128+64+32+16+4+2+1 = 255 + 1 including the zero address itself X.X.X.0

{% hint style="info" %}
If we want to determine the NEID of one host IP we can use the logical multiplication & of the IP and subnet mask
{% endhint %}

**Wildcard mask:** Inverted subnet mask (used by ACL, OSPF, etc.). Written like `0.0.0.255`.

**Prefix:** `192.168.1.10` (predpona)

**Suffix:** `192.168.1.10` (pripona)

Calculating the Network Address

Given an IPv4 address and a subnet mask, you can calculate the network address by using the AND function between the binary representation of the IPv4 address and the binary representation of the subnet mask.

0 AND 0 = 0

1 AND 0 = 0

0 AND 1 = 0

1 AND 1 = 1

![](<../.gitbook/assets/Unknown image (1049)>)

Subnetting

To subnet a network address, you will borrow host bits and use them as subnet bits. You will use the subnet mask to indicate how many host bits have been borrowed. Bits must be borrowed consecutively, starting with the first host bit on the left. This approach introduces classless networks.

To implement subnets, follow this procedure:

Determine the IP address for your network as assigned by the registry authority or network administrator.

Based on your organizational and administrative structure, determine the number of subnets that are required for the network. Be sure to plan for growth

Based on the required number of subnets, determine the number of bits that you need to borrow from the host bits

Determine the binary and decimal value of the new subnet mask that results from borrowing bits from the host ID.

Apply the subnet mask to the network IP address to determine the subnets and the available host addresses. Also, determine the network and broadcast addresses for each subnet

Assign subnet addresses to all subnets. Assign host addresses to all devices that are connected to each subnet.

Each time that a bit is borrowed, the number of subnet addresses increases, and the number of host addresses that are available per subnet decreases. The algorithm that is used to compute the number of subnets and hosts uses powers of two. Therefore, borrowing one host bit enables you to create 21 = 2 subnets, borrowing 2 bits gives you 22 = 4 subnets, and so on.

As the following figure shows, you can also determine how many host addresses are available per subnet when you borrow a given number of bits. Just like on a network, two addresses are not available to be used as host addresses on a subnet; they are used for the address of the subnet itself (with all of the host bits set to 0) and the directed broadcast address on the subnet (with all of the host bits set to 1). The figure shows that borrowing 1 bit for subnetting the address in the example leaves 7 bits for hosts.

You can use a formula to calculate the number of host addresses that are available when a given number of host bits are borrowed: Number of hosts = 2h – 2 (where h is the number of host bits that are remaining after bits are borrowed)

![](<../.gitbook/assets/Unknown image (1050)>)

![](<../.gitbook/assets/Unknown image (1051)>)

![](<../.gitbook/assets/Unknown image (1052)>)

If a network address is subnetted, the first subnet that is obtained after subnetting the network address is called subnet zero, because all of the subnet bits are binary zero. To determine each subsequent subnet address, increase the subnet address by the bit value for the last bit that you borrowed.

8 bits are borrowed for subnetting the network address, 172.16.0.0/16. The first subnet address is 172.16.0.0/24; this is subnet zero. The last bit borrowed is the bit with the value of 1 in the third octet, so the next subnet address is 172.16.1.0/24.

Class B's 172.16.0.0/16 network address has been subnetted by borrowing two host bits in the following figure. The first subnet address is 172.16.0.0/18, the zero subnet. The last bit borrowed is the bit with the value of 64, so the next subnet address is 172.16.64.0/18.

![](<../.gitbook/assets/Unknown image (1053)>)

Here is one more example of subnetting the same /16 network address, this time borrowing 11 host bits for subnetting. The first subnet address is 172.16.0.0/27. The second subnet address is 172.16.0.32/27 because the last borrowed bit has a value of 32. Notice that this time, the last borrowed bit is in the fourth octet. Therefore, the increment of 32 (the value of the last borrowed bit) is first applied in the fourth octet.

Once all the possible subnet addresses in the fourth octet have been calculated in this manner, you move back into the third octet since you have borrowed bits from the third octet as well. You can use all the third octet values from 1 to 255 for your subnet addresses as well.

![](<../.gitbook/assets/Unknown image (1054)>)

### Cheatsheets

![](<../.gitbook/assets/Unknown image (1055)>)

![](<../.gitbook/assets/Unknown image (1056)>)

Explain the trick that the sum of all previous bits are always one less than the value in the next bit

Meaning that the sum of first 5bits is 31 and the sixth bit starts on 32 and so on...

Don't forget that the sum of all bits also include the zero, so in fact there are always sum+1 addresses

When all 5bits are flipped the sum is 31+1 including the zero address used for network id

![](<../.gitbook/assets/Unknown image (1057)>)

| Network ID | Host range | Broadcast address |
| ---------- | ---------- | ----------------- |

| #1: 192.168.57.0   | 192.168.57.1-192.168.57.62    | 192.168.57.63  |
| ------------------ | ----------------------------- | -------------- |
| #2: 192.168.57.64  | 192.168.57.65-192.168.57.126  | 192.168.57.127 |
| #3: 192.168.57.128 | 192.168.57.129-192.168.57.190 | 192.168.57.191 |
| #4: 192.168.57.192 | 192.168.57.193-192.168.57.254 | 192.168.57.255 |

![](<../.gitbook/assets/Unknown image (1058)>)

224

30

8

0, 32, 64, 96, 128, 160, 192, 224

193-222

191

![](<../.gitbook/assets/Unknown image (1059)>)

/26 255.255.255.192

62

4

0, 64,127,192

192.168.2.129 - 192.168.2.190

192.168.2.255

#### Fixed Length Subnet Masks (FLSM)

is not the same thing as Classful assignments. FLSM is simply using one size subnet mask on all the router interfaces, for all the routers in your topology.

It was a inefficient way to utilize the address space, but it was used because it saved bits for protocols such as RIP as it didn't had to include subnet mask in advertisements

![](<../.gitbook/assets/Unknown image (1060)>)

#### Variable Length Subnet Masks (VLSM)

VLSM allows you to use more than one subnet mask within a network to efficiently use IP addresses. Instead of using the same subnet mask for all subnets, you can use the most efficient subnet mask for each subnet. The most efficient subnet mask for a subnet is the mask that provides an appropriate number of host addresses for that individual subnet. For example, subnet 172.16.6.0 has only 19 hosts, so it does not need the 254 host addresses that the 24-bit mask allows. A 27-bit mask would provide 30 host addresses, which is much more appropriate for this subnet.

![](<../.gitbook/assets/Unknown image (1061)>)

In the next figure, the 172.16.0.0/16 network is first divided into subnetworks using a 24-bit subnet mask. However, one of the subnetworks in this range, 172.16.14.0/24, is further divided into smaller subnetworks using a 27-bit mask to accommodate the subnets that have 19 or 28 hosts. These smaller subnetworks range from 172.16.14.0/27 to 172.16.14.224/27. Then, one of these smaller subnets, 172.16.14.128/27, is further divided using a 30-bit mask, which creates subnets with only two hosts to be used on the WAN links. The subnets with the 30-bit mask range from 172.16.14.128/30 to 172.16.14.156/30.

{% hint style="info" %}
Also router interfaces must be configured with an IP address so that they know the networks they are connected to, and also to serve as an IP default gateway for the hosts in the subnet. The same applies to point-to-point connections between routers, where each interface must have an IP address to enable routing protocol communication and proper packet forwarding
{% endhint %}

If no IP address is configured, even if the interface is in the "up/up" state, the router will not attempt to send and receive IP packets on the interface. To attain proper operation, for every interface that a router should use for forwarding IPv4 packets, the router needs an IPv4 address.

![](<../.gitbook/assets/Unknown image (1062)>)

In addition to providing a solution to the problem of wasted IP addresses, VLSM has another important benefit: support for route summarization, which is also called route aggregation. The hierarchical addressing design of VLSM enables easier summarization of network addresses. Route summarization reduces the number of routes in routing tables by representing a range of network subnets in a single summary address. Smaller routing tables require less CPU time for routing lookups.

easiest way to assign the subnets is to assign the subnets with the largest number of hosts first.

![](<../.gitbook/assets/Unknown image (1063)>)

### IP address conflicts

The presence of multiple MAC addresses associated with the same IP address confuses the network devices, that leads to intermittent connectivity issues or complete loss of connectivity for both conflicting hosts. Network packets may be sent to the wrong device, causing data loss or corruption.

### Host forwarding logic (bitwise operations)

Bitwise operation is used for comparison of two binary values, in networking case it can be source IP portion and destination IP portion and manipulates the output based on used operator

**AND (&):** Sets each bit to 1 if both corresponding bits are 1.

**OR (|):** Sets each bit to 1 if either corresponding bit is 1.

**XOR (^):** Sets each bit to 1 if only one of the corresponding bits is 1.

**NOT (\~):** Inverts all the bits.

When a host wants to send and IP packet to the destination host he has to perform Bitwise XOR and AND operations to determine first if the destination it is trying to send the packet is in local subnet, so that he can send the packet directly, or remote, so he has to send it to the default gateway

So the host machine first perform the XOR operation to identify which bits are different between it's source and the destination address

If the compared bits have different values the XOR result for that bit will be a 1; otherwise 0.

After the XOR operation the AND operation is performed which sets the result to 1 if both compared bits have the value 1. If one, or both, have the bit-value set to 0, the AND-operator result will be 0

The XOR result is then compared with the subnet mask using an AND operator. If the AND result within the network portion is all-zeroes, the two IP-addresses belong to the same subnet.

The host will then proceed to examine it's arp cache, to find the destination host MAC address if the result of bitwise found that they are within the same subnet.

If there is not entry in the arp table for given destination IP, the host proceeds to send broadcast ARP request within his local subnet.

If there is entry it proceeds to encapsulate the data within the frame and send it directly to the destination

If the Bitwise result came to that the destination host is in the different subnet it proceeds to examine the arp cache again to determine the MAC address of its default gateway and proceeds to send it to the gateway. Router/Gateway then follows in described [Routing Process](https://onenote/#Routing\&section-id={6EC9C333-3E04-41C1-8C01-0E8D85C128A8}\&page-id={23330675-9420-47AE-8368-54C860A78BB9}\&object-id={52581BD7-7745-0862-3434-AF316314E545}&7C\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE/L3.one)

![](<../.gitbook/assets/Unknown image (1064)>)

{% hint style="info" %}
If a device with IP 10.2.2.1/24 connects to an L3 switch port in VLAN 10 (10.1.1.0/24), it won’t communicate because its IP doesn’t match the VLAN subnet. The device tries to send packets via 10.1.1.1, but it won’t ARP for it since the gateway is outside its subnet. The switch, seeing an unknown subnet, ignores the request. Without a valid L3 path, the device is isolated
{% endhint %}
