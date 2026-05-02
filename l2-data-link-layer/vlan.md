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

# VLAN

VLANs split a switch into multiple Layer 2 broadcast domains.

To understand VLANs, you need a solid understanding of LANs. A LAN is a group of devices that share a common broadcast domain. When a device on the LAN sends broadcast messages, the switch floods the broadcast messages (as well as unknown unicast) to all ports except the incoming port. Therefore, all other devices on the LAN receive them. You can think of a LAN and a broadcast domain as being basically the same thing. Without VLANs, a switch considers all its interfaces to be in the same broadcast domain. In other words, all connected devices are in the same LAN. With VLANs, a switch can put some interfaces into one broadcast domain and some into another. The individual broadcast domains that are created by the switch are called VLANs.

### Virtual LAN (VLAN)

**Virtual LAN (VLAN)** is a L2 technology enabling to virtually separate physical switch ports, separating them into different LAN segments, different LAN segment. This allows a switch to create several separate LANs on a single switch

Each VLAN is a separate Layer 2 broadcast domain that is usually mapped to a unique IP subnet (Layer 3 broadcast domain). A VLAN can exist on a single switch or span multiple switches.

Each port on the switch can be assigned to a different VLAN with its IP subnet range (One VLAN one IP subnet)

Users in one VLAN can communicate only with users in the same VLAN and cannot communicate with users in another VLAN

Each VLAN is a separate broadcast domain, so the switch forwards incoming broadcasts only out of the port belonging to the same VLAN

VLAN database and configuration is stored in the vlan.dat file in a flash memory

Within the switched network, VLANs can provide segmentation and organizational flexibility. You can design a VLAN structure that lets you group devices by functions, project teams, or applications without regard to the physical location of the users. VLANs also serve as the basis for network segmentation, which is one of the most important parts of implementing network security. They allow you to map Layer 2 broadcast domains to Layer 3 broadcast domains, which later allows you to implement access and security policies for specific groups of users.

Usually, subnet numbers are chosen to reflect which VLANs they are associated. The figure shows that VLAN 2 uses subnet 10.0.2.0/24, VLAN 3 uses 10.0.3.0/24, and VLAN 4 uses 10.0.4.0/24. In this example, the third octet clearly identifies the VLAN that the device belongs to. The VLAN design must take into consideration the implementation of a hierarchical, network-addressing scheme.

![](<../.gitbook/assets/Unknown image (635)>)

Each VLAN is distinguished by VLAN number (ID)

VLANs 1 and 1002–1005 are automatically created by the switch while the others have to be created manually.

0 Reserved for PCP

1-1001: Normal Range VLANs

1002-1005: Reserved VLANs

1006-4094: Extended Range VLANs

4095: Reserved

{% hint style="info" %}
Without VLAN the network would be a "Flat" network area - only one network on the switch, where all devices communicate with each other, which increases the broadcast domain and security risks, since all devices have to receive the broadcast
{% endhint %}

### Access ports

**Access port** is a port assigned to a single VLAN, where devices such as PCs, printers, or IP phones are typically connected to

When a device is connected to an access port, it becomes a member of the VLAN associated with that port.

For example, suppose a PC is connected to an access port configured for VLAN 20. In this case, the PC will belong to VLAN 20, and any traffic it sends or receives will be confined to VLAN 20. Similarly, if an IP phone is connected to the same access port, it can be assigned both the Access VLAN (untagged) and the Voice VLAN (already tagged), allowing for separate voice and data traffic

If a switch port is operating as an access port, it can be assigned to only one VLAN, which adds a layer of security. Multiple ports can be assigned to each VLAN. Ports in the same VLAN share broadcast domain, while ports in different VLANs do not share a broadcast domain. Containing broadcasts within a VLAN improves the overall performance of the network.

The end device connected to the switch has no knowledge of a configured VLAN on the switch. The configuration is only performed on the switch port. The end device has an IP address and subnet mask that associates it with a subnet. This subnet then maps to the VLAN that is configured on the switch port to which the end device is connected.

Access port: untagged in/untagged out (drops unexpected tags; never adds a tag on the wire)

### Trunk ports

**Trunk port** is a point-to-point link to another switch or router carrying traffic of multiple VLANs over a single physical link. Trunk ports use VLAN tagging protocols such as IEEE 802.1Q to differentiate between VLANs. When a frame enters a trunk port, the switch tags it with the appropriate VLAN ID based on the source VLAN of the frame.

If we wanted in the early VLAN deployments to connect users in the same VLAN on a different switch, we had to connect the switches with a link for each VLAN separately, which was inefficient and took up ports on the switch that could be used by stations

Trunk: tags frames on egress.

QinQ Dot1q-tunnel: preserves inner tag and adds an outer tag (Q-in-Q).

![](<../.gitbook/assets/Unknown image (636)>)

Trunking was developed to address the issue, as it allows to transmit data from all VLANs configured on a switch to the another switch with single link

Each frame entering the trunk gets a tag to which VLAN it belongs to

A trunk could also be used between a network device and a server or another device that is equipped with an appropriate trunk capable network interface card (NIC)

If your network includes VLANs that span multiple interconnected switches, the switches must use VLAN trunking on the connections between them. Switches use a process called VLAN tagging in which the sending switch adds another header to the frame before sending it over the trunk. This extra header is called a tag and includes a VID field so that the sending switch can list the VLAN ID and the receiving switch can identify the VLAN that each frame belongs to

Switch 1 adds a header that identifies the frame as belonging to VLAN 1. This header tells Switch 2 that the frame should be forwarded to the VLAN 1 ports. Switch 2 removes the header and then forwards the frame for all ports that are part of VLAN 1.

![](<../.gitbook/assets/Unknown image (637)>)

#### Why PC can't be connected to the trunk port

A typical PC network interface card (NIC) does not support VLAN tagging by default. PCs are designed to operate in access mode, where they expect untagged Ethernet frames.

When a PC is directly connected to a trunk port, it sends untagged Ethernet frames, assuming they will be accepted by the switch.

However, the switch expects tagged frames on the trunk port. In the absence of VLAN tags from the PC, the switch adds a VLAN tag to the frames, assigning them to the native VLAN configured on that trunk port. The native VLAN is a VLAN that is allowed to traverse the trunk without being tagged. Frames received on the native VLAN are not tagged when transmitted over the trunk link.

