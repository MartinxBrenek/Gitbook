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

# IPsec

### IP Security (IPsec)

**IP Security (IPsec)** is a framework of protocols combined to ensure the confidentiality, integrity, and authenticity of data transmitted between devices over unsecured networks such as the Internet

Confidentiality: by encrypting our data, nobody except the sender and receiver will be able to read our data.

Integrity: we want to make sure that nobody changes the data in our packets

By calculating a hash value, the sender and receiver will be able to check if changes have been made to the packet.

Authentication: the sender and receiver will authenticate each other to make sure that we are really talking with the device we intend to.

Anti-replay: even if a packet is encrypted and authenticated, an attacker could try to capture these packets and send them again

By using sequence numbers, IPsec will not transmit any duplicate packets.

Note ACL can be defined to limit the only the intended traffic that should be encrypted and sent through IPsec tunnel

### Internet Key Exchange (IKE)

**Internet Key Exchange (IKE)** is a protocol that ensures authentication between two endpoints by establishing IKE tunnels called security associations (SAs), that carries control and data plane traffic for IPsec

IKEv1 (ISAKMP)

Internet Security Association Key Management uses Oakley Protocol key exchange technique that provides PFS for keys, identity protection, and authentication

Skeme technique provides anonymity and quick key refreshment

Perfect Forward Secrecy (PFS) is a function for phase 2, where another DH exchanges are performed requiring additional CPU cycles, but creates greater resistance to crypto attacks

IKE builds the tunnels for us but it doesn’t authenticate or encrypt user data. We use two other protocols for this:

#### Authentication Header (AH) (not recommended)

AH offers authentication and integrity but it doesn’t offer any encryption. It protects the IP packet by calculating a hash value over almost all fields in the IP header. The fields it excludes are the ones that can be changed in transit (TTL and header checksum)

Does not support encryption and NAT-T; Uses protocol number 51 in the IP header creates a digital signature similar to a checksum to ensure that the packet has not been modified

#### Encapsulating Security Payload (ESP)

**Encapsulating Security Payload (ESP)** ensures that the original payload (before encapsulation) maintains data integrity and confidentiality by encrypting the payload and adding a new set of headers during transport across a public network. ESP supports NAT-T and is identified by the IP protocol number 50 i nthe IP header

Note it’s possible to use AH and ESP at the same time

### IPsec modes

**Tunnel mode** is the default mode, where the entire original IP packet is encrypted and a new IP header is added

This allows to create the overlay in order to establish VPN between sites and allow communication of multiple devices behind the IPsec gateway

**Transport mode** encrypts and authenticates only the packet payload

It routes based on the original IP headers, which saves 20bytes for the header in the contrary of Tunnel mode. So the transport mode is only used for communication of a single device that is generating and receiving the encrypted traffic 1to1 or host to host

Routers however automatically switch the mode to tunnel when a host behind it tries to send data that are matched to be encrypted by the ACL

![](<../../.gitbook/assets/Unknown image (955)>)

![](<../../.gitbook/assets/Unknown image (956)>)

### IKE phase 1

In IKE phase 1, two peers will negotiate about the encryption, authentication, hashing and other protocols that they want to use and some other parameters that are required. In this phase, an ISAKMP (Internet Security Association and Key Management Protocol) session is established. This is also called the ISAKMP tunnel or IKE phase 1 tunnel.

The collection of parameters that the two devices will use is called a **SA (Security Association)**

The IKE phase 1 tunnel is only used for management traffic. We use this tunnel as a secure method to establish the second tunnel called the IKE phase 2 tunnel or IPsec tunnel and for management traffic like keepalives. Once IKE phase 2 is completed, we have an IKE phase 2 tunnel (or IPsec tunnel) that we can use to protect our user data

Step 1 Negotiation

The peer that has traffic that should be protected will initiate the IKE phase 1 negotiation. The two peers will negotiate about the following items:

1. Authentication to authenticate peers

RSA signatures are cryptographic system used for mutual authentication between peers with public-keys.

It uses digital certificates to authenticate the identity of each peer

Pre-Shared Key locally configured key used as a credential for mutual authentication between peers

1. Hashing to verify the integrity

Message Digest 5 (MD5) A one-way, 128-bit hash algorithm used for data authentication. Weak

Secure Hash Algorithm (SHA) A one-way, 160-bit hash algorithm used for data authentication

SHA-1 HMAC (Hash Message Authentication Code) is Cisco version, providing protection against MitM attacks

