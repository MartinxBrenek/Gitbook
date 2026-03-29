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

# E OAM

### Overview

This page covers two related Ethernet OAM toolsets:

* **Ethernet Link OAM (IEEE 802.3ah / EFM OAM)**: single-hop, link-level OAM.
* **Connectivity Fault Management (IEEE 802.1ag / CFM)**: end-to-end, per-service-instance (per VLAN/service) OAM.

**E-OAM (802.3ah)** is a protocol for installing, monitoring, and troubleshooting Metro Ethernet networks and Ethernet WANs.

It relies on an optional sublayer in the data link layer of the OSI model between LLC and MAC. You can implement E-OAM on any full-duplex point-to-point or emulated point-to-point Ethernet link. Systemwide implementation is unnecessary.

Normal link operation does not require E-OAM. **OAM frames (OAM PDUs)** use the slow protocol destination MAC address `0180.c200.0002`. The MAC sublayer intercepts them, so they do not propagate beyond a single hop.

E-OAM is a relatively slow protocol with modest bandwidth requirements. The frame transmission rate is limited to a maximum of 10 frames per second. The impact on normal operations is negligible. However, when you enable link monitoring, the CPU must poll error counters frequently. The required CPU cycles scale with the number of polled interfaces.

#### E-OAM refresher

* E-OAM contains two major components, the OAM client and the OAM sublayer.
* The OAM client establishes and manages E-OAM on a link. The OAM client also enables and configures the OAM sublayer. During the OAM discovery phase, the OAM client monitors OAM PDUs that it receives from the remote peer. It enables OAM functionality on the link that is based on the local and remote state and configuration settings. Beyond the discovery phase (at steady state), the OAM client manages the rules of response to OAM PDUs and the OAM remote loopback mode.
* The OAM sublayer presents two standard IEEE 802.3 MAC service interfaces: one faces the superior sublayers, which include the MAC client (or link aggregation), and the other interface faces the subordinate MAC control sublayer. The OAM sublayer provides a dedicated interface for passing OAM control information and OAM PDUs to and from a client.
* The OAM sublayer has three components: control block, multiplexer, and p-parser:
* The control block provides the interface between the OAM client and other blocks that are internal to the OAM sublayer. The control block incorporates the discovery process, which detects the existence and capabilities of remote OAM peers. It also includes the transmit process, which governs the transmission of OAM PDUs to the multiplexer, and a set of rules that govern the receipt of OAM PDUs from the p-parser.
* The multiplexer manages frames that generate (or relay) from the MAC client, control block, and p-parser. The multiplexer passes untouched through frames that the MAC client generates. It passes OAM PDUs that the control block generates to the subordinate sublayer; for example, the MAC sublayer. Similarly, the multiplexer passes loopback frames from the p-parser to the same subordinate sublayer when the interface is in OAM remote loopback mode.
* The p-parser classifies frames as OAM PDUs, MAC client frames, or loopback frames and then dispatches each class to the appropriate entity. OAM PDUs transmit to the control block. MAC client frames pass to the superior sublayer. Loopback frames dispatch to the multiplexer.

### Cisco E-OAM implementation

* The Cisco IOS Software implementation of E-OAM consists of the E-OAM shim and the E-OAM module:
* The E-OAM shim is a thin layer that connects the E-OAM module and the platform code. It is implemented in the platform code (driver). The shim also communicates port state and error conditions to the E-OAM module via control signals.
* The E-OAM module, which is implemented within the control plane, handles the OAM client and control-block functionality of the OAM sublayer. This module interacts with the CLI and Simple Network Management Protocol (SNMP) programmatic interface via control signals. Also, this module interacts with the E-OAM shim through OAM PDU flows.

### E-OAM features

* The OAM features as defined by IEEE 802.3ah, Ethernet in the First Mile (EFM), are discovery, link monitoring, remote fault detection, remote loopback, and vendor-specific extensions.

#### Discovery

