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

# ICMP

**ICMP** is a supporting protocol in the TCP/IP protocol suite. It is used by network devices, including routers, to send error messages and operational information indicating, for example, that a requested service is not available or that a host or router could not be reached.

ICMP uses the support of IP as if it were a higher-level protocol—However, ICMP is integral to IP. Although standard IP packets contain ICMP messages, they are usually processed separately, from IP processing

ICMP is a network layer protocol. There is no TCP or UDP port number associated with ICMP packets because these numbers are associated with the transport layer above.

If the packet is not successfully delivered to the receiver, the last device, that were not able to send it to the final destination sends ICMP message back to the sender informing him about the unsuccessful delivery with additional information

Other situations in which network devices use ICMP are when a datagram cannot reach its designated destination, when a network device does not have enough buffering capacity to accommodate the datagram, or when a gateway can redirect the host to use a shorter path to the destination. Because IP was not designed with absolute reliability, ICMP allows the devices to receive feedback on the traffic they are sending and any problems that might occur.

ICMP's mechanism is used by networking tools such as ping and traceroute to test the IP reachability and provide information how packet is transmitted through the network

{% hint style="info" %}
If an ICMP packet is lost or dropped along the way, no additional message is generated. This prevents infinite ICMP loops that would worsen congestion.
{% endhint %}

### ICMP header format

The ICMP header starts after the IPv4 header (indicated in IP header as protocol number 1) since the ICMP messages are encapsulated in IPv4 packets. The first 4 bytes of the ICMP header are fixed in the following format:

**Type (8 bits)**: Specifies the type of ICMP message, indicating its purpose or function. Common types include Echo Reply, Destination Unreachable, Echo Request (Ping), Time Exceeded, Parameter Problem, etc

**Code (8 bits)**: Describes additional details or context for the ICMP message specified in the Type field. The interpretation of the Code field depends on the specific Type of ICMP message.

**Checksum (16 bits)**: Used for error-checking of the ICMP header and data. The checksum is calculated over the entire ICMP message, including the ICMP header and data.

![](<../.gitbook/assets/Unknown image (850)>)

**Identifier (16 bits)**: Present in Echo Request and Echo Reply messages, the Identifier field is used to match requests with replies. The sender assigns a unique identifier value to each ICMP Echo Request, and the corresponding ICMP Echo Reply includes the same identifier to facilitate matching.

**Sequence Number (16 bits)**: Also found in Echo Request and Echo Reply messages, the Sequence Number field provides additional sequencing information. Each Echo Request message increments the Sequence Number, allowing the sender to track the order of requests and replies.

![](<../.gitbook/assets/Unknown image (851)>)

| ICMP Type | Meaning and Code Values                                                                                                                                                                                     |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0         | Echo-reply                                                                                                                                                                                                  |
| 3         | Destination unreachable code 0 = net unreachable 1 = host unreachable 2 = protocol unreachable 3 = port unreachable 4 = fragmentation needed and DF set 5 = source route failed                             |
| 4         | Source-quench                                                                                                                                                                                               |
| 5         | Redirect code 0 = redirect datagrams for the network 1 = redirect datagrams for the host 2 = redirect datagrams for the type of service and network 3 = redirect datagrams for the type of service and host |
| 6         | Alternate-address                                                                                                                                                                                           |
| 8         | Echo                                                                                                                                                                                                        |
| 9         | Router-advertisement                                                                                                                                                                                        |
| 10        | Router-solicitation                                                                                                                                                                                         |
| 11        | Time-exceeded code 0 = time to live exceeded in transit 1 = fragment reassembly time exceeded                                                                                                               |

### ICMP error message types

Destination Unreachable

If the destination traffic is unreachable according to the router’s routing table information, it will send back a Destination Unreachable message to the source. The message is also sent if the traffic must be fragmented but the “Don’t Fragment” flag is set to on.

There is a rate limit to how many unreachable messages can router send to the sender

