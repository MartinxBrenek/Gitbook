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

# Cisco Security Design and Technology

## Cisco SAFE

**Cisco SAFE** is a security architectural framework that helps design secure solutions. Cisco Validated Design (CVD) guides provide detailed networking design and implementation guidance

### Places in the network (PINs)

#### Branch

**Branch** PINs are prime targets. They are typically less secure than campus and data center PINs because they aim to be cost-effective.

It is important to ensure that vital security capabilities are included in the design while keeping it costveffective.

Top threats on branch PINs include endpoint malware (point-of-sale \[POS] malware), MitM attacks, unauthorized/malicious client activity

#### Campus

**Campus** PINs contain large numbers of users (employees and guests). They are easy targets for phishing, web-based exploits, unauthorized access, malware propagation, and botnets.

#### Data center

**Data center** PINs contain an organization’s most critical information assets and intellectual capital. They often have hundreds or thousands of servers, which makes it hard to manage security rules. Typical threats include data extraction, malware propagation, and unauthorized access.

#### Edge

**Edge** is the primary ingress and egress point for Internet traffic. For this reason, it is the highest-risk PIN.

Typical threats seen on the edge include web server vulnerabilities, distributed denial-of-service (DDoS) attacks, and on endpoints its macro attack and ransomware

#### Cloud

Security in the cloud is dictated by service-level agreements (SLAs) with the cloud service provider and requires independent certification audits and risk assessments

The primary threats are web server vulnerabilities, data loss, malware

#### Wide area network (WAN)

**Wide area network (WAN)** connects PINs together. In large organizations, managing WAN security is challenging. Threats include malware propagation and WAN sniffing.

### Evaluation concepts for PINs

Management of devices and systems using centralized services is critical for consistent policy deployment, change management, and keeping systems patched

Security intelligence provides detection of emerging malware and cyber threats. It enables an infrastructure to enforce policy dynamically to keep up with the new threats

Compliance data security standards PCI DSS 3.0 and HIPAA

Segmentation involves establishing boundaries for both data and users. Advanced segmentation reduces operational challenges by identity-aware infrastructure to enforce policies in an automated and scalable manner

#### Threat defense

**Threat defense** enables assessment of the nature and potential risk of suspicious activity so the correct response can be taken.

### Next-generation endpoint security

**Next-generation endpoint security** addresses endpoints as high-value targets (smartphones, PCs, IP phones, IoT).

### Cisco Talos

**Cisco Talos** tracks threats across endpoints, networks, cloud environments, the web, and email to provide a comprehensive view of threats, root causes, and outbreak scope.

Consist of specialized Cisco teams that use multiple feeds to keep security up to date and implement them to following products and solutions

### Cisco Threat Grid

**Cisco Threat Grid** performs static file analysis (filenames, MD5, file type) and dynamic analysis by executing files in a monitored sandbox. It helps determine if a sample is malware, what it does, and how to defend. Some malware detects sandboxes and refuses to run. You can also use **Glovebox** to safely interact with suspicious files.

### Cisco Advanced Malware Protection (AMP)

**Cisco Advanced Malware Protection (AMP)** is a malware analysis and protection solution that goes beyond point-in-time detection. Point-in-time detection is blind to the scope and depth of a breach after it happens.

![](<../.gitbook/assets/Unknown image (867)>)

### Attack structure

#### Before attack

full knowledge of all the assets that need to be protected is required, and the types of threats that could target these assets need to be identified. This phase involves establishing policies and implementing prevention to reduce risk. NGFW, network access control, network security analysis

Global threat intelligence from Cisco Talos and Cisco Threat Grid updates features into AMP to protect against known and new emerging threats

#### During attack

defines the abilities and actions that are required when attack gets through. NGIPS, NGFW detects, blocks, and defends against attacks that have penetrated the network and are in progress. File reputation to determine, whether a file is clean or malicious as well as sandboxing are used to identify threats during an attack

#### After attack

defines the ability to detect, contain, and remediate an attack. Any lessons learned need to be incorporated into the existing security solution.

