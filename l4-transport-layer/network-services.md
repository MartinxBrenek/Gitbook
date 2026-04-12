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

# Network Services

## Dynamic Host Configuration Protocol (DHCP)

Imagine that your company has a large number of devices that need to be assigned an IP address to communicate with each other.

We can manually assign an IP address to each device, but what if our company has several networks, spread all over the world, and each network is growing with more and more devices. For this reason, manually configuring IP addresses and maintaining a manual allocation database is very inefficient and time consuming

DHCP can greatly decrease the workload of the network administrator. DHCP automatically assigns an IPv4 address from an IPv4 address pool that the administrator defines. However, DHCP is much more than just a mechanism that allocates IPv4 addresses. This service automates the assignment of IPv4 addresses, subnet masks, gateways, and other required networking parameters.

**Stateful address assignment** keeps a track of assigned IP addresses and the address pool availability and resolves duplicated address conflicts. It also logs every assignment and keeps track of the expiration times.



**Dynamic Host Configuration Protocol (DHCP)** is a client/server protocol that automatically provides an IP address to host and other configuration parameters such as the subnet mask, default gateway, DNS server IP and other

**Client server model** is used in network communication especially for providing services such as DHCP or DNS, where the client - a station that requires a certain service from the providing DHCP or DNS server

The term "client" refers to a host that is requesting initialization parameters from a DHCP server. Most endpoint devices on today’s networks are DHCP clients, including Cisco IP phones, desktop PCs, laptops, printers, and even Blu-Ray players. Just about any device that you can configure to participate on a TCP/IP network has the option of using DHCP to obtain its IPv4 configuration.

Note: If the DHCP server is not in the local subnet, we have to use DHCP Relay agent

DHCP server should be placed in the same VLAN as the newly added client devices, so that the server can receive DHCP broadcast messages from clients

History Note: DHCP is the successor to the Bootstrap Protocol (BOOTP), offering more parameters than the BOOTP

Dynamic allocation of IPv4 addresses is the most common type of address assignment. As devices boot and activate their Ethernet interfaces, the DHCP client service triggers a DHCP Discover broadcast that includes the DHCP client MAC address.

Automatic allocation: Automatic allocation of IPv4 addresses is very similar to dynamic allocation, except that the lease time is set never to expire. This setting results in the DHCP client always being associated with the same IPv4 address.

Static allocation is an alternative that is generally used for devices such as servers and printers, where the device needs to keep the same IPv4 address configuration permanently.

### Operation (DORA)

Client and Server message exchange is called **DORA (Discover-Offer-Request-Acknowledge)**. Those messages are encapsulated in the UDP header.

Client (bootp) and DHCP relays uses UDP port 68 and Servers (bootps) use UDP port 67

![](<../.gitbook/assets/Unknown image (716)>)

| DHCP Discover Message Src IP: 0.0.0.0 port 68 Dst: 255.255.255.255 port 67 Src MAC: Dst MAC: FF:FF:FF:FF:FF:FF | When the client boots up it immediately sends DHCP discover broadcast message to discover DHCP servers in a network A DHCP client may also request an IP address in the DHCPDISCOVER, which the server may take into account when selecting an address to offer. Since DHCP client doesn’t know the IP address of the server the message is sent as broadcast with the source IP as 0.0.0.0 since the client does not have any IP address yet                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| -------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| DHCP Offer Message Src: port 67 Dest: \<Offerred IP (YlAddress) or 255.255.255.255> port 68 Src MAC: Dst MAC:  | When a DHCP server receives a DHCPDISCOVER message from a client, which is an IP address lease request, the DHCP server reserves an IP address for the client and makes a lease offer by sending a DHCPOFFER message to the client. This message contains the client's client id (traditionally a MAC address), the IP address that the server is offering, the subnet mask, the lease duration, and the IP address of the DHCP server making the offer. The destination IP address of the offer message is the offered IP with the destination MAC as the MAC of the DHCP client Normally, DHCP servers and BOOTP relay agents attempt to deliver DHCPOFFER, DHCPACK and DHCPNAK messages directly to the client using unicast delivery. The IP destination address (in the IP header) is set to the DHCP 'yiaddr' address and the link-layer destination address is set to the DHCP 'chaddr' address. Unfortunately, some client implementations are unable to receive such unicast IP datagrams until the implementation has been configured with a valid IP address (leading to a deadlock in which the client's IP address cannot be delivered until the client has been configured with an IP address). A client that cannot receive unicast IP datagrams until its protocol software has been configured with an IP address SHOULD set the BROADCAST bit in the 'flags' field to 1 in any DHCPDISCOVER or DHCPREQUEST messages that client sends. The BROADCAST bit will provide a hint to the DHCP server and BOOTP relay agent to broadcast any messages to the client on the client's subnet. A client that can receive unicast IP datagrams before its protocol software has been configured SHOULD clear the BROADCAST bit to 0. The BOOTP clarifications document discusses the ramifications of the use of the BROADCAST bit Destination can be in case of relay is used |
| DHCP Request Message Src: 0.0.0.0 port 68 Dest: 255.255.255.255 port 68 Src MAC: Dst MAC: FF:FF:FF:FF:FF:FF    | In response to the DHCP offer, the client replies with a DHCPREQUEST message, broadcast to the server, requesting the offered address. A client can receive DHCP offers from multiple servers, but it will accept only one DHCP offer Before claiming an IP address, the client will broadcast an ARP request, in order to find if there is another host present in the network with the proposed IP address. If there is no reply, this address does not conflict with that of another host, so it is free to be used. The client must send the server identification option in the DHCPREQUEST message, indicating the server whose offer the client has selected. When other DHCP servers receive this message, they withdraw any offers that they have made to the client and return their offered IP address to the pool of available addresses. If the client receive a response from other device for the IP offered by the server, it proceeds to send special DHCP Decline message to inform the server that the IP is in use and requesting for different. Moreover broadcasting the DHCP request message allows the client to receive the IP configuration from the fastest DHCP server on the segment DHCP Request serves also for following operations: Request parameters from one server and implicitly decline offers from other servers. Confirm that a previously allocated address is still available after a system reboot. Extend/renew the lease of a network address - 2step exchange - Request and Ack                                                                                                                                                                                                                                                                                                                                                         |
| DHCP Acknowledge Message Src: port 67 Dest: 255.255.255.255 port 68 Src MAC: Dst MAC:                          | Server responds to the client with the acknowledgement message containing the IP address, subnet mask, lease time and other configuration parameters that the client might have requested The protocol expects the DHCP client to configure its network interface with the negotiated parameters. After the client obtains an IP address, it should probe the newly received address(e.g. with ARP Address Resolution Protocol) to prevent address conflicts caused by overlapping address pools of DHCP servers. If this probe finds another computer using that address, the computer should send DHCPDECLINE, broadcast, to the server. Destination can be in case of relay is used                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |

#### DORA packet capture

