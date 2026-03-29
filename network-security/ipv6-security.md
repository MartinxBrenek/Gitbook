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

# IPv6 Security

### IPv6-specific threats

Endpoints automatically accept router advertisements (RAs) regardless of which device they come from. This poses a security risk because any device can send an RA and your endpoints will still receive it. An attacker can use a variety of attacks that can completely compromise network security.

#### Neighbor spoofing (rogue RA)

Attacker can send gratuitous RAs to announce themselves as gateway and redirect traffic to their machine or use it for MitM.

Attacker can also send so many RAs that it causes a denial of service (DOS) since hosts will be too busy with re-configuring IPv6 prefixes.

#### Neighbor cache poisoning (NA spoofing)

An attacker can manipulate neighbor cache entries on a target device by sending unsolicited NA stating that he is an owner of B address with a new MAC. It changes CAM table on the switch and neighbor mapping on node A. It also can hijack the session established between two different devices.

#### Denial of Address Initialization (DAD DoS)

An attacker can block address assignment by sending NA for target, simply saying it's my address.

Address cannot be used and victim or victims cannot configure IPv6 addresses and cannot communicate at all.

#### Fragmentation evasion (RA Guard bypass)

If an IPv6 packet is fragmented, the ICMPv6 message can be transported in second fragment. The switch is then not able to detect and block RA.

### IPv6 First Hop Security (FHS)

**IPv6 First Hop Security (FHS)** is a label for a group of security features that protect networks from these L2 attacks

Most FHS features are configured in a two-step: firstly you define a policy which describes the behavior of the feature, then you apply this policy to a VLAN or interface.

Note Another way, how to defend against hostile routers , is an ACL deployment on physical ports of the switch, however it turned out that this wouldn't block the RA at all - described in the fragmentation attacks section

#### Methods to prevent node-to-node Layer 2 communication

* Private VLANs (PVLAN) where nodes (isolated ports) can only contact the official router (promiscuous port)
* Link-local multicast (RA, DHCP request, etc.) sent only to the local legitimate router: Side effect: breaks DAD

Thus, as a protection against unwanted routers, it is recommended to deploy a special defense called RA guard

### Switch Integrated Security Features (SISF)

**Switch Integrated Security Features (SISF)** is a framework developed to optimize security in Layer 2 domains. It merges IP Device Tracking (IPDT) and certain IPv6 first-hop security (FHS) functionality such as RA and DHCPv6 guard

Note The terms “SISF” “device-tracking” and “SISF-based device-tracking” are used interchangeably in this document and refer to the same feature. Neither term is used to mean or should be confused with the legacy IPDT or IPv6 Snooping features.

The SISF infrastructure is built around the binding table. The binding table contains information about the hosts that are connected to the ports of a switch and the IP and MAC address of these hosts. This helps to create a physical map of all the hosts that are connected to a switch and secure both IPv4 and IPv6 networks

Binding table is populated by gleaning information from both IPv4 and IPv6 protocols such as ARP, ND, and DHCP packets

Preference depends on the source of info (Static, DHCP, ND etc.)

Preference: PSTATIC>PDHCPv6>PND , PDHCP>PARP

The SISF-based binding table is used to track association of IPv4 and IPv6 to MAC address with a VLAN number and it's corresponding port and preference, that indicates from which source (static/dhcp/nd) created given entry

The SISF infrastructure provides a unified database that is used by:

IPv6 FHS features: IPv6 Router Advertisement (RA) Guard, IPv6 DHCP Guard, Layer 2 DHCP Relay, IPv6 Duplicate Address Detection (DAD) Proxy, Flooding Suppression, IPv6 Source Guard, IPv6 Destination Guard, RA Throttler, and IPv6 Prefix Guard.

Features like Cisco TrustSec, IEEE 802.1X, Locator ID Separation Protocol (LISP), Ethernet VPN (EVPN), and Web Authentication, which act as clients for SISF.

