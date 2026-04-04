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

# NAT

### Overview

**Network Address Translation (NAT)**

configures a network device such as router or a firewall to modify the source or a destination IP address in a packet header

Every established connection of a NAT router has its own NAT session. All depending connection information (addresses, ports and timeouts) are stored in a NAT table

Based on this stored information, the router can send return packets back to the source client. After a NAT session has finished or expired, the entry on the NAT table will be removed

The maximum amount of concurrent sessions depends on the platform (hardware/software).

### NAT use cases

NAT can also be used when there is an addressing overlap between two private networks. An example of this implementation would be when two companies merge and they were both using the same private address range. In this case, NAT can be used to translate one intranet's private addresses into another private range, avoiding an addressing conflict and enabling devices from one intranet to connect to devices on the other intranet. Therefore, NAT is not implemented only for translations between private and public IPv4 address spaces, but it can also be used for generic translations between any two different IPv4 address spaces.

### NAT types

**Source NAT (SNAT)** translates the source IP address of a packet, typically used for outgoing traffic.

**Destination NAT (DNAT)** translates the destination IP address of a packet, typically used for incoming traffic from the outside interface.

DNAT can also be used as PAT to translate destination port number to a different port, so that the packet is redirected - port forwarding

Note DNAT that changes the destination IP address of outgoing packet from the inside is not common and not configurable

**NAT44** involves translation of an IPv4 address to another IPv4 address.

It includes static NAT, dynamic NAT, and PAT.

![](<../.gitbook/assets/Unknown image (1485)>)

### NAT address terminology

**Inside local** address of the host in the private network

**Inside global** public IP address that represents one or more inside local host IP addresses to the outside

**Outside local** IP address of an outside host as it appears to the inside network

**Outside global** public IP address of the destination host on the outside network

Note When a packet comes from the outside interface, the NAT (Network Address Translation) is performed first before any further actions or decisions by the router. This is because the packet first has to have its destination IP address translated to an internal (private) IP address that corresponds to the actual device within the local network.

Once the NAT translation is applied, the router can then determine the appropriate next steps based on the translated IP address. This may include checking access control lists (ACLs), routing the packet to the correct interface, or applying other policies. The sequence ensures that the packet is directed accurately within the network, as the router would otherwise have no way of knowing which internal device the packet is intended for without first translating the public IP address to a private one.

This ordering is essential for the router to properly process incoming connections and maintain network security and routing integrity

### Static NAT

**Static NAT** maps a local IPv4 address to a global IPv4 address (one to one)

Note the static translation has no timeouts and is always present in NAT table until removed - as shown below

This allows traffic coming from outside to be always accepted by the router without reuquiring inside source address to initiate traffic to create an entry in the NAT table

Static NAT is usually used when a company has a server that must be always reachable, from both inside and outside networks.

The server's local IPv4 address will always be translated to the known global IPv4 address. This fact also implies that one global address cannot be assigned to any other device.

The mapping includes local to global mapping for both inside and outside addresses. When only inside address translation is performed, outside local and outside global address are the same. In an outbound packet (a packet leaving an inside network and going to an outside network), the inside address is present in the source address IPv4 header field, and the outside address is present in the destination address IPv4 header field.

Config steps

Specify inside and outside interfaces. You must instruct the border device on where to expect the inside traffic that needs to be translated (inside interface) and where to inspect outside traffic (outside interface) that needs to be translated. Inside/outside interface specification is required regardless of whether you are configuring inside only NAT or outside only NAT.

You can specify more than one inside interface.

Specify local addresses that need to be translated. NAT might not be performed for all inside segments and you have to specify exactly which local addresses require translation.

Specify global addresses available for translations.

Specify NAT type using ip nat inside source command. The syntax of the command is different for different NAT types.

| (config)# ip nat inside source static 192.168.0.254 209.165.201.5                                                                                                                                                                                                                                                             | Translates the source IP address of packets that travel from inside to outside.                                                                                                                                                                                                                                                                                                                      |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config)# ip nat outside source static 198.214.92.113 192.168.183.1                                                                                                                                                                                                                                                           | Translates the source IP address of packets that travel from outside to inside.                                                                                                                                                                                                                                                                                                                      |
| Router(config)# interface Gi0/1 Router(config-if)# description LAN Router(config-if)# ip address 192.168.0.1 255.255.255.0 Router(config-if)# ip nat inside Router(config-if)# interface Gi0/0 Router(config-if)# description WAN Router(config-if)# ip address 209.165.201.1 255.255.255.0 Router(config-if)# ip nat outside | Stateless keyword does not create the flow entries for static mapping after configuring the NAT, specify the inside and outside interface                                                                                                                                                                                                                                                            |
| debug ip nat \[ACL-num]                                                                                                                                                                                                                                                                                                       | to observe NAT operation for a specific ACL                                                                                                                                                                                                                                                                                                                                                          |
| R1(config)#ip nat inside source static 192.168.1.1 192.168.12.100 extendable R1(config)#ip nat inside source static 192.168.1.1 192.168.13.100 extendable![s1 r1 isp1 isp2 nat topology](<../.gitbook/assets/Unknown image (1488)>)                                                                                           | extendable keyword allows the user to configure several ambiguous static translations, where an ambiguous translations are translations with the same local or global address Note Keep in mind that the first rule in your configuration will be used for traffic that originates from the inside. So the translation for the second entry via ISP2 won't be performed and the connection times out |

