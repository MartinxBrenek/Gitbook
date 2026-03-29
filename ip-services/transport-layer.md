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

# Transport Layer

The **transport layer** breaks down data streams received from the upper application layer into smaller chunks called **segments** before sending it to L3 for encapsulation

The application relies on TCP to ensure that each chunk is broken up into smaller segments that will fit the maximum transmission unit (MTU) of the underlying network layers. UDP does not provide segmentation services. UDP instead expects the application process to perform any necessary segmentation and supply it with data chunks that do not exceed the MTU of lower layers.

Transport layer handles only the logical part of the data delivery, it is not aware of the media used to send the data

The basic service that the transport layer provides is tracking communication between applications on the source and destination hosts. This service is called session multiplexing, and both UDP and TCP perform it. A major difference between TCP and UDP is that TCP can help ensure that the data is delivered, while UDP does not ensure delivery.

Multiple communications often occur at once; for instance, you may be searching the web and using FTP to transfer a file at the same time. The transport layer tracks these communications and keeps them separate

The header of each segment include information such as source and destination ports to identify the application communicating over TCP or UDP

This port is then combined with the IP address called **IP socket**. Example 10.10.10.1:443 (IP:port)

Each IP socket serves to distinguish and direct incoming packets to operating system that directs the incoming data to their respective applications or services

There are three categories of reserved ports allocated and assigned by IANA to the internet and content delivery authorities

**Well-Known Ports** \<range 0; 1023> Commonly used ports reserved for specific applications (servers)

**Registered Ports** \<range 1024; 49 151> Used similarly to well-known ports (proprietary applications)

**Dynamic or Private Ports** \<range 49 152; 65 535> Freely usable ports for identification of outgoing communication

If Layer 4 is found to be healthy in packet capture, it isolates the issue to the application layer

If Layer 4 is found with retransmission and disrupted, it isolates the issue to network layer

![](<../.gitbook/assets/Unknown image (1350)>)

### Well known ports

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

### UDP (User Datagram Protocol)

**UDP (User Datagram Protocol)** does not establish connection and does not guarantee packet delivery, therefore it is referred to as an unreliable protocol.

UDP performs only checksum to indicate whether the segment is not corrupted

UDP provides applications with best-effort delivery and does not need to maintain state information about previously sent data.

There are many situations in which best-effort delivery is more desirable than reliable delivery.

UDP is used for performance delivery of packets, where error checking and correction are either not necessary or are handled by the application

Time-sensitive voice or video applications uses UDP, since they present data in real-time, and they prefer to lose packet during the transmission rather than having to delay the entire data stream with retransmissions

Reliability (guaranteed delivery) is not always necessary, or even desirable. For example, if one or two segments of a VoIP stream fail to arrive, it would only create a momentary disruption in the stream. This disruption might appear as a momentary distortion of the voice quality, but the user may not even notice. In real-time applications, such as voice streaming, dropped packets can be tolerated as long as the overall percentage of dropped packets is low.

Here are some common applications that use UDP:

UDP is also better for transaction type services, such as DNS or DHCP. In transaction type services, there is only a simple query and response. If the client does not receive a response, it simply sends another query, which is more efficient and consumes fewer resources than TCP.

VoIP

TFTP

NTP

SNMP

Online gaming services

#### UDP segment header

The low overhead of UDP is evident when you review the UDP header length of only 64 bits (8 bytes). The UDP header length is significantly smaller compared with the TCP minimum header length of 20 bytes.

![](<../.gitbook/assets/Unknown image (1351)>)

Source port: Calling port number (16 bits)

Destination port: Called port number (16 bits)

Length: Length of UDP header and UDP data (16 bits)

Checksum: Calculated checksum of the header and data fields (16 bits)

Data: ULP data (varies in size)

![](<../.gitbook/assets/Unknown image (1352)>)

### TCP (Transmission Control Protocol)

**TCP (Transmission Control Protocol)** is a stateful connection-oriented protocol, that keep track of how much data has been transmitted and ensures that all data have been reliable transmitted to the destination and executes retransmission if they haven't