#### Binding entry lifecycle

Each entry in Binding table have it's lifecycle. It maintains its state and confirms if an entry is still connected or not by sending periodic NS or ARP messages

If an entry is in REACHABLE state, it means the host (IP and MAC address) from which a control packet was received, is a verified and valid host

A reachable entry has a default lifetime of 5 minutes. You can also configure a duration. By configuring a reachable-lifetime, you specify how long a host can remain in a REACHABLE state, after the last incoming control packet from that host. Down entry means that the connecting interface is down - lifetime 24h

If the switch does not receive a polling response after three attempts, the entry changes to the STALE state

It remains in this state for 24 hours. If an event is detected during the stale lifetime, the state of the entry is changed back to REACHABLE.

If no response to the polling is received, the entry is DELETEd

![](<../.gitbook/assets/Unknown image (902)>)

Binding Table Recovery is a mechanism that ensures the recovery and preservation of the binding table data in the event of a device failure/reboot

Binding table and dynamic PACL entries are stored in flash and installed in TCAM of the line card #show dir

![](<../.gitbook/assets/Unknown image (903)>)

#### Binding table operation

When a host receives NA for an address, it checks binding table. If it is new entry, it installs it in the table

If it is known entry, it checks the anchor e.g. if it received on the same port

If yes, it refreshes its state, if not, it checks the source from which it received it

If the source have lesser preference (e.g NDP tries to overwrite static or dhcp entry), it evaluated as an address theft and drops packet

If it is from the same source received on a different port the switch checks whether the existing host is connected to the original port. If it is, it is evaluated as an address theft and packet is dropped.

If the entry source is higher than the existing one it replaces it

This scenario displays an inefficient use of the binding table, because each host is being validated multiple times, which does not make it more secure than if just one switch validates host. Secondly, entries for the same host in multiple binding tables could mean that the address count limit is reached sooner. After the limit is reached, any further entries are rejected and required entries may be missed this way.

![](<../.gitbook/assets/Unknown image (904)>)

By configuring device-role siwtch and trusted port on the trunk ports, the binding entries will be created only on switches where the host appears on an access port, and not creating entries for a host that appears over an uplink port or trusted port, each switch in the set-up validates and makes only the required entries on his ports, thus achieving an efficient distribution of the creation of binding table entries. Confirmed this in LAB

![](<../.gitbook/assets/Unknown image (905)>)

Note Cisco does not recommend to configure the device-role switch and trusted-port towards non-cisco switches or switches that does not support SISF.

Further, we recommended that you maintain a centralized binding table - on the distribution switch. When you do, all the binding entries for all the hosts connected to non-Cisco switches and switches that do not support the feature, are validated by the distribution switch and still secure your network. The figure below illustrates the same.

![](<../.gitbook/assets/Unknown image (906)>)

