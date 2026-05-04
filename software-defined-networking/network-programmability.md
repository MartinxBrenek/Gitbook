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

# Network Programmability

Network programmability is a method of remote configuration where software is used to interact with and configure network devices, avoiding the need for manual CLI configuration. This approach streamlines network management, enabling automation, centralized control, and dynamic adjustments to meet evolving requirements.

It also allows for more efficient management of networks, and enables dynamic and adaptive network behavior in response to changing conditions and requirements.

Network programmability typically involves using software development tools and languages, such as Python, to write scripts and programs that can interact with network infrastructure.

Open Source software is part of a community-driven trend to develop and promote open standards development particularly in the networking industry.

Traditional network management methods like SNMP and SSH/Telnet - CLI are inadequate for modern networks due to their manual nature, lack of scalability, and limitations in automation. The evolution of network management is shifting towards programmatic approaches using open APIs, standardized data models, and open transport protocols to address these shortcomings. This new framework offers increased agility and scalability compared to legacy methods

Model-driven programmability is an approach to automate device programming using standardized data model languages like YANG, enabling consistent configuration, monitoring, and operation across devices through protocols like NETCONF or gRPC.

On-box programming/automation refers to scripting mechanisms such as the Tool Command Language (Tcl) or Embedded Event Manager (EEM) which are both pre-built into the NOS of various Cisco platforms. Several platforms expose a native Linux interface and offer access to a Python execution engine used to extend on-box programmability. On-box mechanisms are normally specific to the platform itself.

Off-box programming/automation refers to scripting mechanisms that exist outside a network device. It can be in the form of an external controller or some external server that often communicates to the network device using robust and modern APIs. Examples of these APIs include NETCONF, REST, and RESTCONF.

![](<../.gitbook/assets/Unknown image (997)>)

#### Cisco IOS XE programmability references