Before transmission, TCP employes Three-way handshake, to prepare both ends for data transmission and negotiate the MSS and the speed of the transmission, so that a sender doesn't overwhelm a receiver's buffer, where the data are stored before directing it to the application

After this three-way negotiation, the TCP keep a connection between the source and destination to ensure ensure flow control and dynamically adjust it based on the network conditions

Error-control and retransmission is ensured labeling each segment in a L4 header with its sequence number and requiring an acknowledgement from the receiver for each received segment

Here are some common applications that use TCP:

Web browsers

FTP

Network printing

Database transactions

#### Three-way handshake

The two-way connection is established with three way handshake SYN,SYN-ACK,ACK

**SYN** The client sends a synchronization message to the server with a random unique numerical value. Sequence number refers to "Start at" and "Got it" refers to Acknowledgement

**SYN-ACK** The server sends back an Acknowledgment message incremented by 1 to indicate that the SYN from client was received and it's SYN+1 to initiate SYN from his side.

**ACK** The client responds with an Acknowledgment of server's SYN (ACK+1) and completes the connection establishment

From that point the sender sends the data and receiver acknowledges it up until the session is terminated

Note The server is in a “passive open” state (listening for a connection) and the client is in “active open:” state (initiates the connection with TCP SYN)

![](<../.gitbook/assets/Unknown image (1353)>)

#### Connection termination (TCP FIN)

HostA has no more data to send. So it initiates Termination

Host B Acknowledges Termination. So HostA— Host B connection Ends Acknowledge d

Host B has no more data to send. So initiates Termination

Host A Acknowledges Termination. So Host B — Host A connection Ends

![](<../.gitbook/assets/Unknown image (1354)>)

#### TCP segment header

The TCP header follows the IP header and supplies information that is specific to the TCP protocol. Flow control, reliability, and other TCP characteristics are achieved by using fields in the TCP header. Each field has a specific function.

20-60 bytes (40 for options)

![](<../.gitbook/assets/Unknown image (1355)>)

Source port this is a 16 bit field that specifies the port number used by the sender to connect to the receiver

Destination port this is a 16 bit field that specifies the port number identifying the application running on the destination device

Sequence number the sequence number is a 32 bit field that indicates how much data is sent within the TCP segment

When you establish a new TCP connection (3 way handshake) then the initial sequence number is a random 32 bit value.

The receiver will use this sequence number and sends back an acknowledgment.

Protocol analyzers like wireshark will often use a relative sequence number of 0 since it’s easier to read than actual sequence number which is some high random number.

Acknowledgment number this 32 bit field is used by the receiver to request the next TCP segment. This value will be the sequence number incremented by 1.

Header length 4bit field that indicates the length of the TCP header specifying the size in 32-bit increments, to indicate where the actual data begins.

Reserved unused 3 bits always set to 0

Flags there are 9 bits for control flags used to establish connections, send data and terminate connections:

Nonce Sum (NS): Enables the receiver to demonstrate to the sender that segments are being acknowledged

Congestion Window Reduced (CWR): Acknowledges that the congestion-indication echoing was received

Explicit Congestion Notification Echo (ECE): Indication of congestion

URG (0x20) indicates that the Urgent Pointer field in the TCP header is valid and that the segment should be processed as urgent.

ACK (0x10) indicates that the Acknowledgment Number field in the TCP header is valid and that the segment is an acknowledgment of previously received data.

PSH (0x08) indicates that the receiver should pass the data to the application as soon as possible, without waiting for the TCP buffer to fill up.

RST (0x04) indicates that the connection should be reset. Not an agreed termination, but rather a rejection by one of the ends

SYN (0x02) indicates that this segment is a synchronization request to initiate a connection.

FIN (0x01) indicates that the sender has no more data to send and is closing the connection.

Window size the 16 bit window field indicating the amount of data (in bytes) a sender can transmit before requiring an acknowledgment from the receiver

It represents the receiver's buffer space available for incoming data

Checksum 16 bits are used for a checksum to check if the TCP header is not corrupted or not

Urgent pointer these 16 bits are used when the URG bit has been set, the urgent pointer is used to indicate where the urgent data ends

