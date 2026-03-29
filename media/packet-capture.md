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

# Packet Capture

### Embedded Packet Capture (EPC)

IOS-integrated packet capture facility, when enabled, the router captures the sent and received packets. The packets are stored within a buffer in DRAM and do not persist through a reload. Once the data are captured, it can be examined in a summary or detailed view on the router. In addition, the data can be exported as a packet capture (PCAP) file to be exported to the FTP or local disk for further examination in Wireshark. ACL's can be used to limit the traffic to capture. Buffer is stored in the DRAM and will not persist through reloads

| monitor capture mycap interface <> \<tx\|rx\|both>  |                                           |
| --------------------------------------------------- | ----------------------------------------- |
| access-list extended test permit ip host <> host <> | define access list aswell for SIP and DIP |
| monitor capture mycap access-list test              |                                           |
| monitor capture test start                          |                                           |
| monitor capture test stop                           |                                           |
| show monitor capture mycap buffer                   |                                           |

### Switched Port Analyzer (SPAN)

is switch specific tool that copies Ethernet frames passing through switch ports and send these frames out to specific port, to network analyzer (HW appliance or PC with Wireshark)

Switch itself doesn’t analyze these copied frames. As traffic is duplicated with SPAN feature, the production traffic could starve (keep in mind)

SPAN features two different port types. The source port is a port that is monitored for traffic analysis. SPAN can copy ingress, egress, or both types of traffic from a source port. Both Layer 2 and Layer 3 ports can be configured as SPAN source ports. The traffic is copied to the destination (also called monitor) port.

The association of source ports and a destination port is called a SPAN session. In a single session, you can monitor at least one source port. Depending on the switch series, you might be able to copy session traffic to more than one destination port.

Alternatively, you can specify a source VLAN, where all ports in the source VLAN become sources of SPAN traffic. Each SPAN session can have either ports or VLANs as sources, but not both.

#### Local SPAN

A local SPAN session is an association of a source ports and source VLANs with one or more destination ports. You can configure local SPAN on a single switch. Local SPAN does not have separate source and destination sessions.

The SPAN feature allows you to instruct a switch to send copies of packets that are seen on one port to another port on the same switch

If you would like to analyze the traffic flowing from PC1 to PC2, you need to specify a source port. You can either configure the GigabitEthernet0/1 interface to capture the ingress traffic or the GigabitEthernet0/2 interface to capture the egress traffic. Second, specify the GigabitEthernet0/3 interface as a destination port. Traffic that flows from PC1 to PC2 will then be copied to that interface and you will be able to analyze it with a traffic sniffer.

Specifying the Source Ports

monitor session session-id source {interface interface-id | vlan vlan-id} \[rx | tx | both]

monitor session session-id filter vlan vlan-range

Specifying the Destination Ports

monitor session session-id destination interface interface-id

monitor session session-id destination interface interface-id \[encapsulation replicate] // last option configures switch to catch all L2 protocols

monitor session session-id destination interface interface-id ingress {dot1q vlan vlan-id | untagged vlan vlan-id} // to enable destination port to send and receive traffic

STP is disabled on the destination port to prevent extra BPDUs from being included in the network analysis. Great care should be taken to prevent a forwarding loop on this port

| <p>SW1(config)#** monitor session 1 source interface Gigabit0/1**<br>SW1(config)#** monitor session 1 destination interface Gigabit0/2**</p> | The SPAN session is identified by a session number; in this example, it is 1. The first step is then that you associate the SPAN session with source ports or VLANs by using the following command: If you do not specify a traffic direction, the source interface sends both transmitted (Tx) and received (Rx) traffic to the destination port to be monitored. You can specify the following options: Rx: Monitor received traffic. Tx: Monitor transmitted traffic. both: Monitor both received and transmitted traffic (default). Similarly, you associate the destination port with the SPAN session number by using the following command: |
| -------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show monitor session {session-id \[detail] \| local \[detail]}                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

