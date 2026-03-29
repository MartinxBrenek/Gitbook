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

# MACsec

### Media Access Control Security (MACsec)

**Media Access Control Security (MACsec)** is per-hop MAC-layer (link-layer) encryption. It provides line-rate encryption at Ethernet port speeds (1/10/40/100Gbps) bidirectionally regardless of packet size by executing the encryption function in the PHY. Unlike IPsec, which is typically performed on a centralized ASIC, MACsec is enabled per-port with no performance impact.

It can be configured on the link between switch-to-switch or switch-to-host. The traffic is then unencrypted as it is processed internally within the switch. This allows the switch to look into the inner packets for things like SGT tags to perform packet enforcement or QoS prioritization

Note MACsec was primarily designed to be used in conjunction with IEEE 802.1X

![](<../../.gitbook/assets/Unknown image (908)>)

For a router capable of forwarding terabits of traffic, IPsec encryption will be the bottleneck and limiting factor of maximum throughput of the device. For example, if a router has multiterabit forwarding capabilities, and ten 100-GE ports require encryption at line-rate, the MACsec solution offers 1 Tbps of AES-256 encryption on each port, regardless of the packet size, so the overall encryption throughput utilizing MACsec can leverage the full forwarding capability of the router, while also offering encryption of each bit on the Ethernet wire.

While MACsec offers a new set of high-speed encryption capabilities, IPsec is now, and will remain, a vital element to network designs, offering an extremely agile design option when IP (public or private) is the transport available. MACsec offers network designers another option when Ethernet can be leveraged as the endto-end WAN/Metro transport and high-speed encryption is vital to the overall business requirement.

### 802.1AE header (MACsec tag)

16-byte MACsec Security Tag field (802.1AE header) and a 16-byte Integrity Check Value (ICV) field are added.

MACsec EtherType (first two octets): Set to 0x88e5, designating the frame as a MACsec frame

Tag Control Information/Association (TCI/AN) (third octet) Number field, designating the version number and if confidentiality or integrity is used on its own

Short Length (SL) (fourth octet): field designating the length of the encrypted data

Packet Number (octets 5–8): The packet number for replay protection and building of the initialization vector

Secure Channel Identifier (SCI) (octets 9–16): for classifying the connection to the virtual port

![](<../../.gitbook/assets/Unknown image (909)>)

MACsec offers complete transparency as it does not deal with the content of the Ethernet frame, but only encrypts it and transmits it. In other words, MACsec doesn't examine what's inside the frame – there could be an IP packet, MPLS label, VLAN tag or anything else, but MACsec doesn't see it and encrypts the entire frame as a single unit

When Cisco says "line rate MACsec", this is after accounting for MACsec overhead of 32 B

### Security Association Protocol (SAP)

**Security Association Protocol (SAP)** is Cisco proprietary and used only between Cisco switches. It has many limitations, so MKA is recommended.

### MACsec Key Agreement (MKA)

The purpose of MKA is to provide a method for discovering MACsec peers and negotiating the security keys needed to secure the link

It is a MACsec keying mechanism that provides the required session keys and manages the required encryption keys. Supported on both Switch-to-host and Switch-to-switch connections

MKA and MACsec are implemented after successful authentication using certificate-based MACsec or Pre Shared Key (PSK) framework

#### MKA terminology

**Connectivity Association (CA)** A security relationship between MACsec-capable devices on a LAN or WAN.

**Master Session Key (MSK)** – generated during EAP exchange. Supplicant and authentication server uses the MSK to generate the CAK and CKN. MSK is not used when MKA Pre-Shared-Key is configured.

**Connectivity Association Key (CAK)** – used by MKA to derive a transient session key called the SAK. Is either a manually entered Pre-shared Key, derived from the MSK if an EAP method is used or a key delivered from a MKA Key Server. The CAK is a long-lived master key used to generate all other keys used for MACsec.

**Connectivity Key Name (CKN)** – used as a container for storing the CAK. CKN is transmitted across the wire in clear text to the peer to assist the peer in validating the CAK

**Key Encrypting Key (KEK)** Used to protect the MACsec keys (SAK).

**Secure Association Key (SAK)** – a key derived from the CAK and used to encrypt data sent between devices