<figure><img src="../.gitbook/assets/Unknown image (1487)" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Unknown image (1486)" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Unknown image (1489)" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Unknown image (1490)" alt=""><figcaption></figcaption></figure>

### Dynamic NAT

**Dynamic NAT** maps local IPv4 addresses to a pool of global IPv4 addresses. When an inside device accesses an outside network, it is assigned a global address that is available at the moment of translation.

In Cisco IOS Software terminology, a group of addresses is called a pool of addresses. Address pools are named and are referenced by their name in commands and command outputs. IP addresses that belong to a pool are specified using a reference IP address and a subnet mask or prefix length

The assignment follows a first-come first-served algorithm, there are no fixed mappings; therefore, the translation is dynamic. The number of translations is limited by the size of the pool of global addresses. When using dynamic NAT, make sure that enough global addresses are available to satisfy the needed number of user sessions.

Dynamic NAT entries time out. These entries have a default timeout value of 86400 seconds (24 hours), after which they are removed from the table if there is no activity for the duration of the timeout.

After this time elapses, the mapping is no longer valid and the global IPv4 address is made available for new translations. An example of when dynamic NAT is used is a merger of two companies that are using the same private address space. Dynamic NAT effectively readdresses packets from one network and is an alternative to complete readdressing of one network.

Dynamic and Static NAT does not conserve IPv4 address space, since it would be necessary to have a public IP address for each private host, but it's still used in internal network in specific scenarios. Dynamic NAT is the least used one as it have many disadvantages, such as non-deterministic IP assignments since they are assigned dynamically

| Router(config)# ip nat pool NAT-POOL 209.165.201.1 209.165.201.4 netmask 255.255.255.0 Router(config)# access-list 1 permit 10.1.1.0 0.0.0.255 Router(config)# ip nat inside source list 1 pool NAT-POOL Router(config)# interface Gi0/1 Router(config)# ip address 10.1.1.1 255.255.255.0 Router(config-if)# ip nat inside Router(config-if)# interface Gi0/0 Router(config-if)# ip address 209.165.201.5 255.255.255.0 Router(config-if)# ip nat outside Note in dynamic NAT individual separate connections from a single inside IP to multiple outside IP's are distinguished by the port number, however in this scenario one single inside IP 10.1.1.1 is mapped to a single outside IP 209.165.201.1 as defined in the dynamic NAT pool When other host with a different inside IP pings, it gets different public IP from the pool | create NAT pool for the outside address range create ACL to include inside network to be mapped to the outside pool range maps the inside source addresses from ACL to outside address from the NAT pool rotary keyword - router will distribute outgoing connections among the available addresses in a rotating or round-robin fashion. This load-balancing approach helps evenly distribute the load across the specified IP addresses in the NAT pool, providing a form of load balancing for outbound traffic SIP: 10.1.1.1 DIP 192.168.0.1 SIP: 10.1.1.1 DIP 209.165.201.6 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| clear ip nat translations \*                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | to clear all NAT entries in the table                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ip nat translation timeout <>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | After a certain amount of idle NAT time, the global IP address is returned to the pool. Default lease time is 24h                                                                                                                                                                                                                                                                                                                                                                                                                                                                |

<figure><img src="../.gitbook/assets/Unknown image (1491)" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Unknown image (1492)" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Unknown image (1493)" alt=""><figcaption></figcaption></figure>

### Port Address Translation (PAT / NAPT)

**Port Address Translation (PAT / NAPT)** maps multiple local IPv4 addresses to just a single global IPv4 address (many to one)

PAT is also known as NAT overloading, because you overload one global address with ports until you exhaust available port numbers. The mappings in the case of PAT have the format of local\_IP:local\_port – global\_IP:global\_port. PAT enables multiple local devices to access the internet, even when the device bordering the ISP has only one public IPv4 address assigned. PAT is the most common type of network address translation.

conserved the IPv4 space by allowing to connect multiple private networks with just one public IP configured on it's internet-facing WAN interface