* **Discovery** is the first phase of E-OAM, and it identifies the devices in the network and their OAM capabilities. Discovery uses information OAM PDUs. During the discovery phase, the following information advertises within periodic information OAM PDUs:
* OAM mode: Conveyed to the remote OAM entity. The mode can be either active or passive and can determine device functionality. In active mode, the device initiates the discovery process sending traffic to the slow multicast MAC address (0180.c200.0002) in the Ethernet link.
* OAM configuration (capabilities): Advertises the capabilities of the local OAM entity. With this information, a peer can determine what functions are supported and accessible; for example, loopback capability.
* OAM PDU configuration: Includes the maximum OAM PDU size for receipt and delivery. This information, along with the rate limiting of 10 frames per second, can limit the bandwidth that you allocate to OAM traffic.
* Platform identity: A combination of an organization unique identifier (OUI) and 32 bits of vendor-specific information. OUI allocation, which is controlled by the IEEE, is typically the first three bytes of a MAC address.
* Discovery includes an optional phase in which the local station can accept or reject the request for configuration from the peer OAM entity. For example, a node may require that its partner support loopback capability for acceptance into the management network. You may implement these policy decisions as vendor-specific extensions.

#### Link monitoring

* **Link monitoring** in E-OAM detects and indicates link faults under various conditions. Link monitoring uses the event notification OAM PDU and sends events to the remote OAM entity when it detects problems on the link. The error events include the following:
* Error symbol period (error symbols per second): The number of symbol errors that occur during a specified period that exceed a threshold. These errors are coding symbol errors.
* Error frame (error frames per second): The number of frame errors that it detects during a specified period that exceed a threshold.
* Error frame period (error frames per n frames): The number of frame errors within the last n frames that exceed a threshold.
* Error frame seconds summary (error seconds per m seconds): The number of error seconds (1-second intervals with at least one frame error) within the last m seconds that exceed a threshold.
* Because IEEE 802.3ah OAM provides no guaranteed delivery of any OAM PDU, the event notification OAM PDU may transmit multiple times to reduce the probability of a lost notification. A sequence number recognizes duplicate events.

#### Remote Failure Indication

* **Remote Failure Indication** provides a mechanism for an OAM entity to convey failure conditions to its peer via specific flags in the OAM PDU. The following failure conditions can disseminate:
* Link fault: The receiver detects loss of signal. For instance, the peer’s laser is malfunctioning. A link fault transmits once per second in the information OAM PDU. Link fault applies only when the physical sublayer is capable of independent transmit and receive operations.
* Dying gasp: An unrecoverable condition occurred, such as a power failure. This type of condition is vendor-specific. A notification about the condition may transmit immediately and continuously.
* Critical event: An unspecified critical event occurred that is vendor-specific. A critical event may transmit immediately and continuously.

#### Remote loopback

* **Remote loopback** allows an OAM entity to put its remote peer into loopback mode by using the loopback control OAM PDU. Loopback mode helps an administrator ensure the quality of links during installation or troubleshooting. In loopback mode, every frame that is received transmits back on the same port except for OAM PDUs and pause frames. The periodic exchange of OAM PDUs must continue during the loopback state to maintain the OAM session.
* The loopback command is acknowledged by responding with an information OAM PDU with the loopback state that is indicated in the state field. This acknowledgment allows an administrator, for example, to estimate if a network segment can satisfy a service-level agreement. Acknowledgment makes it possible to test delay, jitter, and throughput.
* When you set an interface to the remote loopback mode, the interface no longer participates in any other Layer 2 or Layer 3 protocols such as STP or Open Shortest Path First (OSPF). The reason is that when two connected ports are in a loopback session, no frames other than the OAM PDUs transmit to the CPU for software processing. The non-OAM PDU frames either loop back at the MAC level or discard at the MAC level.
* From a user’s perspective, an interface in loopback mode is in a link-up state.

#### Vendor-specific extensions

* **Vendor-specific extensions** allow vendors to extend the protocol by creating their own type-length-value (TLV) fields.

### E-OAM messages

* E-OAM messages or OAM PDUs are standard length, untagged Ethernet frames within the normal frame length bounds of 64 to 1518 bytes. Two peers negotiate the maximum OAM PDU frame size that exchanges between them during the discovery phase.
* OAM PDUs always have the destination address of slow protocols (0180.c200.0002) and an EtherType of 8809. OAM PDUs do not transmit beyond a single hop and have a hard-set maximum transmission rate of 10 OAM PDUs per second. Some OAM PDU types may transmit multiple times to increase the likelihood that a deteriorating link will successfully receive them.
* E-OAM supports four types of OAM messages:
* Information OAM PDU: A variable-length OAM PDU that you use for discovery. This OAM PDU includes local, remote, and organization-specific information.
* Event notification OAM PDU: A variable-length OAM PDU that you use for link monitoring. This type of OAM PDU may transmit multiple times to increase the chance of a successful receipt, as in the case of high-bit errors. Event notification OAM PDUs also may include a time stamp when they generate.
* Loopback control OAM PDU: An OAM PDU that is fixed at 64 bytes in length and enables or disables the remote loopback mode.
* Vendor-specific OAM PDU: A variable-length OAM PDU that allows the addition of vendor-specific extensions to OAM.