![](<../.gitbook/assets/Unknown image (600)>)

#### RSPAN

extends this monitoring capability by allowing network traffic to be copied from ports on one switch to a monitoring port on a different switch in the same broadcast domain

Monitor session can source interface or vlan not both at the same time

RSPAN consists of the RSPAN source session, RSPAN VLAN, and RSPAN destination session.

You separately configure the RSPAN source sessions and destination sessions on different switches. Your monitored traffic is flooded into an RSPAN VLAN that is dedicated for the RSPAN session in all participating switches. The RSPAN destination port can then be anywhere in that VLAN.

![](<../.gitbook/assets/Unknown image (601)>)

#### RSPAN configuration

| <p>SW1(config)#** vlan 100**<br>SW1(config-vlan)#** name SPAN-VLAN**<br>SW1(config-vlan)#** remote-span**<br>SW1(config)#** monitor session 2 source interface Gig0/1**<br>SW1(config)#** monitor session 2 destination remote vlan 100**</p> | These two sessions need to be defined on both the local and remote switches. Session numbers are local to each switch, so they do not need to be the same on every switch. |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>SW2(config)#** vlan 100**<br>SW2(config-vlan)#** name SPAN-VLAN**<br>SW2(config-vlan)#** remote-span**<br>SW2(config)#** monitor session 3 destination interface Gig0/2**<br>SW2(config)#** monitor session 3 source remote vlan 100**</p> |                                                                                                                                                                            |

STP operates on the RSPAN VLAN, and STP BPDUs cannot be filtered as filtering could introduce a forwarding loop

#### ERSPAN

The Cisco ERSPAN mirrors traffic on one or more “source” ports and delivers the mirrored traffic to one or more “destination” ports on another switch. The traffic is encapsulated in Generic Routing Encapsulation (GRE) and is, therefore, routable across a Layer 3 network between the “source” switch and the “destination” switch.

ERSPAN consists of an ERSPAN source session, routable ERSPAN GRE encapsulated traffic, and an ERSPAN destination session.

A device that has only an ERSPAN source session configured is called an ERSPAN source device, and a device that has only an ERSPAN destination session configured is called an ERSPAN termination device

To configure an ERSPAN source session on one switch, you associate a set of source ports or VLANs with a destination IP address, ERSPAN ID number, and optionally with a Virtual Routing and Forwarding (VRF) name. To configure an ERSPAN destination session on another switch, you associate the destinations with the source IP address, ERSPAN ID number, and optionally with a VRF name.

ERSPAN source sessions do not copy locally sourced RSPAN VLAN traffic from source trunk ports that carry RSPAN VLANs. ERSPAN source sessions do not copy locally sourced ERSPAN GRE-encapsulated traffic from source ports. Each ERSPAN source session can have either ports or VLANs as sources, but not both. The ERSPAN source session copies traffic from the source ports or source VLANs and forwards the traffic using routable GRE-encapsulated packets to the ERSPAN destination session. The ERSPAN destination session switches the traffic to the destinations.

Which Cisco feature allows you to monitor traffic on one or more ports or more VLANs and send the monitored traffic to one or more destination ports? - ERSPAN

![](<../.gitbook/assets/Unknown image (602)>)

[https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9300/software/release/17-12/configuration\_guide/nmgmt/b\_1712\_nmgmt\_9300\_cg/configuring\_erspan.html](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9300/software/release/17-12/configuration_guide/nmgmt/b_1712_nmgmt_9300_cg/configuring_erspan.html)

![](<../.gitbook/assets/Unknown image (603)>)

#### ERSPAN source configuration

The source interface command associates the ERSPAN source session number with the source ports or VLANs and selects the traffic direction to be monitored.

The destination command enters the ERSPAN source session destination configuration mode.

The erspan-id configures the ID number used by the source and destination sessions to identify the ERSPAN traffic, which must also be entered in the ERSPAN destination session configuration.

The IP address command configures the ERSPAN flow destination IP address, which must also be configured on an interface on the destination switch and be entered in the ERSPAN destination session configuration.

