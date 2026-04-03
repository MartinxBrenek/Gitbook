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

# Security Threats

Security threats span many layers. This page groups common threats and typical mitigations.

### Terminology

* **Threat**: Any circumstance or event with the potential to cause harm to an asset in the form of destruction, disclosure, adverse modification of data, or denial of service (DoS). An example of a threat is malicious software that targets workstations.
* **Vulnerability**: A weakness that compromises either the security or the functionality of a system. Weak or easily guessed passwords are considered vulnerabilities.
* **Exploit**: A mechanism that uses a vulnerability to compromise the security or functionality of a system. An example of an exploit is malicious code that gains internal access. When a vulnerability is disclosed to the public, attackers often create a tool that implements an exploit for the vulnerability. If they release this tool or proof of concept code to the internet, other less-skilled attackers and hackers (the so-called script kiddies) can then easily exploit the vulnerability.
* **Risk**: The likelihood that a particular threat using a specific attack will exploit a particular vulnerability of an asset that results in an undesirable consequence.
* **Mitigation techniques**: Methods and corrective actions to protect against threats and different exploits, such as implementing updates and patches, to reduce the possible impact and reduce risks.

### Hacking tools

The distinction between a security tool and a hacking (or attack) tool is in the intent of the user. A penetration tester legitimately uses tools to penetrate an organization's security defenses. The organization uses the results of the penetration test to improve its security defenses. However, the same tools that the penetration tester uses can be used illegitimately by an attacker.

Innumerable quantities of hacking (or security) tools can be found on the internet. The following list provides a few examples. More important than the details of any single example are the understanding of how easy it is now to obtain and use very powerful attack tools.

#### sectools.org

A website run by the Nmap Project, which regularly polls the network security community regarding their favorite security tools. It lists the top security tools in order of popularity. A short description is provided for each tool, along with user reviews and links to the publisher's website. There are password auditors, sniffers, vulnerability scanners, packet crafters, and exploitation tools, among the many categories. The site provides information disclosure. Security professionals should review the list and read the descriptions of the tools. Network attackers certainly will.

#### Metasploit

When Metasploit was first introduced, it had a big impact on the network security industry. It was a very potent addition to the penetration tester's toolbox. While it provided a framework for advanced security engineers to develop and test exploit code, it also lowered the threshold for the experience required for a novice attacker to perform sophisticated attacks. The framework separates the exploit (code that uses a system vulnerability) from the payload (code injected to the compromised system). The framework is distributed with hundreds of exploit modules and dozens of payload modules. To launch an attack with Metasploit, you must first select and configure an exploit. Each exploits targets a vulnerability of an unpatched operating system or application server. The use of a vulnerability scanner can help determine the most appropriate exploits to attempt. The exploit must be configured with relevant information such as the target IP address. Next, you must select a payload. The payload might be remote shell access, Virtual Network Computing (VNC) access, or remote file downloads. You can add exploits incrementally. Metasploit exploits are often published with or shortly after the public disclosure of vulnerabilities.

### Malicious software (malware)

**Malicious software (malware)** is a broad term encompassing various types of harmful software, including viruses, worms, trojans, and spyware. It is designed to damage, disrupt, or gain unauthorized access to systems or data.

#### Viruses

Viruses are programs that attach themselves to legitimate files and spread by infecting other files or programs. They can cause data corruption, spread to other systems, and execute malicious actions.

#### Trojan horses

A Trojan horse is named after the wooden horse the Greeks used to infiltrate Troy. It is a harmful piece of software that looks legitimate. Users are typically tricked into loading and executing it on their systems. After it is activated, it can achieve any number of attacks on the host, from irritating the user (popping up windows or changing desktops) to damaging the host (deleting files, stealing data, or activating and spreading other malware, such as viruses). Trojans are also known to create back doors to give malicious users access to the system. Unlike viruses and worms, Trojans do not reproduce by infecting other files, nor do they self-replicate. Trojans must spread through user interaction, such as opening an email attachment or downloading and running a file from the Internet.

#### Spyware

Spyware refers to malicious software that is designed to secretly gather information from a computer or device without the user's knowledge or consent. It can monitor a user's online activities, capture sensitive information like passwords and credit card details, and transmit this data to a remote attacker.

#### Worms

Worms are similar to viruses in that they replicate functional copies of themselves and can cause the same type of damage. In contrast to viruses, which require spreading an infected host file, worms are standalone software and use self-propagation; they don't require a host program or human help to propagate. To spread independently, worms often make use of common attack and exploit techniques. A worm enters a computer through a vulnerability in the system and takes advantage of file-transport or information-transport features on the system, allowing it to travel unaided.

#### Ransomware

Ransomware encrypts a user's data and demands a ransom payment in exchange for the decryption key.

More recently, with the rise of cryptocurrencies, ransomware has risen in prominence. Ransomware is an attack that encrypts the data on your local drives and the shared network drives, making it unusable without a key. The ransomware then presents means of buying the encryption key from the attackers, usually utilizing cryptocurrencies. An example of a ransomware attack is the WannaCry cryptoworm, which was introduced in May 2017.

#### Rootkits and bootkits

Rootkits are designed to gain privileged access to a system and hide their presence. Once activated, the malicious program sets up a backdoor exploit and may deliver additional malware. Bootkits take this a step further by infecting the master boot prior to the operating system booting up, making them harder to detect.

#### Backdoors

Backdoors are hidden entry points into a system that bypass normal authentication mechanisms. Attackers can use them to access systems without being detected.

#### Advanced persistent threats (APTs)

