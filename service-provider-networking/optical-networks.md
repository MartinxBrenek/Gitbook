# Optical Networks

## Dense Wavelength Division Multiplexing (DWDM)

Transporting bandwidths beyond standard interface rates (10G, 40G, 100G) requires multiple channels. With traditional interfaces, this necessitates multiple fiber pairs, which can be both costly and scarce.

Dense Wavelength Division Multiplexing (DWDM) overcomes this limitation by enabling multiple channels to be transmitted over a single fiber pair, often proving more cost-effective than deploying additional fiber pairs.

Additionally, standard interfaces have inherent distance limitations (e.g., LX: 10 km, EX: 40 km, ZX: 80 km). Extending beyond these distances typically requires electrical regeneration at each intermediate point, usually through router or switch interfaces. DWDM, however, can support single-span distances of up to 250 km and, when amplified, can extend across multiple spans reaching thousands of kilometers—without requiring electrical regeneration.

Moreover, traditional interfaces constrain network topology to the physical fiber layout. Given the high cost and limited availability of fiber, metro and regional deployments often rely on ring topologies for efficiency. DWDM, particularly with ROADM, allows for flexible Layer 1 network topologies—such as hub-and-spoke or mesh—over existing fiber infrastructures, typically deployed in a ring configuration.

NB. # of Degrees of a node: # of Directions entering/coming out from a node.

![](<../.gitbook/assets/Unknown image (1248)>)

**Gray client signal** refers to a standard (non-DWDM) optical signal that typically operates at a single fixed wavelength, such as 850nm (for multimode) or 1310/1550nm (for single-mode). This signal is then aggregated or converted by a muxponder (multiplexing transponder) into multiple wavelengths for transmission over a DWDM (Dense Wavelength Division Multiplexing) network

![](<../.gitbook/assets/Unknown image (1249)>)

### DWDM components

Optical transceivers

DWDM multiplexer (MUX) and demultiplexer filters

Optical amplifiers

Transponders (wavelength converters)

Muxponders

Optical add/drop multiplexers (OADMs)

Note all components like MUX/DEMUX,amplifiers, transponder,muxponder and OADMS were separate devices in the older networks and the technology advancements collapsed them into single small factor SFP called coherent optics

![](<../.gitbook/assets/Unknown image (1250)>)

####

### Digital Coherent Optics (DCO)

DCO pluggable transceivers now offer compact, high-performance alternatives to traditional DWDM transponders, streamlining optical network deployments

It is an advanced optical communication technology that leverages coherent detection - tunable lasers and coherent receivers, digital signal processing (DSP), and advanced modulation formats (such as DP-QPSK and QAM) to enable high-speed, long-distance data transmission with improved spectral efficiency.

DCO pluggable SFP's completely replace the traditional transponders

Modern coherent optical systems integrate DSP directly into SFP transceivers, handling modulation, demodulation, signal encoding, and synchronization. This ensures accurate, high-performance transmission while compensating for fiber impairments like chromatic dispersion and polarization mode dispersion (PMD).

Unlike traditional direct-detection systems (which measure only signal intensity), coherent optics processes both amplitude and phase, supporting higher bandwidths (e.g., 400G/800G Ethernet) and extended reach (up to 1000 km). The integrated DSP enhances signal quality, noise resilience (higher SNR), and Forward Error Correction (FEC), making coherent optics the foundation of modern DWDM networks.

![](<../.gitbook/assets/Unknown image (1253)>)

Cisco QSFP-DD800:

Supports 800 Gbps per wavelength over single-mode fiber (SMF).

Ideal for ultra-high-bandwidth applications within data centers and core networks.

Limited reach, typically reaching up to 20 kilometers for optimal performance.

Cisco QSFP-DD400:

Supports 400 Gbps per wavelength over both SMF and multi-mode fiber (MMF).

Offers versatile performance for high-bandwidth applications in data centers, core networks, and access aggregation.

Reaches up to 40 kilometers with SMF and 10 kilometers with MMF.

Cisco QSFP28:

Provides 200Gbps per wavelength over SMF, a step-up from traditional 100Gbps SFP28 modules.

Offers longer reach compared to higher-speed options, reaching up to 80 kilometers with SMF.

![](<../.gitbook/assets/Unknown image (1254)>)

Coherent Optics standard specifications

ZR (400ZR) A standardized specification for coherent optical transceivers supporting 400 Gbps data rates over distances up to 80 km, aimed at data center interconnects and metro networks.

ZR+ An extension of the 400ZR standard with enhanced capabilities for longer distances and more flexible data rate configurations.

Bright ZR+ is cisco-specific enhancement. It is able to launch at a much higher (+1dBm) transmit power. Even with this new version of the 400G ZR+ the QSFP-DD OLS still provides value as it further extends the reach of the 400G Bright QSFP-DD optics beyond the 80 to 120 km limit and, more importantly, it enables the support of a multichannel line system directly in the router.

![](<../.gitbook/assets/Unknown image (1255)>)

Optical fiber types used in DWDM deployments

1. G.652 (Standard Single Mode Fiber - SMF)

Common name: SMF, B1.1 or B1.3 fiber

Dispersion @1550 nm: \~17 ps/nm/km

Attenuation @1550 nm: \~0.20–0.25 dB/km

Use case: Metro, short-haul, general-purpose DWDM

Notes: Most widely deployed; sensitive to nonlinear effects like four-wave mixing (FWM) in dense channel plans

2. G.653 (Dispersion-Shifted Fiber - DSF)

Dispersion @1550 nm: \~0 ps/nm/km

Attenuation: Similar to G.652

Use case: Older long-haul DWDM systems

Notes: Very susceptible to FWM; not recommended for modern DWDM; largely obsolete

3. G.654 (Cutoff Shifted Fiber / Ultra-Low Loss)

Dispersion @1550 nm: \~20 ps/nm/km

Attenuation @1550 nm: \~0.17 dB/km or lower

Use case: Very long-haul, subsea, undersea cables

Notes: Optimized for low attenuation and high power; large effective area reduces nonlinearities

4. G.655 (Non-Zero Dispersion Shifted Fiber - NZDSF)

Dispersion @1550 nm: \~4–7 ps/nm/km

Attenuation @1550 nm: \~0.22–0.25 dB/km

Use case: Long-haul DWDM

Notes: Balanced dispersion helps reduce FWM and nonlinearities; suitable for dense channel spacing

Subtypes:

G.655.A: Positive dispersion

G.655.B/C: Optimized for better system compatibility

5. G.656 (Medium Dispersion Fiber)

Dispersion range: 6–13 ps/nm/km

Use case: DWDM in extended L-band (1565–1625 nm)

Notes: Designed for use beyond C-band; good balance between low dispersion and low nonlinear effects

6. G.657 (Bend-Insensitive Fiber)

Dispersion: Similar to G.652

Attenuation: Comparable to G.652

Use case: FTTH, metro access networks

Notes: Not typically used in long-haul DWDM, but sometimes used in patching areas; high bend tolerance

| **Fiber Type** | **Dispersion @1550 nm** | **Attenuation**   | **DWDM Suitability** | **Notes**            |
| -------------- | ----------------------- | ----------------- | -------------------- | -------------------- |
| **G.652**      | \~17 ps/nm/km           | \~0.20–0.25 dB/km | Metro/Short-Haul     | Most common          |
| **G.653**      | \~0 ps/nm/km            | \~0.25 dB/km      | Obsolete             | High FWM risk        |
| **G.654**      | \~20 ps/nm/km           | \~0.17 dB/km      | Long-Haul/Subsea     | Ultra-low loss       |
| **G.655**      | \~4–7 ps/nm/km          | \~0.22–0.25 dB/km | Long-Haul            | Balanced dispersion  |
| **G.656**      | 6–13 ps/nm/km           | \~0.23 dB/km      | L-Band DWDM          | Mid-range dispersion |
| **G.657**      | \~17 ps/nm/km           | \~0.20 dB/km      | Not typical          | Bend-insensitive     |

In a classic point-to-point (P2P) grey optical connection — meaning non-DWDM, single lambda, no wavelength multiplexing — the standard and most commonly used fiber type is:

G.652.D (Standard Single Mode Fiber - SMF)

Key Reasons:

Optimized for 1310 nm window (low dispersion and low attenuation).

Widely available and cost-effective.

Compatible with direct-detect transceivers (e.g., 1G/10G/25G LR optics).

Low chromatic dispersion at 1310 nm, which is ideal for grey optics.

Summary:

Fiber type: G.652.D

Typical wavelengths: 1310 nm (sometimes 1550 nm for longer P2P links)

Transceivers: 1G LX, 10G LR, 25G LR, 100G LR4, etc.

Distance: Up to 10–40 km typical without amplification

Application: Campus links, metro access, enterprise point-to-point, data center interconnect (short haul)

#### Multiplexer (MUX) and demultiplexer (DEMUX) filters (CMD)

Multiple wavelengths created by multiple transmitters operating on different fibers are combined onto one fiber by way of an optical filter (multiplexer filter). The output signal of an optical multiplexer is referred to as a composite signal (or composite beam)

The demultiplexing process takes the following steps:

An optical demultiplexer receives a composite beam containing multiplexed signals, usually differentiated by their wavelengths.

Inside the device, the light beam interacts with various optical components such as prisms, gratings, or filters.

These components separate the different wavelengths based on their unique characteristics, such as color or frequency.

Each separated wavelength is then directed to a separate output port, effectively demultiplexing the original signal.

There are different types of demultiplexers.

Passive demultiplexers: These rely on purely physical principles like refraction and diffraction to separate wavelengths. They are generally simpler and more cost-effective but may have limitations in terms of channel isolation and wavelength accuracy.

Active demultiplexers: These employ electronic components like tunable filters or acousto-optic modulators to achieve precise and dynamic separation of wavelengths. They offer greater flexibility and wavelength accuracy but are typically more expensive and complex.

Mux and demux components are usually within one device, allowing bidirectional (transmit and receive) operation

This requires the use of a pair of optical fibers; one for transmit, one for receive.

Arrayed Waveguide Filter (AWG) is a type of optical filter multiplexer/demultiplexer that works based on interference and diffraction principles. It consists of a series of waveguides that are precisely arranged and designed to split or combine signals at different wavelengths

![](<../.gitbook/assets/Unknown image (1256)>)

![](<../.gitbook/assets/Unknown image (1257)>)

![](<../.gitbook/assets/Unknown image (1258)>)

#### Optical amplifiers