**Key Server (KS)** Responsible for selecting and advertising a cipher suite and generating the SAK.

**Secure Channel Identifier (SCI)** – a concatenation of the MAC Address and the Virtual Port ID. Virtual Port ID can be determined from the IF-ID column.

**Two methods to derive encryption keys:**

When Pre-shared Keys (PSK) are used the pre-shared key (PSK) is equal to the connectivity association key (CAK) and the connectivity association key name (CKN) must be manually entered and is stored in the device’s configuration. The CAK is then used to generate the rest of the MACsec encryption keys (ICK, KEK, and SAK)

When 802.1x EAP-TLS is utilized the master session key (MSK) is generated as a by-product of the EAP Authentication process. The CAK is then derived from the MSK. Unlike the pre-shared key method

where the connectivity association key name is manually entered, the CAK is also derived from the MSK. As withthe pre-shared key method, the remainder of the MACsec keys derived from the CAK

Note the .1x method requires:

Require Certificate Authority

ISE 2.0 +

802.1x AAA config

can use local switch user

CAK and SAK are not visible for user

Device certificates (SUDI) does not have any direct role in generating of secret keys

![](<../../.gitbook/assets/Unknown image (910)>)

![](<../../.gitbook/assets/Unknown image (911)>)

### Operation

MACsec frames are encrypted and protected with an integrity check value (ICV). When the switch receives frames from the MKA peer, it decrypts them and calculates the correct ICV by using session keys provided by MKA. The switch compares that ICV to the ICV within the frame. If they are not identical, the frame is dropped. The switch also encrypts and adds an ICV to any frames sent over the secured port

The EAP framework implements MKA as a newly defined EAP-over-LAN (EAPOL) packet. EAP authentication produces a master session key (MSK) shared by both partners in the data exchange. Entering the EAP session ID generates a secure connectivity association key name (CKN). The switch acts as the authenticator for both uplink and downlink; and acts as the key server for downlink. It generates a random secure association key (SAK), which is sent to the client partner. The client is never a key server and can only interact with a single MKA entity, the key server. After key derivation and generation, the switch sends periodic transports to the partner at a default interval of 2 seconds.

The packet body in an EAPOL Protocol Data Unit (PDU) is referred to as a MACsec Key Agreement PDU (MKPDU). MKA sessions and participants are deleted when the MKA lifetime (6 seconds) passes with no MKPDU received from a participant. For example, if a MKA peer disconnects, the participant on the switch continues to operate MKA until 6 seconds have elapsed after the last MKPDU is received from the MKA peer.

Note The MSK is delivered in the RADIUS vendor-specific attributes (VSAs) MS-MPPE-Send-Key and MS-MPPE-Recv-Key. Along with the MSK, the authentication server sends an EAP key identifier that is derived from the EAP exchange and is delivered to the authenticator in the EAP Key-Name attribute of the Access-Accept message.

![](<../../.gitbook/assets/Unknown image (912)>)

![](<../../.gitbook/assets/Unknown image (913)>)

### Rekey

MACsec periodically performs rekey of the SAK based on the rekey period. The rekey period is calculated according to the speed of the port or it can be manually set

It is recommended to configure keys such that there is overlap between the lifetime of the keys so that CAK rekey is successful and there is a seamless transition between the Keys/CA (without any traffic loss or session restart)

show mka sessions can show the rekey counter, the Pairwise CAK rekyes counter indicate that the CAK has been changed

### Fallback key

The Fallback Key feature establishes an MKA session with the pre-shared fallback key whenever the primary pre-shared key (PSK) fails to establish a session because of key mismatch. This feature prevents downtime and ensures traffic hitless scenario during CAK mismatch (primary PSK) between the peers. The purpose of the fallback key chain is to act as a last resort key. The fallback key feature is only applicable for PSK based MKA or MACsec sessions.

### Replay protection window size

Replay protection is a feature provided by MACsec to counter replay attacks. Each encrypted packet is assigned a unique sequence number and the sequence is verified at the remote end. Frames transmitted through a Metro Ethernet service provider network are highly susceptible to reordering due to prioritization and load balancing mechanisms used within the network.