NAT table is populated by PAT that binds each local host's IP address to the publically routable IP address leveraging the Layer 4 port numbers in a transport layer header to perform and track a dynamic many-to-one mappings, that is many local IP addresses to single public WAN IP address (PAT is essentailly socket multiplexing)

The number of concurrent connections for each shared IP address in a dynamic PAT is limited by the number of available ports in a transport layer header which is 16bit - 65536 ports

Additional IP addresses can be added to increase the capacity. PAT also can be used for merged companies with overlapping address space, by translating overlapping network subnet into one unique address (For example two networks with same address range 10.0.0.0/24 - we can use one IP from the /24 to represent the destination network, so that the source overlapping network can access the destination network on that one IP (the actual hosts are distinguished by a port))

#### Dynamic PAT

The mechanism of translating port numbers tries to preserve the original local port number determined by the originating host, meaning that it tries to avoid port translation. If more than one connection uses the same original local port number, PAT will preserve the port number only for the first connection translated. All other connections will have the port number translated.

**Dynamic PAT** is unidirectional; traffic must be initiated from the inside, this essentially creates an entry in NAT table of the router so it can translate back the return traffic

If the router has no entry in it's NAT table for incoming traffic from the outside, it drops the packet

NAT does not allow requests initiated from the outside.

As with dynamic NAT, all mappings created by PAT have a timeout. Once they expire, the mappings are deleted from the mapping table

Similarly if the timeout expires for an NAT entry translated for the inside device it will also drop the incoming packet from the outside

If the return communication is received after the timeout expires, there would be no mappings, and the packets will be discarded. You will not encounter this issue in static NAT. A static NAT configuration creates static mappings, which are not time limited. In other words, statically created mappings are always present. Therefore, those packets from outside can arrive at any moment, and they can be either requests initiating communication from the outside, or they can be responses to requests sent from inside.

Example

The outside host Z wants to initiate connection to router's WAN IP on port 443, however this router does not have such entry in its NAT table, so it drops the packet

To allow specific ports back through the dynamic PAT, a static PAT can be combined with dynamic PAT. This is known as port forwarding

![](<../.gitbook/assets/Unknown image (1494)>)

| Router(config-if)# interface Gi0/0 Router(config-if)# ip address 10.1.1.254 255.255.255.0 Router(config-if)# ip nat inside Router(config-if)# interface Gi0/1 Router(config-if)# ip address 192.168.2.1 Router(config-if)# ip nat outside Router(config)# ip nat inside source list 1 interface Gi0/1 overload Router(config)# access-list 1 permit 192.168.0.0 0.0.0.255 | keyword overload enables dynamic PAT Note You can specify Global IP instead of interface |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |

![](<../.gitbook/assets/Unknown image (1495)>)

| ip nat pool GLOB-ADR 128.66.2.1 128.66.2.2 netmask 255.255.255.0 ip nat inside source list 1 pool GLOB-ADR overload ! interface Ethernet0/0 ip address 10.1.1.10 255.255.255.0 ip nat inside ! interface Serial0/0 ip address 128.66.2.1 255.255.255.0 ip nat outside ! access-list 1 permit 10.1.1.0 0.0.0.255 | PAT can also be used with a pool of specific outside IP addresses and not limited to only IP address of the outside interface Translate all addresses from 10.1.1.0/24 to 128.66.2.1 or 128.66.2.2 |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### Static PAT (port forwarding)

**Static PAT (port forwarding)** specifies a static mapping that translates both inside local IPv4 address and port number to inside global IPv4 address and port number. As with all static mappings, port forwarding mapping will always be present at the border device, and when packets arrive from outside networks, the border device would be able to translate global address and port to corresponding local address and port

It is used to redirect packet by changing the source port number of incoming packets to a different port (and or IP aswell)

This is used for example to expose specific services in the internal network to the Internet

Since the pre-translation IP:Port and post-translation IP:Port in a static PAT are explicitly defined, the initial packet could have come from either the Internet hosts or the inside hosts. Therefore, a Static PAT translation is bidirectional

| ip nat \[inside \| outside] source \[static \| list] { tcp \| udp } \[extendable] |   |
| --------------------------------------------------------------------------------- | - |

| ip nat inside source static tcp 192.168.0.5 80 171.68.1.1 443  | translates incoming packet on inside interface (192.168.0.5:80) to the 171.68.1.1:443                                    |
| -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| ip nat outside source static tcp 172.68.1.1 443 192.168.0.5 80 | translates incoming packet on outside interface - useful if we want to access internal service from the outside Internet |

![](<../.gitbook/assets/Unknown image (1496)>)

### Excluding non-NAT traffic from NAT operation

