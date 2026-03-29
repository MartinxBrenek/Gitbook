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

# L1   PHY

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
See also: [Packet Forwarding Architecture](../routing/packet-forwarding-architecture.md)

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

## Network Interface Module (NIM) and Line Card (LC)

Both NIM and Line Card are components which contain multiple ports that can be plugged to the modular switches or router chassis to extend the base build-in ports of the network device. Line cards are hot-swappable, which means they can be inserted or removed from the chassis without powering down the device

Linecards and NIMs can also be modular, allowing to build custom port-density, forwarding speed and power efficiency, since all the ports in the fixed linecards and NIMs must be powered, even though they are not used for active connections

Modular Port Adapter (MPA) are individual components containing various port parameters, that can be plugged into modular line card or NIM

![](<../.gitbook/assets/Unknown image (404)>)

Switch Module/Slot is the physical location where line cards are inserted

![](<../.gitbook/assets/Unknown image (405)>)

#### Stack

refers to the group of network devices interconnected, creating one entity. Each device is identified by a stack number

See: [Switch Stacking](switch-stacking.md)

stack number/module number/port number

interface gigabitethernet1/0/4 on 3750 means interface 4 on module 0 on stack switch 1.

#### Patch cord

Is a short, flexible cable used to connect two electronic or optical devices for signal routing. In networking, patch cords are commonly used to connect devices like switches, routers, and patch panels within a structured cabling system. They serve as temporary or permanent connections in telecommunications, data centers, and other networked environments

![](<../.gitbook/assets/Unknown image (406)>)