Options this field is optional and can be anywhere between 0 and 320 bits.

Data: Upper-layer protocol data, varies in size

#### TCP maximum segment size (MSS)

**TCP maximum segment size (MSS)** refers to the largest amount of data that can be sent in a single TCP segment; does not have to match between client and server

MSS is dynamically adjusted during the TCP negotiation, which helps manage and avoid L3 fragmentation at the sending and receiving ends

However it has no visibility on the intermediary devices, thus cannot handle fragmentation along the entire path, thus PMTUD can be implemented to synchronize the path MTU

Ideally each network hop along the path between the source and destination must have an MTU that can accommodate the TCP segments without fragmentation, otherwise MSS can be adjusted under interface config level with #ip tcp adjust-mss to accommodate additional header overhead that may be introduced by tunnels or other protocols

The absence of an MSS value in the TCP handshake results in a default MSS value of 536 bytes, which is the smallest TCP segment length that can be sent

This MSS is derived from the RFC879, where it was defined that THE TCP MAXIMUM SEGMENT SIZE IS THE IP MAXIMUM DATAGRAM SIZE (which was defined as 576 to comply with all MTU standards) MINUS 40 = 576 - 20 bytes of IP header and - 20bytes of TCP header

So in Ethernet the MSS = MTU - TCP/IP headers. Ethernet MTU = 1500bytes, TCP/IP headers=40bytes. MSS=1460

#### Sequence number

Every segment, sender sends has an associated sequence number. The sequence number is set by the sender according to size of each tcp packet size.

In case the transmission is not related to the initial TCP window, it is an accumulated sequence number, with regard to the first data byte of that ongoing TCP session.

When sender sends 501 bytes of data, it prepends to TCP header the next sequence number to be 502 from the receiver

The sender stores copy of those 501 bytes and holds it in it's buffer until the receiver responds back with ACK value of 502, then clear it's buffer once the expected sequence number is received

If the receiver's next seq and seq number from his last packet stays the same, it indicates that the sender didn't send any more data and just follows to acknowledge incoming data

TCP Reassembly process of reassembling TCP segments into original data based on the sequence number

The example shows that after the first 3 packets, the TCP window transmits application data, therefore, Packet 4 contains a GET message, where Host A requires 200 Bytes from Host B, starting with sequence number 1. This means that the next expected sequence number will be 201.

Packet 5 shows that Host B responds with the sequence number equal to 1 since Host B has not yet sent any data while being engaged with a redirecting task. At the same time Host B acknowledges the receipt of 200 Bytes from Host A, by sending the acknowledgment number 201. This packet contains 300 Bytes of useful payload. The next sequence number that Host B should issue will later be 301.

Packet 6 is sent as another GET request from Host A, with a length of 400 Bytes. The accumulated sequence number is 601=201+400. This packet acknowledges 300 Bytes, sent by Host B, by sending an Ack value of 301.

Finally, in Packet 7, Host B requests 800 Bytes of data, with the starting sequence number of 301. Therefore, the following sequence number would be 1101. It received 400 Bytes from Host A and it acknowledges 601.

It is relevant to say, that both Hosts A and B maintain their sequence numbers.

![](<../.gitbook/assets/Unknown image (1356)>)

![](<../.gitbook/assets/Unknown image (1357)>)

![](<../.gitbook/assets/Unknown image (1358)>)

#### TCP window size

**TCP window size** the 16 bit window field represents the amount of data (in bytes) a sender can transmit before requiring an acknowledgment from the receiver. This makes the data transfer more efficient. This value does not have to match aswell. In traditional TCP, the maximum window size is 64 KB (65,535 B). Extensions to TCP, specified in RFC 1323, allow for tuning TCP by extending the maximum TCP window size to 1 GB (230 B, or 1,073,741,823 B).

Basically it represents the receiver's buffer space available for incoming data - Receive window

**Congestion window (CWND)** is used to control the sender's data transmission rate to avoid network congestion. The CWND is dynamically adjusted based on network conditions.

