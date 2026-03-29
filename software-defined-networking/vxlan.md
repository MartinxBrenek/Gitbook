# VXLAN

### Virtual Extensible LAN (VXLAN)

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
