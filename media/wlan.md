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

# WLAN

### Wireless personal area network (WPAN)

A personal area network (PAN) is a network that exists within a relatively small area and connects electronic devices such as desktop computers, printers, scanners, fax machines, and notebook computers. In the past, connecting these devices required extensive cabling, connectors, and adapters. Now, these devices typically use Bluetooth to connect to the WPAN.

Typical applications for WPANs are in the office environment. WPAN there enables communication between devices in proximity (within several meters of each other) to communicate as if they were connected by a cable.

It is a wireless network established by personal devices within the range of an individual person, such as a smartphone, tablet, or laptop connected via Bluetooth, AirPlay or other short-range wireless technologies.

#### Wi‑Fi Direct

**Wi-Fi Direct** is used to connect wireless devices for printing, sharing, syncing, and display.

Not everyone has (or wants) access to a Wi-Fi AP or hotspot. However, users often carry content and applications that they want to share, print, display, or synchronize. Wi-Fi Direct is a certification by the Wi-Fi Alliance. The intent is the creation of peer-to-peer Wi-Fi connections between devices, without the need for an AP. It is another example of a WPAN.

This connection, which can operate without a dedicated AP, does not operate in IBSS mode. Wi-Fi Direct is an innovation that operates as an extension to the infrastructure mode of operation. With the technology that underlies Wi-Fi Direct, a device can maintain a peer-to-peer connection to another device inside an infrastructure network—an impossible task in ad hoc mode.

Wi-Fi Direct devices include Wi-Fi Protected Setup (WPS), which makes it easy to set up a connection and enable security protections. Often, these processes are as simple as pushing a button on each device.

Devices can operate one-to-one or one-to-many for connectivity.

Miracast connections over Wi-Fi Direct allow a device to display photos, files, and videos on an external monitor or television.

Wi-Fi Direct for Digital Living Network Alliance (DLNA) lets devices stream music and video between each other.

Wi-Fi Direct Print gives users the ability to print documents directly from a smart phone, tablet, or PC.

### Wireless local area network (WLAN)

In contrast to WPANs, WLANs provide more robust wireless network connectivity over a local area, up to approximately 100 m (328 feet) between an AP and associated clients. The goal is not to connect one device to another but to connect end devices to the backbone network (typically a wired LAN) without the need for cables. WLANs today are based on the IEEE 802.11 standard and are referred to as Wi-Fi networks.

#### Wi‑Fi (IEEE 802.11)

**Wi‑Fi (IEEE 802.11)** is a combination of wireless network protocols based on the IEEE 802.11 family of standards used to allow devices in the wireless LAN access the Internet

WiFi specifies how to physically create a wireless network using approaches similar to the Ethernet standard

WiFi is built into most of today's computers and mobile devices, such as smartphones and IoT devices

WiFi networks transmits have signal at the 2.4 GHz and 5 GHz radio frequency range reaching to about 100 meters, therefore, they are best for indoor use

#### Wi‑Fi generations

| **Wi-Fi Name** | **IEEE Standard**   | **Max Speed** | **Frequency Bands** | **Channel Width** | **Year Released** | **Key Features**                              |
| -------------- | ------------------- | ------------- | ------------------- | ----------------- | ----------------- | --------------------------------------------- |
| Wi-Fi 1        | 802.11b             | 11 Mbps       | 2.4 GHz             | 20 MHz            | 1999              | DSSS, legacy standard                         |
| Wi-Fi 2        | 802.11a             | 54 Mbps       | 5 GHz               | 20 MHz            | 1999              | OFDM, higher speed than 11b                   |
| Wi-Fi 3        | 802.11g             | 54 Mbps       | 2.4 GHz             | 20 MHz            | 2003              | Backward compatible with 11b                  |
| Wi-Fi 4        | 802.11n             | 600 Mbps      | 2.4 & 5 GHz         | 20/40 MHz         | 2009              | MIMO (up to 4x4), frame aggregation           |
| Wi-Fi 5        | 802.11ac            | 6.9 Gbps      | 5 GHz               | 20/40/80/160 MHz  | 2014              | MU-MIMO (DL), Beamforming, 256-QAM            |
| Wi-Fi 6        | 802.11ax            | 9.6 Gbps      | 2.4 & 5 GHz         | 20–160 MHz        | 2019              | OFDMA, MU-MIMO (DL/UL), BSS coloring          |
| Wi-Fi 6E       | 802.11ax (extended) | 9.6 Gbps      | 2.4, 5 & **6 GHz**  | 20–160 MHz        | 2021              | Adds 6 GHz band support                       |
| Wi-Fi 7        | 802.11be            | 46 Gbps       | 2.4, 5 & 6 GHz      | 20–320 MHz        | 2024–2025         | 4K-QAM, MLO, 16x MU-MIMO, preamble puncturing |

