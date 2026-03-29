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

# Metallic

### Coaxial cabling

**Coaxial cabling** is metallic (made from copper) cable that transmit signals in a form of electromagnetic pulses by the network interface of a switch or a router

It has an inner conductor such as copper or aluminum that runs down the middle of the cable, transferring signal

The conductor is surrounded by a layer of insulation made from polyvinyl chloride (PVC)

Which is then surrounded by conducting Shielding Wire mesh from copper/aluminum, making this type of cabling resistant to outside interference

The outer conductor is often made of copper or aluminum and may be covered with a metal foil or wire to provide better protection against interference.

The entire cable is wrapped in a protective jacket called Outer insulator, which shields the inner cable structure from mechanical damage and external influences such as moisture, dust, and temperature effects. Maximum cable length is 500m with speed up to 1000Mbps

For cheaper tarifs, ISPs often employ coaxial cabling to connect home modems into the internet

![](<../.gitbook/assets/Unknown image (407)>)

#### Common coax types (RG)

* RG-6: Typically used for cable television (CATV), satellite TV, and broadband internet connections.
* RG-59: Often used for analog video signals, such as CCTV cameras and older television installations.
* RG-58: Commonly used for Ethernet and computer network connections.
* RG-11: Suitable for longer cable runs and higher-frequency applications, such as backbone cabling in large networks.

#### Connectors

* F-type Connector: Used primarily for cable television (CATV) and satellite TV connections - used by ISP's to connect home modems to the internet
* BNC Connector (Bayonet Neill-Concelman): Commonly used in networking, video surveillance, and amateur radio applications.
* N-type Connector: Often used in wireless and RF applications, such as Wi-Fi antennas and cellular base stations.
* SMA Connector (SubMiniature version A): Found in high-frequency applications like RF test equipment and GPS antennas.

![](<../.gitbook/assets/Unknown image (408)>)

### Twisted-Pair (TP) cabling

**TP (twisted-pair)** is metallic (made from copper) cable that transmit signals in a form of electromagnetic pulses. It is also referred to as **patch cord**

**UTP (Unshielded Twisted-Pair)** has four pair of wires twisted around each other to reduce crosstalk and outside electromagnetic interference (EMI)

The outer jacket of UTP cables is typically made from PVC (polyvinyl chloride), LSZH (low smoke zero halogen)

This type of cabling is common in current LANs; Maximum cable length is 100m

**STP (Shielded Twisted-Pair)** has an additional layer of insulation that protects data from EMI

**FTP (Foiled Twisted Pair)** have an overall foil shield surrounding all pairs of twisted wires but do not have individual shields for each pair

**SFTP (Screened Foiled Twisted Pair)** have an overall foil shield surrounding all pairs of twisted wires

![](<../.gitbook/assets/Unknown image (409)>)

Ethernet links operates on a parallel communication with higher data rates

They use packet-switching technology to transmit data in frames over a shared or dedicated medium operating on parallel communication with higher data rates.

The parallel communication is achieved using multiple pairs of wires within a single cable

**Registered jack (RJ) RJ45 connector** is a standardized male connector designed to connect devices with twisted pair Ethernet cables

RJ45 connector is an eight-position, eight-contact (8P8C) modular connector. It has eight conductors, which typically correspond to eight wires within the Ethernet cable

The RJ-45 plug is the male component, which is crimped at the end of the cable. As you look at the plug from the front, as shown in the figure, the pin locations are numbered from 8 on the left to 1 on the right.

**RJ45 Keystone Jack** is a component in a network device, wall, or patch panel. As you look at the socket from the front, as shown in the figure, the pin locations are numbered from 1 on the left to 8 on the right.

It is an RJ45 connector socket, it terminates cables at various points of the network, typically at wall sockets that terminate Ethernet cables routed in the wall to an active network element such as a switch, or it is also used in patch panels that allow better organization of large amounts of cable in network rooms

The jack is the female component in a network device, wall, or patch panel. As you look at the socket from the front, as shown in the figure, the pin locations are numbered from 1 on the left to 8 on the right. It can also be used to extend short RJ45 cables

![](<../.gitbook/assets/Unknown image (410)>)

![](<../.gitbook/assets/Unknown image (411)>)

### RJ45 and structured cabling

**Structured cabling** is installed during the building construction phase in modern business environments. The cables are routed from the wiring closet to the sockets in the walls or on office desks. To connect to the local network, you would connect the computer's network adapter, which has a female RJ45 jack, to the wall socket, which also has a female RJ45 jack, by using a UTP cable with a male RJ45 connector on both ends.

The figure shows a typical office structured cabling installation. Notice that the cable from the wall socket to the wiring closet is called horizontal cabling. Vertical cabling is the term usually reserved for links between wiring closets.

![](<../.gitbook/assets/Unknown image (412)>)

#### Pins and pairs

**PIN** = conductive contact

**RJ45 connector** has 8 pins, which correspond to four twisted pairs of UTP cables

Each pin has its own number (1 to 8) and is used to transmit electrical signals

Both standard use pin 1 and 2 for sending signal - TX and 3 and 6 for receiving signal - RX

![](<../.gitbook/assets/Unknown image (413)>)

![](<../.gitbook/assets/Unknown image (414)>)

### UTP layout types

There are two types of cables, straight-through UTP cable layout and crossover UTP cable layout

The straight-through cable has the same standard (either the A or the B one) used on both ends and therefore the pin order on both ends is the same

This connected the sending pins (TX) of one device to the receiving pins (RX) of the other device.

