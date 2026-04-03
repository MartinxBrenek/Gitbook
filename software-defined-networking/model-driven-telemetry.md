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

# Model Driven Telemetry

### Overview

**Telemetry** refers to automated remote collection of data and it's transmission to remote nodes, which uses these data for monitoring and analysis

**Streaming Telemetry** uses a subscription model to identify information sources and destinations, replaing the need for the periodic polling of network elements; instead a continuous stream of the requested data is being send each defined interval

When streaming telemetry is combined with YANG models as a Data Definition Language, it’s known as **Model Driven Telemetry (MDT)**

The data to be streamed is driven through subscription. Subscriptions allow applications to subscribe to updates (automatic and continuous updates) from a YANG datastore, and this subscription enables the publisher to push and, in effect, stream those updates.

### SNMP limitations

With SNMP, all requested data must be edited and sent at once - with Push based, the sending of individual data can be spread out, which reduces the load on the network and devices

.

1. Limited Data Granularity: SNMP's limited data coverage and predefined polling intervals hinder real-time monitoring and analysis of network conditions, as it may not capture comprehensive data or transient events occurring between intervals.
2. Polling Overhead: SNMP's polling mechanism, involving periodic data requests from the management system, adds network traffic and overhead, impacting performance, especially in large-scale networks with numerous devices.
3. Unreliable Transport: SNMP traps use UDP for transport. UDP is inherently unreliable. If a trap doesn't reach a data collector, the information will be lost.
4. Scalability Challenges: SNMP encounters scalability challenges as the number of managed devices grows, requiring the management system to handle connection maintenance, polling intervals, and data processing, which can strain resources and hinder efficient management of large networks.
5. Limited Event-Driven Monitoring: Due to its reliance on polling, SNMP is less adept at capturing and responding to event-driven conditions, resulting in potential delays in detecting and reporting critical network events or anomalies unless they coincide with polling intervals.
6. Lack of Flexibility: Extending or modifying the hierarchical data model of SNMP, defined by MIBs, involves complex and time-consuming updates to both the management system and network devices. This process limits flexibility in adding new metrics or adapting to evolving network requirements.