| (config-if)# no ip unreachables  | Typically, when an IP datagram is dropped, an Internet Control Message Protocol (ICMP) unreachable message is sent back to the source giving the reason why the packet could not be delivered to its final destination. In most cases, when traffic is deliberately dropped by being forwarded to a null interface, you do not want to overburden the router by making it send this unreachable message to the source address. Also, these messages would create additional traffic on the network and inform the source that the packets are being dropped. So, it is recommended that when a Null0 interface is created at the edges, the ICMP unreachable message is disabled for this interface |
| -------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ip icmp rate-limit unreachable n | If ICMP unreachable messages are not disabled, it is strongly recommended that they be rate limited so they will not flood the network. One way to rate-limit ICMP unreachable messages is by using the command. In this command, n specifies the number of milliseconds between two consecutive ICMP unreachable messages. A sample configuration for an edge router is shown below                                                                                                                                                                                                                                                                                                                |

![](<../.gitbook/assets/Unknown image (852)>)

Parameter Problem:

If the device processing the datagram finds an issue with one of the header’s parameters, it will send back a message to the source. For example, if an option is required but not set in the header or if the option contains incorrect values.

ICMP Redirect

are used by routers to inform a host that there is a better route for a particular destination. This helps optimize routing in local network. Can be disabled with #no ip redirects

![](<../.gitbook/assets/Unknown image (853)>)

ICMP Time Exceeded

is used to inform a sender that a router couldn't forward the packet because the IP TTL expired

Or to inform a sender that a device had to discard a fragmented packet because not all fragments arrived in time

![](<../.gitbook/assets/Unknown image (854)>)

### Echo request/reply (ping)

One of the most well-known uses of ICMP Echo Request and reply is the Ping and traceroute utilities used for troubleshooting

Packet Internet Groper (Ping) is a utility that sends series of ICMP echo requests to another IP address to check it's reachability with the round-trip time (RTT)

The ping command first sends an echo request packet to an address, then waits for a reply. The ping is successful only if the echo request gets to the destination, and the destination is able to send an echo reply to the source within a predetermined time called a timeout. The default value of this timeout is 2 seconds on Cisco devices.

The computer with target IP address should reply with an ICMP echo reply. If that works, you successfully have tested the IP network connectivity

Ping uses two ICMP message types: Echo Request (Type 8) and Echo Reply (Type O).

Identifier is used by a device to keep track of consecutive pings it sends. The Reply messages will use the same Identifier as the Request messages.

Sequence is used to keep track of each Request and Reply exchange in a series

Request 1 = Sequence 0, Reply 1 = Sequence 0. Request 2 = Sequence 1, Reply 2 = Sequence 1, etc.

The Payload is usually just a string of ASCII characters.

When you send a ping with a specific size, the payload of the ICMP packet is filled with a pattern of data. This pattern is typically generated by the ping command to create a payload of the desired size, even if there is no real data to transmit. The actual pattern used can vary depending on the operating system and the implementation of the ping command

A ping can be sent with no Payload. The message size will be 28 bytes (IP Header + ICMP Header).

In this case Layer 2 will have to add 18 bytes of padding (minimum Ethernet payload size is 46 bytes).

![](<../.gitbook/assets/Unknown image (855)>)

| ping \[probe 1] | The IP of outgoing interface is used, if no source IP is specified in ping probe <> to run single probe to prevent to show both paths if there is ECMP in case of two equal cost paths Extended ping offers additional options to test L3/L4 connectivity, specify different source IP or to specify IP header flags. To execute extended ping > Press enter after specifying the ping in CLI Note when performing ping test from router with multiple subnets, it is a best practise to always use source interface of desired subnet |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

In the last two scenarios, devices that experience failure, such as network unreachable or TTL exceeded in transit, create a new ICMP flow. These new flows have the source IP address of the device that experiences failure. In the case of the TTL expired in transit failure, router RB creates the new flow and sends the ICMP error message to the IP address of the requesting device, the blue workstation. In the case of network unreachable condition on router RA, the router becomes the source of the ICMP flow and sends the ICMP error message to the originating device, the blue workstation.

![](<../.gitbook/assets/Unknown image (856)>)

![](<../.gitbook/assets/Unknown image (857)>)

For example, after sending ICMP echo requests, if an ICMP echo reply packet is received within the default, 2-second (configurable) timeout, an exclamation point (!) is the output, meaning that

