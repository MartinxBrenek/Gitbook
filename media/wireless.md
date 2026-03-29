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

# Wireless

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