Any nontranslated packet that flows through the NAT interface goes through a series of checks to determine whether the packet must be translated or not

These checks result in increased latency for nontranslated packet flows and thus negatively impact the packet processing latency of all packet flows through the NAT interface.

| ip access-list extended NAT-ACL deny ip 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255 permit ip 192.168.1.0 0.0.0.255 any ! interface GigabitEthernet0/0 description Link to LAN ip address 192.168.1.1 255.255.255.0 ip nat inside ! interface GigabitEthernet0/1 description Link to Internet ip address 100.100.100.2 255.255.255.252 ip nat outside ! ip nat inside source list NAT-ACL interface GigabitEthernet0/1 overload | traffic to be excluded from NAT operation Allow all other traffic from LAN-1 to be NATed Enable the NAT functionality on inside and outside interfaces Enable NAT |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Benefits of NAT

NAT conserves public addresses by enabling multiple privately addressed hosts to communicate using a limited, small number of public addresses instead of acquiring a public address for each host that needs to connect to internet. The conserving effect of NAT is most pronounced with PAT, where internal hosts can share a single public IPv4 address for all external communication.

NAT increases the flexibility of connections to the public network.

NAT provides consistency for internal network addressing schemes. When a public IPv4 address scheme changes, NAT eliminates the need to readdress all hosts that require external access, saving time and money. The changes are applied to the NAT configuration only. Therefore, an organization could change ISPs and not need to change any of its inside clients.

NAT can be configured to translate all private addresses to only one public address or to a smaller pool of public addresses.

When NAT is configured, the entire internal network hides behind one address or a few addresses. To the outside, it seems that there is only one or a limited number of devices in the inside network. This hiding of the internal network helps provide additional security as a side benefit of NAT.

### Disadvantages of NAT

End-to-end functionality is lost. Many applications depend on the end-to-end property of IPv4-based communication. Some applications expect the IPv4 header parameters to be determined only at endpoints of communication. NAT interferes by changing the IPv4 address and sometimes transport protocol port (if using PAT) numbers at network intermediary points.

Changed header information can block applications.

* For instance, call signaling application protocols include the information about the device's IPv4 address in its headers.

Although the application protocol information is going to be encapsulated in the IPv4 header as data is passed down the

TCP/IP stack, the application protocol header still includes the device's IPv4 address as part of its own information.

* The transmitted packet will include the sender's IPv4 address twice: in the IPv4 header and in the application header.

When NAT makes changes to the source IPv4 address (along the path of the packet), it will change only the address in the IPv4 header. NAT will not change IPv4 address information that is included in the application header.

* At the recipient, the application protocol will rely only on the information in the application header. Other headers will be removed in the de-encapsulation process.

Therefore, the recipient application protocol will not be aware of the change

NAT has made and it will perform its functions and create response packets using the information in the application header.

* This process results in creating responses for unroutable IPv4 addresses and ultimately prevents calls from being established. Besides signaling protocols, some security applications, such as digital signatures, fail because the source

IPv4 address changes. Sometimes, you can avoid this problem by implementing static NAT mappings.

Single point of failure and performance

End-to-end IPv4 traceability is also lost. It becomes much more difficult to trace packets that undergo numerous packet address changes over multiple NAT hops, so troubleshooting is challenging. On the other hand, for malicious users, it becomes more difficult to trace or obtain the original source or destination addresses.

Using NAT also creates difficulties for the tunneling protocols, such as IP Security (IPsec), because NAT modifies the

values in the headers. Integrity checks declare packets invalid if anything changes in them along the path. NAT changes interfere with the integrity checking mechanisms that IPsec and other tunneling protocols perform.

Services that require the initiation of TCP connections from an outside network (or stateless protocols, such as those using

UDP) can be disrupted. Unless the NAT router makes specific effort to support such protocols, inbound packets cannot reach their destination. Some protocols can accommodate one instance of NAT between participating hosts (passive mode

FTP, for example) but fail when NAT is performed at multiple points between communicating systems, for instance both in the source and in the destination network.

NAT can degrade network performance. It increases forwarding delays because the translation of each IPv4 address within

the packet headers takes time. For each packet, the router must determine whether it should undergo translation. If translation is performed, the router alters the IPv4 header and possibly the TCP or UDP header. All checksums must be recalculated for packets in order for packets to pass the integrity checks at the destination. This processing is most time consuming for the first packet of each defined mapping. The performance degradation of NAT is particularly disadvantageous for real time applications, such as VolP.

### VPN with overlapping networks

In the example below, there are two sites – Seattle and Denver – connected with a VPN tunnel between R1 and R2

Both Seattle and Denver are using 10.0.0.0/24 for their internal network.