It's a part of the TCP congestion control algorithms, such as TCP Reno or TCP Cubic.

**TCP Slow Start** starts with a small window size and every time when there is a successful acknowledgement, the window size will exponentially increase until it reaches receiver's maximum receive buffer. Host initially sends 1 packet and wait for ACK, after ACK it sends 2 packets and waits for ACK and so on...

When the receiver doesn’t send an acknowledgment within a certain time period of time then the window size will be reduced and slow start restarted

![](<../.gitbook/assets/Unknown image (1359)>)

![](<../.gitbook/assets/Unknown image (1360)>)

**Window Size Scaling Factor** advertised and agreed upon TCP factor (in multiplies of 2) that dynamically multiplies the advertised window size during a connection to optimize data transfer efficiency

In above pcap from TCP connection, focus on calculated window size instead of window size as it is the multiplied window size that the client advertises as it's receive window.

If the calculated window size from client in his ACK to server decreases, that means that the client waits for the application to process received data (by this TCP prevents the "bucket" to overflow, which would cause retransmission and re-synchronization). Once the application processes the data in the TCP buffer, the TCP hand over the already received data from it's buffer to the application and informs the server of it's refreshed calculated window size as the space for receiving data has become available.

If there is calculated window size 0, that means that the TCP is stuck and cannot clear it's buffer, because the application does not keep up with the received data

| ip tcp-window-size <> | must be enabled on both ends; #show tcp brief |
| --------------------- | --------------------------------------------- |

#### TCP keep-alive

**TCP keep-alive** is a packet (64bytes in length) that servers as a mechanism to maintain open connections often used to prevent inactivity-related connection closures

If there's a period of inactivity where the client hasn't received any data from the server, it sends a Keep-Alive packet to check if the server is still responsive

The server responds with a Keep-Alive ACK to confirm its availability. They are not necessarily indicative of network issues but can help diagnose application performance problems

#### TCP retransmission

**TCP retransmission** is an error-control method that retransmits data packets if they are lost during the transmission across the network. It uses three tools to drive reliability:

**Timeout** is the maximum interval allowed to pass between the data origination and recipient

It ensures that the connection does not remain open too long and minimizes exposure to online threats and bad actors

**Checksum** The destination will check the data in the checksum field for integrity, and if it is corrupted, it will not send back an ACK

**Acknowledgment** If a data stream is not acknowledged, then the protocol tries retransmission. Also, if three consecutive ACK values are the same, then TCP initiates retransmission

The fast retransmit algorithm allows the TCP sender to rely on duplicate ACKs instead of a retransmission timer timeout. When a TCP sender receives three duplicate ACKs, it determines that the segment has been lost and immediately retransmits the segment. When the retransmit occurs, the TCP sender will reduce its CWND to half of the value it was when the segment loss occurred and enter into congestion avoidance

In the figure, a station transmits four packets to the receiving station. The second packet is dropped somewhere in the network. Therefore, the receiver sends an ACK N + 1 to request the missing packet. Because the transmitter does not know if the ACK was just a duplicate ACK, it will wait for three ACK N + 1 packets from the receiver. Upon receipt of the third ACK, the missing packet, packet 2, is resent to the receiver. The receiver now sends an ACK N + 4 indicating that it has already received packets 2 and 3 and is ready for the next packet.

![](<../.gitbook/assets/Unknown image (1361)>)

Selective ACK can improve the time it takes for the sender to recover from multiple packet losses, because discontiguous blocks of data can be acknowledged, and the sender only has to retransmit the data that is actually lost. The SACK is used to convey extended acknowledgment information from the receiver to the sender to inform the sender of discontiguous blocks of data that have been received. Using the example in the figure, instead of sending back an ACK N + 1, the receiver can send a SACK N + 1 and also indicate back to the sender that N + 3 has been correctly received with the SACK option.

#### Retransmission timeout

If a TCP sender does not receive acknowledgment for sent segments, it cannot wait indefinitely before it assumes that the data segment that was sent never arrived at the receiver. TCP senders maintain the retransmission timer to trigger a segment retransmission. The retransmission timer can impact TCP performance. If the retransmission timer is too short, duplicate data will be sent into the network unnecessarily. If the retransmission timer is too long, the sender will wait (remain idle) for too long, slowing down the flow of data. The TCP algorithm constantly updates the retransmission timer, based on the RTT experienced by the connection.