Using Stealthwatch to quickly and effectively scope, contain, and remediate an attack to minimize damage

Cisco AMP provides retrospection analysis, indicators of compromise (IoCs), breach detection, tracking, and surgical remediation after an attack

### AMP components

AMP Cloud (private or public; used to deposit threats detected by endpoints for analysis)

Threat intelligence from Cisco Talos and Cisco Threat Grid

AMP connectors

AMP4E for Endpoints (Cisco Security Connector (CSC))

AMP for Networks (NGFW, NGIPS, ISRs)

AMP for Email (ESA)

AMP for Web (WSA)

AMP for Meraki MX

### Cisco AnyConnect

**Cisco AnyConnect** is a modular endpoint VPN product using TLS and IPsec IKEv2.

### Cisco Umbrella

**Cisco Umbrella** provides first-line defense by blocking requests to malicious destinations (domains, IPs, URLs) via Anycast DNS before a connection is established or a file is downloaded. It is 100% cloud delivered.

### Cisco Web Security Appliance (WSA)

**Cisco Web Security Appliance (WSA)** is an all-in-one web gateway that can block hidden malware from suspicious and legitimate websites. It can be deployed in the cloud, as a virtual appliance, on-premises, or hybrid.

![](<../.gitbook/assets/Unknown image (868)>)

#### Before attack

set of known URLs that are allowed; each unknown URL is given reputation score by quickly scanning type of the

site, content, time when was created and domain owner

Dynamic Content Analysis (DCA) engine scans text, scores the text for relevancy, calculates model document

proximity, and returns the closest category match. Frequently updated by Talos with support from other vendors

Cisco Application Visibility and Control (AVC) provide administrators with the most granular control over app and

usage behavior. For ex: Facebook or Youtube can be allowed, while like button or some of it's content denied

#### During attack

identify and block zero-day threats that managed to infiltrate the network

WSA Enhances malware defense coverage with multiple anti-malware scanning engines running in parallel on a single appliance while maintaining high processing speeds and preventing traffic bottlenecks. WSA scans L4 traffic, ports, and protocols to detect and block spyware

WSA Captures a fingerprint of each file as it traverses the gateway and sends it to AMP Cloud for a reputation verdict checked against all zero-day files

WSA uses Internet Control Adaptation Protocol (ICAP) to integrate with Data loss prevention (DLP) solutions from leading third-party DLP vendors. By directing all outbound traffic to the third-party DLP appliance, content is allowed or blocked based on the third-party rules and policies. Engines inspect outbound traffic and analyze it for content markers, such as confidential files, credit card numbers, customer personal data and prevent this data from being uploaded into cloud file-sharing services such as iCloud and Dropbox

#### After attack

WSA using AMP retrospection capabilities and continues to scan files using the latest detection capabilities and collective threat intelligence from Talos and AMP Thread Grid

![](<../.gitbook/assets/Unknown image (1937)>)

### Cisco Email Security Appliance (ESA)

**Cisco Email Security Appliance (ESA)** helps users communicate securely via email and helps organizations combat email threats. Cisco Context Adaptive Scanning Engine (CASE) blocks spam emails.

Forged email detection protects high-value targets. Automatic data protection for sensitive/confidential content in outgoing emails

Cisco Advanced Phishing Protection (CAPP) uses Talos this intelligence to stop identity deception–based attacks such as fraudulent senders, social engineering,...

Cisco Domain Protection (CDP) for external email helps prevent phishing emails from being sent using a customer domains. Graymail consists of marketing, social networking, and bulk messages which comes with an unsubscribe link, which may be used for phishing > solved by Safe Unsubscribe. URL filtering and scanning of URLs in attachments

Outbreak filters can rewrite URLs included in suspicious email messages to direct user to WSA and display block screen > also tracks users that clicked this rewritten URL

### Next-Generation Intrusion Prevention System (NGIPS)

Intrusion detection system (IDS) passively monitors and analyzes network traffic for potential network intrusion attacks and logs intrusion attack data for security analysis

Intrusion prevention system (IPS) provides IDS functions and also automatically blocks intrusion attacks