| device-tracking policy POLICY                                  | Note you can use [template interface](onenote:L2.one#Switching\&section-id={EE338BC5-FE62-4714-B561-D35D9358E092}\&page-id={C107D1F8-6395-4825-A10A-0E6128CC4AA3}\&object-id={FF82F6C4-4022-06B4-210E-8FF305AE04EB}\&F\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE) to apply device-tracking aswell                                                          |
| -------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| device-role node                                               | (default) will allow the creation of binding entries, inspect and drops RA and DHCP packets Gleaning available from all sources (ND, DHCPv6, ARP and DHCPv4) Suitable for most passive LAN nodes (PCs, IP phones, printers etc.)                                                                                                                                                    |
| device-role switch                                             | No entry is created for a host that appears over a trunk port, however binding entries are only created on an access port Since each individual switch inspects and validates connected hosts, there is no need to maintain it's host entries on other switches (as shown above in the screenshots of multi-switch setup's). It can also be configured on uplink port to the router |
| security-level guard                                           | (default) SISF extracts the IP and MAC address and drops any un-authorized messages with invalid IP and MAC address - applied on access ports connecting end-hosts glean SISF extracts the IP and MAC address and enters them into the binding table, without any verification inspect Gleans and validates messages                                                                |
| trusted-port                                                   | Disables the guard function - suitable for trunk links, since a switch sending the NDP or DHCP message already verified the validity of binding entry. Can be also configured on the access port, where legitimate DHCP server is connected                                                                                                                                         |
| prefix-glean                                                   | Enables learning of prefixes from either IPv6 Router Advertisements or from DHCP-PD. Probably used by prefix-guard                                                                                                                                                                                                                                                                  |
| Applying policy to vlan and interfaces                         |                                                                                                                                                                                                                                                                                                                                                                                     |
| vlan configuration 10 device-tracking                          | applies device-tracking default policy - parameters (role node level guard)                                                                                                                                                                                                                                                                                                         |
| int gi1/0/1 device-tracking attach-policy <>                   | then assign specific policy to individual trunk or trusted port that overwrites the vlan-level policy                                                                                                                                                                                                                                                                               |
| Optional                                                       |                                                                                                                                                                                                                                                                                                                                                                                     |
| limit address-count                                            | specifies the maximum IP's per port that can be added to the binding table                                                                                                                                                                                                                                                                                                          |
| device-tracking binding max-entries                            | limits the binding entries globally                                                                                                                                                                                                                                                                                                                                                 |
| device-tracking binding vlan <> \<ipv4/6 prefix>               | to create static entry in binding table                                                                                                                                                                                                                                                                                                                                             |
| device-tracking binding \[reachable-stale-down-lifetime <>]    | to adjust the lifetime of entries                                                                                                                                                                                                                                                                                                                                                   |
| tracking \[enable\|disable]                                    | doesn't do anything more - tested in LAB as well as destination/data-glean and other options                                                                                                                                                                                                                                                                                        |
| show device-tracking policy \[name \| vlan <> \| interface <>] | show device-tracking policies                                                                                                                                                                                                                                                                                                                                                       |
| show device-tracking database                                  |                                                                                                                                                                                                                                                                                                                                                                                     |
| show device-tracking \[events\|messages]                       | to display what the switch does with individual messages on a port                                                                                                                                                                                                                                                                                                                  |

Restriction it is recommended to use either manual or auto-generated SVI IP (on vlan interface) with #ipv6 enable command

This address is used as the source IP address of the SISF probe, thus preventing the duplicate IP address issue with windows

if no link-local or SVI is configured, you must configure (config)#device-tracking tracking auto-source to avoid using 0.0.0.0 as source IP address in ARP/NDP probes, which would confuse windows hosts during their DHCP and DAD process

### IPv6 RA Guard (Router Advertisement Guard)

**IPv6 RA Guard (Router Advertisement Guard)** validates the content of RAs and redirect messages, and blocks unauthorized RA messages from untrusted interfaces

Requires an extension of ASIC features, not available on the older HW platform:

Cat2960 – support for Cat2960-S/SF and Cat2960-X/XR

Cat3560/3750 - support for Cat3560E/3750E and Cat3560X/3750X

Cat4500 – support from supervisor Sup6

Full support on newer switch platforms

Cat3650/3850, Cat4500-X and Cat9000 family

Host role (default) expects that trusted ports are connected to hosts that do not send any RA or Redirect messages and drops them if received on a given port - allows only RS

Router role configured on port connected to legitimate router, allowing trusted ports to send RA and Router Redirect messages. Doesn't work for IPv6 tunneled traffic

Switch role all received RA are trusted and flooded to synchronize states, all received RS also forwarded

Monitor role receives valid and rogue RA, and all RS, cannot send RA/RS

#### Roles summary

