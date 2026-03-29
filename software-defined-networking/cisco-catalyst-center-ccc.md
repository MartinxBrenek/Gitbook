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

# Cisco Catalyst Center (CCC)

### Overview

**Cisco Catalyst Center (CCC)** (formerly DNACenter) is a Cisco SDN controller for enterprise networks—branch, campus, and WAN

It is a single pane of glass centralized management platform that serves as unified interface or dashabord to provide comprehensive visibility and control over various systems, applications, or network elements from a single location

This simplifies provisioning, automation, management, monitoring, and troubleshooting by consolidating multiple disparate tools and interfaces into a single, intuitive interface

Cisco Catalyst Center provides open programmable APIs for policy-based management and security through a single controller

Cisco Catalyst Center can program the network in an automated way, based on the application requirements, and it represents a basis for intent-based networking.

The controller consistently provisions network services and provides rich network information and analytics across all network resources: both LAN and WAN, wired and wireless, and physical and virtual infrastructures. This visibility allows you to optimize services and support new applications and business models. The controller bridges the gap between open, programmable network elements and the applications that communicate with them, automating the provisioning of the entire end-to-end infrastructure.

Cisco Catalyst Center provides a single dashboard for managing and controlling the enterprise network. It uses workflows to simplify provisioning of user access policies combined with advanced assurance capabilities. It also provides open platform APIs, adapters, and SDKs for integration with business applications and orchestrators.

![](<../.gitbook/assets/Unknown image (59)>)

The enterprise programmable network infrastructure sends data to the Cisco Catalyst Center virtual appliance. The appliance activates features and capabilities on your network devices using the Cisco Catalyst software. Everything is managed from the Cisco Catalyst Center dashboard.

Cisco Catalyst Center is a robust software solution that is designed to simplify and enhance network management. It offers a variety of deployment options to meet the diverse operational requirements of different organizations:

Cloud-based deployments: Cisco Catalyst Center Virtual Appliance can be deployed in major public cloud environments such as AWS (expected to be supported in Microsoft Azure and Google Cloud in the future) offering flexibility and scalability. This approach eliminates the need for on-premises hardware, reduces capital expenditure, and provides access to advanced cloud features.

On-premises virtualization: For organizations preferring on-premises solutions, Cisco Catalyst Center supports deployment on virtual environments like VMware ESXi (expected to support Microsoft Hyper-V, and KVM in the future). This enables customers to leverage existing virtual infrastructure, enhancing resource utilization and reducing costs.

Physical appliance: Cisco offers the option to deploy Cisco Catalyst Center as a physical appliance for customers who need dedicated hardware. This option is ideal for environments with strict security or performance requirements, ensuring maximum control and reliability.

![](<../.gitbook/assets/Unknown image (60)>)

![](<../.gitbook/assets/Unknown image (61)>)

### Cisco Catalyst Center dashboard and tools

The Cisco Catalyst Center dashboard provides an overview of network health and helps identify and remedy issues. Automation and orchestration capabilities provide zero-touch provisioning based on profiles, facilitating network deployment in remote branches. Advanced assurance and analytics capabilities use deep insights from devices, streaming telemetry, and rich context to deliver an experience while proactively monitoring, troubleshooting, and optimizing your wired and wireless network.

![](<../.gitbook/assets/Unknown image (62)>)

**Discovery:** The Discovery feature is designed to scan the network for connected devices. Once the devices are discovered, this feature compiles and sends the list of connected devices to the inventory, ensuring that the network inventory is always up to date and accurately reflects the current network topology.

**Topology:** The Topology tool provides a graphical representation of your network. Cisco Catalyst Center uses the Discovery settings that you have configured to identify the devices within your network and assigns a device role to them. These roles can be assigned during discovery or later modified in the Device Inventory. Based on the assigned roles, Cisco Catalyst Center constructs a detailed physical topology map. This map includes comprehensive device-level data, allowing for a clear and organized visualization of your network's structure and components.

**Command Runner:** The Command Runner tool enables you to send diagnostic CLI commands to the selected devices within your network. It allows you to run and view the output of these commands directly from Cisco Catalyst Center. Currently, only show and other read-only commands are permitted, ensuring that the tool is used for diagnostic purposes. While Command Runner supports only a subset of shortcuts available in a standalone terminal, it provides a convenient way to gather important diagnostic information from your network devices.

**License Manager:** The Cisco Catalyst Center License Manager tool helps you visualize and manage all your Cisco product licenses, including Smart Account licenses. This feature provides a centralized interface to keep track of license usage, compliance, and expiration, ensuring that you have the necessary licenses for your network devices and services.

**Template Hub:** Cisco Catalyst Center provides an interactive template hub for authoring CLI templates. You can easily design templates with predefined configurations using parameterized elements or variables. Once a template is created, it can be deployed across multiple devices and sites within your network. This functionality allows for streamlined and consistent configuration management, ensuring that all devices adhere to the desired configuration standards and policies.