### E-OAM supported high availability features

* High availability is necessary in access and service provider networks that use Ethernet technology, especially on E-OAM components that manage EVC connectivity. End-to-end connectivity status information is critical, and you must use a hot standby Route Switch Processor (RSP) to maintain it. (A standby RSP must have the same software image as the active RSP and support synchronization of line card, protocol, and application state information between RSPs for supported features and protocols.)
* The customer edge (CE), provider edge (PE), and access aggregation PE (uPE) network nodes maintain end-to-end connectivity status that is based on information that protocols receive (such as Connectivity Fault Management \[CFM] and 802.3ah). This status information stops traffic or switches to backup paths when an EVC is down. Metro Ethernet clients (for example, CFM and 802.3ah) maintain configuration data and dynamic data, which they learn through protocols. Every transaction involves either accessing or updating data among the various databases. If the databases synchronize across active and standby modules, the RSPs are transparent to clients.
* Cisco infrastructure provides various component APIs for clients that are helpful in maintaining a hot standby RSP. Metro Ethernet high-availability clients interact with these components, update the databases, and trigger necessary events to other components. Examples include high availability In-Service Software Upgrade (ISSU), CFM high availability ISSU, and 802.3ah high availability ISSU.
* Benefits of 802.3ah high availability include the following:
* Eliminates network downtime for Cisco software image upgrades, which results in higher availability.
* Eliminates resource scheduling challenges that associate with planned outages and late-night maintenance windows.
* Accelerates deployment of new services and applications and enables faster implementation of new features, hardware, and fixes by eliminating network downtime during upgrades.
* Reduces operating costs due to outages while delivering higher service levels by eliminating network downtime during upgrades.

### Set up, configure, and verify E-OAM

* Custom Ethernet Link OAM (ELO) settings can be configured and shared on multiple interfaces by creating an ELO profile in Ethernet configuration mode and then attaching the profile to individual interfaces. The profile configuration does not take effect until the profile is attached to an interface. After an ELO profile is attached to an interface, individual Ethernet Link OAM features can be configured separately on the interface to override the profile settings when desired. Perform these steps to attach an Ethernet Link OAM (ELO) profile to an interface:
* RP/0/RP0/CPU0:Device# configure\
  RP/0/RP0/CPU0:Device(config)# interface\
  TenGigE 0/0/0/0\
  RP/0/RP0/CPU0:Device(config-if)# ethernet oam\
  RP/0/RP0/CPU0:Device(config-if-oam)# profile Profile\_1\
  RP/0/RP0/CPU0:Device(config-if-oam)# commit
* Use the no ethernet oam command to disable the E-OAM feature on the interface.

#### E-OAM link-monitoring options

* The figure illustrates some examples of the E-OAM link-monitoring commands. You can perform the E-OAM link-monitoring option parameters in any order on a chosen interface. You can perform the E-OAM link-monitoring commands on the interface or OAM profile. The following example shows how to make the configurations using a OAM profile.
* RP/0/RP0/CPU0:router(config)# ethernet oam profile Profile\_1\
  RP/0/RP0/CPU0:router(config-eoam)# link-monitor\
  RP/0/RP0/CPU0:router(config-eoam-lm)# symbol-period window 60000\
  RP/0/RP0/CPU0:router(config-eoam-lm)# symbol-period threshold ppm low 1 high 1000000\
  RP/0/RP0/CPU0:router(config-eoam-lm)# frame window 6000\
  RP/0/RP0/CPU0:router(config-eoam-lm)# frame threshold low 10000000 high 60000000\
  RP/0/RP0/CPU0:router(config-eoam-lm)# frame-period window 60000\
  RP/0/RP0/CPU0:router(config-eoam-lm)# frame-period threshold ppm low 100 high 1000000\
  RP/0/RP0/CPU0:router(config-eoam-lm)# frame-seconds window 900000\
  RP/0/RP0/CPU0:router(config-eoam-lm)# frame-seconds threshold low 3 high 900