The origin IP address configures the IP address used as the source of the ERSPAN traffic.

![](<../.gitbook/assets/Unknown image (604)>)

#### ERSPAN destination configuration

The destination interface associates the ERSPAN destination session number with the destinations.

The source command enters ERSPAN destination session source configuration mode.

The erspan-id configures the ID number used by the destination and destination sessions to identify the ERSPAN traffic. This must match the ID that you entered in the ERSPAN source session.

The IP address configures the ERSPAN flow destination IP address. This must be an address on a local interface and match the address that you entered in the ERSPAN source session.

![](<../.gitbook/assets/Unknown image (605)>)

| show monitor session erspan-source session |   |
| ------------------------------------------ | - |

### tcpdump

is a CLI real-time packet capturing feature, that prints a line of text for each incoming or outgoing packet and can be saved as a file for further analysis outside of CLI

![](<../.gitbook/assets/Unknown image (606)>)

-n Turning off name resolution

First, it slows down packet interception. It’s not a big deal when there are only few packets, but when there are thousands and tens of thousands it introduces a delay into the process. Amount of delay can be different, depending on the traffic.

Another, much more serious problem occurs when there is no DNS server around or when DNS server is not working properly. If this is the case, tcpdump spends few seconds trying to figure out two hostnames for each IP packet. This means virtually stopping intercepting the traffic.

Example # tcpdump -ni eth1 -w file.cap not port 22

Saving captured data # tcpdump -w file.cap

By default, when capturing packets into a file, it will save only 68 bytes of the data from each packet. To prevent this use: # tcpdump -w file.cap -s 0

To read the capture file: # tcpdump -r file.cap

-tt causes tcpdump to print time stamp as number of seconds since Jan. 1st 1970 and a fraction of a second.

-ttt prints the delta between this line and a previous one

-tttt causes tcpdump to print time stamp in it’s regular format preceeded by date

### IOS XE packet tracing (FIA / Packet Trace)

is a feature that is used for troubleshooting and debugging network issues in Cisco IOS-XE software

It is a tool that enables users to capture per-packet process details based on a class of user-defined conditions. FIA can be used to trace packets and display the results of the trace 1. It can be enabled with either the path-trace or FIA trace option 1. FIA can be used to trace packets on platforms such as ASR1000, ISR4000, ISR1000, Catalyst 1000, Catalyst 8000, CSR1000v, and Catalyst 8000v series routers 1. The feature is not supported on the ASR900 series aggregation services routers or the Catalyst series switches that run Cisco IOS-XE software 1.

In order to identify issues such as misconfiguration, capacity overload, or even the ordinary software bug while troubleshooting, it is necessary to understand what happens to a packet within a system. Packet-trace Accounting keeps a count of all packet-trace interesting packets that enter and leave the “packet processor

Three basic count groups.

Summary Counts

Packets Matched –packets that matched conditions

Packets Traced – packets that were traced

Arrival Counts

Ingress – packets entering via external interfaces

Inject\* – number of packets seen as injected from control plane

Departure Counts

Forward – number of packets scheduled/queued for delivery

Punt\* – number of packets punted to control plane

Drop\* – number of packets specifically dropped by packet processing

Consume – number of packets consumed during packet process (e.g. ping request)

Acronyms

RP – Route Processor

FP – Forwarding Processor = ESP (Embedded Service Processor)

CPP – Cisco Packet Processor Complex= QFP (Quantum Flow Processor)

Embedded Services Processor (ESP) is responsible for providing additional processing power and capabilities for functions such as VPN encryption, firewall services, or deep packet inspectio

Quantum Flow Processor (QFP) "gut of forwarding engine" is a key component in Cisco's networking devices, it performs packet forwarding and processing functions of these devices

PPE – Packet Processing Engine

IOCP – I/O Control Processor

FECP – Forwarding Engine Control Processor

SPA – Shared Port Adapter