{% hint style="info" %}
An **access port expects untagged frames** and internally assigns them to its configured access VLAN. If a device sends **802.1Q-tagged frames to an access port**, the switch does not process the VLAN tag correctly, so the frames are dropped or not classified into the VLAN; configuring the port as a **trunk allows the switch to interpret the VLAN tag and forward the frames properly**.
{% endhint %}

### 802.1Q VLAN tagging

When a switch puts an Ethernet frame on a trunk, it needs to add a VLAN tag with information about the VLAN to which the frame belongs. The switch does so by using the 802.1Q encapsulation header. IEEE 802.1Q uses an internal tagging mechanism that inserts an extra 4-byte tag field into the original Ethernet frame between the Source Address and Type or Length fields. As a result, the frame still has the original source and destination MAC addresses. Also, because the original header has been expanded, 802.1Q encapsulation forces a recalculation of the original frame check sequence (FCS) field in the Ethernet trailer, because the FCS is based on the content of the entire frame. It is the responsibility of the receiving Ethernet switch to look at the 4-byte tag field and determine where to deliver the frame.

When a device sends data on a VLAN, it gets tagged by the originating VLAN in the Ethernet header inserted between the source MAC address and the EtherType/Length field

ISL cisco proprietary vlan tagging protocol and no longer in use (deprecated)

![](<../.gitbook/assets/Unknown image (638)>)

Type or tag protocol identifier is set to a value of 0x8100 to identify the frame as an IEEE 802.1Q-tagged frame.

**Priority code point (PCP)**

A 3-bit field which refers to the IEEE 802.1p class of service (CoS) and maps to the frame priority level. Different PCP values can be used to prioritize different classes of traffic

**Canonical Format Identifier (CFI)** is a 1-bit identifier that enables Token Ring frames to be carried across Ethernet links

it is also called Drop eligible indicator (DEI) A 1-bit field. (formerly CFI\[c]) May be used separately or in conjunction with PCP to indicate frames eligible to be dropped in the presence of congestion

**VLAN identifier (VID)**

A 12-bit field identifying the VLAN to which the frame belongs. The values of 0 and 4095 (0x000 and 0xFFF in hexadecimal) are reserved. All other values may be used as VLAN identifiers, allowing up to 4,094 VLANs. The reserved value 0x000 indicates that the frame does not carry a VLAN ID

On bridges, VID 0x001 (the default VLAN ID) is often reserved for a network management VLAN; this is vendor-specific. The VID value 0xFFF is reserved for implementation use; it must not be configured or transmitted

![](<../.gitbook/assets/Unknown image (639)>)

### Default VLAN 1

**Default VLAN 1** is a factory-default Ethernet VLAN and all switch ports are members of a default vlan until they are added to a different vlan, thus cannot be modified as it is integrated in each switch already.

Ports not assigned to any vlan will always exist in default vlan 1. Vlan 1 cannot be deleted.

So .. if a port is assigned to a vlan, a switch can lookup the mac-address-table for MAC-Port-Vlan entry to forward the frames.

If the port is not assigned to a vlan it will have entry for MAC-Port-Vlan for vlan 1.

### Native VLAN

**Native VLAN** is a special vlan used to send and receive untagged frames over a trunk port.

On an 802.1Q trunk port, there is one VLAN, called the native VLAN, which is untagged. By default, the native VLAN is VLAN 1, which means that the switch does not insert an extra 802.1Q tag inside an Ethernet frame. When the switch on the receiving side receives the Ethernet frame that does not have an 802.1Q tag, it knows that the frame belongs to the native VLAN. All other VLANs are tagged with a VID. IEEE 802.1Q specifies that native VLANs are backward compatible with legacy LAN scenarios, where untagged traffic is common.

The native vlan can be changed to a specific vlan ID, and most importantly the native VLAN must be consistently configured across all switches in the network to prevent loops, inconsistencies and to enhance security.

The native vlan is is typically used for traffic that doesn't need to be tagged, such as management traffic, however it is always recommended to create separate vlan with it's unique ID for management traffic

If the native vlan is used for management traffic, changing it to a different ID to prevent potential attackers from exploiting switch ports that are part of the native VLAN, which could give them unauthorized access to management traffic

Also all ports, where endpoints and users are expected to connect must be placed in their respective vlan, separated from the management vlan

If the native VLAN on one end of the trunk is different from the native VLAN on the other end, spanning-tree loops might result or the frames meant for native vlan can be misrouted to a different vlan if not configured consistently

Always make sure that the native VLAN for an trunk port is the same on both ends

### VLAN configuration

Each port on a switch belongs to a VLAN. If the VLAN to which the port belongs is deleted, the port becomes inactive. Also, a port becomes inactive if it is assigned to a nonexistent VLAN. All inactive ports are unable to communicate with the rest of the network.

{% hint style="info" %}
If a VLAN span multiple switches then it must be also configured on those switches, otherwise the switch that doesn't have such vlan configured drops the frame
{% endhint %}

| (config)# vlan                                                                                            | if no vlan is created use command (config)#vlan configuration enters and creates VLAN feature configuration mode                                                                            |
| --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-vlan)# name CustomerA                                                                             | it is recommended to configure at least name, to assure that the vlan is created, since vlan <> may not be sufficient                                                                       |
| (config)# no vlan                                                                                         | When you delete a VLAN, any ports assigned to that VLAN become inactive They remain associated with the VLAN (and thus inactive) until you assign them to a new VLAN.                       |
| (config-if)# switchport mode access                                                                       | configure port as an access. If you assign an interface to a VLAN that does not exist, the new VLAN is created                                                                              |
| (config-if)# switchport access vlan                                                                       |                                                                                                                                                                                             |
| (config-if)#switchport trunk encapsulation {isl \| dot1q \| negotiate } (config-if)#switchport mode trunk | configure port as a trunk.                                                                                                                                                                  |
| (config-if)# switchport trunk native vlan                                                                 | Configures the native VLAN that is sending and receiving untagged traffic on the trunk port Note you must also create that VLAN on the switch                                               |
| (config-if)#switchport trunk allowed vlan \[X,X]                                                          | by default all vlans are trunked over the trunked port, to limit what vlans can be forwarded out of an trunk interface - used as na security tool or to limit broadcast for unallowed vlans |
| show vlan \[brief]                                                                                        | show vlan-switch # on a router                                                                                                                                                              |
| show vlan id <>                                                                                           |                                                                                                                                                                                             |
| show interfaces trunk                                                                                     |                                                                                                                                                                                             |

{% hint style="info" %}
To delete all (range) vlans on a switch type (config)#no vlan xxx-xxx , since #delete vlan.dat
{% endhint %}