1. Diffie-Hellman (DH) generates shared secret symmetric keys used by two VPN peers for symmetrical algorithms, such as AES

The DH exchange itself is asymmetrical and CPU intensive, and the resulting shared secret keys that are generated are symmetrical

DH group refers to the length e.g strength of the key (modulus size) used for DH key exchange

1. Encryption to encrypt the packets

Data Encryption Standard (DES) A 56-bit symmetric data encryption algorithm, however weak

Triple DES (3DES) runs the DES algorithm three times with three different 56-bit keys. Weak

Advanced Encryption Standard (AES) symmetric encryption based on the Rijndael algorithm. Supporting key lengths of 128 bits, 192 bits, or 256 bits

1. Lifetime how long does the IKE phase 1 tunnel stand up? the shorter the lifetime, the more secure it is because rebuilding it means we will also use new keying material. Each vendor uses a different lifetime, a common default value is 86400 seconds (1 day). Only parameter that does not have to match on both peers

Step 2: DH Key Exchange

Once the negotiation has succeeded, the two peers will know what policy to use. They will now use the DH group that they negotiated to exchange keying material. The end result will be that both peers will have a shared key.

Step 3: Authentication

The last step is that the two peers will authenticate each other using the authentication method that they agreed upon on in the negotiation. When the authentication is successful, we have completed IKE phase 1. The end result is a IKE phase 1 tunnel (aka ISAKMP tunnel) which is bidirectional. This means that both peers can send and receive on this tunnel.

The three steps above can be completed using two different modes:

#### Main mode (MM)

more secure with longer negotiation

MM1: first message that the Initiator sends to a Responder. One or multiple SA proposals are offered and the responder needs to match one of the them for this phase to succeed

The SA proposals include different transform sets

MM2: sent from the responder to the initiator with the SA proposal that it matched

MM3: initiator starts the DH key exchange as agreed in previous steps

MM4: responder sends its own key to the initiator > established encryption for ISAKMP SA

MM5: The initiator starts authentication by sending the peer router its IP address

MM6: The responder sends back a similar packet and authenticates the session > ISAKMP SA established fully

**SPI (Security Parameter Index)** this is an 32-bit identifier so the receiver knows to which flow this packet belongs.

#### Aggressive mode (AM)

only requires three messages to establish the security association. It’s quicker than main mode since it adds all the information required for the DH exchange in the first two messages. Main mode is considered more secure since identification is encrypted, aggressive mode does this in clear-text.

AM1: initiator sends all the information contained in MM1 through MM3 and MM5 at once

AM2: send information contained in MM2, MM4, and MM6.

AM3 The initiator starts authentication by sending the peer router its IP address

### IKE phase 2

After a secure tunnel is established in Phase 1, the next step in setting up the VPN is to negotiate the IPSEc security parameters that will be used to protect the actual data and messages within the tunnel. Rather than negotiate each encryption and authentication protocol individually, the protocols are grouped into Transform Set

Phase 2 establishes separate unidirectional IPsec SAs for securing traffic in each direction (in and out) separately within the IPsec tunnel

There is only one mode to build the IKE phase 2 tunnel which is called quick mode

Just like in IKE phase 1, our peers will negotiate about a number of items:

IPsec Protocol: do we use AH or ESP?

Encapsulation Mode: transport or tunnel mode?

Encryption: what encryption algorithm do we use? DES, 3DES or AES?

Authentication: what authentication algorithm do we use? MD5 or SHA?

Lifetime: how long is the IKE phase 2 tunnel valid? When the tunnel is about to expire, we will refresh the keying material.

(Optional) DH exchange: used for PFS (Perfect Forward Secrecy).

The IPsec Phase 2 can be configured either using crypto map or IPsec profiles, which is new more simplified and organized configuration

#### Quick mode

QM1: The initiator (either peer) can start multiple IPsec SAs in a single exchange message. Includes phase 1 agreed-upon algorithms, as well as what traffic is to be encrypted or secured

QM2: This message from the responder has matching IPsec parameters

QM3: unidirectional IPsec SAs between peers established

QM\_IDLE indicates that SA remains authenticated and may be used for subsequent QM exchange for additional IPsec SAs

![R1 R2 data through IPsec tunnel](<../../.gitbook/assets/Unknown image (957)>)

### IPsec VTIs (Virtual Tunnel Interface)

In the old method, an extended ACL must be defined to match which traffic will be encrypted