* **Host role (default)**: expects that trusted ports are connected to hosts that do not send any RA or Redirect messages and drops them if received on a given port - allows only RS
* **Router role**: configured on port connected to legitimate router, allowing trusted ports to send RA and Router Redirect messages. Doesn't work for IPv6 tunneled traffic
* **Switch role**: all received RA are trusted and flooded to synchronize states, all received RS also forwarded
* **Monitor role**: receives valid and rogue RA, and all RS, cannot send RA/RS

| (config-if)#ipv6 nd raguard                                                                                                                                                                                                      | allows only RS on an interface - suitable for access ports                                                                                                                           |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| (config)#ipv6 nd raguard policy (config)#device-role \<host \| router \| switch>(config-if/vlan)#ipv6 nd raguard attach-policy \[NAME]                                                                                           | RA guard applied as policy apply policy to an interface or vlan                                                                                                                      |
| ipv6 nd raguard policy HOST device-role host ! ipv6 nd raguard policy ROUTER device-role router ! vlan configuration 1 ipv6 nd raguard attach-policy HOST ! interface GigabitEthernet1/0/10 ipv6 nd raguard attach-policy ROUTER | Recommended Example we set the entire vlan as a host and then exclude router-facing interface by setting it as router role, since interface config takes precedence over VLAN config |
| show ipv6 nd raguard \[policy \| interface]                                                                                                                                                                                      |                                                                                                                                                                                      |

### DHCPv6 Guard

**DHCPv6 Guard** is the IPv6 version of DHCP snooping. It drops DHCP Offer/ACK messages from untrusted DHCP servers.

Server Allows all DHCP messages

Clients Allows DHCP Solicitation and Request packets, blocks DHCP Advertise and Reply packets

#### Roles summary

* **Server**: allows all DHCP messages
* **Client**: allows DHCP Solicitation/Request, blocks DHCP Advertise/Reply

| Switch(config)# ipv6 dhcp guard policy \[NAME]     | When setting the device role to server, additional configuration parameters appear: match: Used in conjunction with access-lists to specify allowed advertised prefixes and/or source address of DHCP messages preference: DHCPv6 preference option min/max values can be defined so that DHCP messages will be filtered out if they exceed the min/max value |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-nd-raguard)# device-role                   |                                                                                                                                                                                                                                                                                                                                                               |
| (config-if)# ipv6 dhcp guard attach-policy \[NAME] |                                                                                                                                                                                                                                                                                                                                                               |
| show ipv6 dhcp guard policy                        |                                                                                                                                                                                                                                                                                                                                                               |

### IPv6 Source Guard

**IPv6 Source Guard** is similar to the IPv4 Source Guard. It blocks Layer 2 ingress traffic from unknown sources without a SISF binding entry.

Initially only allows NDP solicitation and DHCP discover packets before binding entry is created

| ipv6 source-guard policy \[NAME]                     |                                                                                                                                                                                                                                                                                                                                                                             |
| ---------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| validate address                                     |                                                                                                                                                                                                                                                                                                                                                                             |
| validate prefix                                      | enables the IPv6 Prefix Guard // the default is address which is essentially the IPv6 source guard feature monitors ingress RA messages and compare the advertised prefixes with SISF-based binding table entries, and blocks any traffic sourced from address outside this range, preventing further propagation to the network. It works within IPv6 source-guard feature |
| (config-if)# ipv6 source-guard attach-policy \[NAME] |                                                                                                                                                                                                                                                                                                                                                                             |
| show ipv6 source-guard policy \[NAME]                |                                                                                                                                                                                                                                                                                                                                                                             |

### IPv6 Neighbor Discovery multicast suppression

The IPv6 Neighbor Discovery (ND) multicast suppress feature stops the ND multicast Neighbor Solicit (NS) messages by dropping them (and responding to solicitations on behalf of the targets) or by converting them into unicast traffic. This feature reduces the amount of control traffic necessary for proper link operations.

When an address is inserted into the binding table, an address resolution request sent to a multicast address is intercepted, and the device either responds on behalf of the address owner or converts the request into a unicast message and forwards it to its destination.