A replay window is necessary to support use of MACsec over provider networks that reorder frames. Frames within the window can be received out of order, but are not replay protected. The default window size is set to 64. Use the macsec replay-protection window-size command to change the replay window size. The range for window size is 0 to 4294967295.

The replay protection window may be set to zero to enforce strict reception ordering and replay protection.

### MKA high availability

feature adds support for Platforms which support SSO redundancy mode. The feature allows to preserve the existing MKA sessions

### MACsec eXtended Packet Numbering (XPN)

MACsec uses Packet Numbers (PN) to ensure no packet is repeated. Every MACsec frame contains 32-bit PN number and it is unique for a given SAK. Upon PN exhaustion, SAK rekey takes place to refresh the keys. Since 32 bits minimum-sized IEEE 802.3 frames can be sent in approximately 3 minutes at 10 Gb/s, this can force SAK rekey.

For higher capacity links like 40 Gb/s, PN will exhaust within few seconds which will bring overhead of frequent SAK rekey to the control plane. XPN feature in MKA/MACsec eliminates this frequent SAK rekey problem that may occur in high capacity links.

XPN cannot be supported on 9400

MACSEC XPN Link work only if the devices on both sides of the link support XPN

When XPN is used, the PN value for MACsec frame would be logically of 64 bit. MACsec frame would contain the lowest 32 bits only and most significant 32 bits would be maintained by the peer itself i.e. both sending and receiving peer. The most significant 32 bits of the PN will be incremented at receiving end when the MSB of LLPN for respective peer is set and the MSB of PN value received in MACsec frame has zero value. By this way, both sending and receiving peer would maintain same PN value without changing MACsec frame structure. Since we are using 64 bit PN value, so it will require years to exhaust the PN and hence the frequent SAK rekey will get eliminated.

### Switch-to-host (downlink) MACsec

Encryption on a link between an endpoint and a switch. The encryption between the endpoint and the switch is handled by the MKA keying protocol. This requires a MACsec-capable switch and a MACsec-capable supplicant on the endpoint (such as Cisco AnyConnect). The encryption on the endpoint may be handled in hardware (if the endpoint possesses the correct hardware) or in software, using the main CPU for encryption and decryption. On cisco switch we can configure encryption manually per port or dynamically as an authorization option from Cisco ISE. If ISE returns an encryption policy with the authorization result, the policy issued by ISE overrides anything set using the switch CLI

### Switch-to-switch (uplink) MACsec

Encryption on a link between switches with 802.1AE. By default, uplink MACsec uses Cisco proprietary SAP encryption. The encryption is the same AES-GCM-128 encryption used with both uplink and downlink MACsec. Uplink MACsec may be achieved manually or dynamically. Dynamic MACsec requires 802.1x authentication between the switches.

On ingress the MACsec frame is decrypted in the PHY prior to performing all ingress functions (MPLS label imposition, queuing, scheduling, access control lists \[ACLs], etc.). On egress, the process is reversed such that Layer 2–Layer 7 services are performed prior to MACsec encryption of the frame, which is done on the PHY

### WAN MACsec

The WAN MACsec offering is standards based but offers additional capabilities not found in earlier MACsec capabilities. More specifically, MACsec can be leveraged by enterprise customers over public carrier Ethernet offerings, allowing customers to adapt to the public carrier Ethernet service offering and capabilities (or restrictions).

New enhancements for WAN MACsec include .1q tag in clear

#### 802.1Q tag in the clear

This enhancement offers the ability to expose the 802.1Q tag outside the encrypted MACsec header. Exposing this field offers a multitude of design options with MACsec

While offering high-speed encryption, the multipoint use case exposes limitations and impracticalities in recent MACsec-offered solutions. Why: Because earlier MACsec solutions did not offer the ability to expose the 802.1Q tag in the header, requiring a physical Ethernet connection on the central site, per branch. This was not a realistic design due to complexity of cabling, cost of each port, and “box” real estate required in the router to terminate this 1-to-1 remote site to physical-port requirement.

As described earlier, and as shown in Figure 10, the original MACsec header format encoded the 802.1Q tag as part of the encrypted payload, thus hiding it from the public Ethernet transport, which also limited the topologies and network design options that could be leveraged when transporting Ethernet frames over public or private