In a crossover cable, the wiring on one end follows the T568A standard, while the other end follows the T568B standard. This creates a crossing of the transmit and receive pairs

This allows to connect devices that have identical pins for receiving and transmitting the signal

Computers and routers use wires 1 and 2 to transmit data and wires 3 and 6 to receive data, do it is necessary that the distribution of these wires into pins on both sides is differentso that when the computer sends a signal to the router from pins 1 and 2, the router receives it on the receiving pins 3 and 6, therefore, the straight through could not be used because it connects the receiving and transmitting of the signal at both ends equally, therefore, a crossover layout must be used, where the transmitting wire 1 and 2 is connected on the other side to pins 3 and 6, so that the router is able to expect to receive the signal

Hubs and switches use wires 1 and 2 to receive data and wires 3 and 6 to send data, so we can connect the computer with the switch or hub or a router with a switch or hub with a straight through the cable, because those devices have switched receive/transmit layout

![](<../.gitbook/assets/Unknown image (415)>)

![](<../.gitbook/assets/Unknown image (416)>)

#### Auto MDI-X

**Auto MDI-X** is a technology on a NIC card that automatically detects and adjusts the tx and rx based on the connected cable, which removes the need for a specific cable type between devices

![](<../.gitbook/assets/Unknown image (417)>)

### IEEE Ethernet standards (copper)

**Ethernet** is defined in a number of IEEE 802.3 standards. These standards define the physical and data-link layer specifications for Ethernet

#### Naming convention

First number in the name of the standard represents the speed of the network in megabits per second

The middle defines whether Baseband or Broadband is used as a transmission method

The last part of the standard name refers to the medium cabling used to carry signals and its Physical Coding Sublayer (PCS) encoding method,which is responsible for preparing data for transmission over the physical medium, it is handled by the hardware interated in NIC

During the encoding process for various purposes such as maintaining synchronization, ensuring DC balance, and enabling error detection

These additional bits are then used by the receiving end for various purposes, including error detection and, if necessary, correction

T = twisted pair, -T1 = single-pair twisted pair

S = 850 nm short wavelength (multi-mode fiber), L = 1300 nm long wavelength (mostly single-mode fiber), E or Z = 1500 nm extra long wavelength (single-mode)

B = bidirectional fiber (mostly single-mode) using WDM, P = passive optical (PON), C = copper/twinax, K = backplane, 2 or 5 or 36 = coax with 185/500/3600 m reach (obsolete)

F = fiber, various wavelengths, H = plastic optical fiber

X for 8b/10b block encoding (4B5B for Fast Ethernet) every 8 bits of data are converted into a 10-bit symbol before being transmitted

R for large block encoding (64b/66b) With 64b/66b encoding, every 64 bits of data are converted into a 66-bit block before transmission - more efficient in terms of bandwidth utilization and error detection

There might be also specified number after the medium and PCS type:

1, 2, 4, 10 – for LAN PHYs indicates number of lanes used per link; for WAN PHYs indicates reach in kilometers

Example 1000Base-T means that the speed of the network is up to 1000 Mbps, baseband signaling is used, and the twisted-pair cabling will be used

![](<../.gitbook/assets/Unknown image (418)>)

#### 1000BASE-T

Type: Twisted-pair copper cable.

Medium: Cat 5e, Cat 6, or higher.

Range: Up to 100 meters.

Usage: Gigabit Ethernet over standard Ethernet cabling.

Connector: RJ45.

#### Twisted pair Ethernet standards

**CAT (Category) 3**

10Base-T (IEEE 802.3) – 10 Mbps with category 3 unshielded twisted pair (UTP) wiring, up to 100 meters long. Used in older telephone systems and early Ethernet networks.

**CAT 5**

100Base-T (IEEE 802.3u) – known as Fast Ethernet, uses category 5, 5E, or 6 UTP wiring, up to 100 meters long.

**CAT 5e (enhanced)**

1000Base-T (IEEE 802.3ab) – Gigabit Ethernet that uses Category 5 UTP wiring, up to 100 meters long.

**CAT 6**

1000Base-TX (IEEE 802.3ab) – Gigabit Ethernet that uses Category 6 UTP wiring, up to 100 meters long.

**CAT 6a (Augmented)**

10GBase-T (IEEE 802.3an) – known as 10 Gigabit Ethernet supports 10 Gbps connections over Category 6a UTP cables.

**CAT 7**

support data transfer speeds up to 10 Gbps and provide enhanced shielding for reduced interference.

**CAT 8**

support data transfer speeds up to 25 or 40 Gbps over short distances, used in high-speed data centers and demanding environments.

![History of Ethernet LAN Cables’ Categories](<../.gitbook/assets/Unknown image (419)>)

![What is Single Pair Ethernet?](<../.gitbook/assets/Unknown image (420)>)

### Metalic transceivers

**Metallic transceivers** convert the electrical signals from the switch's internal circuitry into a format suitable for transmission over metallic mediums, such as twisted pair or coaxial cables. It also performs the reverse operation, converting incoming electrical signals from the cable into a format that the switch's internal circuitry can understand.

includes components such as line drivers, which amplify the outgoing signals, and receivers, which detect and interpret incoming signal

It also handles tasks like signal modulation/demodulation, error detection and correction, and signal conditioning to ensure reliable communication over the metallic medium

Essentially each port on a switch typically includes both a metallic transceiver, however the external metalic transceiver helps to amplify the output signal from the switch port
