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

# Cisco Hardware Portfolio

#### Unique Device Identifier (UDI) / Product Identifier (PID)

Unique Device Identifier (UDI) / Product Identifier (PID) is unique of each part number (appliance, slot, module, processor or license). Also referred in datasheets as Part number or model

| show diag all eeprom | displays the PID, VID, PCB serial number, hardware revision, and other such information |
| -------------------- | --------------------------------------------------------------------------------------- |

#### EOS/EOL and replacements

Once the device and its license is in End-of-Sale (EOS) or End-Of-Life (EOL), go through the EOL/EOS datasheet where the replacement is mentioned in the table.

It may happen that you will still see the EOS/EOL device or its license in the CCW, but that does not mean that it will be available for purchase, so refer the customer directly to purchase the official replacement

### Design considerations

Always prefer to choose same platforms when arranging presales solutions (e.g customer wants switches with different port density - dont choose one from 1200 and another from 1300)

### Cisco Catalyst 9000 (9k) enterprise switches

are L3 access-layer switches with redundant power supplies and optional modular uplinks.

9400 are modular multi-slot switches, we can purchase and insert new network cards to expand the number of ports

9200CX are compact models, that cannot be stacked, 9200 cannot be managed through meraki dashboard, only added and monitored, only 9300 and higher series can be managed

9k Switches without L have modular uplinks

Note Devices with SKU in CCW ending with -E are supplied with Network Essentials license and -A devices are supplied with Network Advantage license

Switching capacity measures the total capacity that a switch can receive, store and process

Forwarding rate refers to how quickly the switch can forward packets out of a ports

Apphosting - the device is able to run third-party applications such as Thousandeyes or ASA separately and act as a firewall or server on which these applications run

Supported on Catalyst 9400 and 9500

#### PoE capabilities (Catalyst 9k)

● IEEE 802.3at PoE+ (up to 30W per port) is supported on Cisco Catalyst 9200 Series switches to lower

the total cost of ownership for deployments that incorporate Cisco IP phones, Cisco Aironet wireless

access points, or other standards-compliant PoE+ end devices. PoE+ removes the need to supply wall

power to PoE-enabled devices and eliminates the cost of adding electrical cabling and circuits that

would otherwise be necessary in IP phone and WLAN deployments. With Cisco Catalyst 9200 Series

switches, PoE+ power allocation is dynamic, and power mapping scales up to a maximum of 1440W of

PoE+ power.

● IEEE 802.3bt Class 6 and Cisco UPOE (up to 60W per port) is supported on Catalyst 9200CX Series

mGig model. This facilitates delivery of network power to devices requiring higher power.

● PoE Powered Device (PD) - Catalyst 9200CX-12T-2X2G can be powered through the uplink with IEEE

802.3bt class 6 or UPOE+ power from upstream switch.

● Perpetual PoE is supported on Cisco Catalyst 9200 Series switches, and maintains the PoE+ power

during a switch reload. This is important for critical endpoints such as medical devices and for Internet of

Things (IoT) endpoints such as PoE-powered lights, so that there is no disruption during a switch reboot.

Fast PoE: When power is restored to a switch, Fast PoE starts delivering power to endpoints without waiting for the operating system to fully load, thereby speeding up the time for the endpoint to start up

Full PoE can deliver the maximum amount of power specified by the PoE standard, which is currently 30 watts (802.3bt standard or PoE++)

Partial POE switch has lower power budget, but can support either POE or POE+, not all ports can simultaneously deliver full PoE/PoE+/UPoE power — either because

#### Stacking licensing (Catalyst 9k)

Vertical Stacking - Classic StackWise is supported in Essential license, only switches from the same series can form a stack

Horizontal Stacking - StackWise Virtual requires Advantage license

### Cisco Catalyst 1000 / 1200 / 1300 and CBS

Cisco Catalyst 1000 series are EOL - will be EOS in 2025

Cisco Catalyst 1200,1300 series are managed L3 switches for small business all 10G options support 100/1000 Ethernet

Cisco Business Series (CBS) affordable series - all will be EOL/EOS soon

### Cisco Integrated Services Router (ISR) enterprise routers