![](<../.gitbook/assets/Unknown image (717)>)

#### Additional operations

| Additional Operations |                                                                                                                                                                                                                                                                                                                                                                                |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| DHCPNAK               | Server to client negative acknowledgment indicating the client’s understanding of the network address is incorrect (for example, if the client has moved to a new subnet), or a client’s lease has expired                                                                                                                                                                     |
| DHCP DECLINE          | Client to server message indicating the network address is already being used.                                                                                                                                                                                                                                                                                                 |
| DHCP RELEASE          | Client to server message to inform that the client is deactivating the IP configuration so that the server can update his lease table and release the IP to the pool                                                                                                                                                                                                           |
| DHCP INFORM           | Client to server message requesting only local configuration parameters or client has an externally configured network address. A DHCP client may request more information than the server sent with the original DHCPOFFER. The client may also request repeat data for a particular application. For example, browsers use DHCP Inform to obtain web proxy settings via WPAD |

### DHCP header

![](<../.gitbook/assets/Unknown image (718)>)

HTYPE is set to 1, to specify that the medium used is Ethernet, HLEN is set to 6 because an Ethernet address (MAC address) is 6 octets long

ChAddress - (Client Hardware (MAC) Address)

giaddress - represents the IP address of the gateway or DHCP relay agent that forwarded the DHCP message

YlAddress - (Your IP Address) IP address that the DHCP server assigns to the client. It is used when the DHCP server and client are on different network segments or VLANs. The DHCP relay agent fills in the giaddr field with its own IP address before forwarding the client's DHCP request to the server. This helps the server identify the client's location and allocate the appropriate IP address and configuration options from the corresponding network segment or VLAN

SAddress - DHCP Server IP Address

CIADDR - IP address currently assigned to the DHCP client. DHCP clients use it to indicate their current IP address when renewing or releasing their lease.

SNAME - DHCP Server Name for DNS

Magic cookie is set to 0x63825363 in hexadecimal format to indicate the presence of DHCP options

![](<../.gitbook/assets/Unknown image (719)>)

### How DHCP selects the correct VLAN / scope

DHCP itself does not inherently know about VLANs. The differentiation is done at the Layer 2/3 boundary, usually by the switch or router that is the first hop for the DHCP broadcast.

Client sends a DHCP Discover – The client broadcasts at Layer 2 (MAC broadcast). This frame is only visible inside its own VLAN (because VLANs are isolated broadcast domains).

Switch forwards within VLAN – Since the DHCP Discover is a broadcast, it stays inside that VLAN and doesn’t leak into others.

DHCP Relay (IP Helper Address) – If the DHCP server is not in the same VLAN as the client, the router or Layer 3 switch interface (SVI) receives the broadcast. That interface knows the VLAN/subnet it belongs to. It then relays the DHCP Discover as a unicast to the DHCP server, adding Option 82 (DHCP Relay Agent Information), which includes the VLAN or interface information.

DHCP server assigns an address – The DHCP server uses the source interface (or Option 82 data) to determine which IP scope/pool to use. That pool corresponds to the VLAN/subnet the request originated from.

If the router itself is the DHCP server (not just a relay), then it already knows the VLAN because:

Each VLAN interface (SVI or routed subinterface) has its own IP address, which is the default gateway for clients in that VLAN.

When a client in VLAN X broadcasts a DHCP Discover, the router receives it on that VLAN’s interface.

Since the router DHCP server configuration ties each DHCP pool to a specific network (subnet), it can directly match the request with the correct pool.

### DHCP options

**DHCP options** are additional parameters that provide specific network parameters, such as DNS,NTP server addresses and domain names, to the DHCP clients

Additionally, clients and DHCP relay agents can convey information back to the DHCP server, allowing for customized configuration

In some network setups, there may be switches between the DHCP client and the DHCP relay agent. These switches have the capability to insert an additional DHCP option called

#### DHCP option 82 (relay agent information)

Option 82 contains information about the switch, vlan, port, or other network details

Option 82 was designed to allow a DHCP Relay Agent to insert circuit−specific information into a request that is being forwarded to a DHCP server. This option works by setting two suboptions:

Circuit ID suboption includes information that is specific to the circuit the request came in on. This suboption is an identifier that is specific to the relay agent. Thus, the circuit that is described will vary depending on the relay agent.

Remote ID suboption includes information on the remote host–end of the circuit.

This suboption usually contains information that identifies the relay agent. In a wireless network, this would likely be a unique identifier of the wireless access point

Note this is enabled and inserted by cisco switches by default, if you want to disable it use #no ip dhcp snooping information option

In an example application, DHCP clients are connected to two ports of a single switch. Each port can be configured to be part of two VLANs: VLAN1 and VLAN2

Each VLAN has its own subnet and all DHCP messages from the same VLAN (same switch) will have the giaddr field set to the same value indicating the subnet of the VLAN.

The problem is that for a DHCP client connecting to port 1 of VLAN1, it must be allocated an IP address from one range within the VLAN's subnet, whereas a DHCP client connecting to port 2 of VLAN2 must be allocated an IP address from another range

In the normal DHCP address allocation, the DHCP server will look only at the giaddr field and thus will not be able to differentiate between the two ranges.

To solve this problem, a relay agent, typically a switch, inserts the relay information option (option 82), which carries information specific to the port, so that the DHCP server can inspect the giaddr field and the DHCP server must inspect both the giaddr field and the inserted option 82 during the address selection process

