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

# OSPFv3

### Overview

OSPF Version 3 is a multiprotocol implementation of OSPF that supports IP version 6 (IPv6) in addition to IP version 4 (IPv4).

OSPF Version 3 (OSPFv3) expands on OSPF version 2 (OSPFv2), providing support for IPv6 routing prefixes.

OSPFv3 packets are transported directly in the IPv6 header without encapsulation in a transport-layer protocol like TCP or UDP

The Next Header value in the IPv6 header is set to 89 to indicate an OSPF packet. This is the same as OSPFv2, which uses Protocol 89 in the IPv4 header

Same message types (Hello, DD, LSU, LSR, LSACK)

Same neighbor detection and binding mechanisms (All OSPF Routers FF02::5, All OSPF DRs FF02::6)

Same mechanisms for LSA flooding and aging

Same metric – interface cost

Multi-area network design with Area Border Routers (ABRs) that segment the network

Same area types – stub, NSSA

Same interface types (P2P, P2MP, Broadcast, NBMA, Virtual)

### OSPFv3 versions

**OSPFv3 RFC 2740** – legacy version that supports IPv6 only

**OSPFv3 RFC 5340** – multiprotocol version that supports both address families (AFIs): IPv4 and IPv6

Most recent and recommended OSPFv3 version and backward compatible with RFC 2740

Each address family in OSPFv3 operates independently

They run separate SPF and maintain separate LSDB

**Router ID**: a 32-bit value that must be configured (written in IPv4 format)

OSPFv3 requires an IPv6 address on each OSPF interface because OSPFv3 uses IPv6 as the transport

`ipv6 unicast-routing` must be enabled and an IPv6 address must be assigned to the interface, even if you only run the IPv4 AFI

Link-local addresses are used to establish and maintain adjacencies

Regardless of assigned prefixes, two devices can communicate using link-local addresses, therefore OSPFv3 is running per link instead of per IP prefix

OSPFv3 is enabled per-link (no network command) identifying which networks (prefixes) are attached to that link (Multiple IPv6 prefixes can be assigned to the same link)

### OSPFv3 packet header

New Instance ID field in OSPF packet header

Allows running multiple instances per link

Useful for traffic separation, multiple areas per link

OSPFv3 uses native functionality offered by IPv6:

**IPsec AH** for authentication and integrity check

**IPsec ESP** for payload encryption

Security policy definition on the router is mandatory:

Key

Security parameter index (SPI) value

Authtype and Authentication field in the OSPF packet header have been removed to prevent undesired adjacencies and rogue routes to be inserted into OSPF

![](<../.gitbook/assets/Unknown image (700)>)

### OSPFv3 LSA enhancements

Two LSAs have been renamed:

Interarea Prefix LSAs (Type 3)

Interarea Router LSAs (Type 4)

Two new LSAs have been added to OSPFv3:

Link LSAs (Type 8)

Intra-Area Prefix LSAs (Type 9)

LSAs now have a flooding scope:

**Link-local**: flood to all routers on the link

**Area**: flood to all routers within an OSPF area

**Autonomous system**: flood to all routers within the entire OSPF autonomous system

LSA Type 1 and LSA Type 2 contain only 32-bit identifiers

They do not contain addresses

Changing the link addresses between two routers no longer triggers an SPF run

Handling and forwarding of unknown LSAs is supported to handle future OSPF extensions

In OSPFv2, unknown LSAs were discarded

**U-bit set**: if an LSA has an unknown type code and the U-bit is `1` (store + flood), the router floods it as if understood. This keeps newer LSA types compatible with older routers.

**U-bit not set**: if the U-bit is `0`, the router may discard the LSA (platform/implementation dependent).

The U-bit allows for the introduction of new LSA types without disrupting existing OSPFv3 networks. Routers can still propagate these unknown LSAs, ensuring compatibility with future advancements.

This approach allows for selective handling of unknown LSAs. Routers can be configured to discard specific unknown types if they are not relevant to the network or store and flood them for potential future use.

| **LSA name**          | **LSA #** |
| --------------------- | --------- |
| Router-LSA            | 1         |
| Network-LSA           | 2         |
| Inter-Area-Prefix-LSA | 3         |
| Inter-Area-Router-LSA | 4         |
| AS-External-LSA       | 5         |
| NSSA-LSA              | 7         |
| Link-LSA              | 8         |
| Intra-Area-Prefix-LSA | 9         |

#### LSA type field

The new LSA Type is a four-digit hex number, the final digit is what we’d usually refer to as the LSA Type. For example, when the final digit is a 1, we have a Router LSA. When it’s a 9, we have an Intra-Area Prefix LSA. The first digit in the hex number tells us how far within our network the advertisement will be flooded – the “flooding scope”

0 = LINK LOCAL ONLY