### Voice VLAN

Usually, IP phones are placed next to a computer in the working environment. They use Ethernet and require the same network cables as computers. Hence, you can use two separate connections to the network, the computer and the IP phone.

Alternatively, you can connect the computer to an Ethernet port on the IP phone, and then the connection from the IP phone to the network carries the traffic from both the computer and the IP phone. This is enabled on some Cisco Catalyst switches with a unique feature that is called voice VLAN; it lets you overlay a voice topology onto a data network. You can segment phones into separate logical networks, even though the data and voice infrastructure are physically the same.

network administrators have the ability to prioritize voice traffic over data traffic.

The voice VLAN feature allows voice traffic from the attached IP phone and data traffic from an end station to be transmitted on different VLANs.

You create a voice VLAN in the same way as you create data VLAN, using the vlan global configuration command. The following example shows how to create VLAN 3 and how to assign this VLAN as a voice VLAN to the FastEthernet0/2 interface.

![](<../.gitbook/assets/Unknown image (640)>)

| interface range GigabitEthernet0/1 - 24          |                                                                                                                                           |
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| switchport access vlan 100                       |                                                                                                                                           |
| switchport mode access                           |                                                                                                                                           |
| switchport voice vlan 100                        |                                                                                                                                           |
| mls qos trust cos                                | Trusts the CoS (Class of Service) value for QoS on these ports, ensuring proper prioritization of voice traffic                           |
| mls qos trust device cisco-phone                 | is used to trust the CoS markings from a connected Cisco IP phone                                                                         |
| switchport priority extend {cos value \| trust } | is used to extend trust to incoming packets, preserving their CoS or DSCP markings when they are forwarded by the switch to other devices |
| switchport voice vlan dot1p 5                    | Assign CoS value 5 to voice traffic                                                                                                       |

### End-to-end VLANs

End-to-End VLANs (also called Campus-wide VLANs) extend across multiple switches and are present throughout the entire network. This means users in the same VLAN can be located in different buildings or floors while still being part of the same broadcast domain. These VLANs were more common in older network designs but are now less preferred due to scalability and security concerns.

![](<../.gitbook/assets/Unknown image (641)>)

### Local VLANs

Local VLANs are restricted to a single switch or a small group of switches within a specific geographic area, such as a single building or floor. This design improves network efficiency by limiting broadcast traffic and enhances security by reducing the scope of VLAN-related attacks. Modern best practices recommend using local VLANs with Layer 3 routing between VLANs to improve network performance and manageability.

![](<../.gitbook/assets/Unknown image (642)>)

### Private VLANs (PVLANs)

**Private VLANs (PVLANs)** partitions a VLAN into subdomains to provide additional layer of isolation and control

Subdomain is represented by a pair of VLANs: a primary VLAN and a secondary VLAN. The secondary VLAN ID differentiates one subdomain from another

PVLANs also addresses scalability problem and provides IP address management benefits for service providers.

Port types

Promiscuous Ports uplink ports belonging to the primary VLAN, can talk with all Isolated and Community ports.

Isolated Ports are isolated from each other, communication between isolated ports is blocked

Community Ports can communicate with other community ports and promiscuous ports within the PVLAN, but not with isolated ports or other PVLAN communities

![Private VLANs Topic Notes - The Bit-Bucket](<../.gitbook/assets/Unknown image (643)>)

| ## Configuring PVLAN Primary Switch(config-vlan)# private-vlan primary ## Configuring PVLAN Secondary Isolated Switch(config-vlan)# private-vlan isolated ## Configuring PVLAN Secondary Community Switch(config-vlan)# private-vlan community ## Associating PVLAN Primary with PVLAN Secondary Switch(config)# vlan Switch(config-vlan)# private-vlan association ## Configuring interface as Promiscuous port Switch(config-if)# switchport mode private-vlan promiscuous Switch(config-if)# switchport private-vlan mapping add ## Configuring interface as Host port Switch(config-if)# switchport mode private-vlan host Switch(config-if)# switchport private-vlan host-association ## Configuring SVI as PVLAN gateway Switch(config-if)# private-vlan mapping | Considerations VTP must be off/transparent mode or VTPv3 must be used for Private VLANs to work Switch SVI as PVLAN Gateway: Secondary PVLANs must be mapped PVLAN-Switch to Router/Gateway: Promiscuous port 802.1q tag from the secondary VLAN gets rewritten and replaced with the primary VLAN ID. PVLAN-Switch to PVLAN-Switch: Standard trunk port PVLAN-Switch to Non-PVLAN-Switch: Isolated trunk port Frame has the VLAN ID of the secondary VLAN. show vlan private-vlan |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### VLAN design considerations

The maximum number of VLANs is switch-dependent.

VLAN 1 is the factory-default Ethernet VLAN.

Keep management traffic in a separate VLAN.

Change the native VLAN to something other than VLAN 1.

Also, all unused switch ports should be assigned to black hole VLAN and set to be administratively down. A black hole VLAN is a term for a VLAN that is associated with a subnet that has no route, or no default-gateway to other networks within your organization, or to the internet. Hence, you can mitigate the security risks associated with the default VLAN 1.

A good security practice is to separate management and user data traffic. This is because you do not want users to be able to establish Secure Shell (SSH) sessions to the switch when directly connected to it. By default, the management VLAN is VLAN 1 and it should be changed to a different VLAN. If you want to communicate with a Cisco switch remotely for management purposes, the switch must have an IP address and a default gateway configured, so you can reach it. Both the IP address and the default gateway should be in the management VLAN. In this case, users that are not in the management VLAN cannot access the switch, unless they were routed into the management VLAN. Separating management into a dedicated VLAN allows you to easily control who has access to the switch with access and security policies, making your network more secure.

Make sure that the native VLAN for an 802.1Q trunk is the same on both ends of the trunk port.

Only allow specific VLANs to traverse through the trunk port.

Another good security practice is to change the native VLAN to something other than VLAN 1 because all control traffic is sent on VLAN 1. The native VLAN should be changed to be a VLAN that is not used for any other traffic. By default, the native VLAN is not tagged, but it is recommended to tag the native VLAN. The example below shows how to change the native VLAN and tag it.

SW1(config)# interface Ethernet0/0

SW1(config-if)# switchport mode trunk

SW1(config-if)# switchport trunk native vlan 90

SW1(config-if)# switchport trunk native vlan tag

### DTP (Dynamic Trunking Protocol)