Since we use GRE as the encapsulation protocol for all IP packet, then we used an ACL to match the GRE packet sourced from 1.1.1.1 destined to 2.2.2.2, because all traffic that goes through the tunnel will encapsulated with the Public IP header defined in the tunnel source and tunnel destination command under the tunnel interface.

Then after setting this ACL, we need the popular crypto map for phase 2 IPsec, under the crypto map, we had to assign the ACL to set address 100 command and set peer 2.2.2.2 command, and the transform set using the set transform-set command, finally we apply the crypto map on the physical interface.

Now, why moving to Tunnel Protection or IPsec Profile?, simply because when we use IPsec with GRE (GRE over IPsec), there are many duplicate configuration:

1. The set peer 2.2.2.2 command under the crypto map has the same meaning as the tunnel destination 2.2.2.2 command under the tunnel interface.
2. The second duplication is for the ACL, previously using the old method crypto ACL, we need to identify the GRE packet and associate this ACL to crypto map using the match address 100, the ACL + match address 100 have the same meaning as the Tunnel source 1.1.1.1 and Tunnel destination 2.2.2.2 commands

With IPsec Profile, you associate the transform-net then you apply the IPsec Profile on the Tunnel interface and thats all

There are is no need of ACL or peer statement, all these information's are already there in the tunnel interface

| GRE over IPSec Crypto map access-list 100 permit gre host 1.1.1.1 host 2.2.2.2 ! crypto ipsec transform-set TS esp-aes esp-sha-hmac ! crypto map TEST set peer 2.2.2.2 match address 100 set transform-set TS ! int G0/0 ip address 1.1.1.1 255.255.255.0 crypto map TEST ! int tunnel 1 ip add 172.16.1.1 255.255.255.0 tunnel source G0/0 tunnel destination 2.2.2.2 | Tunnel Protection (IPsec Profile) crypto ipsec transform-set TS esp-aes esp-sha-hmac ! crypto ipsec profile TEST set transform-set TS ! int tunnel 1 ip add 172.16.1.1 255.255.255.0 tunnel source G0/0 tunnel destination 2.2.2.2 tunnel protection ipsec TEST |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IKEv1 Site-to-site with IPsec profile /w PSK                                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                 |
| 1. ISAKMP policy for IKE SA crypto isakmp policy                                                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                                                                 |
| encryption {des \| 3des \| aes \| aes 192 \| aes 256}                                                                                                                                                                                                                                                                                                                  |                                                                                                                                                                                                                                                                 |
| hash {sha \| sha256 \| sha384 \| md5}                                                                                                                                                                                                                                                                                                                                  |                                                                                                                                                                                                                                                                 |
| authentication {rsa-sig \| rsa-encr \| pre-share}                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                                                                                                 |
| group {1 \| 2 \| 5 \| 14 \| 15 \| 16 \| 19 \| 20 \| 24}                                                                                                                                                                                                                                                                                                                |                                                                                                                                                                                                                                                                 |
| lifetime                                                                                                                                                                                                                                                                                                                                                               |                                                                                                                                                                                                                                                                 |
| 2. Specifies PSK with Peer IP in global crypto keyring \[NAME] pre-shared-key address key                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                 |
| 3. Configure the ISAKMP Profile match identity address keyring                                                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                                 |
| 4. Configure Phase 2 in global crypto ipsec transform-set                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                 |
| mode \[tunnel \| transport]                                                                                                                                                                                                                                                                                                                                            | tunnel is recommended                                                                                                                                                                                                                                           |
| 5. Define IPsec profile crypto ipsec profile                                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                 |
| set transform-set                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                                                                                                 |
| set pfs {group15 \| group16 \| group19 \| group20 \| group24}                                                                                                                                                                                                                                                                                                          |                                                                                                                                                                                                                                                                 |
| set security-association lifetime seconds                                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                 |
| set isakmp-profile                                                                                                                                                                                                                                                                                                                                                     |                                                                                                                                                                                                                                                                 |
| 6. Configure VTI interface and apply profile to it - same as IKEv2                                                                                                                                                                                                                                                                                                     |                                                                                                                                                                                                                                                                 |

### IKEv2

require only 4 messages and less bandwidth to establish an IPsec SA; supports EAP authentication; IKEv2 has built-in support for NAT traversal

IKEv2 doesn’t have a main or aggressive mode for phase 1 and there’s no quick mode in phase 2.