2 = AREA ONLY

4 = ENTIRE AUTONOMOUS SYSTEM

| OSPFv3 LSA TYPE | LSA NAME               | OSPFv2 LSA TYPE | LSA NAME               |
| --------------- | ---------------------- | --------------- | ---------------------- |
| 0x2001          | Router LSA             | 1               | Router LSA             |
| 0x2002          | Network LSA            | 2               | Network LSA            |
| 0x2003          | Inter-Area Prefix LSA  | 3               | Summary LSA            |
| 0x2004          | Inter-Area Router LSA  | 4               | ASBR Summary LSA       |
| 0x4005          | AS-External LSA        | 5               | AS-External LSA        |
| 0x2006          | Multicast LSA          | 6               | Multicast LSA          |
| 0x2007          | Not-So-Stubby-Area LSA | 7               | Not-So-Stubby-Area LSA |
| 0x0008          | Link LSA               |                 |                        |
| 0x2009          | Intra-Area Prefix LSA  |                 |                        |

#### Type 1: Router LSA

Describes a router (RID) and link interface type:

1 – point to point to the neighbor

2 – transit network

3 – reserved

4 – virtual link

Flooding scope: **area**

Contains no address information (only router IDs as an address-independent way of referring to a neighbor object)

Address information is included in a separate Type 8 LSA and Type 9 LSA for each of it’s links with the IPv6 prefix information Address information is included in a separate Type 8 LSA and Type 9 LSA for each of its links with the IPv6 prefix information

**Bit V**: set if the router terminates a virtual link

**Bit E**: set if the router is an ASBR

**Bit B**: set if the router is an ABR

**Bit x**: historical MOSPF bit; not used

**Bit Nt**: set if the router is an NSSA translator (ABR translates Type 7 to Type 5)

![](<../.gitbook/assets/Unknown image (701)>)

#### Type 2: Network LSA

Describes all router IDs of a multi-access network

Generated by the DR and advertised within the area to which the DR belongs

Contains no address information (only router IDs as an address-independent way of referring to a neighbor object)

(comparable to TLOCs in SD-WAN)

![](<../.gitbook/assets/Unknown image (702)>)

#### Type 3: Inter-Area Prefix LSA

Renamed Summary LSA (Type 3)

OSPFv3 LSA Type 3 is generated by an ABR to advertise IPv6 prefixes from one area to other connected areas.

![](<../.gitbook/assets/Unknown image (703)>)

#### Type 4: Inter-Area Router LSA

Renamed ASBR Summary Router LSA

Originated by ABRs to describe routes to ASBRs in other areas, and are advertised to all related areas excluding those that the ASBRs belong to

![](<../.gitbook/assets/Unknown image (704)>)

#### Type 5: External LSA

AS-external-LSAs are originated by ASBRs to describe external routes

**Bit E** – metric type (0 - E1, 1 - E2)

**Bit F** – contains the forwarding address

**Bit T** – contains the tag

![](<../.gitbook/assets/Unknown image (705)>)

#### Type 7: NSSA External LSA

NSSA-LSAs are originated by ASBRs within an NSSA and describe routes to destinations external to the AS.

#### Type 8: Link LSA

A router originates a separate Link LSA for each attached link describing the IPv6 prefix of the link and the link-local address

e.g Global unicast address configured on the link with OSPF neighbor

Link LSAs have link-local flooding scope

In addition, they allow the router to assert a collection of option bits to associate with the network LSA that will be originated for the link

{% hint style="info" %}
The prefix is configured on the link to the OSPF neighbor.
{% endhint %}

![](<../.gitbook/assets/Unknown image (706)>)

#### Type 9: Intra-Area Prefix LSA

A router generates intra-area prefix LSAs for each of its connected networks

Each with a unique link-state ID

Refers to LSA Type 1 or Type 2, supplementing the address information

It contains:

The IPv6 prefix information of the advertising router

The referenced Link state ID which describes its association to either the router LSA or network LSA (from DR)

It has area flooding scope

![](<../.gitbook/assets/Unknown image (707)>)

### Configuration (IPv6-only)

To configure simple OSPFv3 configure router ospf process, RID, and assign an interface to OSPF