the reply was received before the timeout expired. A period (.) is the output if the reply was not received before the timeout expired.

| Return Code (Cisco CLI) | Description                                                                 |
| ----------------------- | --------------------------------------------------------------------------- |
| !                       | Success:Packet reached its destination, and an ICMP Echo Reply was received |
| .                       | Timeout:No response was received within the timeout period                  |
| U                       | Unreachable:Corresponds to ICMP Type 3 (Destination Unreachable)            |
|                         | - Code 0: Network Unreachable                                               |
| H                       | - Code 1: Host Unreachable                                                  |
|                         | - Code 2: Protocol Unreachable                                              |
|                         | - Code 3: Port Unreachable                                                  |
|                         | - Code 4: Fragmentation Needed (MTU Issue, DF Flag Set)                     |
|                         | - Code 5: Source Route Failed                                               |
| M                       | Could Not Fragment: ICMP Type 3, Code 4 (Fragmentation Needed)              |
| ?                       | Unknown Packet Type:An unrecognized response was received                   |
| & or T                  | TTL Exceeded:ICMP Type 11, Code 0 (Time-to-Live expired in transit)         |
|                         | - Code 1: Fragment reassembly time exceeded                                 |
| Q                       | Destination Too Busy:Indicates congestion or queuing at the destination     |

### Traceroute

is used to test the path that packets take through the network. It sends out either an ICMP echo request (Microsoft Windows) or UDP (most implementations) messages, gradually

increasing IPv4 TTL values to probe the path by which a packet traverses the network. The first packet with the TTL set to 1 will be discarded by the first-hop router, which will send an ICMP "time

exceeded" message sourced from its IPv4 address. The device that initiated the traceroute therefore knows the address of the first-hop router. When the TTL is set to 2, the packets will arrive

at the second router, which will respond with an ICMP "time exceeded" message from its IPv4 address. This process continues until the message reaches its final destination; the destination

device will return either an ICMP echo reply (Windows) or an ICMP port unreachable, indicating that the request or message has reached its destination.

Cisco traceroute works by sending a sequence of three packets for each TTL value, with different destination UDP ports, which allows it to report routers that have multiple, equal-cost paths to the

destination. For example, the first three packets with TTL 1 use UDP ports 33434 (first packet), 33435 (second packet), and 33436 (third packet). The next three UDP datagrams are sent with a

TTL of 2 to destination ports 33437, 33438, and 33439.

![](<../.gitbook/assets/Unknown image (858)>)

Traceroute in the internal network

![](<../.gitbook/assets/Unknown image (859)>)

Traceroute Behavior – Last Hop Response from Outgoing Interface

Context:

When performing a traceroute from PE-001-IOS to the remote loopback 172.16.100.2, the final response in the traceroute output shows 172.16.10.2, not 172.16.100.2.

![](<../.gitbook/assets/Unknown image (860)>)

![](<../.gitbook/assets/Unknown image (861)>)

Reason for This Behavior:

Traceroute operates by incrementally increasing the TTL (Time-To-Live) of probe packets.

Each router that decrements the TTL to 0 sends back an ICMP Time Exceeded message to the source.

When the packet finally reaches the destination, the TTL expires just before the destination device processes it at the incoming interface

The ICMP TTL Expired message is therefore generated by the router’s outgoing interface (the interface where the packet arrived), not by the logical interface or loopback address you were targeting.

Hence, the last responding address corresponds to the interface on which the probe arrived — 172.16.10.2 in this case — and not the actual loopback IP (172.16.100.2) that was the intended target.

Traceroute through internet

![](<../.gitbook/assets/Unknown image (862)>)

Hop # RTT 1 RTT 2 RTT 3 Hostname/IP Address

4 11 ms 13 ms 13 ms 142.250.264.176

Hop Number/TTL Value refers to each device the trace passes through with the TTL value that the traceroute prepended to the ICMP packet

Round trip time (RTT) measure for packet to reach given hop and return to source computer.

There are three columns because the traceroute sends three separate ICMP packets to display RTT consistency for given hop

Hostname/IP of the device

Sudden increase in a hop that keeps increasing to the destination, indicates an issue starting at the hop with the increase