SIP – SPA Interface Processor

IOSd – IOS image that runs as a process on the RP

FMAN – Forwarding manager (FMAN-RP, FMAN-FP)

EOBC = Ethernet Out of Band Channels – Packet Interface for Card to Card Control Traffic

IOS-XE (BinOS) = Linux Based Software Infrastructure for IOS-XE

#### Configuration

#### Enable platform conditional debugs

The Packet Trace feature relies on the conditional debug infrastructure in order to determine the packets to be traced. The conditional debug infrastructure provides the ability to filter traffic based on

| debug platform condition \[ipv4 \| ipv6] \[interface interface ]\[access-list access-list -name \| ipv4-address / subnet-mask \| ipv6-address / subnet-mask ] \[ingress \| egress \|both] |   |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

| debug platform condition ipv4 10.0.0.1/32 both       | --> matches in and out packets with source or destination as 10.0.0.1/32 |
| ---------------------------------------------------- | ------------------------------------------------------------------------ |
| debug platform condition ipv4 access-list <> egress  | --> matches egress packets corresponding to ACL-list <>                  |
| debug platform condition interface gig 0/0/0 ingress | --> matches all ingress packets on interface gig 0/0/0                   |
| show platform conditions                             | --> shows the platform conditions configure                              |

#### Enable packet trace

| debug platform packet-trace packet \<pkt-size/pkt-num> \[fia-trace \| summary-only] \[circular] \[data-size ] |   |
| ------------------------------------------------------------------------------------------------------------- | - |

| debug platform packet-trace packet 1024           | -> basic path-trace, and automatically stops tracing packets after 1024 packets. You can use "circular" option if needed                                |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| debug platform packet-trace packet 1024 fia-trace | -> enables detailed fia trace, stops tracing packets after 1024 packets                                                                                 |
| debug platform packet-trace drop \[code ]         | Drop Trace optionally allows you to specify the retention of packets for a specific drop code. show platform hardware qfp active statistics drop detail |
| show platform packet-trace configuration          |                                                                                                                                                         |
| show debugging                                    | --> this can show both platform conditions and platform packet-trace configured                                                                         |

| Start/stop the packet-trace                                  |
| ------------------------------------------------------------ |
| debug platform condition start debug platform condition stop |

#### Display the packet trace results

| show platform packet-trace statistics                                     | --> statistics of packets traced                                                                       |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| show platform packet-trace summary                                        | --> summary of all the packets traced, with input and output interfaces, processing result and reason. |
| show platform packet-trace packet 12                                      | -> Display path trace of FIA trace details for the 12th packet in the trace buffer                     |
| show platform hardware qfp active infrastructure exmem statistics         | You can check the current data-plane DRAM memory consumption by using the                              |
| show platform hardware qfp active interface if-name GigabitEthernet 0/0/1 | Check the FIA Associated with an Interface                                                             |

#### Cleanup (clear trace and conditions)

| clear platform packet-trace statistics | --> clear the packet trace buffer                                      |
| -------------------------------------- | ---------------------------------------------------------------------- |
| clear platform condition all           | --> clears both platform conditions and the packet trace configuration |

#### Example

| Router# debug platform packet-trace packet 128 fia-trace Router# debug platform packet-trace punt Router# debug platform condition interface g0/0/1 ingress Router# debug platform condition start Router#! ping to UUT Router# debug platform condition stop Router# show platform packet-trace packet 0 | show platform hardware qfp active memory // to check control plane memory |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |

![](<../.gitbook/assets/Unknown image (607)>)

#### Inject and punt traces

to trace punt (packets that are received on the FP that are punted to the control plane) and inject (packets that are injected to the FP from the control plane) packets.

ASR1000#debug platform condition ipv4 172.16.10.2/32 both

ASR1000#debug platform condition start

ASR1000#debug platform packet-trace punt

ASR1000#debug platform packet-trace inject

ASR1000#debug platform packet-trace packet 16

![](<../.gitbook/assets/Unknown image (608)>)