combines routing, switching, Wi-Fi, integrated security, and DSL and LTE uplink connectivity options in a single, lightweight, high-performance device. Choose between the traditional Cisco IOS® XE or controller-managed Cisco IOS XE SD-WAN software to deploy your network quickly and reliably on all Cisco ISR 1000 Series routers.

Designed for small to medium-sized businesses and enterprise branch offices

ISR 800,900,4000 series

Are EoS

| show platform hardware throughput level | To determine the current throughput level of the router |
| --------------------------------------- | ------------------------------------------------------- |

### Cisco Catalyst 8000 edge platforms

are modern routing and security platforms running on IOS-XE offerring advanced routing, SD-WAN, security, wireless and application optimization capabilities, making them ideal for connecting branch offices and remote locations securely and efficiently

8200 and 8300 are licensed either with catalyst routing essentials with basic routing capabilities with unlocked 2Gbps throughput (inc crypto IPsec) or as DNA essential and advantage (including network stack essentials and advantage) that requires additional Tier licensing, which unlocks the throughput of the forwarding and IPsec.

Note for 8200/8300 - Don't forget to add embedded support in CCW when choosing catalyst routing essentials licence

The Tier dictates the throughput of encryption, not the entire IPv4 forwarding - which is unlocked to the maximum value from datasheet when DNA license is purchased

E.g When you buy DNA advantage license with a specific tier - you will get the Network advantage license aswell as the throughput for encrypted traffic of the tier including the maximum IPv4 forwarding throughput of the system in datasheet

Same appies to 8500, where you must choose either DNA Advantage or Premier to get the feature stack and unlock the full IPv4 throughput of the system (including the throughput of the purchased Tier for crypt)

| show platform hardware throughput crypto | To determine the current throughput level of the router. Use keyword level for virtual router |
| ---------------------------------------- | --------------------------------------------------------------------------------------------- |

### Meraki

Ohledne kompatibility Meraki Dashboardu a Catalyst portfolia – existuji dve moznosti:

Cloud Monitoring – Catalyst prvky jsou monitorovany Cloud Dashboardem ale konfigurace se resi pres SSH/CLI/GUI jednotlivych boxu.