Ethernet.

One of the primary use cases designers are looking to leverage with this new “tag in the clear” capability is the ability to build hub/spoke networks with WAN MACsec over public Ethernet Virtual Private Line (E-LINE) services.

It should be noted that while Cisco WAN MACsec solution can leverage tag in the clear for virtual segmentation of connections, these tags can also leverage the 802.1p bits carried in that tag, for QoS service offerings. Without this capability, the QoS offerings will be much more coarse and typically very limited.

![](<../../.gitbook/assets/Unknown image (914)>)

![](<../../.gitbook/assets/Unknown image (915)>)

### Use cases

WAN MACsec is used mainly for Secure High-Speed Data Center

Cloud Interconnection or Secure High-Speed Branch Router Backhaul

Carrier Ethernet Transport

To elaborate further, Metro Ethernet Forum (MEF) standards dictate a specific set of well known MAC addresses deemed as “for me” frames to the carrier Ethernet forwarders—meaning transit Carrier Ethernet switches consume the frames containing these MAC addresses into their control plane for processing. Current MACsec and MKA implementations leverage an EAP over LAN (EAPoL) packet for MKA key negotiation and these EAPoL MAC addresses fall under the MEF “well known” MAC addresses for consumption. This means that customers deploying

MACsec over a public Carrier Ethernet transport that operate this Ethernet service with Carrier Ethernet switches that consume these EAPoL frames, cannot leverage MACsec across these providers.

To mitigate this problem, Cisco introduced the ability for the operator deploying WAN MACsec to change the EAPoL destination address and/or EtherType to an address that is defined in the provider’s bridge as “uninteresting.”

The “eapol destination-address” command allows the operator to change the destination MAC address of an EAPoL packet that is transmitted on an interface towards the service provider Ethernet transport

Network designers, for example, can now leverage this ability to apply a logical Layer 3 subinterface per remote site that is a subrate of bandwidth from say the 10 GE PHY, offering subrate capabilities from the physical bandwidth. The subinterface can support E-LINE or E-LAN services and can also support hierarchical traffic shaping to align with the prescribed subrate interface

When planning for a smaller number of remote branches, it's crucial to consider the Security Association (SA) scale limitations of the PHY layer for MACsec. Each physical Ethernet interface that supports MACsec has a vendor-defined limit on the number of SAs it can handle.

For example, on the Cisco ASR 1001-X, each 10GE PHY interface supports up to 64 SAs. However, to accommodate hitless key rollover, this number is effectively halved, reducing the practical limit to 32 branch sites per interface. In contrast, on the ASR 9000, a 100GE interface can support up to 256 SAs, highlighting that SA capacity is hardware-dependent and a key consideration when designing WAN MACsec deployments.

Important Note: The SA limit applies per interface, not per router. A hub site router can scale beyond these limitations by leveraging multiple 10GE interfaces.

Secure IP/MPLS and Metro Ethernet Backbone Networks

The per-hop encryption of MACsec offers high-speed per link encryption, while offering complete transparency to the functions of an IP/MPLS architecture

MACsec is being leveraged as the encryption recommendation for newly offered segment routing

capabilities, for all of the reasons listed previously, specifically offering 100-Gbps encryption while remaining transparent to the segment routing control and data plane functions and service requirements needed per hop.

Secure PE-CE Links for Managed Private IP VPN Transport

In the case of an service provider (SP)-managed service, SPs could leverage WAN MACsec to offer customers a secure encrypted PE to CE backhaul link to the provider cloud (e.g. PE device). The SP could extend this security service end to end if the SP expanded MACsec into their IP/MPLS MPLS backbone

It would eliminate the need for the CE to deploy IPsec overlay solutions (DMVPN or GET VPN). This type of deployment would greatly reduce the complexity for the end customers, not having to deploy a secure IP VPN overlay, while reducing operating expenses (OpEx) on the SP side and expanding the SP service catalog to their end customers

Hybrid Design Using WAN MACsec with IPsec