* The following describes the purpose of each command or action:
* ethernet oam profile profile-name
* Creates a new Ethernet Link OAM (ELO) profile and enters Ethernet OAM configuration mode.
* link-monitor
* Enters the Ethernet OAM link monitor configuration mode.
* symbol-period window window
* Configures the window size (in milliseconds) for an Ethernet OAM symbol-period error event. The IEEE 802.3 standard defines the window size as a number of symbols rather than a time duration. These two formats can be converted either way by using a knowledge of the interface speed and encoding. The range is 1000 to 60000. The default value is 1000:
* symbol-period threshold low threshold high threshold symbol-period threshold { ppm \[ low threshold ] \[ high threshold ] | symbols \[ low threshold \[ thousand | million | billion ]] \[ high threshold \[ thousand | million | billion ]]}
* Configures the thresholds (in symbols) that trigger an Ethernet OAM symbol-period error event. The high threshold is optional and is configurable only in conjunction with the low threshold.The range is 1 to 1000000.The default low threshold is 1.
* frame window window
* Configures the frame window size (in milliseconds) of an OAM frame error event.The range is from 1000 to 60000.The default value is 1000.
* frame threshold low threshold high threshold
* Configures the thresholds (in symbols) that triggers an Ethernet OAM frame error event. The high threshold is optional and is configurable only in conjunction with the low threshold.The range is from 0 to 60000000.The default low threshold is 1.
* frame-period threshold low threshold high threshold frame-period threshold { ppm \[ low threshold ] \[ high threshold ] | frames \[ low threshold \[ thousand | million | billion ]] \[ high threshold \[ thousand | million | billion ]]}
* Configures the window size (in milliseconds) for an Ethernet OAM frame-period error event. The IEEE 802.3 standard defines the window size as number of frames rather than a time duration. These two formats can be converted either way by using a knowledge of the interface speed. Note that the conversion assumes that all frames are of the minimum size. The range is from 1000 to 60000.The default value is 1000.
* frame-period window window
* Configures the thresholds (in errors per million frames ) that trigger an Ethernet OAM frame-period error event. The frame period window is defined in the IEEE specification as a number of received frames, in our implementation it is x milliseconds. The high threshold is optional and is configurable only in conjunction with the low threshold. The range is from 1 to 1000000. The default low threshold is 1. To obtain the number of frames, the configured time interval is converted to a window size in frames using the interface speed. For example, for a 1Gbps interface, the IEEE defines minimum frame size as 512 bits. So, we get a maximum of approximately 1.5 million frames per second. If the window size is configured to be 8 seconds (8000ms) then this would give us a Window of 12 million frames in the specification's definition of Errored Frame Window. The thresholds for frame-period are measured in errors per million frames. Hence, if you configure a window of 8000ms (that is a window of 12 million frames) and a high threshold of 100, then the threshold would be crossed if there are 1200 errored frames in that period (that is, 100 per million for 12 million).
* frame-seconds window window
* Configures the window size (in milliseconds) for the OAM frame-seconds error event. The range is 10000 to 900000. The default value is 60000.
* frame-seconds threshold low threshold high threshold
* Configures the window size (in milliseconds) for the OAM frame-seconds error event.The range is 10000 to 900000. The default value is 60000.

#### Configure global E-OAM options

* The below figure illustrates some examples commands that you can use when configuring E-OAM interfaces or profile.
* RP/0/RP0/CPU0:router(config-eoam)# mib-retrieval\
  RP/0/RP0/CPU0:router(config-eoam)# connection timeout 30\
  RP/0/RP0/CPU0:router(config-eoam)# hello-interval 100ms\
  RP/0/RP0/CPU0:router(config-eoam)# mode passive\
  RP/0/RP0/CPU0:router(config-eoam)# require-remote mode active\
  RP/0/RP0/CPU0:router(config-eoam)# require-remote mib-retrieval\
  RP/0/RP0/CPU0:router(config-eoam)# action capabilities-conflict efd\
  RP/0/RP0/CPU0:router(config-eoam)# action critical-event error-disable-interface\
  RP/0/RP0/CPU0:router(config-eoam)# action discovery-timeout efd\
  RP/0/RP0/CPU0:router(config-eoam)# action dying-gasp error-disable-interface\
  RP/0/RP0/CPU0:router(config-eoam)# action high-threshold error-disable-interface\
  RP/0/RP0/CPU0:router(config-eoam)# action session-down efd\
  RP/0/RP0/CPU0:router(config-eoam)# action session-up disable\
  RP/0/RP0/CPU0:router(config-eoam)# action session-down efd\
  RP/0/RP0/CPU0:router(config-eoam)# uni-directional link-fault detection