Host A in Seattle (10.0.0.77) needs to speak to Host D in Denver (10.0.0.88).

Since Host A is configured with the IP 10.0.0.77/24, Host A believes that every IP address in the range of 10.0.0.0 – 10.0.0.255 exists on its own local network in Seattle

Therefore, if Host A attempts to send a packet to 10.0.0.88, it will not send the packet to the Router.

Host D will have the same problem, Host D is configured with the IP 10.0.0.88/24 and also believes that the range 10.0.0.0 – 10.0.0.255 exists on its own local network in Denver

Therefore, any packet Host D sends to the IP 10.0.0.77 will be sent to the local network, and not to the Router.

If the packets are not sent to the Router, then the Routers are unable to forward them through the VPN tunnel to the other side

As a result, because of the overlapping networks, neither side will be able to speak to the other.

![VPN Overlapping Networks - The Problem](<../.gitbook/assets/Unknown image (1497)>)

The solution to the problem is to convince each host that the other host is on a foreign network

That would cause them to send packets to the Router, which can then send them through the VPN tunnel.

This will be attained by making the Seattle network appear as 10.1.1.0/24 when speaking to Denver, and making the Denver network appear as 10.2.2.0/24 when speaking to Seattle.

Configuring policy NAT on both routers

R1’s Policy NAT configuration will match packets with a Source IP of 10.0.0.0/24 (Seattle’s actual network) and a Destination IP of 10.2.2.0/24 (Denver’s masked network), and translate the Source IP to the 10.1.1.0/24 network (Seattle’s masked network).

R2’s Policy NAT configuration will match packets with a Source IP of 10.0.0.0/24 (Denver’s actual network) and a Destination IP of 10.1.1.0/24 (Seattle’s masked network), and translate the Source IP to the 10.2.2.0/24 network (Denver’s masked network).

In this way, R1 is masking the Seattle 10.0.0.0/24 network as 10.1.1.0/24, and R2 is masking the Denver 10.0.0.0/24 network as 10.2.2.0/24.

### Policy-Based NAT / Conditional NAT

**Policy-Based NAT / Conditional NAT** is a combination of route-maps in order to define more specific policies

In a route-map, one of the things you can use is access-lists, so you can create NAT rules based on anything you can match in an access-list

| ip access-list extended client-serverweb permit ip host 172.16.0.5 host 10.0.1.100   |                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| route-map route-client-serverweb permit 10 match ip address client-serverweb         |                                                                                                                                                                                                       |
| ip nat inside source static 172.16.0.5 172.16.100.5 route-map route-client-serverweb | translate the source ip address 172.16.0.5 to 172.16.100.5 only when the source ip address 172.16.0.5 tries to connect to the server web 10.0.1.100, otherwise don't translate the source ip address. |

### Twice NAT

**Twice NAT** is a translation of both the source and destination of packets. Unlike traditional NAT, which only translates the source in the outbound direction - performed usually by firewalls

For example if a company wants to force corporate device to use their internal DNS server instead of the public one

![](<../.gitbook/assets/Unknown image (1498)>)

### VRF-aware NAT (VASI)

Devices that run on Cisco IOS XE do not support classical inter-VRF NAT configurations as those found on Cisco IOS devices

Support for inter-VRF NAT on Cisco IOS XE is achieved via VRF-Aware Software Infrastructure implementation

**VASI** provides the ability to create virtual interfaces that can be associated with a different VRF instance

This allows traffic from one VRF instance to be translated using NAT and sent out an interface associated with a different VRF instance

VASI is implemented by configuring VASI pairs- vasileft and vasiright interface, where each of the interfaces in the pair is associated with a different VRF instance.

The VASI virtual interface is the next-hop interface for any packet that needs to be switched between these two VRF instances.

VASI interface numbers must match in order to be a pair (eg. vasileft1/vasiright1)

![](<../.gitbook/assets/Unknown image (1499)>)

