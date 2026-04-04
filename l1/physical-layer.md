---
description: L1 - PHY
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

# Physical Layer

### Signal

**Signal** is a physical quantity that transfers data between systems.\
It travels over a medium like copper, fiber, or air.

#### Electrical signals

**Electrical signals** travel through conductive mediums like copper.\
They represent bits by varying voltage or current (high = `1`, low = `0`).\
The network interface card (NIC) encodes data using an encoding scheme.

#### Light signals (optical)

**Optical signals** are pulses of light over fiber.\
They use LEDs or laser diodes to modulate the light.

Light is an electromagnetic wave.\
Its electric and magnetic fields oscillate perpendicular to each other.

#### Radio waves (wireless)

**Radio waves** propagate through air as electromagnetic waves.\
They enable wireless transmission without cables.

**Microwave signals**

Operate in the range `1 GHz` to `300 GHz`.\
Used for point-to-point links, satellite, radar, and microwave ovens.

**Infrared signals**

Operate between visible light and microwaves.\
Used for short-range comms like remote controls, IrDA ports, and sensors.

**Ultrasonic signals**

High-frequency sound above human hearing range.\
Used for distance measurement, ultrasound imaging, and underwater comms.

#### Signal metrics by communication domain

| **Domain**           | **Signal Metric**         | **Unit**  |
| -------------------- | ------------------------- | --------- |
| Optical (Fiber, PON) | Optical Power             | dBm       |
| Wireless (RF)        | Signal Amplitude, Voltage | Volts (V) |
| Baseband/Electronics | Voltage Levels, Amplitude | Volts (V) |

#### Electromagnetic spectrum

![Electromagnetic Wave (Light Wave) vs. Mechanical Wave | Webb](<../.gitbook/assets/Unknown image (361)>)

![](<../.gitbook/assets/Unknown image (362)>)

![](<../.gitbook/assets/Unknown image (363)>)

### Bit and binary encoding

#### Bit

**Bit** is the smallest unit of digital information and can have one of two values: 0 or 1. It represents the fundamental building block of all digital data and computing. 0 represents zero voltage or light off and 1 is represented with increased voltage or a light on. The most commonly used unit is **Byte**, which consists of 8 bits.

#### Binary data encoding

**Binary data encoding** is when digital communication data are encoded in a binary, where 0s and 1s are typically represented by two distinct levels, which can be electrical voltage levels or optical power levels, depending on the medium used for data transmission

#### Line encoding examples

**NRZ (Non-Return-to-Zero)**

Binary representation:

* `0`: low voltage (for example `0V`)
* `1`: high voltage (for example `+5V`)

Simple and efficient for representing binary data.

Lacks inherent synchronization, which can be a problem for long sequences of identical bits.

![](<../.gitbook/assets/Unknown image (364)>)

**NRZI (Non-Return-to-Zero Inverted)**

* `0`: no change in signal
* `1`: change in signal (transition from high to low or low to high)

**Manchester encoding**

* `0`: low-to-high transition in the middle of the bit period
* `1`: high-to-low transition in the middle of the bit period

Provides a transition in the middle of each bit period, aiding synchronization, which requires more bandwidth compared to NRZ.

**Differential Manchester encoding**

* `0`: transition at the start of the bit period
* `1`: no transition at the start, but a transition in the middle

Ensures at least one transition per bit period for reliable clock recovery.

**Bipolar encoding (AMI - Alternate Mark Inversion)**

* `0`: zero voltage
* `1`: alternating positive and negative voltages

Helps to maintain DC balance and can be useful for detecting errors.

**4B/5B encoding**

Encodes 4 bits of data into 5-bit code words.

Common in Ethernet standards.

**8B/10B encoding**

Encodes 8 bits of data into 10-bit symbols.

Balances the number of 1s and 0s to improve error detection and maintain DC balance.

Used in high-speed data communication like Gigabit Ethernet and Fibre Channel.

### Electromagnetic signal properties

#### Baseband signaling

**Baseband signaling** uses digital signaling only on one fixed frequency of the entire bandwidth of a wire.

This means that the signal is not modulated onto a carrier frequency. The statement "the entire bandwidth of the communication channel is used to send only one digital signal" means that the digital signal is transmitted using the full capacity of the channel, without sharing it with other signals or frequencies. This allows for the efficient transmission of the digital signal without the need for modulation and demodulation processes. Ethernet is one example of baseband technology

![](<../.gitbook/assets/Unknown image (365)>)

#### Broadband signaling

**Broadband signaling** transmits analog or digital signals using multiple frequency ranges. The available bandwidth of the medium (e.g., copper, fiber, or wireless) is divided into multiple frequency bands, and each band can carry a separate data stream. This is typically achieved using **Frequency-Division Multiplexing (FDM)**.

Examples of broadband technologies:

DSL (Digital Subscriber Line) over copper wire

Cable Internet

Fiber-optic connections

Wireless technologies such as Wi-Fi, LTE, and 5G

![](<../.gitbook/assets/Unknown image (366)>)

![](<../.gitbook/assets/Unknown image (367)>)

![](<../.gitbook/assets/Unknown image (368)>)

### Signal types

#### Analog signals

**Analog signal** is a continuous range of values representing stream of signal. It can be represented by infinite number of values at any given time (signal can jump from 2 volts to 10 in each interval).

Devices receiving analog signals must accurately measure and interpret the precise voltage or current levels present in the signal at any given moment.

However, factors such as noise, distortion, and signal attenuation can introduce inaccuracies or errors in the received analog signal.

Additionally, analog signal processing techniques often require complex circuitry and precise calibration to achieve accurate signal reproduction and interpretation

Video and audio transmissions are often transferred or recorded using analog signals

![](<../.gitbook/assets/Unknown image (369)>)

#### Digital signals

**Digital signal** is not a continuous (discrete) signal represented in binary, thus can represent value 0 or 1 at any given time

Digital signals are more resistant and less suspectible to interference or degradation, since devices can sense whether receving high voltage signal (1) or low voltage signal (0)

![](<../.gitbook/assets/Unknown image (370)>)

#### Analog to digital conversion (ADC)

**Analog-to-digital conversion (ADC)** is a fundamental process in modern electronics and communication systems, allowing analog signals to be processed, stored, and transmitted using digital

By converting analog signals into digital format, they can be processed, manipulated, and transmitted using digital technology. Digital signal processing techniques, such as filtering, modulation, and error correction, can be applied to digital signals to enhance their quality and reliabilitytechnology.

Analog waves are smooth and continuous, digital waves are stepping, square, and discrete. (it can jump from one value up and down, whereas analog can jump from a value to the infinite possibile values - for exampe from 4.56 to 6.83V)

![Digital Sine Wave](<../.gitbook/assets/Unknown image (371)>)

### Signal loss and attenuation

Several factors can influence the strength and quality of a signal as it propagates through a medium

The greater the distance the signal has to travel through the medium, the more it will be attenuated or weakened.

This attenuation occurs due to factors such as signal loss over distance, dispersion, and absorption by the medium.

For example, in wireless communication, signals can be attenuated by obstacles such as walls, buildings, or terrain, as well as by atmospheric conditions like rain, fog, or humidity

In wired communication systems, such as Ethernet or coaxial cables, signal strength can be attenuated by resistive losses, dielectric losses, and other factors inherent to the transmission medium itself.Also external sources of electromagnetic interference, such as other electronic devices, power lines, or radio transmissions, can distort or weaken the signal as it travels through the medium.

Signal processing techniques such as amplification, equalization, and error correction can be used to compensate for signal attenuation and improve signal quality during transmission

{% hint style="info" %}
The signal is regenerated and its attenuation is reduced as network equipment receives it and forwards it toward the destination.
{% endhint %}

Natural electrical noise might be from lightning discharges in thunderstorms. Electrical noise that originates from the Sun is called solar noise. Distant stars generate electrical noise called cosmic noise. While these stars are too far away to individually affect terrestrial communications systems, their large number leads to appreciable collective effects. Apart from that, there is a substantial amount of signal disruption from artificial electrical noise.

Signal attenuation refers to the loss of signal strength as it travels through a medium, such as an optical fiber, coaxial cable, or wireless transmission medium

This loss of signal intensity can occur due to various factors and can have a significant impact on the quality and reliability of communication systems

Distance Attenuation

This type of attenuation occurs as signal strength inherently decreasing as with increasing distance from the transmitter. The farther the signal travels, the weaker it becomes. The strength of the signal is measured in decibels (dB), which represent the signal's power relative to a reference level

The loss of signal strength is calculated as a logarithmic function of the distance.

Signal Absorption occurs when some of the signal energy is absorbed by the material of the fiber itself. This absorption can be due to impurities in the fiber material or other factors. As the signal travels through the fiber, its energy gradually decreases, leading to attenuation or loss of signal strength.

Reflection happens when signal encounters a boundary between two different media with different refractive indices, such as air and glass. Some of the light is reflected back into the fiber when it reaches such a boundary. This reflection can cause signal loss and impairments, particularly if not properly managed with anti-reflection coatings or techniques.

Refraction occurs when signal passes from one medium to another and changes direction due to a change in the speed of light. In optical fibers, refraction happens at the core-cladding interface, where the refractive index changes. Improperly designed or manufactured fibers can lead to excessive refraction, causing signal distortion and loss.

Diffraction is the bending of signal waves around obstacles or through narrow openings. In optical fibers, diffraction can occur at bends, splices, or discontinuities in the fiber structure. This bending can scatter the light and cause signal loss or distortion, particularly in tightly bent or improperly installed fibers.

Scattering refers to the random redirection of signal waves by irregularities or impurities in the fiber material. There are various types of scattering, including Rayleigh scattering, which occurs due to small variations in the refractive index along the fiber length, and Mie scattering, which is caused by larger irregularities or impurities. Scattering contributes to signal degradation by causing signal loss and increasing background noise

![](<../.gitbook/assets/Unknown image (372)>)

### Data transmission

Serial transmission involves transmitting data bits one after the other over a single communication channel. It is suitable for long-distance communication.

Parallel transmission involves the simultaneous transmission of multiple bits over separate channels or wires.

It is typically faster and efficient than serial communication but is generally limited to shorter distances due to the increased complexity and the need for multiple wires.

![](<../.gitbook/assets/Unknown image (373)>)

Networking equipment, such as routers, switches, and network interface cards (NICs), often have integrated clock mechanisms in their hardware circuitry.

These clocks generate timing signals that help regulate the transmission and reception of data

When data transmission begins, the sender initiates the transmission by sending a start-of-frame (SOF) or similar synchronization signal. This signal serves as a marker to indicate the start of a data frame and to synchronize the receiver

After transmitting the data frame, the sender may send an end-of-frame (EOF) signal or a similar marker to indicate the end of the transmission

Synchronous transmission data signals are streamed continuously in frames, where both sender and receiver use synchronized timing signals.

Example : Transfer of large text files; Video/voice calls

![](<../.gitbook/assets/Unknown image (374)>)

Asynchronous transmission data is sent in a form of bytes without a continuous clock signal. One of the characteristics of asynchronous transmission is that the receiver is largely unaware of when data will arrive > Start Stop bits are used to mark the beginning and end of each data packet.

Commonly used for sporadic or variable-speed data transfer (mouse/keyboard input; Emails; Radios)

![](<../.gitbook/assets/Unknown image (375)>)

#### Circuit switching vs packet switching

**Ethernet (packet switching)**

Ethernet is the dominant modern standard based on Packet Switching. Data is broken into small, individually addressed frames or packets which are sent dynamically across the network. This method offers high efficiency because capacity is only utilized when data is actively being sent. Ethernet is highly scalable (from 100 Mb/s up to 100 Gb/s and beyond) and flexible, supporting all modern IP-based services (Internet, VoIP, video) on a single infrastructure. The main drawback is a variable latency (jitter), as packets must wait their turn to be processed, which can affect time-sensitive applications if not properly managed.

**E1/TDM (time division multiplexing)**

E1/TDM is a legacy standard based on Circuit Switching where communication is organized into fixed-size time slots. A fixed, continuous slot is permanently reserved for each channel (e.g., a phone call) for the entire duration of the connection, regardless of whether data is being transmitted. This guarantees low and stable latency, which was critical for traditional voice telephony. However, TDM is highly inefficient, as reserved capacity goes unused during silent periods. E1 has a fixed, non-scalable speed of 2.048 Mb/s, making it too slow and rigid for modern data demands. Telecom operators are actively replacing TDM infrastructure with Ethernet using techniques like TDMoIP (TDM over IP) to maintain service stability while leveraging the cost efficiency of packet networks.

#### Communication modes (simplex / half / full duplex)

The term duplex communication is used to describe a communications channel that can carry signals in both directions, as opposed to a simplex channel, which carries a signal in only one direction. There are two types of duplex settings that are used for communications on an Ethernet network—full duplex and half duplex.

Half-duplex communication relies on a unidirectional data flow, which means that data can go only in one direction at a time. Sending and receiving data are not performed at the same time. Usually asynchronous). This was the initial capability of stations, since there was no technology that allowed to differentiate between send and receive signal

Because data can flow in only one direction at a time, each device in a half-duplex system must constantly wait its turn to transmit data

If a device transmits while another is also transmitting, a collision occurs. Therefore, half-duplex communication implements Ethernet Carrier Sense Multiple Access with Collision Detection (CSMA/CD) to help reduce the potential for collisions and to detect them when they do occur. CSMA/CD allows a collision to be detected, which causes the offending devices to stop transmitting. Each device retransmits after a random amount of time has passed. Because the time at which each device retransmits is random, the possibility that they again collide during retransmission is very small.

Full-duplex the data flow is bidirectional, so that data can be sent and received at the same time. The bidirectional support enhances performance by reducing the wait time between transmissions (synchronous)

It is achieved with UTP cables, which have certain wires dedicated for transmission and certain for receiving. Full-duplex over coaxial cable is achieved by specialized modulating devices (modems). They employ Pulse-amplitude modulation (PAM) to differentiate between what signals are for send and what for receive on a single copper wire such as coaxial cable

In optics, different wavelengths are used to differentiate send and receive signal

The NIC has a separate channels for sending and receiving incoming signals

The bidirectional support enhances performance by reducing the wait time between transmissions. Ethernet, Fast Ethernet, and Gigabit Ethernet NICs sold today offer the full-duplex capability. In full-duplex mode, the collision-detection circuit is disabled. Frames that the two connected end nodes send cannot collide because the end nodes use two separate circuits in the network cable.

### Transmission terms and metrics

Bit Rate represents the number of bits transmitted per second (bps)

Bits represent the actual data, while Baud Rate is the unit for symbol rate or modulation rate in symbols per second or pulses per second. It is the number of distinct symbol changes (signalling events) made to the transmission medium per second in a digitally modulated signal or a bd rate line code.

It is measured in baud (BD). One symbol can be represented by multiple bits, so the Bit Rate can be higher than the Baud Rate

Clock rate (for serial links) specifies how many bits can be transmitted at a certain period

Interface Speed (Interface transmission rate, also access rate) speed command will change the actual operational bandwidth of the interface

It will only allow you to configure specific speeds, those that the interface is capable of operating at

Bandwidth (BW) indicates how many packets can theoretically interface send (measured in seconds Kbps/Mbps/Gbps). BW in Cisco is displayed in kbps

{% hint style="info" %}
In Cisco, interface `bandwidth` does not change physical link capacity.\
It is a label used by routing protocols for path selection.
{% endhint %}

Throughput refers to the actual amout of data that can be send over the available bandwidth of the link

Device Throughput is the amount of packets per second that a device can process at a time, the more services are running on device, the less throughput it has available for forwarding. If switch has a 1GB throughput, its split between all it's ports

Latency The amount of time required for a packet to travel between two points in a network, and is the sum of all delays between those two points