Cisco Firepower Management Center (FMC) is centralized dashboard management for Firepower including IDS and IPS. Available as VM or physical appliance

Automatically correlates threat events, contextual information, and network vulnerability data to optimize defenses by automating protection policy updates.

Detecting the spread of malware by baselining normal network traffic that is used to compare with abnormal network behaviour.

Applies remediation on compromised hosts with ISE:

Quarantine Limits or blocks an endpoint’s access to the network

Unquarantine Removes the quarantine

Shutdown Shuts down the port that a compromised endpoint is attached to

### Next-Generation Firewall (NGFW)

Cisco Stateful firewall is called Adaptive Security Appliances (ASA)

Cisco Firepower NGFW can block threats such as advanced malware and application-layer attacks.

Software for hardware appliances available: ASA, ASA with Firepower and FDT sw images

Firepower Threat Defense (FTD) provides IPS/IDS capabilities. FTD software image merges the ASA software image and the Firepower Services image into a single unified image

Management via FMC and for ASA it's Cisco Security Manager (CSM)/Cisco Defense Orchestrator/Adaptive Security Device Manager (ASDM)

### Cisco Stealthwatch

is a agentless collector and aggregator of network telemetry data that performs network security analysis and monitoring to automatically detect threats that manage to infiltrate a network as well as the ones that originate from within a network

#### Stealthwatch Enterprise

provides real-time visibility into activities occurring within the network; can be scaled into the cloud, across the network, to branch locations, in the data center, and down to endpoints

**Components**

Flow Rate License required for the collection, management, and analysis of flow telemetry data and aggregates flows at the Stealthwatch Management Console

Flow Collector collects and analyzes enterprise telemetry data such as NetFlow

It can also pinpoint malicious patterns in encrypted traffic using Encrypted Traffic Analytics (ETA) without having to decrypt it to identify threats (HW appliance or VM)

Stealthwatch Management Console (SMC) GUI control center for Stealthwatch, analysis from up to 25 Flow Collectors, Cisco ISE, and other sources. HW appliance or VM

Flow Sensor for networks that can't generate NetFlow by their own. Provides visibility into the application layer data. HW appliance or VM

UDP Director is a virtual appliance that serves as a central collector for flow data generated by flow-enabled devices. Every router is configured

with a single NetFlow export and send the data to the UDP Director. The UDP Director takes the data and replicates the NetFlow data from all

routers to the multiple destinations in single stream of data. HW appliance or VM

![](<../.gitbook/assets/Unknown image (1938)>)

#### Stealthwatch Cloud

is a cloud-based software-as-a-service (SaaS) solution which provides the visibility and continuous threat detection required to secure the

on-premises, hybrid, and multicloud environments

Accurately detect threats in real time, regardless of whether an attack is taking place on the network, in the cloud, or across both environments

Consumes metadata only. The actual packet payloads are never retained or transferred outside the network

**Public Cloud Monitoring**

provides visibility and threat detection in AWS, GCP, and Microsoft Azure cloud infrastructures.

It is SaaS-based solution that can be deployed easily and quickly. Provided also agentless

**Private Network Monitoring**

detection for the on-premises network, delivered from a cloud-based SaaS solution. For organizations that want better awareness and security in their on-premises environments while reducing capital expenditure and operational overhead. A lightweight virtual appliance needs to be installed in a virtual machine or server. The collected metadata is encrypted and sent to the Stealthwatch Cloud analytics platform for analysis. Is licensed based on the average monthly number of active endpoint

### Cisco Identity Services Engine (ISE)

**Cisco Identity Services Engine (ISE)** is a centralized security policy management platform that identifies users and devices connecting to the network. It provides network access control and network segmentation.

Web-based detailed visibility into what is happening in the network, such as who is connected (endpoints, users, and devices), which applications are installed and running on endpoints

Integrates with DNA center. Supports RADIUS and TACACS+ protocol. Implements Cisco TrustSec. Can act as an internal certificate authority.

Automates 802.1x supplicant provisioning and certificate enrollment. It also integrates with mobile device management (MDM)/enterprise mobility management (EMM) vendors for mobile device compliance and enrollment