**Wi‑Fi 7 highlights**

* Faster speeds (up to 46 Gbps)

Theoretical max: \~46 Gbps using 16 spatial streams.

Practical use: 2–5x faster than Wi-Fi 6.

* 320 MHz channel width

Doubles the 160 MHz max from Wi-Fi 6E.

Only available in the 6 GHz band.

* 4096‑QAM (4K‑QAM)

Increases data rate by 20% compared to 1024-QAM (Wi-Fi 6).

Requires very clean signal conditions.

* Multi-Link Operation (MLO)

Simultaneous use of multiple bands (2.4, 5, 6 GHz).

Increases reliability, throughput, and reduces latency.

* Lower latency

Better for real-time apps (AR/VR, gaming, video conferencing).

* Enhanced MU‑MIMO

Up to 16 spatial streams (was 8 in Wi-Fi 6).

Supports more simultaneous users/devices.

* Preamble puncturing

Allows using clean parts of partially overlapping channels.

Improves spectrum efficiency.

* Improved OFDMA scheduling

More efficient resource allocation, especially in dense environments.

### Wireless metropolitan area network (WMAN)

**Wireless metropolitan area network (WMAN)** is a wireless communications network that covers a large geographic area, such as a city or a suburb. In this type of area, the wireless signal often provides a point-to-point or point-to-multipoint backbone. Wireless can be used to create links at a low cost: organizations need only two end devices that point at each other instead of a complex and costly wired infrastructure.

![](<../.gitbook/assets/Unknown image (453)>)

### WLAN building blocks

#### Access point (AP)

**Access points (APs)** are Layer 2 devices whose primary function is to bridge 802.11 WLAN traffic to 802.3 Ethernet traffic. APs can have internal (integrated) or external antennas to radiate the wireless signal and provide coverage with the wireless network.

#### Basic Service Area (BSA) and Basic Service Set (BSS)

**Basic Service Area (BSA)** physical range of AP signal

The central device in the BSA or wireless cell is an AP, which is close in concept to an Ethernet hub in relaying communication. But, as in an ad hoc network, all devices share the same frequency. Only one device can communicate at a given time, sending its frame to the AP, which then relays the frame to its final destination—this is half-duplex communication.

Although the system might be more complex than a simple peer-to-peer network, an AP is usually better equipped to manage congestion. An AP can also connect one client to another in the same Wi-Fi space or to the wired network—a crucial capability.

The comparison to a hub is made because of the half-duplex aspect of the WLAN client communication. However, APs have some functions that a wired hub simply does not possess. For example, an AP can address and direct Wi-Fi traffic. Managed switches maintain dynamic MAC address tables that can direct packets to ports that are based on the destination MAC address of the frame. Similarly, an AP directs traffic to the network backbone or back into the wireless medium, based on MAC addresses. The IEEE 802.11 header of a wireless frame typically has three MAC addresses but can have as many as four in certain situations. The receiver is identified by MAC Address 1, and the transmitter is identified by MAC Address 2. The receiver uses MAC Address 3 for filtering purposes, and MAC Address 4 is only present in specific designs in a mesh network. The AP uses the specific Layer 2 addressing scheme of the wireless frames to forward the upper-layer information to the network backbone or back to the wireless space toward another wireless client.

**Basic service set (BSS)** closed group of wireless devices connected to one AP within it's BSA

AP is dedicated to centralizing the communication between clients. This AP defines the frequency and wireless workgroup values. The clients need to connect to the AP in order to communicate with the other clients in the group and to access other network devices and resources.

#### SSID and BSSID