Podpora switchu je siroka viz zde -> [https://documentation.meraki.com/Cloud\_Monitoring\_for\_Catalyst/Onboarding/Supported\_Catalyst\_9000\_Series\_Switches\_(Cloud\_Monitoring)](https://documentation.meraki.com/Cloud_Monitoring_for_Catalyst/Onboarding/Supported_Catalyst_9000_Series_Switches_\(Cloud_Monitoring\))

Cloud Management – Plny management a monitoring. Lokalni management/monitoring primo na boxu NENI mozny. Support pro WiFi CW916x radu, u switchu zatim C9300 rada.

Zatim vsechny modely C9300 podporujici management zde -> [https://documentation.meraki.com/MS/Deployment\_Guides/FAQs\_Migrate\_to\_Meraki\_management\_mode](https://documentation.meraki.com/MS/Deployment_Guides/FAQs_Migrate_to_Meraki_management_mode)

Je samozrejme jasne ze portfolio Catalyst ktere bude manageovatelne z Cloud Dashboardu se bude rozsirovat. Nicmene co se tyka switchu tak Meraki ma svoje stavajici portfolio ktere je obdobne popr. sirsi jako Cisco Catalyst

### Service provider routers (MIG)

#### Which mass-scale router to choose

Density, power, form factor and temperature range, MD scale, pricing, dual RP, features

If NCS-540 family does the job, go for it.

More density/scale leads you to NCS-5500.

Another step in density/scale gets you to NCS-5700

NCS-560/NCS-57C3 for medium dual RP boxes

8000 for core/peering 400G applications

ASR9K for the rest

### Cisco Advanced Services Router (ASR)

high-density and high-performance routers designed for service providers and large enterprise networks with Cisco proprietary Hardware

ASR 1000 series are routers with IOS-XE designed for SDA and SDWAN (also 90X routers run IOS-XE)

ASR 900 Series routers are built as fully modular systems with IOS-XR (can run XE aswell). The router chassis supports online field replacement and upgrades of all components. The Cisco ASR 907 Router is designed to contain one fan tray, up to three power supplies, two Route Switch Processor (RSP) cards, and up to 16 interface module cards. The Cisco ASR 903 Router is designed to contain one fan tray, two power supplies, two RSP cards, and up to six interface module cards. The ASR 902 Router uses the same design as the ASR 903 Router, but due to its smaller size, it has four interface module cards and one RSP card. All components support online replacement and field upgrades, with the exception of the RSP card on the ASR 902 Router, which requires the system to be brought down for a replacement or upgrade

920 series IOS-XE

9000 series large scale systems with IOS-XR. Designed for peering, DCI,Backhaul networks

Note Cisco will plan to terminate this platform since Cisco 8k offers more future-proof performance and will be the main, since it's based on Cisco proprietary silicon one chipset

### Network Convergence System (NCS)

are based on Broadcom processor with IOS-XR, supporting apphosting

Non-official note Since this platform is based on non-proprietary chip, Cisco will prioritize 8k series and ASR and probably will terminate NCS in the future

Base platforms without external-TCAM, relying only on the on-chip resources available for feature scale

Scale platforms equipped with external-TCAM (eTCAM) used to provide extended scale in addition to the on-chip scale

![](<../.gitbook/assets/Unknown image (540)>)

500 series are resilient XR-based service provider routers, which makes them suitable for outdoor aggregation deployments with the optional apphosting

They are designed for ultra-low-latency and high capacity networks, supporting architectural flexibility for 5G wireless networks, Mobile Broadband, IoT, Carrier Ethernet, or WAN MPLS.

520 (IOS-XE) - meant for ethernet access

540 - small, medium and large density with fronthaul router

560 modular SP edge routers

6000 series - core

5000, 5500, 5700 series - are high-density, high-scale Aggregation/Edge routers designed for pre-aggregation and aggregation mass scale or CDN networks

There are fixed 5500 and 5700 and modular chassis 5500 with 5500 and 5700 line cards

Modular has orthogonal chassis design (kolmý), so that each fabric module is connected to all linecards cards, eliminating the need for the midplane. Each line card have multiple forwarding ASICs

### NCS 2000 series (optical transport)

The NCS 2000 series is a traditional solution with a long history (15+ years) for optical networks and can be used in building metro, regional, national or long haul optical networks. The NCS 2000 solution has been using ROADM technology since its inception. The NCS 2000 solution is very flexible and it is possible to build networks with basic ROADM functionality up to complex solutions using "colorless", "omnidirectional", "contentionless add/drop", multi-directional ROADM nodes including flex spectrum technology. The NCS 2000 solution can be used both in the traditional all-in-one solution architecture, where both the optical and electrical (transponder) layers are provided within one solution, and in modern solutions built on the separation of the optical and electrical layers (in such cases, the NCS 2000 is most often used in the optical layer). In terms of transmission speeds provided by the electrical layer (transponders) within the solution, it is possible to transmit 100Gbs, 200Gbps, 400Gbps signals in one lambda. In case of requirements for higher transmission speeds, it is possible to combine the NCS 2000 solution with the electrical layer of other series, for example NCS 1000.

It is a high-performance, scalable optical transport platform supporting up to 9.6 Tbps of DWDM capacity. It is part of Cisco’s Intelligent Optical Network architecture and integrates seamlessly with Cisco routing and switching solutions. Key features include advanced efficiency through coherent optics and flex-grid, high reliability with built-in redundancy, scalability for evolving network demands, and flexibility to support diverse topologies and services.

Applications of Cisco NCS 2000:

Metro and Access Networks: High-capacity backhaul, data center interconnect (DCI), and business services like Ethernet Private Line and Fibre Channel over Ethernet.

Regional and Long-Haul Networks: Long-distance transport, inter-regional connectivity, and cloud connectivity with providers like AWS and Azure.

Core Networks: Internet backbone, national research and education networks (NRENs), and content delivery networks (CDNs).

Emerging Use Cases: High-capacity 5G networks, IoT device connectivity, and AI/ML data transport.

![](<../.gitbook/assets/Unknown image (541)>)

### NCS 1000 series

is a 2RU DWDM optical-electrical layer solution with four xponder/muxponder module slots that is mechanically optimized to maximize capacity while minimizing space and power requirements.

It supports a wide range of services, including:

100 GE, 200 GE, and 250 GE Ethernet

OTN

Fibre Channel

Storage Area Networks (SANs)

High density: Up to 250 Tbps of capacity per rack unit

High bandwidth: Up to 10 Tbps per wavelength

Programmable: Software-defined networking (SDN) and network function virtualization (NFV) capabilities

Scalable: Can be scaled from a few ports to thousands of ports

Flexible: Supports a wide range of services and applications

Modular design: Allows the platform to be easily scaled and customized.

1004:

![](<../.gitbook/assets/Unknown image (542)>)

#### NCS 1010

Ideal for: High service demands or fiber constraints, with L band support.

Target Areas: Long-Haul (LH), Ultra-Long-Haul (ULH), and subsea.

Customers: Service Providers, web, OTT, and smaller clients valuing automation and

simplicity.

Features: Next Generation Open Line System with RON and automation.

NCS 2000

Suitable for: Greenfield and brownfield opportunities for all customer sizes.

Optimized for: Access, Metro, Regional, and also LH, ULH, DCI.

Focus: Low rate requirements and Routed Optical Networking.

### NCS 4000 series

High-capacity, multi-service transport platform.

Supports IP, MPLS, OTN, and packet switching.

Targeted at large-scale core networks and data center interconnects.

Modular and supports high throughput (up to Tbps scale).

Supporting SONET/SDH, Ethernet, and Channelized OTN; 10 Gigabit Ethernet (OTU-2), 40 Gigabit Ethernet (OTU-3), and 100 Gigabit Ethernet (OTU-4).

NCS 4000 and 4200 are generally considered legacy or mature platforms. Cisco is moving toward newer platforms like NCS 1000 for optical and NCS 5500/5700 for packet-centric routing.

### NCS 4200 series

Legacy optical metro networks based on SONET and Synchronous Digital Hierarchy (SDH) technologies set the standards for reliability, capacity, and efficiency in transporting Time-Division Multiplexing (TDM) traffic and carrying voice and data services. Service providers and carriers are nonetheless faced with many challenges and limitations imposed by these legacy networks and need alternatives for migrating their circuit-switched transport networks to future-proof packet-based networks.

The Cisco NCS 4200 Series addresses the legacy network inefficiencies by delivering a cost-effective, modular solution based on a protocol-independent fabric architecture. The Cisco NCS 4200 Series, as part of the Cisco Evolved Programmable Network (EPN) architecture, is capable of delivering unbounded scale and unmatched CEM and OTN capabilities over a redundant and protected packet-based network (Multiprotocol Label Switching \[MPLS]/FlexLSP and Segment Routing).

### Cisco 8000 series routers

are high-performance carrier peering routers designed for service providers and large-scale DC networks with 400G or 800G applications

They are powered by Cisco's IOS XR operating system, which is specifically optimized for high-performance, high-availability networking environments

8100 8200 Fixed is a standalone 1RU router with single/non-redundant data plane NPU ASIC chip and an RP CPU-control plane

Router-on-Chip (RoC) architecture, which means that all of the routing functionality is integrated into a single ASIC. This makes the 8100 and 8200 Series routers more compact and less expensive than the 8800 Series routers. However, they also have lower performance

8800 Distributed is a modular chassis system, offered in 4 variants 4,8,12,18 slots, where line cards are inserted

ASICs are distributed on each line card with variety of port speed and density

Redundant route processors manage the control and management plane e.g performs route processing and distributes forwarding tables to the line cards

It also controls fans, alarms, and power supplies using an I2C communication link to each fan tray and power supply

All linecards and RP components are interconnected with the fabric cards (FC)

8600 Centralized is a new modular chassis with improved flexibility and efficienty, providing optional RP and switch card (SC) redundant setup

Modular port adapters (MPA’s) are inserted into the centralized system, which are modular network adapters with a various port density designed solely for forwarding, without it’s own processor, which is more power efficient in compare to the modular distributed system, where line cards with their own processor have higher port density, that have to be powered despite that only a few of them are actively used

The Switch cards are connected and controls individual MPA’s and they are responsible for all forwarding decisions and packet processing

It contains the Cisco Silicon One Q200 with HBM of up to 8GB

Switch cards (SC's) and Route processors (RP’s) are interconnected and form centralized control and data plane without the need of fabric cards as it is in distributed system

RP’s distributes the forwarding calculations and configurations to the the Switch cards

The centralized system is intended for customers who want a modular system, but do not plan to use the capacities and power supply requirements of a distributed system

It has orthogonal chassis design (kolmý), so that each switch cards is connected to all modular port adapters, eliminating the need for the midplane

Redundant or non-redundant setup

Up to 10 ms traffic drop is expected during active RP/SC Failover. No traffic loss during standby RP/SC reload

These MPAs are CPU-less, they have the PHY and Optics components for network connectivity to other devices in the network

Each MPA connects to both Switch Cards (SC0, SC1)

The PHY on the ingress MPA bi-cast the packet to both the Switch cards and the both Q200 ASICs in the switch cards processes the packet, so that in the event of the failure of the active switch card, the standby switch card have all the packets.

Both of the switch cards then forward the packet to the egress MPA and the packet of the standby is filtered by the PHY and only the packet from the active is sent out of the respective egress port in the MPA

Cisco sillicon Q100, Q200 and P100 include HBM located on the chip package which is connected to the Cisco silicon One ASIC via an ultra-fast silicon interface

Port side intake vs. Port side exhaust

FAN-PI-V3 Cisco 8000 FAN - port-side intake

FAN-PE-V3 Cisco 8000 FAN - port-side Exhaust

Standard-airflow fan: This option provides front-to-back airflow, with ports aligned with the hot aisle (port-side exhaust). This option is ideal for ToR deployments to align the fabric extender ports with the server ports in a given rack

Reversed-airflow fan: This option provides back-to-front airflow, with ports aligned with the cold aisle (port-side intake). This option is ideal for network rack deployments in which various network equipment ports need to be aligned

![](<../.gitbook/assets/Unknown image (543)>)

![](<../.gitbook/assets/Unknown image (544)>)

#### SD-WAN

![](<../.gitbook/assets/Unknown image (545)>)

### Virtual and cloud routers

#### Cloud Services Router 1000 series (CSR1000)

is a virtual router that runs on virtualized environments such as VMware, AWS, or Microsoft Azure. It offers advanced networking features and can be deployed in cloud-based or on-premises environments, providing flexible and scalable routing solutions

Cisco Catalyst 8000V Edge Software

Cisco Cloud Services Router 1000V Series

Cisco IOS XRd vRouter

### Industrial Ethernet switches and routers

IE is a robust series that is designed to operate under abnormal external conditions

IE switches have I/O ports, allowing to perform electrical signaling to control or monitor external IoT equipment

### Network Functions Virtualization (NFV) and uCPE

is a Cisco Enterprise NFV Infrastructure Software installed on a below platforms

Cisco 5000 Series Enterprise Network Compute System (ENCS) combines the functionality of a traditional router and a traditional server

Cisco Catalyst 8200 and 8300 Series Edge Universal Customer Premises Equipment (uCPE)

is a the result of virtualization technologies, it is a general purpose “white box” networking device that uses software to run VNFs on a single standard x86-based hardware replacing traditional hardware-based CPE.

Traditionally, telecommunications and networking services have been provided to customers with specialized hardware devices that perform dedicated network functions (such as a router, firewall, WAN accelerator or wireless LAN controller). For each function, the service provider needs to source, configure, install, test and maintain a separate device at the customer premises, which is not only time and resource consuming, but also expensive

uCPE allows for all these different functions such as router, switch or firewall to be deployed on a single, standard hardware device. And once it has been delivered and installed at the customer’s premises, new networking services or functionality can be delivered to the customer on demand as they are developed, without needing to send a technician to install a new piece of hardware or remove an old one. This is because these functions are all now software-based, and can be virtually deployed and updated, helping reduce both CAPEX and OPEX for the network operator, who can then pass on these cost savings to the customer

### Licensing

#### Licensing models

Subscription licenses: Provide software access for a set subscription period, offering fast access to updates and predictable costs.

Perpetual licenses: Offer indefinite software access, typically tied to a device, with additional fees for support and maintenance.

#### Purchasing options

Software Buying Programs: Economies of scale and simplified license management through Cisco's programs like Enterprise Agreement (EA), Managed Service License Agreement (MSLA), and Service Provider Networking Agreement (SPNA).

Transactional: Both perpetual and subscription licenses available à la carte according to Cisco's price list.

#### License deployment and management

Product Activation Keys (PAK) Licensing Configures devices individually using PAKs , with limited visibility of owned licenses.

Cisco Smart Software Licensing licenses are managed centrally through the cloud web server Cisco Smart Software Manager (CSSM) or Cisco Smart Licensing portal

It gives visibility to what you have purchased and what you are using

Smart Licensing establishes a pool of software licenses that can be used across your company - no more entering Product Activation Keys

Cisco smart account is created and managed on: [https://software.cisco.com/](https://software.cisco.com/)

Limited Smart account is meant for small businesses that don't have their own @xxx email domain and use public @gmail @yahoo

Virtual Smart Accounts provide additional segmentation capabilities for organizing licenses and resources within larger Smart Account

Can be used to group licenses for a specific location or for specific customer if you use cisco smart account as a service provider

Furthermore we can tag particular licenses for better tracking and visibility

![](<../.gitbook/assets/Unknown image (546)>)

#### Smart Licensing Using Policy (SLP)

is an evolved version of Cisco Smart Licensing, where licenses can be dynamically assigned to devices based on the defined policies, rather than manually allocating licenses to individual devices.

When a device connects to the network or undergoes a change in configuration, the Smart Licensing infrastructure evaluates the applicable policies and automatically assigns the appropriate licenses to the device.

Once you receive the product, it will directly boot up instead of going into evaluation mode

SLP enables license mobility and flexibility by allowing licenses to be moved or transferred between devices based on policy-driven rules.

Cisco Smart Licensing Utility (CSLU) If a Product instance (PI - purchased device with license) cannot communicate directly with the cloud web server CSSM due to customer's security reasons, it can use CLU which is a lightweight Windows application that is used to pull software use data from the Cisco device and report the software use to CSSM (acts as a proxy/aggregator). CSSM can be also deployed on Prem

It can be deployed as standalone micro service or be integrated as software component with controller-based products.

If this still doesn't suit the environment, Cisco expose API's that can be used by the customer to report licensing

![](<../.gitbook/assets/Unknown image (547)>)

![](<../.gitbook/assets/Unknown image (548)>)

#### Specific License Reservation (SLR)

Specific License Reservation (SLR) is a functionality that enables you to deploy a software license on a device without communicating usage information to Cisco

This functionality is especially used in highly secure networks, and it is supported on platforms that have Smart Licensing enabled.

#### Catalyst 8000 licensing matrix

[https://www.cisco.com/c/m/en\_us/products/software/sd-wan-routing-matrix.html?oid=otren019258](https://www.cisco.com/c/m/en_us/products/software/sd-wan-routing-matrix.html?oid=otren019258)

Catalyst Routing Essentials license is a tailor-made entry-level Base feature package intended for Traditional Routing deployments with Catalyst Edge 8300 and 8200 series routers

The Catalyst Routing Essentials license is only offered as a 7-year term-based subscription. There is no bandwidth tiering necessary for Catalyst Routing Essentials. The platform-class is reflected in the new license SKU.

What happens if a customer fails to renew the routing license after 7 years, will the router stop working?

The router will keep working with the entitlements for which it was purchased. There is no hard enforcement in the IOS-XE software, but the platform will be out of compliance in CSSM.

#### Catalyst 9000 licensing matrix

[https://www.cisco.com/c/m/en\_us/products/software/dna-subscription-switching/en-sw-sub-matrix-switching.html](https://www.cisco.com/c/m/en_us/products/software/dna-subscription-switching/en-sw-sub-matrix-switching.html)

Network Essentials offers baseline networking capabilities, including essential switching capabilities, technical support, manual software updates, and zero-touch provisioning

Another primary reason to subscribe to Essentials is that campus networks do not require advanced dynamic routing protocols like BGP

These protocols are primarily necessary for data centers and multi-branch organizations. Other protocols, such as EIGRP, RIP, and OSPF, are supported to some extent.

Network Advantage includes essential features with additional capabilities like full L3 protocol support such as BGP, analytic tools, better technical support, and software patches

| switch(config)#license boot level \[network-essentials \| network-advantage] \[addon] \[dna-advantage] | You can change the license level on the switch There are two levels - network-essentials & network-advantage, with the DNA add-on's of dna-essentials & dna-advantage |
| ------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

DNA Essentials provides basic network management, security features, network automation and assurance capabilities, such as automated device onboarding, network segmentation, and network performance monitoring.

DNA Advantage is an upgrade from the Essentials license. It contains all the features offered by DNA Essentials. It is a software license that provides advanced network management and security features for Cisco enterprise networks

It offers advanced security features such as threat detection and response, network segmentation firewall, and software-defined access.

### Support and operations

| show tech                                 | generates comprehensive diagnostic data from a device, that can be analyzed and provided to cisco TAC - may take up a few minutes log CLI session to capture all data in text, since it is printed directly in a sequence. Insert the show tech file into the cisco CLI analyzer to get the comprehensive details of vulnerabilities and issues with the device |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show tech \| redirect \<bootflash: tftp:> | to generate show tech as a file on bootflash or tftp server                                                                                                                                                                                                                                                                                                     |

Note crashinfo can be read only by TAC, as they have their internal tool to decode the crash log

Partner support service (PSS) Cisco service support is provided to the customer by intermediary provider (such as we Alef), which is responsible for opening TAC cases

Cisco Smartnet Cisco provides support to the customer directly, so that customer can open Technical Assistance Center (TAC) case directly

![](<../.gitbook/assets/Unknown image (549)>)

#### SKU (Stock Keeping Unit)

handled by sales

refers to a specific identifier assigned to a service offering or subscription provided by Cisco

This SKU distinguishes the service level, duration, features, and support options associated with the subscription

Cisco SKUs for service levels help customers easily identify and purchase the appropriate service package tailored to their needs, whether it's technical support, software updates, or other service-related offerings provided by Cisco

#### Return Materials Authorization (RMA)

Return Materials Authorization (RMA) is the process wherein organizations return faulty or broken assets to the manufacturer or vendor [RMA](https://ibpm.cisco.com/rma/home/) [SCM](https://mycase.cloudapps.cisco.com/case) - zaklada Marek Berka

#### Field Notices

Field Notices are Cisco's way of notifying customers and partners about significant issues in Cisco products that may require upgrades, workarounds, or other actions.

Field Notices serve a specific purpose and differ from other Cisco notifications such as Security Advisories, Software Advisories, and End-of-Life Notice

Serial Number Validation (SNV) Tool: Cisco provides a Serial Number Validation (SNV) Tool within Field Notices to help customers determine if their product is affected by the issue described. Customers can use this tool to input serial numbers and verify if they are affected

#### Smart Bonding API

Smart Bonding API an easy way to connect a Cisco Partner's ITSM (Information Technology Service Management) system with Cisco's support ticketing system for the purpose of ticket synchronization

#### RADKit

RADKit is a Software Development Kit (SDK) a set of ready-to-use tools and Python modules allowing efficient and scalable interactions with local or remote equipment

![](<../.gitbook/assets/Unknown image (550)>)

### Provisioning a new device

Verify DNS server installed (sh run | s name-server) and check if DNS works

ping tools.cisco.com

smartreceiver.cisco.com

ICMP might be disabled and only http/https is allowed, try:

| telnet tools.cisco.com 443 /ipv4 | If port is open, then DNS and connectivity works |
| -------------------------------- | ------------------------------------------------ |

Enable smart-licensing:

| License smart enable                                                                                                             |   |
| -------------------------------------------------------------------------------------------------------------------------------- | - |
| license smart url [https://smartreceiver.cisco.com/licservice/license](https://smartreceiver.cisco.com/licservice/license)       |   |
| license smart url smart [https://smartreceiver.cisco.com/licservice/license](https://smartreceiver.cisco.com/licservice/license) |   |
| license smart transport smart                                                                                                    |   |

Register for token - from SA

| license smart trust idtoken all force |   |
| ------------------------------------- | - |

Verify if successfully registered

| show license all     | There should be "Trust Code Installed" |
| -------------------- | -------------------------------------- |
| show license summary |                                        |

Proxy deployment - device doesn't have access to internet

| license smart proxy address proxy.justice.cz |   |
| -------------------------------------------- | - |
| license smart proxy port 3128                |   |