### IPv6 Neighbor Discovery proxy

IPv6 neighbor discovery proxy restricts IPv6 hosts within a VLAN from communicating directly with each other and allows them to communicate only via the gateway. A device operating as an IPv6 neighbor discovery proxy responds to packets on behalf of the target.

IPv6 neighbor discovery proxy operations are achieved using the following implementations:

#### IPv6 routing proxy

A device operating as an IPv6 routing proxy listens to all neighbor discovery proxy messages sent on the link and responds unconditionally to neighbor solicitation lookup and neighbor-unreachability-detection messages with neighbor advertisement (setting the SVI MAC address in the TLLA option) on behalf of the destination hosts to attract the traffic to itself.

IPv6 DAD proxy feature responds to DAD queries on behalf of a node that owns the queried address. IPv6 DAD proxy depends on a device tracking database to ensure uniqueness of IPv6 addresses.

When receiving a DAD request from a host for a target, the DAD proxy performs a lookup into the binding table, and if the lookup returns a location, it sends an neighbor solicitation neighbor-unreachability-detection message to verify that the target is still alive.

If the target replies to the neighbor-unreachability-detection message, the DAD proxy sends back an neighbor advertisement to the host (setting the SVI MAC address in the TLLA option).

If the device does not respond to the neighbor-unreachability-detection message, the DAD proxy does not send any response to DAD request.

### IPv6 Destination Guard

**IPv6 Destination Guard** configures the last-hop router to drop NDP packets without a binding entry. This helps prevent prefix-scanning attacks and protects the ND cache.

It performs resolutions only for existing binding entries

Note However this feature is not widely used, because last-hop routers usually have ACL’s in place and the resolutions are limited only to communicate with other network devices rather than performing and receiving resolutions from hosts

| ipv6 destination-guard policy \[NAME]                          |   |
| -------------------------------------------------------------- | - |
| enforcement {always \| stressed}                               |   |
| (config-if)ipv6 destination-guard attach-policy \[policy-name] |   |
| show ipv6 destination-guard policy \[policy-name]              |   |

### Secure Neighbor Discovery (SeND) and Cryptographically Generated Addresses (CGA)

**Secure Neighbor Discovery (SeND)** and **Cryptographically Generated Addresses (CGA)** are ND extensions that introduce PKI into the neighbor discovery process.

It allows hosts to obtain and verify certificates from trusted authorities, ensuring the authenticity and integrity of neighbor advertisements and solicitations to prevent neighbor cache poisoning attacks. SeND and CGA are recently released RFCs, and implementations are uncommon at this time. Currently there is limited support due to its complexity and cost, current operating systems and network devices do not fully support the SeND protocol - Windows does not and Linux does on most distributions. In the future, however, subnet-local security will be enhanced via these mechanisms, and high-security organizations should consider them for deployment.

CGAs are IPv6 addresses that are generated from the cryptographic hash of a public key and auxiliary parameters. This approach provides a method for securely associating a cryptographic public key with an IPv6 address in the SeND protocol.

The node that generates a CGA must first obtain a Rivest, Shamir, and Adleman (RSA) key pair. (SeND uses an RSA public/private key pair.) The node then computes the interface identifier part (which is the right-most 64 bits) and appends the result to the prefix to form the CGA.

CGA generation is a one-time event. A valid CGA cannot be spoofed, and the parameters that are associated to a particular CGA are reused. The message must be signed with the private key that matches the public key that is used for CGA generation, which only the address owner will have.

A user cannot replay the complete SeND message (including the CGA address, CGA parameters, and CGA signature) because the signature has only a limited lifetime.

