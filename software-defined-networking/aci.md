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

# ACI

### Application Centric Infrastructure (ACI)

**Application Centric Infrastructure (ACI)** is an SDN solution from Cisco for Data Centers, simply ACI is a Network policy-based automation model solution developed by Cisco.

Cisco Network Services Orchestrator (NSO) is industry-leading software for automating services across traditional and virtualized networks.

Cisco ACI is a shift in data center architecture to integrate physical and virtual elements in an open ecosystem model utilizing APIs, open standards, and Open Source elements

It is Cisco enhanced VXLAN, which enables the automation and orchestration of network infrastructure to meet the requirements of modern data centers and cloud environments.

ACI is a layer 3 fabric, where either OSPF or IS-IS is used as routing protocol to build the routing table.VXLAN is used for building Overlay Network.

![](<../.gitbook/assets/Unknown image (834)>)

![](<../.gitbook/assets/Unknown image (835)>)

### ACI components

#### Application Policy Infrastructure Controller (APIC)

**Application Policy Infrastructure Controller (APIC)** is the centralized management and automation platform that provides a single point of control for the entire infrastructure. It allows administrators to define policies and automate network provisioning, security, and optimization across the data center. Configuration is stored in XML or JSON format, these can be configured using APIs as well.

If all APICs go down, there will be no negative impact or outage in the environment because policy is already received by the fabric switches and APIC does not sit in the forwarding plane.

APIC is connected to Leaf Switch. Cisco recommends having minimum 3 APIC servers in odd numbers 3,5,7.

#### Spine and leaf switches

ACI uses spine and leaf Nexus 9000 switches in 2-Tier architecture. ECMP is in place

Spine switches are the core and they connect to the leaf switches, which, in turn, connect to the endpoints such as servers, VMs, ...

NXOS mode device will be managed individually

ACI Mode switch will be managed using APIC. Both modes are mutually exclusive i.e. switching between the mode require deleting of complete configuration.

#### Application Network Profile (ANP)

**Application Network Profile (ANP)** is a logical grouping of network policies that define how a particular application should be treated within the network. ANPs can include policies related to security, QoS, and network connectivity.

#### Endpoint Groups (EPGs)

**Endpoint Groups (EPGs)** are logical groupings of endpoints that share the same network policies. EPGs can include endpoints such as servers, virtual machines, or other network devices.

#### Contracts

**Contracts** define the communication policies between different EPGs within the network. Contracts can include policies related to security, QoS, and network connectivity.

### ACI operation

Define the Application Network Profile (ANP): The ANP defines the network policies required for the application. It includes policies related to security, QoS, and network connectivity.

Create Endpoint Groups (EPGs): EPGs are created to group the endpoints that share the same network policies

Define Contracts: Contracts are defined to specify the communication policies between different EPGs within the network.

Configure Spine and Leaf Switches: Spine and leaf switches are configured to provide connectivity between endpoints in the network.

Deploy the Application: Once the ANP, EPGs, contracts, and switches are configured, the application can be deployed. ACI automatically manages the underlying resources to ensure the application runs as intended

### ACI benefits

Simplified Network Management: ACI provides a centralized management platform that simplifies network management and reduces operational costs.

Automated Provisioning: ACI automates network provisioning, enabling organizations to rapidly deploy and scale applications.

Improved Visibility: ACI provides better visibility into network traffic and application performance, making it easier to troubleshoot issues.

Consistent Security: ACI provides consistent security policies across the entire network, ensuring that applications are protected from cyber threats.

Scalability: ACI is designed to scale to meet the requirements of modern data centers and cloud environments, making it an ideal solution for organizations of all sizes.

### Fabric discovery and protocols

**Discovery Protocol:** When a new ACI fabric is deployed, the leaf switches send out a discovery message to the spine switches to identify themselves. The spine switches then respond with their own discovery message, and the leaf switches store the spine switch information.

**CDP and LLDP:** The fabric uses to discover and exchange information about the network topology. This information is used to build a network topology map that is used by the APIC to manage the fabric.

VXLAN protocol is used to encapsulate and transport the traffic between endpoints in the ACI fabric. VXLAN provides a scalable way to extend Layer 2 networks over a Layer 3 infrastructure.

BGP is used by the spine switches to exchange routing information between each other. BGP is used to distribute the IP prefixes that are associated with the various EPGs in the fabric.

OpFlex is a protocol used by the APIC to communicate with the leaf switches to configure the network policies. The APIC sends policy information to the leaf switches using OpFlex, and the leaf switches translate the policies into configuration commands that are applied to the network devices.

**ACI Policy Model:** The ACI fabric uses a policy model to define and enforce network policies. The policy model is based on the concept of Application Network Profiles (ANPs) and Endpoint Groups (EPGs). ANPs define the policies for a specific application, and EPGs are used to group endpoints that share the same policies.

### Management options

**APIC GUI:** is the centralized management console for the ACI fabric. The APIC GUI provides a web-based interface to configure and manage the fabric. It offers a comprehensive view of the network topology, network policies, endpoints, and various other aspects of the fabric. The APIC GUI is user-friendly and can be accessed from anywhere with an internet connection.

**APIC REST API:** ACI also provides a REST API that allows for programmatic configuration and management of the fabric. The APIC REST API is widely used by developers and automation tools to automate network configuration and management tasks.

**CLI:** ACI also provides a Command Line Interface (CLI) to configure and manage the fabric. The CLI is similar to the traditional Cisco IOS CLI and can be used to perform low-level configuration tasks on the fabric.

Automation and Orchestration Tools: ACI is designed to work with various automation and orchestration tools, such as Ansible, Chef, Puppet, and Terraform. These tools can be used to automate network configuration and management tasks, making it easier to deploy, scale, and manage the ACI fabric.

Third-party Management Tools: ACI can also be integrated with third-party management tools, such as network monitoring and security tools. This integration allows for better visibility and control over the network, enabling more efficient management and troubleshooting.

### Virtual Device Context (VDC)

**Virtual Device Context (VDC)** is a feature provided by some network devices, particularly Cisco Nexus switches. VDC networking allows the physical switch to be partitioned into multiple virtual switches, each operating independently with its own set of resources and configurations. It enables network administrators to logically separate and manage different network environments within a single physical device.

### Troubleshooting notes

Operations > EP Tracker > insert IP or MAC in question (if the entry is flapping there might be issue with interface > tier2 to be advised)

// take a snip of endpoint entry and write this

down to notepad, as you will be taking the snip

in citrix and it allows you to open desktop and open notepad

![](<../.gitbook/assets/Unknown image (836)>)

Navigate to section Fabric and in inventory search for node

2213 > Interfaces > VPC Interfaces > 213 > then you have to

open up thos entries to find VPC with that interface

then investigate in those available sections

then proceed to do same health checks for 2214

Interfaces > physical interfaces > eth 1/8

Endpoint entry on the right snip confirms following:

Learned 2213/2214

vPC number #213

Connected to interface 1/8 (10G)

Section Visibility & Troubleshooting for traceroute

between targets (you have to click on entry)

![](<../.gitbook/assets/Unknown image (837)>)