**Service Set Identifier (SSID)** To roam between different APs within a network, the APs must share the same network name. This network name is called the Service Set Identifier (SSID), which has as many as 32 ASCII characters and is configured on both the AP and the client stations that wish to join (associate) with this AP. However, the SSID may also require some type of authorization to determine which station has the right to connect. The term WLAN is often used to define both the SSID and the associated parameters (VLAN, security, quality of service \[QoS], and so on). Human-readable name of the AP (doesn't have to be unique)

**BSS identifier (BSSID)** MAC address, usually derived from the radio MAC address and associated with an SSID, - unique

Because this BSSID is a MAC address that is derived from the radio MAC address, APs can often generate several values. This ability allows the AP to support several SSIDs in a single cell.

An administrator can create several SSIDs on the same AP (for example, a guest SSID and an internal SSID). The criteria by which a station is allowed on one or the other SSID will be different, but the AP will be the same. This configuration is an example of Multiple Basic SSIDs (MBSSIDs).

MBSSIDs are basically virtual APs. All of the configured SSIDs share the same physical device, which has a half-duplex radio. As a result, if two users of two SSIDs on the same AP try to send a frame at the same time, the frames will collide. Even if the SSIDs are different, the Wi-Fi space is the same. Using MBSSIDs is only a way of differentiating the traffic that reaches the AP, not a way to increase the capacity of the AP.

SSIDs can be either broadcast (or advertised) or not broadcast (or hidden) by the APs. A hidden network is still detectable. APs periodically send out special frames called "beacon frames" over the air. These frames contain information about the network, including the SSID. Also, when a device wants to connect to a Wi-Fi network, it sends out a "probe request" looking for networks with a specific SSID. Access Points that have the requested SSID respond with a "probe response," providing details about the network and inviting the device to connect.

![](<../.gitbook/assets/Unknown image (454)>)

#### Mapping SSIDs to VLANs

VLANs provide an ideal way of separating users on different WLAN SSIDs when they access the wired side of the network. By associating each SSID to a different VLAN, you can group users on the Ethernet segment the same way that they were grouped in the WLAN. You can also isolate groups from each other, in the same way that they were isolated on the WLAN.

When the frames are in different SSIDs in the wireless space, they are isolated from each other. Different authentication and encryption mechanisms per SSID and subnet isolate them, even though they share the same wireless space.

When frames come from the wireless space and reach the Cisco WLC, they contain the SSID information in the 802.11 encapsulated header. The Cisco WLC uses the information to determine which SSID the client was on.

When configuring the Cisco WLC, the administrator associates each SSID to a VLAN ID. As a result, the Cisco WLC changes the 802.11 header into an 802.3 header, and adds the VLAN ID that is associated with the SSID. The frame is then sent on the wired trunk link with that VLAN ID.

**Distribution system (DS)** interconnect BSS and wired network; it is connection from AP to the switch

WLCs and APs are usually connected to switches. The switch interfaces must be configured appropriately, and the switch must be configured with the appropriate VLANs. The configuration on switches regarding the VLANs is the same as usual. The configuration differs on interfaces though, depending on if the deployment is centralized (using a WLC) or autonomous (without a WLC).

The following types of VLANs are required with WLANs:

Management VLAN

AP VLAN

Data VLAN

The management VLAN is for the WLC management interface configured on the WLC. The APs that register to the WLC can use the same VLAN as the WLC management VLAN, or they can use a separate VLAN. The APs can use this VLAN to obtain IP addresses through DHCP and send their discovery request to the WLC management interface using those IP addresses. To support wireless clients, you will need a VLAN (or VLANs) with which to map the client SSIDs to the WLC. You may also want to use the DHCP server for the clients

Layer 3 mode is the dominant mode today, where the AP interfaces are on a different subnet than the WLC management interface.

Second, you will need either the Layer 3 switch or a router to perform inter-VLAN routing. Usually, inter-VLAN routing is configured along with the VLAN creation.

![](<../.gitbook/assets/Unknown image (455)>)

### WLAN operating modes and topologies

#### Independent BSS (IBSS / ad hoc)

**Independent BSS (IBSS / ad hoc)** is when 2+ wireless devices connect directly without AP. Also called ad hoc network. Examples: Airdrop/Airplay

Because the computers in an ad hoc network communicate without other devices (AP, switch, and so on), the BSS in an ad hoc network is called an IBSS. Computer-to-computer wireless communication is most commonly referred to as an ad hoc network, IBSS, or peer-to-peer (wireless) network.

![](<../.gitbook/assets/Unknown image (456)>)

#### Extended Service Set (ESS)

When the distribution system links two APs, or two cells, the group is called an Extended Service Set (ESS). This scenario is common in most Wi-Fi networks because it allows Wi-Fi stations in two separate areas of the network to communicate and, with the proper design, also permits roaming.

In a Wi-Fi network, roaming occurs when a station moves. It leaves the coverage area of the AP to which it was originally connected and arrives at the BSA of another AP. In a proper design scenario, a station detects the signal of the second AP and jumps to it before losing the signal of the first AP.

Because an overlap exists between the cells, it is better to ensure that the APs do not work on the same frequency (also called a channel). Otherwise, any client that stays in the overlapping area affects the communication of both cells. This problem occurs because Wi-Fi is half duplex. The problem is called co-channel interference and must be avoided by making sure that neighbor APs are set on frequencies that do not overlap.

![](<../.gitbook/assets/Unknown image (457)>)

#### Mesh BSS (MBSS)

**Mesh BSS (MBSS)** is used in situation where it's difficult to run an Ethernet connection to every AP

Multiple AP's form a mesh network which is used to bridge traffic from AP to AP connected to DS

Using one radio, each mesh AP can provide wireless coverage for client devices within its area, while backhauling traffic through the second radio. Usually, network access to users is delivered over the 2.4-GHz frequency and the 5-GHz band is used to backhaul traffic.

![](<../.gitbook/assets/Unknown image (458)>)

#### Additional AP operational modes

Repeater extends range of BSS by re-transmitting any signal from AP Two radios recommended to receive on one channel and retransmit on another channel

![](<../.gitbook/assets/Unknown image (459)>)

Universal workgroup bridge (uWGB) A single wired device can be bridged to a wireless network

Workgroup bridge (WGB) is an AP that is configured to bridge between its Ethernet and wireless interfaces.

To enable the WGB to communicate with the lightweight access point, create a WLAN and make sure that Aironet IE is enabled

is an AP that is configured to bridge between its Ethernet and wireless interfaces.

![](<../.gitbook/assets/Unknown image (460)>)

Outdoor Bridge used to connect networks over long distances without physical cable

![](<../.gitbook/assets/Unknown image (461)>)

### 802.11 frames and association

#### Association process (client ↔ AP)

**Association process** for endpoint to be able to joint the network and send traffic through AP, it must be associated with AP

Active scanning the endpoint probe requests and listens for a probe response from an AP

Passive scanning endpoint listens for beacon messages from AP which are sent periodically by APs to advertise BSS

![](<../.gitbook/assets/Unknown image (462)>)

#### Message types

**Management** used to manage the BSS. Beacon,probe,auth,assoc

**Control** used to control access to the medium. Assist with delivery of management and data frames. RTS,CTS,ACK

**Data** used to send actual data packets

#### 802.11 frame format

![](<../.gitbook/assets/Unknown image (463)>)

Frame Control provides information such as the message type and subtype

Duration/ID depending on the message type it can indicate: the time the channel will be dedicated for transmission of the frame; identifier for the association

Addresses up to 4 add can be present in .11 frame. Destination: the final recipient of the fame; Source: original sender; Receiver immediate recipient; Transmitter immediate sender

Sequence control used to reassemble fragments and eliminate duplicate frames

QoS used to prioritize certain traffic

High throughput control added in .11n to enable HT operations. .11ac is known as Very high throughput (VHT) Wi-Fi

### WLAN architectures

#### Autonomous access points

**Autonomous access points** are self-contained APs, each offering one or more standalone BSS

configured with management IP address to enable remote management

not scalable as each AP have to be configured manually and separately (or could use Cisco Prime Infrastructure)

AP has a single wired Ethernet interface that has to be trunk in case multiple WLANs are operating on the AP

AP inherently trunks 802.11 frames by marking them with the BSSID of the associated WLAN

![](<../.gitbook/assets/Unknown image (464)>)

#### Centralized (controller-based) wireless

The centralized, or lightweight, architecture allows the splitting of 802.11 functions between the controller-based AP, which processes real-time portions of the protocol, and the WLC (Wireless LAN Controller) , which manages items that are not time-sensitive. This model is also called split MAC. Split MAC is an architecture for the Control and Provisioning of Wireless Access Points (CAPWAP) protocol

AP must associate with a WLC (Wireless LAN Controller) to become fully functional

AP handles most of the real-time processes and the WLC performs the management functions

AP needs only a single IP address for the tunnel

AP also has as a single wired Ethernet interface, however WLANs are mapped within the tunnel to WLC so switchport has to be configured as access

#### CAPWAP (Control and Provisioning of Wireless Access Points)

**CAPWAP (Control and Provisioning of Wireless Access Points)** is an open protocol that enables a WLC to manage a collection of wireless APs. CAPWAP control messages are exchanged between the WLC and AP across an encrypted tunnel. CAPWAP includes the WLC discovery and join process, AP configuration and firmware push from the WLC, and statistics gathering and wireless security enforcement.

After the AP discovers the WLC, a CAPWAP tunnel is formed between the WLC and AP. This CAPWAP tunnel can be IPv4 or IPv6. CAPWAP supports only Layer 3 WLC discovery.

Once an AP joins a WLC, the APs will download any new software or configuration changes. For CAPWAP operations, any firewalls should allow the control plane (UDP port 5246) and the data plane (UDP port 5247).

Both data and control connections are part of one CAPWAP tunnel

![](<../.gitbook/assets/Unknown image (465)>)

![](<../.gitbook/assets/Unknown image (466)>)

All MAC functionality that is not real time is processed by the Cisco WLC. The APs handle only real-time MAC functionality, which includes the following:

Frame exchange handshake between client and AP when connecting to a wireless network

Frame exchange handshake between client and AP when transferring a frame

Transmission of beacon frames, which advertise all the nonhidden SSIDs

Buffering and transmission of frames for clients in a power-save operation

Providing real-time signal quality information to WLC with every received frame

Monitoring all radio channels for noise, interference, and other WLANs, and monitoring for the presence of other APs

Wireless encryption and decryption of 802.11 frames

All remaining functionality is managed in Cisco WLC, where time sensitivity is not a concern and WLC-wide visibility is required. Some of the MAC functions that are provided in the Cisco WLC are as follows:

802.11 authentication

802.11 association and reassociation (roaming)

802.11 frame translation and bridging to non-802.11 networks, such as 802.3

Radio frequency (RF) management

Security management

QoS management

#### AP operational modes (controller-based)

Local the default operational mode of APs when connected to the Cisco WLC. When an AP is operating in local mode, all user traffic is tunneled to the WLC, where VLANs are defined.. Can scan rogue APs

Monitor The AP does not transmit at all, but its receiver is enabled to act as a dedicated sensor. The AP checks for IDS events, detects rogue access points

FlexConnect Cisco wireless solution for branch and remote office deployments, to eliminate the need for WLC on each location. In FlexConnect mode, client traffic may be switched locally on the AP instead of tunneled to the WLC. Can scan rogue APs

Sniffer An AP dedicates its radios to receiving 802.11 traffic from other sources, much like a sniffer or packet capture device

Rogue detector AP detecting rogue devices that causes interference by correlating MAC addresses heard on the wired network with those heard over the air

Bridge An AP becomes a dedicated bridge (P2P or point-to-multipoint) between two networks. Two APs in bridge mode can be used to link two locations separated by a distance

Flex+Bridge FlexConnect operation is enabled on a mesh AP

SE-Connect The AP dedicates its radios to spectrum analysis on all wireless channels

Sensor mode the AP can function much like a WLAN client would associating and identifying client connectivity issues within the network in real time without requiring an IT to be on site

#### Controller placement models

Centralized WLC is usually placed in data center (DC) and Unified WLC is placed at higher hierarchy level of a site

Round-trip time (RTT) time it takes for traffic to reach WLC via CAPWAP tunnel, where is decapsulated, examined and sent to destination. Latency should be less than 100 ms, so communication can be maintained in real time. AP may disconnect and look for another more reliable WLC, if any.

![](<../.gitbook/assets/Unknown image (467)>)

Embedded WLC is co-located with access layer switch

![](<../.gitbook/assets/Unknown image (468)>)

Mobility Express WLC just one embedded WLC to manage all AP's

![](<../.gitbook/assets/Unknown image (469)>)

#### AP join state machine

Every AP and WLC must also authenticate each other with digital certificates. Cisco lightweight APs are designed to be “touch free,” and don't have to be pre-configured. You have to configure the switch port, where the AP connects, with the correct access VLAN, access mode, and inline power settings

AP states

1. AP boots: Once an AP receives power, it boots on a small IOS image so that it can work through the remaining states and communicate over its network connection.The AP must also receive an IP address from either a DHCP server or a static configuration so that it can communicate over the network
2. WLC discovery: The AP goes through a series of steps to find one or more controllers that it might join
3. CAPWAP tunnel: The AP attempts to build a CAPWAP tunnel with one or more controllers. The tunnel will provide a secure Datagram Transport Layer Security (DTLS) channel for subsequent AP-WLC control messages. The AP and WLC authenticate each other through an exchange of digital certificates.
4. WLC join: The AP selects a WLC from a list of candidates and then sends a CAPWAP Join Request message to it. The WLC replies with a CAPWAP Join Response message.
5. Download image: The WLC informs the AP of its software release. If the AP’s own software is a different release, the AP downloads a matching image from the controller, reboots to apply the new image, and then returns to step 1. If the two are running identical releases, no download is needed
6. Download config: The AP pulls configuration parameters down from the WLC. Settings include RF, SSID, security, and quality of service (QoS) parameters
7. Run state: Once the AP is fully initialized, the WLC places it in the “run” state. The AP and WLC then begin providing a BSS and begin accepting wireless clients
8. Reset: If an AP is reset by WLC, it tears down existing client associations and any CAPWAP tunnels to WLCs. The AP then reboots and starts through the entire state machine again

You can predownload the new release to the controller’s APs. APs will download the new image but will keep running the previous release > reboot to apply new release during MW

#### Discovering a WLC

1. AP sends a unicast CAPWAP Discovery Request to a controller’s IP over UDP port 5246 or a broadcast to the local subnet > WLC returns a CAPWAP Discovery Response to AP
2. AP attempts to contact as many controllers as possible to build a list of candidates
3. DHCP server can also send DHCP option 43 to suggest a list of WLC addresses to the AP // specified as a hex string. The hex string represents the IP address of the WLC
4. AP attempts to resolve the name CISCO-CAPWAP-CONTROLLER.localdomain with a DNS request (where localdomain is the domain name learned from DHCP). If the name resolves to an IP address, the AP attempts to contact a WLC at that address
5. If none of the steps has been successful, the AP resets itself and starts the discovery process all over again

#### Selecting a WLC

1. If the AP has previously joined a controller and has been configured or “primed” with a primary, secondary, and tertiary controller, it tries to join those controllers in succession
2. If the AP does not know of any candidate controller, it tries to discover one
3. The AP attempts to join the least-loaded WLC, in an effort to load balance APs (During the discovery phase, each

controller reports its load—the ratio of the number of currently joined APs to the total AP capacity)

Once WLC reaches the maximum number (defined by license/platform), it rejects an AP with the lowest priority to make room for a new one that has a higher priority (can be configured)

When there is a master controller enabled, all newly added access points with no primary, secondary, or tertiary controllers assigned associate with the master controller on the same subnet

#### Maintaining WLC availability

Keepalives are sent from the WLC to APs every 30 seconds. If a keepalive is not answered, an AP escalates the test by sending four more keepalives at 3-second intervals. If still not answered, the AP moves quickly to find a successor controller to join. WLCs also support high availability (HA) with stateful switchover (SSO) redundancy.

{% hint style="info" %}
You can configure the local router to relay broadcast CAPWAP discovery (UDP/5246) to specific controller addresses.
{% endhint %}

| router(config)# ip forward-protocol udp 5246 router(config)# interface vlan n router (config-int)# ip helper-address WLC1-MGMT-ADDR router(config-int)# ip helper-address WLC2-MGMT-ADDR |   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

#### WLC activities (RRM)

Dynamic channel assignment (DCA) The WLC can automatically choose and configure the RF channel used by AP, based on other active access points in the area

When a radio changes from one channel to another, the user gets disconnected shortly. Increase DCA to more than 10 mins (default DCA) to reduce the number of times users are disconnected

Transmit power optimization The WLC can automatically set the transmit power of each AP based on the coverage area needed

Self-healing wireless coverage If an AP radio dies, the coverage hole can be “healed” by turning up the transmit power of surrounding APs automatically

Flexible client roaming Clients can roam between APs with very fast roaming times

Dynamic client load balancing If two or more APs are positioned to cover the same geographic area, the WLC can associate clients with the least used AP

RF monitoring Remotely gather information about RF interference, noise, signals from neighboring APs, and signals from rogue APs

Security management The WLC can authenticate clients from a central service and can require wireless clients to obtain an IP address from a trusted DHCP server

### Cloud-managed WLAN (Meraki)

AP management function is managed by Internet cloud. AP management is provided through the Meraki cloud website dashboard

The Cisco Meraki cloud also adds the intelligence needed to automatically instruct each AP on which channel and transmit power level to use. It can also collect information from all of the APs about things such as RF interference, rogue or unexpected wireless devices that were overheard, and wireless usage statistics

Control plane Traffic used to control, configure, manage, and monitor the AP itself

Data plane End-user traffic passing through the AP

![](<../.gitbook/assets/Unknown image (470)>)

![](<../.gitbook/assets/Unknown image (471)>)

![](<../.gitbook/assets/Unknown image (472)>)

### Roaming

**Roaming** occurs when a wireless client device moves outside the usable range of one access point (AP) and connects to a different AP within the range

Client actively scans channels and sends probe requests to discover candidate APs, and then the client selects one and tries to reassociate with it

If AP 1 still has any leftover wireless frames destined for the client after the roam, it forwards them to AP 2 over the wired infrastructure

Association Requests are used to form a new association and join BSS

Reassociation Requests are used to roam from one AP to another, preserving the client’s original association status

![](<../.gitbook/assets/Unknown image (473)>)

#### Layer 2 roaming

no need to tunnel client data between WLCs since the clients IP subnet doesn't change

Intra-controller Roam managed by WLC (10ms)

![](<../.gitbook/assets/Unknown image (474)>)

Inter-controller Roam between WLC's

![](<../.gitbook/assets/Unknown image (475)>)

#### Layer 3 roaming

when both WLC's are using different VLAN ID and range

CAPWAP is also formed between WLCs in such situation. The first or statically preconfigured WLC1 will become an anchor for client, if he roams to WLC2 with different VLAN ID and subnet range, WLC2 will take on the foreign role and same IP from WLC1 VLAN range would still be left for the client

Mobility groups is used to cover larger area. WLC's trusts and maintaining relationship only with WLC's in the same group; Roaming is possible, but CCKM and key caching do not work

![](<../.gitbook/assets/Unknown image (476)>)

Mobility MAC cluster should be configured to set up HA in WLC mobility group

![](<../.gitbook/assets/Unknown image (477)>)

To achieve efficient roaming, both of these processes should be streamlined as much as possible

DHCP The client may be programmed to renew the DHCP lease on its IP address or to request a new address

Client authentication The controller might be configured to use an 802.1x method to authenticate each client on a WLAN

Cisco controllers offer three techniques to minimize the time and effort spent on key exchanges during roams

#### Fast roaming techniques

Cisco Centralized Key Management (CCKM) One controller maintains a database of clients and keys on behalf of its APs and provides them to other controllers and their APs as needed during client roams. CCKM requires Cisco Compatible Extensions (CCX) support from clients

Key caching Each client maintains a list of keys used with prior AP associations and presents them as it roams

802.11r This 802.11 amendment addresses fast roaming or fast BSS transition; a client can cache a portion of the authentication server’s key and present that to future APs as it roams

Adaptive R feature under .11r allows the legacy clients to connect while still allowing other clients to use fast transition

### Location services (RTLS)

AP can use the received signal strength (RSS) of a client device as a measure of the distance between the two. The components of a wireless network can be coupled with additional resources to provide real-time location services (RTLS)(important part of tracking assets in a business; eg tracking hosts around shoppping park)

As long as Wi-Fi is enabled on the device, it will probably probe for available APs on all channels that would locate not even connected device

![](<../.gitbook/assets/Unknown image (478)>)

### Multicast over WLAN

The client station sends a directed (Layer 2 unicast) frame, which requires an acknowledgement, containing multicast data (an IGMP Join) to the access point.

The access point responds with an 802.11 acknowledgement frame.

• If the acknowledgement is not received the client station resends the frame.

• • The access point forwards the IGMP Join across the network.

• • When the access point receives the requested multicast, it forwards the packets on 802.11 group (broadcast or multicast) frames which require no acknowledgement and can be consumed by all client stations.

• • When the client station is done with the multicast it sends an IGMP Leave in a directed frame.

• The access point responds with an 802.11 acknowledgement frame.

• The access point forwards the IGMP leave across the network.

• The Ethernet network IGMP process determines if multicast continues to other clients.

### Building and operating a WLAN

#### Connecting a Cisco AP

To configure and manage Cisco APs, you can connect a serial console cable from your PC to the console port on the AP. Once the AP is operational and has an IP address, you can also use Telnet or SSH to connect to its CLI over the wired network. Autonomous APs support browser-based management sessions via HTTP and HTTPS. You can manage lightweight APs from a browser session to the WLC

#### Accessing a Cisco WLC

To connect and configure a WLC, you will need to open a web browser to the WLC’s management address with either HTTP or HTTPS. This can be done only after the WLC has an initial configuration and a management IP address assigned to its management interface. Users can be authenticated against an internal list of local usernames or against AS.

When you are successfully logged in, the WLC will display a monitoring GUI dashboard. You will not be able to make any configuration changes there, so you must click on the Advanced link in the upper-right corner. This will bring up the full WLC GUI.

{% code title="Common WLC show commands" %}
```
show sysinfo
show wlan summary
show ap [uptime | summary]
show ap dot11 24ghz { summary | load-info }
show ap dot11 5ghz load-info
show wireless client vlan summary
show wireless stats ap [discovery|history|join summary]
```
{% endcode %}

#### Switch port connected to WLC (high level)

#### WLC ports

Service port: Used for out-of-band management, system recovery, and initial boot functions; always connects to a switch port in access mode

Distribution system port: Used for all normal AP and management traffic; usually connects to a switch port in 802.1Q trunk mode

Console port: Used for out-of-band management, system recovery, and initial boot functions; asynchronous connection to a terminal emulator (9600 baud, 8 data bits, 1 stop bit)

Redundancy port: Used to connect to a peer controller for high availability (HA) operation

#### WLC interface types

Management interface: Used for normal management traffic, such as RADIUS user authentication, WLC-to-WLC communication, web-based and SSH sessions, SNMP, NTP, syslog, and so on. The management interface is also used to terminate CAPWAP tunnels between the controller and its APs

Redundancy management: The management IP address of a redundant WLC that is part of a high availability pair of controllers. The active WLC uses the management interface address, while the standby WLC uses the redundancy management address

Virtual interface: IP address facing wireless clients when the controller is relaying client DHCP requests, performing client web authentication, and supporting client mobility RFC defined following address blocks: 192.0.2.0/24, 198.51.100.0/24, 203.0.113.0/24

Service port interface: Bound to the service port and used for out-of-band management. Is assigned to a management VLAN so that you can access the controller with SSH or a web browser to perform initial configuration or for maintenance Supports only a single VLAN, so the corresponding switch port must be configured for access mode only

Dynamic interface: map WLANs to VLANs. One Dynamic interface for each WLAN

RADIUS Server Overwrite interface check box to enable the per-WLAN RADIUS source support:

1.When enabled, the controller uses the interface specified on the WLAN configuration as identity and source for all RADIUS related traffic on that WLAN.

2. When disabled, the controller uses the management interface

![](<../.gitbook/assets/Unknown image (479)>)

Port towards the WLC must be trunk, port towards to access point is in access vlan for access points

LAG (Etherchannel equivalent) is configured between DS and WLC to get most out of a switchport. LACP or PaGP are not supported

![](<../.gitbook/assets/Unknown image (480)>)

Based on the switch port configuration, the AP is connected to the switch on an access port (the VLAN for AP to get DHCP). The WLC is connected to the switch on a trunk port, allowing VLANs for WLC management (VLAN 11), AP (VLAN 12), and the wireless clients (VLAN 14).

The AP and WLC create a CAPWAP tunnel.

The client associates to the AP with an SSID of "CORP."

The AP sends the client data that is marked with SSID "CORP" through the CAPWAP tunnel to the WLC.

The WLC decapsulates the CAPWAP traffic.

The SSID of "CORP" is mapped in the WLC to VLAN ID 14.

The WLC tags the data with VLAN 14 before sending it back on the trunk port (where VLAN 14 is allowed) to the switch.

The switch sends it on to the network (based on the destination in the packet).

![](<../.gitbook/assets/Unknown image (481)>)

Autonomous AP connects to a trunk port. On the trunk, a native (untagged) VLAN is required for management of the AP. By default, all VLANs are allowed over the trunk link. To enhance security, you should specify which VLANs are permitted over the trunk link, which should include the AP management VLAN.

Based on the switch port configuration, the AP is connected to the switch as a trunk port, allowing VLANs for AP management (VLAN 12) and the wireless clients (VLAN 14).

The client associates to the AP with an SSID of "CORP."

The SSID of "CORP" is mapped in the AP to VLAN ID 14.

The AP tags the data with VLAN 14 before sending it on the trunk port (where VLAN 14 is allowed) to the switch.

The switch may send it on to the network (based on the destination in packet).

![](<../.gitbook/assets/Unknown image (482)>)

### Configuring a WLAN (SSID → VLAN)

To complete the path between the SSID and the VLAN, you must first define a WLAN on the controller

Every AP must broadcast beacon management frames at regular intervals to advertise the existence of a BSS. Because each WLAN is bound to a BSS, each WLAN must be advertised with its own beacons. Beacons are normally sent 10 times per second, or once every 100 ms, at the lowest mandatory data rate. The more WLANs you have created, the more beacons you will need to announce. If you create too many WLANs, a channel can be starved of any usable airtime. Clients will have a hard time transmitting their own data because the channel is overly busy with beacon transmissions coming from the AP. As a rule of thumb, always limit the number of WLANs to five or fewer; a maximum of three WLANs is best

#### Step 1. Configure a RADIUS server

Security > AAA > RADIUS > Authentication

#### Step 2. Create a dynamic interface

Controller > Interfaces

#### Step 3. Create a new WLAN

WLAN ID is only locally significant and is not passed between controllers

#### Security

#### QoS profiles

* Platinum (voice)
* Gold (video)
* Silver (best effort)
* Bronze (background)

{% hint style="info" %}
By default, a controller blocks management sessions initiated from a WLAN.\
This prevents access to the WLC GUI/CLI from wireless clients.\
You can change this globally via **Mgmt Via Wireless**.
{% endhint %}

#### Client isolation (peer-to-peer blocking)

Peer 2 Peer blocking is a feature that blocks direct communication between wireless

clients that present on the WLAN on the same Wireless LAN Controller

#### AAA override and dynamic VLAN assignment (ISE)

Enable AAA Override and Cisco Identity Services Engine (ISE) Assign VLANs features are

often used together

Enable AAA Override is a feature that allows the AAA server to override the VLAN assignment of a user's device. This allows the AAA server, such as Cisco ISE, to assign a specific VLAN to a user's device based on the user's credentials and the policies configured on the AAA server