[https://www.cisco.com/en/US/docs/ios-xml/ios/sec\_data\_acl/configuration/15-2mt/ip6-send.html#GUID-AA022818-146A-4B92-912C-622166402E82:\~:text=hosts%20and%20devices.-,SeND%20Protocol,defines%20the%20mechanisms%20defined%20in%20the%20following%20sections%20for%20securing%20ND,-%3A](https://www.cisco.com/en/US/docs/ios-xml/ios/sec_data_acl/configuration/15-2mt/ip6-send.html#GUID-AA022818-146A-4B92-912C-622166402E82)

The only safe way to prevent unauthorized entry is to authenticate network access by encrypting and authenticating all traffic with IPsec, by authenticating devices with 802.1x, or by implementing SeND.

| ipv6 nd secured sec-level \[minimum value]                                                   | This command configures the SeND security level:                                              |
| -------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| ipv6 nd secured key-length \[\[minimum \| maximum] value]                                    | This command configures the SeND key-length options:                                          |
| ipv6 cga rsakeypair key-label                                                                | This command specifies which RSA key pair should be used on a specified interface:            |
| ipv6 address {ipv6-address/prefix-length \[cga] \|prefix-name sub-bits/prefix-length \[cga]} | The CGA link-local addresses must be configured by using the ipv6 address link-local command. |
| interface FastEthernet0/0 ipv6 cga rsakeypair CGA\_keys\ ipv6 address 2001:db8:AE::/64       |                                                                                               |

### Other IPv6 security considerations

| ipv6 access-list from\_outside deny ipv6 FF00::/8 any log permit ipv6 any any                         | Multicast addresses should never appear in a source field, so filtering such packets poses no harm.                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ----------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Source Routing Attack                                                                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ipv6 access-list from\_outside deny ipv6 any any routing log permit ipv6 any any no ipv6 source-route | Attackers can use source routing to change the path of the traffic, by specifying intermediate hops in the IP header. IPv4 uses options to specify hops; IPv6 uses routing headers. Like in IPv4, processing of routing headers in IPv6 can be stopped by configuring the no ipv6 source-route command. This configuration does not prevent end hosts from processing routing headers. Those hosts must be protected locally. As an alternative, you can filter all packets with a routing header, if this functionality is not required in your network. |

Peer-to-peer applications will be deployed from what are considered to be internal nodes to external, or Internet-based, nodes. Early in IPv6 deployment, security engineers are likely to try to refuse requests to allow peer-to-peer applications, but will be unable to do so as peer-to-peer applications become central to enterprise efforts. Many of these applications will be IPsec-protected (Encapsulating Security Payload \[ESP]), and currently used edge devices, such as edge firewalls and network-based intrusion detection systems (IDSs), will be ineffectual. Inspection at the peer itself will be the only effective protection.

All hosts that may be peers will need DNS entries, mapping names to addresses. As more nodes need to engage in peer-to-peer sessions, the number of hosts in the DNS will rise dramatically. Dynamic DNS (DDNS) will likely be needed, and dictionary attacks against node names might be effective. Hiding a host location will be difficult, and protection at the node level will be needed.

![](<../.gitbook/assets/Unknown image (907)>)

There are various ways to accomplish topology hiding:

Use universal LAN addressing and proxy servers to provide Internet-based services to internal nodes. This approach works well as long as end-to-end services are not needed.

Use MultiChannel Interface Processor version 6 (MIPv6). If the enterprise nodes are always mobile, then they can be associated with an IPv6 home agent on a known subnet and can roam internally onto different internal, cloaked subnets. Viewed from the outside, all hosts appear to be on a single or small set of subnets, regardless of the true number of active networks. Route optimization must not be used.

### ICMPv6 considerations

**ICMPv6 considerations** are important to the health of an IPv6 network. ICMPv6 must be allowed to flow more broadly than it is in IPv4 networks.

ICMPv6 is used for many crucial functions in IPv6, including the PMTUD function. For nodes that are involved in peer-to-peer sessions across the enterprise edge firewall, being able to discover the appropriate packet size and learning about changes in the path or unreachable nodes is crucial to providing the user with a high-quality experience. The traditional approach to ICMP traffic management—blockading all of it at the network edge—will not be appropriate.

Careful management of specific ICMPv6 message type and code combinations, unidirectional across the network border and at internal nodes that are protected by individual packet filters, will be common in the future.

Rate limiting (allowing a prescribed number of messages of a certain type across a point in the network) must be implemented

### Fragmentation attacks

#### Upper-layer info not in the first fragment

Upper Layer Information is not in the first fragment – many unneeded tiny fragments, which results in upper layer protocol header not being in the first fragment and detection tools may not detect attack​, for example switch cannot detect and block RA

IPv6 ACLs, particularly on hardware-based switches, often only inspect the first fragment of a fragmented packet.

If the RA information resides in the second fragment, the switch doesn't "see" enough information to match the ACL rule, so the RA is allowed through.

The limitation arises because inspecting deeper fragments requires additional processing resources. Many hardware-based network devices prioritize performance and don't perform deep inspection on fragmented packets.

Mitigation​:

* Firewall should filter packets that do not contain upper layer protocol in the first fragment
* Filter out packets having two and more fragmentation headers
* Dropping all fragments (except last one) with size less than 1280B

On Cisco IOS

| deny ipv6 any any fragments |   |
| --------------------------- | - |

#### Fragments inside fragments (multiple Fragment Headers)

Fragments inside fragments – IPv6 packet contains several fragmentation headers in the same packet. Such packet may bypass security checks​. Attackers can hide their activities by breaking malicious payloads into fragments and layering multiple Fragment Headers. This makes it harder for security systems to detect patterns or signatures

Some network devices or security systems may crash or behave unpredictably when encountering packets violating the IPv6 standard, such as those with multiple Fragment Headers

Mitigation​: Filter out packets having two and more fragmentation headers

#### IPv6 atomic fragments

IPv6 atomic fragments are IPv6 packets that contain a Fragment Header with the fragment offset set to 0 and the M flag set to 0. A packet is fragmented into 1 fragment – it is not fragmented packet in fact

Caused by spoofed ICMPv6 PTB message with MTU<1280B

This allows for using fragmentation-based DoS attack on non-fragmented traffic

Process atomic fragments separately without reassembly and other steps (RFC6946)

As specified in Section 5 of \[RFC2460], when a host receives an ICMPv6 "Packet Too Big" message advertising a "Next-Hop MTU" smaller than 1280 (the minimum IPv6 MTU), it is not required to reduce the assumed Path-MTU, but must simply include a Fragment Header in all subsequent packets sent to that destination.

Not sending last fragment

Attacker may not send the last fragment resulting in memory consumption (waiting for the last packet that never comes)

Mitigation

Set a timeout (60 seconds in IPv6 standard)

### Network scanning

#### Goals

Attacker scans the networks to find:

Active hosts

Active services

#### What an attacker needs

To find a node, one must have:

* Network prefix (similar to network part of an IPv4)
* Interface ID (the actual node address in LAN)

#### Why IPv6 scanning is harder

In IPv4, most subnets can have between 254 and 65534 possible hosts.

In IPv6, a /64 subnet has approx. 2\*10^19 possible hosts.

18,446,744,073,709,551,616 IPv6 addresses / (10,000,000 IPv6 addresses/second \* 60 sec/min \* 60 min/hr. \* 24 hr./day \* 365 days/yr.) = 58,494 years

Once we know the network prefix, we can guess the interface ID

#### DoS angle

Router or firewall keeps state table entries for nonexistent hosts until they time out

#### Guessing the IID

**EUI-64 (Extended Unique Identifier)**

A special address format that consist of MAC address plus 0xfffe

24 higher bits of EUI-64 is vendor OUI, 24 lower bits (IID) is much easier to guess

**Trivial patterns**

Many addresses include lot of bytes set to 0 (2001::3)

**IPv4-based IIDs**

An IPv4 is embedded into the IPv6 address (2001::192:168:0:14)
