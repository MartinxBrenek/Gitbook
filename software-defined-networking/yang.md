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

# YANG

### Yet Another Next Generation (YANG)

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