| Return Code (Cisco CLI) | Description                                                                  |
| ----------------------- | ---------------------------------------------------------------------------- |
| !                       | Success:Packet reached its destination, and an ICMP Echo Reply was received. |
| .                       | Timeout:No response was received within the timeout period.                  |
| U                       | Unreachable:Corresponds to ICMP Type 3 (Destination Unreachable).            |
|                         | - Code 0: Network Unreachable                                                |
|                         | - Code 1: Host Unreachable                                                   |
|                         | - Code 2: Protocol Unreachable                                               |
|                         | - Code 3: Port Unreachable                                                   |
|                         | - Code 4: Fragmentation Needed (MTU Issue, DF Flag Set)                      |
|                         | - Code 5: Source Route Failed                                                |
| M                       | Could Not Fragment:ICMP Type 3, Code 4 (Fragmentation Needed).               |
| ?                       | Unknown Packet Type:An unrecognized response was received.                   |
| & or T                  | TTL Exceeded:ICMP Type 11, Code 0 (Time-to-Live expired in transit).         |
|                         | - Code 1: Fragment reassembly time exceeded.                                 |
| Q                       | Destination Too Busy:Indicates congestion or queuing at the destination.     |

![](<../.gitbook/assets/Unknown image (863)>)

If a hop's RTT increases, but remain consistent throughout the rest of the traceroute, this does not indicate an issue

Timeouts at the very beginning of the traceroute might indicate that given hop does not respond to the ICMP requests

Timeouts at the end may occur because of the target's firewall that may be blocking ICMP requests.

The target is still most probably reachable with a normal HTTP request or service

| tracert -d 8.8.8.8 | Traceroute in Windows -d is optional: it tells the PC to attempt to resolve the IP addresses to host names. This makes the Traceroute much taster. |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |

| traceroute numberic | Traceroute in IOS numeric keyword is the same as -d in windows |
| ------------------- | -------------------------------------------------------------- |

{% hint style="info" %}
When performing a traceroute to a specific IP (for example a loopback), the final reply might come from the device’s outgoing interface IP rather than the loopback itself.
{% endhint %}

Below is one example of this behavior, where POC-B router sends ICMP echo reply to directly connected POC-A router with it's outgoing interface IP, which connectes them, and not it's loopback IP, which we were testing with ping

![](<../.gitbook/assets/Unknown image (864)>)

![](<../.gitbook/assets/Unknown image (865)>)

![](<../.gitbook/assets/Unknown image (866)>)

{% hint style="info" %}
Sometimes you’ll ping a host across multiple networks and see low latency. But traceroute can show high latency at some intermediate hops.

This usually does not mean there’s a real network issue. Providers protect router control planes. Routers prioritize forwarding real traffic. They rate-limit ICMP/traceroute responses.

So traceroute hops can look “slow” while end-to-end traffic stays fine.
{% endhint %}

### TCLSH macro ping test

are features with which you can automate repetitive tasks, perform bulk operations, and streamline troubleshooting procedures on Cisco devices

show ip alias command will show you all active IP addresses on your device. You can also use show ip interface brief | exclude unassigned

| Config                                                                                                                                                                                          |                                                                                                |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| enable                                                                                                                                                                                          |                                                                                                |
| tclsh                                                                                                                                                                                           | To ping entire subnet                                                                          |
| set pingDestinations {10.19.55.201 10.19.55.202 10.19.55.203 10.19.55.204 10.19.55.205 10.19.55.206 10.19.55.207 10.19.55.208 10.19.55.209 10.19.55.210 10.19.55.211 10.19.55.212 10.19.55.213} | for {set i 1} {$i <= 254} {incr i} { set var 10.19.55. append var $i ping $var rep 3 time 1} } |
| set pingCount 5                                                                                                                                                                                 |                                                                                                |
| set pingTimeout 2                                                                                                                                                                               |                                                                                                |
| set pingSize 100                                                                                                                                                                                |                                                                                                |
| foreach destination $pingDestinations { ping $destination repeat $pingCount timeout $pingTimeout size $pingSize }                                                                               |                                                                                                |
| tclquit                                                                                                                                                                                         | you need to type tclquit to exit TCLSH scripting                                               |