Malware is commonly utilized by advanced persistent threats (APTs), a set of continuous hacking processes targeting a specific entity, often with a specific goal. Some characteristics of APTs are obvious from the name. They are advanced; the attackers have the most advanced intelligence systems and techniques at their disposal and will use what is optimal for each step. They may utilize commonly available security tools when they are sufficient, but they may also discover and exploit zero-day (unpublished) vulnerabilities when necessary. They are also persistent. The attackers focus on their goal. They do not cash in on short-term opportunities. Instead, they maintain discreet access, slowly but surely infiltrating deeper into systems until their objectives can be met.

The structure of an APT attack does not follow a blueprint. As with any network attack, the scenario varies with circumstance. However, a common methodology is as follows:

1. Initial compromise
2. Escalation of privileges
3. Internal reconnaissance
4. Lateral propagation, compromising other systems on track towards its goal
5. Mission completion

### Threats by layer

#### Local access and physical threats (Layer 1)

Description: Attackers gain physical access to devices like switches, routers, or server rooms to compromise the network.

Remediation:

Secure server rooms with physical locks, biometric access, or security guards.

Use cable locks or secure rack enclosures.

**Environmental threats**: Extreme temperature (heat or cold) or humidity extremes (too wet or too dry) can present a threat. Mitigation techniques for this type of threat include creating the proper operating environment through temperature control, humidity control, positive air flow, remote environmental alarms, and recording and monitoring.

**Electrical threats**: Voltage spikes, insufficient supply voltage (brownouts), unconditioned power (noise), and total power loss are potential electrical threats. Mitigation techniques for this type of threat include limiting potential electrical supply problems by installing uninterruptible power supply (UPS) systems and generator sets, following a preventative maintenance plan, installing redundant power supplies, and using remote alarms and monitoring.

**Maintenance threats**: These threats include improper handling of important electronic components, lack of critical spare parts, poor cabling, and inadequate labeling. Mitigation techniques for this type of threat include using neat cable runs, labeling critical cables and components, stocking critical spares, and controlling access to console ports.

#### Layer 2 threats

**MAC flooding attack**

Description: Attackers overload the MAC address table on switches with fake entries, causing the switch to broadcast all traffic and enabling eavesdropping.

Remediation:

Enable Port Security to limit MAC addresses per port.

Monitor for unusual MAC address activity.

**IP/MAC spoofing & ARP cache poisoning (Man-in-the-Middle)**

An attack is considered a spoofing attack when an attacker injects traffic that appears to be sourced from a system other than the attacker's system itself. Spoofing is not specifically an attack, but spoofing can be incorporated into various types of attacks. Unlike other attack types, most spoofing can be easily prevented by well-known mitigation techniques.

**IP address spoofing**: IP address spoofing is the most common type of spoofing. To perform IP address spoofing, attackers inject a source IP address in the IP header of packets different from their real IP addresses.

**MAC address spoofing**: To perform MAC address spoofing, attackers use MAC addresses that are not their own. MAC address spoofing is generally used to exploit weaknesses at Layer 2 of the network.

**Application or service spoofing**: An example is DHCP spoofing for IPv4, which can be done with either the DHCP server or the DHCP client. To perform DHCP server spoofing, the attacker enables a rogue DHCP server on a network. When a victim host requests a DHCP configuration, the rogue DHCP server responds before the authentic DHCP server. The victim is assigned an attacker-defined IPv4 configuration. An attacker can spoof many DHCP client requests from the client-side, specifying a unique MAC address per request. This process may exhaust the DHCP server's IPv4 address pool, leading to a DoS against valid DHCP client requests. Another simple example of spoofing at the application layer is an email from an attacker that appears to have been sourced from a trusted email account.

In normal ARP operation, a host sends a broadcast to determine the MAC address of a destination host with a particular IPv4 address. The device with the IPv4 address replies with its MAC address. The originating host caches the ARP response, using it to populate the destination MAC address in frames that encapsulate packets sent to that IPv4 address. By spoofing an ARP reply from a legitimate device with a malicious ARP reply, an attacking device appears to be the destination host that is sought by the sender. The ARP message from the attacker causes the sender to store the MAC address of the attacking system in its ARP cache. All packets that are destined for that IPv4 address are forwarded to the attacker system.

![](<../.gitbook/assets/Unknown image (788)>)

Remediation:

Use Dynamic ARP Inspection (DAI) and IP Source Guard.

Encrypt sensitive communication with SSL/TLS or VPNs.

**Spanning Tree Protocol (STP) attacks**

Description: Malicious BPDU messages manipulate STP, disrupting network topology and potentially causing outages.

Remediation:

Enable BPDU Guard, Root Guard, and Loop Guard on switches.

![](<../.gitbook/assets/Unknown image (789)>)

**Replay attacks**

Description: Attackers capture and retransmit legitimate traffic to impersonate a user.

Remediation:

Use time-sensitive authentication tokens.

Employ encrypted protocols like SSL/TLS to protect sessions.

**VLAN-based attacks**

**VLAN hopping attack**

VLAN hopping allows traffic from one VLAN to be seen by another VLAN without first crossing a router. Under certain circumstances, attackers can sniff data and extract passwords and other sensitive information at will.

The attack works by taking advantage of an incorrectly configured trunk port. By default, trunk ports have access to all VLANs and pass traffic for multiple VLANs across the same physical link, generally between switches.