The packet traces show the QFP Security Association (SA) handle in the trace that is used in order to encrypt the packet, which is useful when you troubleshoot IPsec VPN issues in order to verify that the correct SA is used for encryption.

![](<../.gitbook/assets/Unknown image (609)>)

#### XE packet drop troubleshooting (QFP)

| show platform hardware qfp active statistics drop detail show platform hardware qfp active datapath utilization |
| --------------------------------------------------------------------------------------------------------------- |

### Wireshark

software for packet capture and analysis that provides advanced filtering options to filter and observe specific network communication.

It is preferrable to perform packet capture on a network device along the path or as close to the source or destination

Client capture itself can have different perceptions about the network. We should aim to get a packet capture from client and server to see the whole picture of the communication

Network traffic monitoring device such as IOTA is able to capture traffic in high data-rate environment such as DC (wireshark on laptop is able to capture around 100MBps of traffic)

<\<Wireshark\_Guide.pdf>>

#### How to start a capture

Input

Left button starts packet capture, the second allows us to set up, which interface we will choose for packet capture on a machine (snaplen gives the option to capture only certain amout of data per frame, so we can set it to capture only headers without frame data, this is to lower the overall pcap size with still being able to capture important part of frames)

![](<../.gitbook/assets/Unknown image (610)>)

#### Capture output formats

PCAP (Packet Capture Data): PCAP is the older and more widely recognized file format for packet capture. It uses the ".pcap" file extension.

PCAPNG (PCAP Next Generation): PCAPNG is a newer and more versatile file format designed to overcome limitations of the original PCAP format. It uses the ".pcapng" file extension.

PCAPNG features a more flexible header structure that allows for the storage of additional metadata and information about the capture, such as interface details, comments, and custom-defined blocks

Ring buffer We can set up wireshark to create another file after it reaches certain size and Ring buffer allows to specify how many of those files we want to generate

![](<../.gitbook/assets/Unknown image (611)>)

Set up

| View > Time display format > UTC |   |
| -------------------------------- | - |

| Statistics > Conversations                  | pcap overview of how many TCP/IP streams are contained in the pcap (you can right click > apply as filter > selected > A<>B to apply it in filter) |
| ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| Statistics > protocol hierarchy             | to see the traffic for each protocol contained in the pcap                                                                                         |
| Statistics > capture file properties        | view captured time and packets in given pcap. Big red flag if there are lots of packets in short period of time, this may reveal routing loop      |
| Statistics > TCP stream graphs > tcptrace   | Graph interpretation of a tcp stream in pcap Green line in graph represents receive window/ceiling Ideally no data transmission should reach that  |
| Right click on packet > Conversation filter | allows to select                                                                                                                                   |
| Right click > edit resolved name            | to append hostname for given IP (can say it is Client, Server, Gateway). This will appended to the pcapng file                                     |

#### Filters

Capture filters is set before doing the packet capture and cannot be modified during the capture. They reduce the size of raw packet capture as it captures only filtered communication