It still has two phases though, phase 1 is called the IKE\_SA\_INIT and the second phase is called IKE\_AUTH. Only four messages are required for the entire exchange

CREATE\_CHILD\_SA exchange is for any additional IPsec SAs

Additional features in compare to v1:

Elliptic Curve Digital Signature Algorithm (ECDSA-SIG) newer and more efficient alternative to public keys

Next generation encryption (NGE) meets the security and scalability requirements for the next two decades. Asymmetric authentication (peers can use different authentication methods)

Anti-DoS IKEv2 detects whether an IPsec router is under attack and prevents resources consumption

#### IKEv2 site-to-site with IPsec profile (PSK)

**Phase 1 configuration**

| 1. IKEv2 Proposal crypto ikev2 proposal \[NAME1] encryption aes-cbc-256 integrity <.. \| sha512>group 14                                                                                                                   | recommended only AES the higher the better 14:2048 bit                                                                                                                                                                               |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 2. IKEv2 Policy crypto ikev2 policy \[NAME2] match fvrf \[WAN-vrf-name] match address local \[local\_WANIP] proposal \[NAME1]                                                                                              | >>>> used in VRF-Aware (F-VRF) deployment                                                                                                                                                                                            |
| 3. IKEv2 Keyring crypto ikev2 keyring \[NAME3] peer \[NAME4] address pre-shared-key                                                                                                                                        |                                                                                                                                                                                                                                      |
| 4. IKEv2 Profile crypto ikev2 profile \[NAME5] match fvrf \[WAN-vrf-name] match identity remote address authentication remote pre-share authentication local pre-share keyring local \[NAME3] dpd \[on-demand \| periodic] | >>>> used in VRF-Aware (F-VRF) deployment Dead Peer Detection (DPD) is used to check the availability of the IKEv2 peer. This helps to ensure that the VPN tunnel is still active and can detect if the peer is no longer responding |

**Phase 2 configuration**

| 5. IPSEC Transform set crypto ipsec transform-set mode \[tunnel \| transport]                                                                                                                                                       | tunnel is recommended                                                                                                                                                                                                                                                                                                  |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 6. IPSEC Profile crypto ipsec profile \[NAME7] set transform-set \[NAME6] set ikev2-profile \[NAME5] set pfs {group15 \| group16 \| group19 \| group20 \| group24} set security-association lifetime \[days\|seconds\|kilobytes> <> | optional optional                                                                                                                                                                                                                                                                                                      |
| 7. Configure VTI interface and apply profile to it interface Tunnel1 tunnel mode ipsec \[ipv4\|ipv6] tunnel protection profile ipsec \[NAME5] tunnel source tunnel destination ip address \<tunnel\_ip>                             |                                                                                                                                                                                                                                                                                                                        |
| Optional MTU adjustment                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                        |
| ip mtu 1400 ip tcp adjust-mss 1360                                                                                                                                                                                                  | Optional MTU adjustments configures Pre-fragmentation under tunnel intf                                                                                                                                                                                                                                                |
| (config)# crypto ikev2 fragmentation mtu 1400                                                                                                                                                                                       | Configures the IKEv2 fragmentation The MTU size refers to the IP or UDP encapsulated IKEv2 packets                                                                                                                                                                                                                     |
| (config)# crypto ipsec fragmentation after-encryption                                                                                                                                                                               | (Optional) enables fragmentation of packets after encryption, which ensures that the entire packet, including the IPsec headers, are considered when determining whether to fragment the packet. This is important because if fragmentation is performed before encryption, the IPsec headers will be left unencrypted |

### Verification

| show crypto isakmp sa                    | Showing IPsec/ISAKMP (IKEv1) Phase 1 security associations                     |
| ---------------------------------------- | ------------------------------------------------------------------------------ |
| show crypto ipsec sa \[detail]           | Showing IPsec Phase 2 security associations and packets that were de/encrypted |
| show crypto ikev2 \[sa\|stats]           | Showing IPsec/IKEv2 Phase 1 security associations                              |
| show crypto session \[detail \| brief]   | General check of the peers                                                     |
| show crypto engine accelerator statistic |                                                                                |
| clear crypto ikev2 sa remote             |                                                                                |
| clear crypto \[session \| remote ]       | to reset associations                                                          |
| Debug crypto ikev2 error                 |                                                                                |
| Debug crypto ikev2 internal              |                                                                                |
| Debug crypto ikev2 packet                |                                                                                |

![](<../../.gitbook/assets/Unknown image (958)>)