Delay A component of latency that measures the amount of time for a specific transmission or processing task

Fixed network delay: Two types of fixed delays are serialization and propagation delays. Serialization is the process of placing bits on the circuit. The higher the circuit speed, the less time it takes to place the bits on the circuit. Therefore, the higher the speed of the link, the less serialization delay is incurred. Propagation delay is the time that it takes for frames to transit the physical media.

Variable network delay: A processing delay is a type of variable delay. It is the time that is required by a networking device to look up the route, change the header, and complete other switching tasks. Sometimes, the packet must also be manipulated, for example, when the encapsulation type or the Time to Live (TTL) must be changed. Each of these steps can contribute to the processing delay. Another important contributor to variable delay is the queuing delay. The queuing delay is the time that a job waits in a queue until it can be executed.

Propagation delay is the time it takes for a packet to travel from the source to a destination at the speed of light over a medium such as fiber-optic cables or copper wires

Serialization delay is the time it takes to place all the bits of a packet onto a link. It is a fixed value that depends on the link speed; the higher the link speed, the lower the delay

{% code title="Serialization delay example" %}
```
s = (packet size in bits) / (line speed in bps)
s = (1500 bytes × 8) / 1 Gbps
s = 12,000 bits / 1,000,000,000 bps
s = 0.000012 s = 0.012 ms = 12 μs
```
{% endcode %}

Processing delay is fixed amount of time it takes for a networking device to take the packet from an input interface and place the packet onto the output queue of the output interface