Optical amplifiers are in-fiber devices used to increase the amplitude of passing light pulses, allowing for transmission across greater distances. They achieve this by stimulating the photons of the signal with extra energy to extend the reach of optical networks.

This is particularly important for longer distances where the signal can weaken due to attenuation (loss of signal strength).

Optical amplifiers amplify the amplitude of optical signals (gain is measured in dB).

Optical receivers require acceptable OSNR values to distinguish signals from system noise.

How do optical amplifiers differ from repeater-type devices? Optical amplifiers amplify optical signals without optical-to-electrical conversion.

![](<../.gitbook/assets/Unknown image (1259)>)

Optical amplifiers generate a small amount of noise internally due to physical limitations (like spontaneous emission). This noise is added to the signal as it gets amplified.

The main source of noise in an EDFA is Amplified Spontaneous Emission (ASE). As the erbium ions in the fiber are pumped to higher energy levels, they spontaneously emit photons. These photons get amplified along with the signal, contributing to the noise.

Think of an EDFA as a loudspeaker amplifying music:

Signal (Desired Music): The optical signal represents the music being played.

Noise (Unwanted Background Hiss): The EDFA amplifies both the music and a faint background hiss, just like a speaker increases both sound and noise.

The speaker adds its own hiss as it plays louder (this is like ASE noise being added by the EDFA).

If the music is weak (low signal), the hiss becomes more noticeable compared to the music. This explains why low Optical Signal-to-Noise Ratios (OSNR) cause errors.

Designers need to carefully plan systems to:

Minimize the number of amplifiers in the signal path.

Use high-quality amplifiers with low noise generation.

Use techniques like pre-amplification or inline amplification to keep the OSNR acceptable.

When you use several amplifiers in a row (cascade them):

The noise from earlier amplifiers gets amplified again by the next amplifiers.

Each amplifier adds more of its own noise to the signal.

Over time, the noise grows much faster than the signal strength.

Why is this a problem?

If the noise grows too much, it reduces the OSNR.

A lower OSNR means the receiver may struggle to distinguish between the actual signal and the noise.

If the OSNR gets too low, the receiver makes errors in interpreting the signal, causing bit errors.

Design Considerations

Designers need to carefully plan systems to:

Minimize the number of amplifiers in the signal path.

Use high-quality amplifiers with low noise generation.

Use techniques like pre-amplification or inline amplification to keep the OSNR acceptable.

Erbium-Doped Fiber Amplifier (EDFA) is the most commonly used type of in-fiber optical amplifier that amplifies optical signals directly without the need to first convert them into electrical signals

• Erbium-Doped Fiber: Core of the EDFA. It is made by doping the rare earth element Erbium into quartz optical fiber.

• Pumping Laser: Used to raise energy for Erbium. 1480nm wavelength is proved to work best, followed by 980nm wavelength.

• Isolator: Used to restrain the optical lights from reflecting and ensure the optical amplifier works stably.

• Coupler: Used to couple optical lights and pump lights into Erbium-doped fiber.

• Optical Filter: Narrow band-passed optical filter with bandwidth within 1nm. Used to eliminate the spontaneous emission light of the EDFA amplifier to reduce the EDFA noise.

Basic Working Principles of EDFA Amplifier

The working principle of an EDFA amplifier is based on the stimulated emission of photons. Here's a step-by-step explanation of the process:

Doping the Fiber: A segment of optical fiber is doped with erbium ions (Er3+). This doping process involves infusing the fiber with erbium atoms, which can later interact with incoming light signals.

Pump Laser: The doped fiber is then pumped with a high-power laser light at a wavelength of either 980 nm or 1480 nm. This pump light excites the erbium ions from their ground state to a higher energy state.

Signal Injection: The weak optical signal that needs amplification is introduced into the erbium-doped fiber.

Stimulated Emission: When the weak signal light interacts with the excited erbium ions, it stimulates the ions to drop back to their ground state, releasing their excess energy in the form of additional photons at the same wavelength as the incoming signal. This process amplifies the signal light.

Output: The amplified signal exits the fiber with much higher intensity than it had when it entered.

Detailed Mechanism

Energy Levels: Erbium ions in the fiber have specific energy levels. When pumped with light at 980 nm or 1480 nm, electrons in the erbium ions transition from the ground state (E1) to an excited state (E3 or E2).

Metastable State: Electrons in the excited state (E3) quickly decay to a lower energy state (E2), which is a metastable state. This state has a relatively long lifetime, allowing the erbium ions to store energy temporarily.

Stimulated Emission: When a photon from the incoming signal light (around 1550 nm) passes through the doped fiber, it stimulates the excited electrons in the erbium ions to return to the ground state (E1), emitting additional photons with the same phase and direction as the incoming signal. This results in signal amplification.

![EDFA Selection Guide - Fiber Cabling Solution](<../.gitbook/assets/Unknown image (1260)>)

![the components of EDFA](<../.gitbook/assets/Unknown image (1261)>)

1. Why Er-doped fiber amplifier is most widely used? Which rare-earth ions can also be used in the optical amplifier except for Erbium?

Except for the Er-doped fiber amplifier, there are also Pr-doped, and Tm-doped fiber amplifiers.

• EDFA (Er-doped Fiber Amplifier) Works at 1550nm wavelength.

• PDFA (Pr-doped Fiber Amplifier) Works at 1300nm wavelength.

• TDFA (Tm-doped Fiber Amplifier) Works at 1400nm wavelength.

The working wavelength of EDFA is exactly the minimum-loss window of optical communication, which is one of the reasons why EDFA is most widely used.

2. Why 980nm and 1480nm are chosen as the pump source?

The wavelengths of the source can be 520nm, 650nm, 980nm, and 1480nm, but the practice has proved that the pump efficiency of 980nm and 1480nm is higher than others. The pump efficiency of EDFA pumped at 1480nm is higher than that at 980nm. However, the 980nm pump source has a lower noise figure, so if you need better noise performance, 980nm is more recommended.

Optical amplifiers are used in three locations within a DWDM system or path:

Preamplifiers boost signal levels at the end of a span, just before a demultiplexer or OADM.

![](<../.gitbook/assets/Unknown image (1262)>)

Mid-span (line amplifier)

![](<../.gitbook/assets/Unknown image (1263)>)

Post-amplifiers operate at the beginning of a DWDM path. This includes placement after a DWDM multiplexer or after an OADM. A post-amplifier is located after a multiplexer. In our system, it is called a booster amplifier.

![](<../.gitbook/assets/Unknown image (1264)>)

**Semiconductor optical amplifier (SOA)**

Semiconductor optical amplifier is one type of optical amplifier which use a semiconductor to provide the gain medium. They have a similar structure to Fabry–Perot laser diodes but with anti-reflection design elements at the end faces. Unlike other optical amplifiers SOAs are pumped electronically (i.e. directly via an applied current), and a separate pump laser is not required.

**Raman amplifier**

Amplifiers based on RAMAN technology are available for amplifying long and ultra-long paths.

The RAMAN amplifier uses the intrinsic properties of silicon fibers to amplify the signal, so that the transmission fibers themselves can be used as the amplification medium, which allows the attenuation of the optical signal transmitted over the fiber in the fiber itself to be alleviated.

Optical signal is amplified due to stimulated Raman scattering (SRS). In general, FRA can is divided into lumped type called LRA and distributed type called DRA. The fiber gain media of the former is generally within 10 km. In addition, it requires on higher pump power, generally in a few to a dozen watts that can produce 40 dB or even over gains. It is mainly used to amplify the optical signal band of which EDFA cannot satisfy. The fiber gain media of DRA is usually longer than LRA, generally for dozens of kilometers while pump source power is down to hundreds of megawatts. It is mainly used in DWDM communication system, auxiliarying EDFA to improve the performance of the system, inhibiting nonlinear effect, reducing the incidence of signal power, improving the signal to noise ratio and amplifing online.. It is more expensive and implemented only when required

![](<../.gitbook/assets/Unknown image (1265)>)

#### Transponders

**Transponders** are optical-electrical-optical (O-E-O) wavelength converters connected to the end devices like routers and switches, converting electrical signals (e.g. Ethernet) to optical signals

Transponders can convert one wavelength of light into another wavelength. They are necessary when the wavelengths used by the transmitter and receiver are incompatible or when the network requires wavelength conversion for specific purposes.

Transponders are classified by the range of wavelengths that can be handled at the inputs and outputs.

A transponder is located between a client device and DWDM system - two applications: before optical multiplexers on the transmit side or/and after demultiplexers on the receive side. From left to right, the transponder receives an optical bit stream operating at a particular wavelength.

The transponder converts the operating wavelength of the incoming bit stream to an ITU-compliant wavelength. It transmits its output into a DWDM system.

On the receive side (right to left), the process is reversed. The transponder receives an ITU-compliant bit stream and converts the signals back to the wavelength used by the client device.

On the client side, there can be SONET or SDH terminals, or add/drop multiplexers (ADMs), Asynchronous Transfer Mode (ATM) switches, routers, or devices operating with a number of different protocols and bit rates.

Transponder converting incoming optical signals into the precise ITU-standard wavelengths to be multiplexed, transponders offer an additional interface into DWDM systems.

An optical receiver detects incoming optical signals and converts them to an electrical equivalent. The signals are processed electronically.

An optical transmitter then creates a new light pulse at an ITU-compliant wavelength.

A transponder performs an O-E-O operation to convert wavelengths of light. Within the DWDM system, a transponder converts the client optical signal back to an electrical signal (O-E) and then performs either 2R (reamplify and reshape) or 3R (reamplify, reshape, and retime) functions.

Note Transponders can also encrypt traffic at the layer 1

![](<../.gitbook/assets/Unknown image (1266)>)

![](/broken/files/cc854d512094caf6c55d63b48aebe378f51560d2)

#### Muxponder

**Muxponder** are similar to transponders, but unlike transponders that can only carry a single signal per wavelength, the muxponder combines multiple lower-speed client signals (like 10G Ethernet or Fibre Channel) into a single high-speed optical wavelength (e.g. 100G or 200G) using OTN framing (G.709) and digital signal processing. Instead of traditional TDM, it maps each client into an OTN container, multiplexes them byte-wise in the electrical domain, and sends the combined signal over one coherent DWDM wavelength. At the receiving end, the signal is demodulated and each client stream is extracted and delivered individually.

![](<../.gitbook/assets/Unknown image (1267)>)

**Crossponder (X)**

Combines the functionality of a muxponder and a transponder.

Aggregates multiple signals (like a muxponder) and can also convert or adapt protocols (like a transponder).