* The following describes the purpose of each command or action:
* mib-retrieval
* Enables MIB retrieval in an Ethernet OAM profile or on an Ethernet OAM interface:
* connection timeout \<timeout>
* Configures the connection timeout period for an Ethernet OAM session. as a multiple of the hello interval. The range is 2 to 30. The default value is 5:
* hello-interval {100ms|1s\}}
* Configures the time interval between hello packets for an Ethernet OAM session. The default is 1 second (1s ).
* mode {active|passive\}}
* Configures the Ethernet OAM mode. The default is active:
* require-remote mode {active|passive}
* Requires that active mode or passive mode is configured on the remote end before the OAM session comes up:
* require-remote mib-retrieval
* Requires that MIB-retrieval is configured on the remote end before the OAM session comes up:
* action capabilities-conflict {disable | efd | error-disable-interface}
* Specifies the action that is taken on an interface when a capabilities-conflict event occurs. The default action is to create a syslog entry:
* action critical-event {disable | error-disable-interface}
* Specifies the action that is taken on an interface when a critical-event notification is received from the remote Ethernet OAM peer. The default action is to create a syslog entry:
* action discovery-timeout {disable | efd | error-disable-interface}
* Specifies the action that is taken on an interface when a connection timeout occurs. The default action is to create a syslog entry:
* action dying-gasp {disable | error-disable-interface}
* Specifies the action that is taken on an interface when a dying-gasp notification is received from the remote Ethernet OAM peer. The default action is to create a syslog entry:
* action high-threshold {error-disable-interface | log}
* Specifies the action that is taken on an interface when a high threshold is exceeded. The default is to take no action when a high threshold is exceeded:
* action session-down {disable | efd | error-disable-interface}
* Specifies the action that is taken on an interface when an Ethernet OAM session goes down:
* action session-up disable
* Specifies that no action is taken on an interface when an Ethernet OAM session is established. The default action is to create a syslog entry:
* action wiring-conflict {disable | efd | log}
* Specifies the action that is taken on an interface when a wiring-conflict event occurs. The default is to put the interface into error-disable state:
* uni-directional link-fault detection
* Enables detection of a local, unidirectional link fault and sends notification of that fault to an Ethernet OAM peer:

#### Verify an E-OAM session

* The following figure shows various commands you can use to verify Ethernet OAM .
* show ethernet oam configuration\
  show ethernet oam discovery\
  show ethernet oam event-log\
  show ethernet oam interfaces\
  show ethernet oam summary\
  show ethernet oam statistics
* To display the current active Ethernet OAM configuration on an interface, use the show ethernet oam configuration command. The command displays the Ethernet OAM configuration information for all interfaces, or a specified interface. The following example shows how to display the configuration for all EOAM interfaces:
* RP/0/RP0RSP0/CPU0:router# show ethernet oam configuration\
  Thu Aug 5 22:07:06.870 DST\
  GigabitEthernet0/4/0/0:\
  Hello interval: 1s\
  Link monitoring enabled: Y\
  Remote loopback enabled: N\
  Mib retrieval enabled: N\
  Uni-directional link-fault detection enabled: N\
  Configured mode: Active\
  Connection timeout: 5\
  Symbol period window: 0\
  Symbol period low threshold: 1\
  Symbol period high threshold: None\
  Frame window: 1000\
  Frame low threshold: 1\
  Frame high threshold: None\
  Frame period window: 1000\
  Frame period low threshold: 1\
  Frame period high threshold: None\
  Frame seconds window: 60000\
  Frame seconds low threshold: 1\
  Frame seconds high threshold: None\
  High threshold action: None\
  Link fault action: Log\
  Dying gasp action: Log\
  Critical event action: Log\
  Discovery timeout action: Log\
  Capabilities conflict action: Log\
  Wiring conflict action: Error-Disable\
  Session up action: Log\
  Session down action: Log\
  Remote loopback action: Log\
  Require remote mode: Ignore\
  Require remote MIB retrieval: N\
  Require remote loopback support: N\
  Require remote link monitoring: N