| SanJose interface GigabitEthernet0/0/0 ip address 192.168.1.1 255.255.255.0 ip route 0.0.0.0 0.0.0.0 192.168.1.2 Sydney interface GigabitEthernet0/0/0 ip address 172.16.1.1 255.255.255.0 ip route 0.0.0.0 0.0.0.0 172.16.1.2 Bombay vrf definition VRF\_LEFT rd 1:1 ! address-family ipv4 exit-address-family vrf definition VRF\_RIGHT rd 2:2 ! address-family ipv4 exit-address-family interface GigabitEthernet0/0/0 vrf forwarding VRF\_LEFT ip address 192.168.1.2 255.255.255.0 ip nat inside ! interface GigabitEthernet0/0/1 vrf forwarding VRF\_RIGHT ip address 172.16.1.2 255.255.255.0 ! interface vasileft1 vrf forwarding VRF\_LEFT ip address 10.1.1.1 255.255.255.252 ip nat outside ! interface vasiright1 vrf forwarding VRF\_RIGHT ip address 10.1.1.2 255.255.255.252 ip access-list extended 100 10 permit tcp 192.168.1.0 0.0.0.255 host 172.16.1.1 20 permit udp 192.168.1.0 0.0.0.255 host 172.16.1.1 30 permit icmp 192.168.1.0 0.0.0.255 host 172.16.1.1 ip route vrf VRF\_LEFT 172.16.0.0 255.255.0.0 vasileft1 10.1.1.2 ip route vrf VRF\_RIGHT 192.168.0.0 255.255.0.0 vasiright1 10.1.1.1 ip nat pool POOL 172.16.1.5 172.16.1.5 prefix-length 24 ip nat inside source list 100 pool POOL vrf VRF\_RIGHT overload | Traceroute goes via the vasi interface Note i added one more (.3) host in VRF\_LEFT to confirm that the single 172.16.1.5 will be used as PAT |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |

<figure><img src="../.gitbook/assets/Unknown image (1500)" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Unknown image (1501)" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Unknown image (1502)" alt=""><figcaption></figcaption></figure>

**match-in-vrf** command is needed when the NAT translation should occur in the same VRF and not in GRT (which is default)

![](<../.gitbook/assets/Unknown image (1503)>)

### NAT Virtual Interface (NVI)

The most common method to configure NAT on Cisco IOS routers is what we call “domain-based NAT”. Each interface on the router has to be configured as “inside” or “outside”

This method of configuring NAT is considered the legacy method. The new way of configuring NAT is by using the NAT virtual interface.

**NAT Virtual Interface (NVI)** feature allows NAT traffic flows on the virtual interface, eliminating the need to specify inside and outside domains, so that the NAT can be performed from inside to inside. NVI is designed for traffic from one VRF to another and not for routing between subnets in the global routing table

This changes the order of router operation, the router will perform routing first then the nat translation and then proceeds with routing again.

This essentially allows a system to NAT to its own interface, eg. “inside to inside”.

Note this configuration is available only on IOS and IOSv and not IOS XE and XR

| (config-if)# ip nat enable                      | Configures an interface connecting VPNs and the Internet for NAT translation.                                                         |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| ip nat pool <> <> <> prefix-length <> add-route | add-route creates a static host route for the outside local IP so that return traffic from the inside network can be correctly routed |
| ip nat source list 1 pool <>                    | it will enable NVI and autmatically create NVI virtual interface. You can use overload aswell                                         |

### NAT Reflection (Hairpinning)

**NAT Reflection (Hairpinning)** is used wherever the systems behind a firewall (or a NAT device) want to access another system in the same subnet using it’s public IP address instead of directly accessing through its private IP address belonging to the same subnet (essentially loop backs the traffic out of the same port creating U)

This is usually employed in a VPN scenario, where Firewall is responsible for hairpining. This is also useful for testing purposes such as enforcing certain policies to apply on the packet

### Carrier-Grade NAT (CGNAT / CGN)

**Carrier-Grade NAT (CGNAT / CGN)** is a large-scale type of NAT that is used by ISPs to provide internet access to their customers. CGN works same as PAT - allowing multiple customers to share a single, public IP address.

CGN is also called **NAT444** because outbound traffic from the local network to the Internet would pass through three different IPv4 addressing domains: the customer's own private network, the carrier's private network (which is well-known reserved range 100.64.0.0/10 - 100.127.255.255) and the public Internet.

CGN device gets a WAN-side unique routable public IP address, but your own router, and your neighbor's router now gets a private CGN non-routable address.

Yours is 100.64.1.3 - it exists only between your router's WAN (Internet side) interface and the CGN device and cannot be reached from elsewhere on the Internet.

The packets in CGN range are not propagated to the internet and is dropped by the ISP (as a non-routable address)

On the Internet, your packets will appear to come from the ISP public routable IP 200.100.5.1 , including all other customers connected to this ISP

The CGN device figures out who incoming data is for, but only when it's a reply to an outgoing request (unidirectional as PAT)

As an example in our environment we map each /24 of CGN space to 1 external IP with 100 ports per internal IP. For small business networks that often only have a single IP address to work with the norm is for thousands of hosts to be NATed behind a single IP so this generally isn't an approach that works for them but as an ISP with public space being able to turn each /24 into a single IP is a huge reduction in the amount of public space needed to service customers.

Note

