# DHCP

Imagine that your company has a large number of devices that need to be assigned an IP address to communicate with each other.

We can manually assign an IP address to each device, but what if our company has several networks, spread all over the world, and each network is growing with more and more devices. For this reason, manually configuring IP addresses and maintaining a manual allocation database is very inefficient and time consuming

DHCP can greatly decrease the workload of the network administrator. DHCP automatically assigns an IPv4 address from an IPv4 address pool that the administrator defines. However, DHCP is much more than just a mechanism that allocates IPv4 addresses. This service automates the assignment of IPv4 addresses, subnet masks, gateways, and other required networking parameters.

**Stateful address assignment** keeps a track of assigned IP addresses and the address pool availability and resolves duplicated address conflicts. It also logs every assignment and keeps track of the expiration times.

### Dynamic Host Configuration Protocol (DHCP)

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