In a basic VLAN hopping attack, the attacker takes advantage of the fact that DTP is enabled by default on most switches. The network attacker configures a system to use DTP to negotiate a trunk link to the switch. As a result, the attacker is a member of all the VLANs that are trunked on the switch and can “hop” between VLANs. In other words, the attacker can send and receive traffic on all those VLANs.

The best way to prevent a basic VLAN hopping attack is to turn off DTP on all ports, and explicitly configure trunking mode or access mode as appropriate on each port.

**Double-tagging VLAN hopping attack**

The double-tagging (or double-encapsulated) VLAN hopping attack takes advantage of the way that hardware operates on some switches. Some switches perform only one level of 802.1Q decapsulation and allow an attacker, in specific situations, to embed a second 802.1Q tag inside the frame. This tag allows the frame to go to a VLAN that the outer 802.1Q tag did not specify. An important characteristic of the double-encapsulated VLAN hopping attack is that it can work even if DTP is disabled on the attacker’s access port.

Works only when the attacker’s VLAN and trunk port native VLAN are the same. Stopping this type of attack is not as easy as stopping basic VLAN hopping attacks. The best approach is to create a VLAN to use as the native VLAN on all trunk ports and explicitly do not use that VLAN for any access ports.

Specify a unique native VLAN for use on all trunk ports and do not use that VLAN anywhere else on the switch.

| switchport trunk native vlan vlan\_number | interface configuration command to set the native VLAN on the trunk to an unused VLAN. The default native VLAN is VLAN 1. |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |

The attacker sends a double-tagged 802.1Q frame to the switch. The outer header has the VLAN tag of the attacker, which is the same as the native VLAN of the trunk port. For the purposes of this example, assume that it is VLAN 10. The inner tag is the victim's VLAN, VLAN 20.

The frame arrives on the switch. Assume that there is no MAC address table entry for the destination MAC address. Therefore, the switch sends the frame out to all VLAN 10 ports (including the trunk). On egress out of the trunk port, the switch sees that the frame has an 802.1Q tag for the native VLAN, and it strips that tag (because 802.1Q specifies that native VLAN traffic is not tagged). The inner tag to VLAN 20 remains.

The frame arrives at the second switch, which has no knowledge that it was supposed to be for VLAN 10. The second switch looks only at the 802.1Q tag (the former inner tag that the attacker sent) and sees that the frame is destined for VLAN 20 (the victim VLAN).

The second switch sends the packet on to the victim port, or floods it, depending on whether there is an existing MAC address table entry for the victim host.

![](<../.gitbook/assets/Unknown image (790)>)

#### Layer 3 threats (Network Layer)

**Routing protocol and FHRP attacks**

Description: Attacks that exploit routing protocols (e.g., OSPF, BGP) or first-hop redundancy protocols (e.g., HSRP, VRRP) to intercept or manipulate traffic.

Remediation:

Use secure versions of protocols with authentication (e.g., MD5).

Implement route filtering and monitor routing tables.

**Smurf attack**

