# OTN

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