Inter-packet Delay Variation/FDV (Frame Delay Variation (Jitter) is the difference in the latency between packets in a single flow. For ex, if one packet takes 50 ms to traverse the network from the source to destination, and the following packet takes 70 ms, the jitter is 20 ms. This happesns due to the queueing delay experienced by packets during periods of network congestion

Queuing delay the time a packet spends in router queues; depends on queue length and type.

Round-trip time (RTT) specifically refers to the total time it takes for a packet to travel from the source to the destination and back to the source

It is measured in seconds or milliseconds (ms)

Packet Loss result of CRC errors or a congestion on an interface, resulting in new incoming packets are being dropped

Burst loss multiple consecutive packets are lost due to sudden route change in a transit device that creates temporary black hole, or by a noise on the transmission media that kills all the packets

Packet sequencing

Packet reordering

![](<../.gitbook/assets/Unknown image (376)>)

#### Monitoring interface utilization (IOS XR)

| monitor interface | IOS XR command to observe current traffic/utilization on a port |
| ----------------- | --------------------------------------------------------------- |

### Multiplexing

**Multiplexing** is a method by which multiple analog or digital signals are combined into one signal over a shared medium/cable

The multiplexing divides the capacity of the communication channel into several logical channels, one for each message signal or data stream to be transferred

A reverse process, known as demultiplexing, extracts the original channels on the receiver end.

A device that performs the multiplexing is called a multiplexer (MUX), and a device that performs the reverse process is called a demultiplexer (DEMUX or DMX).

#### Frequency-division multiplexing (FDM)

**Frequency-division multiplexing (FDM)** is a technique of multiplexing which means combining more than one signal over a shared medium. In FDM, signals of different frequencies are combined for concurrent transmission.

In FDM, the total bandwidth is divided to a set of frequency bands that do not overlap. Each of these bands is a carrier of a different signal that is generated and modulated by one of the sending devices. The frequency bands are separated from one another by strips of unused frequencies called the guard bands, to prevent overlapping of signals.

![MULTIPLEXING](<../.gitbook/assets/Unknown image (377)>)

#### Time division multiplexing (TDM)

**Time division multiplexing (TDM)** is a communications process that transmits two or more streaming digital signals over a common channel. In TDM, incoming signals are divided into equal fixed-length time slots. After multiplexing, these signals are transmitted over a shared medium and reassembled into their original format after de-multiplexing

In TDM, the channel is divided into several time slots, and each signal is transmitted during its allocated time slot.

![Time Division Multiplexing](<../.gitbook/assets/Unknown image (378)>)

WDM,CDWM,DWDM is related to the multiplexing in fiber optics, where multiple wavelengths are transmitted over a single fiber cable. Explained in fiber optics

### Modulation

Modern systems are designed to simultaneously interpret multiple modulation types, enabling them to receive more data efficiently. Imagine signaling someone with a flashlight: the recipient could interpret the intensity, the duration of the light, or even its color, with each element conveying a distinct piece of information.

A modulation scheme continuously alters the property or properties of a waveform. In this case, it is light, in order to encode the binary information into the waveform. Modern optical networks use a variety of modulation schemes in order to transport the data across hundreds to thousands of kilometers.

DWDM networks utilize several properties of light in order to efficiently encode the information.

Electrical transmission of data has significant distance limitations compared to optical transmission. Legacy optical encoding schemes using on/off signaling such as Non-Return to Zero (NRZ) suffer from the effects of chromatic dispersion (CD), limiting the effective distance without the use of dispersion compensation units (DCU). In order to effectively transfer data across many kilometers at rates in excess of 10 Gbps, transceivers must use coherent modulation schemes.

Changing the phase and/or amplitude of a wave encodes information as a symbol, a single unit of transmission containing one or more bits.

All of the listed schemes can use polarization multiplexing in order to increase the data rate.

![](<../.gitbook/assets/Unknown image (379)>)

#### PAM (Pulse Amplitude Modulation)

Uses multiple voltage levels to represent more bits per symbol (e.g., PAM-4 uses four levels to encode 2 bits per symbol).

Efficient for high-speed data transmission.

Requires more complex signal processing.

![](<../.gitbook/assets/Unknown image (380)>)

#### Phase Shift Keying (PSK)

PSK modulation shifts the phase of the signal in order to encode a bit or bits. As the phase of the signal can change as it traverses the fiber, the receiver measures the difference in phase between successive symbols to more accurately determine their value

**Binary Phase Shift Keying (BPSK)**

Changes the phase of the signal by π radians or 180 degrees in order to encode either a 0 or a 1. The notable difference between the phases results in low Optical Signal to Noise Ratio (OSNR) requirements and signals using this modulation can travel potentially thousands of kilometers. The low number of bits per symbol limits the data rate of BPSK signals to around 100 Gbps.

![](<../.gitbook/assets/Unknown image (381)>)

**Quadrature Phase Shift Keying (QPSK)**

QPSK changes the phase between successive symbols by π/2 radians or 90 degrees. The smaller change in phase increases information density to two bits per symbol as QPSK has four possible states

0° → 00

90° → 01

180° → 10

270° → 11

![](<../.gitbook/assets/Unknown image (382)>)

Dual Polarization Quadrature Phase-Shift Keying (DP-QPSK) is an enhaced version of QPSK. It uses four different phases as well as two polarizations (vertical and horizontal), so when a DP-QPSK symbol is sent, information for four bits is transmitted. DP-QPSK is commonly used in long-distance 100G coherent optics lines and typically requires a coherent optical receiver.

#### Quadrature Amplitude Modulation (QAM)

In order to further increase the number of bits per symbol, the transmitter can change the amplitude of the signal in addition to the phase. The number of points in the constellation (symbols) defines the type of QAM.

8-QAM

Eight possible states give three bits per symbol for this modulation scheme.

![](<../.gitbook/assets/Unknown image (383)>)

16-QAM

At baud rates around 30 Gbaud, 16-QAM has a data rate of 200 Gbps. Increasing to 60 Gbaud gives rates up to 400 Gbps. Smaller changes in phase and amplitude increase the OSNR requirements and limit its range to a few hundred kilometers.

![](<../.gitbook/assets/Unknown image (384)>)

![](<../.gitbook/assets/Unknown image (385)>)

32-QAM and 64-QAM

These two high-order modulation schemes use five and six bits per symbol, respectively, enabling transmission rates up to 600 Gbps. The high OSNR requirements of 64-QAM limit the effective range to less than 200 km.

![](<../.gitbook/assets/Unknown image (386)>)

![](<../.gitbook/assets/Unknown image (387)>)

The QAM technology allows you to alter both the amplitude and phase of the carrier signal. In 16-QAM, each symbol represents four bits, whereas in 64-QAM, each symbol represents six bits. 16-QAM modulation technique is commonly used in 400G Coherent optics lines, while 64-QAM modulation in 800G Coherent optics lines.

Note In optical and radio communication contexts, "constellation" refers the way information is encoded into the phases and amplitudes of the carrier signal.

### Signal error detection and correction

#### Parity check

**Parity** is a simple method for checking whether a received data block has a bit error. Parity check involves appending a binary bit known as a parity bit to the data block. The value of the parity bit (1 or 0) depends upon whether the number of 1s in the data frame is even or odd. If a single bit flips during transmission, it will change the value of the parity bit and the system will detect an error.

Parity check is only reliable for detecting single-bit errors. It should therefore be reserved for deployments in which errors are expected to be rare and only occur as single bits. If conditions are likely to cause burst errors, then another error detection technique should be used.

#### Checksum

**Checksum** is a simple method of redundancy checking used to detect errors in data transmission. In this method, a checksum algorithm operates on the data before transmission to generate a checksum value. This checksum gets appended to the data and sent along with the data frames. The receiver computes a new checksum based on the values of the received data blocks and then compares it with the checksum that was sent by the transmitter. If the two checksums match, then the frame is accepted; otherwise, it is discarded. Checksum is useful for detecting both single-bit and burst errors.

#### Cyclic Redundancy Check (CRC)

**CRC** takes advantage of the fact that a binary data block can be expressed as a polynomial. As with integer division, a dividend polynomial can be divided by a divisor polynomial, leaving a quotient with a remainder that can range from zero to some value.

The CRC process starts with generating a polynomial divisor and converting the data block into a polynomial dividend on the transmitter side. Next, the data polynomial is divided by the polynomial divisor. The remainder is subtracted from the quotient, and this value is appended to the data block. At the receiver end, the received bitstream is once again converted to a polynomial dividend. When it is divided by the quotient and the remainder that is sent in the codeword is subtracted, the result should be zero. If it is not, an error has occurred. CRC is effective for both single-bit and burst errors.

Reed-Solomon (RS) codes are widely used ECCs. The algorithms operate on symbols rather than on individual bits

FC-FEC (Fire Code FEC) also known as BASE-R FEC: Standardized by IEEE 802.3. Commonly used in 10G Ethernet links. Offers less error correction capability compared to RS-FEC.

{% hint style="warning" %}
If one end uses RS-FEC and the other uses FC-FEC, the link will not work.\
The encode/decode processes differ and are not compatible.
{% endhint %}

![](<../.gitbook/assets/Unknown image (388)>)

![](<../.gitbook/assets/Unknown image (389)>)

CRC-32 (commonly used in Ethernet and other standards) uses the generator polynomial 0x04C11DB7, which is a 32-bit polynomial.

CRC-16 uses the polynomial 0x8005 or similar, which is 16 bits long.

CRC-3 uses a polynomial like x³ + x + 1, which is 1011 in binary (4 bits).

### Media and Ethernet switching basics

When you connect three or more devices, you typically need a network device (usually a switch).\
Each cable between an end device and the switch is a **segment**.\
Ethernet cables and segments have limited maximum length.

![](<../.gitbook/assets/Unknown image (390)>)

#### Ethernet

is a unified naming structure IEEE Standard 802.3 that defines the data transmission metrics such as speed, the medium lengths, the Layer 2 physical addressing and other technical properties for proper communication between systems

Ethernet is baseband technology that defines the physical and data link layer specifications, allowing devices to connect and communicate within a local network

Ethernet was primarily designed and used for LANs because of its simplicity, cost-efficiency, and ease of deployment, however it is also used by Service providers for transport purposes

Fast Ethernet (10/100Mbps) and 1/10/40/100 Gigabit Ethernet are evolution of Ethernet, defining higher transmission speed

Ethernet is popular because it strikes a good balance between speed, cost and ease of installation

Other LAN types include Token Ring, Fiber Distributed Data Interface (FDDI), Asynchronous Transfer Mode (ATM)

Note A link by definition is a medium over which two nodes can communicate at link layer

#### Shared Ethernet

refers to a networking environment where multiple devices share the same Ethernet segment or medium for communication

In the beginning the stations were connected by coaxial cable or a hub. which was a shared communication medium between all stations

#### Switched Ethernet

network refers to a networking environment utilizing switch, that intelligently perform packet switching based on Layer 2 address

#### Repeater

is a device that sends the received signal on one interface to another interface and thus amplifies the signal and enables transmission over a longer distance

According to the types of signal generated: Analog repeater, Digital repeater

By type of network to which they connect: Wired repeater, Wireless repeater

#### Collision domain

is a group of stations whose interaction can lead to a collision

Collision occurs when two or more devices attempt to transmit data on a shared network segment simultaneously, causing signal to be distorted and thus unreadable

#### Hub

was a precursor to the Ethernet switch that wasn't aware of Layer 2 addressing, essentially functioning like a repeater

Hub repeated a signal received on one port to all other ports, except the port, from which the frame was received

Hubs had internal physical structure as a common bus. This means that all devices connected to the hub shared the same bus medium of a hub and thus the same bandwidth.

Although the hub has multiple ports like a switch, once multiple devices start transmitting at the same time, a collision can occur, which means that the ports on the hub do not separate devices from each other and fall into the same collision domain.

![](<../.gitbook/assets/Unknown image (391)>)

![CSMA/CD Explained - Study CCNA](<../.gitbook/assets/Unknown image (392)>)

Hub extends LAN segments and serves as a repeater to amplify a signal that might be degraded during transmission.

Hub can still be useful for troubleshooting purposes

Monitoring Network Traffic: By connecting a monitoring device (such as a network analyzer or packet sniffer) to a port on the hub, you can capture and analyze all traffic passing through the hub. This can help diagnose network issues, identify abnormal traffic patterns, and troubleshoot connectivity problems.

#### Access to the shared medium

Deterministic: only one station can access the medium at a time.\
Example: Token Ring (only device with the token can send).

Random: stations transmit whenever they have data.\
This is common in Ethernet.

However collisions may occur, since stations can transmit whenever it has data to send.

In older networks collision detection technology called CSMA/CD was developed

In modern networks this is solved with full-duplex communication

**Carrier Sense Multiple Access with Collision Detection (CSMA/CD)**

was a networking protocol implemented on a NIC circuitry used in Ethernet networks to manage access to the shared communication medium

It is no longer used in modern Ethernet networks as it was replaced by full-duplex ethernet

Carrier sense - The NIC contains circuitry that allows it to monitor the electrical signals on the network medium

This circuitry enables the NIC to detect whether the medium is currently in use (i.e., whether another device is transmitting) or idle.

Before transmitting data, a device would listen to the network medium to determine if any other devices were currently transmitting

If the medium was idle (no other transmissions were detected), the device would proceed to transmit its data

Multiple access - means that multiple devices can attempt to transmit data on the same medium

Collision detection - The NIC is also equipped with collision detection mechanisms that enable it to identify when collisions occur during transmission. When a device transmits data, its NIC simultaneously listens for any signals being received from the network. If the NIC detects signal while it is transmitting, it stops the transmission and proceeds to send jam signal to inform and synchronize all devices on the shared medium to stop sending, and after certain period of time it attempts to retransmit signal

JAM signal carries a 32-bit binary pattern sent by a data station to inform the other transmitting stations of the collision and that they must not transmit

The JAM pattern is specific set of binary value that is distinguishable from a regular data signal

After sending the JAM signal, the device waits for a random period of time before attempting to retransmit its data

This random backoff period helps to minimize the likelihood of another collision

Backoff Algorithm - In the event of a collision, the NIC implements a backoff algorithm to determine when it can attempt to retransmit the data. This algorithm involves selecting a random backoff period during which the NIC waits before attempting to retransmit. The backoff period is increased exponentially with each successive collision, helping to reduce the likelihood of another collision occurring when multiple devices attempt to retransmit simultaneously

#### Broadcast domain

refers to a group of devices within a local area network (LAN) that are connected by a Layer 2 network device, such as a switch

Stations within the same broadcast domain are directly reachable by broadcast messages from other stations

#### Bridge

is a device similar to the switch that is not used in modern networks anymore, however it's maximum capacity are 2 ports, so the bridge was primarily used only to connect "bridge" two LAN networks. Moreover packet forwarding in a bridge is performed in software, which is slower and less efficient

#### Switch

is a more intelligent device with specialized hardware, software and memory that can temporarily store and process signals, allowing switch to process multiple concurrent signal streams from multiple stations - called microsegmentation

Switches operate at multiple layers of the OSI model. At Layer 1 of the OSI model, switches provide an interface to the physical media. At Layer 2 of the OSI model, they provide switching of frames based on MAC addresses. Therefore, switch problems are generally seen as Layer 1 and Layer 2 issues

The switch internal physical structure allows the switch to isolate individual ports and its traffic. This means that collisions on one port will not affect traffic on other ports

Switch will delay sending signal if it detects that another station is already sending signal on the same segment as the one to which the switch should send the signal, separating each port into it's collision domain - in case there are multiple devices sharing the same medium connected to the port

The figure shows that each switch interface (also called a switch port) connects to a single PC or server. Each switch port represents a segment. By default, a switch and all interconnected switches belong to a single LAN.

![](<../.gitbook/assets/Unknown image (393)>)

![](<../.gitbook/assets/Unknown image (394)>)

For each port there is a buffer memory that acts as a queue into which frames are be stored. Queues are available for both incoming and outgoing data

This queue is acting as FIFO - (first in first out), so each frame is serviced based on the sequence they are received by the switch.

This can be changed and configure switch to send frames with higher priprity, called QoS

Ethernet switch supports Full-duplex communication, where half of the UTP wires are to receive and other half of UTP wires to send traffic, eliminating collisions to happen completely

For example, point-to-point 100-Mbps connections have 100 Mbps of transmission capacity and 100 Mbps of receiving capacity for an effective 200-Mbps capacity on a single connection.

The switch examines MAC addresses and stores them with ingress ports in CAM (MAC address table).\
See also: [Packet Forwarding Architecture](../l3/packet-forwarding.md)

As devices are added or removed from the network, the switch update it's CAM table, adding new MAC addresses dynamically and aging out those that were disconnected

A switch can store multiple MAC address entries on a single port, so that another switch can be connected to it to further expand the network and available ports. Multiple MAC addresses can also indicate that there is connected some virtualization server hosting multiple virtual machines.

Switch operates transparently, so they don't modify the data packets passing through them, their role is just to learn and forward the traffic between stations

Bridge Group/System MAC Address is switch's MAC address used to identify the switch within the network used primarily for control protocols like STP

![](<../.gitbook/assets/Unknown image (395)>)

Packet switching is performed on dedicated hardware called Application Specific Integrated Circuits (ASIC), which are fundamental to how an Ethernet switch works. An ASIC is a silicon microchip designed for a specific task (such as switching or routing packets) rather than general-purpose processing such as a CPU. A generic CPU is too slow for forwarding traffic in a switch. While a general-purpose CPU may be fast at running a random application on a laptop or server, manipulating and forwarding network traffic is a different matter. Traffic handling requires constant lookups against large memory tables.

Media-rate adaptation function that allows switches to adjust transmission rate or speed of data flow between connected devices based on their individual transmission capabilities

![](<../.gitbook/assets/Unknown image (396)>)

#### Unmanaged switch

(i.e. not configurable) switch does not need and does not have any own MAC addresses because it is never a source or a destination of an Ethernet frame. It simply relays frames between its ports without being their source or destination. To the hosts connected to this switch, it is not visible; sometimes we say it is a transparent switch or bridge

This makes them the go to choice for home networks, and, small business installations

#### Managed switch

is a switch with an added intelligence to be able to participate in special control functions like STP, CDP,LLDP,SSH or Etherchannel, where it is originator or receiver of the control messages, so it must have its own MAC address to generate frame with the source of his egress port to the destination of the control protocol

Cisco Catalyst switches usually have one unique MAC address per physical port, plus a set of surplus MAC addresses for diverse virtual interfaces (Port-channels, Switched Virtual Interfaces)

![](<../.gitbook/assets/Unknown image (397)>)

![](<../.gitbook/assets/Unknown image (398)>)

![](<../.gitbook/assets/Unknown image (399)>)

![](<../.gitbook/assets/Unknown image (400)>)

![](<../.gitbook/assets/Unknown image (401)>)

#### Modern Switches

L2 switches are more than 20+ years capable to process frames based on the EtherType directly on ASICs

This allows them to modify Layer 3 header, for example to assign a packet an DSCP value for QoS tagging (since CoS is not commonly utilized) or they also can proceed to examine Layer 4 to determine the source and destination port to run features such as DHCP Snooping, where the switch tracks the IP assignments and intercepts specifically DHCP packets, that can be recognized on the Layer 4 or even on the application layer 5

Switches connect LAN segments, determine the segment to send the data, and reduce network traffic. Some important characteristics of switches:

High port density: Switches have high port densities: 24-, 32-, and 48-port switches operate at speeds of 100 Mbps, 1 Gbps, 10 Gbps, 25 Gbps, 40 Gbps, and 100 Gbps. Large enterprise switches may support hundreds of ports.

Large frame buffers: The ability to store more received frames before having to start dropping them is useful, particularly when there may be congested ports connected to servers or other heavily used parts of the network.

Port speed: Depending on the switch, port speed may be possible to support a range of bandwidths. Ports of 100 Mbps, 1Gbps, and 10 Gbps are expected, but 40- or 100-Gbps ports allow even more flexibility.

Fast internal switching: Having fast internal switching allows higher bandwidths: 100 Mbps, 1 Gbps, 10 Gbps, 25 Gbps, 40 Gbps, and 100 Gbps.

![](<../.gitbook/assets/Unknown image (402)>)

### Network Adapters

Port is a single connection point on a network device for connecting a host to a network, so that it can send and receive data

A port is typically located on the front or back panel of a network device, such as a switch or a router, and can be used to connect devices like computers, servers, printers, and other networking equipment.

#### Network interface card (NIC)

in order for the device to be able to send data and connect to the site, a Network interface card (NIC) is needed, which is already integrated into the motherboard, or it can be connected externally (also called Ethernet Adapter)

There are several types of network cards, each designed for its own way of communication, whether it is electric, optical (using light) or wireless

Each NIC is distinguished by its address

Gigabit Ethernet Dual-Identity Enhanced High-Speed WAN Interface Card (EHWIC) cards support various technologies for high-speed internet access, digital subscriber line services, synchronous digital transmission, mobile data connectivity, and wireless broadband communication

PCI (Peripheral Component Interconnect) is a common connection interface for attaching peripherals to a motherboard in a PC

Personal Computer Memory Card International Association (PCMCIA) is a standard for external expansion cards used in portable computing devices

In addition, it allows so-called hot swapping, i.e. swapping while the system is running without the need for shutdown or reboot

![](<../.gitbook/assets/Unknown image (403)>)

Modular refers to a design or system characterized by the use of separate components or modules that can be independently created, replaced, or modified without affecting the entire system. In a modular approach, complex systems are broken down into smaller, more manageable units or modules, each serving a specific function

### Network Interface Module (NIM) and Line Card (LC)

Both NIM and Line Card are components which contain multiple ports that can be plugged to the modular switches or router chassis to extend the base build-in ports of the network device. Line cards are hot-swappable, which means they can be inserted or removed from the chassis without powering down the device

Linecards and NIMs can also be modular, allowing to build custom port-density, forwarding speed and power efficiency, since all the ports in the fixed linecards and NIMs must be powered, even though they are not used for active connections

Modular Port Adapter (MPA) are individual components containing various port parameters, that can be plugged into modular line card or NIM

![](<../.gitbook/assets/Unknown image (404)>)

Switch Module/Slot is the physical location where line cards are inserted

![](<../.gitbook/assets/Unknown image (405)>)

### Stack

refers to the group of network devices interconnected, creating one entity. Each device is identified by a stack number

See: [Switch Stacking](/broken/spaces/5O3GcKL6Aux8mNe6vDmI/pages/5cbfcb865cdbbb9021d53181232471ada2621478)

stack number/module number/port number

interface gigabitethernet1/0/4 on 3750 means interface 4 on module 0 on stack switch 1.

### Patch cord

Is a short, flexible cable used to connect two electronic or optical devices for signal routing. In networking, patch cords are commonly used to connect devices like switches, routers, and patch panels within a structured cabling system. They serve as temporary or permanent connections in telecommunications, data centers, and other networked environments

![](<../.gitbook/assets/Unknown image (406)>)

## Metallic

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

## Fiber Optics

Light travels in optical fibers through a phenomenon called **total internal reflection**. This occurs when light travels from a denser medium (such as the glass core of an optical fiber) to a less dense medium (such as the cladding that surrounds the core). The light will bend away from the interface between the two media and continue to reflect back and forth within the core of the fiber. This process allows light to travel long distances with very little loss of signal

Light signals used in fiber optics have a much higher frequency than electrical signals transmitted through copper wires, allowing for the encoding of more data onto each light signal, resulting in greater data transmission capacity.

Light signals propagate through fiber optic cables at near the speed of light, which is much faster than the speed of an electron in the case of electric current transmission

The speed of light is 299,792,458 meters per second in a vacuum. The lack of vacuum conditions in a fiber-optic cable or a copper wire slows down the speed of light by a ratio known as the refractive index; the larger the refractive index value, the slower light travels.

The average refractive index value of an optical fiber is about 1.5. The speed of light through a medium v is equal to the speed of light in a vacuum c divided by the refractive index n, or v = c / n. This means the speed of light through a fiber-optic cable with a refractive index of 1.5 is approximately 200,000,000 meters per second (that is, 300,000,000 / 1.5).

Fiber is not affected by lightning, corrosion, or temperature changes as much as copper

Fiber can carry signals tens or hundreds of kilometers before amplification, while copper supports -

Standard Ethernet over copper (Cat5e–Cat6A) → 100 meters max

High-speed Cat8 (25/40G) → 30 meters

Longer runs require repeaters, switches, or fiber optics

![](<../.gitbook/assets/Unknown image (306)>)

![](<../.gitbook/assets/Unknown image (307)>)

Fiber optic systems use **Wavelength Division Multiplexing (WDM)** to simultaneously transmit multiple signals over the same fiber by using different wavelengths (colors) of light

This effectively increases the amount of data that can be transmitted over a single fiber optic cable, leading to greater bandwidth capacity.

Fiber optic cables can support parallel data transmission, where multiple data streams are transmitted simultaneously over separate optical fibers within the same cable

Fiber optic transmission uses less power than copper because the optical signal experiences much lower attenuation and does not suffer from electrical losses like resistance, skin effect, or crosstalk. As a result, it requires less signal amplification and digital signal processing, which reduces overall energy consumption, especially at higher speeds and longer distances.

Light signals are immune to **Electromagnetic interference (EMI)** and experience minimal loss of intensity over long distances, allowing for signal transmission over much greater distances compared to copper wires

WDM is protocol-agnostic - e.g it operates independently of any specific system, standard, or technology. It can seamlessly support various protocols such as Ethernet (at any rate), Fibre Channel, and others.

Additionally, WDM is also bitrate-agnostic—each wavelength can operate at a different bitrate. As new technologies emerge that support higher bitrates, the same fiber optic infrastructure remains compatible, enabling transmission without the need for significant physical upgrades. This flexibility makes WDM a future-proof solution for evolving network demands

![](<../.gitbook/assets/Unknown image (308)>)

### Transmission

Before transmitting data over the optical cable, the digital data (0s and 1s) are encoded into optical signal

One common encoding technique is called **Non-Return-to-Zero (NRZ)**, where a high intensity light pulse represents a binary 1, and the absence of a pulse represents a binary 0

At the receiving end of the optical cable, a **photodetector** or **photodiode** is used to detect the incoming optical signals. When a light pulse arrives, the photodetector generates an electrical signal, indicating the presence of a binary 1.

Conversely, when no light pulse arrives, the photodetector does not generate any electrical signal, indicating the absence of a binary 0

The electrical signals generated by the photodetector are then processed by electronic circuits in a network device to extract the original digital data

### Fiber Optic Cable Structure

Electrical pulses generated on a network devices are converted by the transceiver and sent as light pulses over the fiber optic cable that is made of glass or plastic fibers that allows the light to propagate through the fiber optic cable from the beggining of the cable to the other end of the cable

It is the most expensive medium on the market, but with considerable transmitting improvements. Same as metallic cable - the optic cable is shortly referred to as patch cord

**Core** is the central part of the fiber where light propagates surrounded by the **cladding** that helps to confine light within the core

**Buffer coating** protects the fiber from environmental factors such as moisture and physical damage.

**Strengthener** serves to increase cable's durability and resistance to external forces and prevent mechanical damage

**Jacket** is the outer layer that provides additional protection and durability.

Fiber-optic cables can be made up of pure silica, doped silica, glass composite, or plastics. Fibers made from pure or doped silica have the best characteristics for telecommunications. Glass composite or plastic fibers have high attenuation and low bandwidth, and are used in short distance, low transmission rate, and lighting systems.

#### Indoor optical fiber

Indoor optical fiber is mainly suitable for horizontal wiring subsystems and vertical backbone subsystems. The following figure presents the components of an indoor optical fiber.

![](<../.gitbook/assets/Unknown image (309)>)

#### Outdoor optical fiber

The outdoor optical fiber has higher tensile strength and a thicker protective layer. The figure shows a typical outdoor optical fiber.

![](<../.gitbook/assets/Unknown image (310)>)

### Single-mode vs multi-mode

**Single-mode fiber (SMF)** uses a single beam of light in the center of the 9 µm wide core, typically generated by a laser. This single-mode transmission minimizes signal distortion caused by dispersion because the light follows a single path (or mode) through the fiber. As a result, SMF is capable of supporting higher transmission speeds and covering longer distances, up to 80–100 km without signal boosters, and potentially up to 180 km or more with advanced technologies like **Dense Wavelength Division Multiplexing (DWDM)**. The smaller core and precise manufacturing, along with the use of laser transmitters, make SMF more expensive. SMF cables are usually identified by their yellow outer jacket

![](<../.gitbook/assets/Unknown image (311)>)

**Multi-mode fiber (MMF)** uses a larger core, typically 50 or 62.5 µm, and allows multiple light beams, generated by LED emitters, to propagate simultaneously through the core. These beams travel at different angles and follow multiple paths (modes), which is why it is called “multimode.” This design makes MMF suitable for short-distance communication, such as within buildings or data centers, where distances are typically limited to 550 meters for 10 Gbps Ethernet and up to 2 kilometers for lower speeds like 100 Mbps or 1 Gbps. MMF cables are generally less expensive than SMF, and their jackets are often orange or aqua . However, the multiple paths of light in MMF lead to modal dispersion, which limits its effective range and speed compared to SMF

![](<../.gitbook/assets/Unknown image (312)>)

The key reason a single beam of light (in SMF) can carry more data than multiple beams (in MMF) lies in the reduction of modal dispersion. In MMF, the various paths (modes) taken by light beams differ slightly in length, causing them to arrive at different times and creating signal overlap and distortion. This limits the data rate and distance MMF can achieve. In SMF, with only one path for light to follow, there is minimal signal overlap, allowing for clearer transmission and the ability to encode more data into the single beam

![](<../.gitbook/assets/Unknown image (313)>)

![](<../.gitbook/assets/Unknown image (314)>)

Single-mode fiber (SMF) transceivers can sometimes be physically connected to multi-mode fiber (MMF) cables, but significant signal loss will likely result in a non-functional or unreliable link; conversely, multi-mode fiber (MMF) transceivers are not compatible with single-mode fiber (SMF) cables due to the mismatch in core sizes, preventing effective light transmission

![](<../.gitbook/assets/Unknown image (315)>)

### Optical transceivers (transmitters/receivers)

is an electronic component that converts electrical signals into optical signals and vice versa. Optical transceivers are used in telecommunications networks to connect various devices, such as routers, switches, and servers.

**Operation**

Stream of digital information is sent to a physical layer device, whose output is a light source (an LED or a laser) that interfaces a fiber optic cable. This device converts the incoming digital signal from an electrical form (electrons) to an optical form (photons), or an electrical-to-optical (E-O) conversion. Electrical "1" and "0" bits trigger a light source that flashes (for example: light = 1, little or no light = 0) into the core of an optical fiber (Optical 1 and 0 bits can be represented by flashes of light of different magnitudes rather than flash or no-flash signaling).

E-O conversion is non-traffic affecting. Pulses of light propagate across the optical fiber by way of total internal reflection. At the receiving end, another optical sensor (photodiode) detects light pulses and converts the incoming optical signal back to electrical form. A pair of fibers (one transmit fiber, one receive fiber) usually connects any two devices. Optical signals carried by fiber optic cables must be converted into electrical signals, so that electronic devices can process and forward signals further. Optical transceivers, also known as optical transmitters and receivers, are used in fiber optic networks to convert electrical signals into optical signals for transmission over fiber optic cables, and vice versa. The example of an optical transceiver that serves as a modular pluggable interface is an SFP. SFP modules can also serve as a reduction in case you need to connect distinct devices, one with metallic RJ45 ports and the second with pure SFP ports—using a metallic SFP allows you to connect an RJ45 cable to the SFP port of the second device. SFPs are available for both optical cables and copper cables, providing flexibility for different networking scenarios

An SFP (regular or DWDM) is typically limited to one or a few wavelengths.

DWDM systems rely on DWDM SFPs and Mux/Demux to carry multiple wavelengths over a single fiber, enabling high-density optical communication

#### GBIC (Gigabit Interface Converter) Predecessor of SFP

SFP (Small Form-Factor Pluggable) is a compact, hot-pluggable/swappable (can be inserted or removed from a networking device without turning it off) network interface module format used for data transmission. SFP interface on networking hardware is a modular slot for a media-specific transceiver, such as for a fiber-optic cable or a copper cable

The advantage of using SFPs compared to fixed interfaces (e.g. modular connectors in Ethernet switches) is that individual ports can be equipped with different types of transceivers as required.

Single mode: Single-mode optical transceivers are designed for high-bandwidth applications and are typically used in long-haul networks.

Multimode: Multimode optical transceivers are less expensive and less sensitive to alignment errors than single-mode transceivers. They are typically used in shorter-reach applications, such as data centers and campus networks.

| show int <> capabilities        | to display SFP information                                                                                                                                                   |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| service unsupported-transceiver | to allow swithc to accept non-cisco SFP's - shut and no shut them since they have been err-disabled If the port still won't come up, try to adjust the speed and negotiation |

![](<../.gitbook/assets/Unknown image (1251)>)

#### **Types**

SFP: Standard SFP modules for Fast Ethernet and Gigabit Ethernet.

SFP+: Enhanced SFP modules for 10 Gigabit Ethernet.

SFP28: SFP modules for 25 Gigabit Ethernet

Note Cisco SFP+ (10 Gbps) and SFP28 (25 Gbps) modules have the same physical dimensions

QSFP (Quad Small Form-factor Pluggable)

larger modules designed for higher data rates, starting from 40 Gigabit Ethernet and going up to 400 Gigabit Ethernet or more

They have a larger, more complex connector with multiple channels

Types

QSFP: Originally designed for 4-channel 10 Gigabit Ethernet (40Gbps).

QSFP+: Enhanced version of QSFP, supporting 4-channel 10 Gigabit Ethernet (40Gbps)

QSFP28: Supports 4-channel 25 Gigabit Ethernet (100Gbps)

QSFP56: Supports 4-channel 50 Gigabit Ethernet (200Gbps)

**Important notes**

The SFP module types must match on both ends - e.g 10g on both or 1G on both, there is no compatibility between 1G and 10G. It is also possible to plug 1G SFP module to 10G port of the switch to support 1G switches with 1G SFP module.

If you want to plug an SFP+ (smaller dimension SFP) to QSFP port - you would have to order the CVR-QSFP-SFP10G adapter so that the SFP+ can be plugged into the larger QSFP port

Example below shows uplink module for 40/100G QSFP, supporting 10G with the mentioned adapter

![](<../.gitbook/assets/Unknown image (1252)>)

#### SFP types

**SFP+ vs QSFP+**

The primary difference between QSFP+ and SFP is the quad form. QSFP+ is an evolution of QSFP to support four 10 Gbit/s channels carrying 10-Gigabit Ethernet, 10G Fiber Channel, or InfiniBand, which allows for 4x 10G cables and stackable networking designs that achieve better throughput. QSFP+ can replace 4 standard SFP+ transceivers, resulting in greater port density and overall system cost savings over SFP+. Get details about QSFP here: What Is QSFP+ Module: QSFP+ Transceiver Wiki and Types.

| **Name**       | **Nominal\*\*\*\*speed** | **Lanes** | **Standard**                                                                                                       | **Introduced** | **Backward-compatible**                                                                                                              | [**PHY**](https://en.wikipedia.org/wiki/PHY#Ethernet_physical_transceiver)\*\* interface\*\* | **Connector**  |
| -------------- | ------------------------ | --------- | ------------------------------------------------------------------------------------------------------------------ | -------------- | ------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------- | -------------- |
| SFP            | 100 Mbit/s               | 1         | [SFF](https://en.wikipedia.org/wiki/Small_Form_Factor_Committee) INF-8074i                                         | 2001-05-01     | None                                                                                                                                 | MII                                                                                          | LC, RJ45       |
| SFP            | 1 Gbit/s                 | 1         | [SFF](https://en.wikipedia.org/wiki/Small_Form_Factor_Committee) INF-8074i                                         | 2001-05-01     | 100 Mbit/s SFP\*                                                                                                                     | SGMII                                                                                        | LC, RJ45       |
| cSFP           | 1 Gbit/s                 | 2         |                                                                                                                    |                |                                                                                                                                      |                                                                                              | LC             |
| SFP+           | 10 Gbit/s                | 1         | [SFF](https://en.wikipedia.org/wiki/Small_Form_Factor_Committee) SFF-8431 4.1                                      | 2009-07-06     | SFP                                                                                                                                  | XGMII                                                                                        | LC, RJ45       |
| SFP28          | 25 Gbit/s                | 1         | [SFF](https://en.wikipedia.org/wiki/Small_Form_Factor_Committee) SFF-8402                                          | 2014-09-13     | SFP, SFP+                                                                                                                            |                                                                                              | LC             |
| SFP56          | 50 Gbit/s                | 1         |                                                                                                                    |                | SFP, SFP+, SFP28                                                                                                                     |                                                                                              | LC             |
| SFP-DD         | 100 Gbit/s               | 2         | SFP-DD MSA [<sup>\[18\]</sup>](https://en.wikipedia.org/wiki/Small_Form-factor_Pluggable#cite_note-sfp-dd.spec-18) | 2018-01-26     | SFP, SFP+, SFP28, SFP56                                                                                                              |                                                                                              | LC             |
| SFP112         | 100 Gbit/s               | 1         |                                                                                                                    | 2018-01-26     | SFP, SFP+, SFP28, SFP56                                                                                                              |                                                                                              | LC             |
| SFP-DD112      | 200 Gbit/s               | 2         |                                                                                                                    | 2018-01-26     | SFP, SFP+, SFP28, SFP56, SFP-DD, SFP112                                                                                              |                                                                                              | LC             |
| **QSFP types** |                          |           |                                                                                                                    |                |                                                                                                                                      |                                                                                              |                |
| QSFP           | 4 Gbit/s                 | 4         | [SFF](https://en.wikipedia.org/wiki/Small_Form_Factor_Committee) INF-8438                                          | 2006-11-01     | None                                                                                                                                 | GMII                                                                                         |                |
| QSFP+          | 40 Gbit/s                | 4         | [SFF](https://en.wikipedia.org/wiki/Small_Form_Factor_Committee) SFF-8436                                          | 2012-04-01     | None                                                                                                                                 | XGMII                                                                                        | LC, MTP/MPO    |
| QSFP28         | 50 Gbit/s                | 2         | [SFF](https://en.wikipedia.org/wiki/Small_Form_Factor_Committee) SFF-8665                                          | 2014-09-13     | QSFP+                                                                                                                                |                                                                                              | LC             |
| QSFP28         | 100 Gbit/s               | 4         | [SFF](https://en.wikipedia.org/wiki/Small_Form_Factor_Committee) SFF-8665                                          | 2014-09-13     | QSFP+                                                                                                                                |                                                                                              | LC, MTP/MPO-12 |
| QSFP56         | 200 Gbit/s               | 4         | [SFF](https://en.wikipedia.org/wiki/Small_Form_Factor_Committee) SFF-8665                                          | 2015-06-29     | QSFP+, QSFP28                                                                                                                        |                                                                                              | LC, MTP/MPO-12 |
| QSFP112        | 400 Gbit/s               | 4         | [SFF](https://en.wikipedia.org/wiki/Small_Form_Factor_Committee) SFF-8665                                          | 2015-06-29     | QSFP+, QSFP28, QSFP56                                                                                                                |                                                                                              | LC, MTP/MPO-12 |
| QSFP-DD        | 400 Gbit/s               | 8         | [SFF](https://en.wikipedia.org/wiki/Small_Form_Factor_Committee) INF-8628                                          | 2016-06-27     | QSFP+, QSFP28, [<sup>\[19\]</sup>](https://en.wikipedia.org/wiki/Small_Form-factor_Pluggable#cite_note-Cisco-400G_QSFP-DD-19) QSFP56 |                                                                                              | LC, MTP/MPO-16 |

**SFP-DD (Double-density)**

SFP112: 100 Gbit/s using PAM4 on a single pair (not double density)

SFP-DD: 100 Gbit/s using PAM4 and 50 Gbit/s using NRZ

SFP-DD112: 200 Gbit/s using PAM4

QSFP112: 400 Gbit/s (4 × 112 Gbit/s)

QSFP-DD: 400 Gbit/s/200 Gbit/s (8 × 50 Gbit/s and 8 × 25 Gbit/s)

QSFP-DD800 (formerly QSFP-DD112): 800 Gbit/s (8 × 112 Gbit/s)

OSFP (Octal Small Format Pluggable)

capable of 800 Gbit/s links between network equipment. It is a slightly larger version than the QSFP form factor allowing for larger power outputs

**OEM SFP (Original Equipment Manufacturer SFP)** refers to a Small Form-Factor Pluggable (SFP) module that is typically designed and produced by a third-party manufacturer rather than the Original Brand of the networking device (e.g., Cisco, Juniper, Arista, etc.). These modules are compatible with the original brand's hardware but are not manufactured by the brand itself.

But they are not recommended, since they may not function or be compatible with the original devices at all

**LPO (Linear Pluggable Optics)**

is a type of optical module designed for use in short-reach, linear optical networks. It employs SERDES with built-in equalization to handle signals over lower-loss channels. While it offers advantages like reduced power consumption, it has limitations in terms of reach and full interoperability due to limited gain and equalization capabilities.

**Bidirectional (Bidir) SFP** with single strand cables - WDM

**When Would You Use Regular Two-Strand Fiber Instead?**

If you have plenty of available fiber strands, traditional duplex SFPs (two-strand) may be preferable since they are more common, widely supported, and sometimes slightly cheaper than BiDi SFPs.

If your network design does not support WDM or requires specific transceiver compatibility, two-strand fiber might be the default choice.SFP modules shown in the images are 1000BASE-BX10 BiDi (Bidirectional) SFPs, which are designed for use over single-strand (single-mode) fiber (SMF). These modules operate in pairs—GLC-BX-U (Upstream) and GLC-BX-D (Downstream)—using Wavelength Division Multiplexing (WDM) to transmit and receive signals over a single fiber strand

Scenarios Where These Modules Are Useful

Fiber Scarcity (Limited Fiber Availability)

If there is only one fiber strand available between locations (instead of a fiber pair), BiDi SFPs allow full-duplex communication over that single fiber.

Useful in situations where deploying additional fiber is difficult or expensive.

Cost-Effective Fiber Utilization: Maximizes the number of connections that can be run through an existing fiber infrastructure.

If there is an ODF between locations, these modules can be a good fit, especially in environments like campus networks, data centers, and service provider networks, where single-strand fiber routing is common.

### Patch cords (simplex vs duplex)

**Single-fiber patch cord**

Description: One fiber, uses different wavelengths (e.g., 1310 nm for TX, 1550 nm for RX) via WDM for bidirectional transmission.

![Patch cord SC/UPC-LC/UPC SM 10m Simplex Slim](<../.gitbook/assets/Unknown image (316)>)

**Dual-fiber patch cord**

Description: Two fibers, one for RX and one for TX, using the same wavelength (e.g., 1310 nm).

It consists of one strand for sending data (tx) and another strand for receiving data (rx)

![](<../.gitbook/assets/Unknown image (317)>)

### Connector types and polishing

The main difference between the connector types is the dimensions and mechanical connection methods

Three basic types of connectors: threaded, bayonet, push-pull

Connectors are made of the following materials: metal, plastic housing

**Lucent/Little Connector (LC)** are the most prevalent type for SFP modules. They are small, secure, and designed for use with fiber optic cables. LC connectors are used for both single-mode and multi-mode fiber connections. Moreover they have RJ45-like valve. For enterprise equipment and small form-factor pluggable (SFP) modules

**Subscriber/Standard Connector (SC)** are larger than LC connectors and are used primarily for older networking - gigabit interface converter GBIC, X2 or older SFP's CFP, CPAK. For enterprise equipment

**ST (Straight Tip)** connectors are commonly used in older networking equipment and are known for their robustness. They feature a bayonet-style twist lock and are widely used in both networking and industrial applications. For patch panels

**FC (Fiber Connector)** are used primarily in telecommunication networks and are known for their threaded coupling mechanism, which provides a secure connection. They are commonly used in test equipment and in some older fiber optic networks. For patch panels and in service providers

**MTP/MPO (Multi-Fiber Push-On/Pull-Off)** connectors are designed to accommodate multiple fibers in a single connector, making them ideal for high-density applications like data centers. They are commonly used for backbone cabling and in high-speed networks.

MPO connectors typically come in 12-fiber or 24-fiber versions. In the 24-fiber MPO connector, fibers are arranged in two rows of 12 fibers (a 2x12 configuration), while in a 16-fiber MPO, they are arranged in a 2x8 configuration.

**SC/APC and LC/APC** These connectors are similar to their SC and LC counterparts but feature an angled physical contact (APC) polish on the fiber end face, which helps reduce back reflections. They are commonly used in applications requiring high signal quality, such as long-distance transmission and fiber-to-the-home (FTTH) installations.

Note Cisco doesn't offer convertors such as 10/100/1000Base-T/1000Base-LX (SC, SM, 10km, 1310nm)

MT-RJ: For enterprise equipment

In data communications and telecommunications applications today, small form factor (SFF) connectors (for example, LCs) are replacing the traditional connectors (for example, SCs) mainly to pack more connectors on the faceplate and, as a result, reduce system footprints.

![](<../.gitbook/assets/Unknown image (318)>)

![](<../.gitbook/assets/Unknown image (319)>)

![](<../.gitbook/assets/Unknown image (320)>)

**LC/UPC (Ultra Physical Contact):**

Polished with no angle (0°)

Return Loss: ≤ -50 dB

Ideal for data, voice, and enterprise networks

Color code: Blue

LC/APC (Angled Physical Contact):

Polished with an 8° angle

Return Loss: ≤ -60 dB

Best for GPON, 10G-PON, RF video, and long-distance links

Color code: Green

Pro tip:

Never mix LC/UPC and LC/APC — it leads to high insertion loss and signal issues.

![No alternative text description for this image](<../.gitbook/assets/Unknown image (321)>)

[Standards](https://en.wikipedia.org/wiki/Small_Form-factor_Pluggable)

### Common Ethernet optics and standards

100Base-FX (IEEE 802.3u) – a version of Fast Ethernet that uses multi-mode optical fiber, up to 412 meters long.

1000Base-SX (IEEE 802.3z) – 1 Gigabit Ethernet running over multi-mode fiber-optic cable.

1000Base-LX (IEEE 802.3z) – 1 Gigabit Ethernet running over single-mode fiber or MMF. Up to 5 kilometers on SMF, shorter on MMF

1000BASE-ZX Medium: Single-mode fiber (SMF). Range: Up to 70-100 kilometers. Usage: Very long-range Gigabit Ethernet, suitable for WAN or metropolitan networks.

10GBASE-SR (IEEE 802.3ae) – supports 10 Gbps connections over multi-mode fiber, up to 300m range.

10GBASE-LR (IEEE 802.3ae) – supports 10 Gbps connections over single-mode fiber, up to 10 km range.

1000BASE-ZX Medium: Single-mode fiber (SMF). Range: Up to 70-100 kilometers. Usage: Very long-range Gigabit Ethernet, suitable for WAN or metropolitan networks.

Note SFP-10GBase-LRM is long reach multimode for 220m supporting also single mode

40GBASE-SR4, 40GBASE-CSR4, 40GBASE-FR, 40GBASE-LR4, 40GBASE-ER4 - various standards for 40 Gbps Ethernet over fiber.

100GBASE-SR10, 100GBASE-LR4, 100GBASE-ER4 - signal is carried over four wavelengths reaching 100Gbps.

Multiplexing and demultiplexing of the four wavelengths are managed within the device

LR = long reach eg. long wavelength with large encoding

SR = short reach e.g short wave length with large encoding

SFP-10G-SR-S pro 400m multimode

SFP-10G-LR-S pro 10km single mode

SFP-10G-ER pro 40km single mode

SFP-10G-ZR pro 80km single mode

10GBASE-AOC - with 50m reach

Gray Optics refers to the setup of interface and transceiver that can send and receive only one fixed wavelength, thus it doesn't support DWDM

![](<../.gitbook/assets/Unknown image (322)>)

### DAC and AOC

Direct Attach Cables (DAC) is a copper cable with connectors on both ends. DAC cables are designed for intra-rack interconnection e.g within the same rack

Active Optical Cables (AOCs) are multimode fiber optic cables with SFP connectors on both ends. This design requires an external power source to drive the signal, which is why “active” appears in the name. Signals do not pass passively through fiber optic lines.

The AOC cable has an optoelectric transceiver on the cable ends that converts the electrical signals into optical signals and then sends them over the fiber. AOC cable typically offers a higher transmission distance of up to 100 meters or more and can transmit at higher speeds of up to 400 Gbps

If the distance is less than 5 meters, it is most cost-effective to use DAC cables; if the distance is more than 5 meters, it is better to use AOC cables, They are also called Twinax

Optical transceivers are listed here

### Breakout

Breakouts take advantage of ports with multiple optical lanes for both the Tx and Rx

• Optical lanes in this context means pairs of fibers

• e.g. 400G DR4 optical connector has 4 pairs of fibers, each pair can be configured as a 100G-DR

• A breakout is when a single port is configured as multiple lower speed interfaces

• Breakout transceivers generally use MPO connectors which have multiple fibers for

both the Tx and Rx

• The port controls how the module will be configured either for breakout or non breakout operation

• For 2x breakout, module’s support regular LC connectors too

• Breakouts can also be done for copper cables and AOCs

• Cables and AOCs are fixed for either breakout or non-breakout applications

A breakout cable contains multiple individual fibers, each carrying its own independent signal. These fibers are not multiplexed together. Instead, each fiber operates as a separate data link.

For example:

If you have an MPO/MTP breakout cable (which is common in high-density data centers), a single MPO/MTP connector on one end might split into multiple LC or SC connectors on the other end. This doesn’t mean multiple devices are sharing one port—it means one high-density port (like a 40G QSFP+) is being broken out into multiple lower-speed ports (like 4×10G SFP+).

Cisco provides patch panel solutions for breakout deployments [https://www.cisco.com/c/en/us/products/collateral/interfaces-modules/transceiver-modules/patch-panel-breakout-connectivity-so.html](https://www.cisco.com/c/en/us/products/collateral/interfaces-modules/transceiver-modules/patch-panel-breakout-connectivity-so.html)

![](<../.gitbook/assets/Unknown image (323)>)

![](<../.gitbook/assets/Unknown image (324)>)

![](<../.gitbook/assets/Unknown image (325)>)

![](<../.gitbook/assets/Unknown image (326)>)

### Wavelength Division Multiplexing (WDM)

Faced with the challenge of dramatically increasing capacity while constraining costs, carriers have two options: install new fiber or increase the effective bandwidth of existing fiber.

Laying new fiber is the traditional means used by carriers to expand their networks. Deploying new fiber, however, is a costly proposition. Increasing the effective capacity of existing fiber can be accomplished in two ways:

Increase the bit rate of existing systems.

Increase the number of wavelengths on a fiber.

WDM is a technology that enables the simultaneous transmission of multiple signals over the same optical fiber by using different wavelengths of light (each representing a different color).

It basically aggregate optical signals into a single pair of cable

![Introduction to WDM Theory. WDM is the abbreviation for Wavelength… | by longyinn | Medium](<../.gitbook/assets/Unknown image (327)>)

#### Coarse Wavelength Division Multiplexing (CWDM)

operates in the wavelength range from around 1270 nm to 1610 nm using wider spacing between channels (typically 20 nm) compared to DWDM, allowing for simpler and less expensive optical components.

CWDM systems are often used in shorter-distance applications, such as metropolitan area networks (MANs) or access networks, where the demand for bandwidth is moderate and cost-effectiveness is critical

#### Dense Wavelength Division Multiplexing (DWDM)

operates in the C-band (conventional band) and L-band (long wavelength band) of the optical spectrum, typically from around 1525 nm to 1610 nm

DWDM systems are capable of transmitting data over longer distances and are commonly deployed in Long-haul (LH) (dlouhy dosah) and ultra-long-haul networks, including submarine cables and backbone networks. It utilizes tighter channel spacing (typically 0.8 nm or less), enabling higher channel counts and greater scalability compared to CWDM.

The difference between WDM and DWDM is fundamentally one of degree. DWDM spaces the wavelengths more closely than WDM does and, therefore, has a greater overall capacity. DWDM has various other notable features. These include the ability to amplify all the wavelengths at once without first converting them to electrical signals, and the ability to carry signals of different speeds and types simultaneously and transparently over the fiber (protocol and bit rate independence). WDM and DWDM use single-mode fiber to carry multiple light waves of differing frequencies

850 nm (LED)

1310 nm (LED or laser)

1550 nm (high-quality laser)

![](<../.gitbook/assets/Unknown image (328)>)

![](<../.gitbook/assets/Unknown image (329)>)

#### ITU-T Grid

defines the standard channel spacing and frequencies used to design Wavelength Division Multiplexing (WDM) for optical networks. This grid is simply a channel plan that can be used to support interoperability between multiple vendors.

While this grid defines a standard, users are free to use the wavelengths in arbitrary ways and to choose from any part of the spectrum

Each wavelength (lambda, denoted in nanometers, nm) corresponds to a specific channel in the WDM system.

These channels are centered around 1550 nm, which corresponds to 193 THz in frequency. This wavelength range is in the C-band, a common band used in optical communication due to low attenuation

Frequency (THz) and wavelength (nm) are inversely proportional, meaning higher frequencies correspond to shorter wavelengths (simply: wavelength is the same as frequency)

0.4 nm Spacing: The distance between adjacent wavelengths in terms of nanometers.

50 GHz Spacing: The corresponding spacing between channels in frequency terms.

These spacings ensure that channels are distinct and do not interfere with one another

The center channel is defined at 1552.52 nm or 193.1 THz. This acts as a reference for the ITU grid

DWDM systems require very precise wavelengths of light in order to operate without inter-channel distortion or crosstalk. Several individual lasers are typically used to create the individual channels of a DWDM system. Each laser operates at a slightly different wavelength.

![](<../.gitbook/assets/Unknown image (330)>)

Performance by Fiber Type and Wavelength:

G.652 (Conventional NDS Fiber): Widely used standard fiber. Works well in the O-band (1260–1360 nm) due to low attenuation and low dispersion.

Can operate in the C-band (1530–1565 nm) but requires dispersion compensation because dispersion increases significantly here.

G.653 (DS Fiber): Optimized for the C-band (1530–1565 nm) with almost zero dispersion to support single-wavelength systems.

However, it's bad for DWDM because zero dispersion causes non-linear effects like four-wave mixing (FWM).

G.655 (NZDS Fiber): Designed for DWDM in C-band (1530–1565 nm) and L-band (1565–1625 nm).

Has small but non-zero dispersion, which reduces non-linear effects like FWM and improves multi-channel systems.

Which Wavelength is Best for Each Technology?

Ethernet (Early/1.3 µm): Best in O-Band (1260–1360 nm), typically on G.652 fibers.

DWDM (Dense Wavelength Division Multiplexing): Best in C-Band (1530–1565 nm) and L-Band (1565–1625 nm), primarily on G.655 fibers.

Long-Haul Communication: Works best in C-band and L-band using G.655 fibers due to low attenuation and low non-linear effects.

![](<../.gitbook/assets/Unknown image (331)>)

| **Band**                  | **Wavelength Range (nm)** | **Use / Explanation**                                                                                                                                    |
| ------------------------- | ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **O-Band (Original)**     | 1260–1360                 | Used mainly for legacy telecom systems and metro Ethernet. Signals travel with minimal chromatic dispersion, making it good for short-haul transmission. |
| **E-Band (Extended)**     | 1360–1460                 | Rarely used in practice due to high water peak attenuation in standard fibers; some interest in newer low-water-peak fibers.                             |
| **S-Band (Short)**        | 1460–1530                 | Also limited by attenuation, but sometimes explored for additional capacity in experimental systems.                                                     |
| **C-Band (Conventional)** | 1530–1565                 | **Most widely used** band in DWDM systems due to optimal fiber loss and amplifier (EDFA) support; core of long-haul and metro DWDM networks.             |
| **L-Band (Long)**         | 1565–1625                 | Used to extend the capacity of C-band systems; often deployed in conjunction with C-band in high-capacity long-haul DWDM networks.                       |
| **U-Band (Ultra-long)**   | 1625–1675                 | Mostly used for **fiber monitoring** and supervisory channels (e.g. OTDR testing), due to higher fiber attenuation.                                      |

1. Frequency Ranges

C-band: \~4.4 THz (4400 GHz) for standard, \~6 THz (6000 GHz) for Super C-band.

L-band: \~7.2 THz (7200 GHz).

C+L band: \~11.6 THz (11,600 GHz) for standard C-band + L-band, or \~13.2 THz (13,200 GHz) with Super C-band.

2. Channel Counts by Spacing

![](<../.gitbook/assets/Unknown image (332)>)

| **Channel Spacing (GHz)** | **C-band Channels (4.4 THz)** | **Super C-band Channels (6 THz)** | **L-band Channels (7.2 THz)** | **C+L Band Channels (11.6 THz / 13.2 THz)** |
| ------------------------- | ----------------------------- | --------------------------------- | ----------------------------- | ------------------------------------------- |
| 50                        | 88                            | 120                               | 144                           | 232 (264)                                   |
| 75                        | 58                            | 80                                | 96                            | 154 (176)                                   |
| 100                       | 44                            | 60                                | 72                            | 116 (132)                                   |
| 150                       | 29                            | 40                                | 48                            | 77 (88)                                     |
| 200                       | 22                            | 30                                | 36                            | 58 (66)                                     |
| 300                       | 14                            | 20                                | 24                            | 38 (44)                                     |
| 400                       | 11                            | 15                                | 18                            | 29 (33)                                     |
| 500                       | 8                             | 12                                | 14                            | 23 (26)                                     |
| 600                       | 7                             | 10                                | 12                            | 19 (22)                                     |
| 800                       | 5                             | 7                                 | 9                             | 14 (16)                                     |
| 1000                      | 4                             | 6                                 | 7                             | 11 (13)                                     |

Practical Considerations: Spacings beyond 400 GHz are rare because they reduce channel counts significantly (e.g., only 11 channels at 1000 GHz in C+L band), making them less efficient for dense multiplexing. Most DWDM systems use 50–150 GHz for optimal capacity, with 400 GHz for high-baud-rate applications.

### Optical power budget and dB math

**Optical power budget** is the total amount of optical power available in a system to overcome all losses (attenuation, connector losses, splice losses) and ensure the signal reaches the receiver with sufficient strength for proper detection. In the optical domain, the transmitters create optical signals with an output power measured in dBm - it is an absolute measurement referenced to 1mW

Power is measured in watts (W), but in telecommunications, it is often expressed in decibels relative to 1 milliwatt (dBm). The 'm' in dBm indicates that the 0 dBm reference point corresponds to 1 milliwatt (1 mW = 0.001 watt)

Optical budget is affected by:

Transmitter Power: The power level of the optical signal generated by the transmitter (typically a laser or LED), measured in dBm.

Receiver Sensitivity: The minimum optical power required at the receiver to decode the signal reliably.

Optical Budget=Transmitter Power (dBm)−Receiver Sensitivity (dBm)

dB and dBm values are additive in calculations: you can add or subtract dB values to or from a dBm value to determine the resulting power level after a gain or loss.

![](<../.gitbook/assets/Unknown image (333)>)

![](<../.gitbook/assets/Unknown image (334)>)

10dBm = 10mW

logarithm is the inverse of an exponent. It answers the question:

"To what power must 10 be raised to equal this number?"

It compresses large or small numbers into manageable scales by representing them as powers of 10.

![](<../.gitbook/assets/Unknown image (335)>)

Why Use Logarithms for dBm?

In communications, power levels can vary hugely. For example:

1 mW (milliwatt) is small.

1,000,000 mW (1,000 W) is huge.

If we worked directly with these numbers, it would be messy and hard to compare. Using logarithms simplifies this:

Instead of saying "1,000 times bigger," we say "+30 dB".

Instead of saying "0.001 times smaller," we say "-30 dB"

Law of Zero A value of 0 dB means that the two absolute power values are equal

Law of 3s +3 dB = double the power; -3 dB = half the power

Law of 10s A value of 10 dB = 10 times the power; value of −10 dB = 1/10 of the the power

In systems like fiber optics, where power levels combine or attenuate, logarithmic units allow us to add or subtract directly rather than multiply or divide

![](<../.gitbook/assets/Unknown image (336)>)

![](<../.gitbook/assets/Unknown image (337)>)

### Fiber Optic Transmission Impairments

#### Micro and macro bends in fiber

There are two main types of bends in optical fibers: macrobends and microbends. Microbends are small, localized bends that are typically caused by manufacturing defects or external forces. Macrobends are larger bends that can be caused by improper installation or cable damage.

Microbends are more common than macrobends, but they are also less harmful. This is because microbends only cause a small amount of signal loss, and the light can usually be re-coupled back into the core of the fiber. However, macrobends can cause significant signal loss and data corruption. In some cases, macrobends can even cause the fiber to break.

If you must bend an optical fiber, it is important to do so gently and gradually. You should also avoid bending the fiber in the same place repeatedly.

For reflection to occur, the angle of incidence must be less than a critical value. When the light hits the cladding at a more acute angle, it can penetrate the cladding and be absorbed in the coating. The signal is thus attenuated and might be lost.

#### Fiber splicing

Optical fiber splicing is the process of joining two optical fibers together to create a continuous optical path, typically using fusion or mechanical methods. It ensures minimal signal loss, is cost-effective for maintenance, and is commonly used in network installations and repairs

Repair breaks: Damaged or severed fibers can be repaired through splicing, restoring functionality and avoiding costly cable replacements.

Extend reach: Splicing multiple shorter cables can extend the total length of a fiber-optic link, reaching farther distances without signal degradation.

Customize connections: Splicing allows for the creation of custom fiber optic configurations, catering to specific needs and layouts.

Following are types of splicing techniques:

Fusion splicing: This method uses an electric arc to melt the two fiber ends together, creating a permanent and high-quality bond. It's the most common and reliable splicing technique.

Mechanical splicing: This method uses pre-aligned sleeves and clamps to hold the fiber ends together. It's faster and easier to perform than fusion splicing but offers slightly lower optical performance.

Following are the steps involved in splicing optical fibers:

Preparing the fibers: Both fiber ends are stripped of their protective coatings and cleaved to create clean, perpendicular faces.

Alignment: The fiber ends are precisely aligned in a splicing machine using various methods such as core-to-core alignment or profile alignment.

Fusion or clamping: In fusion splicing, the fibers are fused together with an electric arc. In mechanical splicing, the fibers are clamped within the pre-aligned sleeve.

Testing and inspection: The spliced joint is tested for optical loss and inspected for any imperfections.

Following are the safety considerations to keep in mind while splicing optical fibers:

Laser safety: Fusion splicers use high-powered lasers, so proper eye protection and adherence to safety protocols are essential.

Fiber handling: Fibers are fragile and susceptible to damage, so careful handling and proper tools are crucial.

Cleanliness: Dust and contamination can significantly impact the quality of the splice, so maintaining a clean work environment is vital.

Splice Losses: Typically 0.1–0.3 dB per splice.

Safety Margin: Extra power reserved for unforeseen losses or aging effects (typically 3–6 dB).

These losses must be subtracted from the total optical budget, leaving less available for the fiber's inherent attenuation

#### Contamination (dirt,oil on connectors)

Contamination shows up as darker spots on the fiber. These deposits can be transferred to an otherwise clean fiber when installed, so be sure to clean them off.

Light Loss: When light encounters dirt or contamination on a fiber connector, it gets scattered and absorbed, leading to signal loss. This weakens the signal and reduces the efficiency of data transmission.

Increased Attenuation: The presence of dirt acts like a barrier, attenuating the light signal and further diminishing its strength. This can lead to slower data speeds, unreliable connections, and even complete signal failure.

Reflections: Contaminants can also cause light to reflect back within the fiber, creating unwanted reflections that interfere with the original signal. This can distort the data and lead to errors and data corruption.

Reduced System Lifespan: Neglecting cleaning can shorten the lifespan of your fiber-optic equipment, leading to premature degradation and the need for more frequent replacements.

Inspect before and after cleaning to ensure no contamination.

Dry Cleaning: Use lint-free wipes or swabs.

Wet Cleaning: Use fiber-safe cleaning solutions for tough grime.

Warnings:

Do not clean or inspect connectors with active optical power.

Optical power can damage eyes and skin.

Ensure proper grounding and avoid unfiltered magnifiers.

#### Attenuation

**Attenuation** refers to the gradual loss of signal strength as light travels through the fiber. Factors contributing to attenuation include absorption by the fiber material and scattering losses. Attenuation is typically measured in decibels per kilometer (dB/km) and varies with wavelength

When optical signals are multiplexed or demultiplexed, loss (attenuation) of the optical signal occurs. Loss associated with coupling (multiplexing), or filtering (demultiplexing) optical signals is referred to as insertion loss

Different wavelengths of light experience different levels of attenuation in optical fiber. Three transmission “windows” are employed by modern fiber-optic systems. These windows are centered about the 850 nm, 1300 nm, and 1550 nm wavelengths in the infrared spectrum

Fiber-optic systems that use the 850-nm window use a light-emitting diode (LED) as the source. This is referred to as a multimode system (850-nm wavelength) and it uses multimode fiber. LED light scatters much easier than laser light and is only used for short-distance transmission.

Single-mode systems use a laser as the light source (1310 nm and 1550 nm wavelengths) and employ single-mode fiber.

Path Pannel/ Connector Losses: Typically 0.3–0.5 dB per connector

Fiber Losses: Attenuation due to fiber characteristics (e.g., 0.2 dB/km in modern single-mode fibers at 1550 nm).

![](<../.gitbook/assets/Unknown image (338)>)

The total attenuation (in dB) is proportional to the fiber length.

If the attenuation is too high, the received signal may fall below the receiver sensitivity.

Example: If fiber attenuation is 0.2 dB/km and the system has a 20 dB optical budget, the maximum distance is:

![](<../.gitbook/assets/Unknown image (339)>)

<>

#### Chromatic Dispersion (CD)

**Chromatic Dispersion (CD)** is a phenomenon where different wavelengths of light travel at slightly different speeds within an optical fiber.

The relationship between wavelength and speed is such that longer wavelengths travel faster than shorter wavelengths

This causes the optical pulses carrying data to spread out over time, leading to pulse broadening, which can cause overlapping of signals or Inter-Symbol Interference (ISI). This makes it harder for the receiver to distinguish between consecutive bits, leading to errors, especially at higher data rates. The amount of dispersion varies with wavelength and fiber type. For example, standard single-mode fibers (G.652) have zero dispersion around 1310 nm but exhibit increasing dispersion at longer wavelengths.

The longer the fiber, the more time light has to spread out, increasing the dispersion effect

Dispersion Factor This is a property of the fiber that determines how much dispersion occurs per unit length

Chromatic Dispersion becomes a bigger issue at higher data rates because pulses are shorter (narrower in time). Even a small spread can cause significant overlap, limiting the maximum transmission distance

![](<../.gitbook/assets/Unknown image (340)>)

![](<../.gitbook/assets/Unknown image (341)>)

### Detection and compensation

#### Direct Detection

This is the simpler of the two detection methods, traditionally used in optical communication systems.

How It Works:

Detects the intensity (power) of the incoming light signal (1 or 0 light on/light off), but ignores phase and polarization information.

Commonly uses On-Off Keying (OOK), where the presence or absence of light represents binary data

Dispersion (spreading of light pulses) and other impairments like chromatic dispersion and polarization mode dispersion are corrected using physical components like Negative Dispersion Fiber Modules (DCM/DCF): also called Dispersion Compensating Units (DCUs), which are fibers specially designed to reverse the dispersion effect by introducing an opposite dispersion.

In negative dispersion fibers, this relationship is reversed. That is, shorter wavelengths travel faster than longer wavelengths

Downsides:

They add losses, which means signal amplification might be needed.

These corrections are costly, bulky, and less adaptable to dynamic network changes.

Can only address a narrow range of impairments (e.g., doesn't handle nonlinear effects well).

Correction is done mostly at the physical layer, leaving limited flexibility for additional compensation

To ensure proper performance, networks using Direct Detection must limit transmission distances and channel spacing.

Often requires signal regenerators at intermediate points in the network to restore degraded signals, adding cost and complexity.

Relies on basic Forward Error Correction (FEC) to handle errors but lacks advanced processing capabilities to further improve performance

Note Each device participating in the light transmission like xponder, DCU and also FEC calculation adds minor latency to the transmission, however in modern systems it is close to zero

![](<../.gitbook/assets/Unknown image (342)>)

#### Coherent detection

This is a more advanced approach, moving the impairment detection and correction from the physical domain (DCU) to digital domain within DSP

How It Works:

Detects not only the intensity of light but also the phase and polarization of the optical signal.

Uses a local oscillator (laser) to mix the incoming light signal, which allows for extracting phase and frequency information.

Requires advanced Digital Signal Processing (DSP) for signal reconstruction and impairment correction

Most impairments (dispersion, nonlinearities, polarization mode dispersion) are corrected using DSP.

This eliminates the need for physical components like DCUs, reducing cost, complexity, and physical space requirements.

Handles more errors without requiring frequent signal regeneration

Supports higher-order modulation formats (e.g., QPSK, 16-QAM) that allow more data to be transmitted per optical channel

Implements advanced FEC

Forward Error Correction (FEC) is a technique used to detect and correct a certain number of errors in a bitstream by appending redundant bits and error-checking code to the message block before transmission

BER (Bit Error Rate) measures the fraction of bits received incorrectly compared to total bits sent—it indicates link quality. In coherent optical systems, BER is analyzed both pre-FEC (before error correction) and post-FEC (after correction)

Optical Signal-to-Noise Ratio (OSNR): measures the ratio of the desired optical signal power to the noise power within the system.

It essentially tells you how strong and clear the desired signal is, compared to the unwanted noise that is inevitably present in the system

A high OSNR means the signal is strong compared to the noise (good quality). A low OSNR means the noise is relatively strong compared to the signal (poor quality).

Factors affecting OSNR include amplifier noise, fiber nonlinearities, and insertion losses from components like connectors and splices.

Analogy: Imagine you're in a room where two people are speaking: one is the person you want to listen to (signal), and the other is just background chatter (noise). If the person you want to hear speaks much louder than the other person, you can clearly differentiate their voice, representing a high Optical Signal-to-Noise Ratio (OSNR).

However, if both people are speaking at the same volume, their voices blend together, making it difficult to recognize who you're trying to listen to. This represents a low OSNR, where the noise level is nearly equal to the signal level, degrading communication clarity

![](<../.gitbook/assets/Unknown image (343)>)

![](<../.gitbook/assets/Unknown image (344)>)

### Regeneration (3Rs)

The "3Rs" of Regeneration

**Reshaping:** This step restores the original shape of the optical pulses. As the signal propagates through the fiber, it undergoes distortion, causing the pulses to spread out and overlap. Reshaping aims to recreate the original, clean pulse shape.

**Retiming:** This process reestablishes the timing of the optical pulses. Over long distances, the timing jitter (variations in the arrival time of pulses) can accumulate. Retiming ensures that the pulses arrive at the receiver with the correct timing.

**Reamplification:** This step boosts the optical power of the signal to compensate for the signal loss that occurs during transmission.

This is least preferred method due to:

Cost: Optical regenerators are expensive devices.

Complexity: Implementing regeneration in a network can increase complexity and operational overhead.

Power Consumption: Regenerators consume significant power, which can be a concern for energy-efficient networks.

![](<../.gitbook/assets/Unknown image (345)>)

### Polarization and PMD

Polarization Mode Dispersion (PMD)

is a type of distortion caused by the different speeds at which light in various polarization states travels through an optical fiber

Differental Group Delay (DGD) - the difference in propagation time of the two polarization modes from transmitter to receiver measured in picoseconds.

Polarization Mode Dispersion (PMD) - this quantity relates to DGD and represents the total accrued difference in propagation time between the polarization modes measured in picoseconds.

![](<../.gitbook/assets/Unknown image (346)>)

Polarization refers to its orientation in the horizontal or vertical plane e.g to the orientation of the electric field in a light wave. Polarized light oscillates in only one direction (e.g., up/down, left/right, or diagonally), while unpolarized light has electric field components in all directions.

A polarizer is a material that allows light to pass only in one orientation (vertical, horizontal, or diagonal). Polarizers block or absorb light that does not match their orientation. (Example: Polarized glasses)

![](<../.gitbook/assets/Unknown image (347)>)

![Linear Antenna Polarizations](<../.gitbook/assets/Unknown image (348)>)

![](<../.gitbook/assets/Unknown image (349)>)

Note there is also circularly polarized light, each lens of your 3D glasses can separate images for the left and right eyes, even if your head tilts.

Light Wave Components: Light is an electromagnetic wave with two key components:

An electric field oscillating in one direction.

A magnetic field oscillating perpendicular to it.

Linear Polarization: If the electric field oscillates in only one fixed direction (e.g., up and down or left and right), the light is linearly polarized.

Adding Two Waves: Circular polarization happens when two linearly polarized light waves combine:

These two waves must:

Oscillate perpendicularly to each other.

Be out of phase by 90° (one wave reaches its peak slightly after the other).

Polarization Maintaining (PM) Fiber is a type of single-mode fiber designed to preserve and transmit the polarization state of light, unlike regular single-mode fiber, which does not maintain polarization due to environmental factors.

The polarization state in light refers to its orientation in the horizontal or vertical plane. In regular single-mode fiber, the two orthogonally polarized modes travel at the same speed, leading to possible cross-coupling between them.

PM Fiber forces two orthogonal polarized modes to travel at different velocities (propagation constants).

This characteristic is achieved during the manufacturing process by inducing stresses in the material itself. There are two categories of polarization maintaining fiber (PMF) available, linear polarization maintaining fiber (LPMF) and circular polarization maintaining fiber (CPMF).

Geometric Stress: The core is made elliptical to introduce a polarization-maintaining effect.

Controlled Stress: The fiber is wrapped with areas of high expansion glass that shrink more than the surrounding silica, creating tension that forces the two polarized modes to propagate at different velocities.

Panda Fiber: Common in telecom, this design was developed by NTT Japan in the 1980s and has low attenuation suitable for telecom applications.

Bai Fiber: Designed for sensors and still used for that purpose today. It has higher attenuation and is not suitable for telecom but excels in sensor applications

The stress-induced difference in propagation velocities ensures that the polarization state of the light is preserved along either the fast or slow axis of the fiber, making cross-coupling of light difficult without significant perturbations.

![](<../.gitbook/assets/Unknown image (350)>)

Second Order Polarization Mode Dispersion (SOPMD) - similar to chromatic dispersion, the effect of polarization mode dispersion depends on the wavelength. SOPMD characterizes this dependency with unit picoseconds squared (ps^2).

Polarization Change Rate (PCR) - the average rate at which the polarization states change as the signal traverses the fiber measured in multiples of radians per second.

Polarization Dependent Loss (PDL) - the effective attenuation in dB due to changes in the polarization states across the fiber.

### Optical Fiber Installation and Maintenance

Key Elements of Optical Safety

Providing and mandating the use of appropriate eye protection, such as safety glasses, when working with or near optical fiber systems.

Ensuring that optical fiber cables are organized and secured to prevent accidental tripping or damage.

Implementing safety features in optical transceivers, such as automatic power control and laser safety interlocks.

Ensuring that transceivers operate within specified wavelengths and power limits to prevent harm to human eyes.

Using protective enclosures for splicing and termination points to contain laser light and prevent accidental exposure.

Ensuring proper ventilation in areas where optical fiber equipment is located, and monitoring air quality to address potential environmental hazards.

#### Outdoor installation

Fiber-optic conduits provide fiber-optic cables with superior protection and versatility. Fiber-optic conduits provide the best security and protection for fiber-optic cables. Traditional conduits mostly consist of polyvinyl chloride (PVC) and other nonmetallic materials. In contrast, fiber-optic conduits are primarily constructed of steel or metal, with PVC or fiberglass braiding occasionally incorporated as well.

Choose a cable with a robust outer jacket, resistant to UV radiation, extreme temperatures, and moisture. This usually involves a polyethylene or LSZH (low-smoke zero-halogen) sheath.

![](<../.gitbook/assets/Unknown image (351)>)

#### Splice Enclosure

Fiber-optic splice enclosures are used to protect stripped fiber-optic cable and fiber-optic splices from the environment, and they are available for indoor as well as outdoor mounting. Outdoor fiber-optic enclosures are usually weatherproof with watertight seals.

![](<../.gitbook/assets/Unknown image (352)>)

Trenching: For underground installations, a trench is dug to accommodate the conduit. The depth varies depending on local regulations and soil conditions, but typically ranges from 18 inches to 3 feet.

Aerial installation: For aerial installations, the cable is suspended between poles or other structures using messenger wire or strand.

Splicing and termination: At each connection point, the fibers are carefully spliced together using specialized tools and techniques. The splices are then protected using enclosures.

#### Indoor installation

Plan the cable route to minimize bends and avoid potential sources of damage, such as sharp edges or electrical cables.

In some cases, the cable can be buried directly in the walls or floor, but care must be taken to avoid damage during construction or modifications.

Outdoor cables require a more robust jacket for protection against the elements, while indoor cables need to be flame-retardant.

Conduit is often used outdoors for additional protection, while it's optional indoors.

#### Fiber-optic advanced testing procedures

**Attenuation testing:** Measures the signal loss as it travels through the fiber, ensuring it meets acceptable levels for reliable data transmission.

Continuity testing: Verifies the physical integrity of the fiber and identifies any breaks or disruptions.

**Optical return loss (ORL) testing:** Determines the amount of light reflected back from the fiber connectors, which can affect signal quality.

High return loss typically indicates poor connector quality, such as misalignment, contamination, or damage. This can significantly degrade signal quality and cause performance issues

Chromatic dispersion testing: Measures the spreading of light pulses due to different wavelengths, impacting high-speed data transmission.

**Optical Time Domain Reflectometry (OTDR):** Analyzes the light reflected back from the fiber, revealing breaks, bends, and other anomalies along its length.

An optical light source is needed to provide a stable light signal for testing purposes. A common tool for this is a tunable laser. A tunable laser is useful for testing a new system and for troubleshooting. A typical test is to set the tunable laser for a dense wavelength-division multiplexing (DWDM) wavelength and connect it to the receive port of a 32/40 multiplexer (MUX) card. The port is placed in the Out-of-Service and Maintenance (OOS-MT) state. The internal input power level can be read via Cisco Transport Controller. The signal can be traced through the 32/40 MUX common transmit to the OPT-BST common receive and so on through the system.

Optical time domain reflectometer (OTDR): Generates and analyzes reflected light pulses to map the fiber path and identify issues.

The OTDR sends out a light pulse and then measures the time it takes for the signal to return.

The main purpose of the OTDR measurements is to determine the backscattering response of the fiber under test. This backscattering is the result of noise, insertion losses, and fiber attenuation.

Generally, the output on the screen shows the reflected signal level on a logarithmic scale (dB) on the vertical access and the distance on the horizontal access.

OTDRs can only measure time, so distance is measured based upon a conversion factor of 10 microseconds per km, which is the propagation delay of the fiber.

An OTDR measures:

End-to-end loss

Connector loss

Splice loss

Reflectance

Note OTDR is for diagnosing and mapping the fiber, while an Optical Power Meter is for checking signal strength.

![](<../.gitbook/assets/Unknown image (353)>)

**Optical power meter:** Measures the intensity of the light signal at different points in the fiber. Optical power meters are commonly used during the installation, maintenance, and troubleshooting of fiber-optic systems. Technicians use them to verify that signal levels are within the acceptable range, to identify issues such as signal loss or excessive power, and to ensure the overall health of the optical communication infrastructure.

**Photodetector/Photodiode:** The core of an optical power meter is a photodetector or photodiode. This semiconductor device converts optical signals into electrical signals. The amount of electrical current generated is proportional to the optical power received.

**Calibrated Sensor:** The photodetector is often part of a calibrated sensor that accurately measures the optical power in terms of decibels (dB) or milliwatts (mW). Calibration ensures that the readings are reliable and consistent.

**Wavelength Range:** Optical power meters are designed to operate within specific wavelength ranges. It is important to choose a meter that matches the wavelength of the optical signals being measured. Common wavelength ranges include 850 nm, 1310 nm, and 1550 nm.

**Connector Types:** Optical power meters come with different types of connectors, such as SC, FC, ST, or LC, depending on the specific requirements of the fiber-optic network. The connector type must match the connectors used in the optical link.

**Display Unit:** The device typically has a digital or analog display unit that shows the measured optical power. Readings are usually displayed in decibels or milliwatts. Some modern power meters may have additional features, such as the ability to display the wavelength or store measurements.

![](<../.gitbook/assets/Unknown image (354)>)

### Attenuators and splitters

**Fiber Optic Attenuators**

used to reduce optical signal strength when it is too strong, ensuring proper transmission and preventing receiver saturation

Helps equalize power levels in multi-wavelength systems.

**Types of Fiber Optic Attenuators**

Fixed Attenuators: Have a set attenuation value (e.g., 5 dB) and use doping or other mechanisms to reduce power.

Variable Attenuators: Allow adjustable attenuation levels (e.g., 1 dB to 20 dB) using methods like neutral density filters.

**Applications of Attenuators**

Testing Power Levels: Used in fiber optic testing to simulate signal loss.

Permanent Installation: Installed in communication links to balance transmitter and receiver signal levels.

**Common Designs of Optical Attenuators**

Fixed Attenuators: Female-to-female or male-to-female connectors.

Variable Attenuators: Can be adjusted using a screw, nut, or other mechanisms.

Instrument-Type Attenuators: Used for precise testing, with high attenuation ranges (e.g., 5 dB to 70 dB) and fine resolution (0.1 dB or even 0.01 dB).

![Guideline for Fixed Fiber Attenuator](<../.gitbook/assets/Unknown image (355)>)

![Guideline for Fixed Fiber Attenuator](<../.gitbook/assets/Unknown image (356)>)

Note Attenuators can be stacked together to further increase the attenuation as shown below, where 3 attenuators of 5dbm are stacked together and connected with female connector to another cable

![5dB 5dB](<../.gitbook/assets/Unknown image (357)>)

Fiber optic splitters introduce attenuation as they divide optical signals among multiple output ports. The attenuation occurs due to power distribution and insertion loss. The theoretical attenuation for a 1:N splitter can be estimated using the formula:

![](<../.gitbook/assets/Unknown image (358)>)

For example, a 1:32 splitter has an expected loss of approximately 15 dB, but real-world factors such as insertion loss and manufacturing tolerances can increase this to 16–18 dB. The actual attenuation varies based on splitter quality, fiber type, and operating wavelength (e.g., 1310 nm or 1550 nm).

### Serializer/Deserializer (SerDes)

are electrical links that move signals between connected optics and ASICs

It handles data formatting for transmission, since the data from the device still needs to be serialized before being converted into parallel light pulses for transmission through the optical fiber. Similarly, at the receiving end, the incoming light pulses are deserialized - converted back into electrical signals to recover the original parallel data

![](<../.gitbook/assets/Unknown image (359)>)

### Increasing the data rate

Doubling the baud rate has always been lowest cost, lowest power solution for doubling data rate.

• Faster baud rate = wider (& fewer) channels

• 400G (Class 2) utilized 75 GHz channel spacings

• 800G will move to 150 GHz spacings

Baud Rate Class

• 1.6T anticipated to move to 300 GHz spacings

![](<../.gitbook/assets/Unknown image (360)>)

## Wireless

**Wireless communication** sends data over radio frequencies (RF).\
It uses electromagnetic waves instead of a physical cable.

Wireless communication is a method of transmitting data in the form of radio waves transmitted at certain radio frequencies (RF), without the need for a physical medium connection

### Overview

The transmitter sends an alternating current into a section of wire, that is antenna, which sets up moving electric and magnetic fields that propagate out and away from the wire as traveling waves. The electric and magnetic fields travel along together and are always at right angles to each other.

The signal must keep changing, or alternating, by cycling up and down, to keep the electric and magnetic fields cycling and pushing ever outward

The waves produced from the tiny point ideal antenna expand outward in a spherical shape in all directions

At the receiving end of a wireless link, the process is reversed. As the electromagnetic waves reach the receiver’s antenna, they induce an electrical signal

A radio wave traveling from point A to point B will be characterized by three properties. These are the Amplitude, Wavelength, and Frequency. Amplitude is referred to as the Power of the signal itself. Radio signals lose power when traveling over a distance. Therefore, there is a direct relation between increasing the power or amplitude of a signal and increasing the distance it travels. Wavelength is the distance from one peak in the wave to the next. Frequency is how often a wave will oscillate per second measured in Hertz \[Hz]. Wavelength is inversely proportional to Frequency. Therefore, the higher the Frequency, the lower the Wavelength and vice versa.

All wireless technology is based on the basics of wireless radio communication. These wireless communication basics are:

* All devices communicate at the same frequencies
* Overlapping and interfering communication needs to be regulated
* Signal power from both source and destination affects the coverage area
* Multiple different factors affect coverage area, such as the type of antenna and space around wireless devices

![](<../.gitbook/assets/Unknown image (276)>)

#### Wireless access points (AP)

**Wireless access points (APs)** are Layer 2 devices whose primary function is to bridge 802.11 WLAN traffic to 802.3 Ethernet traffic. APs can have internal (integrated) or external antennas to radiate the wireless signal and provide coverage with the wireless network.

AP forms a Wireless local area networks (WLANs) and connects the wireless users to the wired network infrastructure, such as an Ethernet network, through an Ethernet cable, enabling wireless clients to communicate with wired network

{% hint style="info" %}
In wireless communications, frames are broadcast over the air.\
Anyone in RF range can receive them.\
Security depends on the WLAN security method (encryption/auth).
{% endhint %}

### RF signal fundamentals

#### Frequency

**Frequency** is the number of times the signal makes one complete up and down cycle in 1 second. Measured in Hertz

For wireless the most common frequencies used in LAN networks is unlicensed 2.4GHz band, 5Ghz band or 6Ghz band

![](<../.gitbook/assets/Unknown image (277)>)

* Hertz (Hz): cycles per second
* Kilohertz (kHz): 1000 Hz
* Megahertz (MHz): 1,000,000 Hz
* Gigahertz (GHz): 1,000,000,000 Hz

#### Phase

**Phase** is a measure of difference between two signals

![](<../.gitbook/assets/Unknown image (278)>)

#### Wavelength

**Wavelength** is a measure of the physical distance that a wave travels over one complete cycle; measured in meters; expressed in lambda λ. 2.4GHz is 12.497cm; 5GHz is 6cm

In general, high frequency is associated with shorter wavelength, and low frequency is associated with longer wavelength. This relationship is described by the equation c = λν, where c is the speed of light, λ is the wavelength, and ν is the frequency. As frequency increases, the wavelength decreases, and vice versa.

![](<../.gitbook/assets/Unknown image (279)>)

![](<../.gitbook/assets/Unknown image (280)>)

#### Amplitude (signal power)

**Amplitude (signal power)** measures in watts (W) the strength of signal (top peak to the bottom peak of the signal’s waveform). Decreases tremendously over distance = attenuation

For example: AM radio station broadcasts at a power of 50,000 W; FM radio station use 16,000 W. Wireless transmitter between 0.1 W (100 mW) and 0.001 W (1 mW)

The same decibel theory from fiber optics applies to the electromagnetic waves

![](<../.gitbook/assets/Unknown image (281)>)

#### Polarization

**Polarization** is electrical field wave’s orientation, with respect to the horizon.

Electromagnetic waves have two primary polarization states defined by the electric and magnetic fields.

{% hint style="info" %}
In modern systems, polarization can also carry information (via modulation).\
Antenna alignment still matters for many real deployments.
{% endhint %}

![](<../.gitbook/assets/Unknown image (282)>)

Antennas that produce vertical oscillation are vertically polarized; those that produce horizontal oscillation are horizontally polarized.

Antenna polarization at the transmitter must be matched to the polarization at the receiver or the received signal can be severely degraded

Bottom example of mismatched polarization:

![](<../.gitbook/assets/Unknown image (283)>)

### Carrying data over an RF signal

**Carrier signal** is steady, predictable frequency by which modulation can be applied to transfer information

**Modulation** is the process by which a carrier signal is changed to transfer information

**Frequency Modulation (FM)** Binary data is represented by variations in the frequency of radio waves.

**Amplitude Modulation (AM)** Binary data is represented by variations in the amplitude of radio waves.

**Phase Modulation (PM)** Binary data is represented by variations in the phase of radio waves.

![](<../.gitbook/assets/Unknown image (284)>)

![](<../.gitbook/assets/Unknown image (285)>)

### Shared medium, channels, and CSMA/CA

All wireless technology sends radio waves on one of two main frequencies: 2.4GHz or 5GHz. Both of these frequencies are assigned by the IEEE as free use unlicensed frequencies, hence why most wireless devices use them. Wireless radio waves sent through the air all share the same space in which they travel. When multiple devices send wireless radio waves within the same time frame, this leads to overcrowded airspace that can lead to issues, such as signal collision

Since all stations share one medium - that is certain radio frequency range with one AP, they also share one collision domain

Wi-Fi operates on frequency channels within a band (e.g., channel 1, 6, 11 in 2.4 GHz).

One channel is shared by all devices on that frequency.

There’s no separate uplink and downlink channel like in cellular networks. Both upload and download use the same frequency/channel, just at different times (time-division).

On a given band (e.g., 2.4 GHz), all SSIDs use the same RF channel. So:

If all 5 SSIDs are on 2.4 GHz, they share the same 20 MHz channel.

It's still one half-duplex medium, just split into virtual interfaces.

Clients across different SSIDs must still take turns to transmit.

CSMA/CD does can't solve the collision in wireless network, due to wireless transmitters desensing their receivers during the transmission

Moreover most of the signal energy is lost during the transmission, so collision detection signal would result in only 5 to 10% additional energy for other stations to sense it, which is no effective, so **CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance)** was developed, unlike CSMA/CD, the CSMA/CA avoids collision to occur rather than detect them

CSMA/CA is designed so that each station has roughly equal access to the radio channel so the more stations attached to the access point the higher the potential latency and lower effective throughput to an individual station

This is accomplished feature called **Request to Send/Clear to Send (RTS/CTS)**

A station will send an RTS signal to a WAP to let it know that it is ready to transmit.

Once the WAP has received the RTS signal, it then responds with a CTS reply. The Wap and then halts all other traffic while the station sends its data

Before transmitting data, the station waits for an **Interframe Space (IFS)** period after the channel becomes idle

**Contention window** is a time that is divided into time slots. When the station is ready for data transmission after waiting for IFS then it chooses the random amount of slots for waiting. After waiting for the random number of slots if the channel is still busy then the station does not initiate the whole process again, the station stops its timer and restarts again when the channel is sensed idle

There may be a chance of collision or data may be corrupted during the transmission. Positive acknowledgment and time-out are used in addition to ensuring that the receiver has successfully received the data

![](<../.gitbook/assets/Unknown image (286)>)

### Signal power and RF link budget

The power of the signal being transmitted affects the amplitude of the wave, which therefore affects the distance that the signal travels. This directly affects the overall wireless coverage of these devices.

Signal power is not the only factor affecting coverage for access points. The type of antennas used in devices directly affects the coverage area. There are many types of antennas that are used in wireless devices.

Some of these antenna types are:

* Omnidirectional antennas
* Directional antennas
* Patch antennas

Radio waves hitting walls in their path will have their power absorbed by the wall. The amount of power absorbed by the wall differs based on the material that the wall is made of. For example, a dry concrete wall differs from a glass wall. Electrical equipment, such as generators, construction tools, and microwaves cause electromagnetic interference with the radio signals. Weather will also affect radio signals. For example, on a rainy day, the humidity in the air increases. This makes the radio signals more likely to refract. All these different situations combined have an effect to reduce the overall coverage area of APs.

Signal absorption, reflection, refraction, diffraction and scatterring should be taken into consideration as they cause signal degradation

**Power/Link budget** is a summary of all the gains and losses in a transmission system

In wireless communication the signals are electrical and can be represented as waveforms with amplitude measured in volts (V)

Relative amplitude measured in Decibels (dB) (relative change), which represents a gain or loss in power as a signal travels through the space. It is a ratio of two different power levels used to measure signal gain or loss. 10 mW = 10 dBm

**Effective isotropic radiated power (EIRP)** is the actual power level that will be radiated from the antenna (regulation by law applied)

EIRP (dBm) = Tx Power (dBm) − Tx cable loss (dB) + Tx antenna gain (dBi)

**Free space path loss (FSPL)** decreasing amplitude strength of wave due to wave's expansion and spreading during transmission

**Received signal strength indicator (RSSI)** is a measure of observed energy received by an antenna of a signal (best 0 to worst -100dBm)

**Sensitivity level** is the dBm threshold at which a receiver distinguishes between intelligible and unintelligible signals

**Noise** is unexpected signals received on the same frequency as the receiving frequency; ranging from 0 dBm (worst) to −100 dBm (best) or more

**Noise floor** is the average dBm strength of received erroneous signals

**Signal-to-Noise Ratio (SNR)** difference between RSSI of intended signal and the noise floor

![](<../.gitbook/assets/Unknown image (287)>)

![](<../.gitbook/assets/Unknown image (288)>)

![](<../.gitbook/assets/Unknown image (289)>)

### 802.11 standards and Wi‑Fi

**IEEE 802.11** defines the protocols for wireless local area networks (WLANs) such as what RF signals, modulation, coding, bands, channels, and data rates should be used

The Wi-Fi standards up through 802.11ac have operated on the principle that only one device can claim air time to transmit to another device (half duplex)

In 2009, a new amendment was ratified, 802.11n, which tried to address the negative aspects of previous amendments. Using different techniques (modulation, beamforming, and so on), it improved the performance of the wireless significantly, also increasing the data rates up to 600 Mbps. 802.11n is the first amendment that supports both frequency bands, 2.4 GHz and 5 GHz, and is, therefore, backward compatible with all the existing amendments—802.11a, b, and g.

Wi-Fi 5 (802.11ac) introduced MU-MIMO and 256-QAM.

Wi-Fi 6 (802.11ax) introduced OFDMA, TWT, and uplink MU-MIMO.

Wi-Fi 6E is not a new protocol but extends Wi-Fi 6 into the 6 GHz band.

Wi-Fi 7 (802.11be) supports 4096-QAM, Multi-Link Operation (MLO), and 320 MHz channels, improving peak throughput and latency.

Wi-Fi 8 (802.11bn) is under development with goals around AI/ML integration and 100 Gbps+ speeds.

Wi-Fi-compatible devices can connect to a network or the internet via a WLAN and an AP. Coverage can be as small as a single room, or as large as many square kilometers achieved by using multiple overlapping APs. Wi-Fi works best in line of sight. Many construction materials and other obstacles absorb or reflect Wi-Fi, which restricts Wi-Fi's range below its 100-meter maximum distance.

Wi-Fi networks are based on the IEEE 802.11 standard and operate in the 2.4-GHz and 5-GHz spectrum, which is allocated for unlicensed industrial, scientific, and medical (ISM) usage.

#### Regulatory domains (unlicensed does not mean unregulated)

{% hint style="warning" %}
Devices in unlicensed bands do not require a spectrum license.\
You still must follow local regulations (region/regulatory domain).
{% endhint %}

| **Wi-Fi Gen** | **IEEE Std** | **Year** | **Max Data Rate (Mbit/s)** | **Freq Bands (GHz)** | **Channel Widths Supported** |
| ------------- | ------------ | -------- | -------------------------- | -------------------- | ---------------------------- |
| Wi-Fi         | 802.11       | 1997     | 1–2                        | 2.4                  | 20 MHz                       |
| Wi-Fi 1       | 802.11b      | 1999     | 1–11                       | 2.4                  | 20 MHz                       |
| Wi-Fi 2       | 802.11a      | 1999     | 6–54                       | 5                    | 20 MHz                       |
| Wi-Fi 3       | 802.11g      | 2003     | Up to 54                   | 2.4                  | 20 MHz                       |
| Wi-Fi 4       | 802.11n      | 2009     | 6.5–600                    | 2.4, 5               | 20, 40 MHz                   |
| Wi-Fi 5       | 802.11ac     | 2013     | 6.5–6933                   | 5                    | 20, 40, 80, 160 MHz          |
| Wi-Fi 6       | 802.11ax     | 2021     | 0.4–9608                   | 2.4, 5               | 20, 40, 80, 160 MHz          |
| Wi-Fi 6E      | 802.11ax     | 2021     | Same as Wi-Fi 6            | 2.4, 5, 6            | 20, 40, 80, 160 MHz          |
| Wi-Fi 7       | 802.11be     | 2024     | Up to 23,059               | 2.4, 5, 6            | 20, 40, 80, 160, 320 MHz     |
| Wi-Fi 8       | 802.11bn     | TBD      | Up to 100,000 (projected)  | 2.4, 5, 6            | 20–320 MHz (likely + more)   |

Wi-Fi-compatible devices can connect to a network or the internet via a WLAN and an AP. Coverage can be as small as a single room, or as large as many square kilometers achieved by using multiple overlapping APs. Wi-Fi works best in line of sight. Many construction materials and other obstacles absorb or reflect Wi-Fi, which restricts Wi-Fi's range below its 100-meter maximum distance.

Each amendment is backward compatible with the other amendments that operate at the same frequency. This compatibility enables you to, for example, replace the APs but still keep older client devices.

The following are the available channels for Wi-Fi usage:

The 2.4-GHz ISM band ranges from 2.4 to 2.4835 GHz or 2.497 GHz in Japan (11 available channels in the U.S., 13 in Europe, 14 in Japan).

The 5-GHz Unlicensed National Information Infrastructure (UNII) band is subdivided into four ranges:

UNII-1 ranges from 5.15 to 5.25 GHz

UNII-2 ranges from 5.25 to 5.35 GHz

UNII-2 extended ranges from 5.470 to 5.725 GHz

UNII-3 ranges from 5.725 to 5.825 GHz

The 5-GHz also has an ISM band that ranges from 5.725 to 5.875 GHz (overlaps with UNII-3 band).

Each AP operates in one channel. The goal is that neighboring APs do not use the same channel, so you need multiple non-overlapping channels. Using overlapping channels could lead to:

Co-channel interference: APs use the same channel.

Adjacent channel interference: APs use channels that are too close to each other (for example, channel 1 and 3)

The difference between co-channel and adjacent channel interference is that co-channel interference just slows down the wireless operation, while adjacent channel interference leads to collisions and, therefore, disrupts wireless operation.

### Wi‑Fi bands and channels

Wi-Fi band refers to a specific range of radio frequencies used for wireless communication between devices and access points (routers). Each band has unique characteristics in terms of speed, range, interference, and device compatibility.

They are divided into Channels. Each channel is known by a channel number and is assigned to a specific frequency, so channels won't overlap and interfere with other.

For wireless the most common frequencies used in LAN networks is unlicensed 2.4GHz band, 5Ghz band or 6Ghz band

Unlicensed means that it can be used for wireless communication without requiring a specific license from regulatory authorities so that it is open for public use, and anyone can deploy devices and networks that operate within those frequency bands without needing to obtain permission or pay fees for spectrum usage.

![2.4 GHz vs. 5 GHz (Which One Is Better?)](<../.gitbook/assets/Unknown image (290)>)

Higher frequencies have less permeability to transmit signal through objects and can travel less distance than lower frequencies, but they can carry more data than lower frequencies. Effective range for 5GHz is smaller than for 2.4GHz. Despite 2.4GHz band is less prone to be disrupted by the physical obstacles, it is also widely used by many other non-licensed items including microwave ovens, baby monitors, RF video cameras, wireless game controller, wireless headphones, Bluetooth, etc, making it more suspectible to interference

Most of these devices are high-powered, and they do not send IEEE 802.11 frames but can still cause interference for Wi-Fi networks.

For example, RF video cameras operate by exchanging information (the image stream) between a transmitter (the camera) and the receiver (linking to a video display). These cameras usually use 100 milliwatt (mW) and a channel that is narrower than Wi-Fi. The stream of information is continuous and severely affects any Wi-Fi network in the neighboring channels. These cameras and Wi-Fi are incompatible—an AP cannot natively receive and understand a camera video stream.

Microwave ovens provide a pulse form of interference in the middle of the Wi-Fi, 2.4-GHz band at a much higher power. Wi-Fi AP transmitters are measured in milliwatts, while microwave ovens use a power level of over 1000 W.

Fluorescent lights also can interact with Wi-Fi systems but not as interference. The form of the interaction is that the lamps are driven with alternating current (AC) power, so they switch on and off many times each second. When the lights are on, the gas in the tube is ionized and conductive. Because the gas is conductive, it reflects RF. When the tube is off, the gas does not reflect RF. The net effect is a potential source of interference that comes and goes many times per second.

Generally speaking, any device that uses a radio should be checked to determine whether it works in one of the Wi-Fi spectrums.

#### 2.4 GHz band

There are 11 channels available in the United States, 13 in Europe, and 14 in Japan.

But if a device uses a channel that is 22-MHz wide (11 MHz on each side of the peak channel), then this channel will encroach on the neighboring channels. As a result, there are only three nonoverlapping channels in the United States and in Europe: 1, 6, and 11. Any attempt to use channels that are closer to each other will result in interference issues. Nonoverlapping channels need to be separated by 25 MHz at center frequency or by five channel bands. In Japan, four channels (1, 6, 11, and 14)

The signal takes up more than one channel, so wireless communication is recommended to be separated to channels like:

![](<../.gitbook/assets/Unknown image (291)>)

#### 5 GHz band

is divided into several sections: four UNII bands and one ISM band. Channels in the sections are spaced at 20-MHz intervals and are considered noninterfering, however, they do have a slight overlap in frequency spectrum. Consecutive channels can be used in neighboring cell coverage, but neighboring cell channels should be separated by at least one channel when possible.

Since there are more non-overlapping channels in 5 GHz, you can use so-called "channel bonding," where you can merge two adjacent channels together and achieve wider channels (40-MHz, 80-MHz, or 160-MHz wide instead of 20 MHz), which in practice means multiplied data rates by 2, 4, or 8.

Many regulatory domains enforce different laws for each of these bands, so even though they may all be considered 5-GHz bands, operation in each set of channels may be different. Also, some of the channels might not be available in all regulatory domains (United States, Europe, Japan).

![](<../.gitbook/assets/Unknown image (292)>)

![](<../.gitbook/assets/Unknown image (293)>)

![](<../.gitbook/assets/Unknown image (294)>)

#### Spread spectrum, OFDM, and DFS

Spread Spectrum are wireless communications where the frequency is deliberately spread through a band to increase bandwidth while minimazing interference and noise

Direct-sequence Spread Spectrum (DSSS) 14 channels; 22MHz wide; non-overlapping channels (1,6,11); phase modulation more resilient than FSS

Orthogonal Frequency Division Multiplexing (OFDM) same as DSSS, both phase and amplitude are modulated with quadrature amplitude modulation (QAM) to move the most data efficiently; each channel is divided into many subcarriers (subchannels) part of most recent Wifi 6 generation

Dynamic Frequency Selection (DFS) is a function of using 5 GHz Wi-Fi frequencies that are generally reserved for radar. Because the 2.4Ghz band is free of radar, the DFS rules only apply to the 5.250 – 5.725 Ghz band

UNII-2 (5.250-5.350 GHz and 5.470-5.725 GHz) shared with radar systems. Therefore, APs operating on UNII-2 channels are required to use DFS to avoid interfering with radar signals

### MIMO, beamforming, and rate adaptation

Spatial multiplexing to increase data throughput data are multiplexed or distributed across two or more radio chains—all operating on the same channel, but separated through spatial diversity to avoid interference

When a transmitter with a single radio chain sends an RF signal, any receivers that are present have an equal opportunity to receive and interpret the signal. This changed with n,ac and ax as they offer a method to customize the transmitted signal to prefer one receiver over others

Usually multiple signals travel over slightly different paths to reach a receiver, so they can arrive delayed and out of phase with each other. This is normally destructive, resulting in a lower SNR and a corrupted signal

With transmit beamforming (T×BF), the phase of the signal is altered as it is fed into each transmitting antenna so that the resulting signals will all arrive in phase at a specific receiver

Maximal-Ratio Combining (MRC)

When an RF signal is received on a device, it may look very little like the original transmitted signal. The signal may be degraded or distorted due to a variety of conditions. If that same signal can be transmitted over multiple antennas, as in the case of a MIMO device, then the receiving device can attempt to restore it to its original state. The receiving device can use multiple antennas and radio chains to receive the multiple transmitted copies of the signal. One copy might be better than the others, or one copy might be better for a time, and then

become worse than the others. In any event, MRC can combine the copies to produce one signal that represents the best version at any given time

The end result is a reconstructed signal with an improved SNR and receiver sensitivity

Dynamic rate shifting (DRS) dynamically negotiates selected modulation between the transmitter and receiver

Each move outward of the receiver, causes a dynamic shift to a reduced data rate, in an effort to maintain the data integrity

![](<../.gitbook/assets/Unknown image (295)>)

### Client power saving (DTIM)

802.11 has a feature within it for power savings in the client stations which allow the wireless card to go to sleep and use very little power. If the wireless card is powered down then the client station would not be able to receive necessary broadcasts and multicasts. To solve this, when a client is in power saving mode it informs the access point which buff ers the broadcast and multicast traffic. Periodic beacons sent out by the access point, typically about every 100ms.

When an access point knows a client is in power savings mode, a Delivery Traffic Indication Message (DTIM) is sent with a periodic beacon. A DTIM interval is set within the access point to establish how often the DTIM is sent. If the DTIM interval is set to 5, the device in power savings mode only has to wake up every 5th beacon, to see if it has broadcast or multicast data it needs to listen to. This means that all multicast data would be buffered for \~500ms before it is forwarded.

If no clients are in power savings mode, the multicast is forwarded through without being buffered.

### Antennas

#### Isotropic antenna

is ideal antenna (unreal in the real world). The radiation pattern describes the range of antenna's strength

Radiation pattern of Isotropic antenna

![](<../.gitbook/assets/Unknown image (296)>)

![](<../.gitbook/assets/Unknown image (297)>)

![](<../.gitbook/assets/Unknown image (298)>)

#### Gain and beamwidth

Gain of an antenna is a measure of how effectively it can focus RF energy in a certain direction (do not amplify a transmitter’s signal)

Beamwidth strongest point of radiation pattern

![](<../.gitbook/assets/Unknown image (299)>)

#### Omnidirectional antennas

antennas propagate a signal equally in all directions away from the cylinder but not along the cylinder’s length

Dipole is the most common type, it can be fixed or flexible. Has two separate wires that radiate an RF signal when an alternating current is applied across them; gain of around +2 for 2.4GHz to +5 dBi for 5GHz

![](<../.gitbook/assets/Unknown image (300)>)

Dipole

![](<../.gitbook/assets/Unknown image (301)>)

![](<../.gitbook/assets/Unknown image (302)>)

#### Directional antennas

have a higher gain than omnidirectional antennas as they focus the RF energy in one general direction; flat rectangular shape; 6-8 dBi in 2.4 GHz 7-10 dBi for 5 GHz

![](<../.gitbook/assets/Unknown image (303)>)

![](<../.gitbook/assets/Unknown image (304)>)

#### Yagi antenna

![](<../.gitbook/assets/Unknown image (305)>)