**Dynamic Trunking Protocol (DTP)** allows Cisco switches to automatically determine the Operational Mode (Trunk/Access) and Trunking Encapsulation (802.1Q/lSL) for that trunk.

During DTP negotiation, the ports will not participate in the Spanning-Tree Protocol. Only after the port type is configured to be one of the three types (access, ISL trunk, or 802.1Q trunk), the port will be added to spanning tree. Whenever a port fails to negotiate to become a trunk port, it will stay an access port. If the negotiating ports allow, DTP prefers ISL to 802.1Q.

Switches must be in the same VTP domain to negotiate a trunk using DTP.

Best practice is to disable DTP and manually configure trunk and access ports. DTP poses a security risk, as an unauthorized switch could connect and automatically form a trunk, potentially allowing VLAN hopping, recalculate STP topology or other attacks.

DTP is enabled on Cisco switch ports by default

| (config-if)# switchport mode dynamic {auto \| desirable } | ON statically turned on dynamic auto: the interface will form a trunk only if it receives DTP messages to do so from the other side switch. An interface configured in dynamic auto mode does not generate DTP messages and only listens for incoming DTP messages. dynamic desirable: the interface will negotiate the mode automatically and will actively try to convert the link to a trunk link. An interface configured in dynamic desirable mode generates DTP messages and listens for incoming DTP messages. If the port on the other side switch interface is capable to form a trunk, a trunk link will be formed. |
| --------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-if)# switchport trunk encapsulation negotiate     | configures the dynamic encapsulation type; if applied, the #switchport mode trunk can't be configured                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| show dtp                                                  | DTP-enabled switch ports send DTP frames once every 30 seconds.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| switchport nonegotiate                                    | used on Cisco switches to disable Dynamic Trunking Protocol (DTP) negotiation on an interface. This means the port will not send or respond to DTP messages, effectively preventing it from dynamically forming a trunk link                                                                                                                                                                                                                                                                                                                                                                                                  |

| Interface mode on one side        | Interface mode on other side | Resulting operational mode |
| --------------------------------- | ---------------------------- | -------------------------- |
| dynamic auto                      | dynamic auto                 | access                     |
| dynamic auto                      | dynamic desirable            | trunk                      |
| dynamic desirable                 | dynamic desirable            | trunk                      |
| dynamic auto or dynamic desirable | trunk                        | trunk                      |
| dynamic auto or dynamic desirable | access                       | access                     |
| access                            | trunk                        | limited connectivity       |
| trunk                             | access                       | limited connectivity       |

### VTP (VLAN Trunking Protocol)

**VLAN Trunking Protocol (VTP)** is a Cisco proprietary Layer 2 Messaging protocol that maintains VLAN configuration consistency by managing the addition, deletion, and renaming of VLANs on a networkwide basis. It reduces administration overhead in a switched network. The switch supports VLANs in VTP client, server, and transparent modes.

VTP domain name and authentication must match (key sensitive). Configuring password is not required

Port connected to other switches has to be trunk, to send the VTP advertisements

Configuration revision (+1 with each change to VLAN configuration). Highest revision number is most up to date. VTP information is saved in the VTP VLAN database

VTP Switch Modes

Server Create and delete VLANs and advertises them to clients. This is the default mode. If the switch detects a failure while writing a configuration to NVRAM, VTP mode automatically changes from server mode to client mode. Preferably all switches should be in server mode

VTP messages are sent as multicast frames to address 01-00-0C-CC-CC-CC

VTP servers and clients are synchronized to the highest revision number

The revision number is incremented by +1 with each change in the VLAN database

VTP messages are sent every 5 minutes or when the VLAN database changes

Client Accept and advertises VTP messages and can update new switch (with lower conf rev) in VTP domain, but cannot modify VLANs

Transparent Can create and modify VLANs, but it will stay local on that switch and won't be propagated to other switches. Transparently forwards other VTP advertisements

Revision number is not significant and always 0

VTPv3

introduced #vtp primary server, that prevents configuration revision overwrite problem that was in v1 (default) and v2, where a new switch that joined to the domain and had configured highest revision number than other switches erased the configuration of other switches in the domain

VTPv3 have enhanced authentication, where you can configure the password as hidden or secret

hidden the secret key from the password string is saved in the VLAN database file, but it does not appear in plain text in the configuration

Instead, the key associated with the password is saved in hexadecimal format in the running configuration

secret you can directly configure the password secret key

Supports extended VLAN range and Private VLAN

VLAN pruning prevents unnecessary VLAN traffic from being forwarded to switches that do not have those VLANs configured. This optimizes network performance by reducing unnecessary traffic flooding and conserving bandwidth.

Added as part of VTP or configured manually only on server