| (config-if)# ipv6 ospf area <>                                                                                                          | enables IPv6 OSPFv3 under an interface                                                                 |
| --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| ipv6 router ospf                                                                                                                        |                                                                                                        |
| (config-router)# router-id 1.1.1.1                                                                                                      | Router ID must always be manually assigned (in IPv4 format). RID is set to 0.0.0.0 when not configured |
| (config-router)# area range 2001:db8:0:0::/48                                                                                           | summarization on ABR                                                                                   |
| (config-router)# redistribute \<ospf \| eigrp \| bgp \| connected> metric <> route-map <>                                               | ASBR external network injection                                                                        |
| (config-router)# redistribute maximum-prefix \[value]                                                                                   | Sets a maximum number of IPv6 prefixes that are allowed to be redistributed                            |
| Verification                                                                                                                            |                                                                                                        |
| clear ipv6 ospf \[process-id] {process \| force-spf \| redistribution \| counters \[neighbor \[neighbor-interface]]}                    | to clear process                                                                                       |
| show ipv6 route \[ospfv3]                                                                                                               | show ipv6 ospf                                                                                         |
| show ipv6 ospf neighbor \[detail]                                                                                                       |                                                                                                        |
| show ipv6 ospf database                                                                                                                 |                                                                                                        |
| debug ipv6 ospf \[ adj \| hello \| spf \| flooding \| events \| lsa-generation \| database-timer \| packets \| retransmission \| tree ] |                                                                                                        |

### Configuration (multiprotocol IPv4/IPv6)

This enables to configure two concurrent IPv4 and IPv6 processes under a single interface

| (config-if)# ospfv3 \<ipv6 \| ipv4>                                                         | enables OSPFv3 under interface                                                                                                                                  |
| ------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config)# router ospfv3                                                                     |                                                                                                                                                                 |
| (config-router)# address-family ipv6 unicast                                                |                                                                                                                                                                 |
| (config-router-af)# default-information originate \[always] \[metric 100] \[metric-type 2]  |                                                                                                                                                                 |
| Verification                                                                                |                                                                                                                                                                 |
| show ospfv3 \[ipv4 \| ipv6] \[rib \| interface \| neighbor \| \[database \| border-routers] | options same as in OSPFv2 - to show each LSA type as well as adv router etc.. You can specify no AFI and it will show you output for both AFI > show ospfv3 ... |
| show ospfv3 ipv6 summary-prefix                                                             |                                                                                                                                                                 |
| clear ospfv3 \[force-spf \| process \| redistribution ]                                     |                                                                                                                                                                 |

### Authentication and encryption

The OSPFv3 leverages the IPv6 AH and ESP IPSec extension headers to authenticate and encrypt OSPF packets

| Device(config-if)# ospfv3 authentication md5 0 <>OR R1(config-if)# ipv6 ospf authentication ipsec spi 256-4294967295 \[{md5 \| sha1}] \[0\|7] key \| null | Specifies the authentication type for an interface. The SPI is used to determine the security parameter index. This is used to identify several IPsec sessions between the same pair of hosts and does not directly apply to OSPFv3; this is required for the security policy to be functional                            |
| --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Device(config-router)# area 1 authentication ipsec spi 678 md5 <>                                                                                         | Enables authentication in an OSPFv3 area. To make all routers in a given area authenticate routing updates, you can configure area-wide authentication. This is useful if you have several routers on a broadcast-type link (such as Ethernet), and you do not want to define authentication parameters for every router. |
| Device(config-if)# ospfv3 encryption ipsec spi 1001 esp null md5 0 <>OR Device(config-if)# ipv6 ospf encryption ipsec spi 1001 esp null sha1 <>           | Specifies the encryption type for the interface.                                                                                                                                                                                                                                                                          |
| Device(config-router)# area 1 encryption ipsec spi 500 esp null md5 <>                                                                                    | Enables encryption in an OSPFv3 area.                                                                                                                                                                                                                                                                                     |

If you want to decrypt the ESP header in the wireshark and you have the corresponding key (configured in the OSPFv3 config):

![](<../.gitbook/assets/Unknown image (708)>)

#### Example

| hostname R1 interface FastEthernet0/0 mac-address 0011.1111.1111 ipv6 address 2001:DB8:1212::1/64 ipv6 ospf 1 area 0 ipv6 ospf encryption ipsec spi 256 esp 3des 24E692732D80FAC4F6DC2B9ABFB73678EF660BAB12345678 sha1 24E692732D80FAC4F6DC2B9ABFB73678EF660BAB hostname R2 interface FastEthernet0/0 mac-address 0022.2222.2222 ipv6 address 2001:DB8:1212::2/64 ipv6 ospf 1 area 0 ipv6 ospf encryption ipsec spi 256 esp 3des 24E692732D80FAC4F6DC2B9ABFB73678EF660BAB12345678 sha1 24E692732D80FAC4F6DC2B9ABFB73678EF660BAB |   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

#### Virtual link authentication and encryption

| Device(config-router)# area 1 virtual-link 10.0.0.1 authentication ipsec spi 940 md5 <>Device(config-router)# area 1 virtual-link 10.1.0.1 hello-interval 2 dead-interval 10 encryption ipsec spi 3944 esp null sha1 <> | Enables authentication for virtual links in an OSPFv3 area. Enables encryption for virtual links in the OSPFv3 area. |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