[The issue with DHCP option 82 and DHCP snooping](onenote:Security.one#Infrastructure%20Security\&section-id={0B7736F7-88AD-4CBC-BEE3-A076F5EBB1FC}\&page-id={B1A793A6-B737-4A8D-B1E9-06C1D5B765BC}\&object-id={44829C5D-91A7-0DBE-1375-BF310D785D36}&11\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE)

#### Well-known DHCP options

Option 1: Subnet Mask

Option 3: Router (Default Gateway)

Option 6: Domain Name Servers (DNS)

Option 15: Domain Name

Option 42: NTP server

Option 43: Vendor specific (useful for CAPWAP APs, DNA Center, …)

Optin 69: SMTP server

Option 51: IP Address Lease Time

Option 53: DHCP Message Type

Option 66: TFTP Server Name

Option 67: Bootfile Name

| 61 | Client-identifier | Minimum of 2 octets |
| -- | ----------------- | ------------------- |
| 54 | Server identifier | 4 octets            |

Full List: [https://www.incognito.com/tutorials/dhcp-options-in-plain-english/](https://www.incognito.com/tutorials/dhcp-options-in-plain-english/)

### DHCP relay

In small networks, where only one IP subnet is being managed, DHCP clients communicate directly with DHCP servers. However, DHCP servers can also provide IP addresses for multiple subnets. In this case, a DHCP client that has not yet acquired an IP address cannot communicate directly with a DHCP server not on the same subnet, as the client's broadcast can only be received on its own subnet

In order to allow DHCP clients on subnets not directly served by DHCP servers to communicate with DHCP servers, **DHCP relay agents** can be installed on these subnets. A DHCP relay agent runs on a network device, capable of routing between the client's subnet and the subnet of the DHCP server. The DHCP client broadcasts on the local link; the relay agent receives the broadcast and cnverts it to unicast and send it to the DHCP server in a different network. The IP addresses of the DHCP servers are manually configured in the relay agent. The relay agent stores its own IP address, from the interface on which it has received the client's broadcast, in the GIADDR field of the DHCP packet. The DHCP server uses the GIADDR-value to determine the subnet, and subsequently the corresponding address pool, from which to allocate an IP address. When the DHCP server replies to the client, it sends the reply to the GIADDR-address, again using unicast. The relay agent then retransmits the response on the local network, using unicast to the newly reserved IP address and Client MAC address, the client should accept the packet as its own, even when that IP address is not yet set on the interface

If the client's implementation of the IP stack does not accept unicast packets when it has no IP address yet, the client may set the broadcast bit in the FLAGS field when sending a DHCPDISCOVER packet. The relay agent will use the 255.255.255.255 broadcast IP address (and the clients MAC address) to inform the client of the server's DHCPOFFER.

The communication between the relay agent and the DHCP server typically uses both a source and destination UDP port of 67.

Note: And same applies with translation of received unicast offer/ack from the server to broadcast back to the client

These steps show how DHCP requests are processed when DHCP relay is used:

Step 1: A DHCP client broadcasts a DHCP request

Step 2: DHCP relay includes option 82 and sends the DHCP request as a unicast packet to the DHCP server. Option 82 includes remote ID and circuit ID.

Step 3: The DHCP server responds to the DHCP relay

Step 4: The DHCP relay strips off option 82 and sends the response to the DHCP client

Problem when using HSRP and a DHCP relay is configured on each HSRP router, the DHCP traffic will get duplicated. This isn’t a problem under normal circumstances but can lead to unneccessary excessive traffic, etc.

Solution using the keyword redundancy limits the DHCP relay to only be active on the current active FHRP router.

| Router(config-if)# ip helper-address | ## Configuring a DHCP relay (aka IP helper address) on an interface                                                                                                                                                                                                   |
| ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ip forward-protocol udp \<port\_id>  | specifies of the protocol (and destination port) whose broadcasts will be forwarded If IP Helper is started on an interface, the forwarding of UDP broadcasts for several services is automatically turned on: - DHCP (port 67 and 68), TFTP (port 69), DNS (53), ... |

![](<../.gitbook/assets/Unknown image (720)>)

### DHCP server configuration (Cisco IOS)

| ip dhcp pool                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ---------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| network                                                          | // assigns the dhcp pool subnet range                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| default-router                                                   | // this will set the default gateway for end hosts; it is automatically excluded from the pool                                                                                                                                                                                                                                                                                                                                                                                                           |
| lease <> <>                                                      | Specifies the duration of the lease. The default is a one-day lease.                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| dns-server                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ip dhcp excluded-address                                         | // excluding address from the pool to have it reserved or prevented to be assigned; configured in global config mode                                                                                                                                                                                                                                                                                                                                                                                     |
| Router(dhcp-config)# host Router(dhcp-config)# client-identifier | Static DHCPv4 reservations on Cisco IOS require the creation of separate DHCP pools for each reservation. Reservations can be based on either the client's MAC address or client identifier. The client identifier is a Cisco-specific string in hex format, such as "cisco-aabb.cc00.0e00-Et0/0." By default, Cisco devices send the client identifier instead of the MAC address to a DHCP server. When using the MAC address as the identifier, prepend "01" (value for Ethernet) before entering it. |
| show ip dhcp \[ pool \| binding ]                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| show dhcp lease                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| show hosts                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| service dhcp command                                             | DHCP server and relay agent are enabled by default. Use to re-enable the functionality if necessary                                                                                                                                                                                                                                                                                                                                                                                                      |
| (config-if)#ip address dhcp                                      | to configure cisco device interface to obtain address from dhcp server                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| release renew dhcp                                               | to release and renew IP from dhcp                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |

### DHCP on IOS XR

| pool vrf PON ipv4 pool network 10.1.1.0/24 ! dhcp ipv4 profile Profile server lease 10 pool pool default-router 10.1.1.1 ! interface TenGigE0/0/0/5.10 server profile Profile |   |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

![](<../.gitbook/assets/Unknown image (721)>)

### Microsoft Automatic Private IP Addressing (APIPA)

Reserved range 169.254.0.0-169.254.255.255 is assigned automatically to network interface

When a client doesn't get a response to multiple DHCP dicover messages, so they cannot obtain a valid IP address from a DHCP server or they haven't been configured manually with the IP address

This basically provides the startup IP for the station, so it is able to communicate with the devices on the local network - assuming that all other devices fallen into assigned APIPA

Only valid and significant within a specific local network segment or link. It cannot be used for communication beyond the local network segment

![](<../.gitbook/assets/Unknown image (722)>)

## DHCPv6

### Stateful DHCPv6

**Stateful DHCPv6** provides IPv6 addresses and "other information" to hosts. It also keeps track of the state of each assignment. It tracks the address pool availability and resolves duplicated address conflicts. It also logs every assignment and keeps track of the expiration times. However, there are a big differences between DHCPv6 and DHCPv4.

#### Router Advertisements (RA) and flags

In IPv6 the client first detects the presence of routers on the link - If found, the client examines router advertisements to determine if DHCP can be used

In IPv4 DHCP server typically provides default gateway addresses to hosts

In IPv6, only routers sending Router Advertisement messages can provide a default gateway address dynamically and guide hosts whether autoconfigure based on RA or contact DHCPv6 instead

RA Flags are: A=0 O=0 M=1

#### Process (stateful)

Step 1 - PC1 sends out a Router Solicitation message destined to the all-routers multicast address FF02::2.

Step 2 - Upon receiving the RS from PC1, Router 1 generates a Router Advertisement message with the M-flag set to 1 and the A-flag set to 0. This informs PC1 that SLAAC is not allowed on this segment and it must use a Stateful DHCPv6 for addressing and other configuration. Note that RA messages are sent to the all-nodes multicast group FF02::1 and are received by all neighbors on a local segment.

Note If no router is found or if DHCP can be used, the client sends DHCP solicit message to the all-DHCP-agents multicast address

Step 3 - Upon receiving the Route Advertisement, PC1 sets the source IPv6 address of Router 1 (FE80::1) as its default gateway.

Because the A-flag is set to 0, PC1 does not perform SLAAC

Step 4 - Because the M-flag in the RA message is set to 1, PC1 sends out a DHCPv6 SOLICIT message to the all-dhcpv6-servers multicast group FF02::1:2, searching for DHCP server with the link-local as a source address. All servers listen on all of the three multicast addresses and respond to the messages sent to those addresses

Step 5 - Upon hearing the solicit message, the server responds with a DHCPv6 ADVERTISE message. It is destined directly as unicast to the link-local address of PC1.

Step 6 - PC1 then knows that a DHCPv6 service is available and sends out a REQUEST packet asking for addressing information.

Step 7 - Upon receiving the REQUEST, the server responds with a DHCPv6 REPLY that contains the global unicast address and all other information that is available for assignment.

Step 8 - In the end, PC1 performs Duplicate Address Detection (DAD) on the received GUA address to ensure that it is unique.

#### Ports and identifiers

Clients/relays use udp/546 whereas servers use udp/547 for communication

**DHCP Uniquie Identifier (DUID)** is used to identify active clients and their assigned IPv6 address from the pool

The second identification construct is **identity association (IA)**. This is typically a cluster of configuration information assigned to a single interface, bearing a unique identifier (IAID). These identifiers are assigned by the client computer to each interface, for which it wishes to use DHCPv6. Again, they should be consistent and not change over time.

![](<../.gitbook/assets/Unknown image (103)>)

#### Cisco IOS configuration (stateful)

| Command / snippet                                                                                                                                     | Notes                                                                                                                                                                 |
| ----------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ipv6 unicast-routing                                                                                                                                  | Enable IPv6 routing                                                                                                                                                   |
| Configuring a DHCPv6 address pool: `ipv6 dhcp pool` → `address prefix <prefix/length>` → `dns-server` → `domain-name`                                 | Defines the stateful pool options                                                                                                                                     |
| Attaching pool to an interface (stateful): `ipv6 nd managed-config-flag` → `ipv6 nd prefix default no-autoconfig` → `ipv6 dhcp server [rapid-commit]` | Sets the RA M-flag and prevents SLAAC on that link. With `rapid-commit`, only 2 messages (Solicit, Reply) are used instead of 4 (Solicit, Advertise, Request, Reply). |
| show ipv6 dhcp \[pool \| bindings]                                                                                                                    | Verification                                                                                                                                                          |

### DHCPv6 relay

equal to DHCP relay in IPv4. Relay sent as unicast or to the well-known link-local multicast address FF02::1:2 (all DHCPv6 servers)

Note the site-local ff05::1:3 and 4 multicasts are used for communication between servers and relays, in case they request their unicast information for example

#### Cisco IOS configuration (relay)

| (config-if)# ipv6 dhcp relay destination      | Configuring a DHCPv6 relay on an interface (Note configured where DHCP REQUESTS are incoming, e.g on LAN interface) |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| (config-if)# ipv6 dhcp relay source Loopback1 | you can also configure the source interface that is used for communication to the dhcp server                       |

### Message types

| **Code** | **Name**            | **INFO**                                                                                                                                                                       |
| -------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1        | SOLICIT             | Message from host sent to FF02::1:2 (all DHCPv6 relays and servers) to locate DHCPv6 servers (multicast)                                                                       |
| 2        | ADVERTISE           | Message one or several DHCPv6 server(s) to tell the host is has DHCPv6 services (unicast).                                                                                     |
| 3        | REQUEST             | Message from host to server to request configuration parameters In case of stateful DHCPv6 it’s a REQUEST, in case of stateless DHCPv6 it’s an INFORMATION-REQUEST (multicast) |
| 4        | CONFIRM             |                                                                                                                                                                                |
| 5        | RENEW               |                                                                                                                                                                                |
| 6        | REBIND              |                                                                                                                                                                                |
| 7        | REPLY               | Message from server to host with address (if stateful DHCPv6 only) and other configuration parameters (unicast)                                                                |
| 8        | RELEASE             |                                                                                                                                                                                |
| 9        | DECLINE             |                                                                                                                                                                                |
| 10       | RECONFIGURE         |                                                                                                                                                                                |
| 11       | INFORMATION-REQUEST | used t request only specific information from a stateless DHCPv6 server                                                                                                        |
| 12       | RELAY-FORW          |                                                                                                                                                                                |
| 13       | RELAY-REPL          |                                                                                                                                                                                |
| 14       | LEASEQUERY          |                                                                                                                                                                                |
| 15       | LEASEQUERY-REPLY    |                                                                                                                                                                                |
| 16       | LEASEQUERY-DONE     |                                                                                                                                                                                |
| 17       | LEASEQUERY-DATA     |                                                                                                                                                                                |
| 18       | RECONFIGURE-REQUEST |                                                                                                                                                                                |
| 19       | RECONFIGURE-REPLY   |                                                                                                                                                                                |
| 20       | DHCPV4-QUERY        |                                                                                                                                                                                |
| 21       | DHCPV4-RESPONSE     |                                                                                                                                                                                |
| 22       | ACTIVELEASEQUERY    |                                                                                                                                                                                |
| 23       | STARTTLS            |                                                                                                                                                                                |

### Stateless DHCPv6 (Lite)

**Stateless DHCPv6 (Lite)**: Server doesn’t retain or give out any host addresses, hosts use SLAAC address autoconfiguration. Server “only” provides additional options. RA Flags are: A=1 O=1 M=0

![Stateless DCHPv6 steps](<../.gitbook/assets/Unknown image (104)>)

#### Cisco IOS configuration (stateless)

| Router1(config-if)# ipv6 nd other-config-flag Router1(config-if)# ipv6 dhcp server \[rapid-commit]   | Configuring Router to advertise the O flag with dhcpv6 server ip if client sets the rapid commit flag, it tells the server that it wants to speed up the exchange - to use only Solicit and Reply message instead of standard 4-message exchange, so this keyword must be configured on the server to support such flag from clients |
| ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Router2(config)# ipv6 dhcp pool Router(config-dhcpv6)# dns-server Router(config-dhcpv6)# domain-name | Configuring a stateless DHCPv6 server Note use #ipv6 nd ra suppress all command to prevent Router 2 from sending Router Advertisements because Router 1 is responsible for the SLAAC configuration and Router 2 is only acting as a stateless DHCP server                                                                            |

### DHCPv6 Prefix Delegation

**DHCPv6 Prefix Delegation** is used commonly by ISP's to dedicate sub-prefixes (/56) to their customers (dhcp clients) from their global IPv6 prefix /32

![Ipv6 Prefix Delegation Example](<../.gitbook/assets/Unknown image (105)>)

#### Cisco IOS configuration (prefix delegation)

| ISP - Defining a local IPv6 prefix pool Router(config)# ipv6 local pool \[NAME]                                                                                                |                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------- |
| ISP - Configuring a DHCPv6 pool pointing to the local IPv6 prefix pool Router(config)# ipv6 dhcp pool Router(config-dhcpv6)# prefix-delegation pool                            |                                                       |
| ISP - Attaching the DHCPv6 pool to an interface Router(config)# interface Router(config-if)# ipv6 dhcp server \[rapid-commit]                                                  |                                                       |
| Customer - Configuring interface towards the ISP Router(config)# interface Router(config-if)# ipv6 address \[ipv6\_add] Router(config-if)# ipv6 dhcp client pd \[PREFIX\_NAME] | or you can use autoconfig if applicable to the design |
| Customer - Configuring interface towards the local hosts Router(config)# interface Router(config-if)# ipv6 address \[PREFIX\_NAME]                                             |                                                       |
| show ipv6 local pool show ipv6 dhcp \[binding \| interface] debug ipv6 dhcp \[relay \| detail]                                                                                 |                                                       |

![](<../.gitbook/assets/Unknown image (106)>)

### IPv6 general prefix

**IPv6 general prefix**: Since each site have common global prefix from the ISP, it can be defined in the IOS as the general prefix, allowing to specify only last 64 bits of the IPv6 address on an interface.

When the general prefix is changed, all of the more-specific prefixes based on it will change aswell, which significantly simplifies management and renumbering

If you will configure more general prefixes, it will configure address for each prefix

#### Cisco IOS configuration (general prefix)

| (config)# ipv6 general-prefix \[name] 2001:DB8:A1EF::/48 | In this example we configured the general prefix 2001:DB8:A1EF::/48                                                                                            |
| -------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-if)# ipv6 address \[name] ::1:0:0:0:1/64         | Configures IPv6 address on an interface by obtaining the first 48 bits from the general prefix and specifying the last 64 bits. In this example ::1:0:0:0:1/64 |
| show ipv6 general-prefix                                 | Verification                                                                                                                                                   |

## **DNS**

**Domain Name System (DNS)** is a decentralized naming system for computers, services, or any resource connected to the Internet or a private network.

It translates human-readable domain names, also called **Fully Qualified Domain Names (FQDNs)**, like `google.com`, into IP addresses.

For IPv4, hostname-to-address mapping is an `A` record.

For IPv6, hostname-to-address mapping is an `AAAA` record.

Without DNS, you would have to remember the IP address of every host you want to reach.

DNS uses a distributed database hosted on several servers around the world to resolve names associated with IP addresses.

The DNS protocol defines an automated service that matches resource names with numeric network addresses.

### DNS hierarchy

DNS operates in a hierarchy. Root servers delegate to TLD servers, which delegate to authoritative servers. This system ensures that no single server needs to store every domain name/IP address pair, improving scalability and reliability.

**Top-level domain (TLD)** is the topmost domain in the hierarchical DNS of the Internet, or simply the last part of a domain name, such as `.com`, `.cz`, or `.org`.

### DNS namespace

**DNS namespace** represents all names present in the DNS system. Names, such as [www.google.com](http://www.google.com/), are referred to as DNS domains (domain names). DNS domain names are organized hierarchically in a multi-level architecture.

The architecture begins with a root name, which is common to all names. The root name then branches into multiple branches, with each branch starting with a different top-level domain name.

Each domain can have subdomains, which in turn can have subdomains of their own.

The complete name of the resource follows this structuring. Each level has its own name and the complete domain name is composed by aggregating names of all levels.

### Domain registration

**Domain name registration** assigns a specific domain name to a particular resource and sets up relationships between names and addresses. The name registration process ensures that each name is unique.

To ensure uniqueness, name registration is regulated and supervised. The Internet Corporation for Assigned Names and Numbers (ICANN) operates the internet’s DNS. The registration process itself is not centralized but is shared among multiple authorized entities, called registries and registrars. Within DNS registration, authorized entities record assigned domain names and corresponding data into a database. DNS database is distributed among these different authorities. At the same time, the DNS information is stored and made available on DNS servers.

### Domain name resolution

**Domain name resolution** is the process in which a resource name is resolved into a resource IP address. Domain name resolution happens on devices that are connected to the internet hundreds or thousands of times a day.

Users are usually unaware of the name resolution process. The process is integrated into the client programs, such as web browsers, email clients, or FTP clients. The clients accept domain name input by end users, use it to create name resolution requests, and send the requests to servers. Servers, on the other end, accept name resolution requests, look up the answers, and send the answers to the clients.

### Resolution process

In DNS, the resolution is implemented as a client and server process. It involves several entities of the system—DNS client, DNS servers of various types, and DNS distributed database. Clients and servers communicate by using the DNS protocol. The name resolution relies heavily on the information provided in the registration process. The process of resolution is also closely related to how the name space is structured. To find the information, the name resolution process resolves the given domain name one level at a time, until the final level is reached.

1. When a host types [www.google.com](http://www.google.com/) to the browser to access the internet web-service, it first performs a lookup into its cache memory, which is found in windows by typing in cmd ipconfig/displaydns
2. If no record found, it proceeds to send a query to the DNS server
3. The DNS server performs a lookup into it's database to find A record (IPv4-to-hostname), if no record found it initiates Iterative query (asking another DNS server) to other DNS servers in a following order: root domain servers, top-level domain servers and authoritative servers until the IP address is obtained
4. If a record is found it replies with a message containing the IP-to-hostname record back to the host. The host then proceeds to save this record into its own cache, so it can initiate subsequent communications, without querying the DNS server

Note if you want to manually add an DNS entry to the windows host: [How to Edit Hosts File on Windows](https://phoenixnap.com/kb/windows-hosts-file)

![](<../.gitbook/assets/Unknown image (1511)>)

![](<../.gitbook/assets/Unknown image (1512)>)

![](<../.gitbook/assets/Unknown image (1513)>)

### Common DNS record types

DNS doesn’t just handle domain-to-IP mapping.

#### Common records

* `A`: hostname → IPv4 address
* `AAAA`: hostname → IPv6 address
* `CNAME`: alias one name to another
* `MX`: mail exchanger for a domain
* `PTR`: IP address → hostname (reverse DNS)

#### Reverse DNS (PTR)

**PTR records** translate an IP address to a domain name.

IPv4 PTR records are stored under the reversed IP, plus `.in-addr.arpa`.

For example, the PTR record for the IP address `192.0.2.255` would be stored under `255.2.0.192.in-addr.arpa`.

`.in-addr.arpa` is used because PTR records are stored within the `.arpa` top-level domain in DNS. `.arpa` is a domain used mostly for managing network infrastructure, and it was the first top-level domain name defined for the Internet.

#### DNS server roles

Root servers: know about top-level domains (TLDs) like `.com`.

TLD servers: know about second-level domains (SLDs) like `networkchuck.com`.

Authoritative servers: return the IP address for the specific domain or subdomain (like `academy.networkchuck.com`).

Recursive DNS servers (like Google’s public DNS) help by querying other DNS servers to retrieve the needed IP address. They may also cache the results to respond more quickly to future requests.

**Reverse DNS lookup** is a DNS query for the domain name associated with a given IP address.

### DNS in Cisco IOS

| ip name-server | to configure DNS severs; multiple DNS servers can be specified in one line |
| -------------- | -------------------------------------------------------------------------- |
| ip dns server  | enables dns on cisco device                                                |
| show hosts     | to view dns-related database                                               |

The no ip domain lookup command is usually seen in configurations. By default, any single word entered on a command line that is not recognized as a valid command is considered as a hostname by the router, and the router will by default try to telnet to that hostname. This is extremely annoying, especially when you do a simple typo, as the router will try to translate that typo into an IP address. If you do not have a DNS server configured, the command line will stall for several seconds until the DNS request times out.

Quite frankly, it does not make much sense to have both ip name-server and no ip domain lookup configured. The no ip domain lookup tells the router to stop interacting with any DNS servers entirely. Having a DNS server configured is then a useless thing because it is not going to be used, anyway.

What could be considered a more proper way of doing things, however, is this: Have the DNS server configured using the ip name-server command, and at the same time, on all lines (con 0, aux 0, vty 0 15), deactivate the automatic action of telnetting into all "words" that look like hostnames:

| line con 0 transport preferred none line aux 0 transport preferred none line vty 0 15 transport preferred none |                                                                                                                                                                                                             |
| -------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| no ip domain-lookup                                                                                            | disable IP Domain Name System hostname translation, which improves the show command response time (when you mistype any word or something in CLI and press enter, the system won't try to resolve the word) |

### Windows DNS commands

| ipconfig /flushdns       | Flushes the DNS resolver cache, which can be useful for clearing cached DNS records.                                                                                                                                                                                                                                                                         |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| nslookup \<domain\_name> | An easy way to observe DNS in action can be performed in a command window in Microsoft Windows, Apple Mac OS X, or your favorite Linux distribution. When the command window is open, enter nslookup [www.google.com](http://www.google.com/). This command queries DNS to resolve the domain name into IP address. The result will appear below your query. |

### Packet capture examples

#### Host-to-local DNS (pcap)

![](<../.gitbook/assets/Unknown image (1514)>)

IPv4 A record response

![](<../.gitbook/assets/Unknown image (1515)>)

IPv6 AAAA record response

![](<../.gitbook/assets/Unknown image (1516)>)

### DNS security

Standard DNS queries are unencrypted and vulnerable to attacks, such as DNS spoofing or hijacking, where attackers intercept or manipulate DNS traffic. This can redirect users to malicious websites.

**DNS over HTTPS (DoH)** encrypts DNS queries by tunneling them through an HTTPS connection, which makes it harder for third parties to intercept or modify the queries.

Instead of sending DNS queries over plaintext UDP port 53, DoH wraps DNS queries in HTTPS and sends them over port 443 (the same port used for secure web traffic). This makes it indistinguishable from normal web traffic, providing both encryption and obfuscation.

**DNS over TLS (DoT)** is another method that encrypts DNS queries, but it does so by using TLS (Transport Layer Security) rather than HTTPS.

DoT wraps DNS queries in TLS encryption and sends them over a dedicated port (port 853). Like DoH, it ensures DNS queries are encrypted but doesn’t use the same obfuscation techniques as DoH.

**DNSCrypt** is another protocol that encrypts DNS queries between the user's device and the resolver.

DNSCrypt uses elliptic-curve cryptography to encrypt DNS queries, ensuring that no one can eavesdrop or modify the DNS traffic.

**DNS-based Authentication of Named Entities (DANE)** builds on DNSSEC to provide a way of verifying TLS certificates through DNS.

DANE allows domain owners to publish their TLS certificates in DNS using DNSSEC. When a client connects to a service, it can use DNSSEC to verify that the certificate presented by the server matches the one published in DNS.

DANE depends on DNSSEC, so it shares the same challenges of DNSSEC adoption and complexity.

#### DNSSEC (Domain Name System Security Extensions)

**DNSSEC (Domain Name System Security Extensions)** is an extension of the Domain Name System (DNS) that increases its security. DNSSEC provides users with the assurance that the information they obtain from the DNS was provided by the correct source, is complete, and its integrity has not been compromised in transit. DNSSEC ensures the trustworthiness of data obtained from the DNS

If the DNS service is not secured with DNSSEC, it provides a potential attacker with several places where it is possible to disrupt communication and falsify data. By changing domain name data, the attacker will affect the functioning of other Internet services, which he can abuse with this intervention.

If someone manages to spoof a numeric address, the user will unknowingly end up in a completely different location and will not connect to the service they were expecting.

The attacker can then, for example:

obtain other people's e-mails

obtain passwords, access codes or payment card details, etc. using fake websites

bypass anti-spam protection in DNS and send spam

forge messages and information on websites

redirect or eavesdrop on telephone calls made over the Internet.

In most cases, the user has no chance of knowing that something malicious is happening. Thanks to the implementation of DNSSEC, the user will gain confidence that the information he obtained from the DNS was provided by the correct source, is complete and its integrity was not compromised during transmission. DNSSEC ensures the credibility of the data obtained from the DNS.

#### Operation (high-level)

DNSSEC introduces DNS asymmetric cryptography – i.e. using one key to encrypt and another key to decrypt the content. A similar principle is the basis of the better-known message encryption using PGP or signing emails with an electronic signature. In the case of DNSSEC, the domain holder generates a pair of private and public keys. He then uses his private key to electronically sign the technical data about his domain that he enters into the DNS. The public key can then be used to verify the authenticity of this signature. To make this key available to everyone, the holder publishes it for his domain with a superior authority, which for all .cz domains is the .cz domain registry. At the .cz domain registry level, technical data in the DNS is also signed and the public key for this signature is again passed on by the registry administrator to the superior authority. This creates a chain that ensures the credibility of the data, as long as none of its elements are broken and all electronic signatures agree.

### DNS in IPv6

The core concept of DNS is unchanged from IPv4

1. In a dual-stack case, an IPv4-enabled and IPv6-enabled application query the DNS to resolve an domain name.
2. The DNS sends the query response containing all available records.
3. The application on the host will connect to the supported and preferred IP address (IPv6 is by default preferred in new applications). If the connection fails, it will attempt to connect to the other IP version.

DNS server for IPv6 maintains mapping between IPv6 address and a hostname – called “AAAA” record (4 times longer than IPv4)

This means that it is possible to use IPv4 as the network protocol to connect to a DNS server and resolve an IPv6 (AAAA) record

IPv6 reverse maps use a sequence of nibbles separated by dots with the suffix “.IP6.ARPA” as defined in RFC 3596.

For example, the reverse lookup domain name corresponding to the address 2001:db8:1234:1a00:1:2:3:4 would be 4.0.0.0.3.0.0.0.2.0.0.0.1.0.0.0.0.0.a.1.4.3.2.1.8.b.d.0.1.0.0.2.ip6.arpa.

Note If Windows does not obtain any DNS server IPv6 addresses, it configures default legacy site-local addresses fec0:0:0:ffff::1, fec0:0:0:ffff::2, and fec0:0:0:ffff::3, similar to how APIPA works for IPv4

![](<../.gitbook/assets/Unknown image (1517)>)

DNS64

![](<../.gitbook/assets/Unknown image (1518)>)

### Multicast DNS (mDNS)

**Multicast DNS (mDNS)** is a service that allows devices on a local network to automatically discover each other and resolve hostnames to IP addresses without requiring a traditional Domain Name System (DNS) server.

### Proxy servers

**Proxy servers** are an intermediary server between a client (app or web browser) and another server

When a client requests a resource (web page or file) from another server, the request is first sent to the proxy server, which then forwards the request to the target server on behalf of the client.

The target server responds to the request, and the response is sent back to the proxy server, which then relays the response to the client.

Reverse Proxy Protect servers instead. So the client doesn't know to which server it is connected to

Benefits

Improving performance: By caching frequently accessed resources, a proxy server can reduce the amount of network traffic and improve the speed of access to resources.

Filtering content: Proxy servers can be configured to filter content based on various criteria, such as IP address, domain name, URL, or content type.

This can be used to block access to specific websites, restrict access to certain types of content, or prevent access to malicious sites.

Anonymizing requests: Proxy servers can be used to hide the client's IP and other identifying information

Load balancing: Proxy servers can distribute incoming requests across multiple servers, which can improve performance and ensure high availability.

### Load balancers (LB)

**Load balancers (LB)** distribute network traffic across multiple servers or resources to ensure optimal resource utilization, high availability, and improved performance. Load balancers are commonly used in web applications, database clusters, and Content delivery networks (CDNs).

Vendors: Cisco, F5 Networks, Citrix, and Nginx are some popular vendors offering load balancing solutions.

![](<../.gitbook/assets/Unknown image (1519)>)

### Application Layer Gateways (ALG)

**Application Layer Gateways (ALG)** are specialized network devices or software modules that operate at the application layer of the OSI model. They intercept and modify application-specific traffic to provide various functionalities, such as:

NAT (Network Address Translation): Translating private IP addresses to public IP addresses and vice versa.

Firewalling: Blocking or allowing specific types of traffic based on rules.

Quality of Service (QoS): Prioritizing or limiting certain types of traffic.

Application-Specific Features: Providing features tailored to specific applications (e.g., FTP, HTTP, VoIP).

### Chromecast (example)

**Chromecast** is a small device developed by Google that lets you stream media (like videos, music, or photos) from your phone, tablet, or computer to a TV or speakers. You plug it into your TV's HDMI port, connect it to your Wi-Fi, and then "cast" content to it using apps like YouTube, Netflix, or Spotify. It supports mDNS (Bonjour) for device discovery.

### mDNS (Bonjour) across VLANs

The configuration enables device discovery across different network segments (VLANs) using Cisco routers or switches. Normally, devices like printers, Chromecasts, or Apple AirPlay devices use mDNS (Multicast DNS)—also known as Bonjour in Apple terms—to find each other on the same network. But mDNS traffic doesn’t naturally cross VLAN boundaries, so devices in VLAN 3 can’t see devices in VLAN 6.

This setup turns a Cisco router or switch into an mDNS gateway, allowing it to forward mDNS traffic between VLANs (e.g., VLAN 3, 6, and 17 in your example). As a result:

A Chromecast in VLAN 3 can be discovered by a phone in VLAN 6.

A printer in VLAN 17 can be seen by a laptop in VLAN 3.

Apple devices using AirPlay can find each other across VLANs.

Eliminates VLAN Barriers: Without this, devices in different VLANs can’t discover each other using mDNS, which is a problem in segmented networks (common in offices, schools, or homes with multiple VLANs for security or organization).

Replaces Clunky Alternatives: The common workaround is to use a Linux VM running Avahi to reflect mDNS traffic, but that’s messy—you need a VM with an interface in each VLAN. The Cisco solution is cleaner and uses existing hardware.

Practical Example: If you’re in an office with a printer in VLAN 3 and employees in VLAN 6, this configuration lets those employees find and print to the printer without needing to be in the same VLAN.

How It Works (Simplified):

The Cisco device listens for mDNS traffic (like “Hey, I’m a printer!”) in each VLAN.

It forwards that traffic to the other VLANs, so devices in different VLANs can “hear” each other.

The redistribute mdns-sd option makes it automatically broadcast mDNS traffic across VLANs, but you need to be careful to avoid loops if another reflector is running.

## Network Time Synchronization

Keeping device time consistent matters for certificates, logs, and troubleshooting. Use UTC everywhere. Use local timezone settings only for display.

### System clock fundamentals

Routers, switches, and firewalls track time with an internal system clock. It starts at boot and runs continuously. It tracks time internally in Coordinated Universal Time (UTC).

The system clock can be **authoritative** or **non-authoritative**. Non-authoritative time is for display only. It should not be redistributed.

### Network timing

Modern infrastructure needs stable and consistent timing. Network timing lowers cost versus end-site timing equipment.

Agile Metro components support Precision Time Protocol (PTP). They often support Class C timing accuracy.

Use G.8275.1 when possible for highest accuracy. IOS-XR supports G.8275.1 and G.8275.2 interworking. Use Synchronous Ethernet (SyncE) to stabilize timing to the PRC.

### Clocks on Cisco IOS

Most Cisco IOS devices have two clocks:

* **Software clock** (system clock)
* **Hardware clock** (calendar / RTC)

#### Software clock (system clock)

The software clock initializes at boot from the hardware clock. It tracks seconds and microseconds since boot.

To set the system clock manually, use `clock set` in privileged EXEC mode. Set date and time in UTC. Configure timezone and DST separately.

| Command                                                                | Notes                                                                       |
| ---------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| `clock timezone CET 1 0`                                               | Sets timezone to CET (UTC+1).                                               |
| `clock summer-time CEST recurring last Sun Mar 2:00 last Sun Oct 3:00` | Recurring DST for CET. Switches to CEST (UTC+2).                            |
| `show clock [detail]`                                                  | Shows current software clock time. NTP syncs the software clock by default. |
| `show calendar`                                                        | Shows current hardware clock time.                                          |

In logs and `show clock` output:

* `(*)` means NTP is not configured.
* `(.)` means NTP was synced before, but is currently unreachable.

#### Hardware clock (calendar / RTC)

The hardware clock is backed by a rechargeable battery. It retains date and time across reboots.

It is usually updated from the software clock. This happens after the software clock syncs to an authoritative source.

Avoid setting the hardware clock if you have a reliable external time source.

| Command                                          | Notes                                                                         |
| ------------------------------------------------ | ----------------------------------------------------------------------------- |
| `clock calendar-valid` / `clock update-calendar` | Trust RTC after reload. Also update RTC when software clock is authoritative. |

### Network Time Protocol (NTP)

NTP synchronizes clocks using a distributed client/server model. It uses UDP port `123`. The local clock IP is `127.127.1.1`.

NTP uses a hierarchy of **stratums**:

* Stratum 0: reference clocks (GPS, atomic clocks)
* Stratum 1: servers directly attached to stratum 0
* Stratum 2+: clients/servers downstream

NTP time sources can be:

* Local master clock
* Internet master clocks (for example `ntp.org`)
* GPS or atomic clocks (stratum 0)

NTP can traverse multiple hops. Accuracy improves slowly. Milliseconds of accuracy can take hours or days.

Configure timezone and initial clock settings first. They are used until NTP fully synchronizes.

![Konfigurace NTP serveru v Linuxu - Martinův život Linux](<../.gitbook/assets/Unknown image (1145)>)

#### NTP modes

* **Server**: provides time to clients.
* **Client**: synchronizes to an NTP server.
* **Peer**: exchanges time with other peers.
* **Broadcast/Multicast**: push mode from a server.

Private UTC-synced master clocks are the most secure option. Internet sources are easier but less secure.

NTP does not synchronize to an unsynchronized device. It also avoids sources whose time differs significantly from others.

![](<../.gitbook/assets/Unknown image (1146)>)

#### NTP and clock configuration (IOS-XE)

Configure clock settings and NTP together. This improves timekeeping during convergence.

Reference: [https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/syst-mgmt/b-system-management/m\_bsm-time-calendar-set.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/syst-mgmt/b-system-management/m_bsm-time-calendar-set.html)

**Common IOS-XE configuration snippets**

| Config                                                                    | Notes                                                        |
| ------------------------------------------------------------------------- | ------------------------------------------------------------ |
| `ntp server vrf Mgmt-vrf 162.159.200.1 prefer`                            | Sets preferred NTP server in management VRF.                 |
| `ntp source <interface>`                                                  | Source NTP from a stable interface (often a loopback).       |
| `ntp master [stratum]`                                                    | Makes the device an NTP master. Use with caution.            |
| `ntp authenticate` + `ntp authentication-key ...` + `ntp trusted-key ...` | Enables NTP authentication. Configure on server and clients. |
| `show ntp [associations\|status]`                                         | Shows NTP state and peer associations.                       |

#### IOS-XR notes

| Config                             | Notes                            |
| ---------------------------------- | -------------------------------- |
| `clock timezone CET Europe/Prague` | Sets timezone.                   |
| `ntp server <ip>`                  | NTP sync can take a few minutes. |

### Precision Time Protocol (PTP)

NTP is typically accurate to under \~10 ms. PTP can achieve sub-microsecond accuracy. It is often measured in nanoseconds.

Use PTP when you need tight timing:

* Energy billing (peak and off-peak)
* High-frequency trading
* Industrial automation
* Audio/video synchronization

PTP runs over Ethernet and UDP. Profiles exist because industries need different behavior.

#### Profiles

* **Default profile**: standard IEEE 1588 behavior.
* **Telecom profile**: ITU-T G.8265.1, G.8275.1, G.8275.2.
* **Power profile**: IEEE C37.238 for power grids.
* **802.1AS**: AVB timing profile for audio/video over Ethernet.

#### Delays that matter

* **Propagation delay**: signal travel time in the medium.
* **Queueing delay**: buffering and congestion delay.
* **Processing delay**: parsing, lookups, and switching time.

NIC hardware timestamping improves accuracy. Software timestamping adds variable delay. Closer to the physical layer is better.

![](<../.gitbook/assets/Unknown image (1147)>)

#### Time scale: Unix epoch vs TAI vs UTC

PTP uses the Unix epoch starting at `00:00:00` on 1 January 1970. It synchronizes with International Atomic Time (TAI), not UTC.

UTC has leap seconds. TAI does not. That makes TAI more stable for precise timekeeping.

#### Message exchange: one-step vs two-step

**Hardware PTP** often uses one-step. The Sync message includes the T1 transmit timestamp.

**Software PTP** often uses two-step. The Sync omits T1. A Follow\_Up carries T1 immediately after.

The slave then exchanges Delay\_Req / Delay\_Resp to get T4.

To sync, the slave computes:

```
delay  = ((t2 - t1) + (t4 - t3)) / 2
offset = ((t2 - t1) - (t4 - t3)) / 2
```

The slave updates its clock using the computed offset. Network delay changes over time. The master keeps sending Sync messages.

![Ptp Master Slave Clock Synchronization Messages](<../.gitbook/assets/Unknown image (1148)>)

#### PTP messages

**Event messages (timestamped)**

* **Sync**: periodic timing from the master.
  * One-step: contains T1 in the Sync.
  * Two-step: Sync has no timestamp.
* **Follow\_Up**: sent in two-step mode with T1.
* **Delay\_Req**: slave requests path delay measurement.
* **Delay\_Resp**: master replies with T4.

**General messages (not timestamped)**

* **Announce**: clock quality and priority for BMCA decisions.
* **Management**: access management data (MIB).
* **Signaling**: negotiate intervals and other non-critical parameters.

#### Clock types and roles

PTP uses a master-slave hierarchy. Clocks sync by exchanging timestamped messages.

Each interface can take a **master (M)** or **slave (S)** role. A clock can be master on one interface and slave on another.

**Grandmaster clock (GMC)**

Primary source of time in the domain. It is typically locked to GPS or an atomic clock. It always acts as master on its interface(s).

**Ordinary clock (OC)**

Runs PTP on a single interface. It is usually an end device that needs synchronization.

**Boundary clock (BC)**

Runs PTP on two or more interfaces. Upstream interface is slave toward the grandmaster. Downstream interfaces are master toward other clocks.

BCs improve scale. They prevent all OCs from talking to the grandmaster directly.

![Ptp Boundary Clock Two Ordinary Clocks Vlans](<../.gitbook/assets/Unknown image (1149)>)

**Transparent clocks**

Transparent clocks forward PTP messages. They are not a time source. They typically operate within a VLAN.

They measure residence time and update the correction field.

**End-to-end transparent clock (E2E)**

Sits between the grandmaster and ordinary clock. It forwards PTP messages and accounts for residence time.

![Ptp Transparent Clock End To End Time Sync](<../.gitbook/assets/Unknown image (1150)>)

**Peer-to-peer transparent clock (P2P)**

Measures delay per link, per interface. This scales better than end-to-end delay measurement.

Peer delay messages stay on a single hop. Ordinary clocks do not send delay messages to the grandmaster.

![Ptp Transparent Clock Peer To Peer Time Sync](<../.gitbook/assets/Unknown image (1151)>)

#### Best Master Clock Algorithm (BMCA)

Clocks compare Announce messages to pick the best grandmaster. Comparison uses this order:

1. **Priority1** (0–255, manual)
2. **Class** (source type, for example GPS)
3. **Accuracy** (lower is better)
4. **Variance** (lower is better)
5. **Priority2** (0–255, manual)
6. **Identity** (unique clock identifier, often MAC-derived)

After selection, the master sends Sync at regular intervals. If a better master appears, roles switch.

## **Network Address Translation (NAT)**

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