[https://wiki.wireshark.org/CaptureFilters#capture-filter-is-not-a-display-filter](https://wiki.wireshark.org/CaptureFilters#capture-filter-is-not-a-display-filter)

Display filters are used to filter captured file to focus on certain communication/protocol [https://wiki.wireshark.org/DisplayFilters](https://wiki.wireshark.org/DisplayFilters)

You can use either words or symbols as shown below

![](<../.gitbook/assets/Unknown image (612)>)

| ip.addr==192.168.0.1 && tcp.port==443 |                        |
| ------------------------------------- | ---------------------- |
| !tcp.port==80                         | (excludes tcp port 80) |

Special filters

![](<../.gitbook/assets/Unknown image (613)>)

Button Filters

| tcp.analysis.flags && !tcp.analysis.window\_update | TCP flags (TCP events such as retransmissios and duplicate ACKs) Look for RTT inconsistencies / Retransmission                                                 |
| -------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| tcp.flags.reset == 1                               |                                                                                                                                                                |
| http.time>25                                       | to filter the expected response time of http requests                                                                                                          |
| tcp.time\_delta>.1                                 | Time= total time between consecutive packets Delta time = time between consecutive packets (RTT) TCP Delta time = Time since previous frame in this TCP stream |

| tcp.analysis.initial\_rtt>1          | initial handshake taking longer than 1 second revelas network latency Delay observed on client side can be caused by slow reaction/typing from user, server delay is sign of an issue Red flag if TTL is not decrementing which isolates the issue to L3 |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| dns.time>1                           |                                                                                                                                                                                                                                                          |
| !(arp or stp or cdp or lldp or icmp) | to filter all unnecessary traffic                                                                                                                                                                                                                        |
| TCP Conn/TCP Flags                   | to nest filters in one main button folder                                                                                                                                                                                                                |

You can always find the syntax for certain flag or part of the packet in the lower left

![](<../.gitbook/assets/Unknown image (614)>)

Stevens Graph shows sequence number timeline for given pcap

If you click on some part of the communication it navigates you to the packet

![](<../.gitbook/assets/Unknown image (615)>)

#### Capture best practices

Use Dumpcap with Ring Buffer to capture traffic

Dumpcap is the CLI version of wireshark that is integrated in the file system for wireshark.

It is the capturing tool for the wireshark, however we can execute dumpcap in command shell of the machine (as shown below)

Set file size limits for each pcap file (e.g., 100MB).

Specify the number of files to keep in the buffer (e.g., 100 files)

![](<../.gitbook/assets/Unknown image (616)>)

![](<../.gitbook/assets/Unknown image (617)>)

"Dump capture for interface 1, to 10 files and 100MB each in size and save it as david.pcap"

| -w "capture\_%Y-%m-%d\_%H-%M-%S.pcap" | Dumpcap will generate pcap filenames with the current date and time, making it easier to identify when the capture was taken |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |

**Continuous capture**

Keep Dumpcap running continuously, allowing it to overwrite older files in the ring buffer. This continuous capture ensures that you don't miss important network events.

**Trigger capture with a desktop shortcut**

Create a simple trigger mechanism for the end-user or client that can be a shortcut on a desktop and name it "It happened"

This shortcut can be mapped to open up a specific website that can serve as a reference point or marker for the network event that occured at that time.

When the intermittent issue that is experienced by the user happens, user can double click the shortcut "It happened" and thus marking the network event in the capture

Later, when analyzing network traffic, the timestamp provided by the client's click on the shortcut helps identify the relevant packets that were around the execution of the event.

![](<../.gitbook/assets/Unknown image (618)>)

![](<../.gitbook/assets/Unknown image (619)>)

![](<../.gitbook/assets/Unknown image (620)>)

#### Additional features

Wireshark can also allows to extract files from unecrypted communication

You can download and extract files containing ASN and IP address location to display them right in the packet capture

You have to point wireshark in preferences in name resolutions to the Maxmind's files so Wireshark can access those and associate IP's contained in the pcap with the ASN/location

[https://www.maxmind.com/en/accounts/915446/geoip/downloads](https://www.maxmind.com/en/accounts/915446/geoip/downloads) and you can then use the city or ASN name as display filter

#### Malware traffic analysis

Basic questions to answer

What are the infected file(s) downloaded and their hashes?

What is the URL of the infected site?

What is the IP of the infected site?

What is the IP of the infected machine?

What is the hostname of the infected machine?

What is the MAC of the infected machine?

| File >export objects > choose protocol | to see and download all files that are contained in the pcap |
| -------------------------------------- | ------------------------------------------------------------ |

Download HashMyFileswhich is a small utility that allows you to calculate the MD5 and SHA1 hashes of one or more files in your system.

Download: [https://www.nirsoft.net/utils/hash\_my](https://www.nirsoft.net/utils/hash_my)...

This hash of the file can be pasted into the website virustotal.com to verify it's reputation