### Zero Trust overview

Zero trust is a strategic approach to security that centers on the concept of eliminating trust from an organization's network architecture.

Trust is neither binary nor permanent. It can no longer be assumed that internal entities are trustworthy, that they can be directly managed to reduce security risk, or that checking them one time is enough. The zero-trust model of security prompts you to question your assumptions of trust at every access attempt.

Traditional security approaches assume that anything inside the corporate network can be trusted. The reality is that this assumption no longer holds true, thanks to mobility, BYOD (Bring Your Own Device), IoT (Internet of Things), cloud adoption, increased collaboration, and a focus on business resiliency. A zero-trust model considers all resources to be external and continuously verifies trust before granting only the required access.

![](<../.gitbook/assets/Unknown image (1939)>)

![](<../.gitbook/assets/Unknown image (1940)>)

User and Device Security: Making sure users and devices can be trusted as they access systems, regardless of location.

Network and Cloud Security: Protect all network resources on-prem and in the cloud, and ensure secure access for all connecting users.

Application and Data Security: Preventing unauthorized access within application environments irrespective of where they are hosted.

### Cisco TrustSec

**Cisco TrustSec** is a next-generation access control enforcement solution to address operational challenges with firewall rules and ACLs.

It uses Security Group Tag (SGT)to perform ingress tagging and egress filtering to enforce access control policy to endpoints.

ISE assigns SGT tags to authenticated and authorized endpoints through .1x, MAB or WebAuth

Network policy is then applied throughout the network, based on the SGT tag instead of a network IP/MAC, while having endpoints not aware of the SGT tag

#### Phases

**Ingress classification**

process of assigning SGT tags endpoints as they ingress the TrustSec network

**Methods**

* Dynamic assignment is downloaded from ISE when authenticating endpoint
* Static assignment: IP to SGT tag / Subnet to SGT tag / VLAN to SGT tag / L2 interface to SGT tag / L3 logical interface to SGT tag / Port to SGT tag

**Propagation**

process of communicating mappings to TrustSec network devices that will enforce policy based on SGT tags

**Methods**