Supports advanced functionalities like signal grooming or regeneration across wavelengths

Note these terms are often interchanged, and many cards can support multiple

**Alien Wavelength (A)** A foreign wavelength that is not generated by the DWDM system itself but comes from an external source (e.g., another vendor's DWDM system or a non-native transceiver).

Transponder: One client signal → One wavelength.

Muxponder: Multiple client signals → One wavelength.

![](<../.gitbook/assets/Unknown image (1268)>)

### ROADM (Reconfigurable optical add/drop multiplexer)

Legacy optical networks deploy SDH/SONET technologies for transporting data across the optical network. These networks are relatively easy to plan and to engineer. New network elements can be easily added to the network. Static WDM networks may require less investment in equipment, especially in metro networks. However, the planning and maintenance of those networks can be a nightmare as engineering rules and scalability are often quite complex.

Bandwidth and wavelengths must be pre-allocated. As wavelengths are bundled in groups and not all groups are terminated at every node, access to specific wavelengths might be impossible at certain sites. Network extensions might require new Optical-Electrical-Optical regeneration and amplifiers or at least power adjustments in the existing sites. Operating static WDM network is manpower intensive.

It is often desirable to be able to switch (remove or insert) one or more wavelengths at some point along a DWDM span. An optical add/drop multiplexer (OADM) performs this function - it is essentially DWDM switch. OADMs act like highway exits and entrances for light signals. They allow specific wavelengths (colors) of light to be added or removed from a fiber-optic cable, while the rest of the traffic continues uninterrupted.

OADMS allows individual or multiple wavelengths carrying data channels to be added or dropped from a transport fiber without having to convert the signals on all WDM channels to electronic signals and back again to optical signals

OADM performs three main functions:

Add: New signals (wavelengths, e.g., λ1, λ2) are added to the network.

Drop: Specific signals (e.g., λ1) are removed from the network and sent to a connected device.

Pass-through: Signals go straight from one direction/degree (West → East or East → West) without modification.

Degree refers to one fiber link direction (i.e. one direction = one degree) it is also referred to as East or West, which is is standard in optical systems to uniquely identify inputs and outputs (e.g. for wavelength management in ROADMs).

East usually indicates the direction from the source or towards the next node in one part of the network.

West indicates the opposite direction (e.g. returning to the previous node).

Degree 1: A ROADM with a degree of 1 would have only one fiber connection.

Degree 2: A ROADM with a degree of 2 would have two fiber connections.

Degree 4: A ROADM with a degree of 4 would have four fiber connections.

Historically OADM systems were fixed, thus adding or dropping wavelengths required manually inserting or replacing wavelength-selective cards. This process was costly and, in some systems, requires that all active traffic is removed from the DWDM system, because inserting or removing the wavelength-specific cards interrupts the multiwavelength optical signal.

**ROADM** is a reconfigurable optical add/drop multiplexer that adds the ability to remotely switch traffic from a WDM system at the wavelength layer

A ROADM generally consists of two major functional elements: A wavelength splitter and a wavelength selective switch (WSS).

#### ROADM types

Simple ROADM is a basic type of ROADM that has fixed wavelengths assigned to specific ports.

Colorless ROADM: Any wavelength can be added/dropped on any port.

Directionless ROADM: The wavelength is not tightly bound to a specific direction.

Contentionless ROADM: Multiple channels with identical wavelengths can be used on different directions or ports simultaneously.

![](<../.gitbook/assets/Unknown image (1269)>)

#### Fixed filter ROADM architecture

The lowest cost but least flexible option with fixed wavelengths assigned to specific ports.

Initially, ROADMs utilized fixed-grid Wavelength Selective Switch (WSS) technology that functioned on a specific channel plan and spacing. These ROADMs were often based on either a 50 GHz or 100 GHz fixed grid. Each wavelength added to the network had to fit within this rigid channel spacing in order to pass through the ROADM. However, as coherent technology shifted to higher baud signals with wider channel sizes, wavelengths began to require more space than a fixed-grid 50 GHz or 100 GHz system could provide. Now ROADMs have evolved to support flexible grid, where individual channels can use different channel widths and spacing.

The fixed ROADM enables the ability to adjust which wavelengths are added and dropped, and it can redirect wavelengths that are passing through the site.

This architecture uses a fixed Channel Mux Demux filters, which follow a specific channel plan, forcing the network to adhere to a rigid channel spacing, for example 50 GHz, 75 GHz, or 100 GHz spacing.

![](<../.gitbook/assets/Unknown image (1270)>)

Furthermore, each filter port is fixed to a specific wavelength frequency, so wavelength1 may be connected to port 1 on the filter, but cannot be connected to any other port. Additionally, each filter is directly connected to a specific direction. As a result, when wavelengths are added to the network, they must be connected to the CMD and ROADM that faces the direction they need to go. To deploy wavelengths that require wider spacing in this network, new CMDs are required—and each of these would need to be connected to an available WSS port

![](<../.gitbook/assets/Unknown image (1271)>)

#### Colorless direct attach ROADM architecture

allows any wavelength (color of light) to be assigned to any port. This makes it more flexible compared to older systems where specific wavelengths were fixed to certain ports.

Coherent Colorless Multiplexer and Demultiplexer (CCMD): A component that enables the colorless feature. When adding or dropping wavelengths, CCMDs must be connected to the correct network direction. If traffic grows or new directions are needed, CCMDs can be added to handle more wavelengths or connections.

![](<../.gitbook/assets/Unknown image (1272)>)

#### Colorless, directionless ROADM architecture

adds the benefit of remotely switching the direction of add/drop wavelengths. This can be used to enable optical layer reconfiguration and restoration. If there is a failure, an optical control plane can redirect add/drop channels from one direction out of a node to a different, alternate direction. However, this architecture does not allow for full wavelength routing flexibility as it does not eliminate wavelength contention when multiple versions of the same color wavelength are being added at a single location to be sent in different directions. This architecture only allows for a single instance of each color wavelength to add/drop at each location.

![](<../.gitbook/assets/Unknown image (1273)>)

#### Colorless, directionless, contentionless ROADM (CDC) with flexible grid (CDC-F)

Colorless, contentionless, omnidirectional, and flex spectrum (CCOFS) - Cisco name

Let’s assume we want to insert a red wavelength in the West direction and one in the North direction. This is possible using a legacy ROADM. One transponder is connected to the West mux/demux and another transponder is connected to the North mux/demux.

The red wavelength can be added to every direction using separate transponders and these wavelengths can be kept physically separate.

As we introduced Directionless ROADM, we have collapsed the add/drop complex into a single device—the Wavelength Selective Switch (WSS)—which dynamically provides access to all ROADM degrees by routing wavelengths without fixed fiber connections

One consequence of the Directionless ROADM is that the add/drop complex is a single point where all the add/drop wavelengths are present. As a result, any color (wavelength) can only be used once, and be routed in a single direction within a single add/drop complex.

For example, if a service provider adds a red wavelength in the West direction, then, a red wavelength cannot be added to any other direction from that directionless module, otherwise it wold lead to wavelength blocking scenarios – or contention

Contentionless means that the same wavelength (i.e. the same frequency of light) can be used for multiple directions in the same ROADM platform without conflict.

![](<../.gitbook/assets/Unknown image (1274)>)

Contentionless ROADM provides a single add/drop complex with colorless, directionless and gridless functionality and the ability to add and drop multiple instances of the same wavelength, or color, on the same drop complex as shown below.

The contentionless nature of the add/drop complex allows the “coexistence” of multiple instances of the same color. The only limitation is the fact that each instance of the same color has to be routed to different directions.

It basically allows the same incoming wavelength to add/drop off of a single CCMD for different directions. In other words, port 1 on the CCMD could be wavelength1 for the east direction, and port 2 on the CCMD could be wavelength1 (i.e., same frequency) for the west direction.

This means that a CDC ROADM node has no restrictions or limitations with respect to wavelength assignment or routing, and it can be used for photonic layer network optimization and dynamic re-routing of wavelengths for traffic restoration

![](<../.gitbook/assets/Unknown image (1275)>)

The CDC ROADM node retains all of the benefits of the other flexible-grid ROADM types, such as the ability to use any coherent technology to gain improvements in spectral efficiency and cost per bit. It provides flexible channel width and spacing and grants directionless capability without wavelength blocking restrictions. CDC ROADM nodes require more sophisticated WSS equipment than other options which increases the overall cost, but they also provide the most flexible and programmable ROADM architecture available today.

Modern ROADM networks can be used to automate the configuration of add/drop ports at a site and easily scale to accommodate new fiber routes. Depending on the architecture, ROADMs can dramatically reduce the amount of wavelength routing and assignment pre-planning that is required for WDM networks. ROADMs can be used in ring or mesh architectures. With the ability to re-route wavelength paths across the network, ROADMs can be used to provide optical layer restoration.

Full CDC-G ROADMs obviously provide the maximum amount of flexibility. The ROADM is fully colorless, directionless, contentionless, and gridless, and there is no risk of wavelength blocking.

This flexibility typically comes at a cost. CDC-G solutions are more expensive and in some cases, their architecture require higher amplification levels leading to lower optical reachability.

Flexible-grid ROADMs future-proof the photonic layer, ensuring that it is compatible with any new coherent technology regardless of the baud being used. Flexible-grid channel spacing works with next-gen coherent modems for high-rate signals, including 800G, to achieve maximum spectral efficiency for WDM applications, and opens the line system to provide the flexibility to use any vendor’s coherent optics.

Flexspectrum/Flex-grid removes the fixed cubbyhole walls and lets the service provider define the spectral width of each wavelength independently

The channel’s spectral width must be a multiple of 12.5GHz. This is commonly referred to as “n x 12.5 GHz”. For example, a wavelength can be assigned a spectral width of:

37.5 GHz = 3 x 12.5 GHz

75.0 GHz = 6 x 12.5 GHz

The channel’s center frequency has to be located on a fixed grid with 6.25 GHz spacing. This means that every channel’s center frequency is located on fixed, pre-assigned positions located 6.25 GHz from each other.

Perhaps the most basic rule is that adjacent channels cannot overlap. While the ROADM software typically manages spectrum allocation and automatically prevents overlaps, Flex-grid allows for manual spectrum planning

![](<../.gitbook/assets/Unknown image (1276)>)

Wavelengths typically require at least a 50 GHz spectral width in order to provide a minimum of 100 Gb/s capacity over a typical metro DWDM network. An operator should be careful in ensuring that they are not leaving unassigned spectrum components between wavelengths that are not at least 50 GHz. The presence of small slices of unusable spectrum is commonly referred to as “spectrum fragmentation”

One approach commonly used is to limit the spectral widths used within a network to 50 GHz, 75 GHz and 100 GHz in order to reduce the likelihood of spectrum fragmentation. However, this approach has the consequence that the network has to be optimized around 30 Gbaud and 60 Gbaud transponders.

![](<../.gitbook/assets/Unknown image (1277)>)

Components of CCOFS/CDC ROADM Architecture:

WSS (Wavelength Selective Switch) is used for switching wavelengths between different ROADM degrees. It provides directionless capabilities but is not contentionless on its own.

MCS (Multicast Switch) is the actual component that enables contentionless add/drop by allowing multiple transponders to share the same wavelength without conflicts. It prevents wavelength blocking by broadcasting the dropped wavelength to multiple transponders, ensuring no single transponder is locked to a specific wavelength.

Optical Cross-Connect (OXC): The OXC further enhances the flexibility of a CDC-F ROADM by enabling more complex routing configurations. It interconnects multiple fibers and directions in a highly flexible manner, allowing wavelengths to be freely added or dropped across different network segments.

Flexible Grid Multiplexer: The flexible grid technology, as mentioned earlier, allows for dynamic spacing of channels, optimizing the use of available spectrum. By adjusting the spacing between channels, operators can efficiently allocate bandwidth depending on the needs of specific services, reducing wastage and maximizing the available capacity.

Array Card: Contains fixed gain amplifiers to compensate for signal losses within MCS.

![](<../.gitbook/assets/Unknown image (1278)>)

20-port Flex Spectrum SMR – Block Diagram

![](<../.gitbook/assets/Unknown image (1279)>)

![](<../.gitbook/assets/Unknown image (1280)>)

Step-by-Step Signal Flow:

Signal Input comes from West:

The Software-Controlled Selectors make the decision on whether a signal:

Goes directly to the splitter and continues as a pass-through signal to the East side.

Or is diverted to the Demux for further processing (adding or dropping).

The DWDM signal (a multiplexed signal containing multiple wavelengths) enters the splitter.

The splitter sends one part of the signal directly to the output on the East side. This is the pass-through signal.

The other part goes to the Demux for further processing.

The incoming signal is demultiplexed into individual wavelengths (e.g., λ1, λ2, λ3).

A specific wavelength (e.g., λ1) is sent to the host device through a DWDM module - Devices that convert electrical signals into optical ones (for adding) or optical into electrical (for dropping).

The host device, which is a router, switch, or transponder that works with individual wavelengths (e.g., data on λ1) can process the dropped signal (e.g., routing it to a different destination)

If the host device generates a new wavelength (e.g., λ2), it sends it through a DWDM module back into the ROADM.

The new wavelength is multiplexed with other signals.

The combined signal is sent to the output, heading to the next direction (East)

Broadcast-and-Select ROADM Architecture:

This architecture relies on broadcasting all wavelengths (optical channels) at each node/degree and then selecting the required channels for output.

WSS or optical filters are then used to select the desired wavelength(s) for each output port.

Unwanted wavelengths are discarded.

Advantages:

Simplicity: Easy to implement because it uses passive optical components like splitters.

Flexibility: All wavelengths are available at every output port for selection.

Ease of expansion: New output ports can easily be added without significantly altering the design.

Disadvantages:

Signal loss: Splitting the optical signal reduces power, requiring amplification.

Inefficiency: Unwanted wavelengths are discarded, wasting resources.

Scalability issues: High splitter losses make it unsuitable for large networks.

Route-and-Select ROADM Architecture:

This architecture selectively routes only the desired wavelengths at each ROADM node, minimizing signal loss and improving efficiency.

How It Works:

At the input of the ROADM, a wavelength-selective switch (WSS) is used to route specific wavelengths to specific output ports.

Only the selected wavelengths are directed toward their intended destinations.

After routing, another WSS or filter may perform a final selection to ensure that only the desired channels are passed to the next node.

Advantages:

Efficiency: Only the required wavelengths are routed, minimizing wasted signal power.

Scalability: Better suited for large networks as it avoids the significant losses caused by signal splitting.

Improved signal quality: Reduced loss and less need for amplification compared to broadcast-and-select.

Disadvantages:

Complexity: Requires more sophisticated WSS and control mechanisms.

Higher cost: Uses more advanced components than broadcast-and-select

![](<../.gitbook/assets/Unknown image (1281)>)

#### ROADM card architecture

ROADM line cards use MPO (Multi-Fiber Push-On) connectors to efficiently manage multiple fiber pairs within a single interface. These MPO connectors allow the card to handle high-density fiber connections, enabling access to multiple degrees (directions) in the optical network. By using MPO, a single ROADM card can support multiple wavelengths across different fiber pairs, optimizing space, simplifying cabling, and enhancing scalability in colorless, directionless, and contentionless (CDC) architectures.

LINE Ports: Interface for incoming and outgoing optical signals.

ADD & DROP Ports: Allow specific wavelengths to be inserted or removed.

**OSC (Optical Supervisory Channel):** is a dedicated optical wavelength (typically 1510 ±5 nm) that carries network management, control, and monitoring traffic over the same fiber as the data-carrying DWDM wavelengths. Used for management and control.

Purpose:

Provides out-of-band communication for management between DWDM network elements (e.g., amplifiers, ROADMs).

Transports:

Alarms

Configuration updates

Performance data

Fault info

Inventory and topology discovery

Enables remote control and automation of optical layer (e.g., automatic power balancing, fault isolation).

**OTDR (Optical Time-Domain Reflectometer)** is a diagnostic tool used to test the integrity of optical fiber links by sending light pulses and measuring backscattered/reflected signals.

Fiber breaks or bends

Connectors and splice losses

Distance to faults

Overall fiber length and attenuation

Works like "fiber radar" — shows a graphical trace of signal loss vs. distance.

Used for:

Installation verification

Troubleshooting

Maintenance

Some DWDM systems (like NCS 1010) support integrated OTDR on OSC or OTS ports for real-time remote testing without external equipment.

DC (Directionless Connection): Enables flexible wavelength routing.

EXP (Express Ports): Used for pass-through wavelengths.

MON (Monitor Ports): Used for performance monitoring and troubleshooting. Blue line represents the dropped signal in the middle ROADM card of the middle site

![](<../.gitbook/assets/Unknown image (1282)>)

![](<../.gitbook/assets/Unknown image (1283)>)

#### ROADM network architecture

In a typical ROADM network, more than 80% of the sites are spur and ring sites with less than four fiber directions (degrees).

Hub sites tend to be busier, in all respects: the amount of traffic, the number of terminating and pass-through wavelengths, and the number of degrees. Over time, spectrum fragmentation can occur due to wavelength rerouting and service moves, adds and changes. Thus hub sites can benefit from contentionless ROADM capability that provides the maximum flexibility to handle wavelengths. This also holds true for busy ring sites supporting many wavelengths, whether it’s express or terminating traffic.

Example

You're adding/dropping 16 channels locally.

You have 2 line directions (east-west, or ring style).

Then you may use the WSS ports like this:

2 ports: Add/drop to transponders or passive mux

2 ports: Line direction 1 (to/from site A)

2 ports: Line direction 2 (to/from site B)

Total: \~6 WSS ports used, and all 16 wavelengths ride those ports.

![](<../.gitbook/assets/Unknown image (1284)>)

### DWDM end-to-end architecture (example)

1. Transponder Interfaces Are Per-Wavelength

A transponder like the NCS 1004 converts client signals (e.g., 100G Ethernet) to a specific DWDM wavelength using OEO (optical-electrical-optical) conversion.

But it does not combine multiple wavelengths into a single fiber; it only handles a single wavelength per port.

Why not skip MUX/DMX?

Because you'd need one fiber per wavelength, which defeats the purpose of DWDM.

2. MUX/DMX Combines and Splits Wavelengths

A multiplexer combines multiple DWDM wavelengths into one fiber for line transport.

A demultiplexer splits the wavelengths back out at the receiving end.

This allows full utilization of the fiber bandwidth (e.g., 40, 80, or 96 wavelengths per fiber).

3. ROADM ≠ Fixed MUX/DMX

A ROADM (like NCS 1010) can dynamically add/drop or pass through wavelengths at specific nodes.

However, it still needs MUX/DMX front-end optics or add/drop panels to:

Connect specific wavelengths to/from transponders.

Interface passive components or service interfaces at the node.

4. Flexibility and Cost-Effective Scaling

Passive MUX/DMX units (like MD32/48/64) allow incremental deployment of wavelengths.

You can deploy only the wavelengths you need and scale over time without replacing line cards or ROADMs.

5. Colorless, Directionless, Contentionless (CDC) Designs

When using colorless add-drop panels, MUX/DMX functionality allows you to connect any wavelength to any port, improving flexibility and operational simplicity.

Summary of Roles:

Component Role

Transponder Converts client signals to DWDM wavelengths

MUX/DMX Combines/splits wavelengths for fiber transport

ROADM Dynamically routes specific wavelengths across a mesh network

Line System Provides optical amplification, dispersion compensation, etc.

Conclusion:

You cannot remove the MUX/DMX stage without losing wavelength multiplexing capability. Transponders and ROADMs serve different roles and still rely on MUX/DMX to enable high-capacity transport on a single fiber pair.

1. Why add/drop and ROADM are mentioned in MUX/DMX context

Even though MUX/DMX is a fixed filter technology (i.e., passive), it plays a role in add/drop multiplexing similar to ROADM — but with less flexibility.

Add/Drop filtering means the module selectively passes or drops specific wavelengths (channels) to/from a DWDM line system.

In passive systems (like MD32), mux/demux filters are hardwired to drop certain fixed wavelengths — that's still a form of add/drop.

When connected to a ROADM-based system, these modules support colorless add-drop, where any wavelength can be routed to any port (with ROADM WSS modules and external filters/couplers).

So in essence:

Even fixed MUX/DMX modules perform a kind of add/drop operation — and when used with the NCS 1010 ROADM, they extend or complement its add-drop capability.

2. What “ODD” and “EVEN” mean in MD32 filters

The DWDM grid (ITU-T G.694.1) defines channels in 50 GHz, 100 GHz, 150 GHz, etc., spacing.

A 150 GHz spaced MD32 filter supports 32 channels.

The EVEN and ODD designation splits the full DWDM spectrum into two interleaved sets:

EVEN channels: e.g., 150 GHz grid channels like 20, 22, 24...

ODD channels: e.g., channels 21, 23, 25...

This lets you:

Build higher-density designs by using both modules together.

Support up to 64 channels (ODD + EVEN) for 400ZR/ZRP coherent optics (32+32 = 64 wavelengths).

So it's a way of splitting the DWDM spectrum across two modules to scale the number of channels

Building Arbitrary and Redundant Topologies

![](<../.gitbook/assets/Unknown image (1285)>)

More in: [https://www.ciscolive.com/c/dam/r/ciscolive/us/docs/2020/pdf/DGTL-BRKOPT-2007.pdf](https://www.ciscolive.com/c/dam/r/ciscolive/us/docs/2020/pdf/DGTL-BRKOPT-2007.pdf)

### DWDM pre-sales questions

What is the deadline to prepare the solution?

1. Topology & Use Case

* Is the network point-to-point, linear, ring, or mesh?
* Will it be reconfigurable (ROADM) or fixed (passive mux)?
* Is this a greenfield (new build) or brownfield (upgrade)?

2. Capacity Requirements

* What is the required client capacity? (e.g. 10x 10G, 2x 100G, 400G)
* How much DWDM capacity per span is needed (e.g. 40x100G)?
* Is future scalability required?

3. Client Interfaces

* What are the client-side interfaces? (10GbE, 100GbE, OTU4, Fibre Channel?)
* What form factors are needed (SFP+, QSFP28, etc.)?
* What is the client-side reach?

4. Line Interfaces / Optical Layer

* What is the line-side interface? (CFP2-DCO? Baud rate? Modulation?)
* What is the fiber type (e.g. G.652, G.655, G.654)?
* Is the fiber already in use by other systems?

5. Span Details

* What is the distance of each span?
* What is the attenuation per km (e.g. 0.25 dB/km)?
* What are the losses at connectors, splices, ODFs?
* Is fiber dispersion (e.g. for G.655) a concern?

6. Amplification & Regeneration

* Do you need amplifiers (EDFA/Raman)? If yes:
* Can they be placed mid-span, or only at terminals?
* Is optical regeneration (3R) required at long distances?

7. Mux/Demux & ROADM

* Will you use passive mux/demux or ROADM?
* How many channels (wavelengths) are needed?
* Grid type? (e.g. Fixed 50GHz, Flex Grid)

8. Power & Space Constraints

* DC or AC power? Voltage? Redundancy?
* Maximum rack space (RU) and shelf depth allowed?
* Cooling/airflow direction requirements?

9. Management & Control

* What NMS/EMS will be used? Cisco EPN-M? Others?
* Are OTN switching and performance monitoring required?
* Is SNMP/NETCONF integration needed?

10. Redundancy & Protection

* Will the network have path redundancy?
* Do you need optical protection switching (OPSM)?
* Client protection (1+1, Y-cable, etc.)?

11. Compliance & Regulatory

* Compliance with G.709, G.694.1, G.652/G.655?
* Power & grounding standards (ETSI, ANSI)?
* Country-specific regulations (e.g. energy telcos, critical infrastructure)?

12. Bill of Materials (BoM) Preparation

* What chassis type? (e.g. NCS 2006, 2015)
* Which cards: Transponders, Amplifiers, ROADM, Supervisors?
* What pluggables, connectors, patch panels, and licenses are needed?
* Delivery: all power cords, jumpers, transceivers?

## The example P2P DCI architecture:

![](<../.gitbook/assets/Unknown image (1286)>)

### Overview

**OTN** is an ITU‑T–defined set of optical network standards leveraging DWDM technologies. It ensures transport, multiplexing, switching, management, supervision, and resiliency of optical channels carrying client signals. Optical network elements (ONEs) provide support for retiming, reamplification, and reshaping (3R) of optical signals.

OTN specifies a **digital wrapper**: a method of encapsulating an existing frame of data (regardless of native protocol such as IP or Ethernet) to create an **Optical Data Unit (ODU)**. The wrapper is flexible in frame size and allows multiple existing frames to be wrapped together so they can be managed more efficiently with less overhead in a multi‑wavelength system. Digital wrapper rates have been defined for 2.5, 10, 40, and 100 Gb/s payloads. The resulting line rates are defined as **Optical Transport Units (OTUs)**.

OTN is predominantly used in backbone networks to support high‑capacity data transport. It is essential in scenarios such as long‑haul transmission, where data needs to be transmitted over long distances with minimal loss. An OTN's ability to encapsulate various types of network traffic makes it versatile for different applications — from internet backbones to cloud data transfer.

***

### Key OTN concepts (expandable)

<details>

<summary>Optical Payload Unit (OPU)</summary>

This layer contains the encapsulated client data and a header describing the type of data.

</details>

<details>

<summary>Optical Data Unit (ODU)</summary>

This layer adds optical path‑level monitoring, alarm indication signals, and automatic protection switching.

</details>

<details>

<summary>Optical Transport Unit (OTU)</summary>

This layer represents a physical optical port (such as OTU2, 10 Gb/s) and adds performance monitoring (for the optical layer) and the FEC.

</details>

<details>

<summary>Forward Error Correction (FEC)</summary>

FEC is a mechanism to detect and correct errors that may occur during data transmission, improving the reliability and quality of the link.

</details>

<details>

<summary>Optical Channel (OCh)</summary>

Represents an end‑to‑end optical path (a single colored wavelength on the fiber).

</details>

***

### Physical layers (OCh, OMS, OTS)

* **OCh (Optical Channel)**: Manages individual optical channels (OTSi) within the multiplexed spectrum.
* **OMS (Optical Multiplex Section)**: Handles the multiplexing and management of optical channels.
* **OTS (Optical Transmission Section)**: Refers to the physical layer, including optical fibers, amplifiers, and multiplexers.

(Images in page are embedded as base64 and left unchanged.)

***

### OTU / ODU rates (marketing vs true)

| OTU | ODU | Marketing Rate | True Signal (OTU) | True Payload (OPU) | Target Client Signals    |
| --- | --- | -------------: | ----------------: | -----------------: | ------------------------ |
| 0   | 0   |          1.25G |                NA |         1.238 Gb/s | Gigabit Ethernet         |
| 1   | 1   |           2.5G |        2.666 Gb/s |         2.488 Gb/s | OC‑48 / STM‑16           |
| 2   | 2   |            10G |       10.709 Gb/s |         9.953 Gb/s | OC‑192 / STM‑64, 10 GbE  |
| 3   | 3   |            40G |       43.018 Gb/s |        39.813 Gb/s | OC‑768 / STM‑256, 40 GbE |
| 4   | 4   |           100G |      111.809 Gb/s |       104.794 Gb/s | 100 GbE                  |

***

### Applications of OTN

* Long‑distance data transport (e.g., geographically dispersed data centers, cloud providers, CDNs)
* Mobile network backhaul (connecting base stations to core networks)
* Data center interconnect (DCI) — metro or longer‑haul DCI
* Enterprise network connectivity (campuses, sites)
* 5G and edge computing (high‑speed transport for mobile/edge services)

***

### Packet Optical Transport Systems

Packet Optical Transport Systems integrate optical transport and packet networking by encapsulating packet data into optical signals for efficient transmission over fiber.

Typical components:

* **OTN switches** — provide interface between optical and packet layers.
* **MPLS** — used to streamline packet traffic and manage paths.
* **Digital Signal Processors (DSPs)** — convert between packet data and optical signals, handling modulation, FEC, etc.

***

### OTN vs DWDM

* **OTN**
  * Multiplexing: aggregates lower‑speed client signals into ODU/OTU.
  * Adds FEC, OAM/monitoring, and management.
  * Can carry Ethernet, SONET/SDH, Fibre Channel, etc.
  * Operates at the transport/network layer (adds framing and operational features).
* **DWDM**
  * Multiplexing: combines many wavelengths (lambdas) on a single fiber.
  * Increases fiber capacity and supports long‑distance transparent transport.
  * Protocol‑agnostic; operates at the physical layer.

Typical deployment: an OTN device maps client signals into ODUs/OTUs and hands a colored wavelength (lambda) to a DWDM system. ROADMs then route these lambdas optically. If OTN switching is present, services can be switched at sub‑lambda (ODU) granularity before coloring.

***

### Example: NCS 1004 + ROADM

* The NCS 1004 handles client signal mapping and OTN framing; it produces colored OTU signals for DWDM.
* A ROADM routes whole lambdas optically across the DWDM network.
* If you have OTN switching in‑line, you can manipulate services at the ODU level (sub‑lambda) prior to optical coloring and ROADM routing.

***

If you’d like, I can:

* Convert any of the definitions above into a stepper flow for onboarding/implementation.
* Break out examples (e.g., mapping 10 GbE into ODU2/OTU2) as a worked example.
* Add an expandable FAQ for common operational questions.

## Challenges of legacy transport

Having different layers with their own management and control planes brings complexity and challenges to planning, building, and operating. Duplication of tasks between IP and optical teams, along with the complexity of network protection and restoration across layers, adds to operational challenges. Furthermore, interoperability issues and the need for hardware conversions between layers, such as OTN and DWDM, result in inefficiencies. For instance, the conversion of OTN and IP network traffic into wavelength signals for DWDM networks often necessitates dedicated external hardware, adding complexity and costs.

### Traditional network design

![](<../.gitbook/assets/Unknown image (820)>)

![](<../.gitbook/assets/Unknown image (821)>)

Despite a shared Fiber topology, a traditional architecture has two (2) distinct topologies for the IP and Optical layers given its complex mesh of wavelengths

![](<../.gitbook/assets/Unknown image (822)>)

### Traditional optical protection schemes

Optical transport networks (OTN, DWDM) use hardware-based redundancy to protect against fiber cuts or node failures. The main optical protection schemes are:

1+1 Protection: This approach provides route diversity by ensuring two dedicated paths for transmission. However, if a fiber cut occurs, it reduces capacity by 50% because traffic is split across the two paths.

1+1+R (Restoration): This method enhances reliability by adding an additional restoration path. When a fiber cut happens, traffic is dynamically rerouted via the restoration path, maintaining full capacity and improving resilience.

![](<../.gitbook/assets/Unknown image (823)>)

When a fiber cut occurs (e.g., at node 7 in the diagram), the network automatically reroutes traffic using the predefined restoration path (green path).

This allows traffic to continue without manual intervention, minimizing downtime.

No Coordination with the IP Layer

Unlike traditional protection mechanisms that may require higher-layer (IP/MPLS) rerouting, Optical Restoration works at the optical layer and can restore connectivity within minutes.

This makes the restoration process faster and transparent to the IP layer, preventing network-wide disruptions.

Challenges with Partial Path Overlap

The restoration path (green) shares some links with the original primary path (purple).

Once the failed link is repaired, the system must revert to the original path (purple) to maintain intended routing.

This raises an operational question: how do you track and manage the home path versus the restored path?

Optical Reversion is Hard

Automatic Reversion: The network can be configured to automatically revert to the primary path after a set time (Wait-to-Restore, WTR), but this can cause issues if not well-coordinated.

Manual Reversion: A more controlled approach is to schedule reversion events in coordination with the IP layer, ensuring minimal traffic disruption.

Multiple Circuits Issue: If several optical circuits revert at once, network instability may occur due to sudden traffic shifts.

![](<../.gitbook/assets/Unknown image (824)>)

## Routed optical networking (RON)

**Routed optical networking (RON)** (also called IP over DWDM, **IPoDWDM**) integrates the IP and optical layers into a single, cost-effective, and easily manageable infrastructure. It collapses the control plane and switching layer

RON architecture aims at making optical and IP topologies congruent which enables:

Optimal traffic forwarding for applications, content and Internet peers

Higher utilization of network assets, wavelengths and higher bit-rate wavelengths given their shorter distances

In routed optical networking, data packets are routed directly over optical wavelengths, bypassing optical layer. This direct routing reduces latency, enhances bandwidth usage, and simplifies the network, leading to improved performance and lower operational costs

[https://www.cisco.com/c/en/us/products/collateral/routed-optical-networking/routed-optical-networking-wp.html](https://www.cisco.com/c/en/us/products/collateral/routed-optical-networking/routed-optical-networking-wp.html)

![](<../.gitbook/assets/Unknown image (825)>)

#### IP / MPLS network utilization (traditional)

Uses more wavelengths per fiber span

Uses less traffic aggregation

Results in lower IP port/wavelength utilization

Traditional architecture splits traffic onto dedicated IP ports and wavelengths toward distant routers (based on destinations) in lower IP port/wavelength bit rates (due to longer distances)

Leads to:

Underutilized network assets

Over-investment in the physical infrastructure

Higher cost per bit

#### IP / MPLS network utilization (RON)

[https://www.cisco.com/c/en/us/products/collateral/routed-optical-networking/routed-optical-networking-wp.html#Arepeopleskillsarealproblemforroutedopticalnetworkingadoption](https://www.cisco.com/c/en/us/products/collateral/routed-optical-networking/routed-optical-networking-wp.html#Arepeopleskillsarealproblemforroutedopticalnetworkingadoption)

Uses less wavelengths per fiber span

Uses more traffic aggregation

Results in higher IP port/wavelength utilization

Results in higher IP port/wavelength bit rates (due to shorter distances)

Leads to:

Aggregates traffic onto fewer IP ports/wavelengths on a given node

Drives higher utilization of network assets

Increases network efficiency which leads to a lower cost per bit

Traffic Engineering (TE) is not required

However, use of TE and a Centralized (SDN) Controller can drive utilization even higher

IP and optical integration is a fundamental part of routed optical networking given the significant economic benefits provided by the current 400-Gbps digital coherent optics pluggable transceiver technology. However, routed optical networking goes far beyond that, and when deployed with modern IP/MPLS technology, it brings network optimizations and efficiencies to another level.

More efficient use of scarce DWDM wavelengths through statistical multiplexing and traffic aggregation leveraging direct router-to-router designs that better align with fiber topology, plus optional centralized traffic engineering.

Statistical Multiplexing is a technique that dynamically shares network resources among multiple data streams based on actual usage rather than reserving fixed capacity for each stream.

Traditional optical bypass networks dedicate a fixed DWDM wavelength to each router connection, often leaving unused capacity and requires more IP ports.

Statistical Multiplexing in Routed Optical Networks allows multiple routers to share wavelengths dynamically, filling them efficiently with traffic and reducing the total number of wavelengths needed.

Full services convergence: IP/MPLS is the de facto multiservice technology and the only networking technology capable of supporting L1 (emulated TDM circuits), L2 (point-to-point and multipoint Ethernet), and L3 services (internet, IP VPNs). The addition of PLE technology enabled by routed optical networking expands the IP/MPLS capabilities further to support bit-transparent services for high-speed circuits, including OTN, Ethernet, and storage protocols as clients.

Optical transport simplification (fewer OTN transponders and intermediate protection layers).

End-to-end traffic engineering and resiliency at the IP/MPLS layer, replacing rigid optical protection schemes.

Faster failover and restoration compared to slow optical restoration mechanisms.

Fast Convergence: IP/MPLS Fast Reroute (FRR) restores traffic in sub-50ms, similar to 1+1 optical protection but with far greater flexibility.

Better Bandwidth Utilization: Unlike optical protection, IP/MPLS doesn’t require full duplication of fiber paths.

Traffic Engineering: MPLS-TE enables dynamic path computation, optimizing traffic flow and minimizing congestion.

End-to-End Protection: IP/MPLS protects not just the transport layer but also application and service layers.

Flexibility: Optical protection is hardware-dependent, whereas IP/MPLS is software-defined and adaptable to changing network conditions.

Typical transport networks switch traffic in the event of fiber, node, or link failures under 50 milliseconds, which is the golden standard for any network technology. That switching time is easily met by multiple solutions available for IP/MPLS networks today. For over a decade, classic IP/MPLS networks have supported RSVP-TE-based fast-reroute for link and node failures. More recently, IP/MPLS-based networks started providing sub-50-ms switching even without RSVP-TE, using loop-free alternate (LFA) fast-reroute and topology-independent loop-free alternate (TI-LFA). As the name implies, TI-LFA applies to any network topology, it’s very simple to configure, and it’s enabled by segment routing, with no need for additional protocols or stateful core tunnels that are operationally demanding to configure and maintain and consume costly router resources.

Finally, it goes without saying that no technology available today except IP/MPLS can provide an optimal transport solution to any L1, L2, or L3 service in terms of scale, efficiency, and flexibility.

RON architecture provides opportunity to converge all services on the RON infrastructure. Instead of having OTN as a separate service, via RON you can provide packet-based services as well as TDM type of services. The following figure presents evolution from traditional multilayer to full convergence approach:

Multi-layered: Traditional services via OTN, DWDM transponder and ROADM

Transponder Integration: Hybrid approach where you are integrating transponder inside the router while keeping OTN and wave services.

Service Convergence Private Line Emulation (PLE): Further simplification by emulating OTN over RON infrastructure via private line emulation, while keeping wave services for strategic customers,

Full Convergence with Circuit Style Segment Routing (CS-SR): Delivering all services via RON infrastructure. CS-SR provides the underlying TDM-like transport to support traditional private line Ethernet services without additional hardware and bit-transparent Ethernet, OTN, SONET/SDH, and Fiber Channel services using Private Line Emulation hardware.

![](<../.gitbook/assets/Unknown image (826)>)

RON allows two types of services:

Connection-less

Classic SR on transport underlay

Ethernet VPN (EVPN) Virtual Private Wire Service (VPWS) on service overlay

Connection-oriented (for example traditional OTN services)

CS-SR on transport underlay, ensuring bandwidth and end-to-end path protection

PLE on service overlay, ensuring bit transparency and various payloads

Note

Segment Routing (SR) is not mandatory for Routed Optical Networking. Classic IP/MPLS is supported as well.

However, Routed Optical Networking will provide the best benefits when deployed over Segment Routing

Both SR MPLS and SRv6 are supported

Each option has its own set of benefits

### Key technologies in the RON IP/MPLS layer

MP-BGP and BGP-LS

DiffServ QoS

YANG model-driven programmability & telemetry

Segment Routing (SR) and SR-TE (Traffic Engineering)

Centralized (SDN) Controller with PCE (Path Computation Engine)

PLE (Private Line Emulation)

### Private Line Emulation (PLE)

**Private Line Emulation (PLE)** is a pillar of the Routed Optical Networking solution. It enables service providers and enterprises to further collapse network layers, decreasing network complexity and increasing network efficiency. PLE enables private line services to be carried over the same MPLS or Segment Routing network for non-Ethernet type services such as SONET/SDH, OTN, and Fiber Channel. PLE also supports bit-transparent Ethernet services where required.

High revenue legacy private line services exist in the network infrastructure of most service providers, often carried over a dedicated inefficient TDM OTN layer. PLE enables service providers to carry SONET/SDH, OTN, Ethernet, and Fiber Channel over a circuit-style segment routed packet network while maintaining existing service SLAs. PLE utilizes Circuit Emulation (CEM) to transparently transfer PLE client frames over MPLS or SR networks without changing the characteristics of the original signal.

#### How PLE works

Ethernet, OTN, Fiber Channel, or SONET/SDH PLE client traffic is carried on an EVPN-VPWS single homed service that is created between PLE endpoints. EVPN-VPWS signalling information is carried using BGP between the PLE circuit endpoints either through direct BGP sessions or through a BGP services route-reflector. The EVPN-VPWS pseudowire channel is set up between the endpoints when the CEM (Circuit Emulation) client interfaces are configured on each endpoint router and end-to-end transport connectivity using MPLS or Segment Routing transport is enabled.

CEM is a method through which client data can be transmitted over MPLS or Segment Routing networks in a bit-transparent manner, retaining the client L1 frame between sender and receiver. CEM over a Packet Switched Network (PSN) places the client bit streams into packet payload with appropriate pseudowire emulation headers.

The PLE initiator encapsulates the PLE client traffic and carries it over the EVPN-VPWS service running on MPLS or Segment Routing transport. The PLE terminator node extracts the bit streams from the EVPN-VPWS packets and places them onto the PLE client interface as defined by the client attribute and CEM profile.

| **PLE Transport Type** | **Supported Payloads**             |
| ---------------------- | ---------------------------------- |
| Ethernet               | 1GE and 10GE                       |
| OTN                    | OTU2 and OTU2e                     |
| SONET                  | OC-48 and OC-192                   |
| SDH                    | STM-16 and STM64                   |
| Fiber Channel          | FC1, FC2, FC4, FC8, FC16, and FC32 |

More on [https://www.cisco.com/c/en/us/td/docs/optical/ron/3-0/solution/guide/b-ron-solution-30/m-ple-configuration.html](https://www.cisco.com/c/en/us/td/docs/optical/ron/3-0/solution/guide/b-ron-solution-30/m-ple-configuration.html)

![](<../.gitbook/assets/Unknown image (827)>)

### Digital coherent optics (DCO)

**Digital coherent optics (DCO)** is a pluggable transceiver that combines silicon photonics and Digital Signal Processors, operating at 400 Gbps speeds and using the Quad Small Form Factor Pluggable Double Density (QSFP-DD) form factor.

The following figure illustrates the benefits of replacing a DWDM transponder with DCO, showcasing reductions in costs, rack space, power consumption, and operational complexity.

![](<../.gitbook/assets/Unknown image (828)>)

![](<../.gitbook/assets/Unknown image (829)>)

DCO relies on several standards to ensure interoperability, efficiency, and reliability in optical communication systems. These standards play a crucial role in shaping the functionality and performance of DCO technology. Three industry optical standards have emerged to cover a variety of use cases:

OpenZR+

Open ROADM

Optical Internetworking Forum (OIF)

![](<../.gitbook/assets/Unknown image (830)>)

![](<../.gitbook/assets/Unknown image (831)>)

![](<../.gitbook/assets/Unknown image (832)>)

Also the OLS is integrated in the pluggable

The Fanout A/D cable in this diagram serves as an optical multiplexing/demultiplexing medium, connecting multiple individual QDD BZR+ modules (each handling a specific wavelength like Lambda 1 or Lambda 2) to a single aggregated optical port on the QDD OLS (Optical Line System). This means the cable branches out on one end to connect to the separate QDD BZR+ modules, and converges into a single connection on the other end that plugs into the QDD OLS, effectively allowing the OLS to "add" (transmit) or "drop" (receive) these multiple wavelengths over a single optical link

![](<../.gitbook/assets/Unknown image (833)>)

## Passive Optical Network (PON)

**Passive Optical Network (PON)** is an cost-effective fiber-optic technology for delivering broadband network access to end-customers or as an alternative to Ethernet switching in enterprise networking

PONs are widely used in telecommunications as a means to implement fiber to the x (FTTX) solutions, including fiber to the home (FTTH), fiber to the building (FTTB), and others

Its architecture implements a point-to-multipoint topology in which a single mode optical fiber serves multiple endpoints by using unpowered (passive) fiber optic splitters to divide the fiber bandwidth among multiple subscribers, making it very scalable and cost-effective. It is deployed in a last mile between an ISP and its customers.

The primary advantages of PONs include their cost-effectiveness, scalability, and lower energy consumption compared to active networks. The lack of active components in the field reduces maintenance costs and increases reliability

### Active Optical Network (AON)

An Active Optical Network (AON) uses powered devices like switches to manage the flow of data between the network and its users. Unlike a Passive Optical Network (PON), which relies on non-powered splitters in a point-to-multipoint topology, AON operates on a point-to-point topology where each user gets a dedicated connection. This allows AON to optimize bandwidth by directing data specifically to the intended recipient, reducing congestion and increasing efficiency. The active devices in AON provide better control and network intelligence, enabling dynamic bandwidth allocation and easier troubleshooting. However, these devices require electricity and cooling, often needing placement in specialized telecom closets, which can complicate deployment and increase costs compared to PON’s simpler infrastructure

![](<../.gitbook/assets/Unknown image (800)>)

![Possive Optical Network](<../.gitbook/assets/Unknown image (801)>)

### Optical Distribution Network (ODN)

**Optical Distribution Network (ODN)** is the last mile network composed of passive optical components, such as optical fibers, and passive optical splitters and amplifiers. It provides optical channels between the OLT and ONTs.

Architectures like PON falls into ODN

![](<../.gitbook/assets/Unknown image (802)>)

ODN is the important part of the PON network which provides optical transmission between

OLTs and ONU/ONTs

Ideal distance is 20 km or sometimes more

Feeder optical fibers, optical distribution point, distribution fibers, optical access point, and drop fibers

form integral parts of ODN

Feeder fibre usually connects the central office to the distribution points

The distribution fibre extends the distribution points to access points

The drop fibers connect the individual ONTs from the access points

ODN is the very path essential to PON data transmission and its quality directly affects the performance,

reliability, and scalability of the PON system

### PON types

Asynchronous Transfer Mode passive optical network (APON) contains an electrical layer built on ATM

Broadband PON (BPON) is the improved successor of APON and maintains the ATM structure. BPON has a transmission rate of up to 622 Mb/s, with downstream capabilities of 155 Mb/s to 622 Mb/s. BPON can be converted to EPON or GPON over time, as ATM bandwidth is not ideal for video.

Ethernet PON (EPON) uses ethernet packets rather than the ATM cells used in APON and BPON. EPON is popular for its 1 Gb/s bandwidth for modern networks.

Gigabit PON (GPON) provides a very high bandwidth of 2.5 Gb/s. GPON uses IP and ATM or GPON encapsulation method (GEM) for encoding.

XG-PON (10G-PON or XG-PON1) Offers enhanced data rates up to 10 Gbps downstream and 2.5 Gbps upstream, providing higher bandwidth for demanding applications such as ultra-high-definition video streaming and cloud services.

XGS-PON (10G-EPON or XG-PON2) Extends the capabilities of XG-PON by supporting symmetrical 10 Gbps data rates both downstream and upstream, enabling symmetric gigabit services for businesses and residential users. XGS PON stands for 10 Gigabit Symmetrical PON

### PON components

![](<../.gitbook/assets/Unknown image (803)>)

#### Optical Line Terminal (OLT) (PE)

**Optical Line Terminal (OLT)** is a device that is placed at the head end of the ISP network

A single fiber optic cable runs from the OLT to a nonpowered (passive) optical beam splitter, which multiplies the signal and relays it to many optical network terminals (ONTs). End-user devices such as PCs and telephones are connected to the ONTs.

Since the splitting function is a one-to-many broadcast of the same data stream, the ONTs are responsible for filtering packets meant for the various connected endpoint devices. Encryption ensures that each ONT reads only the contents addressed to the endpoints connected to it.

The OLT can be either switch like it is employed in Cisco Catalyst PON or as an service provider OLT chassis:

![](<../.gitbook/assets/Unknown image (804)>)

**OMCI (ONT Management and Control Interface)**

OMCI (ONT Management and Control Interface) is a standardized protocol used in Passive Optical Networks (PON) to facilitate communication between the Optical Line Terminal (OLT) and the Optical Network Unit (ONU)/Optical Network Terminal (ONT). It is defined in the ITU-T G.988 recommendation.

OMCI is responsible for provisioning, managing, and monitoring the ONU.

ONU Configuration & Service Provisioning – Setting up VLANs, Quality of Service (QoS), GEM ports, and multicast services.

Performance Monitoring – Collecting statistics on traffic, signal levels, and error rates.

Fault Management – Detecting and reporting failures like signal loss, hardware malfunctions, or link degradation.

Firmware Upgrades & ONU Software Management – Remotely updating ONU firmware to ensure compatibility and security.

Security Management – Handling authentication and encryption mechanisms for secure communication.

OMCI messages are encapsulated within G-PON Encapsulation Method (GEM) frames and transmitted over the PON link.

The OLT sends OMCI commands to configure or request status from the ONU.

The ONU responds with acknowledgments or requested information.

Enables centralized management of multiple ONUs from the OLT.

Supports multi-vendor interoperability since it is standardized under ITU-T G.988.

Reduces the need for on-site configuration by allowing remote provisioning and troubleshooting.

#### Passive Optical Splitter (POS)

Optical splitters take a single light source (a single fiber optic strand) and refract and duplicate it multiple times to "outbound" fibers. In its simplest form, an optical beam splitter splits a light source in two by using two back-to-back prisms.

Typical gigabit-capable passive optical network (GPON) deployments have used a splitting ratio of 1:32 or 1:64. Current GPON standards specify up to 128 splits on a single GPON port. Those same standards set the distance between active devices at 20 kilometers

POS is a passive networking component, meaning it require no power, no climate control, and no maintenance whatsoever

Because PON uses the same strand of fiber to send and receive data, the passive optical splitter also acts as an optical combiner receiving data traffic from the same connected end devices. To achieve this, PON takes advantage of two distinct types of long-established telephony multiplexing concepts: wavelength division and time division.

**Wavelength-division multiplexing (WDM)** allows bidirectional traffic across a single fiber by using a different wavelength for each direction of traffic: the 1490-nanometer (nm) wavelength for downstream traffic and the 1310-nm wavelength for upstream traffic. The 1550-nm wavelength is reserved for optional overlay services, typically RF (analog) video.

Future iterations of the PON standard will define separate wavelengths for backward compatibility.

**Time-division multiplexing (TDM)** allows multiple end devices to transmit and receive independent signals across a single fiber by reserving time slots in a stream of data. PON uses two such technologies: TDM for downstream traffic and **time-division multiple access (TDMA)** for upstream traffic.

Downstream (OLT → ONUs):

Uses TDM: The OLT broadcasts data to all ONUs, but each ONU reads only its own time-slotted data.

All ONUs share the same wavelength (1490 nm or 1577 nm for XGS-PON) and time is divided into slots for signal multiplexing.

Upstream (ONUs → OLT):

Uses TDMA: Each ONU is assigned specific time slots to transmit upstream to avoid collisions.

The OLT coordinates these time slots using a Dynamic Bandwidth Allocation (DBA) algorithm.

TDM is about combining signals on a shared medium.

TDMA is about allowing multiple users to access the medium without interference.

As a passive device, the splitter acts as distribution point, with the single feed of downstream data broadcast to all connected ONT endpoints. The ONT accepts packets assigned to its TDM channel (frame time slot). It filters and discards packets meant for other ONTs.

TDMA enables multiple transmitters to be connected to one receiver. For PON, TDMA is used to recombine the multiple upstream feeds at the coupler. A splitter and a coupler are often found in one device.

Single-mode fiber is used

![Fiber Optic PLC Splitter SC/PC - 8x SC/PC](<../.gitbook/assets/Unknown image (805)>)

#### Optical Network Terminals (ONTs) (CE)

ONT is ITU-T terminology

ONU is IEEE terminology

Both refer to the user equipment in the PON system

They are generally on customer premises

is a simple switch or AP placed and powered at the customer premise that converts the optical signals from the fiber into the electrical signals, to be processed by the electiricity-powered circuit and send over metallic cable or wirelessly, if the ONT has Wireless capabilities

As a passive device, the splitter acts as distribution point, with the single feed of downstream data broadcast to all connected ONT endpoints

The ONT accepts packets assigned to its TDM channel (frame time slot). It filters and discards packets meant for other ONTs.

![Cisco Catalyst PON Series - Cisco](<../.gitbook/assets/Unknown image (806)>)

### Cisco Routed PON (RPON)

Limitations of Traditional PON

The traditional setup requires separate multiple OLTs, ONTs, and BNG/vBNGs, increasing hardware complexity and management overhead for service providers.

Adding new users might involve deploying additional hardware at both the OLT location and potentially the BNG location, making it less scalable

RPON addresses these limitations by collapsing the functionality of the OLT into a pluggable SFP+ form factor

This will make PON network a direct part of the access layer.

![](<../.gitbook/assets/Unknown image (807)>)

![](<../.gitbook/assets/Unknown image (808)>)

Notes

If specific features are required the customer must let us know, so that we can coordinate with the vendor to create proper service profiles and downstream QoS map that can be understood by the vendor's ONU

The failover from the singleVM to the distributed VM will be possibly supported in the next release

upgrade the routed pon to 25.2.1

cfg file pointing to the tftp server doesn't work even after specifying vrf in the tftp client configuration - Alex will follow up with the engineering team

kdyz pridam tag do policka pro "s" - servisni vlana, tak mi to nefunguje - zminit ze je to neco co se da s cisco engineering tymem doplnit podle potreb implementace

Routed PON reseni je nejoptimalnejsi pro SP, kteri maji optickou infrastrukturu zakonecnou centralizovane, proto aby se daly NCS hostujici OLT do stejnych prostoru a nahradily stavajici OLTcka

Na meetingu s Vantivou a KAONEM Zeptat se jestli zapinaji wifi a nastavujou vic veci na ONTcku z OLTcka, nebo to nechavaji na koncovem zakaznikovy ktery ma moznost se lognout do GUI

Cisco RPON deep dive:

<\<PON\_technical\_deep\_dive0524.pptx>>

<\<BRKSP-1169.pdf>>

XGS-PON: A shared-fiber system for broadband access, using multiple layers (Physical, TC, Path) and wavelength management to serve many users over one fiber with TDMA/TDM.

Ethernet: A point-to-point or switched system with a simpler Layer 2 frame structure, designed for LANs/WANs without shared medium scheduling or wavelength considerations.

![](<../.gitbook/assets/Unknown image (809)>)

![](<../.gitbook/assets/Unknown image (810)>)

### MTU in PON

the failure above 9176 bytes is caused by L2/PON encapsulation overhead (VLAN tag, GEM/PON headers, etc.) that reduces the effective path MTU for the IP packet. Below I show the math and how it matches your observed thresholds, then what to check.

Facts / definitions

An ICMP echo on IOS “size” for extended ping is the size of the entire IP datagram (i.e. it includes the IP header + ICMP header + ICMP data).

IP packet on the wire = IP header (20) + ICMP header (8) + ICMP payload (if IPv4, no options). The Ethernet payload carries that IP packet.

802.1Q VLAN tag = 4 bytes (affects L2 frame size). GPON/EPON (GEM) adds its own variable encapsulation overhead when mapping Ethernet into PON.

MTU configured = 9198.

Successful pings up to size 9176.

9177 (and 9180) fail.

Working the math (using Cisco ping semantics)

Using the Cisco semantics (ping size = total IP datagram length):

If size = 9176, then IP datagram length = 9,176 bytes.

That fits your path (ping succeeds).

If size = 9177, IP datagram = 9,177 bytes → fails on the path.

So the effective path MTU for IP datagrams appears to be 9,176 bytes (anything larger is dropped/fragmented and DF causes failure). That is 22 bytes less than your configured MTU of 9,198:

Edit

9198 (configured MTU)

* 9176 (max IP datagram that succeeds)

\= 22 bytes overhead

Where do those 22 bytes come from?

They come from L2 / PON encapsulation that is applied in addition to the IP datagram. Potential contributors:

802.1Q VLAN tag: 4 bytes.

Wikipedia

GPON/EPON GEM or PON encapsulation overhead: typically a few bytes per GEM header and possibly additional bytes for mapping/fragmentation — vendor/implementation dependent (GPON/EPON add small fixed headers and possible alignment padding). Practical GPON documents / vendor notes show per-frame encapsulation bytes (varying between \~4–8+ bytes depending on mode).

Any additional provider tags (Q-in-Q, service tags) or PON-specific TLV/preamble handling could add the remaining bytes.

Example decomposition that matches \~22 bytes (illustrative):

802.1Q tag = 4

GPON/GEM minimal encapsulation/padding = \~5–8

Other tags / alignment / management = \~10–13

Sum ≈ 22 (matching observed difference).

Because the encapsulation is inserted on the wire outside the IP datagram, an IP datagram of 9,198 bytes could become >9,198 on the wire once encapsulation is added — so the device downstream rejects or fragments it, explaining your observed failure at 9177/9180.

### Broadband Remote Access Server (BRAS) / Broadband Network Gateway (BNG)

**Broadband Remote Access Server (BRAS) / Broadband Network Gateway (BNG)** is a broadband service provider component at the edge of the SP network that aggregates and authenticates subscriber sessions and routes traffic between the access network and the ISP's backbone (core) network.

BRAS is deployed by the service provider and is present at the first aggregation point in the network, such as the edge router.

It established and manages subscriber sessions coming in from access networks such as DSL, fiber, or wireless. It be be also virtualized - Virtual BNG (vBNG)

<\<BNG\_PPOE.pdf>>

Main Functions of BRAS:

User Authentication & Authorization:

Communicates with RADIUS or AAA servers.

Verifies user credentials (usually via PPPoE, PPPoA, or DHCP).

IP Address Management:

Assigns IP addresses to clients dynamically (via DHCP or IPCP).

Interacting with the DHCP server to provide IP address to clients.

Policy Enforcement:

Applies Quality of Service (QoS), bandwidth profiles, and filtering rules per user.

Traffic Aggregation:

Collects and aggregates traffic from multiple DSLAMs (DSL Access Multiplexers) or access nodes.

Routing & Forwarding:

Directs user traffic into the ISP’s IP backbone.

Acts as a Layer 3 gateway for subscriber traffic.

Accounting:

Collects usage data for billing, auditing, or network planning.

![](<../.gitbook/assets/Unknown image (811)>)

#### Establishing subscriber sessions

• Each subscriber (or more specifically, an application running on the CPE) connects to the network by a logical session. Based on the protocol used, subscriber sessions are classified into two types:

PPPoE subscriber session: The PPP over

Ethernet (PPPoE) subscriber session is established using the point-to-point(PPP) protocol that runs between the CPE and BNG.

IPoE subscriber session: The IP over Ethernet (IPoE) subscriber session is established using IP protocol that runs between the CPE and BNG; IP addressing is done using the DHCP protocol.

PPPoE was designed for managing how data is

transmitted over Ethernet networks, and it allows a single

server connection to be divided between multiple clients,

using Ethernet. As a result, multiple clients in shared

network can connect to the same server from the Internet

Service Provider and get access to the internet, at the

same time, in parallel. To simplify, PPPoE is a modern

version of the old dial-up connections, which were popular

in the 80s and the 90s.

• P2P protocol over ethernet encapsulating PPP frames in

Ethernet frames (Src MAC, Dst MAC).

• Old days used mainly with ADSL services ( most common

PPPOE over ATM)

• Offers standard PPP features such as authentication,

encryption, and compression

• PPPoE has two distinct stages as defined in RFC 2516:

* Discovery stage
* PPP session stage

• IPoE is essentially DHCP-triggered subscriber interfaces.

• Users are "authenticated" through the use of DHCPv4/v6

Option-82 inserting their Circuit-ID into their initial DHCP

Discovery - this identifies the physical location of the user based

on the tail that they are connected to (this would be done at an

aggregation switch between the xPON network and whatever

backhaul gets them to their ISP of choice).

• The ISP will then service the DHCP request (if the Circuit-ID can

be mapped to a valid user via RADIUS), provide an IP (and

hopefully prefix-delegation if they're offering IPv6) and then

create a logical interface representing that subscriber that you

they apply their filtering/rate-shaping to and start grabbing stats

from.

• Session lifecycle based on DHCP Lease Tracking and Split Lease

• Authentication methods

* DHCP Option82
* DHCP Option 60

-Vlan Encap

BNG relies on an external Remote Authentication Dial-In User Service (RADIUS)

server to provide subscriber Authentication, Authorization, and Accounting (AAA)

functions. During the AAA process, BNG uses RADIUS to:

•authenticate a subscriber before establishing a subscriber session

•authorize the subscriber to access specific network services or resources

•track usage of broadband services for accounting or billing

• The RADIUS server contains a complete database of all subscribers of a service

provider, and provides subscriber data updates to the BNG in the form of attributes

within RADIUS messages. BNG, on the other hand, provides session usage

(accounting) information to the RADIUS server.

• BNG supports connections with more than one RADIUS server to have fail over

redundancy in the AAA process. For example, if RADIUS server A is active, then BNG

directs all messages to the RADIUS server A. If the communication with RADIUS

server A is lost, BNG redirects all messages to RADIUS server B.

• During interactions between the BNG and RADIUS servers, BNG performs load

balancing in a round-robin manner. During the load balancing process, BNG sends

AAA processing requests to RADIUS server A only if it has the bandwidth to do the

processing. Else, the request is send to RADIUS server B.

#### Cloud Native Broadband Network Gateway

**Cloud Native Broadband Gateway (cnBNG)** represents a fundamental shift in how providers build converged access networks by separating the subscriber BNG control-plane functions from user-plane functions. **CUPS (Control/User-Plane Separation)** allows the use of scale-out x86 compute for subscriber control-plane functions, allowing providers to place these network functions at an optimal place in the network, and also allows simplification of user-plane elements. This simplification enables providers to distribute user-plane elements closer to end users, optimizing traffic efficiency to and from subscribers.

**cnBNG control plane**

The cloud native BNG control plane is a highly resilient scale out architecture. Traditional physical BNGs embedded in router software often scale poorly, require complex HA mechnaisms for resiliency, and are relatively painful to upgrade. Moving these network functions to a modern Kubernetes based cloud-native infrastructure reduces operator complexity providing native scale-out capacity growth, in-service software upgrades, and faster feature delivery.

**cnBNG user plane**

The cnBNG user plane in Agile Metro 1.1 is provided by Cisco ASR 9000 routers. The routers are responsible for terminating subscriber sessions (IPoE/PPPoE), communicating with the cnBNG control plane for user authentication and policy, applying subscriber policy elements such as QoS and security policies, and performs subscriber routing. Fixed and modular platforms are supported
