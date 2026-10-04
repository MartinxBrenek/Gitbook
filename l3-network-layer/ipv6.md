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
  actions:
    visible: true
  anchors:
    visible: true
---

# IPv6

### Overview

IPv4 was originally designed for militrary purpsoses and not as fundamental building block of the modern Internet, it's limited address space is not enough to cover the demand of the Internet

IP which is by nature connectionless L3 protocol, has been made into stateful connection-oriented protocol by NAT gateways, just for the sake of meeting the demands of an infinite number of devices trying to connect to the Internet.

NAT tried to conserve it's space, but caused additional overhead to the Internet services and applications, so it was necessary to create a new IP protocol with increased address space, improved security, support for hierarchical routing with the possibility to summarize internet prefixes, optimization for high-speed routing - to get rid of useless fields in the header and avoid fragmentation, offering features to ensure connection establishment by the protocol itself, replace broadcast with multicast, and implement features for easy transition from IPv4

This request for new IP protocol has been sent to IETF

### IPv4 exhaustion timeline

![](<../.gitbook/assets/Unknown image (1017)>)

The early IPv6 Internet was an experimental collection of IPv6 nodes and networks collectively known as the 6bone

The 6bone served as a testbed for address allocation methods and standards testing (3FFE::/16 prefix was used for the 6bone)

Some interim solution has been proposed

Last 16 /8 prefixes – class E (postpone exhaustion for 2 or 3 year)

Inactive allocated addresses can be withdrawn from the users (postpone exhaustion by 5years)

Administrative complexity

Already hardcoded for their purpose in most systems

More strict conditions for address assignment

Everything pointed to the need to create a new protocol with remarkably broader address space.

### Internet Protocol version 6 (IPv6)

**IPv6 address** is a string of 128-bits/16byte represented as 32 hexadecimal digits grouped in 8 groups (hextets) separated by colons, where each group consists of 4 hexadecimal numbers

Each hexadecimal digit is represented by the 4 bits, thus the length of each hextet is 16bits. Format: X:X:X:X:X:X:X:X, where x is a 16-bit hexadecimal field

IPv6 address space provides 2^128 of addresses, which is 340 undecilion (sextilionu) of addresses

![](<../.gitbook/assets/Unknown image (1018)>)

Fomat

![Example of an IPv6 address](<../.gitbook/assets/Unknown image (1019)>)

Standard prefix for a host has been set at /64\
18,446,744,073,709,600,000 IPv6 addresses

Let’s attempt to exhaust all of the available addresses\
We allocate 10,000,000 addresses per second (31,536,000 seconds per year)\
10,000,000 x 31,536,000 = 315,360,000,000,000 addresses per year

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

Assume we allocate a /48 to a Data Centre Network\
A /48 contains 65,536 /64’s\
It takes 58,494 years to deplete a /64 at 10M addresses per second\
65,536 x 58,494 = 3,833,478,626 years\
3.8B years to deplete a /48 at 10M addresses per second !!!

### IPv6 address notation rules

Addresses are case-insensitive, but it is recommended (RFC5952) to be written in lowercase due to the better readability

However when you are configuring it on the network device or end hosts, it can be written in either lower or upper case format, the lowercase recommendation is only for development of applications, where employing regex can have an impact on the interpretation

Since the zero is a fairly common value, two options are offered for shortening the notation.

For address 2001:0DB8:0000:0000:FFFF:0000:0000:0ADC

Leading zeros can be omitted and sections with 0 can be represented by a single 0

2001:DB8:0:0:FFFF:0:0:ADC

Successive fields of 0 can be represented as double colon ::

“::” (double colon) can be used only once

If several groups have the same maximum length,”::” is used for the first occurrence

“::” cannot be used for single “:0000:” group

2001:DB8::FFFF:0:0:ADC is the only correct representation

### IPv6 header

It has a fixed length of 40 bytes, although an IPv6 address is four times longer than an IPv4 address.

80% (32 bytes) of the v6 header is taken up by the source and destination address, which is the most necessary information to send a packet.

There are a few other fields that take up 8 bytes

There is no header checksum in IPv6 header, since the designers considered sufficient that the whole-packet link layer checksumming is provided already by data-link layer like Ehernet, combined with the checksums in upper layer protocols such as TCP and UDP

Thus, IPv6 routers are relieved of the task of recomputing the checksum whenever the packet changes, for instance by the lowering of the Hop limit counter on every hop.

These and other enhancements such as enforcing PMTUD improve hardware-based processing, which provides scalability of the forwarding rate for the next generation of high-speed networks

IPv6 iam to restore the original end-to-end design of the internet by enabling direct communication between hosts without resource-consuming proxy services like NAT or port forwarding

The protocol section from IPv4 has been renamed to next header because of the function called header chaining

![](<../.gitbook/assets/Unknown image (1020)>)

| Version (4 bits)                    | Identifies the IP version; it’s always 6 for IPv6.                                                                                                                                                                                          |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Traffic Class (8 bits)              | Specifies the type of service and priority for the packet.                                                                                                                                                                                  |
| Flow Label (20 bits)                | Used to identify packets belonging to the same flow or stream. With this label, a router does not have to open the transport layer segment to identify the flow; it finds the information in the IP packet header.                          |
| Payload Length (16 bits)            | Indicates the length of the IPv6 payload, excluding headers                                                                                                                                                                                 |
| Next Header (8 bits)                | Specifies the type of the next header or the upper-layer protocol such as TCP or UDP Next header field contain either beginning of real upper layer protocol. Or, in case of header chaining, information about following extension header. |
| Hop Limit (8 bits)                  | Similar to Time to Live (TTL) in IPv4, it sets an upper limit on the number of hops the packet can traverse.                                                                                                                                |
| Source IPv6 Address (128 bits)      | The IPv6 address of the source.                                                                                                                                                                                                             |
| Destination IPv6 Address (128 bits) | The IPv6 address of the destination.                                                                                                                                                                                                        |

#### Extension headers

The IPv6 option mechanism is improved over the IPv4 option mechanism. In a packet, IPv6 options are placed in extension headers between the IPv6 header and the transport-layer header.

This feature has greatly improved the performance of routers containing options. An IPv4 router must explore all options in the presence of any option

In the future, new header types can be added as new requirements of networking and applications evolve

If special handling is required by either intermediate routers or the destination, the sending host adds one or more extension headers

Each extension header must fall on a 64-bit (8-byte) boundary. Extension headers of a fixed size must be an integral multiple of 8 bytes. Extension headers of variable size contain a Header Extension Length field and must use padding as needed to ensure that their size is an integral multiple of 8 bytes.

Next Header field in the IPv6 header is a pointer, which indicates the type of the extension header that comes after the current header

EH can be chained, the main limitation is given by fragmentation – all EHs must be present in the first fragment

If an extension header contains an unrecognized or improper value of the Next Header field, the node discards the packet and sends an ICMP Parameter Problem-Unrecognized Next Header Type Encountered message to the packet source

Each extension header should occur at most once, except for the Destination Options header, which should occur at most twice (once before a Routing header and once before the upper-layer header)

{% hint style="info" %}
Extension headers are used in very specific scenarios or deployment, most of the extension headers are prohibited from the use on the public infrastructures like internet
{% endhint %}

Examle use: SRv6 or Authentication for OSPFv3

![](<../.gitbook/assets/Unknown image (1021)>)

![](<../.gitbook/assets/Unknown image (1022)>)

RFC2460 recommended extension header chain sequence

<table><thead><tr><th width="87">Order</th><th width="206.5999755859375">Header Type</th><th width="82.400146484375">Code</th><th></th></tr></thead><tbody><tr><td>1</td><td>Basic IPv6 Header</td><td>-</td><td></td></tr><tr><td>2</td><td>Hop-by-Hop Options</td><td>0</td><td>Must be placed directly under basic header, it contains options that must be examined by all nodes along the packet’s delivery path. If the transit router sees anything else than 0, it knows it can end up with the datagram’s analysis Types 0 &#x26; 1 option are used for padding with 1 (PAD1) or N bytes (PADN) to ensure 8-byte boundaries Type 5 Router alert used in RSVP when there is a need to reserve bandwidth across the network Because Payload Length field in IPv6 header is 2 bytes, IPv6 does not support natively transport of packets longer than 65535 bytes. To solve this limitation, Type 194 was defined to support transport of Jumbograms, packets with length up to 2^32 bytes. It defined 32bit field to specify the actual payload length and was aimed to the infrastructures that can handle such packets - which is not the case for real-world networks as they still support MTU up to 1500Byte, thus jumbograms were removed from the last IPv6 Node Requirements RFC 8504. Because Hop-by-Hop Options headers must be processed by each router encountered, they have the potential to overburden the Internet routing system. As a result, RFC 6564 [ <a href="https://tools.ietf.org/html/rfc6564.html">https://tools.ietf.org/html/rfc6564.html</a>] strongly discourages new Hop-by-Hop Option headers, unless examination at every hop is essential.</td></tr><tr><td>3</td><td>Destination Options header (for intermediate destinations when the Routing header is present)</td><td>60</td><td>Similar structure to Hop-by-Hop options, containing optional information for specific intermediary destination node. Type 201 Home Address Option is used to indicate the home address of the mobile node. The home address is an address assigned to the mobile node when it is attached to the home link and through which the mobile node is always reachable, regardless of its location on an IPv6 network. Type 15 Performance and Diagnostic Metrics (PDM) prepends sequence and timing information for a packet measurement</td></tr><tr><td>4</td><td>Routing Header (Segment/source routing)</td><td>43</td><td>Specifies one or more nodes through which the datagram must pass before delivery to the destination address Type 0 was solely to perform the source-based routing, specifying nodes it must pass through, however since this mechanism is exploitable for attacks seeking to overwhelm transmission routes. So a node should ignore this type Strict – packet goes hop by hop on defined path Loose – packet must cross specified nodes but take loose path through Type 2 was defined for mobility Routing Header concept has been utilized to define a new Segment Routing Header (SRH) with code 143, which is specifically leveraged by the SRv6 routing protocol to enable segment routing functionality in IPv6 networks</td></tr><tr><td>5</td><td>Fragment Header</td><td>44</td><td>Leveraged for IPv6 fragmentation and reassembly services when a node must send a packet larger than the path MTU The Fragment Offset, More Fragments flag, and Identification fields are used in the same way as the corresponding fields in the IPv4 header</td></tr><tr><td>6</td><td>Authentication Header (IPsec)</td><td>51</td><td>Protects against packet tampering and spoofing by authenticating the sender and verifying the integrity of the packet. The AH computes a cryptographic hash (HMAC) over parts of the IP packet and includes this hash in the header. The receiving system recalculates the hash and compares it to the one in the AH to verify that the packet has not been modified. AH protects the payload and parts of the IP header. However, fields that change during transit, like the Hop Limit, are excluded from the hash computation Provides data authentication, integrity and optional anti-replay protection AH does not encrypt the data, so the packet’s contents remain visible to third parties. It protects only against tampering and spoofing. Leveraged by protocols such as OSPFv3</td></tr><tr><td>7</td><td>Encapsulating Security Payload (ESP)</td><td>50</td><td>Provides encryption for confidentiality, with optional integrity and authentication. Also provides integrity and authentication, often used alongside the Authentication Header in IPsec. In tunnel mode, ESP also protects the original IP header, hiding the identities of the sender and receiver Leveraged by protocols such as OSPFv3</td></tr><tr><td>8</td><td>Destination Options header (for the final destination node)</td><td>60</td><td>processed by the final destination</td></tr><tr><td>9</td><td>Mobility Header</td><td>135</td><td>Supports mobile IPv6 functionality.</td></tr><tr><td>x</td><td>No next header</td><td>59</td><td></td></tr><tr><td>x</td><td>IPv4 header</td><td>4</td><td></td></tr><tr><td>UpperL</td><td>TCP</td><td>6</td><td></td></tr><tr><td>UpperL</td><td>UDP</td><td>17</td><td></td></tr><tr><td>UpperL</td><td>ICMPv6</td><td>58</td><td></td></tr><tr><td></td><td></td><td></td><td></td></tr></tbody></table>

### IPv6 addressing scheme