When the retransmission timer expires, the TCP sender retransmits the lost segment, drops the CWND to the start value, and begins the slowstart algorithm.

#### TCP congestion control algorithms

In the figure, the TCP connection is established with a CWND of 1 packet and a SSTHRESH of 10 packets, equal to the maximum window size of the receiver. When the window size reaches 8 packets, the TCP sender detects segment loss with the receipt of three duplicate ACKs and starts the fast retransmit and fast recovery algorithms. The SSTHRESH is set to half the size of the window when the packet loss was experienced, 4. The CWND is set to the new SSTHRESH value plus three segment sizes, or 7. Once the ACK is received for the previously lost segment the CWND is reduced to 4 to match the SSTHRESH. The TCP sender is now in congestion avoidance mode, increasing the window size by 1 packet each RTT.

When the CWND reaches 6 packets, another lost segment is detected by the receipt of three duplicate ACKs. The TCP sender performs fast retransmit and fast recovery, setting the SSTHRESH to 3 packets, and resumes congestion avoidance.

When the CWND reaches 5 packets, the TCP sender detects a lost segment due to retransmission timeout. The sender reduces the SSTHRESH to half the value experienced at congestion, sets the CWND to the starting value of 1 packet, and begins the TCP slowstart algorithm again.

![](<../.gitbook/assets/Unknown image (1362)>)

#### TCP global synchronization

**TCP Global Synchronization** occurs when multiple TCP sessions experience packet loss simultaneously due to tail drop, causing all sessions to enter slow start at the same time. This results in a cycle of high congestion followed by underutilization, reducing network efficiency. To prevent this, techniques like Random Early Detection (RED) gradually drop packets to avoid synchronization.

![](<../.gitbook/assets/Unknown image (1363)>)

With RED implemented

RED drops packets randomly before the queue is full, which prevents all flows from backing off simultaneously.

Some flows slow down earlier than others, preventing synchronization.

This results in a more stable link utilization, with fewer drastic drops.

Average link utilization is higher and closer to full bandwidth

![](<../.gitbook/assets/Unknown image (1364)>)

With WRED implemented

Low-priority traffic is dropped first (green line).

Medium-priority traffic is dropped later (red line).

High-priority traffic is dropped last (blue line).

![](<../.gitbook/assets/Unknown image (1365)>)

#### Duplicate acknowledgements (Dup ACK)

**Duplicate acknowledgements (Dup ACK)** when the receiver of a TCP connection encounters gaps in received data segments, it sends duplicate ACKs for the last correctly received segment

This triggers the sender to perform a fast retransmit, which helps recover from potential packet loss.

On the below pcap we can see that the client did not received the 4066 (4067 is the next sequence number that is expected by the server to be received as ack from client)

The client's seq num and next seq num stays the same as he didn't sent any data to the server and indicates that it received only 536 bytes (537 ack), then server retransmits 2 times 536 in packet 9 and 10 and client ack's it with 1609. Then the server retransmit's another 536 data, however 536 is missing for the final of 2681. So on the packet 13 client send's Dup ACK, of the last correctly received data that were on packet 11. With this it informs server that it didn't received those 536 bytes and requests to retransmit it again, however server keeps sending other data and client remains inform him of last correctly received data that were on packet 11.

![](<../.gitbook/assets/Unknown image (1366)>)

#### Selective acknowledgement (SACK)

**Selective acknowledgement (SACK)** indicates which bytes were received successfully with left edge and right edge range. SACK is involved, when the receiver wants to inform sender of unreceived packets

Left edge and right edge indicates that it received those 536 bytes in packet #12 (selective acknowledges them), but missing 536 bytes from last successful ACK 1609 to left edge 2145

(Frame 13)

![](<../.gitbook/assets/Unknown image (1367)>)

#### Bandwidth-delay product (BDP)