| vtp primary                                                                                        | vtp primary server for VTPv3 has to be enabled in the privilege mode, not in the config mode                                                         |
| -------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| vtp domain CISCO                                                                                   | Configures a VTP administrative domain.                                                                                                              |
| vtp mode {client \| server \| transparent \| off }                                                 |                                                                                                                                                      |
| vtp password CISCO {hiddne \| secret}                                                              |                                                                                                                                                      |
| show vtp status                                                                                    | show vtp devices                                                                                                                                     |
| switchport trunk pruning vlan {add \| except \| none \| remove } vlan-list \[,vlan \[,vlan \[,,,]] |                                                                                                                                                      |
| (config)# vtp pruning                                                                              | Enables pruning in the VTP administrative domain. By default, pruning is disabled. You need to enable pruning on only one switch in VTP server mode. |

## Inter-VLAN routing

If we want to allow communication between different VLANs we'll have to use device capable of routing between different LAN networks

Inter-VLAN routing is a process of forwarding network traffic from one VLAN to another VLAN using a Layer 3 device.

Option 1

PC1 and PC2 belong to VLAN 10 and PC3 and PC4 belong to VLAN 20. PC1 and PC2 can send packets to each other directly because they are on the same broadcast domain. The same goes for PC3 and PC4. If PC1 wants to send data to PC3, it requires inter-VLAN routing. PC1 checks the destination IP address and determines that PC3 belongs to a different subnet. PC1 sends the data to its default gateway when the destination IP address belongs to another subnet. The default gateway for each VLAN (VLAN 10 and VLAN 20) is configured on the Layer 3 device. The Layer 3 device (it can be a router or a Layer 3 switch) can route the traffic between the different subnets because the subnets are directly connected networks. The networks appear as directly connected because the default gateway IP addresses are configured on the interfaces. When the Layer 3 device receives the data from PC1, it checks the routing table to find the outgoing interface to reach the destination

Traditional inter-VLAN routing requires multiple physical interfaces on both the router and the switch. VLANs are associated with unique IP subnets on the network. This subnet configuration facilitates the routing process in a multi-VLAN environment. When you use a router to facilitate inter-VLAN routing, the router interfaces are connected to switch interfaces that are in separate VLANs. Devices on these VLANs send traffic through the router to reach other VLANs. However, when you use a separate interface for each VLAN on a router, you can quickly run out of interfaces. This solution is not very scalable.

![](<../.gitbook/assets/Unknown image (644)>)

### Router-on-a-stick (ROAS)

**Router-on-a-stick (ROAS)** uses a single physical interface connected to a switch, divided into virtual sub-interfaces. Each sub-interface is associated with a specific VLAN.

In order for the router to be able to recognize individual VLANs on the single link, which is shared with the switch, a special technology called ROAS (Router on a stick) is used

Without ROAS, it would be same scenario as without trunk ports, there would have to be separate interface between switch and router for each VLAN

Subinterfaces are virtual interfaces created on a port that allow a single physical connection to be split into multiple logical networks. They are often used in combination with VLANs to allow a single physical interface to serve multiple networks, ideal for scenarios where you want to save ports or simplify your infrastructure.

This configuration allows the router to differentiate traffic from various VLANs and apply VLAN tags to outgoing packets with the corresponding vlan. These tags ensure that traffic is routed to the correct VLAN and delivered through the corresponding sub-interface

| Switch Config                                       |   |
| --------------------------------------------------- | - |
| SW1(config)#interface fa0/3                         |   |
| SW1(config-if)#switchport trunk encapsulation dot1q |   |
| SW1(config-if)#switchport mode trunk                |   |
| SW1(config-if)#switchport trunk allowed vlan 10,20  |   |

Router Config

| R1(config)#interface fa0/0.10                            |                                                                                                                                                                                                                                                  |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| R1(config-subif)#encapsulation dot1Q 10                  | Defines the encapsulation format as IEEE 802.1Q (dot1q), and specifies the VLAN identifier, so that the router distinguish from which source VLAN the packet has been sent and what tag to use when sending return traffic for the source device |
| R1(config-subif)#ip address 192.168.10.254 255.255.255.0 |                                                                                                                                                                                                                                                  |
| R1(config)#interface fa0/0.20                            |                                                                                                                                                                                                                                                  |
| R1(config-subif)#encapsulation dot1Q 20                  |                                                                                                                                                                                                                                                  |
| R1(config-subif)#ip address 192.168.20.254 255.255.255.0 |                                                                                                                                                                                                                                                  |
| R1(config-subif)#encapsulation dot1q native              | Native (untagged) traffic is sent directly to the physical IP of the router. Tagged traffic is sent to the VLAN subinterface.                                                                                                                    |
| show vlans                                               |                                                                                                                                                                                                                                                  |

{% hint style="info" %}
`encapsulation dot1q native` sends **untagged** frames to the physical interface IP.

Tagged frames are processed by the matching subinterface VLAN tag.
{% endhint %}

![](<../.gitbook/assets/Unknown image (645)>)

Example to emphasize the role of a router in a router on a stick scenarios

In this example both subinterfaces are tagged with VLAN 200, but they belong to different IP subnets (10.0.200.0/24 and 10.0.210.0/24).

RP/0/RP0/CPU0:N540X-16Z4G8Q2C-A(config-subif)#do show ip route vrf PON

C 10.0.200.0/24 is directly connected, 23:18:47, TenGigE0/0/0/5.200

L 10.0.200.1/32 is directly connected, 23:18:47, TenGigE0/0/0/5.200

C 10.0.210.0/24 is directly connected, 00:00:07, TenGigE0/0/0/11.200

L 10.0.210.1/32 is directly connected, 00:00:07, TenGigE0/0/0/11.200

A VLAN and its VLAN ID are locally significant within a Layer 2 broadcast domain and have no inherent meaning across Layer 3 boundaries. As such, VLAN 10 in the Layer 2 domain behind routed port is entirely separate from a VLAN 10 behind another routed port. Each routed interface defines a distinct IP subnet and broadcast domain, making the VLAN IDs on either side unrelated

The router acts as the boundary between the Layer 2 VLAN and Layer 3 routing. By assigning different IP subnets to the same VLAN ID on different subinterfaces, the router ensures that traffic between 10.0.200.0/24 and 10.0.210.0/24 is routed, not bridged.

This setup might be intentional, such as in a multi-site network where the same VLAN ID is reused for consistency (e.g., VLAN 200 for "user VLAN" across sites), but the subnets are kept separate for security or addressing purposes.

![](<../.gitbook/assets/Unknown image (646)>)

### Switch Virtual Interface (SVI)

**Switch Virtual Interface (SVI)** is an logical vlan interface that allows switch to send and receive IP packets and as a default gateway for the devices within each VLAN

The switch can be then configured to send packets to the next-hop router

Layer 3 switching is more scalable than a router-on-a-stick design because the latter forces all inter-VLAN traffic through a single trunk link to the router, creating a bottleneck. Layer 3 switches can route between VLANs directly in hardware at line speed, eliminating that limitation

Same ARP mechanism applies - When a swich has configured SVI for for example VLAN10 and it doesn't have any access port in that VLAN, and wants to resolve MAC of it's next-hop within the VLAN 10 subnet range, it will flood the ARP via trunk with the vlan ID tag, so that the frame is flooded to reach the destination in the vlan on a different switch - then it will respond with the ARP reply

It does the same if the MAC address is not in his table of the VLAN 10, it will send it over trunk, so that users can resolve MAC of other devices within the VLAN

{% hint style="info" %}
For an SVI there are 2 ways to get the SVI into an up/up state. Either the switch has an access port assigned to that vlan and with a connected device, or the switch has an active trunk that includes that vlan. Without it the SVI will be in down/down state. If the state of the SVI is in down/down you can bring it to up/down by creating the vlan first.

If an SVI (VLAN logical interface) is in an up/down state, it indicates that all physical ports assigned to that VLAN are currently down.”

If you want SVI to be used for differentiated routing (to route different networks over a single L2 trunk connectuon) without any connected access ports in that vlan - you must configure the vlan in a global configuration mode to get it into up/down state and then explicitly configure it to be allowed over the L2 trunk port with a switchport trunk allowed vlan xxx,…, this way you will have the SVI in a up/up state and to be seen by the L3 switch as directly connected subnet/interface.
{% endhint %}

![](<../.gitbook/assets/Unknown image (647)>)

{% hint style="info" %}
SVI's similarly as physical interfaces or loopbacks can be assigned to the VRF. Multiple SVI's can be part of single VRF
{% endhint %}

![](<../.gitbook/assets/Unknown image (648)>)

| SW1(config)#ip routing                                  | To enable routing capabilities on the switch |
| ------------------------------------------------------- | -------------------------------------------- |
| SW1(config)#interface vlan 10                           |                                              |
| SW1(config-if)# ip address 192.168.10.254 255.255.255.0 | also perform no shut                         |
| SW1(config)#interface vlan 20                           |                                              |
| SW1(config-if)# ip address 192.168.20.254 255.255.255.0 | also perform no shut                         |

| (config-if)# no switchport | to disable switching on an interface and enable routing |
| -------------------------- | ------------------------------------------------------- |

#### SVI common use cases

Apart frm being an L3 first hop gateway for the vlan, SVI's can be used to establish an adjacency over an access P2P link interconnecting neighboring L3 switches

#### OSPF over SVI on an access port

Key Concept:

The configuration is valid for establishing OSPF neighborship between two Layer 3 switches.

1. The physical p2p link (GigabitEthernet1/0/1) is in Layer 2 Access Mode for VLAN 6.
2. The L3 processing (IP addressing and OSPF) is performed on the SVI (Interface Vlan6), which is the virtual routing interface for that VLAN.
3. As long as both L3 switches have SVIs configured in the same VLAN (VLAN 6) and use the same OSPF area, they will successfully form a neighbor relationship.

The configuration successfully demonstrates the use of a Layer 2 access link as the underlying transport for a Layer 3 SVI-based OSPF connection.

| interface GigabitEthernet1/0/1 switchport access vlan 6 ! interface Vlan6 ip address 10.88.0.132 255.255.255.240 ip ospf 100 area 0 |   |
| ----------------------------------------------------------------------------------------------------------------------------------- | - |

### Other common use cases for VLANs

VLANs can also be used to achieve specific goals like for example establishing P2P adjacency for OSPF over an L2 transparent infrastructure or even more complex layered infrastructure like shown below in the Logical topology.

PE11 goal is to achieve P2P OSPF adjacency with RSD border to become part of one routing domain. Vlan 4000 is used and configured as subinterface on both edge routers. The adjacent routers ASR1k performs special function as they bind this vlan 4000 coming from the edge routers to L2TP tunnel (L2VPN) over the underlaying infrastructure which comprises of firewalls with which they have traditional P2P IP link in order for them to bind this P2P IP link to IPsec tunnel over the internet.

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

The real physical topology of this setup is even more complicated as there are two additional switches between the ASR1k and Firewall. They perform traditional L2 switching based on the VLAN1300 which is imposed for the P2P IP link between firewall and the ASR1k. The purpose of these switches in this specific setup is to provide additional capability to capture packets for troubleshooting of the setup.

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

## Virtual Extensible LAN (VXLAN)

**Virtual Extensible LAN (VXLAN)** is overlay encapsulation protocol designed to address scalability issues of traditional L2 networks, as they were unable to support virtualizion demands on the physical infrastructure

VXLAN creates one large virtual Layer 2 connections on top of an existing L3 network, to merge remote sites into a single broadcast domain

It does so with MAC-in-IP tunneling on UDP port 4789. To avoid unnecessary broadcast flooding to entire network segment VXLAN domain uses multicast groups instead

The recommended usage of VXLAN is a Spine-Leaf design or in SDA

![](<../.gitbook/assets/Unknown image (1635)>)

**VXLAN network identifier (VNI)** is 24-bit field (unlike VLAN ID 12-bit) increasing available VXLAN segments up to 16 million, that can coexists within the existing infrastructure

**Virtual tunnel endpoints (VTEPs)** devices that originate or terminate VXLAN tunnels over the L3 underlay. They map L2 and L3 packets to the VNI used in the overlay network

Asymmetric IRB ingress VTEP

* routes the incoming packet into its destination VLAN
* bridges and tunnels the packet within the destination VLAN over to the receiving VTEP with the MAC address

With asymmetric IRB, the ingress VTEP performs both Layer-2 bridging and Layer-3 routing lookup, whereas the egress VTEP performs only Layer-2 bridging lookup.

Symmetric IRB ingress VTEP

* tunnels the packet over the transit VNI over to the receiving VTEP
* then: the receiving VTEP routes the packet into the correct destination VLAN and then bridges it to the destination MAC address

**Network Virtualization Edge (NVE)** Logical interface where the encapsulation/decapsulation occurs. Comparable to virtual LISP tunnel interface

### Encapsulation overhead (header)

VXLAN adds 50 byte header (14 byte Ethernet, 20 byte outer IP, 8 byte outer UDP, 8 byte VXLAN)

### VXLAN Group Policy Option (VXLAN-GPO)

New 4byte field was added to the VXLAN header to support up to 64,000 SGT tags

**Group Policy ID:** 16-bit identifier that is used to carry the SGT tag

**Group Based Policy Extension Bit (G Bit):** 1-bit field, when set to 1, indicates an SGT tag is being carried

**Don’t Learn Bit (D Bit):** 1-bit field, when set to 1 indicates that the egress VTEP must not learn the source address of the encapsulated frame

**Policy Applied Bit (A Bit):** 1-bit field that is only defined as the A bit when the G bit field is set to 1. When the A bit is set to 1, it indicates that the group policy has already been applied to this packet, and further policies must not be applied by network devices.

When it is set to 0, group policies must be applied by network devices, and they must set the A bitto 1 after the policy has been applied

![](<../.gitbook/assets/Unknown image (1636)>)

VXLAN header VNID = SD-Access VN (macro segmentation)

VXLAN header GPID = SD-Access SGT (micro segmentation)

VXLAN is a data plane protocol, which was left open to be used with any control plane technology

VXLAN with Multicast underlay; VXLAN with MP-BGP EVPN control plane for DC/private cloud; VXLAN with LISP control plane for campus environments

When some devices can't work with VXLAN technology and still need to use traditional VLANs to separate their networks, a solution comes in the form of a VXLAN gateway

This gateway acts as a bridge between the two worlds, allowing devices that use VXLAN and those that use VLAN to communicate

![](<../.gitbook/assets/Unknown image (1637)>)

## .1Q Tunneling

### Q-in-Q (IEEE 802.1ad) / IEEE 802.1Q tunneling

The IEEE 802.1Q Tunneling feature is designed for service providers who carry traffic of multiple customers across their networks and are required to maintain the VLAN and Layer 2 protocol configurations of each customer without impacting the traffic of other customers.

Business customers of service providers often have specific requirements for VLAN IDs and the number of VLANs to be supported. The VLAN ranges required by different customers in the same service-provider network might overlap, and traffic of customers through the infrastructure might be mixed. Assigning a unique range of VLAN IDs to each customer would restrict customer configurations and could easily exceed the VLAN limit (4096) of the IEEE 802.1Q spAecification.

Using the IEEE 802.1Q tunneling feature, service providers can use a single VLAN to support customers who have multiple VLANs. Customer VLAN IDs are preserved, and traffic from different customers is segregated within the service-provider network, even when they appear to be in the same VLAN. Using IEEE 802.1Q tunneling expands VLAN space by using a VLAN-in-VLAN hierarchy and retagging the tagged packets. A port configured to support IEEE 802.1Q tunneling is called a tunnel port. When you configure tunneling, you assign a tunnel port to a VLAN ID that is dedicated to tunneling. Each customer requires a separate service-provider VLAN ID, but that VLAN ID supports all of the customer’s VLANs.

Customer traffic that is tagged in the normal way with appropriate VLAN IDs comes from an IEEE 802.1Q trunk port on the customer device and into a tunnel port on the service-provider edge device. The link between the customer device and the edge device is asymmetric because one end is configured as an IEEE 802.1Q trunk port, and the other end is configured as a tunnel port. You assign the tunnel port interface to an access VLAN ID that is unique to each customer. Assymetric - the customer port is trunk and the providers port is tunnel port

Packets coming from the customer trunk port into the tunnel port on the service-provider edge device are normally IEEE 802.1Q-tagged with the appropriate VLAN ID. The tagged packets remain intact inside the device and when they exit the trunk port into the service-provider network, they are encapsulated with another layer of an IEEE 802.1Q tag (called the metro tag) that contains the VLAN ID that is unique to the customer. The original customer IEEE 802.1Q tag is preserved in the encapsulated packet. Therefore, packets entering the service-provider network are double-tagged, with the outer (metro) tag containing the customer’s access VLAN ID, and the inner VLAN ID being that of the incoming traffic.

When the double-tagged packet enters another trunk port in a service-provider core device, the outer tag is stripped as the device processes the packet. When the packet exits another trunk port on the same core device, the same metro tag is again added to the packet.

When the packet enters the trunk port of the service-provider egress device, the outer tag is again stripped as the device internally processes the packet. However, the metro tag is not added when the packet is sent out the tunnel port on the edge device into the customer network. The packet is sent as a normal IEEE 802.1Q-tagged frame to preserve the original VLAN numbers in the customer network.

If traffic coming from a customer network is not tagged (native VLAN frames), these packets are bridged or routed as normal packets. All packets entering the service-provider network through a tunnel port on an edge device are treated as untagged packets, whether they are untagged or already tagged with IEEE 802.1Q headers. The packets are encapsulated with the metro tag VLAN ID (set to the access VLAN of the tunnel port) when they are sent through the service-provider network on an IEEE 802.1Q trunk port. The priority field on the metro tag is set to the interface class of service (CoS) priority configured on the tunnel port. (The default is zero if none is configured.)

[https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9200/software/release/17-12/configuration\_guide/lyr2/b\_1712\_lyr2\_9200\_cg/configuring\_ieee\_802\_1q\_tunneling.html](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9200/software/release/17-12/configuration_guide/lyr2/b_1712_lyr2_9200_cg/configuring_ieee_802_1q_tunneling.html)

### Notes

**QinQ** is equivivalent to cisco EVC rewrite push operation

**VLAN Mapping** is equivivalent to Cisco EVC rewrite translation

![Ethernet Frame Q In Q](<../.gitbook/assets/Ethernet Frame Q In Q>)

![Cisco Q In Q Lab Vlan Tags](<../.gitbook/assets/Cisco Q In Q Lab Vlan Tags>)

R1(config)#interface fastEthernet 0/0

R1(config-if)#no shutdown

R1(config-if)#interface fastEthernet 0/0.12

R1(config-subif)#encapsulation dot1Q 12

R1(config-subif)#ip address 192.168.12.1 255.255.255.0

R2(config)#interface fastEthernet 0/0

R2(config-if)#no shutdown

R2(config-if)#interface fastEthernet 0/0.12

R2(config-subif)#encapsulation dot1Q 12

R2(config-subif)#ip address 192.168.12.2 255.255.255.0

R1 and R2 are both configured with sub-interfaces and use subnet 192.168.12.0 /24. All their frames are tagged as VLAN 12.

On the service provider network, we’ll have to configure a number of items. First I will configure 802.1Q trunks between SW1 – SW3 and SW2 – SW3:

SW1(config)#interface fastEthernet 0/19

SW1(config-if)#switchport trunk encapsulation dot1q

SW1(config-if)#switchport mode trunk

SW2(config)#interface fastEthernet 0/21

SW2(config-if)#switchport trunk encapsulation dot1q

SW2(config-if)#switchport mode trunk

SW3(config)#interface fastEthernet 0/19

SW3(config-if)#switchport trunk encapsulation dot1q

SW3(config-if)#switchport mode trunk

SW3(config)#interface fastEthernet 0/21

SW3(config-if)#switchport trunk encapsulation dot1q

SW3(config-if)#switchport mode trunk

The next part is where we configure the actual “Q-in-Q” tunneling. The service provider will use VLAN 123 to transfer everything from our customer. We’ll configure the interfaces toward the customer routers to tag everything for VLAN 123:

SW1(config)#interface fastEthernet 0/1

SW1(config-if)#switchport access vlan 123

SW1(config-if)#switchport mode dot1q-tunnel

SW2(config)#interface fastEthernet 0/2

SW2(config-if)#switchport access vlan 123

SW2(config-if)#switchport mode dot1q-tunnel

### Native VLANs (important caveat)

When configuring IEEE 802.1Q tunneling on an edge device, you must use IEEE 802.1Q trunk ports for sending packets into the service-provider network. However, packets going through the core of the service-provider network can be carried through IEEE 802.1Q trunks, ISL trunks, or nontrunking links. When IEEE 802.1Q trunks are used in these core devices, the native VLANs of the IEEE 802.1Q trunks must not match any native VLAN of the nontrunking (tunneling) port on the same device because traffic on the native VLAN would not be tagged on the IEEE 802.1Q sending trunk port.

In the following network figure, VLAN 40 is configured as the native VLAN for the IEEE 802.1Q trunk port from Customer X at the ingress edge switch in the service-provider network (Switch B). Switch A of Customer X sends a tagged packet on VLAN 30 to the ingress tunnel port of Switch B in the service-provider network, which belongs to access VLAN 40. Because the access VLAN of the tunnel port (VLAN 40) is the same as the native VLAN of the edge switch trunk port (VLAN 40), the metro tag is not added to tagged packets received from the tunnel port. The packet carries only the VLAN 30 tag through the service-provider network to the trunk port of the egress-edge switch (Switch C) and is misdirected through the egress switch tunnel port to Customer Y.

These are some ways to solve this problem:

Use the vlan dot1q tag native global configuration command to configure the edge switches so that all packets going out an IEEE 802.1Q trunk, including the native VLAN, are tagged. If the switch is configured to tag native VLAN packets on all IEEE 802.1Q trunks, the switch drops untagged packets, and sends and receives only tagged packets.

Ensure that the native VLAN ID on the edge switches trunk port is not within the customer VLAN range. For example, if the trunk port carries traffic of VLANs 100 to 200, assign the native VLAN a number outside that range.

### VLAN mapping (VLAN ID translation)

One way to establish translated VLAN IDs (S-VLANs) is to map customer VLANs to VLANs (called VLAN ID translation) on trunk ports that are connected to a customer network. Packets entering the port are mapped to service provider VLAN (S-VLAN) based on the port number and the packet’s original customer VLAN-ID (C-VLAN).

Service providers’ internal assignments might conflict with a customer’s VLAN. To isolate customer traffic, a service provider decides to map a specific VLAN into another one while the traffic is in its cloud.

**One-to-One VLAN Mapping**

One-to-one VLAN mapping occurs at the ingress and egress of the port and maps the customer C-VLAN ID in the 802.1Q tag to the service-provider S-VLAN ID. You can also specify that packets with all other Vlan IDs are forwarded.

**Selective Q-in-Q**

Selective QinQ maps the specified customer VLANs entering the UNI to the specified S-VLAN ID. The S-VLAN ID is added to the incoming unmodified C-VLAN and the packet travels the service provider network double-tagged. At the egress, the S-VLAN ID is removed and the customer VLAN-ID is retained on the packet. By default, packets that do not match the specified customer VLANs are dropped.

**Q-in-Q on a Trunk Port**

QinQ on a trunk port maps all the customer VLANs entering the UNI to the specified S-VLAN ID. Similar to Selective QinQ, the packet is double-tagged and at the egress, the S-VLAN ID is removed.

[https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9200/software/release/17-12/configuration\_guide/lyr2/b\_1712\_lyr2\_9200\_cg/configuring\_vlan\_mapping.html](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9200/software/release/17-12/configuration_guide/lyr2/b_1712_lyr2_9200_cg/configuring_vlan_mapping.html)

### Layer 2 protocol tunneling

[https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9200/software/release/17-12/configuration\_guide/lyr2/b\_1712\_lyr2\_9200\_cg/configuring\_layer2\_protocol\_tunneling.html](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9200/software/release/17-12/configuration_guide/lyr2/b_1712_lyr2_9200_cg/configuring_layer2_protocol_tunneling.html)

### Hairpinning (same-device forwarding)

In the context of service providers, **hairpinning** refers to a scenario where traffic enters a port on a network device and is immediately sent out through another port on the same device—often without traditional routing. This is commonly used for service tagging, VLAN manipulation, and traffic redirection.

Software-based (logical) hairpinning:

Implemented through internal switching or bridging logic within the device. Packets are internally redirected between logical interfaces or subinterfaces. Common in virtualized environments, BNG/PE routers, and service edge platforms where policy-based forwarding or VRF leakage is used.

Hardware-based (physical) hairpinning:

Achieved by physically connecting two ports on the same device (e.g., via a short patch cable). Traffic entering one port exits via the physically linked port, enabling external tagging, shaping, or service insertion. This approach is considered legacy and less efficient but may still be used when hardware or software limitations prevent internal hairpin forwarding.

### Q-in-Q VLAN tag termination on subinterfaces

IEEE 802.1Q-in-Q VLAN Tag Termination simply adds another layer of IEEE 802.1Q tag (called “metro tag” or “PE-VLAN”) to the 802.1Q tagged packets that enter the network. The purpose is to expand the VLAN space by tagging the tagged packets, thus producing a “double-tagged” frame. The expanded VLAN space allows the service provider to provide certain services, such as Internet access on specific VLANs for specific customers, and yet still allows the service provider to provide other types of services for their other customers on other VLANs.

[https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/lan-wan/b-lan-wan/m\_lnsw-ieee-qvlan.html?utm\_source=chatgpt.com](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/lan-wan/b-lan-wan/m_lnsw-ieee-qvlan.html?utm_source=chatgpt.com)

#### Unambiguous vs ambiguous subinterfaces

The encapsulation dot1q command is used to configure Q-in-Q termination on a subinterface. The command accepts an Outer VLAN ID and one or more Inner VLAN IDs. The outer VLAN ID always has a specific value, while inner VLAN ID can either be a specific value or a range of values.

**Unambiguous Q-in-Q subinterface** is a subinterface that is configured with a single Inner VLAN ID. In the following example, Q-in-Q traffic with an Outer VLAN ID of 101 and an Inner VLAN ID of 1001 is mapped to the Gigabit Ethernet 1/1/0.100 subinterface:

Device(config)# interface gigabitEehernet1/1/0.100

Device(config-subif)# encapsulation dot1q 101 second-dot1q 1001

**Ambiguous Q-in-Q subinterface** is a subinterface that is configured with multiple Inner VLAN IDs. By allowing multiple Inner VLAN IDs to be grouped together, ambiguous Q-in-Q subinterfaces allow for a smaller configuration, improved memory usage and better scalability.

In the following example, Q-in-Q traffic with an Outer VLAN ID of 101 and Inner VLAN IDs anywhere in the 2001-2100 and 3001-3100 range is mapped to the Gigabit Ethernet 1/1/0.101 subinterface:

Device(config)# interface gigabitethernet1/1/0.101

Device(config-subif)# encapsulation dot1q 101 second-dot1q 2001-2100,3001-3100

Ambiguous subinterfaces can also use the any keyword to specify the inner VLAN ID.

Only PPPoE is supported on ambiguous subinterfaces. Standard IP routing is not supported on ambiguous subinterfaces.