The addressing architecture of IPv6 is defined in RFC 4291 and allows three different types of transmission: unicast, [Anycast](onenote:#L3%20-%20IPv4\&section-id={6EC9C333-3E04-41C1-8C01-0E8D85C128A8}\&page-id={0BE7C4A5-25A0-4FB3-9FBB-F8118301B662}\&object-id={8D629352-0E5D-0836-0518-5BFAA16EE1B2}&7D\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE/L3.one) and multicast

Unicast addresses are divided into scopes:

Link-Local adresses

Global addresses

Anycast adresses

Special addresses

{% hint style="info" %}
The ambiguity of site local unicast addresses in an organization adds complexity and difficulty for applications, routers and network managers. Consequently, Site-Local address have been deprecated and Unique Local addresses have superseded them with this challenge in mind. However site-local multicast addressed are still used for reserved services
{% endhint %}

IPv6 does not define any broadcast address, instead it is replaced by its efficient multicast equivalent special link-local multicast reserved addresses (for example: FF02::1 (all nodes) and FF02::2 (all routers))

![](<../.gitbook/assets/Unknown image (1023)>)

Another difference between IPv6 and IPv4 is that each interface can be assigned with several addresses or even with several different types of addresses

Each address is used for either local communication between devices on a local segment or to communicate globally outside of our segment

Each IPv6 interface of a host requires a link-local address, additional unicast can be configured either manually or automatically, and should subscribe e.g listen to all-nodes multicast address FF02::1, solicited-node multicast for each of its unicast or anycast addresses

IPv6 host interface requires the following IPv6 addresses for proper operation:

Link-local address

All-nodes multicast address

Solicited-node multicast address for each unicast and anycast addresses

Optional addresses

Any additional unicast and anycast addresses configured, either automatically or manually

Multicast addresses of all other groups to which the host belongs

![](<../.gitbook/assets/Unknown image (1024)>)

IPv6 address is designed as two parts, where the first part is /64 prefix and second part /64 is reserved for Interface identifier (ID), which is used to distinguish individual network interfaces within a subnet to make the IPv6 address unique on the link. This makes the IPv6 relatively wasteful, but it ensures the initial requirements for autoconfiguration and easy transition from IPv4

The interface ID part of the IPv6 address is generated either manually or automatically with EUI-64 or newer and more secure randomized way

![](<../.gitbook/assets/Unknown image (1025)>)

### IPv6 address lifecycle

IPv6 Addresses have lifetime:

**Valid**: address valid and usable for communication, regardless of preferred status. Once expires, address marked invalid and no longer used. Devices must stop using once invalid.

**Preferred**: address preferred for new connections. Once expires address becomes deprecated.

**Deprecated**: used only to maintain existing connections

Lifetimes can differ between router-advertised values and host-applied defaults.

Routers typically advertise longer lifetimes (e.g., 30 days), while hosts like Windows and Linux default to around 7 days preferred and 30 days valid, unless overridden by the Router Advertisement.

![](<../.gitbook/assets/Unknown image (1026)>)

### Interface identifiers

#### EUI-64 (legacy approach)

According to the original specification of SLAAC, the interface identifier was supposed to contain a modified EUI-64, which comes out from the link address (MAC) to generate a unique IPv6 address. It works by inserting the FFFE value into the middle of the MAC address to fill missing 16bits to 64 and flips 7th (UL) bit from default 0 to 1 (locally administered bit)

Prefix: 20001:db8:18::/64 >>>>>>>>> R1's interface MAC: aabb.cc00.0300 >>>>>>>>> Global unicast address: 2001:db8:18:a8bb:ccff:fe00:0300/64

{% hint style="info" %}
This approach has been abandoned due to security risk problems, since it exposes the device type (manufacturer) and can be easily tracked through internet
{% endhint %}

![](<../.gitbook/assets/Unknown image (1027)>)

When SLAAC is used in a network, interface on the end hosts will generate two IPv6 addresses for each assigned prefix:

**Permanent/Stable Address/Stable Random Interface Identifier IID** Generated using the RFC 7217 (Hash) method. This address is stable per network but is privacy-friendly.

It is more sophisticated and secure way that uses periodic algorithm to generate interface ID that is stored in host's cache as MD5 hash of a 64-bit value

It uses a random function to generate IID that is similar to the temporary. But a secured address will not expire or change unless the network prefix portion of the address changes (as when a host moves from one network segment to another) - tagged as secured. It is used mainly for incoming connections

Security: To enhance security and privacy, RFC 7721 recommends avoiding static, IEEE-identifier-based IIDs. Instead, it suggests using temporary or changing IIDs. These temporary IIDs vary over time and reduce the ability for attackers to track a device or correlate its activities across different networks and sessions

**Temporary Address:** Generated using the RFC 8981 (Random) method. This address is regenerated periodically and is used for all outgoing traffic by default in operating systems

At every reboot, or IPv6 stack on/off, or when the Preferred-Lifetime expires this temporary address is re-generated using a Random Interface Identifier

**Random IID** mitigates the risk of someone tracking the user by associating the global IPv6 address to the physical equipment

In practise, the temporary is used as a primary in outbound connection to the internet. If you remove the temporary, the permanent (public) will be used

In some cases, you will see multiple Temporary IPv6 Addresses at a time (could be hundreds), which can indicate that the Maximum Preferred Lifetime didn't expired yet or the address is still used for an open outbound connection. In this case, another Temporary IPv6 address is created, but the old one is not deleted until all opened connections are closed

{% hint style="info" %}
This regular change of IPv6 addresses on the hosts results in more difficult management and enforcement of security rules on the hosts for a large scale enterprise networks
{% endhint %}

Solution: Use DHCPv6 and lock admin rights to the address configuration and disable temporary address option on the hosts or employ Group policies

| netsh interface ipv6 show privacy                                            | In windows you can check if the temporary addres is used                                                                                                                                               |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| netsh interface ipv6> set privacy \[enabled\|disabled]                       | to disable or enable temporary address If you want to remove the current temporary address run the disabled and netsh interface ipv6 delete addresses "Your Network Interface Name"then perform reboot |
| netsh interface ipv6> set global randomizeidentifiers = \[enabled\|disabled] |                                                                                                                                                                                                        |
| netsh interface ipv6 show global                                             | to check, whether Random IID (RandomizeIdentifiers) is used instead of EUI-64                                                                                                                          |

#### Happy Eyeballs (dual-stack host behavior)

Technique that makes dual stack applications faster and more reliable for users

It works by trying to connect using both IPv4 and IPv6 at the same time and choosing the first one that succeeds

This way, it avoids the problems caused by broken or slow IPv6 connections usually improving the user experience when accessing websites or services that use IPv6

![Modified EUI 64 IID - Part 7](<../.gitbook/assets/Unknown image (1028)>)

![](<../.gitbook/assets/Unknown image (1099)>)

### Global Unicast Addressing (GUA) 2000::/3

**Global Unicast Addressing (GUA) 2000::/3** is public range that is advertised and routed on internet. IANA and IRTF - the managers of the IPv6 address space allocated a range 2000::/3 to 3fff::/3 to the IANA

IANA defines that the left most bits set to 001 as GUA that can be distributed to the RIRs

A small portion of the addresses starting with 000 and 111 are allocated for special types. All other possible addresses are reserved for future use and are currently not being allocated.

In GUA the first 48bits [are managed by IANA](https://www.iana.org/assignments/ipv6-unicast-address-assignments/ipv6-unicast-address-assignments.xhtml), that assigns it to the RIRs, which then assign it further for the individual ISP's

The next 16bits are dedicated for subnet ID within ISP network and last 64 bits is generated interface ID of each interface

![IANA’s Allocation of IPv6 Address Space](<../.gitbook/assets/Unknown image (1100)>)

![](<../.gitbook/assets/Unknown image (1101)>)

#### IPv6 allocation to organizations (RIR → ISP → customer)

All registries generally share the same allocation policy to discourage ISPs from attempting to obtain address space from a favorite registry. Address space is not sold, but fees are charged for services and these fees can vary by region.

IANA allocates addresses to the registries from the 2000::/3 block.

Each registry should receive a /12 from IANA.

The registry allocates an initial /32 prefix to a new IPv6 ISP. A larger prefix can be allocated if justified by the ISP.

The ISP allocates a /48 or a /56 prefix (out of the /32) to each customer, depending on the size of the customer network and its needs. Actual allocations may vary depending upon the requirements of the requesting organization.

Therefore, 16 bits (64 to 48) are available to subnet the site, for a maximum of 65,536 LANs per site. A site should have an address plan before beginning to allocate its /48 space

As in the current IPv4 Internet, a small ISP can be connected to a larger ISP that is connected to an even larger ISP. The number of intermediate levels is not fixed. IPv6 does not change this behavior. However, the current IPv6 address allocation forces the aggregation of routing entries, which should limit the number of entries in the global routing table. This situation may change somewhat with the introduction of IPv6 provider-independent addressing.

![AfriNlC 112 - 123 ISP LIR NIR 119 -132 End users 130 - 160 APNIC 112 -123 LIR NIR 119 -132 End users 130 - 160 IANA ARIN 112 -123 LIR NIR 119 - 132 End users /30 - 160 LACNIC 112 - 123 IS p LIR NIR 119 - 132 End users 130 - 160 RIPE NCC 112 - 123 ISP LIR NIR 119 -132 End users 130 - 160](<../.gitbook/assets/Unknown image (1102)>)

**GUA block allocations to RIRs**

| IPv6 address block | Regional registry |
| ------------------ | ----------------- |
| 2001:/16           | Various           |
| 2400:0000::/12     | APNIC             |
| 2600:0000::/12     | ARIN              |
| 2800:0000::/12     | LACNIC            |
| 2A00:0000::/12     | RIPE NCC          |
| 2C00:0000::/12     | AfriNIC           |

IANA, the 2000::/3 block administrator allocates blocks from /12 to /23 to RIRs, they allocates blocks of sizes from /19 to /32 to LIRs/ISPs and they allocate prefixes from range /30 to /60

to customers or enterprises (commonly /48)

LIR assigned predominantly /48 blocks to the customers. Between LIRs, only aggregated prefixes /32 are exchanged

![](<../.gitbook/assets/Unknown image (1103)>)

### Global Internet Aggregation in IPv6

ISP can aggregate all of its customer prefixes into a single prefix and announce this one prefix to the IPv6 Internet

This aggregation promotes efficient and scalable routing

**Provider Aggregatable (PA)** IPv6 addresses are assigned by ISPs to their customers. These addresses are hierarchical and are aggregated by ISPs to reduce the size of routing tables and conserve IPv6 space. PA addresses are suitable for most smaller organizations with single ISP, providing sufficient address space while easily managed by ISPs. This can be an issue for multihomed companies, that would obtain IPv6 prefix from both of their ISP's

**Provider Independent (PI)** addresses are the solution for multihomed customers with their own AS. Prefix 2001:678::/29 is allocated and assigned by the regional Internet registries as /48 blocks to multihomed organizations that want to have their unique prefix. Customer are not limited to fall into their ISP aggregated prefix and associated renumbering issues.

This creates IPv6 De-Aggregation issue, since multiple small prefixes are advertised instead of a single aggregate, leads to inefficiency in IPv6 routing, where routing tables will grow excessively large. PI prefixes are short term solution for multihomed customer.

If you change the ISP or your prefix, you will have to redo the rules on the firewalls, NATing rules, etc...

For a backup connection, it is possible to advertise the prefix from ISP1 and ISP2 to the network. If ISP1 is not working, ISP2 addresses are used. It's not optimal, but it works.

Long-term solutions:

Multihoming solutions using the protocol stack: SCTP, SHIM6, HIP, or using LISP as the routing protocol

**Site Multihoming by IPv6 Intermediation (SHIM6)** is an IPv6-based site multihoming solution that inserts a new sublayer (shim) into the IP stack of end-system hosts. (Still in draft)

Shim6 operates at the host level. It will enable hosts on multihomed sites to use a set of provider-assigned IP address prefixes and switch between them without upsetting transport protocols or applications

The primary downside is that inbound traffic engineering cannot be managed by the multi-homed destination, putting all the control in the hands of the source host. This presents issues, particularly for larger commercial networks that need fine control over routing preferences.

Many ISPs are expected to avoid Shim6 and stick to traditional routing practices

### IPv6 summarization

As in IPv4 networks - in IPv6 summary routes are used to improve network stability and efficiency, the summary is found according to bit that is identical for all included prefixes. Each IPv6 value is 4bit long as it is written in hex format

![](<../.gitbook/assets/Unknown image (1104)>)

### Unique Local Addressing (ULA) FC00::/7

**Unique Local Addressing (ULA) FC00::/7** is private address space (similar to RFC 1918) filtered on the internet, however since it has globally unique prefix and it is accidentally leaked outside of the organization, there will be no conflict with other IPv6 global prefixes.

It allows combining or privately interconnecting sites without creating any address conflicts or requiring a renumbering of interfaces that use these prefixes

Used by organizations or service providers that prefer the concept of private address space to improve security

They are used in scenario, where u have sensitive traffic that you send to a ULA address in your network, the source address should also be a ULA address

And since this prefix is dropped by the internet/WAN routers as advised in RFC4193, it will prevent the traffic from being routed to the public Internet, protecting sensitive internal traffic

The reserved range for ULA is FC00::/7, and according to RFC4193 the 8th bit must be set to 1 to indicate that the address is locally assigned, which divides the ULA block into two subblocks:

Subblock FC00::/8 (1111 1100) This block was reserved with the intent of being globally managed by IEEE (e.g., by a registry or centralized authority), but no such system has been implemented

Subblock FD00::/8 (1111 1101) available for private use

![](<../.gitbook/assets/Unknown image (1105)>)

40-bit: Gobal ID composed of randomly generated string (RFC 4193 offers a suggestion for generating the random identifier to obtain a minimum-quality result if the user does not have access to a good source of random numbers)

16-bit: Subnet ID to identify the subnet within the site

64-bit interface identifier

ULA are "internal use" addresses within your infrastructure

The 40 bits marked as "X" should be unique worldwide, you can register them in the unofficial registry, for example.

They have a lower priority in dual-stack networks than IPv4 and globally routable IPv6

ULA can coexist with GUA, useful for cases where you need a stable internal address of a service, when the GUA can "disappear"

### Link-local addressing (LLA) FE80::/10

**Link-local addressing (LLA) FE80::/10** is auto-generated stable address created whenever IPv6 is enabled on an interface with a combination of reserved link-local prefix FE80::/10 and the EUI-64 or Radom IID

It has limited scope, allowing to communicate only on the local network segment, and is not intended for communication outside of network

Used for autoconfiguration, neighbor & router discovery and by routing protocols

In contrast to IPv4, this address is available even when no address is configured or DHCP is not available

{% hint style="info" %}
Configuration of link-local as next hop for static route is rejected by cisco IOS, as it is not suitable for routing traffic beyond the local network
{% endhint %}

{% hint style="info" %}
Configuring link-local or pinging to link-local address requires to specify the interface, since the link-local addresses belong to one reserved network, and each link-local address can can be used on multiple interfaces simultaneously, since their scope for communication stays on the link
{% endhint %}

![](<../.gitbook/assets/Unknown image (1106)>)

[Using Only Link-Local Addressing inside an IPv6 Network](https://datatracker.ietf.org/doc/html/rfc7404) have its advantages and disadvantages

#### Advantages

simpler address management; lower configuration complexity; routing tables do not need to carry link addressing and can therefore be significantly smaller, which can decrease routing convergence times. An interface of a router is also not reachable beyond the link boundaries, therefore reducing the attack surface

Disadvantages

If an interface doesn't have a routable address, it can only be pinged from a node on the same link

Therefore, it is not possible to ping a specific link interface remotely. A possible workaround is to ping the loopback address of a router instead

LLAs have usually been based on 64-bit Extended Unique Identifiers (EUI-64); hence, they change when the MAC address is changed

This could pose a problem in a case where the routing neighbor must be configured explicitly (e.g., BGP) and a line card needs to be physically replaced, hence changing the EUI-64 LLA and breaking the routing neighborship. LLAs can be statically configured, which can mitigate this issue and set up static routing aswell

However, there is no benefit of static LLA, since the address types with greater scope such as ULA or GUA provide more benefits

### Multicast addressing FF00::/8

IPv6 uses multicast addressing (FF00::/8) for one-to-many communication, such as video streaming, control protocols like SLAAC, OSPF, VRRP, and DHCP.

Serves for traditional purposes as in IPv4 – for protocols that leverage well-known reserved multicast address

A multicast address represents a group of interfaces identified by a single multicast address, known as a multicast group. Packets sent to a multicast group always have a unicast source address, and a multicast address itself cannot be used as a source.

The main advantage over IPv4 is that IPv6 completely eliminates broadcast, replacing it with multicast. This approach is more efficient and scalable, as it allows communication only with devices that have explicitly joined a multicast group.

By removing the need for broadcast, IPv6 saves the single IP address that was previously reserved in each IPv4 subnet for broadcast (the “last” IP)

The main advantage over IPv4 is that IPv6 completely eliminates broadcast, replacing it with multicast. This approach is more efficient and scalable, as it allows communication only with devices that have explicitly joined a multicast group.

By removing the need for broadcast, IPv6 saves the single IP address that was previously reserved in each IPv4 subnet for broadcast (the “last” IP). Although address conservation is less critical in IPv6 due to its vast address space, this design is still more elegant and efficient.

Broadcast traffic in IPv4 was sent to all devices within a subnet, regardless of whether they needed the information, which could overwhelm devices, waste bandwidth, and expose networks to spoofing or flooding attacks. In large networks, excessive broadcast traffic could also degrade performance

In IPv6, the FF02::1 address serves as the “all-nodes multicast” address, which functionally replaces IPv4 broadcast but provides targeted delivery — only devices that are part of the subnet and subscribed to the group receive the packets.

This mechanism reduces unnecessary traffic, minimizes congestion, and improves security. Moreover, multicast group subscriptions can be managed dynamically, unlike IPv4 broadcast, which is static and indiscriminate. The FF02::1 group is commonly used by NDP, ICMPv6, and diagnostic tools.

The multicast addresses begin with eight ones followed by flags, scope and the rest 112 bits is used for the Group ID identifying group of receivers

After the first 8 bits (which define a multicast address), there are 4 bits each for flags and scopes

IPv6 address allows RP address in multicast Group address

No need to configure RP address on multicast routers

Router will get RP mapping directly from PIMv6, MLD or data plane

![](<../.gitbook/assets/Unknown image (1107)>)

![](<../.gitbook/assets/Unknown image (1108)>)

Group ID identify the multicast group within the given scope

Flags is 4bit field, where high order 9th bit is set to 0 and subsequent 10th bit indicates if R flag is applied, 11th bit if P flag is applied, and 12th bit if T flag is applied

The most important is only T to indicate whether it is temporary or well-known reserved multicast address indicated by zero

R (Rendezvous) flag indicates whether the multicast address contains an embedded rendezvous point address. Bit set to 1 = RP to 0 = no RP included in this multicast address

P (Prefix) flag indicates whether the multicast address is based on a unicast address or not. If the value of this flag is set to 0, the multicast address is not assigned based on the unicast address. If the value of this flag is set to 1, the multicast address is based on the network prefix of the unicast subnet address

T (Transient) flag indicates whether the multicast address is transient (bit to 1) or is permanent well-known address assigned by IANA (bit to 0)

Permanent (0): These addresses, known as predefined multicast addresses, are assigned by IANA and include both well-known and solicited multicast.

Nonpermanent (1): These are "transient" or "dynamically" assigned multicast addresses. Multicast applications assign them.

Flags (Binary) Prefix Description

0000 FF00::/12 Permanently assigned 112-bit group ID for well-known addresses (FF02::2 (All Routers) or FF02::6 (OSPF DR Routers))

0001 FF10::/12 Temporarily assigned 112-bit group ID

0011 FF30::/12 Temporarily assigned unicast prefix-based multicast address

0111 FF70::/12 Temporarily assigned unicast prefix-based multicast address with rendezvous point interface ID

![multicast flag](<../.gitbook/assets/Unknown image (1109)>)

Scope bits define the scope of the multicast group. Routers must not forward any multicast packets beyond the scope that is indicated by the Scope field in the destination multicast address.

Nodes must not originate a packet to a multicast address whose scope field contains the reserved value 0. If such a packet is received, it must be silently dropped. Nodes should not originate a packet to a multicast address whose scope field contains the reserved value F. If such a packet is sent or received, it must be treated the same as packets that are destined to a global (scope E) multicast address.

| Value | Scope                    |                                                                                                                                                                                                                                                                                                               |
| ----- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0     | Reserved                 |                                                                                                                                                                                                                                                                                                               |
| 1     | Interface-local scope    | spans only a single interface on a node and is useful only for loopback transmission of multicast. Packets with this destination address may not be sent over any network link but must remain within the current node; this is the multicast equivalent of the unicast loopback address.                     |
| 2     | Link-local scope         | corresponds to a special scope of IPv4 multicast – 224.0.0.0/24 - used on the directly connected segment. This is the only one scope with TTL set (=1)                                                                                                                                                        |
| 3     | Reserved                 |                                                                                                                                                                                                                                                                                                               |
| 4     | Admin-local scope        | is the smallest scope that must be administratively configured; for example, it is not automatically derived from physical connectivity or another, nonmulticast-related configuration                                                                                                                        |
| 5     | Site-local scope         | is intended to span a single site                                                                                                                                                                                                                                                                             |
| 6     | unassigned               | are available for administrators to define additional multicast regions.                                                                                                                                                                                                                                      |
| 7     | unassigned               | are available for administrators to define additional multicast regions.                                                                                                                                                                                                                                      |
| 8     | Organization-local scope | is intended to span multiple sites belonging to a single organization                                                                                                                                                                                                                                         |
| 9-D   | unassigned               | are available for administrators to define additional multicast regions.                                                                                                                                                                                                                                      |
| E     | Global scope             | are eligible to be routed over the public Internet. FF1E:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx "FF" specifies that this is a multicast address "1" specifies a temporary address (not a reserved, permanent, IANA-registered multicast address). "E" specifies a global scope. "X" can be any hexadecimal value. |
| F     | Reserved                 |                                                                                                                                                                                                                                                                                                               |

### Link-local reserved Multicast

FF0X:: (X defines the scope)

Although site-local unicast addresses have been deprecated (RFC 3879), the concept of site-local scope still applies to IPv6 multicast addresses.

| ff02::1               | all nodes are subscribed and listen to this address | ff02::5 | all OSPF routers  | ff02::f   | Universal Plug and Play devices |
| --------------------- | --------------------------------------------------- | ------- | ----------------- | --------- | ------------------------------- |
| ff02::2               | all routers listen to this                          | ff02::6 | all OSPF DRs      | ff02::fb  | multicast DNS IPv6              |
| ff02::12              | VRRP                                                | ff02::9 | all RIPng routers | ff02::101 | NTP                             |
| ff02::1:2             | all DHCP agents \[servers or relays]                | ff02::a | all EIGRP routers | ff05::101 | all NTP server (site)           |
| FF02::1:FFXX:XXXX/104 | Solicited-Node link-local                           | ff02::d | all PIM routers   | FF02::66  | HSRPv2                          |

#### Solicited-node multicast address

Multicast address with a link-local scope used for autoconfiguration and discovery mechanisms

Upon configuring an IPv6 address, every node joins respective Solicited-node multicast address that is automatically created by inserting last 6 hexadecimal digits of the the generate IPv6 address into the last 24bits (6 hexadecimal digits) of the reserved prefix FF02::1:FFxx:xxxx/104 - where xx:xxxx are the last 6 hexadecimal values in the IPv6 unicast address

![](<../.gitbook/assets/Unknown image (1110)>)

### IPv6 over Ethernet

IPv6 has a reserved Ethernet protocol ID (like IPv4)

![](<../.gitbook/assets/Unknown image (1111)>)

**Multicast Mapping over Ethernet**

Also the multicast address must have it's own link-layer address to be addressable withint the local segment - The reserved IPv6 multicast prefix for Ethernet is 33-33 (33:33:00:00:00:00), where the low order bits are filled with last 32 bits from a IPv6 address

IPv6 multicast MACs are not dynamically assigned according to the IPv6 multicast address.

With this mapping design, it is obvious that IPv6 addresses that share the common last 32 bits will map to the same MAC address, which reduces the number of multicast addresses that a node must join.

If an IPv6 solicited-node multicast address is known, then the associated MAC address is known, formed by concatenating the last 32 bits of the IPv6 solicited-node multicast address to **33:33**

Understand that the resulting MAC address is a virtual MAC address: It is not burned into any Ethernet card. Depending on the IPv6 unicast address, which determines the IPv6 solicited-node multicast address, an Ethernet card may be instructed to listen to any of the 2^24 possible virtual MAC addresses that begin with 33-33-ff.

In IPv6, Ethernet cards often listen to multiple virtual multicast MAC addresses and their own burned-in unicast MAC addresses

A node is required to compute and join (on the appropriate interface) the associated solicited-node multicast addresses for all unicast and anycast addresses that have been configured for the node interfaces (manually or automatically).

![](<../.gitbook/assets/Unknown image (1112)>)

This example depicts how the IPv6 got rid of the need for broadcast address or any equivivalent to send packets to all devices within the network which cause unnecessary load to the segment as well as security risks

The Solicited-node multicast with the predefined mapping of IPv6 address to it's associated Ethernet MAC address ensures that there is no need to contact all devices in the local segment to find the MAC of the destination host, but the host can easily determine the destination MAC address of the host by prepending the last 32 bits from the IPv6 address to the reserved ethernet prefix 33-33 and send the frame directly. This mechanisms makes the IPv6 dynamic and efficient

A solicited-node multicast is more efficient than an Ethernet broadcast used by IPv4 ARP. With ARP, all nodes receive and must therefore process the broadcast requests. By using IPv6 solicited-node multicast addresses, fewer devices receive the request. Therefore fewer frames need to be passed to an upper layer to determine whether they are intended for that specific host.

![](<../.gitbook/assets/Unknown image (1113)>)

#### Node information multicast query

allows nodes to identify or resolve the IP address of a known hostname. It works similar to DNS service but only limited to the local network and can be used only for diagnostic and debugging purposes. In this mechanism, instead of querying a DNS server, a query is issued to the node information query address.

The node which wants to resolve the IP address of a known hostname calculates a hash of the hostname by using the 128-bit MD-5 algorithm. From the result, it takes the first 24 bits and appends them to the FF02::2:FF00:0/104 prefix to create a node information query address. After creating this address, the node sends a query message to this address.

When a node receives a message addressed to the information query address, it reverts the process. It creates a hash of its own hostname and compares the first 24 bits of the hash with the last 24 bits of the address of the received message. If it matches, the node replies with the requested information.

![node information query message](<../.gitbook/assets/Unknown image (1114)>)

## Reserved Addresses

Default route ::/0

Localhost/Loopback address ::1/128 same as 127.0.0.1 in IPv4

Unspecified ::/128 used by the Operating Systems in the absence of any valid IP address and processes like DHCP or DAD

IPv4 mapped IPv6 address

The prefix ::FFFF/96 is reserved by the Internet Assigned Numbers Authority (IANA) for IPv4-mapped IPv6 addresses as defined in RFC 4291.

Defined transition technique used in dual-stack environments and automatic tunneling methods

Structure: It is a 128-bit IPv6 address with a specific format:

The first 80 bits are all zeros (0000:0000:0000:0000:0000).

The next 16 bits (one hextect) are all ones (FFFF).

The last 32 bits embed the IPv4 address (w.x.y.z).

Format: The resulting structure is typically written as ::FFFF:w.x.y.z or 0:0:0:0:0:FFFF:w.x.y.z.

"Represent IPv4 address in IPv6 socket API": True. The primary purpose is for a dual-stack node (one that supports both IPv4 and IPv6) to represent an IPv4 address within its IPv6 socket API. This allows an IPv6 application to communicate with an IPv4-only host, with the operating system automatically handling the translation on the wire.

"They should never leave the host" and "They should never appear in any IPv6 packet": Mostly True. IPv4-mapped addresses are local to the host and are a form of internal signaling. They are not intended to be routed over an IPv6 network. A dual-stack node must convert the IPv4-mapped address back to a standard IPv4 packet before sending it out on the network. If they appeared in an IPv6 packet header, they would be non-routable and essentially meaningless outside the context of the originating host's socket layer.

"Inserting them into DNS would be stupid": True. DNS records are meant to map a hostname to a publicly usable address.

An A record maps to a standard IPv4 address.

An AAAA record maps to a standard IPv6 address.

An IPv4-mapped address is not a routable IPv6 address and thus has no place in public DNS resolution.

| **IPv6 Prefix** | **Allocation**       | **Reference** |
| --------------- | -------------------- | ------------- |
| 0000::/8        | Reserved by IETF     | RFC 3513      |
| 0100::/8        | Reserved by IETF     | RFC 3513      |
| 0200::/7        | Reserved by IETF     | RFC 4048      |
| 0400::/6        | Reserved by IETF     | RFC 3513      |
| 0800::/5        | Reserved by IETF     | RFC 3513      |
| 1000::/4        | Reserved by IETF     | RFC 3513      |
| 2000::/3        | Global unicast       | RFC 3513      |
| 4000::/3        | Reserved by IETF     | RFC 3513      |
| 6000::/3        | Reserved by IETF     | RFC 3513      |
| f800::/6        | Reserved by IETF     | RFC 3513      |
| fC00::/7        | Unique local unicast | RFC 4193      |
| fe00::/9        | Reserved by IETF     | RFC 3513      |
| fe80::/10       | Link-local unicast   | RFC 3513      |
| feC0::/10       | Reserved by IETF     | RFC 3879      |
| ff00::/8        | Multicast            | RFC 3513      |

## Neighbor Discovery Protocol (NDP)

**Neighbor Discovery Protocol (NDP)** is a key component of ICMPv6, responsible for dynamic neighbor and gateway discovery in IPv6. It combines several functionalities that were separate in IPv4, including:

Neighbor & Router Discovery (Equivalent and replacement for ARP in IPv4)

Host Autoconfiguration (SLAAC)

Duplicate Address Detection (DAD)

Path MTU discovery

Renumbering

Multicasting (MLD vs. IGMP)

Mobility

Five neighbor discovery messages

Router solicitation (ICMPv6 type 133) – used for SLAAC

Router advertisement (ICMPv6 type 134) – used for SLAAC

Neighbor solicitation (ICMPv6 type 135) – used for ARP and DAD

Neighbor advertisement (ICMPv6 type 136) ) – used for ARP and DAD

Redirect (ICMPV6 type 137)

#### Autoconfiguration: link-local + DAD (NS/NA)

When an IPv6 node is connected to an IPv6 enabled network, the first thing it does is to auto-configure itself with a link-local address, so that it can communicate at Layer 3 with other IPv6 devices in the local segment

The most widely adopted way of auto-configuring a link-local address is by combining the link-local prefix FE80::/64 and the EUI-64 interface identifier, generated from the interface's MAC address, however most secure way that should be used by operating systems is to use Random IID

After the IPv6 host has its autconfigured link-local address it joins a Solicited-node multicast group address FF02::1:FFxx:xxxx/104 where xx:xxxx are the last 6 hexadecimal values in the IPv6 unicast address. For example for link-local address FE80::1, the solicited-node multicast address will be FF02::1:FF00:1

By joining the solicited-node multicast, the host can perform Duplicate Address Detection (DAD) mechanism to check whether this address is already in use by any device, so that it can adopt the new auto-configured IPv6 address.

{% hint style="info" %}
For each configured unicast address, no matter if it is link-local or global, the host joins the respective auto-generated solicited-node multicast group to listen, if any other device attempts to use this address (despite that this is very unlikely)
{% endhint %}

DAD is performed with ICMPv6 Neighbor Solicitation (NS) message that is sent by the host with its tentative auto-configured IPv6 address as a destination and source set to the IPv6 unspecified address

If any device in the network already use this IPv6 address it responds back with unicast ICMPv6 Neighbor Advertisement (NA) to inform the host that this IPv6 address is already in use

The host then release that address and generates a new address and repeats the DAD process again, until it assumes a free address

![PC1 performs IPv6 DAD for its link-local address](<../.gitbook/assets/Unknown image (1115)>)

{% hint style="info" %}
This process is not exactly part of the SLAAC feature but without a link-local address, PC1 won't be able to communicate at layer 3 with any other IPv6 node
{% endhint %}

#### SLAAC: RS/RA → global address

SLAAC is a [Stateless](onenote:L4.one#DHCP\&section-id={E7D8C7C5-105B-4198-A054-E267184A51AE}\&page-id={220CA95E-B42E-4857-98AE-6B9487340AED}\&object-id={3240C289-3EBF-0052-0F65-AB20D938EAD5}&1D\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE) feature that enables IPv6 nodes to auto-generate GUA using Route Advertisements messages sent by a router attached to the local segment

After the host have valid link-local address, it can now start the process of auto-configuring a global unicast address using SLAAC

The host sends ICMPv6 Router Solicitation (RS) message to the all-nodes multicast address (ff02::2) to discover routers that are listening on that multicast group on a local segment

After one of the routers receive RS, it responds back with ICMPv6 Router Advertisements (RA)

{% hint style="info" %}
RAs are also sent periodically each 200seconds by routers to all nodes FF02::1 multicast address to announce their presence, providing it's link-local address and network configuration parameters such as link prefix and length (which is and must be /64 for SLAAC), hop limit, MTU, flags, Address lifetime, default router information
{% endhint %}

The new host will receive either the RA direct response or one of the periodical RAs received on the multicast for all nodes FF02::1 that he has subscribed for

Based on the information obtained from RAs, the new host can autoconfigure its IPv6 address using SLAAC that combines the prefix obtained from the RA message with the host's unique interface identifier EUI-64 or OS's specific algorithm to generate a globally unique IPv6

The hosts then perform DAD for it's autoconfigured global unicast address to ensure that no other device has it and joins the respective multicast address of it's new GUA address

{% hint style="info" %}
RA's are sent on multiaccess link types (e.g. Ethernet) and blocked on point-to-point link types (e.g. serial link) by default
{% endhint %}

![](<../.gitbook/assets/Unknown image (1116)>)

![](<../.gitbook/assets/Unknown image (1117)>)

#### Resolution process

Our host PC1 wants to resolve the MAC of the host PC3. It knows the link-local address of host PC3, so it derives the solicited-node multicast IP and MAC of the host PC3 and sends the NA message to that address

Since the host PC3 listens on the solicited-node multicast IPv6 address and assumed the ethernet multicast aswell, it receives the NA message and it will respond promptly with unicast NA message back to the PC1, containing his MAC address

As the new device communicates with other devices on the network, it exchanges NS and NA messages to populate its Neighbor cache to maintain mappings of IPv6 addresses to corresponding MAC addresses of neighboring devices

![IPv6 Neighbor Solicitation Message](<../.gitbook/assets/Unknown image (1118)>)

![IPv6 Neighbor Advertisement Message](<../.gitbook/assets/Unknown image (1119)>)

![IPv6 Router Advertisement Message](<../.gitbook/assets/Unknown image (1120)>)

Inverse Neighbor Discovery (INS) Uses the Inverse NS message to get the IPv6 address of a known MAC address

Renumbering RA also serves as dynamic renumbering mechanism that can auto-configure all hosts in a given segment, without human intervention on the hosts

ICMPv6 Redirect is used to inform hosts of a better first-hop router for a destination

Neighbor Unreachability detection (NUD) is an integrated feature in NDP that detects when a neighbor is no longer reachable

The NUD mechanism allows a host to determine whether a router (neighbor) in the host default gateway list is unreachable. Hosts receive the NUD value (known as the reachable time) from the routers on the local link via regularly advertised route advertisements. The default reachable time is 30 seconds.

NUD is used when a host determines that the primary gateway for IPv6 unicast traffic is unreachable. A timer is activated; when the timer expires (reachable time value), the neighbor begins to send IPv6 traffic to the next available router in the default gateway list. Under default configurations, using the next gateway in the default gateway list will take no longer than 30 seconds.

NDP sends periodical NS to confirm the reachability of neighbors on a segment, and if no response is received, the neighbor's entry in the neighbor table is updated to reflect that it is no longer reachable

#### NS PCAP

![](<../.gitbook/assets/Unknown image (1121)>)

{% hint style="info" %}
Despite ICMPv6 messages are meant for the local segment, they are set with the maximum hop limit of 255
{% endhint %}

#### RA Flags

Managed address configuration (M) flag if set to 1 - it tells the client to attempt to get an IPv6 address from a DHCPv6 server is set to 1

If the M flag is set, the O flag can be ignored because DHCPv6 will return all available information

Other configuration O-flag if set to 1 - indicates to the client to attempt to get additional configuration such as the address of the DNS servers from a DHCPv6 server

If neither M nor O flags are set, this indicates that no DHCPv6 server is available on the segment

Default Router Preference (Prf) flag when a node receives Router Advertisement messages from multiple routers, the DRP is used to determine which router to prefer as a default gateway can be set to Low (1), Medium (0) - default, or High (3)

![IPv6 Stateless Address Auto-configuration (SLAAC) | NetworkAcademy.io](<../.gitbook/assets/Unknown image (1122)>)

#### RA Options

within the ICMPv6 options (RA options) the router can specify MTU, Prefix information with additional flags to indicate how the hosts should autoconfigure their address

ICMPv6 Prefix information option

On-link L flag if set to 1 (default) it tells to hosts that they can assume that other nodes sharing the same prefix are directly reachable on the same subnet

You can use command ipv6 nd prefix 2001:db10:1:1::/64 off-link If you want to advertise to hosts other subnet or a route that is not reachable on that link/subnet, where the RA is sent, the router can have for example a static route to the subnet

no L - send all traffic via router

| no auto-config | to indicate to hosts to not to use the prefix for auto-configuration |
| -------------- | -------------------------------------------------------------------- |

Autonomous address-configuration (A) flag if set to 1 (default) it indicates to hosts to generate their IPv6 address on that link

![](<../.gitbook/assets/Unknown image (1123)>)

#### IPv6 Address Assignment Methods & Flag Combinations

There are five methods in IPv6 how to assign an IP address to a node: Manual, SLAAC, DHCPv6, SLAAC with Router advertisements or a combination of SLAAC & DHCPv6 lite

Manual

M/O/A = 0/0/0

SLAAC (RFC 4862)

M/O/A = 0/0/1 - default on Cisco routers

[DHCPv6](onenote:L4.one#DHCPv6\&section-id={E7D8C7C5-105B-4198-A054-E267184A51AE}\&page-id={6273F2B2-9AA6-490F-9C12-94CE38BC308A}\&end\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE) (RFC 3315)

M/O/A = 1/0/0

[SLAAC & DHCPv6 Lite (RFC 3736)](onenote:L4.one#DHCPv6\&section-id={E7D8C7C5-105B-4198-A054-E267184A51AE}\&page-id={6273F2B2-9AA6-490F-9C12-94CE38BC308A}\&object-id={E8F161A7-783B-0E22-2DBD-40153DB216A3}&1A\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE)

M/O/A = 0/1/1

SLAAC & IPv6 RA DNS Options (RFC 6106, 11/2010)

M/O/A = 0/0/1, RA with DNS info

{% hint style="info" %}
M/O/A = 1/0/1 - should be avoided since it instructs hosts to create two methods, the SLAAC, but also to use address from DHCPv6 servers - duplicate management and security risk
{% endhint %}

Cisco router IPv6 config

| ipv6 unicast-routing                                                                                                            | disabled by default and must be enabled so that the router can send RA's - this is to prevent routers to send RA's by default                                                                                                                                    |
| ------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config)#interface GigabitEthernet 0/0                                                                                          | show ipv6 interface                                                                                                                                                                                                                                              |
| (config-if)# ipv6 enable                                                                                                        | Enables IPv6 on the interface and Assign IPv6 link-local address to the interface                                                                                                                                                                                |
| (config-if)#ipv6 address 2001:1234:A:B::1/64 \[eui-64]                                                                          | as soon as interface is configured with GU and ipv6 enable, it starts sending RA messages eui-64 keyword to complete the ipv6 prefix - this is not recommended                                                                                                   |
| (config-if)#ipv6 address FE80::1 link-local                                                                                     | to create manual link-local instead of the autogenerated from ipv6 enable or ipv6 address command                                                                                                                                                                |
| (config-if)#ipv6 address autoconfig                                                                                             | it will listen to RA's and autoconfigures it's interface accordingly if you use the default keyword, the router will install a default route - used on WAN interface, where the router is conneced to the ISP for example                                        |
| Router Advertisement config                                                                                                     |                                                                                                                                                                                                                                                                  |
| (config-if)#ipv6 nd prefix \[prefix/length] \[no-advertise]                                                                     | to configure what prefixes you want router to include in RA's - by default, all prefixes configured as addresses on interface are advertised, so if you want to limit, use the no-advertise keyword and no0autoconfig if you dont want hosts to use it for SLAAC |
| (config-if)#ipv6 nd \[other/managed-config-flag]                                                                                | to specify what flag you want the router to include in RA's                                                                                                                                                                                                      |
| (config-if)#ipv6 nd ra \[interval \| lifetime \| suppress]                                                                      | interval specifies how frequent the Router advertisements should be sent lifetime specifies how long the advertising router can be used as default-gateway suppress suppresses RA from being sent "all" keyword suppresses periodic and solicited RA messages    |
| ipv6 nd ra dns server                                                                                                           | includes dns server IP address in RA's                                                                                                                                                                                                                           |
| show ipv6 \[ neighbors \| routers ]                                                                                             | to display router or neighbor cache                                                                                                                                                                                                                              |
| netsh interface ipv6 >show \[] >set address \[interface name] \<add/prefix>> add route ::/0 “Ethernet 2” 2001:db8:ea34:444::254 | to configure ipv6 on windows or ipconfig /all example set address “Ethernet 2” 2001:db8:ea34:444::1/64 to add static default route In GUI if you check the obtain IPv6 automatically instructs host to assume address from RA, not from DHCP like in IPv4        |

<figure><img src="../.gitbook/assets/Unknown image (1124)" alt=""><figcaption></figcaption></figure>

Traceroute to the internet over IPv6-enabled ISP Vodafone

![](<../.gitbook/assets/Unknown image (1125)>)

## IPv6 Mobility

IP Mobility is a very important feature in today's networks. MobileIP is an IETF standard available for both IPv4 and IPv6.

It enables mobile devices to move without breaking current connections.

The routing headers of IPv6 makes mobile IPv6 much more efficient for end nodes than mobile IPv4.

Mobility is available for IPv4 and IPv6 regardless of access technology

An example would be that a car navigation unit would synchronize maps while being parked in the garage through the home WLAN, and continue doing so across the mobile network (3G/4G, etc.) when the car is being driven.

The elements of Mobile IP networks are:

**Mobile Node (MN)**: A node that can change its point of attachment from one link to another, while still being reachable via its home address.

Mobile nodes use a special protocol to discover the home agent while they roam on a foreign link. The discovery process uses ICMPv6 packets, so it is essential that they are permitted to pass through firewalls on their way.

**Home Address**: A unicast routable address that is assigned to a mobile node and is used as the permanent address of the mobile node. The address is within the home link of the mobile node. Standard IP routing mechanisms will deliver packets that are destined for the home address of a mobile node to its home link. Mobile nodes can have multiple home addresses, for instance, when there are multiple home prefixes on the home link.

**Home Link**: The home subnet prefix of a mobile node is defined on the home link.

**Foreign Link**: Any link other than the home link of the mobile node.

**Care-of Address**: A unicast routable address that is associated with a mobile node while visiting a foreign link. The subnet prefix of this IP address is a foreign subnet prefix. Among the multiple care-of addresses that a mobile node may have (for example, with different subnet prefixes), the address that is registered with the home agent of the mobile node, for a given home address, is called its "primary" care-of address.

**Home Agent (HA)**: A router on the home link of a mobile node, with which the mobile node has registered its current care-of address. While the mobile node is away from home, the home agent intercepts packets on the home link that are destined for the home address of the mobile node. The home agent then encapsulates the packets and tunnels them to the registered care-of address of the mobile node.

Corresponding node (CN) represents the destination end of the Mobile node e.g. the destination it is communicating with

**Foreign Agent**: A router storing information about mobile devices visiting the network. It is also used to terminate the tunnels to various home agents. Used only in IPv4 network

Operation

When a Mobile Node leaves its Home Link and is connected to some Foreign Link, the Mobility feature of IPv6 comes into play. After getting connected to a Foreign Link, the Mobile Node acquires an IPv6 address from the Foreign Link. This address is called Care-of Address (CoA). The Mobile Node sends a binding request to its Home Agent with the new Care-of Address. The Home Agent binds the Mobile Node’s Home Address with the Care-of Address, establishing a Tunnel between both.

Whenever a Correspondent Node tries to establish connection with the Mobile Node (on its Home Address), the Home Agent intercepts the packet and forwards to Mobile Node’s Care-of Address over the Tunnel which was already established.

Packets from the correspondent node are routed to the home agent. When using bidirectional tunneling, Mobile IPv6 support is not required on the correspondent node.

Packets from the correspondent node are routed to the home agent and then tunneled to the mobile node.

In this mode, the home agent uses proxy neighbor discovery to intercept any IPv6 packets addressed to the home address (or home addresses) of the mobile node on the home link.

Each intercepted packet is tunneled to the primary CoA of the mobile node. This tunneling is performed using IPv6 encapsulation.

![](<../.gitbook/assets/Unknown image (1126)>)

#### Route Optimization

When a Correspondent Node initiates a communication by sending packets to Mobile the Node on the Home Address, these packets are tunneled to the Mobile Node by the Home Agent

In Route Optimization mode, when the Mobile Node receives a packet from the Correspondent Node, it does not forward replies to the Home Agent. Rather, it sends its packet directly to the Correspondent Node using Home Address as Source Address. This mode is optional and not used by default.

Corresponding node uses a new type of IPv6 routing header to route the packet to the mobile node by way of the CoA indicated in this binding.

Routing packets directly to the CoA of the mobile node allows the shortest communications path to be used. It also eliminates congestion at the home agent of the mobile node and home link. In addition, the impact of any possible failure of the home agent or networks on the path to or from it is reduced.

One advantage of Mobile IPv6 is masking the current CoA for an away-from-home node. When you are not using route optimization—regardless of the CoA of the mobile node—the mobile node appears to be at home to correspondent nodes. Only the home agent knows that the mobile node is away from home. If, however, the mobile node attempts to use the correspondent node binding feature of Mobile IPv6 support, that correspondent node will be aware that the mobile node is no longer on the home network. If this awareness is considered a security issue, route optimization can and should be disabled.

To accommodate mobility requirements, an additional header is inserted between the initial IPv6 header and the transport layer.

When routing packets directly to the mobile node, the correspondent node sets the destination address in the IPv6 header to the CoA of the mobile node. A new type of IPv6 routing header is also added to the packet to carry the desired home address. Similarly, the mobile node sets the source address in the IPv6 header of the packet to its current CoAs. The mobile node adds a new IPv6 home address destination option to carry its home address.

The inclusion of home addresses in these packets makes using the CoA transparent above the network layer (for example, at the transport layer).

![](<../.gitbook/assets/Unknown image (1127)>)

#### Routing Header Type 2

Routing Header Type 2 (RH2) is used to maintain seamless communication without disrupting the active connection.

Routing Header Type 2 (RH2) ensures that packets reach the Care-of Address (CoA) even when the Home Address remains in the destination field

The Home Agent (HA) receives packets from the Correspondent Node (CN) destined for the Home Address of the mobile node.

The HA knows the mobile node's current location (its CoA) because the mobile node registered this information during its mobility process.

The HA modifies the packet to include a Routing Header Type 2 (RH2).

In RH2:

The CoA is added as a routing destination.

The Home Address remains as the final destination in the IPv6 header

IPv6 Routers Understand RH2: When the packet leaves the HA and traverses the network, intermediate routers inspect the Routing Header Type 2 (RH2).

The RH2 instructs the routers to forward the packet toward the CoA (not the Home Address, despite it being in the main destination field).

When the packet reaches the foreign link where the mobile node resides, the final hop processes the RH2.

The RH2 is stripped, leaving the Home Address as the destination.

The mobile node receives the packet as if it were sent directly to its Home Address

The routing process ensures that the packet doesn’t get stuck in a loop:

IPv6 routers along the path read and act upon the RH2. They forward the packet to the CoA, as specified in the routing instructions

#### Mobility Protocols

MIPv6 (Mobile IPv6) Designed for host-based mobility

The MN directly sends Binding Updates (BU) to the HA and CN to update its Care-of Address.

Communication can be routed directly between MN and CN using route optimization.

PMIPv6 (Proxy Mobile IPv6): Designed for network-based mobility.

The MAG sends Proxy Binding Updates (PBU) to the LMA on behalf of the MN.

The MN doesn't need to handle mobility signaling at all.

| **Feature**                  | **MIPv6**          | **PMIPv6**                                       |
| ---------------------------- | ------------------ | ------------------------------------------------ |
| **Type of Mobility**         | Host-based         | Network-based                                    |
| **Signaling Responsibility** | Mobile Node        | MAG on behalf of MN                              |
| **Key Components**           | MN, HA, CN         | MN, MAG, LMA                                     |
| **Addressing**               | Home Address + CoA | Home Address only                                |
| **Tunneling**                | HA ↔ MN            | LMA ↔ MAG                                        |
| **Use Case**                 | General mobility   | Simplified mobility for IoT, enterprise networks |
| **Security**                 | Managed by MN      | Managed by MAG and LMA                           |

#### Dual Stack Mobile IPv6 (DSMIPv6)

Mobile nodes are able to use IPv4 and IPv6 home or care-of addresses simultaneously and update their home agents accordingly.

Mobile nodes need to be able to know the IPv4 address of the home agent as well as its IPv6 address. There is no need for IPv4 prefix discovery.

Mobile nodes need to be able to detect the presence of a NAT device and traverse it in order to communicate with the home agent.

Clients must implement MIP in the kernel (MIP mobility is host-based)

Linux: Most Linux distributions support Mobile IPv6 through tools like mip6d or kernel features.

Windows does not natively include Mobile IPv6 support in recent versions. For mobility, additional software or middleware (e.g., third-party tunneling solutions) is required.

macOS has limited native Mobile IPv6 support and may require third-party software or a custom stack for mobility scenarios.

#### Return routability procedure

enables the correspondent node to obtain some reasonable assurance that the mobile node is, in fact, addressable at its claimed CoA and also at its home address. Only with this assurance can the correspondent node accept binding updates from the mobile node. The mobile node then instructs the correspondent node to direct the data traffic of that mobile node to its claimed CoA.

This procedure tests whether packets that are addressed to the two claimed addresses are routed to the mobile node.

The mobile node can pass the test only if it is able to supply proof that it received certain data (the keygen tokens), which the correspondent node sends to those addresses.

RFC 4449, Securing Mobile IPv6 Route Optimization Using a Static Shared Key, improves the return routability procedure to protect mobile node-correspondent node bindings, at the expense of requiring additional in-advance setup. In this method, the two parties share a secret key that establishes initial trust.

#### Dynamic Home Agent Address Discovery

When the mobile node needs to send a binding update to its home agent to register its new primary CoA, the mobile node may not know the address of any router that can serve as a home agent on its home link. For example, some nodes on the home link of a mobile node may have been reconfigured while the mobile node was away from home. Therefore, a different router replaced the router that was operating as the home agent of the mobile node.

In this case, the mobile node may attempt to discover the address of a suitable home agent on its home link. To do so, the mobile node sends an ICMP home agent address discovery request message for its home subnet prefix to the anycast address of the Mobile IPv6 home agent (the subnet prefix, followed by all 1s except for the Universal and Local bit for EUI-64 addresses, and the last seven bits, which for this anycast address is 7E).

The home agent, on the home link that receives this request message, will return an ICMP home agent address discovery reply message. The message gives the addresses for the home agents operating on the home link. The mobile node, upon receiving this home agent address discovery reply message, may then send its home registration binding update to any of the unicast IP addresses listed in the home agent addresses field in the reply.

{% hint style="info" %}
Dynamic home agent discovery, while a powerful solution for nodes that rarely return to the home network, can also expose certain security issues because these messages are unauthenticated. Suppose a vulnerability is discovered for a well-known and widely deployed home agent. Attackers could sweep the Internet posing as mobile nodes, attempting to connect to Mobile IPv6 home agent anycast addresses. By finding an active home network, they would be told the location of all the home agents. In a high-security deployment, this feature can be disabled.
{% endhint %}

#### Mobile IPv6 Node Returning Home

A mobile node determines that it has returned to its home link through the movement detection algorithm when the mobile node detects that its home subnet prefix is again on-link. The mobile node should then send a binding update to its home agent instructing it to no longer intercept or tunnel packets for it. In this home registration, the mobile node must set the acknowledge (A) bit and the home registration (H) bit. It must also set the CoA for the binding to the home address of the mobile node. The mobile node must use its home address as the source address in the binding update. The mobile node sets the A and H bits as follows:

The sending mobile node sets the A bit to request the return of a binding acknowledgment upon receipt of the binding update.

The sending mobile node sets the H bit to request that the receiving node act as the home agent of this node. The destination of the packet carrying this message must be that of a router sharing the same subnet prefix as the home address of the mobile node in the binding.

In this special case of the mobile node returning home, the mobile node must send a multicast packet and, in addition, set the source address of this neighbor solicitation to the unspecified address (0:0:0:0:0:0:0:0). The target of the neighbor solicitation must be set to the home address of the mobile node. The destination IP address must be set to the solicited-node multicast address of the home address of a mobile node.

The home agent will send a multicast neighbor advertisement back to the mobile node with the solicited (S) flag set to zero. The mobile node then sends its binding update to the MAC address of the home agent, instructing its home agent to no longer serve as a home agent for it. By processing this binding update, the home agent will cease defending the home address of the mobile node for Duplicate Address Detection and will no longer respond to neighbor solicitations for the home address of the mobile node. The mobile node is then the only node on the link receiving packets at the home address of the mobile node.

After the mobile node sends the binding update, it must be prepared to reply to neighbor solicitations for its home address. Such replies must be sent using a unicast neighbor advertisement to the MAC address of the sender. After receiving the binding acknowledgment for its binding update to its home agent, the mobile node must send a multicast packet onto the home link (to the all-nodes multicast address) to advertise the MAC address of the mobile node for its own home address.

#### Network Mobility (NEMO)

The NEMO is an application for Cisco IOS which enables a router to move to any point in the IPv6 Internet and still be reachable from any point at its original IP address. NEMO extends the concept of Mobile IPv6 from a mobile node to a mobile router.

When the mobile router moves away from the home link and attaches to a new access router, it acquires a care-of address (CoA) from the visited link. Using the CoA, it immediately sends a binding update to its home agent. When the home agent receives this binding update, it creates a binding cache entry that binds the home address of the mobile router to its current CoA.

If the mobile router wishes to act as a mobile router and provide connectivity to nodes in the mobile network, it indicates this desire to the home agent by setting a router flag (R) in the binding update. It may also include information about the mobile network prefix in the binding update. The home agent can then forward packets that are meant for nodes in the mobile network to the mobile router. A new mobility header option is specified for mobile networks.

All traffic between the nodes in the mobile network and correspondent nodes passes through the home agent.

The dynamic home agent address discovery (DHAAD) mechanism allows a mobile node to discover the address of the home agent on its home link:

The mobile router sends Internet Control Message Protocol (ICMP) home agent address discovery requests to the Mobile IPv6 home agent's anycast address for the home subnet prefix.

A new flag (R) is introduced in the DHAAD request message, indicating the desire to discover home agents that support mobile routers. This flag is added to the DHAAD reply message as well.

On receiving the home agent address discovery reply message, the mobile router discovers the home agents operating on the home link.

The mobile router attempts home registration to each of the home agents until its registration is accepted. The mobile router waits for the recommended length of time between its home registration attempts with each of its home registration attempts.

The mobile router will acquire the prefix for the mobile network and for the devices attached to it in one of the following ways:

Implicit Prefix Registration: The mobile router does not register any prefixes as part of the binding update with its home agent. This function requires a static configuration at the home agent, and the home agent must have the information of the associated prefixes with the given mobile router for it to set up route forwarding.

Explicit Prefix Registration: The mobile router presents a list of prefixes to the home agent as part of the binding update procedure. If the home agent determines that the mobile router is authorized to use these prefixes, it sends a bind acknowledgment message.

Prefix assignment using Prefix Delegation: The prefix is acquired from the centrally located DHCPv6 server configured for prefix delegation. The mobile router will then use the assigned prefix obtained from DHCP, and advertise it using Router Advertisements on the local link to auto-configure the nodes attached to the mobile router.

![](<../.gitbook/assets/Unknown image (1128)>)

#### Mobile Ad Hoc Networking

A mobile ad hoc network is a collection of mobile nodes that have organized themselves into a network

Initial applications for Mobile Ad Hoc Networking (MANET) involved the military and transportation sectors.

In the military arena, one potential application is to enable small wireless sensors with MANET for deployment in the battlefield. Once dispersed, the sensors would organize themselves into a network to exchange data and information on how to reach the network uplink. Because of the hostile environment, these sensors are expected to be destroyed, moved, or otherwise have their connections to the rest of the sensors disrupted on a regular basis.

In the transportation field, MANET is being considered as a way to dynamically update vehicles regarding traffic conditions. Vehicles near a traffic disruption would alert other vehicles with MANET regarding current traffic conditions. This information would propagate its way from car to car, enabling drivers not yet affected by a traffic incident to select alternative routes to avoid the disruption.

Routing protocols supporting MANET operation include OSPFv3, EIGRP, and IPv6 Routing Protocol for Low-Power Lossy Networks (RPL for LLNs). The key issues are rapidly changing topology (when devices arrive and leave), end link metric reporting when conditions change. Routing protocols feature an interface with the radio system of the device, and react to link quality changes.

Low-power lossy networks consist of devices limited with processing power and radio range, but very small in physical size. Due to their simplicity, such devices can be deployed in very large numbers and organize themselves into a network. Examples would include "sensor dust", etc.

OSPFv3 and EIGRP extensions to support MANET are implemented in Cisco IOS; the IPv6 Routing Protocol for Low-power Lossy Networks is currently in the phase of an IETF draft.

![](<../.gitbook/assets/Unknown image (1129)>)

IPv6 Personal Area Networks

Mobile IPv6-enabled cellular phone acts as a mobile router.

![](<../.gitbook/assets/Unknown image (1130)>)

## Transition to IPv6

Supporting two protocols over time will presumably be more costly than only one protocol and to run IPv6 in our network, it must support IPv6 in the first place. Also modifications to services such as DNS, DHCP and security policies must be implemented to support IPv6

{% hint style="info" %}
Both SD-WAN and SD-Acess support IPv4 and IPv6 and provides seamless transition.
{% endhint %}

To transition to IPv6 we can employ following techniques:

#### Dual-stack

The simplest solution to IPv4 and IPv6 co-existence is to run both IPv6 and IPv4 on interfaces, essentially creating two logical networks

Which version hosts use depends either on the version of packets they receive from a device or the type of address DNS gives them when they query for a domain or device address

Dual stack is however more complex and we simply cannot keep assigning both Ipv4 and Ipv6 address to every interface, because the IPv4 address would still be depleted in some point in time

Advantages:

Relatively simple to deploy

Retains IPv4 support

Support widely available

Disadvantages: Doubles most requirements (two routing tables, two routing processes, security) and requires more control packets to be sent periodically for each protocol, which may consume bandwidth

#### Tunnels

Tunnels are about co-existence, not interoperability. They allow devices or sites of one IP version to communicate across a network segment of the other IP version

So two IPv4 devices or sites can exchange IPv4 packets across an IPv6 network, or two IPv6 devices or sites can exchange IPv6 packets across an IPv4 network.

Benefits:

The cost is low

The solution is simple: interconnect IPv6 islands

There is IPv6 Internet connectivity on existing IPv4 connections

Encapsulation has several drawbacks:

Overhead adds to delay and jitter and consumes resources

Management is complex

MTU is reduced

#### Translation techniques

Translators creates interoperability between an IPv4 device and an IPv6 device by changing the header of a packet of one version to the header of the other version.

Like other transition methods, translation is not a long-term strategy and the ultimate goal is to deploy native IPv6 infrastructure

Benefits of translation mechanism include: Communication is possible with legacy applications that may never attain IPv6 support.

Drawbacks:

Breaks end-to-end security

Single point of failure and performance

using proxies as an alternative:

Application-Layer Gateways

Require application support

#### Dual-Stack

as a standalone technique comprise of IPv4- and IPv6-enabled devices, so that IPv4-enabled and IPv6-enabled applications can query the DNS server and depending to application's preference it can choose to connect either to IPv4 or IPv6 address

![](<../.gitbook/assets/Unknown image (1131)>)

#### Dual Stack Lite (DS-Lite)

used when the service provider provides access to both IPv4 and IPv6 clients, and have it's core IPv6-only, so the traffic of IPv4 clients is tunnelled over SP IPv6 infrastructure

Dual-Stack Lite technology does not involve allocating an IPv4 address to customer-premises equipment (CPE) for providing Internet access.\[32] The CPE distributes private IPv4 addresses for the LAN clients, according to the networking requirement in the local area network. The CPE encapsulates IPv4 packets within IPv6 packets. The CPE uses its global IPv6 connection to deliver the packet to the ISP's carrier-grade NAT (CGN), which has a global IPv4 address. The original IPv4 packet is recovered and NAT is performed upon the IPv4 packet and is routed to the public IPv4 Internet. The CGN uniquely identifies traffic flows by recording the CPE public IPv6 address, the private IPv4 address, and TCP or UDP port number as a session.

Lightweight 4over6 extends DS-Lite by moving the NAT functionality from the ISP side to the CPE, eliminating the need to implement carrier-grade NAT.\[33] This is accomplished by allocating a port range for a shared IPv4 address to each CPE. Moving the NAT functionality to the CPE allows the ISP to reduce the amount of state tracked for each subscriber, which improves the scalability of the translation infrastructure.

It's called CGv6 in config guide [https://www.cisco.com/c/en/us/td/docs/routers/asr9000/software/24xx/cgnat/configuration/guide/b-cgnat-cg-asr9k-24xx/cgipv6-over-vsm.html](https://www.cisco.com/c/en/us/td/docs/routers/asr9000/software/24xx/cgnat/configuration/guide/b-cgnat-cg-asr9k-24xx/cgipv6-over-vsm.html)

![](<../.gitbook/assets/Unknown image (1132)>)

#### Translation

relies on the dual-stack with NAT-enabled infrastructure, allowing to translate IPv6-only devices to IPv4 in order to communicate with IPv4-only devices

Information Encoding can be used to encode information such as VLANs in a IPv6 prefix VLAN 4092 -> 2001:db8:1234:4092::/64

#### IPv4 mapped IPv6

is an RFC4291 defined transition technique employed by multiple transition protocols (such as 6PE/6VPE), where first 80 bits are all zeroes, followed by one hextet full of FFFF and the IPv4 is embeded in the last 32bits of the IPv6 address

Represent IPv4 address in IPv6 socket API

They should never leave the host

They should never appear in any IPv6 packet

Inserting them into DNS would be stupid

![](<../.gitbook/assets/Unknown image (1133)>)

#### Stateless NAT64

translation maps every IPv6 address to the IPv4 address (1:1), so that it will give an IPv6-only host access to the IPv4 world and vice versa, but it consumes an IPv4 address for each IPv6-only device that desires translation. Consequentially, stateless NAT64 is not a solution to the ongoing IPv4 address depletion

However it is a good tool to provide Internet servers with an accessible IP address for both IPv4 and IPv6 on the global Internet

To aggregate many IPv6 users into a single IPv4 address, stateful NAT64 is required.

[https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/ip-addressing/b-ip-addressing/m\_iadnat-stateless-nat64.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/ip-addressing/b-ip-addressing/m_iadnat-stateless-nat64.html)

Static

[https://www.cisco.com/c/en/us/support/docs/ip/network-address-translation-nat/217208-understanding-nat64-and-its-configuratio.html](https://www.cisco.com/c/en/us/support/docs/ip/network-address-translation-nat/217208-understanding-nat64-and-its-configuratio.html)

* 1:1 translation
* No conservation of IPv4 address
* Assures end-to-end address transparency and scalability
* No state or bindings created on the translation
* Requires IPv4-translatable IPv6 address assignment (mandatory requirement)
* Requires either manual or DHCPv6-based address assignment for IPv6 hosts

Stateful NAT64

* 1:N translation - overloading
* Conserves IPv4 address
* State or bindings are created on every unique translation
* No requirement on the nature of IPv6 address assignment
* Free to choose any mode of IPv6 address assignment (DHCPv6 and stateless autoconfiguration)

#### Mapping of Address and Port (MAP)

The purpose of Mapping of Address and Port (MAP) transition mechanisms is to enable communication of IPv4 clients by using an IPv6-only transport network to connect to the IPv4 Internet and mapping IPv4 addresses and ports (TCP and UDP) over IPv6.

There are two forms of MAP:

MAP-E, defined in RFC7597 (2015) and based on the encapsulation of IPv4 over IPv6 and mapping of IPv4 addresses and ports to IPv6 and vice versa.

MAP-T, defined in RFC7599 (2015) and based on the translation of IPv4 addresses and ports to IPv6 and vice versa.

MAP has the following functional components:

MAP CE (Customer Edge): located on the customer side in the CPE.

MAP BR (Border Relay): located in the customer's network, at the edge of the IPv4 Internet.

MAP-E employs IPv4 in IPv6 encapsulation between MAP CE and MAP BR. MAP-T maintains the functional structure of MAP-E but does not use encapsulation; instead, it uses 4>>6 translation in CE and 6>>4 in BR. MAP-T has the advantage of eliminating the overhead that MAP-E introduces

[https://www.lacnic.net/innovaportal/file/5522/1/map-e-and-map-t-en.pdf](https://www.lacnic.net/innovaportal/file/5522/1/map-e-and-map-t-en.pdf)

Advantages:

o Does not require adaptations or modifications in dual-stack or IPv4-only customers.

o IPv6-only transport network: high efficiency and performance, single protocol stack and management - e.g. requires an IPv6 access network

o Promotes IPv6-only deployment in the ISP's transport network.

o Because the transport network is IPv6-only, there are no limitations or need to overlap the addressing of thousands of CPEs.

o Native IPv6 traffic is neither translated nor encapsulated.

o Automatic provisioning of MAP CE with DHCPv6 options.

o Adaptations have no impact on the operator's IPv6 addressing.

o Does not require CGNAT.

o Better performance than DS-Lite, as NAPT is distributed.

Disadvantages

o In the case of MAP-E, encapsulation overhead in the transport network.

o Does not support multicast traffic.

o May require CPE update to support MAP CE.

o Does not solve the problem of IPv4 exhaustion.

o Neither MAP-T nor MAP-E were designed for mobile cellular networks.

o In the case of MAP-E, IPv4/IPv6 encapsulation in the IPv6-only transport network adds some complexity to DPI in the operator's network.

The Mapping of Address and Port Using Translation feature provides connectivity to IPv4 hosts across IPv6 domains. Mapping of address and port using translation (MAP-T) builds on the existing stateless IPv4 and IPv6 address translation techniques that are specified in RFCs 6052, 6144, and 6145.

[https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/ip-addressing/b-ip-addressing/m\_ip-nat-divi-v4v6.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/ip-addressing/b-ip-addressing/m_ip-nat-divi-v4v6.html)

#### Stateful NAT64 and DNS64

RFC 6146: Stateful NAT64: Network Address and Protocol Translation from IPv6 Clients to IPv4 Servers

Stateful NAT64 work together with DNS64 to allow an IPv6-only client to initiate communications to an IPv4-only server

In stateful NAT64, states are maintained. A single IP Address is used for all the private users with different port numbers

NAT64 can also be used for IPv4-only clients initiating communications with IPv6-only servers using static or manual bindings

NAT64 is automatically available on your existing NAT gateways or on any new NAT gateways you create - It's not a feature you enable or disable

Operation

When DNS64 is asked for an AAAA record, but finds or receives from an upstream DNS only an A record, it synthesizes the AAAA record from the A record

The first part of the synthesized AAAA record is a Well-known Prefix (WKP) 64:ff9b::/96 and the second part (/32) of the AAAA record is an IPv4 address of the IPv4 host that is found in the A record. ISP also can use an NSP (network-specific prefix) as part of its IPv6 address space as a translation prefix

When IPv6-only service sends network packets to the synthesized IPv6 address through the NAT gateway the following happens:

When the NAT64 device receives the IPv6 packet, it recognizes the translation prefix, either the well-known or NSP and translates the IPv6 header to the IPv4 header

The destination IPv4 address is determined by stripping the translation prefix from the destination IPv6 addresses

The source IPv4 address is chosen from the IPv4 pool and overloaded into a single IPv4 address

The NAT64 device creates an entry in the translation table that is used for subsequent and returning packets

The response IPv4 packets are destined for NAT gateway, which receives the packets and de-NATs them by replacing its IP (destination IP) with the host’s IPv6 address and prepending back NSP or WKP to the source IPv4 address

{% hint style="info" %}
Stateful NAT64 is shown with a static 1:1 entry. It can also use overloading, as described in the stateful NAT section.
{% endhint %}

![](<../.gitbook/assets/Unknown image (1134)>)

| Prefix (NSP or WKP) | IPv4 Address | IPv4-Embedded IPv6 Address                                        |
| ------------------- | ------------ | ----------------------------------------------------------------- |
| 2001:db8::/64       | 192.0.2.10   | 2001:db8::c000:020a                                               |
| 64:ff9b::/64        | 192.0.2.10   | 64:ff9b::192.0.2.10 (can be written as 64:ff9b::c000:020a in HEX) |

IOS-XE configuration

![](<../.gitbook/assets/Unknown image (1135)>)

#### 464XLAT

(RFC 6877) allows clients on IPv6-only networks to access IPv4-only Internet services.

The client uses a SIIT translator to convert packets from IPv4 to IPv6. These are then sent to a NAT64 translator which translates them from IPv6 back into IPv4 and on to an IPv4-only server. The client translator may be implemented on the client itself or on an intermediate device and is known as the CLAT (Customer-side transLATor). The NAT64 translator, or PLAT (Provider-side transLATor), must be able to reach both the server and the client (through the CLAT). The use of NAT64 limits connections to a client-server model using UDP, TCP, and ICMP.

464XLAT is a translation mechanism designed primarily for mobile networks to enable IPv6-only devices to communicate with the IPv4 internet

XLAT: Zkratka pro "translation" (překlad).

Stateless IP/ICMP Translation (SIIT) translates between the packet header formats in IPv6 and IPv4.\[2] The SIIT method defines a class of IPv6 addresses called IPv4-translated addresses.\[3] They have the prefix ::ffff:0:0:0/96 and may be written as ::ffff:0:a.b.c.d, in which the IPv4 formatted address a.b.c.d refers to an IPv6-enabled node

#### IPv6 Tunneling Introduction and Disadvantages

Tunneling techniques encapsulates IPv6 packets into IPv4 packets (or the other way around) so that a one IP protocol can be routed over infrastructure running on a second IP protocol

However, tunneling methods have an impact on the MTU due to the additional encapsulation overhead

Moreover often blocked by the firewalls configured to drop certain tunneling protocol identifiers in the IP header

Both the static and dynamic tunneling should be avoided, because the aim is to transition to the IPv6 completely with dual-stack running during the transition

Because both methods only try to work-around the IPv4 infrastructure and tunnel the IPv6, which is not desired and the goal is to have the infrastructure based on IPv6

[Generic Routing Encapsulation (GRE)](onenote:https://d.docs.live.net/B03DD2DFB2522723/Notes/Security.one#VPN\&section-id={0B7736F7-88AD-4CBC-BEE3-A076F5EBB1FC}\&page-id={6DDDECA7-1133-4F34-81DB-CE962D0D742C}\&object-id={EE69CD8C-FC7F-038D-1C0E-16F8D7EAB682}&74) Static Tunneling

is used as a static method for a tunneling arbitrary Layer 3 protocol inside IPv4 or IPv6, adding additional 24bytes of overhead

![](<../.gitbook/assets/Unknown image (1136)>)

| interface Tunnel0 ipv6 address 2001:db8:3::1/64 tunnel source GigabitEthernet0/0 tunnel destination 209.165.201.6 tunnel mode gre ip | tunneling of IPv6 over IPv4 using GRE tunnels is not supported on Cisco IOS XR Software platforms |
| ------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------- |

![](<../.gitbook/assets/Unknown image (1029)>)

{% hint style="info" %}
GRE wastes MTU. It adds 24 bytes of overhead.
{% endhint %}

#### 6in4 Static Tunneling

is an IPv6-stack only encapsulation method that is more efficient than GRE, due to the fact that it doesn't use the GRE header, but instead it encapsulates the IPv6 packet directly in and IPv4 header, wherein it identifies itself with the IP protocol number 41 in the IPv4 header

![](<../.gitbook/assets/Unknown image (1030)>)

![](<../.gitbook/assets/Unknown image (1031)>)

#### 6to4 Automatic Tunnels

6to4 uses 6in4 encapsulation to establish automatic stateless IPv6 tunnels over IPv4 infrastructure

It's architecture is based on the reserved prefix 2002::/16 that can be subnetted with a /48 prefix for each IPv6 site, which in turn can be subnetted further for each subnet on the site

The site prefix /48 is created by defining the 2002 fixed prefix and by encoding IPv4 address of the WAN interface of the edge router in hexadecimal

{% hint style="info" %}
Example: if the IPv4 address of the edge router is `209.165.201.1`, the IPv6 site prefix is `2002:d1a5:c901::/48` (because `d1a5:c901` is the hex form of `209.165.201.1`).
{% endhint %}

The edge routers connects individual IPv6 sites and route traffic through the IPv4 infrastructure

Since the 6to4 automatic tunnelling defines the closed 2002::/16 prefix, allowing edge routers to establish automatic tunnel only to destination within the 2002::/16 prefix

If there is a traffic destined outside of the closed 6to4 2002::/16 prefix, all edge routers must use default route pointing to the dedicated relay router, which is responsible for sending traffic outside of 6to4 prefix space. . The relay must be assigned with a reserved IPv4 address 192.88.99.1 as defines in RFC 3068

#### Disadvantages

If IPv4 protocol 41 ("IPv6-in-IPv4 encapsulation") is filtered anywhere along the path, the tunnel will not work.

This method is not recommended and is obsolete since there might be a problems with readdressing when migrating to native IPv6

If the IPv4 address of the 6to4 edge router changes, the entire site /48 self-allocation also changes, and the 6to4 environment must be renumbered

6to4 tunnels are supported only on Cisco ISR Series routers and on Cisco ASR 1000 Series routers

![](<../.gitbook/assets/Unknown image (1032)>)

{% hint style="info" %}
An L3 switch can simulate the IPv4 infrastructure. VLAN 1 SVI is the gateway for the left router. VLAN 2 SVI is the gateway for the right router.
{% endhint %}

![](<../.gitbook/assets/Unknown image (1033)>)

The static route configures the router to send any 6to4 traffic through the Tunnel0 interface, where the #tunnel mode ipv6ip tells the edge router to extract the IPv4 address of the destination edge router from the 17 to 48bit part of the IPv6 destination address

{% hint style="info" %}
The tunnel interface can be configured as unnumbered, with link-local, or with its own subnet within the site prefix.
{% endhint %}

The relay router is configured with a static route to the closed 6to4 prefix via tunnel aswell and another route pointing to the IPv6 internet

| Left-edge router config ipv6 unicast-routing ! interface Loopback0 description IPv6 site ipv6 address 2002:5716:A701:1::/64 eui-64 ! interface Tunnel0 ipv6 unnumbered Loopback0 tunnel source GigabitEthernet3 tunnel mode ipv6ip 6to4 ! interface GigabitEthernet3 description IPv4 WAN ip address 87.22.167.1 255.255.255.252 ! ip route 0.0.0.0 0.0.0.0 GigabitEthernet3 87.22.167.2 ! ipv6 route 2002::/16 Tunnel0 ipv6 route ::/0 2002:c058:6301::1 // c058:6301 is the IPv4 of a relay in hex, which is set as a next-hop for traffic outside of 6to4 | Right-edge router config ipv6 unicast-routing ! interface Loopback0 description IPv6 site ipv6 address 2002:CB2B:8011:1::/64 eui-64 ! interface Tunnel0 ipv6 unnumbered Loopback0 tunnel source GigabitEthernet1 tunnel mode ipv6ip 6to4 ! interface GigabitEthernet1 description IPv4 WAN ip address 203.43.128.17 255.255.255.252 ! ip route 0.0.0.0 0.0.0.0 GigabitEthernet1 203.43.128.18 ! ipv6 route 2002::/16 Tunnel0 ipv6 route ::/0 2002:c0586301::1 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

{% hint style="info" %}
Some service providers may support customers using 6to4/6rd tunneling. In these environments, prefix lists may deny 6to4 prefixes that encode private RFC1918 IPv4 ranges. This prevents customers from using private IPv4 WAN addresses to initiate 6to4 tunnels.

Common blocked prefixes:

* `2002:a00::/24 le 128`
* `2002:ac10::/28 le 128`
* `2002:c0a8::/32 le 128`
{% endhint %}

#### IPv6 6RD (Rapid Deployment) Automatic Tunneling

6RD tunneling is a recommended solution for service providers that want to instantaneously offer IPv6 service to customers without migrating the core network.

The 6rd is similar to 6to4 tunneling, with the improvement over 6to4 automatic tunneling, that is the IPv6 prefix of the service provider is used and delegated to each CE edge router instead of the well-known reserved prefix used in 6to4. Format: 2001:\<ISP\_GUA>:\<CE\_IPv4>::/64

The relay router is called Border router and is maintained by the Service provider. Stateless 6to4 tunnels are used

![](<../.gitbook/assets/Unknown image (1034)>)

#### ISATAP (Intra-Site Automatic Tunnel Addressing Protocol) Tunneling

requires that the IPv6 nodes have an ISATAP address, which is an IPv6 address that is constructed from the IPv4 address of the ISATAP router.

When host sends a packet it encapsulates IPv6 packets in IPv4 packets and transmits them over the IPv4 network

The IPv4 packet header contains an IPv4 address of the ISATAP router. When the packet arrives at the ISATAP router, it is decapsulated and forwarded to the IPv6 destination

The first 64 bits are for the prefix like Global unicast, link-local addresses

0000:5EFe: this is a reserved UOI value which indicates that this is an ISATAP address.

the remaining 32 bits embed the IPv4 address, in hexadecimal.

![Ipv6 Isatap Prefix Format](<../.gitbook/assets/Unknown image (1035)>)

![](<../.gitbook/assets/Unknown image (1036)>)

#### Teredo

is a Microsoft transition technology that gives full IPv6 connectivity for IPv6-capable hosts that are on the IPv4 Internet but have no native connection to an IPv6 network. Unlike similar protocols such as 6to4, it can perform its function even from behind network address translation (NAT) devices such as home routers.

Teredo operates using a platform independent tunneling protocol that provides IPv6 (Internet Protocol version 6) connectivity by encapsulating IPv6 datagram packets within IPv4 User Datagram Protocol (UDP) packets. Teredo routes these datagrams on the IPv4 Internet and through NAT devices

[https://en.wikipedia.org/wiki/Teredo\_tunneling](https://en.wikipedia.org/wiki/Teredo_tunneling)

#### All IPv6 Automatic IPv4-Compatible Tunnels

| int Tunnel0                    |                                                                                                                                                                                                                             |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| tunnel mode ipv6ip auto-tunnel | -> not recommended either by Cisco nor by IETF/rfc7059                                                                                                                                                                      |
| tunnel mode ipv6ip 6to4        | -> for site-to-site/branch-to-branch Limitations: no IGP support, only static route or BGP                                                                                                                                  |
| tunnel mode ipv6ip 6rd         | builds upon the 6to4 tunneling mechanism, and is designed for ISPs -> not for Branch-to-Branch                                                                                                                              |
| tunnel mode ipv6ip isatap      | -> designed for intrasite tunneling (within a site/branch), still can be run between sites Limitations: no IPv6 multicast, OSPFv3, EIGRP: neighbors are formed manually with "neighbor" command or need to use static route |

#### IPv6 Deployment Best practices

The IPv6 migration strategy depends on many initial conditions, business plans and budgets.

Before deploying a dual-stack IPv6 model, it is important to conduct thorough inventory of all network devices, servers, applications, and services to assess their IPv6 compatibility, hardware requirements and recognize the connectivity utilization of the existing network.

Identify potential risks and challenges associated with the transition, such as compatibility issues, security concerns, and user impact.

Develop a detailed transition plan, including timelines, responsibilities, and resource allocation.

The address plan should mimic the existing IPv4 addressing

If public IP addressing allocation is owned, contact your address provider (ISP/RIR/LIR) to obtain an IPv6 PA address range.

For multihomed connections, request a PI address range from your address provider

Based on the provided address range, design and allocate the range to each segment of your network.

It is recommended to use the /64 prefix for each VLAN subnet interface. This prefix is required for SLAAC, DHCP forwarding and privacy extensions. Can I use a "smaller" (or "larger") block than /64? Yes! Whenever SLAAC is not used

Less than /64: There is no use cases where a site needs more addresses than /64 prefix provide, so this size is considered bad practice.

Greater than /64 - /128: /127 Can be used for point-to-point connections and /128 can be used for interfaces

Implement dual-stack by assigning IPv6 addresses to network interfaces and devices based on your addressing schemes

To streamline deployment, feature like IPv6 General Prefix can be used

Do not enable IPv6 on the end nodes unless your network supports it

Deploy IPv6 services in the same way as IPv4 services.

Test all applications and services such as DNS and DHCP for compatibility with IPv6

For DNS, the first option is to use the same FQDN for both protocols by setting two entries, A and AAAA. Or use a different FQDN with 6 in the URL name, for example www6.example.com, w6.example.com, ipv6.example.com

Implement security for IPv6

Configure firewalls with specific IPv6 rules to block malicious and unauthorized inbound and outbound traffic.

Disable or tightly control IPv6 extension headers, as they can be exploited by attackers.

Use ACLs to permit only necessary IPv6 traffic based on business security requirements.

Disable Router Advertisements (RAs) on public interfaces

Implement First hop security (FHS) on the access or distribution switches

Secure your routing protocols with authentication or encryption features

Implement IPsec tunnels for connections over public infrastructures such as Internet

Consider available monitoring tools to facilitate network management and troubleshooting

If IPv6 is confirmed to be functional and your business will no longer need to use IPv4 for active connectivity, plan to remove IPv4 configurations and disable IPv4 services and applications.

[https://denipv6.cz/media/filer\_public/d1/a8/d1a8477a-6b53-4c1c-b7d2-7ac860643e65/zajic.pdf](https://denipv6.cz/media/filer_public/d1/a8/d1a8477a-6b53-4c1c-b7d2-7ac860643e65/zajic.pdf)

#### Service Provider Strategies

There are two main strategies for phased service provider deployment of IPv6:

Provide an IPv6 service at the customer access level: Starting the deployment of IPv6 at the customer access level permits an IPv6 service to be offered immediately, without a major upgrade to the core infrastructure and without affecting current IPv4 services. This approach allows an evaluation of IPv6 products and services before complete network implementation and allows an assessment of the future demand for IPv6, without substantial investment early in the process.

Run IPv6 within the core infrastructure: At the end of the initial evaluation and assessment stage, as support for IPv6 within the routers (particularly IPv6 high-speed forwarding) improves and as network management systems fully embrace IPv6, the network infrastructure can be upgraded to support IPv6. This upgrade path can involve dual-stack routers or, as IPv6 traffic gradually becomes predominant, IPv6-only routers.

Many service providers have already deployed MPLS in their IPv4 backbone for various reasons

#### MPLS can be used to facilitate IPv6 integration

Native IPv6 MPLS

IPv6 provider edge router (6PE) over MPLS

IPv6 VPN provider edge (6VPE) over MPLS