* To display the currently configured OAM information of Ethernet OAM sessions on interfaces, use the show ethernet oam discovery command. The command displays detailed information for Ethernet OAM sessions on all interfaces. The following example shows how to display the minimal, currently configured OAM information for Ethernet OAM sessions on all interfaces:
* RP/0/RP0RSP0/CPU0:router# show ethernet oam discovery brief
* Sat Jul 4 13:52:42.949 PST\
  Flags:\
  L - Link Monitoring support\
  M - MIB Retrieval support\
  R - Remote Loopback support\
  U - Unidirectional detection support\
  \* - data is unavailable
* Local Remote Remote\
  Interface MAC Address Vendor Mode Capability\
  \---------------------- -------------- ------ ------- ----------\
  Gi0/1/5/1 0010.94fd.2bfa 00000A Active L\
  Gi0/1/5/2 0020.95fd.3bfa 00000B Active M\
  Gi0/1/6/1 0030.96fd.6bfa 00000C Passive L R\
  Fa0/1/3/1 0080.09ff.e4a0 00000C Active L R
* To display the most recent OAM event logs per interface, use the show ethernet oam event-log command. The command displays event logs for all interfaces which have OAM configured. The following example shows how to display the event logs for all interfaces which have OAM configured:
* RP/0/RP0RSP0/CPU0:router# show ethernet oam event-log\
  Wed Jan 23 06:16:46.684 PST\
  Local Action Taken:\
  N/A - No action needed EFD - Interface brought down using EFD\
  None - No action taken Err.D - Interface error-disabled\
  Logged - System logged
* GigabitEthernet0/1/0/0\
  \================================================================================\
  Time Type Loc'n Action Threshold Breaching Value\
  \------------------------- -------------- ------ ------ --------- ---------------\
  Wed Jan 23 06:13:25 PST Symbol period Local N/A 1 4\
  Wed Jan 23 06:13:33 PST Frame Local N/A 1 6\
  Wed Jan 23 06:13:37 PST Frame period Local None 9 12\
  Wed Jan 23 06:13:45 PST Frame seconds Local N/A 1 10\
  Wed Jan 23 06:13:57 PST Dying gasp Remote Logged N/A N/A
* GigabitEthernet0/1/0/1\
  \================================================================================\
  Time Type Loc'n Action Threshold Breaching Value\
  \------------------------- -------------- ------ ------ --------- ---------------\
  Wed Jan 23 06:26:14 PST Dying gasp Remote Logged N/A N/A\
  Wed Jan 23 06:33:25 PST Symbol period Local N/A 1 4\
  Wed Jan 23 06:43:33 PST Frame period Remote N/A 9 12\
  Wed Jan 23 06:53:37 PST Critical event Remote Logged N/A N/A\
  Wed Jan 23 07:13:45 PST Link fault Remote EFD N/A N/A\
  Wed Jan 23 07:18:23 PST Dying gasp Local Logged N/A N/A
* To display the current state of Ethernet OAM interfaces, use the show ethernet oam interfaces command. The following example shows how to display the current state for all Ethernet OAM interfaces:
* RP/0/RP0RSP0/CPU0:router# show ethernet oam interfaces\
  GigabitEthernet0/0/0/0\
  In REMOTE\_OK state\
  Local MWD key: 80081234\
  Remote MWD key: 8F08ABCC\
  EFD triggered: Yes (link-fault)
* To display the local and remote Ethernet OAM statistics for interfaces, use the show ethernet oam statistics command. The following example shows how to display Ethernet OAM statistics for a specific interface:
* RP/0/RP0RSP0/CPU0:router# show ethernet oam statistics interface gigabitethernet 0/1/5/1
* GigabitEthernet0/1/5/1:\
  Counters\
  \--------\
  Information OAMPDU Tx 161177\
  Information OAMPDU Rx 151178\
  Unique Event Notification OAMPDU Tx 0\
  Unique Event Notification OAMPDU Rx 0\
  Duplicate Event Notification OAMPDU Tx 0\
  Duplicate Event Notification OAMPDU Rx 0\
  Loopback Control OAMPDU Tx 0\
  Loopback Control OAMPDU Rx 0\
  Variable Request OAMPDU Tx 0\
  Variable Request OAMPDU Rx 0\
  Variable Response OAMPDU Tx 0\
  Variable Response OAMPDU Rx 0\
  Organization Specific OAMPDU Tx 0\
  Organization Specific OAMPDU Rx 0\
  Unsupported OAMPDU Tx 45\
  Unsupported OAMPDU Rx 0\
  Frames Lost due to OAM 23\
  Fixed frames Rx 1