Cisco IOS XR does not support traditional NAT configurations as found in other Cisco operating systems like IOS or IOS XE. Instead, it offers Carrier Grade NAT (CGN), a large-scale NAT solution designed for service providers to manage IPv4 address depletion. CGN allows the translation of private IPv4 addresses to public IPv4 addresses on a large scale, supporting millions of translations to accommodate numerous subscribers.

![Carrier Grade NAT](<../.gitbook/assets/Unknown image (1504)>)

Each CPE does NAT from customer's LAN RFC1918 to CGN address space, which is then NATTed by the ISP Edge router to the Public IP

Comprehensive CGN configuration guide:

[https://www.cisco.com/c/en/us/td/docs/routers/crs/software/crs-r6-4/cgnat/configuration/guide/b-cgnat-cg-crs-64x.pdf](https://www.cisco.com/c/en/us/td/docs/routers/crs/software/crs-r6-4/cgnat/configuration/guide/b-cgnat-cg-crs-64x.pdf)

| Device(config)# ip nat settings mode cgn | sets the router to the cgnat mode, so that it increases NAT scalability and makes NAT stateless |
| ---------------------------------------- | ----------------------------------------------------------------------------------------------- |

Challenges

1. The shared public IP complicates incoming connections, as the ISP's CGNAT router can't determine which customer they are intended for, as already described in Dynamic PAT, where the NAT entry establishes only when the traffic is initiated from the inside, so that it creates a binding entry on the NAT device
2. GCNAT breaks the end-to-end principle of Internet routing. Regular NAT did that too, but that was a necessary compromise and, if required, you can mitigate many issues with port forwarding or redirection rules on your router. With CGNAT, you cannot set up rules on your ISP's router.

Technologies that mitigate challenges above

UPnP (Universal Plug and Play)

allows devices in a local network to automatically discover and communicate with each other for services, such as automatic port forwarding.

UPnP can be used to dynamically open ports on the CGNAT device, enabling external systems to establish connections with specific internal services. This allows for more flexible handling of inbound connections.

PCP (Port Control Protocol)

is a successor to UPnP and is designed to facilitate NAT-T and manage port mappings. It provides a more sophisticated and secure way of handling port mappings compared to UPnP

STUN (Session Traversal Utilities for NAT)

used to discover the presence of a NAT between two endpoints and to determine the public IP address and port.

It's often used in conjunction with other protocols, such as ICE (Interactive Connectivity Establishment), for establishing connections in peer-to-peer communication.

VPN Matcher

Another solution for being able to dial into a VPN host which is behind CGNAT is to use a service/facility such as VPN Matcher from DrayTek

This is a service where each end of the VPN (a DrayTek VPN-capable router or computer with DrayTek's SmartVPN Client) connects to the VPN Matcher service

Both ends of the VPN connection log into the VPN Matcher server and the server determines their actual public IP addresses and port numbers and provides that to the other router (similarly to how a STUN server works).

The instigating end (calling router) can then use that information to connect to the receiving (host) end because both routers have instigated a connection with matching ports

### NAT Traversal (NAT-T)

**NAT Traversal (NAT-T)**, also called IPSec aware NAT, allows to establish IPsec connection over NAT device

IPsec uses ESP to encrypt all packet, encapsulating the L3/L4 headers within an ESP header

ESP is an IP protocol but there is no port number (Layer 4). This is a difference from ISAKMP which uses UDP port 500 as its UDP layer 4.

This precents ESP from passing through PAT devices. Because there is no port to change in the ESP packet, the binding database can't assign a unique port to the packet at the time it changes its RFC 1918 address to the publicly routable address

If the packet can't be assigned a unique port then the database binding won't complete and there is no way to tell which inside host sourced this packet

As a result there is no way for the return traffic to be untranslated successfully.

NAT-T does two things - Detects if both ends support NAT-T and Detects NAT devices along the transmission path (NAT-Discovery)

NAT-T encapsulates ESP packets inside UDP and assigns both the Source and Destination ports as 4500

After this encapsulation there is enough information for the PAT database binding to build successfully. Now ESP packets can be translated through a PAT device.

Operation

In messages one and two MM of ISAKAMP Phase 1, the devices discover, whether they both support NAT-T

![](<../.gitbook/assets/Unknown image (1505)>)

The sender device send a NAT-D payload, inside the NAT-D payload there are a hash of the Source IP address and port (172.16.1.1 and 500) and a hash of the Destination IP address and port (200.1.1.1 and 500).

The RTR-Site1 device (172.16.1.1) sends the following:

A HASH of Source IP address and port (172.16.1.1 and 500): ab18C4efb950c61f568a636561764e6f

A HASH of Destination IP address and port (200.1.1.1 and 500): 8b44b859631968ceeb26b61430014fc6

![](<../.gitbook/assets/Unknown image (1506)>)

The RTR-Site2 (200.1.1.1) device responds the following:

A HASH of Source IP address and port (200.1.1.1 and 500): 8b44b859631968ceeb26b61430014fc6

A HASH of Destination IP address and port (100.1.1.1 and 500): 66718a3d26322b74c7de2c87fb1ff4c9

![](<../.gitbook/assets/Unknown image (1507)>)

The result is that the receiving device RTR-Site2 recalculates the hash based on the Destination Peer IP Address 100.1.1.1 and Port 500 which is 66718a3d26322b74c7de2c87fb1ff4c9 and compares it with the hash it received from RTR-Site1 which is ab18C4efb950c61f568a636561764e6f.

If they don’t match a NAT device exists

If a NAT device has been determined to exist, NAT-T will change the ISAKMP transport with ISAKMP Main Mode messages five and six, at which point all ISAKMP packets change from UDP port 500 to UDP port 4500

![](<../.gitbook/assets/Unknown image (1508)>)

IKE Phase 2 (IPsec Quick Mode) encapsulates the Quick Mode (IPsec Phase 2) inside UDP 4500 aswell

![](<../.gitbook/assets/Unknown image (1509)>)

After Quick Mode negotiation is completed, the Phase 2 is now ready to encrypt the data and ESP Packets are encapsulated inside UDP port 4500 as well, thus providing a port to be used in the NAT device to perform port address translation.

When a packet with source and destination port of 4500 is sent through a PAT device (from inside to outside), the PAT device will change the source port from 4500 to a random high port, while keeping the destination port of 4500. When a different NAT-T session passes through the PAT device, it will change the source port to a different random high port, and so on.

![](<../.gitbook/assets/Unknown image (1510)>)

What is the difference between NAT-T and IPSec-over-UDP ?

When NAT-T is enabled, it encapsulates the ESP packet with UDP only when it encounters a NAT device. Otherwise, no UDP encapsulation is done

But, IPSec Over UDP, always encapsulates the packet with UDP.

NAT-T always use the standard port, UDP-4500

It is not configurable. IPSec over UDP normally uses UDP-10000 but this could be any other port based on the configuration on the VPN server.

### NAT for IPv6

IPv6 was designed to eliminate the need for NAT due to it's significant drawbacks, however few forms of IPv6 NAT features have been defined to either ensure transition from the IPv4 or to provide prefix independence

#### NAT-PT (deprecated)

**NAT-PT (deprecated)** is a rejected mechanism for translating datagrams bidirectionally between IPv4 and IPv6

IPv6 packets routed towards IPv4 hosts should have their src/dst addresses changed to some IPv4 equivalents and vice versa, while IPv4 packets sent toward IPv6 hosts should get both src and dst addresses replaced with IPv6 addresses.

A /96 IPv6 prefix was defined leaving 32 bits for the entire IPv4 address space that could be inserted into the IPv6 address or extracted from IPv6 address by the NAT-PT device

It was implemented either as static entries, dynamic pool allocations or a by a NAT-PT-PT known as overload. Its successor is DNS64

Note It is no longer available in IOS XE or XR software

### NAT66 - IPv6-to-IPv6 Network Prefix Translation (NPTv6)

**NAT66 - IPv6-to-IPv6 Network Prefix Translation (NPTv6)** is a stateless prefix translation, translating prefixes as 1:1. It doesn’t translate the host address, there is no “overload” like NAT where you can have multiple source addresses behind a single address. NPTv6 can translate private ULA from hosts to the GUA routable on the Internet. The host part of the private prefix is copied into the translated global prefix

Reduces provider lock-in for small or medium enterprises that don’t want to renumber their internal network when changing providers

![](<../.gitbook/assets/unknown (7).png>)

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top"><p>(config-if)# ipv6 address FDFF::1/64</p><p>(config-if)# nat66 inside</p><p>(config-if)# ipv6 address 2001:BAA1:FEA:A::1/64</p><p>(config-if)# nat66 outside</p></td><td valign="top"></td></tr><tr><td valign="top">(config)#nat66 prefix inside FDFF::/64 outside 2001:BAA1:FEA:A::/64</td><td valign="top"></td></tr><tr><td valign="top">show nat66 [ prefix | stat ]</td><td valign="top">Note since it is stateless and the router doesn't maintain any NAT sessions, no command like show ip nat translations</td></tr></tbody></table>

[https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/ipaddr\_nat/configuration/xe-16-11/nat-xe-16-11-book/iadnat-asr1k-nptv6.html](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/ipaddr_nat/configuration/xe-16-11/nat-xe-16-11-book/iadnat-asr1k-nptv6.html)