**Model Config Editor:** Model Config allows you to define advanced customizations of the Cisco Validated Designs (CVDs). The Model Config Editor tool in Cisco Catalyst Center simplifies network provisioning—it extracts complex device configurations and enables customizable network setups through an intuitive GUI instead of traditional device-specific CLIs. This approach streamlines the configuration process, making it easier to manage and deploy consistent configurations across multiple devices without needing extensive CLI knowledge.

**Wide Area Bonjour:** Cisco Catalyst Center Wide Area Bonjour is a feature that extends the capabilities of Apple's Bonjour protocol across WANs. Bonjour, also known as zero-configuration networking, is a service discovery protocol that is used by Apple devices to locate and connect to services and file-shares on a local network. Traditionally, Bonjour is limited to local network subnets, which restricts its use in larger, segmented, or geographically distributed networks. Wide Area Bonjour addresses these limitations by enabling Bonjour service discovery and advertisement across multiple subnets and geographic locations. In this way, Wide Area Bonjour expands the matrix to enterprise-grade traditional wired and wireless networks, including overlay networks such as Cisco Software-Defined Access and industry-standard BGP EVPN with VXLAN.

**Security Advisories:** The Cisco Product Security Incident Response Team (PSIRT) plays a crucial role in handling Cisco product security incidents, overseeing the Security Vulnerability Policy, and issuing recommendations for Cisco Security Advisories and Alerts. These advisories are used by the Security Advisories tool. This tool scans the inventory of devices within Cisco Catalyst Center, identifying tools that are affected by known vulnerabilities. By employing this integration, organizations can proactively assess and mitigate security risks across their network infrastructure, ensuring the resilience and security of their digital assets.

**Field Notices:** Field Notices serve as notifications for significant issues directly involving Cisco products, often needing an upgrade, workaround, or other customer action. However, it is important to note that Field Notices do not address security vulnerability-related issues. By using the Field Notices tool within Cisco Catalyst Center, organizations can scan their inventory to identify devices that are affected by these known issues.

**Network Reasoner:** The Network Reasoner tool represents a significant advancement in network management, offering automated Cisco expertise to proactively evaluate your network, or to reactively diagnose complex issues. The dashboard provides concise insights into workflows, detailing the number of affected devices in the last 24 hours and the potential impact of running a workflow on the network.

**Network Bug Identifier:** The Cisco Catalyst Center Network Bug Identifier tool enables network administrators to scan the network for known defects or bugs that are identified by Cisco. By using this tool, administrators can identify specific patterns in device configurations or operational data and match them with known defects. The tool offers both bug-focused and device-focused views, providing comprehensive insights into potential issues that affect the network. Cisco Catalyst Center collects device configuration and operational data through CLI commands. This data is then processed in the Cisco CX Cloud to identify potential security advisories or bugs, ensuring proactive management of network vulnerabilities.

### Design workflow

**Network Hierarchy** sets up geolocation, building, and floorplan details (such as glass, light,thick wall etc..)and associate them with a unique site ID

**Network Settings** sets up DNS, DHCP, SNMP, and AAA servers, device credentials, wireless settings and IP management (compatible with infoblox via rest API)

**Image Repository** manage/download/deploy software images, maintenance updates,set version compliance

**Network Profiles** define LAN, WAN, WLAN connection profiles (such as SSID) and apply them to sites

### Policy workflow

**Dashboard** Used to monitor all the VNs, scalable groups, policies, and recent changes

**Group-Based Access Control** Used to create group-based access control policies, same as SGACLs

Cisco DNA Center integrates with Cisco ISE to simplify the process of creating and maintaining SGACLs

**IP-Based Access Control** Used to create IP-based access control policy the same way as ACL does

**Application** Used to configure QoS in the network through application policies.

**Traffic Copy** configure ERSPAN to copy traffic flow between two entities to a specified remote destination for monitoring and tshoot

**Virtual Network** sets up the VN and associate various scalable groups to them

### Provision workflow

**Devices** Used to assign devices to a site ID, confirm or update the software version,and provision the network

Devices are added/populated by CDP, LLDP, IP address ranges, non-Cisco (third party) devices are added using Software Development Kit (SDK)

**Fabrics** Used to set up the fabric domains

**Fabric Devices** Used to add devices to the fabric domain and specify device roles

Device states for PnP (Plug and Play)

Error - Device had an error and could not be provisioned

Unclaimed - Device has not been assigned a workflow

Planned - Device is added to Network Plug and Play and has been assigned a workflow, but has not yet contacted the server

Provisioned - Device is successfully onboarded and added to inventory

**Host Onboarding** Used to define the host authentication type (static or dynamic) and assign host pools (wired and wireless) to various VNs

### Assurance workflow

**Dashboard** Used to monitor the global health of all (fabric and non-fabric) devices and clients, with scores based on the status of various sites

**Client 360** monitor and resolve client-specific status and issues