* Inline (Native) tagging: switch inserts SGT tag inside a frame to allow upstream devices to read and apply policy. Completely independent of any L3 protocol (IPv4 or IPv6), so the frame or packet can preserve the SGT tag throughout the network infrastructure (supported only by

Cisco network devices with ASIC support for TrustSec; devices that doesn't support drops frame

![](<../.gitbook/assets/Unknown image (1941)>)

![](<../.gitbook/assets/Unknown image (1942)>)

SGT Exchange Protocol (SXP) is a TCP-based peer-to-peer protocol used for network devices that don't support SGT inline tagging in hardware.

IP-to-SGT mappings can be communicated between non-inline tagging switches and other network devices. Non-inline tagging switches also have an SGT mapping database to enforce policy

* Speaker: SXP peer that sends IP-to-SGT bindings
* Listener: IP-to-SGT binding receiver

![](<../.gitbook/assets/Unknown image (869)>)

![](<../.gitbook/assets/Unknown image (870)>)

**Egress enforcement**

policies are enforced by applying them to endpoints, regardless of location for wire/less traffic

**Types**

* Security Group ACL (SGACL): applied to routers and switches.

Access lists provide filtering based on source and destination SGT tags

* Security Group Firewall (SGFW): provides enforcement on firewalls (Cisco ASA/NGFW)

Requires tag-based rules to be defined locally on the firewall

![](<../.gitbook/assets/Unknown image (871)>)

![](<../.gitbook/assets/Unknown image (872)>)

![](<../.gitbook/assets/Unknown image (873)>)

### Secure Access Service Edge (SASE)

**Secure Access Service Edge (SASE)** is a cloud-based model that consolidates networking and security services. It emerged as apps and data moved to the cloud, increasing the need to secure Internet access to cloud services.

SASE is a cloud-based network infrastructure model that consolidates network (NaaS) and security (FWaaS) services into a single service provider

It moves access control to the cloud edge, making it simpler for companies to maintain and monitor all connected devices to the cloud edge

With SASE, corporations can eliminate the need to manage and maintain VPN connections for Internet-based employees, simplifying network administration

#### Components

* **Zero Trust Network Access (ZTNA)**: The Zero Trust security model assumes threats are present both inside and outside a network; therefore, strict contextual verification is required every time a person, app, or device tries to access resources on a corporate network. Zero Trust Network Access (ZTNA) is the technology that makes the Zero Trust approach possible — it sets up one-to-one connections between users and the resources they need, and requires periodic reverification and recreation of those connections.
* **Secure web gateway (SWG)**: prevents cyber threats and protects data by filtering unwanted web traffic content and blocking risky or unauthorized user behavior online. SWGs can be deployed anywhere, making them ideal for securing hybrid work.
* **Cloud access security broker (CASB)**: help keep corporate software-as-a-service (SaaS) applications, along with infrastructure-as-a-service (IaaS) and platform-as-a-service (PaaS) services, safe from cyber attacks and data leak

A number of different security technologies fall under the CASB umbrella, and a CASB solution will typically offer these technologies together in one bundled package. These technologies include shadow IT discovery, access control, and data loss prevention (DLP),

* **Software-defined WAN (SD-WAN) or WANaaS**: In a SASE architecture, organizations adopt either SD-WAN or WAN-as-a-Service (WANaaS) to connect and scale operations (e.g., offices, retail stores, data centers) across large distances. SD-WAN and WANaaS use different approaches:

SD-WAN technology uses software at enterprise sites and a centralized controller to overcome some of the limitations of traditional WAN architectures, simplifying operations and traffic steering decisions.

WANaaS builds on the benefits of SD-WAN by taking a “light branch, heavy cloud” approach that deploys the minimum required hardware within physical locations and uses low-cost Internet connectivity to reach the nearest “service edge” location. This can reduce total costs, offer more integrated security, improve middle mile performance, and better serve cloud infrastructure.

* **Next-Generation Firewall (NGFW)** is an advanced security solution that combines traditional firewall capabilities with additional features such as intrusion prevention, application awareness, and advanced threat protection.

NGFWs provide enhanced security by inspecting and filtering network traffic based on application, user, and content, helping organizations defend against sophisticated cyber threats

* NGFWs that can be deployed in the cloud are called cloud firewalls or firewall-as-a-service (FWaaS)

Depending on the vendor’s capabilities, SASE components above may also be bundled with cloud email security, web application and API protection (WAAP), DNS security, and/or security service edge (SSE) capabilities described further below.

* **Security service edge (SSE)** provides a more concise solution compared to SASE, utilizing solely essential security components without the additional complexities of networking features, which is used by smaller companies

![](<../.gitbook/assets/Unknown image (874)>)

### Cisco DDoS Edge Protection

Cisco's Edge DDoS Protect presents an innovative solution providing fully distributed DDoS protection for all places in the network.

The key component of DDoS Edge Protect is a small, lightweight, and containerized DDoS detection engine, running inside Cisco IOS XR operating system on selected Cisco routers. As a feature of the router itself, the solution provides out-of-path, full detection, and granular mitigation capabilities against volumetric DDoS attacks. This allows the router to become the first line of defense against DDoS attacks where attack traffic can be blocked in-place and no longer needs to be backhauled to dedicated scrubbing infrastructure. This gives operators the opportunity to extend DDoS protection to the edge of the network which would otherwise be cost-prohibitive and negatively impacting the application latency and the quality of experience. The overall scrubbing capacity of the network is also increased since every router now has DDoS scrubbing capability.

You can apply this solution to the existing install base and to new router deployments without requiring any additional hardware.

The management of the on-router DDoS Edge Protect containers is done through Cisco's Edge Protect DDoS controller. The controller manages the deployment of containers to the routers and acts as a single pane of glass for monitoring the current state of the overall solution. The software tracks ongoing attacks and mitigations deployed.

![](<../.gitbook/assets/Unknown image (875)>)

![](<../.gitbook/assets/Unknown image (876)>)