* Local event logs\
  \----------------\
  Errored Symbol Period records 0\
  Errored Frame records 0\
  Errored Frame Period records 0\
  Errored Frame Second records 0
* Remote event logs\
  \-----------------\
  Errored Symbol Period records 0\
  Errored Frame records 0\
  Errored Frame Period records 0\
  Errored Frame Second records 0

### Ethernet CFM (802.1ag)

* **Ethernet CFM (802.1ag)** is an end-to-end per-service-instance Ethernet layer OAM protocol. End to end can be PE to PE or CE to CE. Per service instance means per VLAN, as defined by the IEEE 802.1ag specification.
* End-to-end technology functionality distinguishes CFM from other Metro Ethernet OAM protocols. For example, MPLS, ATM, and SONET OAM help debug Ethernet wires but are not always end to end. 802.3ah OAM is a single-hop and per-physical-wire protocol. It is not end to end or service-aware. Ethernet Local Management Interface (E-LMI) is confined between the uPE and CE and relies on CFM for reporting the Metro Ethernet network status to the CE.
* Ethernet CFM provides a competitive advantage to service providers for which the operational management of link uptime and timeliness in isolating and responding to failures is crucial to daily operations. CFM includes proactive connectivity monitoring, fault verification, and fault isolation for large Ethernet metropolitan-area networks (MANs) and WANs.
* Ethernet CFM provides the following benefits:
* End-to-end service-level OAM technology.
* Reduced operating expense for service provider Ethernet networks.
* Competitive advantage for service providers.
* Supports both distribution and access network environments with the outward-facing maintenance endpoint (MEP) enhancement.

#### Ethernet CFM conceptual information

* The following information about Ethernet CFM is conceptual:
* **Customer service instance:** A customer service instance is an EVC that is identified by a service-provider VLAN (S-VLAN) within an Ethernet island and a globally unique service ID. A customer service instance can be point-to-point or multipoint-to-multipoint.
* **Maintenance domain:** A maintenance domain is a management space for managing and administering a network. A single entity owns and operates a domain and defines the set of ports that are internal to it and at its boundary.
* Ethernet CFM maintenance domain: A network administrator assigns a unique maintenance level in the range of 0 to 7 to each domain. Levels and domain names are useful for defining the hierarchical relationship that exists among domains. The hierarchical relationship of domains parallels the structure of customer, service provider, and operator. The larger the domain, the higher the level value. For example, a customer domain would be larger than an operator domain. The customer domain may have a maintenance level of 7, and the operator domain may have a maintenance level of 0. Typically, operators would have the smallest domains and customers the largest domains, with service provider domains between them in size. All levels of the hierarchy must operate together.\
  Domains should not intersect, because intersection implies management by more than one entity, which is disallowed. Domains may nest or touch, but when two domains nest, the outer domain must have a higher maintenance level than the domain that is nested within it. Nesting your maintenance domains is useful in the business model where a service provider contracts with one or more operators to provide Ethernet service to a customer. Each operator has its own maintenance domain, and the service provider defines its domain as a superset of the operator domains. Furthermore, the customer has its own end-to-end domain, which is in turn a superset of the service provider domain. The administering organizations should communicate about maintenance levels of various nesting domains. For example, one approach would be for the service provider to assign maintenance levels to operators.\
  CFM exchanges messages and performs operations on a per-domain basis. For example, running CFM at the operator level prevents discovery of the network by the higher provider and customer levels.\
  Network designers choose domains and configurations.
* **Maintenance points:** A maintenance point demarcates an interface that participates in a CFM. Maintenance points drop all lower-level frames and forward all higher-level frames. The two types of maintenance points include the following:
* **MEPs:** Points at the edge of the domain that define the boundary and confine CFM messages within the boundary. MEPs are inward-facing by default. Inward-facing means that they communicate through the relay function side, not the wire side (connected to the port), whereas MEPs that you can configure as outward-facing communicate through the wire side and not through the relay function side.
* An inward-facing MEP sends and receives CFM frames through the relay function. It drops all CFM frames at its level or lower that come from the wire side. For CFM frames from the relay side, it processes the frames at its level and drops frames at a lower level. The MEP transparently forwards all CFM frames at a higher level, whether they are received from the relay or wire side. CFM runs at the provider maintenance level (user PE to user PE), specifically with inward-facing MEPs at the UNI.
* An outward-facing MEP sends and receives CFM frames on the wire side. It drops all CFM frames at its level or lower that come from the relay function side. For CFM frames from the wire side, it processes the frames at its level and drops frames at a lower level. OFM transparently forwards all CFM frames at a higher level, whether they are received from the relay or wire side.
* **Maintenance intermediate points (MIPs)** are inside a domain, not at the boundary, and respond to CFM only when traceroute and loopback messages trigger them. They forward CFM frames that MEPs and other MIPs receive, drop all CFM frames at a lower level, and forward all CFM frames at a higher level, whether they are received from the relay or wire side.