**Devices 360** monitor and resolve device-specific status and issues. Option to run all suggested commands directly in DNA center (see 3rd image below)

**Issues** monitor and resolve open issues both reactive and proactive

AP sensors 1800,2800,3800 and wall-mounted are additional devices that can be deployed in the network to continuously test wireless, authentication and application performance

Command Runner is a DNAC feature, that allows network administrators or engineers to remotely execute commands on multiple network devices simultaneously

Contextual data - provides additional information or context to help understand key data and events in the network

With this contextual data, the Cisco DNA Center can provide detailed analysis and recommendations to solve problems or optimize network operations

![](<../.gitbook/assets/Unknown image (63)>)

![](<../.gitbook/assets/Unknown image (64)>)

![](<../.gitbook/assets/Unknown image (65)>)

![](<../.gitbook/assets/Unknown image (66)>)

### Comparative analytics (peer comparison)

Traditional fault-detection engines often rely on simple rule-based systems that follow a straightforward "if A, then B" logic. These systems are effective for detecting known issues and straightforward scenarios but can struggle with more complex, subtle, or previously unknown problems.

For instance, Cisco Catalyst Center incorporates a feature that is known as Cisco AI Network Analytics. This feature significantly enhances traditional fault-detection capabilities by using artificial intelligence (AI) and machine learning (ML) technologies. This way, it provides a more sophisticated approach to network monitoring and fault detection.

The Cisco AI Network Analytics Peer Comparison feature conducts a comprehensive assessment of the network's performance, comparing it to the performance of peer networks. Peers refer to other customers who possess wireless networks that are comparable in size and setup to one's own network. These peer networks serve as points of comparison for evaluating the performance of one's network in relation to others within a similar context. By analyzing the Key Performance Indicator (KPI) values of peer networks alongside one's own, network administrators can gain valuable insights into how their network measures up in terms of performance. They can also identify areas for improvement based on industry benchmarks and best practices. This comparative analysis helps administrators understanding the relative strengths and weaknesses of one's network and enables informed decision-making. This Cisco AI Network Analytics Peer Comparison functionality facilitates a detailed analysis of KPIs across specific days of the week or for the entire week. By comparing KPI metrics with those of peer networks, administrators can gain deeper insights into their network's performance trends and relative standings.

Upon opening the Peer Comparison window, users can access the following components:

KPI selection: A drop-down menu enables users to choose a KPI. There are several options such as Radio Throughput, Cloud Apps Throughput, Radio Resets, Packet Failure Rate, Interference, Onboarding Error Source, Roaming Error Source, and Received Signal Strength Indicator (RSSI). The default selection is RSSI, which is used to quantify the strength of a received wireless signal and represents the power level of the signal as detected by the receiving device, such as a wireless access point (AP) or a client device. It is important for network administrators to often monitor RSSI values as part of their network management and troubleshooting efforts. This way, they can ensure optimal wireless connectivity and identify potential areas of signal degradation or coverage issues within the network. Adjustments to network infrastructure, such as repositioning access points or optimizing antenna configurations, may be made based on RSSI measurements to improve overall network performance and reliability.

Show: Users can specify the time interval for comparing KPI values between their network and peer networks. The default setting is All Days, which provides a comprehensive analysis across the week.

Summary section: AI Network Analytics analyzes bar graphs and offers a summary of the findings for both the 2.4 GHz and 5 GHz band frequencies.

Peer Comparison bar graph: This graph initially showcases KPI values for the user's network in the 2.4 GHz and 5 GHz band. Clicking the Highlight Peers button shifts the focus to display KPI values for peer networks. For the following scenario in the graph, the blue color denotes the user's network, while the pink color indicates peer networks.

The key enabler to Cisco Catalyst Assurance is analytics—the ability to continually collect data from the network and transform it into actionable insights. To achieve this, Cisco Catalyst Center collects a variety of network telemetry, in traditional forms (SNMP, NetFlow, syslogs, and so on) and also emerging forms (NETCONF, YANG, streaming telemetry, and others). Cisco Catalysts Assurance then performs advanced processing to evaluate and correlate events to continually monitor how devices, users, and applications are performing.

Correlation of data is key since it allows for troubleshooting issues and analyzing network performance across both the overlay and underlay portions of the SD-Access fabric. Other solutions often lack this level of correlation and thus lose visibility into underlying traffic issues that may affect the performance of the overlay network. By providing correlated visibility into both underlay and overlay traffic patterns and usage through fabric-aware enhancements to NetFlow, SD-Access ensures that network visibility is not compromised when a fabric deployment is used.

### Notes

{% hint style="info" %}
* If you use tags to filter templates, apply the same tags to the target device. Otherwise provisioning fails with: “Cannot select the device. Not compatible with template.”
* When a device is in **Install Mode**, Cisco Catalyst Center cannot upload its software image directly from the device.
* If the enterprise interface default gateway was configured incorrectly during install, run `sudo maglev-config update`.
{% endhint %}