[https://www.ciscolive.com/c/dam/r/ciscolive/global-event/docs/2024/pdf/BRKDEV-2017.pdf](https://www.ciscolive.com/c/dam/r/ciscolive/global-event/docs/2024/pdf/BRKDEV-2017.pdf)

[https://www.cisco.com/c/en/us/products/collateral/switches/catalyst-9300-series-switches/catalyst-programmability-automation-wp.html#Programmabilityandautomationoverview](https://www.cisco.com/c/en/us/products/collateral/switches/catalyst-9300-series-switches/catalyst-programmability-automation-wp.html#Programmabilityandautomationoverview)

### DevOps basics

**DevOps** is an IT development approach that emphasizes collaboration, communication, and integration between software development and IT operations teams. The goal of DevOps is to deliver high-quality software quickly and efficiently, while also ensuring that it meets the needs of end-users and is reliable and scalable.

**Continuous Integration (CI)** is the practice of frequently integrating code changes into a shared repository and validating those changes through automated testing. By doing so, developers can quickly identify and address issues before they become bigger problems.

**Continuous Delivery (CD)** is the practice of automating the software release process to ensure that changes to the software can be delivered to end-users quickly and reliably. This includes automated testing, building, and deployment processes designed to minimize the risk of errors and downtime.

![](<../.gitbook/assets/Unknown image (998)>)

![](<../.gitbook/assets/Unknown image (999)>)

### APIs used for network programmability

**An API** is software that acts as an interface, enabling two applications to communicate with each other. These applications can reside on a server and a client, which may include network equipment, facilitating interaction and control of network devices. NETCONF, RESTCONF, and gNMI are all APIs used for network device configuration and management, but they have different architectures. An API is a set of functions and procedures that enable communication with a service

**OpenFlow:** An industry-standard API, which the Open Networking Foundation (ONF) defines. OpenFlow allows direct access to and manipulation of the forwarding plane of network devices such as switches and routers, both physical and virtual (hypervisor-based). The actual configuration of the devices is by the use of Network Configuration Protocol (NETCONF).

**NETCONF:** An IETF standardized network management protocol. It provides mechanisms to install, manipulate, and delete the configuration of network devices via Remote Procedure Call (RPC) mechanisms. The messages are encoded by using XML. Not all devices support NETCONF—the ones that do support it advertise their capabilities via the API.

**RESTCONF:** In the simplest terms, RESTCONF adds a REST API to NETCONF.

**OpFlex:** An open-standard protocol that provides a distributed control system that is based on a declarative policy information model. The big difference between OpFlex and OpenFlow lies with their respective SDN models. OpenFlow uses an imperative SDN model, where a centralized controller sends detailed and complex instructions to the control plane of the network elements to implement a new application policy. In contrast, OpFlex uses a declarative SDN model. The controller, which, in this case, is called by its marketing name, Cisco Application Policy Infrastructure Controller (APIC), sends a more abstract policy to the network elements. The controller trusts the network elements to implement the required changes using their own control planes.

**REST:** The software architectural style of the world wide web. REST APIs allow controllers to monitor and manage infrastructure through the HTTP and HTTPS protocols, with the same HTTP verbs (GET, POST, PUT, DELETE, and so on) that web browsers use to retrieve webpages.

**SNMP:** SNMP is used to communicate management information between the network management stations and the agents in the network elements.

**Vendor-specific protocols:** Many vendors use their own proprietary solutions, which provide REST API to a device, for example, Cisco uses NX-API for the Cisco Nexus family of data center switches.

In recent years, NETCONF is becoming a dominant protocol that allows you to modify the configuration of a networking device, whereas OpenFlow is a protocol that allows you to modify its forwarding table. If you need to reconfigure a device, NETCONF is the way to go. If you want to implement a new functionality that is not easily configurable within the software that your networking device is running, you should be able to modify the forwarding plane directly by using OpenFlow, if the networking device supports OpenFlow.

This method provides increased functionality and scalability over traditional network management methods. In order to transmit information, APIs require a transport mechanism such as SSH, HTTP, and HTTPS, though there are other possible transport mechanisms as well.

Model-driven APIs support one or more transport methods including SSH, TLS, and HTTP/HTTPS

![](<../.gitbook/assets/Unknown image (1000)>)

### Data encoding / serialization

**Data encoding / serialization** is the process of converting data into a standardized format that can be stored in a file or transmitted over a network and reconstructed later by a different application

This allows the data to be communicated between applications in a way both applications understand

YANG uses these formats for data serialization. For example, data defined in a YANG model can be encoded in XML (for NETCONF) or JSON (for RESTCONF).

Model-driven APIs support the choice of encoding including XML and JSON, but also custom encodings such as Google protocol buffers

It is in stark contrast to using SSH issuing CLI commands, in which the data is sent as strings over the wire

XML and JSON are used for the data transmission and they are:

Human readable, because they are self-describing

Hierarchical, because they store values within values

Can be parsed and used by lots of programming languages

#### XML (Extensible Markup Language)

offers a way to provide structured data exchange between computer systems. While it is not as easy for humans to understand visually, it is easy for machines to parse and generate. It was originally as a markup languages (ie. HTML) used mainly to format text (font, size, color, headings, etc.)

XML may look similar to HTML, but they are in fact different. While both use tags to define objects and elements, HTML is used to display data. It is the web browser that knows how to display websites (consumes an HTML object and displays it). However, XML is used to describe data in such a way that the XML client (programming language, and so on) can consume an object that has meaning to it.

XML was designed to describe data

HTML was designed to display data

XML tags are created by the author

HTML tags are predefined in the HTML standard

They are complementary

Example

1

Loopback0

10.0.0.4

0.0.0.3

0

Root element is the parent element for all the other elements .

Prolog an optional element and must be the first element in the document, if it's defined.

Tags are case-sensitive. Elements are represented as value

The tag is different from the tag .

All elements must have a closing tag. ... or

All elements must be properly nested within each other.

White space is insignificant

Attributes are part of the XML elements in a form of name/valu.

Comments syntax is similar to that of HTML.

XML namespaces are like a "library" in that it defines the context and meaning of tags. A URI serves as a unique identifier for this "library". The hierarchy of elements and subelements themselves is defined in an XML Schema (XSD) or DTD. The namespace and schema work together to define the structure and meaning of an XML document.

Namespaces are like labels on boxes, preventing confusion when you have similar items from different sources. They work with schemas to provide a complete definition of an XML document's structure and meaning

The main characteristics of the XML namespaces:

Provide a means to mitigate element name conflicts

It is defined with the attribute xmlns:prefix="URI", prefix is used as abbreviation of the namespace in the tag.

You can have a default namespace using xmlns=url eliminating need to have attribute in each tag

Example

\<ipv4-ospf-cfg:ospf xmlns:ipv4-ospf-cfg="http://cisco.com/ns/yang/Cisco-IOS-XE-ospf">

[ipv4-ospf-cfg:id](ipv4-ospf-cfg:id)1\</ipv4-ospf-cfg:id>

[ipv4-ospf-cfg:passive-interface](ipv4-ospf-cfg:passive-interface)

[ipv4-ospf-cfg:interface](ipv4-ospf-cfg:interface)Loopback0\</ipv4-ospf-cfg:interface>

\</ipv4-ospf-cfg:passive-interface>

\</ipv4-ospf-cfg:ospf>

IOS Example

![](<../.gitbook/assets/Unknown image (1001)>)

when no data exist, you can represent a level of XML hierarchy in one of two ways:

using opening and closing tags:

using shorthand notation:

#### JSON (JavaScript Object Notation)

open standard file format and data interchange format that uses human-readable text to store and transmit data objects, commonly used by APIs. Whitespace is insignificant

Primitive data types

String is a text value, surrounded by double quotes "Hello." "five" "true" "null"

Number is a numeric value 1 100 1000

Boolean is a data type that has only two possible values: true false

Null value represents the intentional absence of any object value

![](<../.gitbook/assets/Unknown image (1002)>)

Structured data types

Object is an unordered list of key-value variable pairs surrounded by curly brackets { } Nested Objects are objects within the objects

Key is a string. Value is any valid JSON data type.

![](<../.gitbook/assets/Unknown image (1003)>)

Array is a series of values separated by commas. Surrounded by \[ ]

![](<../.gitbook/assets/Unknown image (1004)>)

![](<../.gitbook/assets/Unknown image (1005)>)

#### YAML (Yet Another Markup Language)

used by the network automation tool Ansible. Whitespace is significant. Reoresented as: key:value Starts with ---

![](<../.gitbook/assets/Unknown image (1006)>)

### Data models

Foundation of the API are data models. Data models define the syntax and semantics including constraints of working with the API.

Data models such as YANG are structured representations that define how data (configuration parameters) are organized, stored, and accessed within a system or database. They provide a blueprint for understanding the attributes abd relationships of the data

The typical CLI format is clear text, which is easy for humans to read and interpret. However, it is suboptimal for computer programs to interpret because it lacks "structure" and often uses white space and position information to indicate meaning

Not used to actually send information to devices and instead rely on protocols such as NETCONF and RESTCONF.

Device configuration can be validated against a data model to check if the changes are a valid for the device before committing the changes.

One misconception is that data models are used to send data to/from a device. But that is not the case. Instead, protocols such as NETCONF/RESTCONF send JSON and XML encoded documents that are governed by a given model.

Model-driven APIs also support multiple options for protocol with the three core protocols being NETCONF, RESTCONF, and gRPC. Remember, they are the core protocols that work with model driven APIs. REST is not explicitly listed because when REST is used in a modeled device, it becomes RESTCONF.

![](<../.gitbook/assets/Unknown image (1007)>)

## Yet Another Next Generation (YANG)

**Yet Another Next Generation (YANG)** is a data modeling language similar to SNMP MIBs

In YANG, data models are represented by definition hierarchies called schema trees. Instances of schema trees are called data trees and are encoded in XML

YANG modules are for RESTCONF/NETCONF to what MIBs are for SNMP with readability being its number one priority.

YANG data models provide a variety of options to configure, manage, and understand the operational state of the network device.

YANG (Yet Another Next Generation) je jazyk pro modelování dat, který definuje strukturu a hierarchii konfigurace síťových zařízení. Samotný YANG pouze popisuje, jak mají být data strukturována, ale neřeší jejich přenos..

Notably, there are industry standard and vendor/platform specific models:

Industry standard: These models come from various working groups. Two core groups are the IETF and the OpenConfig working group. It is their focus to create vendor and platform-independent models—they are core features and operational stats relevant across a wide variety of devices.

Cisco common: Because many features are common across Cisco devices, there are also Cisco native models and native models per Cisco operating system.

Cisco platform-specific: Also, when there are platform or hardware-specific features, there are additional models that are used to ensure even features mapped to a given platform are still model driven meaning APIs can still be used for those features.

All YANG modules are publicly available. You can see the largest collection of models now on GitHub at:

[https://github.com/YangModels/yang](https://github.com/YangModels/yang) This repository includes models from the IEEE and IETF and vendor-specific models.

[https://github.com/openconfig/public](https://github.com/openconfig/public) The OpenConfig working group has also built out a repository for all public models the OC WG has produced.

### Modules and submodules

**Modules:** The basic building blocks in YANG, similar to libraries in programming. They define data models, which can be complete or extend existing ones.

**Submodules:** Extensions of modules that help split large modules into smaller, more manageable parts. A submodule always belongs to a single module.

#### Guidelines

Modules can include submodules (using include) and reference other modules (using import).

Submodules can include other submodules of the same module but cannot be shared across different modules.

Modules and submodules share the same namespace.

Submodules make it easier to manage large models.

![](<../.gitbook/assets/Unknown image (975)>)

Each module must have the following sections:

Module name: Must be the same as the module’s filename. (Case-sensitive equality between module name and filename prevents a warning message when validating a module.)

Header information: Describes the module and gives information about the module itself:

Namespace (mandatory): Defines the Uniform Resource Identifier (URI) of XML namespace.

Prefix (mandatory): The prefix statement is used to define the prefix that is associated with the module and its namespace. The prefix statement’s argument is the prefix string that is used as a prefix to access a module.

Organization, contact (optional): Describes the module origin (such as company and author).

Description (optional): Provides a description of the module (what it is, what it does, and so on).

Revision information: Provides versioning of the module and facilitates the importing of modules based on revisions.

Imports and includes: The import and include statements are used to make definitions available to other modules and submodules.

Type definitions: In addition to built-in data types, custom types can be defined and later used in the data models.

Reusable node declarations: In addition to type definitions, a more complex set of YANG code can be defined and later reused.

Configuration and operational data declarations: The main part of the module where the actual data model is defined.

Configurational: These models are used to add new or modify existing configuration on the network device

Operational: These models are used to retrieve the operational state of a network device

RPC, action, and notification declarations: Defines NETCONF Remote Procedure Calls (RPCs) and notifications to be used within the module.

![](<../.gitbook/assets/Unknown image (976)>)

Cisco ACI data model

Each of these platforms were built using a custom object model that offers the same properties as if they were built using YANG models. For example, with ACI everything is an object. Every object as associated properties and constraints. These constraints are defined in the ACI Management Information Model as opposed to a YANG model. The most important point to note is that YANG is not the only way to model network devices

![](<../.gitbook/assets/Unknown image (977)>)

Example

rw represents configuration data

ro represents operational data/state

module: openconfig-bgp

+--rw bgp!

+--rw global

\| +--rw config

\| | +--rw as

\| | +--rw router-id?

\| +--ro state

\| | +--ro as

\| | +--ro router-id?

\| | +--ro total-paths?

\| | +--ro total-prefixes?

<... omitted ...>

### Data types

Just like programming languages have standard data types such as strings and integers, so does YANG

Built-in data types: YANG has a set of built-in types, similar to those types of many programming languages, but with some differences due to special requirements from the management domain.

Examples:

| Type           | Description         |
| -------------- | ------------------- |
| int8/16/32/64  | Integer             |
| uint8/16/32/64 | Unsigned integer    |
| decimal64      | Non-integer         |
| string         | Unicode string      |
| enumeration    | Set of alternatives |
| boolean        | True or false       |

| To reference built-in data types, use: | type uint32; |
| -------------------------------------- | ------------ |

Common data types: RFC 6991 defines additional data types (IETF YANG data types and Inet data types).

| Type                    | Description                                                                            |
| ----------------------- | -------------------------------------------------------------------------------------- |
| counter32/64            | Non-negative 32/64-bit integer that monotonically increases                            |
| zero-based-counter32/64 | A counter32/64 that has the defined initial value zero                                 |
| gauge32/64              | Non-negative integer, which may increase or decrease                                   |
| date-and-time           | ISO 8601 standard for representation of dates and times                                |
| timestamp               | TimeStamp (SNMPv2-TC)                                                                  |
| phys-address            | Colon-separated hexadecimal pairs (e.g. 1a:ba:da:ba:d0) PhysAddress (SNMPv2-TC)        |
| mac-address             | Six colon-separated hexadecimal pairs (e.g. 1a:ba:da:ba:d0:00) MAC address (SNMPv2-TC) |

| First, you need to import IETF YANG data types module: | import ietf-yang-types { prefix yang; } |
| ------------------------------------------------------ | --------------------------------------- |
| To reference IETF YANG data types, use:                | type yang:counter64;                    |

The following table lists the common data types (IETF Inet data types) defined in RFC 6991.

| Type            | Description                                             |
| --------------- | ------------------------------------------------------- |
| ip-version      | IP protocol version: 1=IPv4, 2=IPv6, 0=unknown          |
| dscp            | Differentiated Services Code Point value: 0 to 63       |
| ipv6-flow-label | 32-bit integer in the range from 0 to 1048575           |
| port-number     | 16-bit integer in the range from 0 to 65535             |
| as-number       | 32-bit integer representing 2 or 4 octet BGP AS numbers |
| ip-address      | IPv4 or IPv6 address                                    |
| ip4-address     | IPv4 address (e.g. 10.1.2.3)                            |
| ipv6-address    | IPv6 address (e.g. fd85:b310:6513:194b::1)              |
| ip-prefix       | IPv4 or IPv6 prefix                                     |
| ip4-prefix      | IPv4 prefix (e.g. 10.1.2.0/24)                          |
| ip6-prefix      | IPv6 prefix (e.g. fd85:b310:6513:194b::/64)             |
| domain-name     | DNS domain name                                         |

| First, you need to import IETF Inet data types module: | import ietf-inet-types { prefix inet; } |
| ------------------------------------------------------ | --------------------------------------- |
| To reference IETF Inet data types, use:                | type inet:ip-address;                   |

Derived data types: Derived (custom) data types can be defined as part of the data model or they may be available by importing other modules that contain additional data type definitions. For example, Cisco NSO comes with a set of additional data types that are useful in describing device or service configurations.

There is a simple rule, that restrictions of the derived type must be equal or more strict as for the base type.

In general, restrictions fall in two categories:

Numeric restrictions: range and fraction digits substatements

String restrictions: length and pattern substatements

OpenConfig Models: These models are created by a vendor-neutral forum known as 'OpenConfig', which is led by Google, Meta, Apple, Microsoft, Comcast, and more. These models serve as a common baseline for all network vendors, such as Cisco, Juniper, Arista, and others.

#### `typedef` statement

The typedef statement defines a new data type (derived data type), which can be be used inside the module or submodule. It can be imported in other modules accordingly to the import rules. The type from which you are deriving a new type is called base type. The type statement of the typedef statement defines base type for the derived type.

In the following list are four substatements to the type statement:

Range Statement: This is like setting a rule for numbers (or decimals) you allow, such as "only numbers from 1 to 10" (written as 1..10), where "min" is the smallest number and "max" is the biggest, and you can list multiple rules like "1..10 | 20..30" meaning either range is okay.

Length Statement: This is a rule for how many characters a text can have, like "a name must be 2 to 5 letters" (2..5), where "min" is the shortest and "max" is the longest, and you can allow multiple lengths like "2..5 | 10..15" for different options.

Pattern Statement: This is like a filter for text that only lets through words matching a specific shape, such as "must start with A" (using a pattern like ^A), and if you add more filters (e.g., "must end with Z"), everything must match all of them, even if the text was already filtered before.

Fraction-Digits Statement: This is a rule for decimal numbers (like 3.14) in a decimal64 type, saying how many decimal places are allowed (e.g., 2 means differences like 0.01), set between 1 and 18, so numbers are limited to something like "5 x 10^-2" (0.05).

![](<../.gitbook/assets/Unknown image (978)>)

Enumeration Built-in Type: This is like a list of specific names (e.g., "red," "blue," "green") you can choose from for a value, where you set these names with an "enum" rule under a "type" statement, and each name must be a non-empty string without extra spaces at the start or end, with no way to limit it further.

Union Built-in Type: This is like a flexible box that can hold one value from different types (e.g., a number or text), defined by listing member types in a "type" statement, where the value is checked against each type in order until it matches one, but it can’t include "empty" or "leafref" types and doesn’t inherit defaults or units from those types

In the first output is the definition of the acl-id-type data type, which is able to store the string value for all notations of the ACL. The second output demonstrates the usage of the union statement in a combination with the enum statement.

![](<../.gitbook/assets/Unknown image (979)>)

### XPath

**XPath** is an XML Path Language defined by the W3C. It uses expressions to reference or extract parts of XML documents. It also contains a number of useful functions for nodes, numbers, and strings. Although there are newer versions of XPath, YANG uses XPath version 1.0. XPath is often used with Leafrefs when referencing Leaf or Leaf-List values elsewhere in the data tree, with when and must statements to add constraints to data model definitions.

Expressions address an XML document and return a result that can be of one of the following type categories:

Node set: multiple elements extracted from the XML document

String: a single string element

Number: a single number element

Boolean: true or false

![](<../.gitbook/assets/Unknown image (980)>)

![](<../.gitbook/assets/Unknown image (981)>)

The following XML source data will be used for the example, to demonstrated the usage of the Xpath expressions.

0

10.1.1.17

255.255.255.255

0/1

10.1.2.1

255.255.255.252

0/2

10.1.2.5

255.255.255.252

<... omitted ...>

The example extracts IP addresses from all interfaces taken from the configuration of a Cisco IOS router encoded in the XML format, with the following XPath expression.

/\*/ip/address/primary/address

or

//primary/address

In the following output is a result of the XPath expression:

10.1.1.1710.1.2.110.1.2.510.1.2.910.1.2.1310.1.3.110.1.3.510.1.3.910.1.3.13

The example uses the index into the node set to extract a specific node or a range of nodes. In XPath, the first element of a node is at index 1.

//GigabitEthernet\[position()=1]//primary/address

or

//GigabitEthernet\[1]//primary/address

In the following output is a result of the XPath expression.

10.1.2.1

### YANG statements

![](<../.gitbook/assets/Unknown image (982)>)

![](<../.gitbook/assets/Unknown image (983)>)

![](<../.gitbook/assets/Unknown image (984)>)

Key: A unique label for each item in a List, like a name to find it easily.

Leaf: A single data point in a List, like an IP address, tied to a specific type. It represents the simplest atom of information—a single value of a specific type. This is similar to scalar variables in programming languages.

![](<../.gitbook/assets/Unknown image (985)>)

| Substatements | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| config        | Whether this leaf is a configurable value \_true\_or operational value \_false.\_Takes as an argument the string true or false. If config is true, the definition represents configuration. Data nodes representing configuration will be part of the reply to a request, and can be sent in a or request. If config is false, the definition represents state data. Data nodes representing state data will be part of the reply to a but not to a request, and cannot be sent in a or request. If config is not specified, the default is the same as the parent schema node’s config value. If the parent node is a case node, the value is the same as the case node’s parent choice node. If the top node does not specify a config statement, the default is true. If a node has config set to false, no node underneath it can have config set to true. |
| default       | Specifies default value for this leaf; implies that leaf is optional                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| mandatory     | Whether the leaf is mandatory \_true\_or optional _false_                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| must          | XPath constraint that will be enforced for this leaf                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| type          | The data type (and range, etc.) of this leaf                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| when          | Conditional leaf, present only if XPath expression is true                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| description   | Human-readable definition and help text for this leaf                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| reference     | Human-readable reference to some other element or spec                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| units         | Human-readable unit specification (e.g., Hz, MBps, ℉)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| status        | Whether this leaf is _current_, _deprecated_, or _obsolete_                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

Leaf-list: A simple list of individual values (e.g., multiple IP addresses) within a Container, similar to an array of scalars.

![](<../.gitbook/assets/Unknown image (986)>)

Container: A folder that groups related information together, like a record in programming.

![](<../.gitbook/assets/Unknown image (987)>)

List: A checklist inside a Container, holding multiple items like a list of interfaces.

substatements, which apply to the list and leaf-list statements:

Max-elements: Maximum number of elements in the list. If max-elements is not specified, there is no upper limit—that is, unbounded.

Min-elements: Minimum number of elements in the list. If min-elements is not specified, there is no lower limit—that is, 0.

Ordered-by: List entries are sorted by system or user. System means that elements are sorted in a natural order (numerically, alphabetically, and so on). “User means that the order in which the operator entered them is preserved. "ordered-by user" is meaningful when the order among the elements has significance—for example, a DNS server search order or firewall rules

![](<../.gitbook/assets/Unknown image (988)>)

![](<../.gitbook/assets/Unknown image (989)>)

![](<../.gitbook/assets/Unknown image (990)>)

### `grouping` statement

Groups of nodes can be assembled into reusable collections with the use of the grouping statement. A grouping defines a set of nodes that are instantiated with the uses statement.

The example illustrates the usage of a template (grouping) that is then used twice to describe the IP and TCP port that is used for two services. Both services further refine the template by adding their default TCP port values

![](<../.gitbook/assets/Unknown image (991)>)

![](<../.gitbook/assets/Unknown image (992)>)

### `leafref`

Leafref: A pointer to actual data elsewhere in the model, like a symbolic link in a Linux file system.

The leafref type is used to reference a particular leaf instance in the data tree. The path substatement selects a set of leaf instances, and the leafref value space is the set of values of these leaf instances. If the leaf with the leafref type represents configuration data, the leaf that it refers to must also represent configuration. Such a leaf puts a constraint on valid data. All leafref nodes must reference existing leaf instances or leafs with default values in use for the data to be valid. There must not be any circular chains of leafrefs. The path keyword is used to match the position in the tree, with one or more keys if referring to a leaf-list or leafs within a list (all key leafs must be referenced

Example

1. Services List

The services list contains two key leafs:

ip: An IPv4 address

port: A 16-bit unsigned integer

yang

Copy

Edit

2. App Container

The app container contains two leafrefs:

address: References an IP from /services/ip

port: References a port from the same service entry that matches the address

How the leafrefs Work

address leaf

Can only hold a value that exists in /services/ip.

This ensures the app can only reference an existing service.

port leaf

Uses path "/services\[ip=current()/../address]/port";

What this means:

current()/../address → Goes to the address leaf in app.

services\[ip=current()/../address] → Finds the matching service where the ip is the same as address.

/port → Selects the corresponding port from that service.

This ensures that the port belongs to the same service as the selected IP.

![](<../.gitbook/assets/Unknown image (993)>)

Example Use Case

{

"services": \[

{ "ip": "192.168.1.10", "port": 8080 },

{ "ip": "192.168.1.20", "port": 9090 }

],

"app": {

"address": "192.168.1.10",

"port": 8080

}

}

### Pyang tool

**pyang** is an open-source tool for the validation, transformation and code generation for the YANG data models. It is written in python and can be used for the validation and visual representation of the YANG modules

The pyang is able to perform the following useful functions:

Convert YANG to YIN

Convert YANG to an HTML file that contains a collapsible tree representing the data model. This conversion is especially useful in large and complex models where a basic editor is no longer the best choice for viewing and editing the YANG data model. In this case, it is recommended that you use a dedicated YANG IDE that supports a collapsible tree view. Alternatively, you can use pyang to convert the YANG file to an HTML file and view the collapsible data model tree using any browser that supports JavaScript.

Convert YANG to a text file where the hierarchy is illustrated, using ASCII characters.

Convert YANG to an UML diagrams for visualization purposes.

Validate YANG modules and submodules.

For the syntax validation of the yang module, you will use pyang.

cisco@linux:\~$ pyang lpbck.yang

lpbck.yang:9: error: unterminated statement definition for keyword "key", looking at u

rcisco@linux:\~$

cisco@linux:\~$ vim lpbck.yang

cisco@linux:\~$

cisco@linux:\~$ pyang lpbck.yang

lpbck.yang:1: warning: unexpected modulename "loopback" in lpbck.yang, should be lpbck

lpbck.yang:4: error: expected keyword "prefix" as child to "import"

lpbck.yang:5: warning: imported module ietf-yang-types not used

lpbck.yang:9: warning: all keys in the list are redundantly present in the unique statement

lpbck.yang:17: error: syntax error in pattern: xmlRegexpCompile() failed

lpbck.yang:22: error: the identifier "ip-address" in the unique argument does not reference an existing container

lpbck.yang:26: error: bad value "0-999" (should be range-arg)

lpbck.yang:26: error: restriction range not allowed for this base type

lpbck.yang:30: error: prefix "inet" is not defined (reported only once)

cisco@linux:\~$

cisco@linux:\~$ mv lpbck.yang loopback.yang

cisco@linux:\~$

cisco@linux:\~$ vim loopback.yang

cisco@linux:\~$

cisco@linux:\~$ pyang loopback.yang

Python most common open source programming language for networking. It is used to interact with networking devices via API calls to extract certain information or push configuration

### Cisco YANG Suite

**Cisco YANG Suite** is a graphical interface tool designed for working with YANG models, used in network automation for NETCONF, RESTCONF, and gNMI protocols. It's commonly used for configuring and managing Cisco devices.

[Getting Started with Cisco YANG Suite](https://www.youtube.com/watch?v=nnd4KqeeqIw\&t=472s\&ab_channel=0x2142-NetworkingNonsense)

## Network Configuration Protocol (NETCONF)

**Network Configuration Protocol (NETCONF)** is a next-generation network management protocol that is designed specifically for transactional-based network management and to improve upon the weaknesses of SNMP.

It is defined by the IETF, allowing to install, manipulate, and delete the configuration of network devices NETCONF and RESTCONF describe the protocols and methods for network management data transport.

NETCONF makes a distinction between configuration and operational data. The information that can be retrieved from a running system is separated into two classes—configuration data and operational data. Configuration data is the set of writable data that is required to transform a system from its initial default state into its current state. Operational data is the additional data on a system that is not configuration data, such as read-only status information and collected statistics.

NETCONF is stateful protocol establishig session over SSH on TCP port 830

YANG describes the data model used by the network systems during the transmission and internal processing

Data model interfaces (DMIs) are a set of services that facilitate the management of network elements

Application layer protocols such as, NETCONF and RESTCONF access these DMIs over a network.

Operations between remote systems are performed with Remote Procedure Call (RPC) (similar to HTTP verb/CRUD) messages in XML format to send the information between hosts using XML-encoding, in order to perform operations upon the device. Such as , and . Information and configurations are stored in datastores

### Terminology

**NETCONF Agent:** NETCONF-capable device

**NETCONF Manager:** Client Application to do configuration stuff

**Datastore:** Database/table of information of the Agent. Target of NETCONF commands.

![](<../.gitbook/assets/Unknown image (966)>)

### RPC operations

![](<../.gitbook/assets/Unknown image (967)>)

![Network Automation and the Rise of NETCONF | by karim okasha | Medium](<../.gitbook/assets/Unknown image (968)>)

RPC offers feature , when a lock is active, only the executer of lock can perform and operations, so he can run NETCONF operation undisturbedly

that is the name of the configuration datastore that is to be locked

\<rpc message-id="101"

xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">

\</rpc

Python ncclient response: m.lock(target='running')

Note

Directly interacting with NETCONF over an SSH channel like in this example is not the best plan. Manually crafting the XML and RPCs is error prone and requires more effort.

Rather, use code libraries and tools that does all the connection handling for you. In a bit, you can see how to use ncclient (netconfclient) with Python to make the this much easier

If a request is made for a data model that doesn’t exist on the Catalyst 3850 or a request is made for a leaf that is not implemented in a data model, the Server (Catalyst 3850) responds with an empty data response. This is expected behavior.

![](<../.gitbook/assets/Unknown image (969)>)

### Datastores

NETCONF utilizes multiple configuration datastores (candidate, running, and startup): This feature is one of the most unique attributes of NETCONF, though a device does not have to implement this feature to "support" the protocol. NETCONF utilizes a candidate configuration, which is simply a configuration with all proposed changes that are applied in an uncommitted state. It is the equivalent of entering CLI commands and having them not take effect right away. You would then commit all the changes as a single transaction. Once committed, you would see them in the running configuration.

NETCONF supports device transaction, which means that when you make an API call configuring multiple objects and one fails, the entire transaction fails, and you do not end up with a partial configuration. They are also configurable and platform dependent. You can still force a change to take so that it fails on error or rolls back on error, and so on

Running configuration datastore containing configuration that is applied and running upon network device

**Startup** The configuration datastore holding the configuration loaded by the device when it boots

**Candidate** A configuration datastore that can be manipulated without impacting the device’s current configuration and that can be committed to the running configuration datastore

**URL** A configuration datastore whose configuration resides within a separate location, accessed via a URL

### Communication flow

Session Establishment Each side sends a , along with its

Operation Request The client then sends its request (operation) to the server via the message

Response is then sent back to the client within

Session Close The session is then closed by the client via

![](<../.gitbook/assets/Unknown image (970)>)

![](<../.gitbook/assets/Unknown image (971)>)

Reply from agent device

![](<../.gitbook/assets/Unknown image (972)>)

Reply from manager

Note All messages to and from the NETCONF server or client must end with ]]>]]>. This way, the client and server know that the other side is done sending the message.

![](<../.gitbook/assets/Unknown image (973)>)

Close session

![](<../.gitbook/assets/Unknown image (974)>)

### NETCONF setup requirements

A device that supports the NetConf protocol, such as a network router or switch.

A client that can communicate with the device using NetConf, such as a computer running a NetConf client software.

Network connectivity between the device and the client.

Once you have these components, you can configure the device to enable the NetConf protocol and establish a connection between the device and the client.

Here are the general steps:

Configure the device to enable the NetConf protocol and specify the port number to be used for NetConf connections.

Start the NetConf client software on the client and specify the IP address and port number of the device.

Connect to the device using the NetConf client software and authenticate using a username and password.

Once the connection is established, you can use the NetConf client software to send and receive XML-encoded data to and from the device.

You can use the data to retrieve information about the device's configuration, monitor its performance, and make changes to its configuration.

### IOS XE configuration

To start working with NETCONF APIs, you must be a user with privilege level 15

The recommended best practice when modifying the device configuration through candidate datastore is:

Lock the running datastore.

Lock the candidate datastore.

Make modifications to the candidate configuration through edit-config RPCs with a target candidate.

Commit the candidate configuration to the running configuration.

Unlock candidate and running configurations.

| To start working with NETCONF APIs, you must be a user with privilege level 15 username name privilege level password password aaa new-model aaa authentication login default local aaa authorization exec default local | show netconf-yang \[datastores \| sessions \| statistics] show platform software yang-management process show netconf {counters \| session\| schema} |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Enable YANG netconf-yang netconf-yang feature candidate-datastore netconf-yang ssh port                                                                                                                                  |                                                                                                                                                      |

### IOS XR configuration

control-plane

management-plane

out-of-band

interface MgmtEth0/RSP0/CPU0/0

allow SSH

allow SNMP

allow NETCONF

!

interface MgmtEth0/RSP1/CPU0/0

allow SSH

allow SNMP

allow NETCONF

!

!

!

!

ssh server v2

ssh server netconf port 830

ssh server netconf vrf default

netconf agent tty

!

netconf-yang agent ssh

commit

exit

!

crypto key generate rsa

#### To test with python script:

import socket

host = "10.19.55.224"

port = 830

sock = socket.socket(socket.AF\_INET, socket.SOCK\_STREAM)

sock.settimeout(5) # 5 seconds timeout

try:

sock.connect((host, port))

print(f"✅ Successfully connected to {host} on port {port} (NETCONF)")

except socket.error as err:

print(f"❌ Connection failed: {err}")

finally:

sock.close()

Get config script

from ncclient import manager

with manager.connect(host="10.19.55.224", port=830, username="cisco", password="Cisco123", hostkey\_verify=False) as nc\_conn:

nc\_config = nc\_conn.get\_config(source='running').data\_xml

print (nc\_config)

## Representational State Transfer Configuration Protocol (RESTCONF)

**Representational State Transfer Configuration Protocol (RESTCONF)** is application layer, HTTPs-based NETCONF running over tcp 443.

The interface is based on standard mechanisms for accessing configuration data, state data, and data-model-specific RPC operations and events, which are defined in the YANG model.

Utilizes YANG data models to communicate with network devices and supports media types XML or JSON

Uses HTTP methods to perform CRUD operations on a target RESTCONF server that is running on managed device (HTTPs server must be enabled on the managed device)

HTTP methods are performed against URI that represents each resource that is running on a target. Only IOS XE supports RESTCONF. A privilege level 15 user is required for RESTCONF

RESTCONF sends single, independent commands whereas NETCONF establishes/maintains a session. RESTCONF = stateless, NETCONF = stateful

RESTCONF offers these utilities and tools:

Same tools that are used for native REST interfaces are used for RESTCONF:

Python requests module

Postman

Firefox RESTClient

### HTTP verbs (CRUD mapping)

![](<../.gitbook/assets/Unknown image (962)>)

Note

The PUT operation has the ability to replace entire sections of configuration that is based on what you send. It is analogous to declarative network configuration. For example, if you use the PATCH method on one static route, it will add the route. If you use the PUT method on the route, you will end up with just one route configured.

| GET [http://csr1kv/restconf/api/config/native](http://csr1kv/restconf/api/config/native)                                                         | Retrieve full running configuration as an object            |
| ------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------- |
| GET [http://csr1kv/restconf/api/config/native/interface](http://csr1kv/restconf/api/config/native/interface)                                     | Retrieve interface-specific attributes                      |
| GET [http://csr1kv/restconf/api/config/native/interface/GigabitEthernet/1](http://csr1kv/restconf/api/config/native/interface/GigabitEthernet/1) | Retrieve interface-specific attributes for GigabitEthernet1 |

### Protocol stack

![](<../.gitbook/assets/Unknown image (963)>)

A RESTCONF device determines the root of the RESTCONF API through the link element: /.well-known/host-meta resource that contains the RESTCONF attribute.

A RESTCONF device uses the RESTCONF API root resource as the initial part of the path in the request URI.

The API resource is the top-level resource located at +restconf.

### URI format and examples

All RESTCONF URIs follow this format: https://

//data/<\[YANG MODULE:]CONTAINER>/\[?]

https://

/restconf/data/ietf-interfaces:interfaces

GigabitEthernet0/0/2 - [https://10.104.50.97/restconf/data/Cisco-IOS-XE-native:native/interface/GigabitEthernet=0%2F0%2F2](https://10.104.50.97/restconf/data/Cisco-IOS-XE-native:native/interface/GigabitEthernet=0%2F0%2F2)

Name and IP - [https://10.85.116.59/restconf/data/Cisco-IOS-XE-native:native/interface?fields=GigabitEthernet/ip/address/primary;name](https://10.85.116.59/restconf/data/Cisco-IOS-XE-native:native/interface?fields=GigabitEthernet/ip/address/primary;name)

MTU - [https://10.85.116.59/restconf/data/Cisco-IOS-XE-native:native/interface/GigabitEthernet=3/mtu](https://10.85.116.59/restconf/data/Cisco-IOS-XE-native:native/interface/GigabitEthernet=3/mtu)

### Configuration (IOS XE)

Ensure that your device is running a version of Cisco IOS that supports RESTCONF.

Enable the HTTP server on the device with the command "ip http server"

Enable the RESTCONF server on the device with the command "restconf" > it starts NGINX process

**NGINX** is an internal webserver that acts as a proxy webserver. It provides (TLS)-based HTTPS

RESTCONF request sent via HTTPS is first received by the NGINXproxy web server and the request is transferred to the confd web server for further syntax check

Configure a username and password for RESTCONF with the command "username \[username] password \[password] privilege 15"

Configure the device's management interface with the command "interface \[interface name]"

Configure the management interface with an IP address and mask with the command "ip address \[IP address] \[mask]"

Configure the management interface with a default gateway with the command "ip default-gateway \[default gateway IP]"

Configure the management interface to use the configured username and password with the command "ip http authentication local"

Configure #ip http secure-server and #ip http port <> to declare what port restconf should be using

Verify the configuration with the command "show ip http server status"

Verification #show platform software yang-management process

Note IOS-XR doesn't support RESTCONF Restconf will be supported in a future release ref [https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5xx/system-security/24xx/b-system-security-cg-24xx-ncs540/configuring-aaa-services.html](https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5xx/system-security/24xx/b-system-security-cg-24xx-ncs540/configuring-aaa-services.html)

**curl** is a command-line tool for getting or sending data using URL syntax. It is commonly used to test the responsiveness of the device to the RESTCONF query

**curl.exe** is the version of cURL used on Windows. It comes pre-installed in Windows 10 and later.

If you're using Linux or macOS, you can simply use curl instead of curl.exe (the syntax remains the same)

### Test with curl

| curl.exe -k -u "alef:poc" -H "Accept: application/yang-data+json" -X GET " [https://10.19.55.143/restconf/data/](https://10.19.55.143/restconf/data/)"                                                     | will return all data       |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- |
| curl.exe -k -u "alef:poc" -H "Accept: application/yang-data+json" -X GET " [https://10.19.55.143/restconf/data/ietf-interfaces:interfaces](https://10.19.55.143/restconf/data/ietf-interfaces:interfaces)" | will return all interfaces |

-k Allows insecure SSL connections (ignores certificate validation). Useful if the device uses self-signed certificates.

-u "username:password" Provides basic authentication with a username and password (in this case, "alef:poc")

-X GET Specifies the HTTP method to use (in this case, GET). Other methods include POST, PUT, and DELETE

The --head (or -I) option also gives you basic information about a remote file without actually downloading it. As shown in the screenshot below, when you use curl with a remote file URL, it displays various headers to give you information about the remote file.

![Delete a file after successful download using curl command](<../.gitbook/assets/Unknown image (964)>)

![Use curl to view the basic information about remote files](<../.gitbook/assets/Unknown image (965)>)

### REST (RESTful APIs)

REST describes and follow set of rules defined by HTTP methods to gather, manipulate data and interact with APIs from multiple vendors

The client uses API calls (HTTP requests) to access the resources on the server (Client-Server architecture)

Must support data caching for future use, improving performance for the client and reduces the load on the server

RESTful APIs are stateless each API exchange is separate event, independent of all past exchanges between client and server, meaning, each exchange has to be authenticated each time

Although REST APIs use HTTP, which uses TCP (stateful) as its Layer 4 protocol, HTTP and REST APIs themselves aren't stateful (The functions of each layer are separate)

Each API call in a RESTful API maps to an individual URI, meaning every configuration change or poll to retrieve data a user makes has a unique URI

CRUD

refers to the operations we perform using REST APIs

Create operations are used to create new variables and set their initial values ie. create variable "ip\_address" and set the value to "10.1.1.1"

Read operations are used to retrieve the value of a variable

Update operations are used to change the value of a variable

Delete operations are used to delete variables

Crud also has additional operations such as Options

HTML (Hypertext Markup Language)

is the standard markup language for creating and structuring web pages and web applications (same as python or C# is for coding software and applications)

Hypertext Transfer Protocol (HTTP)

HTTP is an application layer protocol and is the foundation of communication for the World Wide Web. It is based on a client-server computing model, where the client (e.g., a web browser) and the server (e.g., a web server) use a request-response message format to transfer information. HTTP presumes a reliable underlying transport layer protocol, so TCP is commonly used. However, UDP can also be used in some cases.

The web browser's primary tool is the address bar. By entering a URL into an address bar, it is possible to describe to the browser where to search for the necessary item. The URL will point to a web server and a file on that web server. Consequently, the web browser contacts the web server and requests the item that the server should send back.

The data is exchanged via HTTP Requests and HTTP Responses, which are specialized data formats, used for HTTP communication. A sequence of requests and responses is called an HTTP Session and is initiated by a client by establishing a connection to the server.

An example of using a request-response cycle is web browsing. When a user is browsing the web, a browser sends an HTTP request to get the HTML document representing the page. The server responds to the request and returns the HTTP response with a response code and the content of the page, which is an HTML document. The client browser parses this document, displaying its content according to layout information and resources contained within the page (usually images and videos) and sometimes processing additional requests corresponding to execute scripts. The web browser presents all of this content to a user as a complete webpage.

HTTP uses verbs (methods) that maps to CRUD operations

Versions

HTTP/0.9 This was the first version of HTTP, and it was very simple. It only supported GET requests and didn't have headers or status codes. It was used in the early days of the web.

HTTP/1.0 This version introduced more features, including support for additional request methods (POST, HEAD, etc.), headers, and status codes

It also allowed for multiple requests on a single connection but required each request to establish a new connection.

HTTP/1.1 is one of the most widely used versions of HTTP. It introduced important features like persistent connections (keep-alive), chunked transfer encoding, and host headers for virtual hosting. This version significantly improved the efficiency of web communication.

HTTP/2 is a major revision of the HTTP protocol. It introduced features like multiplexing, header compression, and server push, all aimed at improving the speed and efficiency of web page loading. It is designed to be more efficient than HTTP/1.1 and is widely adopted for modern web applications.

HTTP/3 is the latest version of HTTP, and it's based on the QUIC (Quick UDP Internet Connections) protocol

It aims to further improve web performance and security

HTTPS protocol has improved security by adding an encryption layer and can be used when data confidentiality is required, such as in e-commerce activities.

By default, HTTP is a stateless (or connectionless) protocol, meaning it works without the receiver retaining any client information. Each request can be understood in isolation, without knowing any commands that came before it. HTTP does have some mechanisms, namely HTTP headers, to make the protocol behave as if it was stateful.

#### HTTP request/response basics

![](<../.gitbook/assets/Unknown image (1008)>)

HTTP Verb and URI (Uniform Resource Identifier) identifies the source of the accessing web resource.

The protocol to use (specifying what language a browser and a server should use for the transaction).

A name of the webserver to contact.

A path specifying a file on that webserver.

The format is: protocol://authority/hostname/path\_to\_directory

![](<../.gitbook/assets/Unknown image (1009)>)

Accept header is a way for a client (browser) to specify the media type of response content it is expecting to be received

Content-type is a way for a client to specify media type of request being sent to the server

When a URL, such as [http://www.cisco.com](http://www.cisco.com/) c/en/us/index.html is inserted into the browser's address bar, it initiates the following sequence of actions:

The client (web browser) sends an HTTP GET request to the server [www.cisco.com](http://www.cisco.com/) and asks it to get the file c/en/us/index.html.

The server receives the request.

The server processes the request, retrieves the c/en/us/index.html file, and sends it to the browser.

The server returns an HTTP response.

The client receives the response (for example, the webpage content from the file on the server) and presents it on screen in your browser window.

The web server logs the browser history by recording that it visited the site (browser's history) and the PC keeps a copy of the page in its cache. The browser cache shows traces that a user leaves behind while navigating the web.

![](<../.gitbook/assets/Unknown image (1010)>)

[HTTP Status Codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)

1xx informational the request was received, continuing process

2xx successful the request was successfully received, understood, and accepted

3xx redirection further action needs to be taken in order to complete the request

4xx client error the request contains bad syntax or cannot be fulfilled

HTTP 403 Forbidden response status code indicates that the server understands the request but refuses to authorize it

5xx server error the server failed to fulfill request

![](<../.gitbook/assets/Unknown image (1011)>)

### REST API in Cisco IOS XE (container-based)

REST API container is an application that provides a set of RESTful APIs to manage devices running Cisco IOS XE Software. It is located in a virtual services container, which is a virtualized environment running on the host device. It is also referred to as a virtual machine (VM), virtual service, or container. The REST API virtual service is not a native capability within Cisco IOS XE, but it is instead delivered as an open virtual application (OVA) package file

The Cisco REST API OVA package was bundled with the Cisco IOS XE Software on releases prior to 16.7.1. Starting with Cisco IOS XE release 16.7.1, the OVA package is not bundled with the Cisco IOS XE image, instead it needs to be downloaded from Cisco Software Center and transferred to the Cisco device on which it is to be enabled.

Regardless if bundled with Cisco IOS XE or not, the REST API service is never enabled by default on any Cisco IOS XE release. Customers interested in using the REST API capabilities have to first enable such capabilities on each device by completing the following steps:

1. Log in to the device by using an administrator-level account (with privilege level 15).
2. Install the REST-API container by using the Cisco Virtual Manager (VMAN) CLI.
3. Enter the remote-management configuration mode and configure a local TCP port that will be bind to the management interface of the REST API service.
4. Configure a management interface that will be used to process HTTP requests submitted to the REST API service.
5. Enable the REST-API virtual service container

![](<../.gitbook/assets/Unknown image (1012)>)

![](<../.gitbook/assets/Unknown image (1013)>)

The REST API authentication works as follows:

The authentication uses HTTPS as the transport for full Cisco REST API access.

Clients perform authentication with this service by invoking a POST on this resource with HTTP Basic Auth as the authentication mechanism. The response of this request includes a token-id. Token-ids are short-lived, opaque objects that represents client's successful authentication with the token service.

Clients then access other APIs by including the token id as a custom HTTP header "X-auth-token." If this token is not present or expired, then API access will return an HTTP status code of "401 Unauthorized"

Clients can also explicitly invalidate a token by performing a DELETE operation on the token resource.

The username/password for the HTTPS session should be configured with privilege 15.

The initial HTTP request is performed by clients to authenticate and obtain a token so that it can invoke other APIs. The HTTP POST response contains an opaque URL to be used for HTTP GET and DELETE requests.

The following example shows JSON messages that are transmitted during REST API authentication process:

![](<../.gitbook/assets/Unknown image (1014)>)

In subsequent API accesses, the token-id must appear as a custom HTTP header for successful invocation of APIs. X-auth-token: {token-id}.

To retrieve active tokens use the following resource Uniform Resource Identifier (URI) with the HTTP method GET: GET /api/v1/auth/token-services. To retrieve token details use GET /api/v1/auth/token-services/{opaque-token-id}.

Typically tokens automatically expire after 15 minutes. However, clients can perform explicit invalidation of a token by doing a DELETE on the token resource: DELETE /api/v1/auth/token-services/{opaque-token-id}.

#### REST API security notes

Every request made to server should require validation

Cached credentials should not be allowed

Always use HTTPS

Use hashing PBKDF2, BCrypt, and SCrypt algorithms - secure REST API from brute attacks (attempt to discover a password by systematically trying every possible combination of letters, numbers, and symbols until you discover the one correct combination that works.)

Adding Timestamp in Request (API header) - This will prevent very basic replay attacks from people who are trying to brute force your system

Least Privilege Users should only have enough privileges to do their job

Fail-Safe Defaults Actions should be denied without explicit permission

Economy of Mechanism Security design should be simple and intuitive

Open Design This principle highlights the importance of building a system in an open manner, with no secret or confidential algorithms being exposed in the code or repository.

Separation of Privilege Granting permissions to an entity should not be purely based on a single condition, a combination of conditions based on the type of resource is a better idea.

Least Common Mechanism It concerns the risk of sharing state among different components. If one can corrupt the shared state, it can then corrupt all the other components that depend on it

Psychological Acceptability Security should not make worse the user experience.

Authentication

Should be stateless; OAuth over basic authentication; Authentication and authorization should not be cached

The most common implementations of OAuth (OAuth 2.0) use one or both of these tokens:

* access token: sent like an API key, it allows the application to access a user’s data; optionally, access tokens can expire.
* refresh token: optionally part of an OAuth , refresh tokens retrieve a new access token if they have expired. OAuth2 combines Authentication and Authorization to allow more sophisticated scope and validity control

Complete Mediation

A system must validate access rights to all its resources and must not rely on a cached permission matrix. If the access level to a given resource is revoked but is not reflected in the permission matrix, the security is violated

JSON Web Token (JWT) secures transmission of information between parties (structure consists of header,payload and signature)

Session vs Token Authentication

Session are saved on server

Tokens are saved on client. This type of authentication is used the most as it removes the load and stateful operations to be performed by server

![](<../.gitbook/assets/Unknown image (1015)>)

## Zero-Touch Provisioning (ZTP)

is a process where devices can automatically receive their configurations and software from a centralized server once they are connected to the network. This eliminates the need for manual configuration and allows for quick deployment of network devices, especially in large-scale environments.

The device typically identifies itself (often through its MAC address or serial number) and connects to a pre-configured provisioning server. From there, the device downloads its operating system (if necessary) and configuration files.

Day-zero techniques automate bringing up network devices into a functional state with minimal to no-touch.

ZTP supports autoprovisioning of a router by running customized scripts using a DHCP server.

When a device that supports Zero-Touch Provisioning boots up and does not find the startup configuration (during a fresh install on day zero), the device enters the Zero-Touch Provisioning mode.

The device locates a DHCP server, bootstraps itself with its interface IP address, gateway, and Domain Name System (DNS) server IP address, and enables Guest Shell

The device then obtains the IP address or URL of a TFTP server and downloads the Python script to configure the device.

![](<../.gitbook/assets/Unknown image (1016)>)

#### ZTP security

The secure aspect of ZTP ensures that only trusted devices are provisioned and that sensitive information remains protected during the provisioning process.

Secure ZTP authenticates not only the onboarding network device but also validates the server authenticity and provisioning information that it is receiving from the ZTP server

Secure ZTP uses a three-step validation process to onboard the remote devices securely:

Router Validation: The ZTP server authenticates the router before providing bootstrapping data using the Trust Anchor Certificate (also called SUDI certificate).

Server Validation: The router device in turn validates the ZTP server to make sure that the onboarding happens to the correct network. Upon completion, the ZTP server sends the bootstrapping data (for example, a YANG data model) or artifact to the router.

Artifact Validation: The configuration validates the bootstrapping data or artifact received from the ZTP server.

XR config [https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/b-setup-and-upgrade-cisco8k/secure-ztp.html](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/b-setup-and-upgrade-cisco8k/secure-ztp.html)

Automatic provisioning interprets the rest of the file as a static configuration if the file starts with:

"!! IOS XR"

Explanation:

In Cisco IOS XR, automatic provisioning checks the first line of the configuration file.

If the file starts with "!! IOS XR", the system treats it as a static configuration file, meaning the rest of the file contains CLI-based configuration commands that are applied directly.

If the file starts with a shebang (#!), such as #!/bin/bash, #!/bin/sh, or #!/usr/bin/python, the system interprets it as a script and executes it accordingly.

## Jinja

**Jinja** is a **templating engine**—it doesn’t automate systems by itself, it just **generates text (like config files) from variables using simple logic** (loops, conditions). Tools like **Ansible** and **SaltStack** use Jinja internally to build configs before applying them to machines, while **Python** is a full programming language that can actually execute automation logic. In short: **Jinja formats data into text, Python does the logic, and Ansible/Salt run and manage the automation.**

Jinja is used to generate text files from variables and logic. Most often:

* config files
* YAML/JSON
* shell scripts
* HTML
* device configs
* Kubernetes manifests
* Terraform-like text output
* email/message templates

You write a template with placeholders and simple logic, for example:

```
hostname {{ inventory_hostname }}

{% for dns in dns_servers %}
dns-server {{ dns }}
{% endfor %}
```

***

Jinja is good at:

* variable substitution
* loops
* conditionals
* formatting output
* generating repetitive structured text

It is **not** good at:

* orchestration
* remote execution
* dependency handling
* state enforcement
* complex application logic
* system interaction by itself

So Jinja does not “do” automation alone. It helps **produce the files or commands** used by automation.

## **Git**

**Git** is a version-control system that turns every change into a small, inspectable unit of work. You work on files that represent intent. You ask for a review, and then you merge the new changes with the previous ones.

A Git version control system is most effective for network change tracking when it is used to manage intent rather than outputs. You can store YAML Ain't Markup Language (YAML) or comma-separated values (CSV) inventories, policy maps, Jinja2 templates, variables, Terraform HashiCorp Configuration Language (HCL), pyATS tests, and your CI pipeline files. Exclude secrets like passwords and keys, packet captures and images, large binaries, and Terraform state. Store those in a vault, artifact storage, a config-backup system, or a remote back-end (Terraform state) instead.

You can track your IaC sources the same way you track Cisco IOS XE Software fragments. Git versions plaintext reliably. Every change is reviewable and reversible.

* **Terraform:** Commit \*.tf, modules, variables, and terraform.lock.hcl to pin providers. Do not commit state files or the .terraform working directory.
* **Ansible:** Commit playbooks, roles, group\_vars, host\_vars, and inventory definitions. If you use Ansible Vault, version the encrypted files, never the vault password.
* **Templates and data:** Commit Jinja2 templates, YAML, or CSV inventories, JavaScript Object Notation (JSON) payloads for Representational State Transfer Configuration Protocol (RESTCONF), and small Python utilities for validation.

You can also store full device configurations in Git. This helps with audit trails and comparisons.

* Keep full device configuration snapshots separate from the network intent files. Use a dedicated **snapshots** folder or, better, a separate repository with a predictable structure like _snapshots/\<site>/\<device>/\<YYYY-MM-DD>/running-config.txt,_ to ensure that extensive diffs do not slow down the review process for network changes.
* Redact secrets or avoid committing them. Automate redaction with a pre-commit hook.
* Treat running configuration snapshots as read-only evidence. Make all network changes by editing the desired state in version-controlled IaC files, reviewed in merge requests and applied by automation. Keep device configuration or state as archived snapshots.

## Essential Git Operations for Tracking Network Changes <a href="#page-heading" id="page-heading"></a>

Here are the most common Git commands that include initializing a repository, adding files, and committing changes.

Choose each step of version control to discover these commands.

#### Initializing a Git Repository

The first step in version control is to initialize a repository. Navigate to the directory where your Python scripts are located and run:

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git init
</code></pre></td></tr></tbody></table>

This command creates a .git subfolder, making the current folder a Git repository. After initializing, Git will start tracking the files in this directory.

#### Checking the Status

After you have edited some files in your repository, you can check which files have been modified using the following command:

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git status
On branch main
Changes not staged for commit:
(use "git add &#x3C;file>..." to update what will be committed)
(use "git restore &#x3C;file>..." to discard changes in working directory)
modified:   configs/vlan_site_A.yml
modified:   configs/acl_site_A.yml
no changes added to commit (use "git add" and/or "git commit -a")
</code></pre></td></tr></tbody></table>

The section **Changes not staged for commit** tells you which files were modified and not yet staged.

If a file was already staged with the `add` command, then that file will be shown under the **Changes to be committed**.

#### Checking the Diff

Besides seeing modified files, you can also inspect the modifications. `git diff` shows unstaged changes. Lines with **`-`** will be removed, lines with `+` will be added.

In the following example, you renamed the Guest VLAN, and added VLAN 40.

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git diff
diff --git a/configs/vlan_site_A.yml b/configs/vlan_site_A.yml
index 3b2a9d1..7d8e1c4 100644
--- a/configs/vlan_site_A.yml
+++ b/configs/vlan_site_A.yml
@@ -3,6 +3,7 @@ branches:

id: site_A
vlans:

{ id: 20, name: Users }



 - { id: 30, name: Guest }




 - { id: 30, name: Guest-WiFi }


 - { id: 40, name: IoT }


</code></pre></td></tr></tbody></table>

#### Adding Files to the Repository Staging Area

After initializing the repository, you need to tell Git which files you want to track by adding them to a staging area. To add all files in the directory:

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git add .
</code></pre></td></tr></tbody></table>

This command stages all the files in the current directory for the next commit. If you only want to add specific files, you can list them individually:

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git add network_config.yml
</code></pre></td></tr></tbody></table>

#### Committing Changes

Once you have added the files, it is time to commit the changes. A commit is like taking a snapshot of your project at a particular point in time:

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git commit -m "Initial commit of network configuration intent for Ansible"
</code></pre></td></tr></tbody></table>

The **-**`m` flag allows you to add a short message describing the changes that you have made. This message helps others (and yourself) understand the purpose of the commit.

Writing a good commit message can be very important. It clearly explains the purpose and context of a change, helping others (and yourself in the future) understand what was done and why.

How to write a good commit message?

* Start with a verb, such as "Add," "Fix," "Update," or "Remove."
* Explain what changed and why.
* Keep the message short but not too vague.
* Examples for network changes: "Add VLAN 30 for guest Wi-Fi network," "Fix ACL sequence conflict on engineering subnet," "Remove deprecated sales ACL rules per security audit."

#### Viewing Commit History

To view a history of all the commits in your repository, run:

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git log
commit 7cd6c22e5ebf1d46afe0c77cabaea5b015b12cb5 (HEAD -> main)
Author: Git User &#x3C;user@example.com>
Date:   Wed Oct 29 18:52:08 2025 +0100
Fix vlan bug
commit 9853262b88d1c78059b9a38dd00077a9d4c1e12f
&#x3C;... output omitted ...>
</code></pre></td></tr></tbody></table>

This displays a list of all commits, along with their unique commit IDs, dates, and messages.

`Note Git tracks changes by recording diffs between the current state of a file and its previous version, rather than saving complete copies of the entire file each time.`

## Collaboration and Remote Repositories

When working in teams, Git allows you to store your code in remote repositories (for example, on GitHub, GitLab, or Bitbucket). These platforms, often free but with premium versions available, allow you to collaborate with others, share your code or network configuration files, and synchronize changes.

Once you configure a remote repository URL with `git remote add`, you can use `git push` and `git pull` commands, as shown in the following figure.

You can use the following commands to manage remote and local repositories:

*   If you started a local repository with `git init,` you must add a remote URL yourself. First, create a new remote repository on GitLab (or another platform). Once your remote repository is created, you can link your local repository to it by running:

    ```
    git remote add origin https://gitlab.com/yourusername/your-repo.git
    ```

    Replace _yourusername_ and _your-repo_ with your actual GitLab username and repository name, and make sure that you are using the required authentication.
*   If a remote repository exists and you do not have a local copy, run the following command:

    ```
    git clone https://gitlab.example.com/yourusername/your-repo.git
    ```

    Replace _yourusername_ and _your-repo_ with your project. You can use the SSH form too, git@gitlab.example.com:yourusername/your-repo.git. This creates a local folder, sets the remote name to _origin_, and checks out the default branch, usually _main_. Then cd _your-repo_ and start working.

    If you need to switch from HTTPS to SSH, update the URL.

    ```
    git remote set-url origin git@gitlab.example.com:yourusername/your-repo.git
    ```
*   After committing your changes locally, you can push them to the remote repository:

    ```
    git push -u origin main
    ```

    The _main_ in this command stands for the branch name. If you are working with a different branch, you need to modify this part of the command.
*   If someone else has changed the repository, or you want to work from a different workstation, you can pull the remote changes to your local environment by running:

    ```
    git pull origin main
    ```

    This command fetches changes from the remote repository and merges them with your local version. If you want to only fetch changes without merging them to your local version, use `git fetch`.

{% hint style="info" %}
Why are we always using **origin** as the name of the remote repository when running Git commands?

Because `git clone` automatically creates a remote repository named **origin** for the URL you cloned. **origin** is just a label, it makes commands like `git push origin main` short and consistent. Most teams adhere to this convention to maintain simplicity in documentation, scripts, and CI processes.

You can change it or add more remotes

```
# rename if you prefer a different label
git remote rename origin gitlab

# fork flow, keep origin = your fork, add the source repo as 'upstream'
git remote add upstream git@git.example.com:net/infra-upstream.git
```
{% endhint %}

## Use Git Branches

A branch provides an isolated environment for managing a single change. You isolate the work, write small commits, open a review, and merge when approved. The _main_ branch stays clean and production ready. You can maintain many branches at the same time. Each new branch should address a single objective to ensure reviews remain concentrated and rollbacks are straightforward.

Name branches to make the purpose obvious. Include the scope, the device group or site, and an issue ID if you have one. Examples are _feature/guest-vlan-b01-b02, fix/qos-voice-b01, policy/acl-engineering-101, chore/inventory-cleanup-TEAMS-123._ Avoid spaces. Use short, descriptive names.<br>

For example, if you are working on changing the configuration file for VLANs, you can create a branch to make the changes, without affecting the main project.

*   Make sure you start from the branch that already contains the baseline you want to work from, usually the _main_.

    ```
    git branch

    * feature/guest-vlan-b01-b02
      main
      staging
      production
    ```

    If you intended to be on the main branch but are not currently on it, switch using the `git switch main` or `git checkout main` commands.
*   Once you are on the correct branch, sync to the latest changes. Others may have updated the main branch, so pull first to avoid merge conflicts later.

    ```
    # Fast-forward only, safest default

    git pull --ff-only
    ```

{% hint style="info" %}
The `--ff-only` flag makes sure that Git will update your branch only if it can move the branch pointer ahead without creating a merge commit. If your branch has diverged, Git stops and asks you to resolve it with a merge or a rebase. This keeps the commit history clean and predictable. In practice this happens when you made local commits on main, or you have uncommitted changes on main.
{% endhint %}

*   To create a new branch, use the following command:

    ```
    git checkout -b vlan-changes
    ```

    Alternately, you can use the following:

    ```
    git switch -c vlan-changes
    ```

    This creates and switches to a new branch named _vlan-changes_. Now, you can change the configuration files in this branch without affecting the source branch.

    You can create as many branches as needed to work on new features without affecting the _main_ branch. When you create a new branch, it starts from the branch you were currently on, inheriting its code and commits history.
*   Once you have completed your changes in the branch, you can merge it back into the _main_ branch (or any other branch). First, switch back to the main branch:

    ```
    git checkout main
    ```

    Then, merge the changes from your feature branch:

    ```
    git merge vlan-changes
    ```

    This integrates the changes from the _vlan-changes_ branch into the primary branch, typically named _main_.

Sometimes a merge conflict can occur. This happens when Git cannot combine changes automatically. For example, both branches edited the same lines, one branch deleted a file the other modified, or there are overlapping renames, and Git is unable to automatically determine which file version to retain.

For example, you try to merge _feature/acl-fix_ into the main branch, but it fails because new commits on _main_ changed the same lines in the same files as your _branch_.

```
git switch main
git pull
git merge feature/acl-fix

Auto-merging policies/acl_guest_iosxe.cfg
CONFLICT (content): Merge conflict in policies/acl_guest_iosxe.cfg
Automatic merge failed, fix conflicts and then commit the result.
```

{% hint style="info" %}
So what do those markers in a merge conflict mean?

* `<<<<<<< HEAD` marks the start of your version, representing the content from the currently checked-out branch. In this example, _main._
* `=======` separates the two conflicting versions.
* `>>>>>>> feature/acl-fix` ends with the other side, the incoming branch name that you want to merge from, and its content.
{% endhint %}

Pick the correct configuration lines, delete the unwanted configuration lines, including the markers, save, then `git add` and `git merge --continue` until there are no merge conflicts remaining.

If you decided that the changes coming from the **feature/acl-fix** branch should be merged, then keep those, and delete the lines that are currently in the main branch. Here's the modified file, ready to be merged.

```
# policies/acl_guest_iosxe.cfg
ip access-list extended GUEST_INTERNET
 10 permit tcp any any eq www
 20 deny tcp any any eq 443
 30 permit icmp any any echo-reply
```

You can continue the merge by staging the changed file again, and running the **merge** command with the **--continue** flag.

```
git add policies/acl_guest_iosxe.cfg
git merge --continue
```

{% hint style="info" %}
When you merge two branches, for example **main** and **feature**, each branch has its own list of commits, that list is its _history_. A regular merge makes a new commit that combines the changes from both lists, this new commit is placed at the end of the branch, the tip, which Git also calls **HEAD**. If Git can fast forward, meaning one branch is simply behind the other with no separate commits of its own, it does not create a new commit, it just moves the branch pointer so the tip, HEAD, points to the latest commit. In short, regular merge creates a new merge commit, fast forward just advances the pointer.

HEAD is your current spot in history, usually the latest commit on the branch you have checked out. On main, HEAD points to the latest on main. Switch branches and HEAD moves. Make a new commit and HEAD advances.

You are not restricted to CLI usage for Git operations. Visual tools make it easy to stage by hunk, view diffs, resolve conflicts, browse history, and open reviews. Popular choices include VS Code Source Control and the GitLens extension, JetBrains IDEs, GitHub Desktop, GitKraken, and Sourcetree.
{% endhint %}

Work in short cycles. Start from the latest _main_ branch, create a new branch, make a small change, push, and open a review. Delete the branch after merge. If you need to work on two unrelated changes at once, create two branches.

## Advanced Git Techniques for Efficient Change Control <a href="#page-heading" id="page-heading"></a>

* **Cherry-pick:** Copy an exact commit to another branch when only one fix needs to move forward.
* **Revert:** Undo an incorrect commit on a shared branch while keeping the history.
* **Reset:** Rewrite your local branch before pushing changes to remove unwanted commits or start fresh.
* **Restore:** Discard or recover a specific file from a known version.

The goal is a clean historical record for reviewers, safe network rollouts, and fast recovery when something goes wrong.

### Git Cherry-pick

On a feature branch, you often make several focused commits. Usually, you merge these custom branch changes into the _main_. If you need only one of those changes on another branch, use `git cherry-pick` to copy that exact commit without including the others.

Cherry-pick copies one specific commit onto your current branch. This is perfect for hotfixes that must appear in staging and production without merging unrelated work.

```
# On staging branch, bring in the exact fix from its commit on another branch
git switch staging
git cherry-pick <fix-commit-sha>

# On production, repeat after staging is green
git switch production
git cherry-pick <fix-commit-sha>
```

With `git cherry-pick`, you need to specify the commit SHA, which is the unique ID for a commit. To identify it, use the `git log` command.

```
# Identify the fix you want to move
git log -n 5

commit 7cd6c22e5ebf1d46afe0c77cabaea5b015b12cb5 (HEAD -> main)
Author: Git User <user@laptop>
Date:   Wed Oct 29 18:52:08 2025 +0100

    Fix ACL guest allow 443

commit 9853262b88d1c78059b9a38dd00077a9d4c1e12f
Author: Git User <user@laptop>
Date:   Mon Oct 27 15:54:57 2025 +0100

    Guest VLAN b01 b02
```

The additional flag `--oneline` concatenates the output to show only the commit SHA and commit message. The `-n` flag specifies the number of commits you want to display.

```
# Identify the fix you want to move
git log --oneline -n 5
7cd6c22 (HEAD -> main) Fix ACL guest allow 443
9853262 Guest VLAN b01 b02
...
```

As an example, the following cherry-picking procedure is used for a quick hotfix on a _testing_ branch, without merging unrelated work. The commit SHA _7cd6c22_ has the ACL changes that you want to introduce to another branch, in this case testin&#x67;**.**

The specific commit that you want to cherry-pick was done on another branch. However, the objective is not to merge all changes (commits) from that branch, but rather a particular one that addresses the immediate requirement.

```
# Apply that commit on testing branch
git switch testing
git pull --ff-only
git cherry-pick 7cd6c22

[testing 3c7b0b9] Fix ACL guest allow 443
  1 file changed, 1 insertion(+), 0 deletions(-)
```

### Git Reset

The following figure shows a `git reset` on a branch named _hotfix_, where changes from several commits were removed by referencing one of the commits in the history.

The `git reset` command moves your branch back to a chosen commit. What happens to your files depends on the mode. With `--hard`, Git also updates the index and working tree, discarding later commits, and local edits.

For example, you made 10 commits. The `git reset <first-sha>` command takes you back to the first commit and drops the nine that followed on your local branch. _Do not use this on a shared branch._

Have a look at more `git reset` examples using different reset modes.

Choose each reset mode to discover more about its function.

git reset --soft

The `--soft` mode aims to change the HEAD reference to a specific commit. For instance, if you realize that you forgot to add a file to the commit, you can move back using the `--soft` option with respect to the following format:

* Use `git reset --soft HEAD~n` to move back to the commit with a specific reference (n). For example, `git reset --soft HEAD~1` moves back to the last commit.
* Use `git reset --soft <commit ID>` to move back to the head with the _\<commit ID>_.

The last commit is located at the HEAD. You can fix this issue by running the following three statements.

* Return to the pre-commit phase using `git reset --soft HEAD`, which enables Git to reset the file.
* Add the forgotten file with `git add`.
* Make the changes with a final `git commit`.

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git reset --soft HEAD~1
git add vlan.yml
git commit -m "added the VLAN configuration file"
</code></pre></td></tr></tbody></table>

git reset --mixed

This is the default argument for `git reset`. Running this command has two impacts: it uncommits all the changes and unstages them. For example, if you accidentally added the **customerA\_vlan.yml** file, and you want to remove it.

* Unstage the files that were in the commit with `git reset HEAD`.
* Add only the files that you need for the commit.

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git reset HEAD
git status
On branch master
Untracked files:
(use "git add &#x3C;file>..." to include in what will be committed)
customerA_vlan.yml
customerB_vlan.yml
customerC_vlan.yml
nothing added to commit but untracked files present (use "git add" to track)
git add customerB_vlan.yml  customerC_vlan.yml
git commit -m "Removed the customerA_vlan.yml from the commit"
</code></pre></td></tr></tbody></table>

git reset --hard

This option can be dangerous, so use it with caution. Performing a hard reset on a specific commit forces HEAD to return to that commit and deletes all changes made after it.

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>ls
customerA_vlan.yml  customerB_vlan.yml  customerC_vlan.yml 
git reset --hard 97159bc
HEAD is now at 97159bc added customer A and B VLAN
ls
customerA_vlan.yml customerB_vlan.yml
</code></pre></td></tr></tbody></table>

Notice in the example that the untracked file customerC\_vlan.yml is deleted. Again, ensure that you fully understand the effects of the `reset` command before using it.

### Git Revert

The following figure shows a `git revert` on a **hotfix** branch with three commits. You revert the first commit. Git creates a new commit that undoes the changes only from that first commit, leaving the other commits intact.

`git revert` creates a new commit that inverts a specific previous commit. It keeps history intact, which is why it is the safest way to undo on branches that others pull from.

For example, undo a faulty access list change on the main branch.

<pre><code>git switch main
git pull --ff-only
<strong>git revert &#x3C;bad-commit-sha>
</strong>git push
</code></pre>

### Git Restore

If you do want to revert only changes of a single file and not the whole commit(s), you can use a `git restore [--source=<ref>] <filepath>` . With `git restore`, no new commit is created until you make one.

```
# Discard local changes to a file
git restore policies/acl_guest_iosxe.cfg

# Restore a file to a prior version, then record that as a new commit
git restore --source=HEAD~1 -- policies/acl_guest_iosxe.cfg
git add policies/acl_guest_iosxe.cfg
git commit -m "Restore ACL file to HEAD~1 version"
```

{% hint style="info" %}
When should you use `git revert`, `git reset`, and `git restore`?

On shared branches, use `git revert`. For local history cleanup, use `git reset`. For discarding or restoring one file, use `git` `restore`.

* If you have already pushed your commits, `git reset` is risky because it rewrites history. Prefer `git revert`, which keeps all prior commits and adds a new commit that undoes the target, so the audit trail stays intact.
* Use `git reset` before you push to move your branch back to an earlier commit. Choose the mode based on what you want to keep. `--soft` keeps changes staged, `-`**`-mixed`** keeps them but unstages, `--hard` discards them. This lets you rewrite a message, split a big change into smaller commits, or start fresh.
* Use `git restore` to discard uncommitted edits in a file or to revert a file to a state from a specific commit, tag, or origin/main, then commit that change.
{% endhint %}

## Centralized Storage with GitLab <a href="#page-heading" id="page-heading"></a>

Both GitLab and GitHub host Git repositories and support reviews and automation. GitLab is often chosen for its all-in-one platform in a single product, including built-in CI runners, approvals, code owners, secret scanning, and detailed permissions. If your company standardizes on GitHub, the same ideas apply.

GitLab is a self-hosted or cloud-based platform that enables you to manage the full lifecycle of Git projects, from storage and reviews to CI/CD and releases, through an intuitive web interface.<br>

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

If you have a local Git repository that you want to manage with GitLab, you must first create a project on GitLab. Once the project is created, you can find the SSH or HTTP URL that you can use to configure your remote URL, or run a `git clone`.

Once you have the URL from the project repository, and you have a local Git repository already, you can configure the GitLab remote and push the changes.

```
git remote add origin git@gitlab.example.com:student/network-configs.git
git push -u origin main
```

Use the project page to browse commits and configuration changes, track issues, or open a merge request to merge changes from one branch into another using the web interface.

### GitLab CI/CD Pipeline

GitLab includes built-in CI/CD pipelines, so you automate lint, render, and plan steps in the same place you store your configurations, no extra tools needed.

### Note

When self-hosting GitLab, you need to deploy your own GitLab runners that will execute the pipeline jobs.

When configured, each merge request can run the pipeline, store artifacts, and can block the merge until checks pass. This keeps every change tested, consistent, and auditable before it reaches devices.

The GitLab pipeline consists of:

* **Commit:** A change in the code.
* **Job:** Runner instructions.
* **Pipeline:** A group of jobs divided into different stages.
* **Runner:** A server or agent that implements each job separately and can spin up or down if needed.
* **Stages:** Parts of a job (for example, build or tests). Multiple jobs inside the same stage are executed in parallel.

A GitLab CI/CD pipeline is configured using a YAML file (.gitlab-ci.yml) in the root of the project.

```
stages: [render, plan]

render_acl:
  stage: render
  image: alpine
  script:
    - mkdir -p rendered
    - echo "sample IOS-XE ACL" > rendered/acl.txt
  artifacts: { paths: [rendered/acl.txt] }

terraform_plan:
  stage: plan
  image: alpine
  script:
    - mkdir -p terraform
    - echo "sample terraform plan" > terraform/plan.txt
  artifacts: { paths: [terraform/plan.txt] }
```

The previous CI example configuration runs two jobs in order, **render\_acl** then **terraform\_plan**. Each job starts a clean Alpine container, creates a folder, writes a sample text file, and uploads it as an artifact. GitLab stores these artifacts with the pipeline, you can open them from the job page or the merged request to verify outputs.

The CI example is simple, and can be used for testing to confirm that your CI is wired correctly, stages run, artifacts persist, and permissions work. After you are sure that the pipeline runs as expected, you can replace the echo lines in the previous example with real template rendering and Terraform commands.<br>

Pipelines can be initiated by various events; you can choose triggers to suit specific workflows.

Common triggers are:

* Push to a branch
* Merge request created or updated
* New tag pushed
* A scheduled time
* A manual click
* An API trigger

You control this with rules in the GitLab CI configuration file. You can also skip a pipeline by putting `[skip ci]` in the commit message.

```
# Run only on merge requests
rules:
  - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

```
# Run when files in these folders change
rules:
  - changes:
      - policies/** 
      - terraform/**
```

## Gitbook - ​​What is documentation as code?

**Documentation as code** is the process of creating and maintaining documentation using the same tools that you use to code.

That could mean different things depending on the tools you use, but it will probably involve elements such as version control, Markdown formatting, automated reviews and tests. By following these existing workflows, the development and product teams can work more closely together, and technical writers can be involved in the documentation process earlier.

It also means that your technical documentation is easier to keep up-to-date, because it’s created in sync with the development process. It’s also likely to be more accurate, as the developers themselves will typically write the first draft themselves.

Plus, with the option to include documentation within those automated reviews and tests, you can catch any undocumented code before it’s merged, and check for formatting and style errors. It all adds up to documentation that’s clearer and more consistent.