#### Ethernet CFM messages

* CFM uses standard Ethernet frames that are distinguished by EtherType or (for multicast messages) by MAC address. All CFM messages are confined to a maintenance domain and to an S-VLAN. Bridges forward CFM messages that they cannot interpret as normal data frames. All CFM messages are confined to a maintenance domain and to an S-VLAN (PE-VLAN or Provider-VLAN).
* For the continuity check, the fault-detection multicast heartbeat is where MEPs discover other MEPs and MIPs discover MEPs in the domain.
* The loopback (ping) message is for fault-verification unicast from MEP to MEP or MIP is similar to Internet Control Message Protocol (ICMP) and occurs per EVC MAC ping. The administrator requests it.
*

```
![](/files/G2qbe3vG8KSSzKgQBErT)
```

* The show ethernet cfm peer meps command displays information about the peer Maintenance Entity Group End Points (MEPs) in Ethernet Connectivity Fault Management (CFM) sessions. The command shows details about the peer MEPs, such as the Maintenance Domain (MD), Maintenance Association (MA), and the MEP ID, and other relevant information.
* The command can be used for the following:
* Obtain an overview of the peer MEPs configured in the network, which is helpful for recognizing the CFM topology and checking the connectivity between the devices.
* Verify the proper operation of CFM sessions in the network by ensuring that the expected MEPs are discovered and operational.
* Monitor the health of the Ethernet network, identify potential issues, and diagnose problems related to CFM sessions and connectivity.
* Troubleshoot connectivity problems between network devices by inspecting the information about the peer MEPs, their status, and the configuration of MDs and MAs.
* RP/0/RSP0/CPU0:router# show ethernet cfm peer meps\
  Flags:\
  \> - Ok I - Wrong interval\
  R - Remote Defect received V - Wrong level\
  L - Loop (our MAC received) T - Timed out\
  C - Config (our ID received) M - Missing (cross-check)\
  X - Cross-connect (wrong MAID) U - Unexpected (cross-check)\
  \* - Multiple errors received S - Standby
* Domain irf\_evpn\_up (level 3), Service up\_mep\_evpn\_1\
  Up MEP on TenGigE0/3/0/9/1.1 MEP-ID 3001\
  \===============================================================================\
  St ID MAC Address Port Up/Downtime CcmRcvd SeqErr RDI Error\
  \-- ----- -------------- ------- ----------- --------- ------ ----- -----\
  \> 1 008a.964b.6410 Up 00:09:59 12 0 0 0\
  \===============================================================================
*
* Three types of CFM messages are supported:
* **Continuity Check (Auto and On-demand):** In this fault detection mechanism, multicast heartbeat messages exchange periodically between MEPs that allow MEPs to discover other MEPs within a domain and allow MIPs to discover MEPs. Continuity check messages are confined to a domain or VLAN. The continuity check can detect the following faults:
* Unintended connectivity or service leaks
* Unexpected sites
* Loss of connectivity to a site
* Link connectivity failure
* Device failure (soft and hard)
* Forwarding plane loops
* CFM configuration errors
* **Loopback (ping):** In this fault-verification mechanism, a MEP transmits unicast frames at administrator request to verify connectivity to a particular maintenance point, which indicates whether a destination is reachable. A loopback message is similar to an ICMP ping message. The loopback mechanism contains the following capabilities:
* Per EVC MAC ping (source to single destination).
* Verify bidirectional connectivity between two CFM maintenance points (for varied frame sizes).
* **Traceroute:** This fault-isolation mechanism uses next-hop multicast from MEP to next MEP or MIP along the route. The receiver both replies with a unicast to the original MEP and sends the traceroute to the next MEP or MIP. The link trace mechanism contains the following capabilities:
* Engage per EVC MAC traceroute.
* Discover MIPs on the path from source endpoint to destination endpoint.
* Report ingress action, relay action, and egress action hop by hop.
* Report encountered ACLs or STP-blocked ports.
*