### Path MTU discovery (PMTUD)

ICMP is involved in PMTUD, a mechanism that helps determine the largest packet size that can traverse a network path without fragmentation.

It can inform the sending host that the datagram has been dropped with an ICMP Destination Unreachable message with the status code Fragmentation needed and DF set

An extra field in the ICMP response indicates the maximum MTU the sending router could support on the outgoing link.

### ICMPv6

**ICMPv6** is ICMP implementation for IPv6 with the same message structure and same informational types. ICMPv6 is employed by the NDP and integrates ARP,ICMP and IGMP functionality

| **Type**                          |                                                                                                                                                                  | **Code**  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Value**                         | **Meaning**                                                                                                                                                      | **Value** | **Meaning**                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| **ICMPv6 Error Messages**         |                                                                                                                                                                  |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 1                                 | [Destination unreachable](https://en.wikipedia.org/wiki/Internet_Control_Message_Protocol#Destination_unreachable)                                               | 0         | no route to destination                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                   |                                                                                                                                                                  | 1         | communication with destination administratively prohibited                                                                                                                                                                                                                                                                                                                                                                                                                    |
|                                   |                                                                                                                                                                  | 2         | beyond scope of source address                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|                                   |                                                                                                                                                                  | 3         | address unreachable                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|                                   |                                                                                                                                                                  | 4         | port unreachable                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|                                   |                                                                                                                                                                  | 5         | source address failed ingress/egress policy                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                                   |                                                                                                                                                                  | 6         | reject route to destination                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                                   |                                                                                                                                                                  | 7         | Error in Source Routing Header                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| 2                                 | [Packet too big](https://en.wikipedia.org/wiki/IPv6_packet#Fragmentation)                                                                                        | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 3                                 | [Time exceeded](https://en.wikipedia.org/wiki/Internet_Control_Message_Protocol#Time_exceeded)                                                                   | 0         | hop limit exceeded in transit                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                   |                                                                                                                                                                  | 1         | fragment reassembly time exceeded                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| 4                                 | Parameter problem                                                                                                                                                | 0         | erroneous header field encountered                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|                                   |                                                                                                                                                                  | 1         | unrecognized Next Header type encountered                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                                   |                                                                                                                                                                  | 2         | unrecognized IPv6 option encountered                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| 100                               | Private experimentation                                                                                                                                          |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 101                               | Private experimentation                                                                                                                                          |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 127                               | Reserved for expansion of ICMPv6 error messages                                                                                                                  |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| **ICMPv6 Informational Messages** |                                                                                                                                                                  |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 128                               | [Echo Request](https://en.wikipedia.org/wiki/Echo_Request)                                                                                                       | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 129                               | [Echo Reply](https://en.wikipedia.org/wiki/Echo_Reply)                                                                                                           | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 130                               | Multicast Listener Query ([MLD](https://en.wikipedia.org/wiki/Multicast_Listener_Discovery))                                                                     | 0         | <p>There are two subtypes of Multicast Listener Query messages:</p><ul><li><p></p><ul><li>General Query, used to learn which multicast addresses have listeners on an attached link.</li><li>Multicast-Address-Specific Query, used to learn if a particular multicast address has any listeners on an attached link.</li></ul></li></ul><p>These two subtypes are differentiated by the contents of the Multicast Address field, as described in section 3.6 of RFC 2710</p> |
| 131                               | Multicast Listener Report ([MLD](https://en.wikipedia.org/wiki/Multicast_Listener_Discovery))                                                                    | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 132                               | Multicast Listener Done ([MLD](https://en.wikipedia.org/wiki/Multicast_Listener_Discovery))                                                                      | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 133                               | Router Solicitation ([NDP](https://en.wikipedia.org/wiki/Neighbor_Discovery_Protocol))                                                                           | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 134                               | Router Advertisement ([NDP](https://en.wikipedia.org/wiki/Neighbor_Discovery_Protocol))                                                                          | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 135                               | Neighbor Solicitation ([NDP](https://en.wikipedia.org/wiki/Neighbor_Discovery_Protocol))                                                                         | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 136                               | Neighbor Advertisement ([NDP](https://en.wikipedia.org/wiki/Neighbor_Discovery_Protocol))                                                                        | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 137                               | Redirect Message ([NDP](https://en.wikipedia.org/wiki/Neighbor_Discovery_Protocol))                                                                              | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 138                               | Router Renumbering [<sup>\[3\]</sup>](https://en.wikipedia.org/wiki/ICMPv6#cite_note-rfc2894-3)                                                                  | 0         | Router Renumbering Command                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|                                   |                                                                                                                                                                  | 1         | Router Renumbering Result                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                                   |                                                                                                                                                                  | 255       | Sequence Number Reset                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 139                               | ICMP Node Information Query                                                                                                                                      | 0         | The Data field contains an IPv6 address which is the Subject of this Query.                                                                                                                                                                                                                                                                                                                                                                                                   |
|                                   |                                                                                                                                                                  | 1         | The Data field contains a name which is the Subject of this Query, or is empty, as in the case of a NOOP.                                                                                                                                                                                                                                                                                                                                                                     |
|                                   |                                                                                                                                                                  | 2         | The Data field contains an IPv4 address which is the Subject of this Query.                                                                                                                                                                                                                                                                                                                                                                                                   |
| 140                               | ICMP Node Information Response                                                                                                                                   | 0         | A successful reply. The Reply Data field may or may not be empty.                                                                                                                                                                                                                                                                                                                                                                                                             |
|                                   |                                                                                                                                                                  | 1         | The Responder refuses to supply the answer. The Reply Data field will be empty.                                                                                                                                                                                                                                                                                                                                                                                               |
|                                   |                                                                                                                                                                  | 2         | The Qtype of the Query is unknown to the Responder. The Reply Data field will be empty.                                                                                                                                                                                                                                                                                                                                                                                       |
| 141                               | Inverse Neighbor Discovery Solicitation Message                                                                                                                  | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 142                               | Inverse Neighbor Discovery Advertisement Message                                                                                                                 | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 143                               | Multicast Listener Discovery ([MLDv2](https://en.wikipedia.org/wiki/MLDv2)) reports [<sup>\[4\]</sup>](https://en.wikipedia.org/wiki/ICMPv6#cite_note-rfc3810-4) |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 144                               | Home Agent Address Discovery Request Message                                                                                                                     | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 145                               | Home Agent Address Discovery Reply Message                                                                                                                       | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 146                               | Mobile Prefix Solicitation                                                                                                                                       | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 147                               | Mobile Prefix Advertisement                                                                                                                                      | 0         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 148                               | Certification Path Solicitation ([SEND](https://en.wikipedia.org/wiki/Secure_Neighbor_Discovery_Protocol))                                                       |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 149                               | Certification Path Advertisement (SEND)                                                                                                                          |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 151                               | Multicast Router Advertisement ([MRD](https://en.wikipedia.org/wiki/Multicast_router_discovery))                                                                 |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 152                               | Multicast Router Solicitation ([MRD](https://en.wikipedia.org/wiki/Multicast_router_discovery))                                                                  |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 153                               | Multicast Router Termination ([MRD](https://en.wikipedia.org/wiki/Multicast_router_discovery))                                                                   |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 155                               | RPL Control Message                                                                                                                                              |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 160                               | Extended Echo Request [<sup>\[5\]</sup>](https://en.wikipedia.org/wiki/ICMPv6#cite_note-rfc8335-5)                                                               | 0         | Request Extended Echo                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 161                               | Extended Echo Reply [<sup>\[5\]</sup>](https://en.wikipedia.org/wiki/ICMPv6#cite_note-rfc8335-5)                                                                 | 0         | No Error                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                   |                                                                                                                                                                  | 1         | Malformed Query                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|                                   |                                                                                                                                                                  | 2         | No Such Interface                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|                                   |                                                                                                                                                                  | 3         | No Such Table Entry                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|                                   |                                                                                                                                                                  | 4         | Multiple Interfaces Satisfy Query                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| 200                               | Private experimentation                                                                                                                                          |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 201                               | Private experimentation                                                                                                                                          |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 255                               | Reserved for expansion of ICMPv6 informational messages                                                                                                          |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