[Other SNMP limitation are listed here](https://onenote/#Cisco%20NSO\&section-id={2DC2FCEA-3B33-4FFE-8240-49F35B5E0C99}\&page-id={D049B260-48C9-45D2-BA21-1A53E2067E5B}\&object-id={0E733387-CBE7-0EC0-3D07-818F817E1D13}\&D\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Notes/SDN.one)

![](<../.gitbook/assets/Unknown image (1621)>)

### Core components (session, sensor-path, subscription)

The core components that are used in streaming model-driven telemetry data are:

**Session:** Specifies one or more destinations to collect the streamed data is based on two modes:

**Dial-in mode:** The receiver dials in to the router and dynamically subscribes to one or more sensor paths or subscriptions. The router acts as the server and the receiver is the client. The router streams telemetry data through the same session. The dial-in mode of subscriptions is dynamic. This dynamic subscription terminates when the receiver cancels the subscription or when the session terminates.

**Dial-out mode:** The router dials out to the receiver. This mode of operation is the default one. The router acts as a client and receiver acts as a server. In this mode, sensor paths and destinations are configured and bound together into one or more subscriptions. The router continually attempts to establish a session with each destination in the subscription and streams data to the receiver. The dial-out mode of subscriptions is persistent. When a session terminates, the router continually attempts to re-establish a new session with the receiver every 30 seconds.

**Sensor path:** The sensor path describes a YANG path or a subset of data definitions in a YANG model with a container. In a YANG model, the sensor path can be specified to end at any level in the container hierarchy.

**Subscription:** Binds one or more sensor-paths to destinations, and specifies the criteria to stream data. In cadence-based telemetry, data is streamed continuously at a configured frequency.

### Transport and encoding

Transport and encoding: The router streams telemetry data using a transport mechanism. The generated data is encapsulated into the desired format using encoders.

Model-driven telemetry (MDT) data is streamed through these supported transport mechanisms:

**gRPC:** used for both dial-in and dial-out modes.

**gpbkv** stands for gRPC Protocol Buffer Key-Value.

It is a way of encoding telemetry data. It is the encoding requested by the gRPC client.

gpbkv provides a more efficient and structured format for streaming telemetry compared to older methods (like self-describing-gpb)

**TCP:** used for only dial-out mode.

**UDP:** used for only dial-out mode.

**NETCONF/YANG push:** used for only dial-out mode.

**gNMI:** used for both dial-in and dial-out modes.

Google Protocol Buffer (GPB) encoding

Configuring for GPB encoding requires metadata in the form of compiled .proto files. A .proto file describes the GPB message format, which is used to stream data.

JSON encoding

![](<../.gitbook/assets/Unknown image (1622)>)

### IOS XR references

[https://www.ciscolive.com/c/dam/r/ciscolive/emea/docs/2024/pdf/LTRSP-1216.pdf](https://www.ciscolive.com/c/dam/r/ciscolive/emea/docs/2024/pdf/LTRSP-1216.pdf)

[https://xrdocs.io/programmability/blogs/Dial-in-MDT-with-TIG/](https://xrdocs.io/programmability/blogs/Dial-in-MDT-with-TIG/)

[https://xrdocs.io/programmability/blogs/Dial-out-MDT-with-TIG/](https://xrdocs.io/programmability/blogs/Dial-out-MDT-with-TIG/)

### NetFlow

**NetFlow** is a Cisco-developed protocol used for telemetry and analyzing network traffic flows. Flows are unidirectional streams of traffic containing SIP/DIP and port, Layer 3 protocol type, Type of Service (ToS), and input logical interface information.

The NetFlow technology creates an environment in which you have the tools to understand how network traffic is flowing. When you understand the network behavior, business processes improve, and an audit trail of how the network is used is available. This increased awareness reduces the vulnerability of the service provider network to outages and allows the network to operate efficiently. Improvements in network operation lower costs and encourage higher business revenues by enabling better utilization of the network infrastructure.



**Benefits**

Provides high-level diagnostics to classify and identify network anomalies.

Detects attacks because behavioral changes are obvious with NetFlow

Reports from NetFlow are like a phone bill (Who is talking to whom, over which protocols and ports, for how long, at what speed, and so on.)

NetFlow is important for service providers and enterprise customers because it helps address four key requirements:

Efficiently measuring who is using which network resources for which purpose

Accounting and charging back according to the resource utilization level

Using the measured information to do more effective network planning so that resource allocation and deployment are well-aligned with customer requirements

Using the information to better structure and customize the set of available applications and services to meet user needs and customer service requirements

NetFlow v9 records can contain the input interface number (SNMP ifIndex) that helps identify the incoming interface of a flow. This is essential for performing traceback or detailed flow analysis

#### **Components**

**NetFlow Data Capture** captures traffic statistics on ingress and egress in the Netflow cache

All packets with the same source and destination IP address, source and destination ports, protocol, interface, and class of service (CoS) are grouped into a unique flow and then packets and bytes are tallied. If a packet has one key field that is different from another packet, it is considered to belong to another flow. This methodology of fingerprinting or determining a flow is scalable because a large amount of network information is condensed into a database of NetFlow information that is called the NetFlow cache.

**NetFlow Data Export** exports the statistical data to a NetFlow collector (Cisco DNA Center or Cisco Prime Infrastructure)

**NetFlow v9 packet format:**

Export packet: Built by a device (for example, a router) with NetFlow services enabled, this type of packet is addressed to another device (for example, a NetFlow collector). This other device processes the packet (parses, aggregates, and stores information on IP flows).

Packet header: The first part of an export packet, the packet header provides basic information about the packet, such as the NetFlow version, number of records that are contained within the packet, and sequence numbering, enabling lost packets to be detected.

FlowSet: Following the packet header, an export packet contains information that must be parsed and interpreted by the collector device. A FlowSet is a generic term for a collection of records that follow the packet header in an export packet. There are two different types of FlowSets: template and data. An export packet contains one or more FlowSets, and both template and data FlowSets can be mixed within the same export packet.

Template FlowSet: A template FlowSet is a collection of one or more template records that have been grouped in an export packet.

Template record: A template record is used to define the format of subsequent data records that may be received in current or future export packets. It is important to note that a template record within an export packet does not necessarily indicate the format of data records within that same packet. A collector application must cache any template records received, and then parse any data records it encounters by locating the appropriate template record within the cache.

Template ID: The template ID is a unique number that distinguishes this template record from all other template records that are produced by the same export device. A collector application that is receiving export packets from several devices should be aware that uniqueness is not guaranteed across export devices. Thus, the collector should also cache the address of the export device that produced the template ID to enforce uniqueness.

Data FlowSet: A data FlowSet is a collection of one or more data records that have been grouped in an export packet.

Data record: A data record provides information about an IP flow that exists on the device that produced an export packet. Each group of data records (each data FlowSet) references a previously transmitted template ID, which can be used to parse the data contained within the records.

#### **Netflow Versions**

NetFlow versions 2, 3, 4, and 6 were not released and are not supported. Netflow v9 supports IPv6,multicast and MPLS flows in compare to v5

The main feature of the NetFlow v9 export format is that it is template-based. A template describes a NetFlow record format and the attributes of the fields (such as type and length) within the record. The router assigns each template an ID, which is communicated to the NetFlow Collection Engine along with the template description. The template ID is used for all further communication from the router to the NetFlow Collection Engine.

The basic output of NetFlow is a flow record. In NetFlow v9, a flow record follows the same sequence of fields as specified by the template definition. The template to which NetFlow records belong is determined by adding the template ID to the group of NetFlow records.

Netflow v10 is called Internet Protocol Flow Information Export (IPFIX) and it is an IETF standard based on NetFlow Version 9 (NetFlow v9) with several extensions, the most popular of which are the Information Element identifiers.

### Flexible NetFlow

**Flexible NetFlow** provides enhanced optimization of the network infrastructure, reduces costs, and improves capacity planning and security detection beyond other flow-based technologies available today. Flexible NetFlow supports IPv6 and Network-Based Application Recognition (NBAR) 2 for IPv6. It also supports IPv6 transition techniques (IPv6 inside IPv4).

It performs Deep Packet Inspection (DPI) and supports a wide range of match criteria, including L2-L7 info and to filter ingress traffic destined to a single destination

In Flexible NetFlow, the administrator can specify what to track, resulting in fewer flows. This ability helps to scale in busy networks and use fewer resources that other features and services consume.

#### Components

Records: Flexible NetFlow records consist of key and non-key fields, defining how flow data is stored in the cache. Key fields identify unique flows, while non-key fields (e.g., TCP flags, byte counts) provide additional details.

Flow Exporters: Export flow data from monitors to remote systems, specifying destination, transport (UDP/SCTP), and export format (NetFlow/IPFIX).

Flow Monitors: Applied to interfaces to collect and analyze traffic based on flow records.

Flow Samplers: reduces the CPU overhead by limiting the number of packets that are selected for analysis

**Config**

[https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/ntw-servs/b-network-services/m\_fnf-ipv4-uni.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/ntw-servs/b-network-services/m_fnf-ipv4-uni.html)

To enable NetFlow to provide traceback information, a classification ACL must be configured to identify which type of traffic will be analyzed. NetFlow itself can capture detailed information about traffic flow, but to focus on specific types of traffic for traceback purposes, classification ACLs are necessary. These ACLs help filter and categorize traffic, facilitating a more efficient and precise analysis. While Cisco Express Forwarding (CEF) is a prerequisite for enabling NetFlow on a router, it is not an additional configuration specifically needed for providing traceback information; it is a foundational requirement for NetFlow to function in the first place.

### gNMI (gRPC Network Management Interface)

**gNMI (gRPC Network Management Interface)** is a functional subset of NETCONF - management protocol for streaming telemetry and configuration management that is developed by the OpenConfig community - providing (Open Source) RPC framework

The “gNMI” or “gRPC Network Management Interface” is an interface that uses Protocol Buffer to handle data translation (JSON to binary and vice versa) and provides RPCs to control network devices. GPB (Google Protocol Buffers) – A highly efficient, binary encoding format used for telemetry data transmission

gNMI is, like RESTCONF, a functional sub-set of the NETCONF protocol and uses HTTP/2 for transport protocol. Major differences here include improved security and support for bidirectional streaming. Primarily, it is used for telemetry data streaming but not limited only to it.

gNMI provides the mechanism to install, manipulate, delete the configuration of network devices, and to get operational data.

The content that is provided through gNMI can be modeled using YANG. As a transport protocol, Google remote procedure call (gRPC) is used.

Google remote procedure call (gRPC) is an open-source protocol led by Google, combines Remote Procedure Calls (RPC) with Protocol Buffers to enable efficient communication between systems. Protocol Buffers define the structure of data, allowing for the generation of code to create or parse byte streams representing the structured data. The binary data format employed by gRPC reduces the size of the actual data by approximately 10 times. This compressed data is transmitted over HTTP/2, which supports multiplexing, facilitating concurrent handling of multiple requests using a request/response mechanism. Additionally, gRPC can utilize TLS encryption to ensure secure communication over HTTP/2.

REST uses JSON, which is a text based encoding, which is much heavier as opposed to binary encoding.

It uses HTTP/1.1 which uses request/response model, meaning that if a server gets requests from numerous clients at once, each request is dealt with separately. Also, it doesn’t support TLS.

RPC also uses JSON/XML encoding which is heavier as it’s a text-based.

NETCONF and RESTCONF are best for configuration, while gNMI is best for real-time telemetry and automation

NetFlow shows who is talking to whom, while gNMI provides real-time device health metrics - such as state of BGP neighbor, interface statistics, route statistics etc..

gRPC Runs over HTTP/2 (originally named HTTP/2.0), is lightweight - faster than NETCONF/RESTCONF (binary encoding via gRPC) and is more secure, supports multiplexing, a request/response mechanism, and can handle many requests concurrently.

![](<../.gitbook/assets/Unknown image (931)>)

![](<../.gitbook/assets/Unknown image (932)>)

#### gRPC functions

| RPC                  | Message                                                                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| -------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Capability           | client > CapabilityRequest > target client < CapabilityResponse < target | Used by the client and target as an initial handshake to exchange capability information. In the CapabilityResponse, target will send back description of models supported data encoding supported gNMI service version and gNMI extensions.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Get                  | client > GetRequest > target client < GetResponse < target               | <p>Used to retrieve snapshots of the data on the target by the client. Requested subtree of the data tree will be serialized and transmitted by the target.</p><p>Available path formats: /yang-module:container/container /yang-module:container/container[key=value] /yang-module:/container/container[key=value] /container/container[key=value] The datatype argument may have the following values per gNMI specification: all config state operational The encoding argument may have the following values per gNMI specification: proto ascii json_ietf</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Operation            | Description                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| GetConfig            | Retrieve configuration data.                                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| MergeConfig          | Merge configuration data.                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| DeleteConfig         | Delete configuration data.                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ReplaceConfig        | Replace configuration data.                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| CommitConfig         | Commit configuration data changes.                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ConfigDiscardChanges | Discard configuration data changes.                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| CliConfig            | Merge configuration data in CLI format.                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| GetOper              | Retrieve operational data.                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ShowCmdTextOutput    | Retrieve CLI show-command output data.                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Set                  | client > SetRequest > target client > SetResponse > target               | Used by the client to modify the state of the target. All changes to the state of the target are considered as a part of a transaction. There are 3 types of operations defined in the ‘SetRequest’ message: Update: Adds new configurations (data nodes) based on the provided path-value combination. Delete: Removes an existing configuration (data node) from the router's data tree. The operation succeeds only if the specified path is correct and the data node exists. Otherwise, the entire transaction is rolled back to its initial state. Replace: Replaces the current configuration with the new path-value combination, ensuring the updated data fully replaces the existing one. For both Update and Replace operations, if the specified path does not exist, the target MUST create the corresponding data tree element and populate it with the data from the Update message, provided the path is valid according to the data tree schema. If invalid values are specified, the target MUST stop processing updates within the SetRequest method, revert the data tree to its original state, and return a SetResponse status indicating the encountered error. Update and Delete operations modify only the specified data node, leaving the rest of the configuration unchanged. Replace operation completely replaces the current configuration with the new one. Any existing data nodes not included in the new configuration will be removed—unless they have default values, in which case they will be reset to their default state |
| Subscribe            | client > SubscribeRequest > target client > SubscribeResponse > target   | Used to control subscriptions to data on the target by the client. Client can create subscription with dedicated stream to return once-off data (ONCE); periodically a set of data (POLL); or long-lived stream of data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

#### Dial-in/out and TLS (mTLS) notes

Dial-in: gNMI client connects to a network device and requests data.

Dial-out: The network device actively establishes a connection with the gNMI server and sends telemetry data.

To enable TLS with gNMIc, the ‘ems.pem’ certificate present in the ‘/misc/config/grpc’ directory on the router needs to be copied locally and then it’s location is passed as one of the parameters with CLI.

[https://xrdocs.io/programmability/blogs/OpenConfig-gNMI/](https://xrdocs.io/programmability/blogs/OpenConfig-gNMI/)

[https://github.com/cisco-ie/ios-xr-grpc-python](https://github.com/cisco-ie/ios-xr-grpc-python)

**gNOI (gRPC Network Operations Interface)** is a set of gRPC-based microservices designed for executing operational commands on network devices. Unlike protocols focused on configuration or telemetry (like gNMI), gNOI is built for operational tasks—think actions like rebooting a device, upgrading software, pinging a destination, or transferring files. It uses gRPC as its transport protocol, with messages defined in Protocol Buffers (proto3), making it efficient and vendor-neutral.

gNOI was developed under the OpenConfig initiative to standardize network operations across different vendors, allowing network operators to manage multi-vendor environments more easily

in IOS-XE the emsd (extensible manageability service daemon) process is responsible for streaming telemetry operation

### Cisco Crosswork Data Gateway

**Cisco Crosswork Data Gateway** is a secure, common collection platform for gathering network data from multi-vendor devices. It is an on-premise application deployed close to network devices and supports multiple data collection protocols including MDT, SNMP, CLI, gNMI, Syslog and NETCONF. The number of Crosswork Data Gateways you need depends on the number of devices supported, the amount of data being processed, the frequency at which it is collected and your network architecture.

[https://www.cisco.com/c/en/us/products/collateral/cloud-systems-management/crosswork-network-automation/datasheet-c78-743287.html](https://www.cisco.com/c/en/us/products/collateral/cloud-systems-management/crosswork-network-automation/datasheet-c78-743287.html)

[https://www.cisco.com/c/en/us/td/docs/cloud-systems-management/crosswork-infrastructure/4-4/AdminGuide/b\_CiscoCrossworkAdminGuide\_4\_4/m-crosswork-data-gateway.html](https://www.cisco.com/c/en/us/td/docs/cloud-systems-management/crosswork-infrastructure/4-4/AdminGuide/b_CiscoCrossworkAdminGuide_4_4/m-crosswork-data-gateway.html)