Consider the hybrid design example in Figure 20. In a typical 2-tier design, the option would be to leverage WAN MACsec in the IP/MPLS backbone (regional hubs and DC edge routers) where links speeds could target 10-100 Gbps. The branch locations, typically requiring lower speed links but higher volume of locations, can leverage IPsec with DMVPN or Cisco IWAN, to take advantage of the higher scale site termination IPsec and DMVPN offers.

This hybrid encryption design approach leverages the strengths of each encryption technology, with IPSec targeting higher scale SAs with lower encryption throughput, and MACsec optimizing the solution through extremely high-speed, lower-scale SAs and transparency for MPLS labels, Segment Routing, without the need for MPLS over GRE tunnels.

![](<../../.gitbook/assets/Unknown image (916)>)

### Comparing MACsec to IPsec

While WAN MACsec is that de facto high-speed solution moving forward, it should not be thought of as a replacement for IPSec, but rather another set of tools in the encryption tool bag moving forward, and in some cases, deployed in combination with IPsec in larger scale deployments

● MACsec supports line-rate encryption performance (100 Gbps+), regardless of the MTU and packet size

● MACsec is transparent to upper layer protocols (IPv4/v6, MPLS labels)

● IPsec is extremely flexible from an underlying transport perspective (completely agnostic)

● IPsec supports massive scale (DMVPN moving beyond 4000 connections) from an SA termination perspective

● MACsec support will be dictated by the hardware’s Ethernet PHY capabilities

![](<../../.gitbook/assets/Unknown image (917)>)

[https://www.cisco.com/c/dam/en/us/td/docs/solutions/Enterprise/Security/MACsec/WP-High-Speed-WAN-Encrypt-MACsec.pdf](https://www.cisco.com/c/dam/en/us/td/docs/solutions/Enterprise/Security/MACsec/WP-High-Speed-WAN-Encrypt-MACsec.pdf)

### Configuration (switch-to-switch MACsec)

![](<../../.gitbook/assets/Unknown image (918)>)

<\<MACsec\_C9k.pptx>>

| Configure key chain for PSK                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| key chain $key\_chain\_name macsec                                                                                                                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| key                                                                                                                                                                                                                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| cryptographic-algorithm aes-256-cmac                                                                                                                                                                                                                                                                              | Encryption algorithm for MKA Control packets                                                                                                                                                                                                                                                                                                                                                                                                                              |
| key-string <32/64 hex>                                                                                                                                                                                                                                                                                            | depends on type of algorithm used 256 requires 64, for 128 32 is enough                                                                                                                                                                                                                                                                                                                                                                                                   |
| Configure MKA policy                                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| mka policy $policy\_name                                                                                                                                                                                                                                                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| macsec-cipher-suite gcm-aes-128                                                                                                                                                                                                                                                                                   | Encryption algorithm for Data packets                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Configure macsec on interface                                                                                                                                                                                                                                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| int <>                                                                                                                                                                                                                                                                                                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| macsec access-control should-secure                                                                                                                                                                                                                                                                               | Should-Secure (default): The switch attempts MKA. If MKA succeeds, the switch sends and receives encrypted traffic only. If MKA times out or fails, the network device permits unencrypted traffic. Must-Secure: The network device attempts MKA. If MKA succeeds, only encrypted traffic is sent or received. If MKA times out or fails, the connection is treated as an authorization failure by terminating the session and retry authentication after a quiet period. |
| macsec network-link                                                                                                                                                                                                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| mka policy $policy\_name                                                                                                                                                                                                                                                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| mka pre-shared-key key-chain $key\_chain\_name                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Example key chain KEY macsec key CAFE cryptographic-algorithm aes-256-cmac key-string 1234567890123456789012345678901212345678901234567890123456789012 mka policy MACSEC macsec-cipher-suite gcm-aes-256 interface TenGigabitEthernet1/1/3 macsec network-link mka policy MACSEC mka pre-shared-key key-chain KEY | Note one mka policy can be utilized by multiple ports                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ROLLBACK interface no macsec network-link no mka policy $policy\_name no mka pre-shared-key key-chain $key\_chain\_name ! no mka policy $policy\_name no key chain $key\_chain\_name macsec                                                                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| show mka \[policy \| session \| statistics]                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| show macsec interface                                                                                                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