Description: The attacker sends a large number of ICMP echo request packets (pings) with a spoofed source IP address (the victim's IP address) to the directed broadcast address of one or more networks. The routers then forward these packets to all hosts on those networks. Each host on the network responds to the spoofed source IP address (the victim), overwhelming the victim's system with a flood of ICMP replies.

Remediation:

Disable IP-directed broadcasts on routers.

| (configi-if)# no ip directed-broadcast |   |
| -------------------------------------- | - |

Filter ICMP traffic with firewalls or intrusion prevention systems (IPS).

**DHCP starvation attack**

A DHCP starvation attack works by sending a flood of DHCP requests with spoofed MAC addresses. If enough requests are sent, the network attacker can exhaust the address space available on the DHCP servers. This flooding would cause a loss of network availability to new DHCP clients as they connect to the network. A DHCP starvation attack may be executed before a DHCP spoofing attack. If the legitimate DHCP server’s resources are exhausted, then the rogue DHCP server on the attacker system has no competition when it responds to new DHCP requests from clients on the network.

To mitigate DHCP address starvation attacks, deploy port security address limits, which set an upper limit of secure MAC addresses that can be accepted into the MAC address table from any single port. Because each DHCP request must be sourced from a separate MAC address, this mitigation technique effectively limits the number of IP addresses that can be requested from a switch-port-connected attacker. Set this parameter to a value that is never legitimately exceeded in your environment.

Use DHCP snooping feature

#### Cross-layer threats

**Remote access threats**

Unauthorized remote access is a threat when security is weak in remote access configuration. Mitigation techniques for this type of threat include configuring strong authentication and encryption for remote access policy and rules, configuration of login banners, use of ACLs, and virtual private network (VPN) access.

**Denial-of-Service (DoS) and Distributed DoS (DDoS) attacks**

DoS attacks attempt to consume all critical computer or network resources to make them unavailable for proper use. DoS attacks are considered a major risk because they can easily disrupt the operations of a business, and they are relatively simple to conduct. A TCP synchronization (SYN) flood attack is a classic example of a DoS attack. The TCP SYN flood attack exploits the TCP three-way handshake design by sending multiple TCP SYN packets with random source addresses to a victim host. The victim host sends a synchronization-acknowledgment (SYN-ACK) back to the random source address and adds an entry to the connection table. Because the SYN-ACK is destined for an incorrect or nonexistent host, the last part of the three-way handshake is never completed, and the entry remains in the connection table until a timer expires. By generating TCP SYN packets from random IP addresses rapidly, the attacker can fill up the connection table and deny TCP services (such as email, file transfer, or World Wide Web) to legitimate users. There is no easy way to trace the originator of the attack because the IP address of the source is forged or spoofed. An attacker creates packets with random IP source addresses with IP spoofing to obfuscate the actual originator.

Some DoS attacks, such as the Ping of Death, can cause a service, system, or group of systems to crash. In Ping of Death attacks, the attacker creates a packet fragment, specifying a fragment offset indicating a full packet size of more than 65,535 bytes. 65,535 bytes is the maximum packet size as defined by the IP protocol. A vulnerable machine that receives this type of fragment will attempt to set up buffers to accommodate the packet reassembly, and the out-of-bounds request causes the system to crash or reboot. The Ping of Death exploits vulnerabilities in processing at the IP layer, but similar attacks exploit vulnerabilities at the application layer. Attackers have also exploited vulnerabilities by sending malformed Simple Network Management Protocol (SNMP), system logging (syslog), Domain Name System, or other UDP-based protocol messages. These malformed messages can cause various parsing and processing functions to fail, resulting in a system crash and a reload in most circumstances. The IPv6 Ping of Death—the IPv6 version of the original Ping of Death—was also created.

Because most computer systems have been patched and today's modern firewalls provide protection against protocol anomaly, Ping of Death and other basic protocol attacks are not an issue for most systems.

When a DoS attempt derives from a single host of the network, it constitutes a DoS attack. Malicious hosts can also coordinate to flood a victim with an abundance of attack packets, so that the attack takes place simultaneously from potentially thousands of sources. This type of attack is called a distributed denial of service (DDoS) attack. DDoS attacks typically emanate from networks of compromised systems that are known as botnets. In many cases, users and administrators are not even aware that their system is part of a botnet.

A botnet consists of a group of "zombie" programs known as robots or bots and a main control mechanism that provides direction and control for the zombies. The originator of a botnet uses the main control mechanism on a command-and-control server to control the zombie computers remotely, often by using Internet Relay Chat (IRC) networks.

A botnet typically operates in this manner:

A botnet operator infects computers by infecting them with malicious code, which runs the malicious bot process. A malicious bot is a self-propagating malware designed to infect a host and connect back to the command-and-control server. In addition to its worm-like ability to self-propagate, a bot can include the ability to log keystrokes, gather passwords, capture and analyze packets, gather financial information, launch DoS attacks, relay spam, and open back doors on the infected host. Bots have all the advantages of worms but are generally much more versatile in their infection vector and are often modified within hours of publication of a new exploit. They have been known to exploit back doors opened by worms and viruses, which allows them to access networks with good perimeter control. Bots rarely announce their presence with visible actions such as high scan rates, which negatively affect the network infrastructure; instead, they infect networks in a way that escapes immediate notice.

The bot on the newly infected host logs into the command-and-control server and awaits commands. Often, the command-and-control server is an IRC channel or a web server.

Instructions are sent from the command-and-control server to each bot in the botnet to execute actions. When the bots receive the instructions, they begin generating malicious traffic that is aimed at the victim. Some bots also can be updated to introduce new functionalities to the bot..

Remediation:

Use firewalls, IDS/IPS, and rate-limiting.

Deploy anti-DDoS hardware/software solutions and load balancers.

![](<../.gitbook/assets/Unknown image (791)>)

**Other DoS vectors and evasions**

**UDP flood attack** is triggered by sending many UDP packets to the target system.

**SIP INVITE flood attacks** target Session Initiation Protocol (SIP) servers by sending a large number of INVITE requests, attempting to exhaust the server's resources. This is an application layer attack and not related to IP directed broadcasts.

**Teardrop attack** exploits vulnerabilities in the fragmentation and reassembly of IP packets. While router security features might help, `no ip directed-broadcast` specifically targets broadcast forwarding.

**Fragmentation attacks**: Attackers often use fragmented packets to bypass ACLs by hiding malicious payloads in noninitial fragments (those with fragment offset > 0).

Remediation:

When an IP packet is too large to be transmitted over a network segment (exceeding the MTU), it's split into smaller pieces (fragments). Each fragment has:

The same IP header (mostly)

A fragment offset field indicating its position in the original packet

**Two fragment types:**

**Initial fragment**

Fragment Offset = 0

Contains Layer 4 headers (TCP/UDP/ICMP, ports, etc.)

ACLs can fully inspect it

**Noninitial fragments**

"Noninitial" in the context of IP fragmentation refers to any fragment of a packet that is not the first one — in other words, fragments with a fragment offset > 0.

Fragment Offset > 0

Do NOT contain Layer 4 headers

Only contain part of the payload

Harder for ACLs to inspect (can’t match TCP/UDP ports)

Why This Matters for Security:

Noninitial fragments can be used by attackers to:

Bypass ACLs that rely on TCP/UDP port checks

Flood the device with hard-to-identify traffic

So, filtering or dropping noninitial fragments is a way to block this evasive technique.

Example:

If an attacker sends a 4000-byte packet to port 22 (SSH):

Fragment 1: Offset = 0 (includes TCP header with port 22)

Fragment 2: Offset = 1480 (no TCP header)

A simple ACL checking only for TCP/22 won’t catch fragment 2 — unless you account for noninitial fragments.

access-list 101 deny tcp any your\_public\_infrastructure\_address\_block fragments

access-list 101 deny udp any your\_public\_infrastructure\_address\_block fragments

access-list 101 deny icmp any your\_public\_infrastructure\_address\_block fragments

**DNS spoofing / cache poisoning**

Description: Attacker manipulates DNS responses to redirect users to malicious websites.

Remediation:

Enable DNSSEC to validate DNS records.

Implement proper DNS caching practices and monitor DNS traffic.

**BYOD (Bring Your Own Device)**

Description: Personal devices connecting to corporate networks may introduce vulnerabilities or be used as attack vectors.

Remediation:

Use Mobile Device Management (MDM) and endpoint security solutions.

Enforce VLAN segmentation and ensure device compliance before granting access.

**Man-in-the-middle (MitM)**

Description: Capturing and potentially modifying communication between two parties without their knowledge.

Remediation:

Employ end-to-end encryption like TLS/SSL.

Use secure protocols such as WPA3 for wireless communications

**Privilege escalation**

Description: Exploiting vulnerabilities or misconfigurations to gain higher access rights than intended.

Remediation:

Enforce Principle of Least Privilege (PoLP).

Use Role-Based Access Control (RBAC) and patch vulnerabilities regularly

**Website scams**

The whois\_bot is a very valuable tool, but in this case the scammers are using subdomains.

Using a subdomain of a web hosting provider should be an extremely big red flag on its own. (likewww.something.web.app.com)

A recent registration date would be an extremely large red flag.

Using a subdomain of a hosting provider is an even bigger red flag

### Hardening and security mechanisms

#### Digital certificates

**Digital certificates** bind together an entity name and its public key, signed by a certificate authority (CA). They let clients verify the server is who it claims to be (for example, when a browser validates a website certificate).

#### Biometrics

**Biometrics** use physical traits for authentication. Examples include fingerprint, iris, voice, and face recognition. Many devices combine biometrics with passwords for stronger assurance.

#### Multi-factor authentication (MFA)

**Multi-factor authentication (MFA)** requires at least one extra step beyond username and password. The second factor can be a push notification, a hardware/software token, or SMS. One-Time Passwords (OTP) are a common MFA mechanism.

### Device hardening

**Device hardening** refers to securing and configuring a network device (router, switch, firewall) to minimize risk and improve resilience against attacks.

This involves:

Regularly updating the device’s firmware and software to patch known vulnerabilities

Turning off any services or interfaces that are not in use to reduce potential entry points for attackers

Another security practice is to create a VLAN dedicated for unused ports. Do not create a switch virtual interface (SVI) for that VLAN, to prevent the unauthorized person from attacking the switch itself. Once the VLAN is created, add unused ports to the VLAN. Sometimes, this VLAN is called the "parking" or "black hole" VLAN.

The order in which you type commands to shut down a VLAN is important. All interfaces in the VLAN must be in the shutdown state before shutting down the VLAN. Otherwise, the VLAN will remain active even if you administratively shutdown the VLAN. Also, after a VLAN is shutdown, enabling interfaces that belong to the VLAN will have no effect. You must first enable the VLAN before interfaces in the VLAN can be enabled.

Creating and applying infrastructure ACLs to restrict access to the device’s management interfaces to trusted IP addresses only.

Typically, they should be deployed at network ingress points, as a first line of defense against external threats. For example, iACLs can provide defense against certain types of invalid traffic on the internet. iACL must be applied to all interfaces that face noninfrastructure devices. This includes interfaces that connect to the internet, other organizations, remote access segments, user segments, and segments in data centers.

When developing an iACL, you should understand the required protocols by the specific infrastructure. Although every site has specific requirements, certain protocols are commonly deployed and must be understood

Longer and more complex passwords are time-consuming for the attackers to crack them. Specifying a minimum length of a password and forcing an enlarged character set (upper case, lower case, numeric, and special characters) can have an enormous influence on the feasibility of brute force attacks. However, if users attempt to meet the enlarged character set requirements by making simple adjustments, such as capitalizing the first letter and appending a number and an exclamation point (changing, for example, unicorn to Unicorn1!), little is gained against a dictionary attack using some simple transforms.

Besides the password creation, password policy consists of password management, which includes storage, protection, and password changes. In an enterprise environment, password management should follow these guidelines:

Storing, transmitting, or copying passwords in cleartext should not be possible.

Passwords should be stored in a centralized database, where each user uses their own credentials to access only the passwords they are authorized to access

The access to passwords should be audited and a log kept with a timestamp, user and which password was accessed.

Creating, deleting, and editing passwords should follow company’s security guidelines

Passwords should be changed regularly, depending on the password complexity and company security policy, and old passwords should never be reused.

Cisco Discovery Protocol can be useful for network troubleshooting. Cisco Discovery Protocol is enabled by default in Cisco IOS Software Release 15.0 and later. Some network management software takes advantage of Cisco Discovery Protocol neighbor data to map out topological connectivity. Cisco VoIP deployments can take advantage of Cisco Discovery Protocol to automatically assign the voice VLAN to Cisco IP phones. On the other hand, Cisco Discovery Protocol provides an easy reconnaissance vector to any attacker with an Ethernet connection. For example, when a switch sends a Cisco Discovery Protocol announcement out of a port where a workstation is connected, the workstation normally ignores it. However, with a simple tool such as Wireshark, an attacker can capture and analyze the Cisco Discovery Protocol announcement. Included in the Cisco Discovery Protocol data is the model number and operating system version of the switch. An attacker can then use this information to look up published vulnerabilities that are associated with that operating system version and potentially follow up with an exploit of the vulnerability. The organization must decide whether the convenience that Cisco Discovery Protocol brings is greater than the security risk that comes with Cisco Discovery Protocol.

Full best practise configuration guide [https://www.cisco.com/c/en/us/support/docs/ios-nx-os-software/ios-xe-16/220270-use-cisco-ios-xe-hardening-guide.html](https://www.cisco.com/c/en/us/support/docs/ios-nx-os-software/ios-xe-16/220270-use-cisco-ios-xe-hardening-guide.html)

To display the list of UDP or TCP ports that the device is listening on and to determine which services need to be disabled, use the show control-plane host open-ports command.

As an alternative, Cisco IOS Software provides the AutoSecure function that helps disable these unnecessary services while enabling other security services.

| service tcp-keepalive-in service tcp-keepalive-out | Stale connections use resources and could potentially be hijacked to gain illegitimate access. The TCP keepalives-in service generates keepalive packets on idle outgoing and incoming network connections. This service allows the device to detect when the host fails and drop the session. If enabled, keepalives are sent once per minute on idle connections The connection is closed within five minutes if no keepalives are received or immediately if the host replies with a reset packet Note you need to turn on TCP keepalives on both end routers so that one router will notice when the connection to the other router goes away; otherwise, the far end has no way to know that a reboot or other connection loss has happened |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| no ip proxy-arp                                    | Proxy ARP is a technique in which one device will answer an ARP request that is sent to another device. There are several disadvantages for using proxy ARP. An example is that it leads to an increase in the amount of ARP traffic, causing resource exhaustion. An attacker may utilize this to exhaust the device’s resources.                                                                                                                                                                                                                                                                                                                                                                                                               |
| no service config                                  | Disable Cisco device service for automatic configuration from remote devices through TFTP and other methods                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| no ip source-route                                 | IP Source Routing allows the router to decide on the forwarding path based on the source of the traffic. This can be used by attackers to attempt to route traffic around security devices in the network. Use the no ip source-route command in the global configuration mode to disable IP source routing.                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| no service pad                                     | You should disable Packet Assembler/Disassembler (PAD) service, which is used for X.25 networks.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| no ip gratuitous-arps                              | IP Gratuitous ARP messages are sent when the device updates its MAC Address-to-IP Address mapping. When this occurs, it will send a gratuitous ARP which is broadcast in the local segment. Use the no ip gratuitous-arps command in interface configuration mode to prevent the router from sending Gratuitous ARP messages on PPP links. This will not affect HSRP and VRRP protocols running on the device.                                                                                                                                                                                                                                                                                                                                   |
| no ip redirect                                     | You should disable ICMP redirect messages on routers. When a router receives and transmits a packet on the same interface, it will send an ICMP redirect message back to the source. In a properly functioning IP network, a router will only send an ICMP redirect message to its own local subnets. A malicious user can exploit the ability of the router to send ICMP redirects by continually sending packets to the router. This forces the router to respond with ICMP redirect messages, which impacts the CPU and performance of the router.                                                                                                                                                                                            |
| no ip unreachable                                  | When traffic is filtered by using an interface access-list, this triggers the router to send an ICMP unreachable message back to the source. While the generation of these messages are limited to one packet every 500 milliseconds, they increase the CPU utilization on the network device. Use the no ip unreachable command in interface configuration mode to prevent the router from sending ICMP unreachable messages. If ICMP Unreachable messages are required on the device, you may change the default rate limit by using the ip icmp rate-limit unreachable interval-in-ms global configuration command.                                                                                                                           |
| no ip http secure-server no ip http server         | disable router to act as http server if not used                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| no ip finger                                       | Disable the finger service. The finger service allows users in the network to get a list of users currently using a routing device. The information provided in this command includes the processes running on the system, line number, connection name, idle time, and terminal location.                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ip dhcp bootp ignore                               | Disable the Bootstrap Protocol Service (BOOTP) if not needed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| no mop enabled                                     | Disable the Maintenance Operations Protocol (MOP) service. This service is part of the Decnet protocol suite. This allows access to nodes that are in a state in which only data link layer services are available.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| no ip domain-lookup                                | You should disable DNS resolution service. This service allows for the network device to make DNS lookup requests to a DNS.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

### Network mapper (nmap)

**Network mapper (nmap)** is a tool commonly used by network administrators and security professionals to discover, analyze, and audit network hosts and services.

nmap can discover active hosts on a network, even if they are hidden behind firewalls or routers. It uses various techniques like ICMP ping, TCP and UDP probing, and ARP scanning to identify live hosts. Can be used to assess the security of firewalls and network configurations by identifying open ports and services that might pose security risks.

Can often guess the target host's operating system based on the way it responds to network probes.

Includes a scripting engine called NSE (Nmap Scripting Engine) that allows users to write custom scripts to automate tasks like vulnerability scanning, information gathering, and more.

nmap is available for every linux and windows distribution and can be easily observed in pcap, when it tries to check open ports for given node, it doesn't include any options that are usually contained when actual tcp handshake is being negotiated, moreover you can see that one source IP tries to open many ports

However nmap can be configured to force machine to negotiate the options to look as usual tcp connection and thus hides the scan/penetration test

The "TCP completeness" filter in Wireshark allows to check whether nmp scanning was active, as it usually sends the reset or fin flag after receiving syn ack

| nmap -F \<IP/Mask> | -F (Fast Scan): Nmap will perform a quick scan by only scanning the 100 most common ports on the target host or subnet, which can speed up the processing |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |

![](<../.gitbook/assets/Unknown image (792)>)

| -sS | TCP SYN Scan - The default scan type, scans for open TCP ports. |
| --- | --------------------------------------------------------------- |

![](<../.gitbook/assets/Unknown image (793)>)

| -O:                                                     | OS Detection - Tries to guess the operating system of the target.                        |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| -sV                                                     | Version Detection - Attempts to determine the version of services running on open ports. |
| nmap -sS 192.168.0.1 \| grep open \| cat >> results.txt | to save the output of open ports in txt file cat results.txt                             |

### Cryptography basics

#### Encryption

**Data integrity** means that data must remain unaltered and accurate during transmission.

**Data confidentiality** means that data must be accessible only to authorized systems.

**Forward secrecy** generates a unique temporary session key per communication session. If a long-term key is compromised, past sessions remain protected.

**Message integrity check (MIC)** is a cryptographic technique used to verify the integrity of transmitted data and ensure it has not been altered.

![](<../.gitbook/assets/Unknown image (794)>)

**Hashing** converts input data of any length (for example, a message or password) into a fixed-size output called a hash value.

This is achieved by using a mathematical function called a hash function, which takes the input and generates a (typically) unique hash value.

![](<../.gitbook/assets/Unknown image (795)>)

**WEP (Wired Equivalent Privacy)**

(deprecated) provide both authentication and encryption. Uses easily cracked and unsecure RC4 algorithm. Same key used for auth

**TKIP (Temporal Key Integrity Protocol)**

(deprecated) temporary solution until a new standard was created and new hardware was built

**Features**

A MIC is added to protect the integrity of messages

A Key mixing algorithm is used to create a unique WEP key for every frame

The initialization vector is 24 bits to 48 bits, making brute-force attacks much more difficult.

A TKIP sequence number and timestamp is used to keep track of frames sent from each source MAC add

Prevent replay attacks that involve re-resending a frame that has already been transmitted.

**CCMP (Counter/CBC-MAC Protocol)**

CCMP was developed after TKIP and is more secure. Consists of two algorithms:

AES (Advanced Encryption Standard) is the most secure encryption protocol. There are multiple modes of operation for AES. CCMP uses 'counter mode'

CBC-MAC (Cipher Block Chaining Message Authentication Code) is used as a MIC to ensure the integrity of messages

**GCMP (Galois/Counter Mode Protocol)**

is more secure and efficient than CCMP and uses AES counter mode encryption and GMAC (Galois Message Authentication Code) as a MIC to ensure the integrity of messages

#### Public Key Infrastructure (PKI)

**Public Key Infrastructure (PKI)** is a system of digital certificates, certificate authorities (CA), and registration authorities used to verify identities over the Internet.

The main goal of PKI is to provide a secure way to exchange information online. PKI-based encryption and authentication (for example, RSA) secure website transactions, email, and VPN connections.

**Symmetric cryptography** uses one key to encrypt and decrypt a message.

**Asymmetric cryptography** uses two keys.

Public key used to encrypt data

Private key used to decrypt data

Data encrypted with the public key can only be decrypted with the corresponding private key, and vice versa as they are mathematically related with the special property

**Components**

**Digital certificates**

electronic documents that contain a digital signature and a public key. They are used to verify the identity of an individual or organization.

**Certificate authorities (CA)**

trusted third-party organizations that issue digital certificates. They verify the identity of the certificate holder and then sign the certificate

**Registration authorities (RA)**

organizations or individuals that are responsible for verifying the identity of a certificate holder before the certificate is issued by a CA

**Repositories**

databases used to store digital certificates and other PKI-related information

**Configuration**

1. generate an RSA key
2. (config)#crypto pki trustpoint

(ca-trustpoint)#enrollment terminal

3. (config)#crypto pki enroll

**Group Policy Object (GPO) update** is the process where Group Policy changes are applied to clients or users in a Microsoft Windows environment.

**Active Directory (AD)** is a directory service and identity management system developed by Microsoft. It centralizes users, computers, groups, and security policies.

AD provides authentication and authorization services. When a user logs into a Windows-based network, AD verifies their identity and determines what resources they can access based on their permissions and group memberships. Supports SSO.

#### Transport Layer Security (TLS)

**Transport Layer Security (TLS)** is an IETF standard protocol that provides authentication, privacy, and data integrity between communicating applications.

It's the most widely deployed security protocol in use today and is best suited for web browsers and other applications that require data to be securely exchanged over internet.

It is successor of Secure Sockets Layer (SSL)

TLS uses a client-server handshake mechanism to establish an encrypted and secure connection and to ensure the authenticity of the communication

**Process**

1. Communicating devices exchange encryption capabilities
2. An authentication process occurs using digital certificates to help prove the server is the entity it claims to be
3. A session key exchange occurs. During this process, clients and servers must agree on a key to establish the fact that the secure session is indeed between the client and server

TLS uses a public key exchange process to establish a shared secret between the communicating devices. The two handshake methods are the Rivest-Shamir-Adleman (RSA) handshake and the Diffie-Hellman handshake. Once the keys are exchanged, data transmissions between devices on the encrypted session can begin

**Datagram Transport Layer Security (DTLS)**

covers secure communication over UDP and is based on TLS, and includes enhancements such as sequence numbers and retransmission capability to compensate for the unreliable nature of UDP

![](<../.gitbook/assets/Unknown image (796)>)

### Platforms and vendors

#### Palo Alto

**Palo Alto** is a cybersecurity company that specializes in firewall solutions with GUI-based management.

#### Splunk (Cisco)

**Splunk** is a data analytics and log management platform used for monitoring, searching, and analyzing machine-generated data.

Splunk is a versatile software platform used for a wide range of purposes related to data analytics, log management and information security. It is primarily known for its capabilities in collecting, indexing and analyzing large volumes of machine-generated data from various sources.

Splunk is often used as a SIEM (Security Information and Event Management) platform to monitor and detect security threats and incidents. It can ingest security event data, correlate events and provide alerts and reports on potential security breaches. Beyond SIEM, Splunk enables security analysts to perform in-depth investigations into security incidents, conduct forensics and identify patterns of malicious behavior. It is often used in SOC and in NOC helping IT operations teams monitor the health and performance of IT infrastructure, applications and services. It can proactively identify issues, monitor system uptime and optimize performance.

#### Illumio

**Illumio** is a cybersecurity company focused on micro-segmentation to isolate workloads and applications.

Illumio follows a zero-trust security model, meaning it doesn't trust anything inside or outside its perimeters and verifies every user and device trying to connect to resources.

#### Qualys

**Qualys** is a company specializing in cloud-based security and compliance solutions.

Qualys Cloud Platform: This is the core platform that provides a centralized dashboard and various tools for managing security and compliance across an organization. It includes features such as vulnerability management, policy compliance, threat protection, and asset inventory.

QualysGuard Vulnerability Management: This solution allows organizations to scan their network and systems for vulnerabilities and prioritize remediation efforts based on the severity of the risks identified. It helps in identifying security weaknesses and provides recommendations for addressing them.

Qualys Policy Compliance: This tool helps organizations assess their systems against predefined security policies and industry standards such as PCI DSS, HIPAA, and GDPR. It provides reports and recommendations for achieving and maintaining compliance.

Qualys Threat Protection: This solution helps organizations detect and respond to threats and potential security incidents in real-time. It includes features like continuous monitoring, threat intelligence, and incident response capabilities.

Qualys Asset Inventory: This tool helps organizations gain visibility into their IT assets and their security posture. It provides insights into hardware and software inventory, tracks changes in the environment, and helps identify any unauthorized or vulnerable devices

#### Zscaler Internet Access (ZIA)

**Zscaler Internet Access (ZIA)** is a cloud-native security service edge (SSE) solution. It replaces legacy network security solutions to stop advanced attacks and prevent data loss with a zero trust approach.

In most organizations, you will use the Zscaler Client Connector to connect to ZIA. Traffic may be forwarded in other ways, such as through GRE or IPSec tunnels from a workplace, but a client connector must be installed if you use other services, such as Zscaler Private Access (ZPA) or Zscaler Digital Experience (ZDX)

**Traffic Forwarding Connections to Zero Trust Exchange (ZTE)**

Zero trust connections are, by definition, independent of any network for control or trust. Zero trust ensures access is granted by never sharing the network

between the originator (user/device, loT/OT device, or workload) and the destination app.

By keeping these separate, zero trust can be properly implemented and enforced over any network

![](<../.gitbook/assets/Unknown image (797)>)

#### CrowdStrike Falcon

**CrowdStrike Falcon** is a platform focused on stopping IT breaches. It protects against malware, exploits, zero-days, stolen credentials, and tool-based attacks (for example, PowerShell). It includes antivirus plus endpoint detection and response (EDR), threat intelligence, and forensic capabilities.

The functionality is divided into two parts, primarily the so-called Sensor Content, which is a package of rules distributed always with a new release of the Falcon platform base, while this content is not changed dynamically from the cloud. Here, the user can choose whether he wants to use the current version, the previous one, or the one before it.

The second element is Rapid Response Content, which is already a package dynamically updated from the cloud, reacting as quickly as possible to current events, evaluating behavior corresponding to patterns and using updated telemetry and detection patterns. This package is distributed in the form of a binary file and complements the static definitions distributed with Falcon itself in the system. And it was with the latest Rapid Response Content package that the incriminated error occurred. Rapid Response Content is, of course, subject to testing prior to distribution to customers, which has not revealed an error.

### Identity and access management (IAM)

#### LDAP (Lightweight Directory Access Protocol)

**LDAP** is a protocol for accessing and managing directory information services. It’s commonly used for storing information about users, devices, and resources, and is often paired with Active Directory.

#### Active Directory (AD)

**Active Directory (AD)** is a Microsoft directory service. It stores objects like users, groups, and computers, and provides authentication, authorization, and resource management.

#### Kerberos

**Kerberos** is a network authentication protocol that uses tickets and encryption. It enables secure authentication over untrusted networks.

#### Single Sign-On (SSO)

**Single Sign-On (SSO)** lets users authenticate once and access multiple apps without re-authenticating each time.

**SAML (Security Assertion Markup Language)** is an XML-based framework for exchanging authentication and authorization data between identity providers (IdPs) and service providers (SPs).

**SCIM (System for Cross-domain Identity Management)** is a standard for user identity lifecycle management. It focuses on provisioning and synchronization across systems.

In summary, SAML primarily focuses on authentication and SSO between IDPs and SPs, while SCIM focuses on user provisioning and identity management across different systems.

![](<../.gitbook/assets/Unknown image (798)>)

![](<../.gitbook/assets/Unknown image (799)>)

#### Privileged Access Management (PAM)

**Privileged Access Management (PAM)** is a set of strategies and technologies to control, monitor, and manage privileged access.

#### Endpoint Detection and Response (EDR)

**Endpoint Detection and Response (EDR)** helps detect and respond to threats on endpoints (laptops, desktops, servers, mobile devices). It provides visibility into endpoint activity and flags suspicious behavior.

EDR solutions typically work by installing lightweight agents on endpoints, which monitor endpoint activity and send data back to a central management console for analysis. These agents can identify a range of suspicious activities, including malware infections, unauthorized access attempts, and data exfiltration.

When an EDR solution detects a threat, it can automatically respond by isolating the affected endpoint or taking other remediation steps.