**Bandwidth-delay product (BDP)** represents the maximum amount of data that can be in transit in the network without causing congestion or delays due to the limitations of the network's bandwidth and the propagation delay (time taken for signals to travel across the network)

The problem with high satelite link with high delay is that when the sender sends some data, it has to be wait a very long time for an acknowledgment of the receiver before it can send the next data. During the time we are waiting, nothing happens so the full bandwidth of our link is not utilized

BPD helps determine the optimal window size for data transmission to fully utilize the available bandwidth without causing congestion or excessive latency

Formula: BDP (in bits) = Available Bandwidth (in bits per second) × Round-Trip Time (in seconds)

This example shows that the full capacity of the link is wasted as Host could have already sent more data instead of waiting for ACK

![TCP Long Fat Network Low Window Size](<../.gitbook/assets/Unknown image (1368)>)

![TCP long fat network high window size](<../.gitbook/assets/Unknown image (1369)>)

ADSL 2 Mbit with 50 ms round trip time: 2000000 bits \* 0.05 seconds = bandwidth delay product 100000 bits (or 12500 bytes)

Gigabit LAN Interface with 1 ms round trip time: 1000000000 bits \* 0.001 seconds = bandwidth delay product 1000000 bits (or 125000 bytes)

#### TCP option: no-operation (NOP)

**TCP option: no-operation (NOP)** pads the TCP header to set boundaries between TCP options and pads them to be in multiplies of 4

Below example shows window scale that has 3 bytes, so the next TCP NOP pads it with one byte to have it in multiplies of 4

![](<../.gitbook/assets/Unknown image (1370)>)

### IP socket / session multiplexing

**IP socket / session multiplexing** allow a host to connect multiple services on a server simultaneously. IP sockets are then used to distinguish and manage the individual data streams or service requests.

Transmitting client will attempt to connect to a well-known port number at the destination server for specific service that it wants to access.

The operating system of transmitting source client will use temporary, ephemeral source port that is some arbitrary unique number from the dynamic port range

Each source and destination port pairing identifies a separate virtual connection, allowing multiple connections to share one physical network connection

**Active socket** is used when a client initiates a connection request

**Passive socket** is used to receive connection requests

![](<../.gitbook/assets/Unknown image (1371)>)

#### Socket functions

socket(): Creates a new socket.

bind(): Associates a socket with a specific IP address and port number.

listen(): Puts a socket into a listening state, allowing it to accept incoming connections (used with TCP).

accept(): Accepts an incoming connection request and creates a new socket for communication (used with TCP).

connect(): Initiates a connection to a remote socket.

send(): Sends data over a socket.

recv(): Receives data from a socket

**Socket pair** consists of four elements {local IP, local port, remote IP, and remote port} to determine how incoming and outgoing traffic is routed through a system.

Server socket that listens on port 8080 and can receive requests from any IP can be denoted as: {\*: 8080, -:-} // asterisk \* indicates listening

![IP Sockets - Networking Fundamentals - Part 1 | Linux](<../.gitbook/assets/Unknown image (1372)>)

### Quick UDP Internet Connections (QUIC)

**Quick UDP Internet Connections (QUIC)** is a transport layer network protocol developed by Google. It is designed to improve the performance of web applications by reducing latency and speeding up the establishment of secure connections. Unlike traditional protocols like TCP, QUIC does not require a three-way handshake to establish a connection as it is based on UDP. This reduces the latency in the initial setup

QUIC incorporates security features directly into the protocol. It uses Transport Layer Security (TLS) for encryption, ensuring a secure communication channel.

QUIC is closely associated with HTTP/3, the latest version of the Hypertext Transfer Protocol. It serves as the transport protocol for HTTP/3, enhancing the speed and efficiency of data transfer.

QUIC supports **Zero Round Trip Time (0-RTT)**, allowing clients to send encrypted requests in the first communication without the need for multiple round trips

This is particularly advantageous for known conversations where parameters are already established.

QUIC includes built-in mechanisms for flow control and loss recovery, ensuring efficient and reliable data transmission

![](<../.gitbook/assets/Unknown image (1373)>)
