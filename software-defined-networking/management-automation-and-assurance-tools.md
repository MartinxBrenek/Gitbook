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

# Management, Automation and Assurance tools

## On box Automation tools

### Embedded Event Manager (EEM)

**Embedded Event Manager (EEM)** IOS built-in tool that allows to use scripting and build software applets that can automate many tasks

Scripts can automatically execute, based on the event happening on a managed network device

EEM Applets are composed of multiple building blocks // if-then statement logic

#### Components

**EEM server:** Consists of event detectors/publishers and event subscribers. Triggers a subscriber when a detector/publisher sends a notification about an interesting event (defined by the event subscribers)

**Event detectors/publishers:** Monitoring the device for events happening (defined by the event subscribers) and notify the EEM server in case of an interesting event.

**Event subscribers (Applet):** Scripts that get triggered through the EEM server by an event that happened at one of the event detectors/publishers.

**Policy Director:** The policy director is responsible for coordinating and managing the applets. It ensures that the right applet is triggered when a registered event occurs.

#### Possible detectors/publishers

**Interface:** Allows interface parameters to be monitored (eg. threshold violation).

**Routing:** Allows routing events to be monitored (eg. routes)

**SNMP:** Allows MIB objects to be monitored.

**Syslog:** Allows syslogs to be monitored for a specific pattern/string.

**Timer:** Allows actions to be executed based on a specific time (eg. cron job).

**Track:** Allows tracking objects to be monitored (eg. up/down state).

![](<../.gitbook/assets/Unknown image (959)>)

#### Action examples

![](<../.gitbook/assets/Unknown image (960)>)

Construct a script that changes the routing from gateway 1 to gateway 2 from 11:00 p.m. to 12:00 a.m. (2300 to 2400) only, daily.

![](<../.gitbook/assets/Unknown image (961)>)

10\*\*\* means 1 minute after 0 hour regardless of what day or month it is

abcde

a=minute (0-59)

b=hour (0-23)

c=day of month (1 - 31)

d=month (1 - 12) January is 1

e=day of week (0 - 6) Sunday is 0

### Command Scheduler (KRON)

**Command Scheduler (KRON)** allows customers to schedule fully-qualified EXEC mode CLI commands to run once, at specified intervals, at specified calendar dates and times, or upon system startup

Command Scheduler has two basic processes. A policy list is configured containing lines of fully-qualified EXEC CLI commands to be run at the same time or same interval. One or more policy lists are then scheduled to run after a specified interval of time, at a specified calendar date and time, or upon system startup. Each scheduled occurrence can be set to run either once only or on a recurring basis.

#### Configuration guide

[https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/ntw-servs/b-network-services/m\_cns-cmd-sched.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/ntw-servs/b-network-services/m_cns-cmd-sched.html)

#### Example

| kron occurrence OCCU\_BCKP at 22:00 Sun recurring policy-list BACKUP\_CONF ! kron policy-list BACKUP\_CONF cli write memory |   |
| --------------------------------------------------------------------------------------------------------------------------- | - |

## Off box Automation Tools

**Configuration Drift** happens when individual changes made over time cause a device's configuration to deviate from the standard configurations as defined by the company

Engineers make changes to devices (to troubleshoot and fix network issues, test configurations, etc.), so the configuration of a device can drift away from the standard

Each change should be somehow documented, but text format is deprecated and can lead to typos and inaccurate data

**Configuration provisioning** refers to application of configuration to new devices and also how configuration changes are applied to devices

For network engineers we need to automation to increase network reliability and decrease repetitive tasks

### Network automation benefits

* Human errors reduced
* Scalability, deployments and changes implemented almost immidiately
* Generate and perform configurations for new devices on a large scale
* Opex (operating expenses) costs are reduced due to automation efficiency (each task requires fewer human-hours)

### Automation architecture

Day 0: Provisioning automation - features like ZTP,PXE can be used to provision the device to the network

Day 1: Model-driven programmability 0 - all programmatic interfaces like NETCONF,RESTCONF or gNMI based on YANG data model can be used to configure the device to the desired state

Day 2: Model-driven telemetry - to allow telemetry on the device and get continuous state of the device

Day N: Device optimization

Configuration management tools facilitates the centralized control of large numbers of network devices. Two Main components Template and Variables. Client/server model is used

### Ansible

**Ansible** is a configuration management tool owned by Red Hat written in Python. Most popular; Is declarative

Methology plan PPDIOO (Prepare, Plan, Design, Implement, Observe, Optimize)

**Agentless** doesn't require any special software to run on the managed devices

**Push model** server push configurations to the clients

Control machine uses SSH for communication. Ansible-managed nodes must have an SSH server running

Also supports Windows Remote Management (WinRM) and other transport methods

Ansible Automation Engine UI where users create playbooks for automation

#### Engine components

**Task** the smallest unit of action (configure IP or execute show command)

**Play** a set of tasks; grouping a set of hosts

**Playbooks** a set of plays written in YAML; deploy configuration changes or retrieve info from clients

**Inventory** list of devices, characteristics of each device and their roles (access/core switch, router, fw, ..)

**Modules** tasks invoked by playbooks that are executed against clients

**API** is used to interact with public and cloud managed devices; **Plugins** are pre-built pieces of code

![](<../.gitbook/assets/Unknown image (920)>)

### Puppet

**Agent-based** (Specific software must be installed on the managed devices)

**Pull model** clients pull configurations from the Puppet master

Files use a DSL (Domain-Specific Language) based on Ruby

Ruby is a dynamic, open-source programming language known for its elegant syntax and focus on simplicity and productivity.

Puppet manages systems in a declarative manner meaning you define the state the target system should be in without worrying about how it happens. In reality, that is true for all these tools.

Puppet master (server) linux based; can be deployed with backup

Puppet agent (client) use TCP 8140 to communicate with the Puppet master

#### Components

Facts contain info about puppet agents; sent to master to view current state of agent

Catalogs prepared by master for the agent with configuration changes (secured with SSL/cert during deploy)

Puppet console executes tasks

Catalog structures

Resources a description of the desired machine state

Manifests code that configures the clients

Module collection of manifests used for automation task; stored in PuppetDB on master

// Compile masters are multiple masters to increase supported nodes. An Master of masters (MoM) is then deployed to manage all masters

Puppet Bolt is agentless version and connects via SSH/WinRM. Individual commands can be run in CLI of Linux/Windows or there is available GUI for enterprises

Copies the script into a temporary directory on the remote device, executes the script, captures results, and removes the script from the remote system as if it were never copied there

### Chef

**Agent-based** also use a DSL based on Ruby; Is procedural

**Pull model** The server uses TCP 10002 to send configurations to clients

Key distinction is that Ansible uses YAML, a Python-based configuration language that is easier to learn and oriented to administrators, whereas Chef uses Ruby, a Domain Specific Language (DSL) that is oriented to developers

#### Components

Recipes configuration information written in Ruby

Cookbooks a collections of receipes

Chef-repository on workstation where cookboks are created

Bookshelf respository of the server where cookboks are stored

Knife Command line tool way that workstations communicate with the server (RSA public key pair)

OHAI used to collect the current state of a node to send the information back to the Chef server

kitchen is a place where all recipes and cookbooks can be tested prior to hitting any production nodes

#### Chef server deployment types

Chef Solo: The Chef server is hosted locally on the workstation

Chef Client and Server This is a typical Chef deployment with distributed components

Hosted Chef The Chef server is hosted in the cloud

Private Chef All Chef components are within same enterprise network

### SaltStack

**SaltStack** works as both a push and pull model; agent-based and agentless (Salt SSH); written in Python and is declarative

Instructions pushed out in YAML. Can be run in CLI of server or on GUI

On the right is shown request of execution of network.interfaces on all clients to view IP and MAC info

Salt master server

Minions client

#### Components

Grains info about managed nodes sent to master

Pillar store data that minions can retrieve; contains minion-specific data

Reactor listens for any type of changes in the node or device that differ from the desired state or configuration

Beacon live on minions, to be monitored by reactor

asterisk (\*) includes all nodes

## Cisco Network Services Orchestrator (NSO)

Primarily, **Cisco Network Services Orchestrator (NSO)** is a multivendor service orchestration platform that enables end-to-end service orchestration across physical or virtual devices. Cisco NSO can also be perceived as a multivendor service-layer SDN controller for data centers or service providers. It provides a single application programming interface (API), and a single user interface to the network.

NETCONF is a preferred device management protocol.

YANG is used for service and device modeling.

This means that network devices can be managed in a standardized way, regardless of vendor or hardware differences. Service management systems don’t need to worry about device-specific configurations; they just interact with YANG models

Provide northbound service management application programming interfaces (APIs).

Reliable network configuration management is achieved through networkwide transactions.

Provide southbound management interfaces for non-NETCONF elements.

The **FASTMAP algorithm** further simplifies service design and also ensures optimal and reliable configuration management.

FASTMAP enables automatic management of service instances. Any change to a particular service instance has to comply to its corresponding service definition. Mapping of service instance to the corresponding device configuration that is used in the service create process is defined either through high-level API or a configuration template. The programmer can define custom service create process in corresponding Java or Python method. If later on, a user decides to change or delete an existing service instance, Cisco NSO calculates the changes and applies them automatically.

At any point in time a user or external client can modify a service and Cisco NSO will automatically calculate the required minimum difference that is needed for reflecting that change on the network. Also, deleting a service instance from the network will automatically clean up all configuration data on all devices that are mapped to that service.

**Service dry-run:** Cisco NSO calculates what would happen if the service is committed towards the network.

**Service check-sync:** Is the service configuration in sync with the actual device configuration? This action can detect cases where a network engineer tampers with service configuration on the Cisco NSO or directly on a network device.

**Service re-deploy:** Cisco NSO generates the minimum amount of device configuration that is needed to restore a service on the network.

**FASTMAP southbound operations** are

**create** – Used to create new configurations on the network devices.

**update** – Modifies existing configurations on the devices.

**delete** – Removes configurations from the devices.

### Logical architecture

The logical architecture outlines the Cisco NSO two-layered approach. A device manager that simplifies device interface integration and manages device configuration scenarios, and a service manager that applies service changes to devices. At the core of Cisco NSO is the configuration data store, Configuration Database (CDB), which is in sync with the actual device and service configuration. It also manages relationships between services and devices, and can handle revisions of device interfaces.

![](<../.gitbook/assets/Unknown image (838)>)

Service Layer (Specification Layer)

Acts as an interface for operators and applications, hiding network complexities.

Uses model-to-model mapping to translate high-level services into device-specific configurations.

Transactional, meaning changes are applied fully or not at all, avoiding partial failures and simplifying error handling.

Network Operating System (NOS) Layer

Provides a device-level interface for management.

Transactional, ensuring smooth error recovery and activation processes.

The power of Cisco NSO lies in its model-to-model mapping principles. It is able to describe complex services at the service layer and map those services to device models, which represent various devices from various vendors. This way, complex services can be described and implemented in simple ways and pushed to the devices without any concern about device vendor, configuration semantics, configuration interfaces, and so on

Forwarding Layer

Controls devices via OpenFlow, Cisco CLI, SNMP, NETCONF, etc.

Real-world networks use a mix of traditional and OpenFlow devices.

Supports multiple protocols, ensuring compatibility across different network devices.

Device behaviors are standardized using data models.

Which channel does Cisco NSO use for communication with the forwarding layer?

Network Element Drivers (NEDs) for southbound communication.

Northbound APIs are exposing the NSO services. NEDs are not used for northbound communication. SNMP and CLI can be part of the NED. Southbound APIs are not part of NSO concept.

![](<../.gitbook/assets/Unknown image (839)>)

### Core engine

**Core engine** handles fundamental functions like transactions, high-availability replication, upgrades and downgrades, and other crucial operational functions

The transaction manager handles nested transactions between services and devices, and is capable of rolling back service configurations across multiple devices.

All NSO operations pass through a centralized authentication, authorization, and accounting (AAA) engine that handles authentication and session management.

Role-based access control can be defined at any granularity level.

When high availability is enabled in ncs.conf, the configuration database (CDB) automatically replicates data that is written on the primary node to the connected subordinate nodes. Replication occurs on a per-transaction basis to all the subordinates in parallel. It can be configured to occur asynchronously (best performance) or synchronously in step with the transaction (most secure).

All activities are logged in an audit log.

Cisco NSO runs its own internal semantic and syntactic validation against the model constraints.

Cisco NSO provides transaction rollback and saves configuration rollbacks in separate files.

Core engine also handles upgrades and downgrades.

All other configuration and operational data is handled in the main CDB.

### Configuration Database (CDB)

Cisco NSO uses **Configuration Database (CDB)** to store its own configuration, the configuration of all services, copies of the configuration of all managed devices, and all NSO-specific operational data. CDB is a RAM database, the entire configuration is kept in RAM always. Thus, if you use Cisco NSO to manage a large network, you need a lot of RAM memory. Persistence of the CDB occurs in a way that every transaction, per successful execution, is written to the disk drive of the server. CDB is hierarchical with a tree-like structure.

While it is possible to integrate and run Cisco NSO with databases other than CDB, the trade-offs might not be worthwhile. There are many advantages to CDB, compared to using some external storage for configuration data. CDB has:

A solid model on how to handle configuration data in network devices, including a good update subscription mechanism.

A networked API whereby it is possible for a nonconfigured device to find the configuration data on the network and use that configuration.

Fast and lightweight database access. By default, CDB keeps and operates the entire configuration in RAM. Disk is used for persistence purposes.

CDB is easy to use, is already integrated into Cisco NSO, and the database is lightweight and has no maintenance needs. Writing instrumentation functions for accessing data are easy.

Automatic support for upgrade and downgrade of configuration data

![](<../.gitbook/assets/Unknown image (840)>)

### Service manager

With the **service manager**, you can develop service-aware applications, such as IPTV or Multiprotocol Label Switching (MPLS) VPNs that configure devices. In this case, the data models for the services are not defined by the devices but are the result of a modeling activity. Apart from the YANG modeling activity to define the model, the model must be mapped into the corresponding device operations. Usually, you map the models by specifying configuration templates that transform service parameters to device configuration parameters. You can also perform this task by programmatically "mapping logic" for more complex cases. Both approaches use Cisco NSO FASTMAP to simplify the mapping and to handle the complete lifecycle, for example, by updating and deleting service instances.

Service manager provides these functions:

Service modeling using YANG

Mapping to device models, using templates (XML), and/or Java or Python code

Service activation

Service modification

Service decommissioning

Service restoration

Service synchronization with device configurations

Service testing

Aggregated operational data

Validation of service configuration.

Transaction-safe activation of services across various devices

mapping logic:

Transforms the service models to device models

When templates are not enough:

External callouts can be made

Expressions or algorithms in Java or Python code can be used

Uses FASTMAP

Additional code required for a complex service

![](<../.gitbook/assets/Unknown image (841)>)

### Device manager

The purpose of the **device manager** is to manage various devices using a clean YANG and NETCONF view. Irrespective of an underlying interface such as SNMP or CLI, the device manager will provide a transactional view toward devices. Users or programmers can then configure the devices and read operational data from the devices in a unified way. When a device supports YANG and NETCONF natively, the device manager is totally automatic. Non-NETCONF devices are integrated using the NED toolkit.

Quickly provision a new device by copy-and-edit either from the configuration of another device or from a template configuration.

Deploy configuration changes to multiple devices in a fail-safe way, using distributed transactions.

Validate the integrity of configurations before deploying to the network.

Apply configuration changes to named device groups.

Apply templates (with variables) to named device groups.

Easily roll back changes, if necessary.

Configuration audits: Check whether device configurations are in sync with the Cisco NSO database. If the result is false, the difference can be outputted.

Synchronize the Cisco NSO database and the configurations on devices, in case they are not in sync. You can do this in either direction by either importing the difference to the Cisco NSO database or deploying the difference on devices.

Automatic and code-free integration to NETCONF (such as Juniper), Cisco CLI, and SNMP devices. Limited amount of code to other proprietary interfaces.

### Templates

**Templates** are a flexible and powerful mechanism that simplifies how changes can be made across the configuration data, and also allows a declarative way to describe such manipulations. Templates can be applied in a traditional "fire-and-forget" fashion by using them in actions or when Cisco NSO FASTMAP uses them as part of a service implementation. When a template is used as part of a service implementation, Cisco NSO stores the configuration changes made toward the devices for possible later modification.

Two types:

Device templates: For direct activation, for example, through Cisco NSO CLI or REST

Configuration templates: For service mapping to device configurations

### Package manager

**Package manager:** A **package** is basically a directory of files with a fixed file structure. The package manager takes care of package lifecycle. Packages are extending Cisco NSO built-in functionalities.

A package consists of code, YANG modules, and custom WebUI widgets, and so on, that are necessary for adding an application or function to Cisco NSO. Packages are a controlled way to manage the loading and running of custom applications

Cisco NSO packages contain data models and code for a specific function. It might be a NED for a specific device, a service application like MPLS VPN, a WebUI customization package, and so on. Packages can be added, removed, and upgraded in run-time.

A package for a specific function contains:

Data models (YANG)

Code (Java/Python)

A package might be:

NED for a specific device

Service application like MPLS VPN

WebUI customization

YANG Model (Defines config schema) → ./mypkg/src/yang/mypkg.yang

Templates (Map service data to device config) → ./mypkg/templates/mypkg.xml

Code (Python/Java) (Extends functionality) → ./mypkg/python/mypkg/main.py

package-meta-data.xml (Defines package components to load)

#### Package management

Listed here are the main steps in the creation process for a new package:

Create a package skeleton using the ncs-make-packages tool, which simplifies initial package creation by creating a skeleton directory and file structure.

Develop the package by using a text editor or the appropriate Integrated Development Environment (IDE) to develop the package by modifying and creating the necessary files (YANG, Java, Python, XML templates)

When done, compile the package by issuing the make command in the src subdirectory.

Includes syntax and semantic verification of package definitions

Reads the makefile to determine the components to compile

Activate the package by issuing the packages reload command in Cisco NSO CLI:

Activates new packages or upgrades existing packages.

Reads the package-meta-data.xml file to determine which components to load.

### Alarm manager

An **alarm** is an undesirable state in a resource for which an operator action is required.

Cisco NSO embeds a generic alarm manager. It is used for managing Cisco NSO native alarms and can easily be extended with application-specific alarms. Alarm sources can be notifications from devices, detected undesired states on services, or anything provided via the APIs.

Cisco NSO contains other mechanisms for logging in general. Therefore, Cisco NSO does not naively populate the alarm list with traps that are received in the SNMP notification receiver. State and model-based (service to device model) correlation of alarms rather than notification-based

It has states (e.g., active, cleared) and is linked to resources (e.g., device, time, severity).

Operators manage alarms based on user-defined rules for logging and resolution.

Cisco NSO does not automatically clear alarms—network conditions must trigger clearance

### Cisco NSO interfaces (CLI/WebUI)

Cisco NSO CLI:

Access the Cisco NSO CLI via SSH on port 2024 (default)

Two modes (use the config and exit commands to switch between them):

Operational

Configuration

Two CLI styles (use the switch cli command to switch between them):

Cisco

Juniper

Autocompletion using tab or space characters

Question mark for syntax help and command description

NSO WebUI via HTTP on port 8080 (default)

The NSO WebUI has two modes of operation:

Configuration: [http://localhost:8080/webui-one/](http://localhost:8080/webui-one/)

Legacy: [http://localhost:8080/prime/](http://localhost:8080/prime/)

The ncs.conf file contains the configuration with default management ports.

### Network Element Drivers (NEDs)

**Network Element Drivers (NEDs)** are the components that device manager uses as an interface to manage the devices. NEDs provide the connectivity between Cisco NSO and the devices. NEDs are installed as Cisco NSO packages.

For NETCONF and SNMP devices there is no code. For CLI devices there is a minimum of code managing connecting over SSH/Telnet and looking for version strings. The rest is auto-rendered from the data-model. Independent of underlying device interface technology NEDs come with a data-model in YANG that specifies configuration data and operational data that is supported for the device.

![](<../.gitbook/assets/Unknown image (842)>)

#### NETCONF NED

Having a NETCONF agent running on a device enables the Cisco NSO to communicate directly with the device. No coding is required to support device configuration. Some, but not all vendor support this capability.

Cisco NSO knows how to automatically communicate southbound to NETCONF and SNMP enabled devices. By supplying Cisco NSO with the YANG models of a NETCONF device, Cisco NSO knows the data models of the device, and through the NETCONF protocol knows exactly how to manipulate the device configuration. This can be used for a NETCONF capable device, any device that uses ConfD as management system, or any other device that runs a compliant NETCONF server. Similarly, by providing Cisco NSO with the MIBs for a device, Cisco NSO can automatically manage such a device using SNMP.

Cisco NSO uses XML data internally (NETCONF)

Native NED for Cisco NSO

No coding required

NETCONF NED YANG

YANG model: Defines device XML config

Only YANG device model required

Can be used with any device supporting NETCONF

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

#### CLI NED

is an entirely model driven way to CLI script towards all Cisco like devices. The basic idea is that the Cisco CLI engine found in ConfD, which is part of the Cisco NSO in this case, can be run in both directions:

A sequence of Cisco CLI commands can be turned into the equivalent manipulation of the internal XML tree that represents the configuration inside Network Control System (NCS)/ConfD. This is the normal mode of operations of ConfD, run in Cisco mode. A YANG model, annotated appropriately, will produce a Cisco CLI. The user can enter Cisco commands and ConfD will, using the annotated YANG model, parse the Cisco CLI commands and change the internal XML tree accordingly. Thus, this is the CLI parser and interpreter. ConfD is model driven.

The reverse operation is also possible. Given two different XML trees, each representing a configuration state, in the ConfD case it represents the configuration of a single device, that is, the device using ConfD as management framework, whereas in Cisco NSO's case, it represents the entire network configuration, you can generate the list of Cisco commands that would take you from one XML tree to another. This technology is used by Cisco NSO to generate CLI commands southbound when we manage Cisco like devices.

NSO uses XML data internally (NETCONF)

CLI NED YANG

YANG model: Defines device XML config

YANG extensions: Define device CLI config

Java code

For NED logic

For SSH

ConfD converts between XML and CLI (bidirectional)

![](<../.gitbook/assets/Unknown image (843)>)

#### SNMP NED

Cisco NSO can configure a managed device using SNMP in certain cases, but SNMP is generally unsuitable for configuration due to several limitations. The SNMP SET request is restricted to a single UDP packet, requiring large changes to be split into multiple requests, making rollback difficult if one fails. SNMP’s data model (SMIv2) does not differentiate between configuration and other writable objects, meaning retrieving only configuration data requires explicit knowledge of all MIBs. Additionally, SNMP only supports basic read and write operations, lacking built-in mechanisms for creating or deleting data. Semantic constraints are also limited, making it hard to ensure a configuration will apply cleanly.

Despite these challenges, Cisco NSO can configure devices over SNMP if the MIBs are properly annotated to guide NSO in handling configuration data correctly.

![](<../.gitbook/assets/Unknown image (844)>)

To add a device, the following steps need to be followed. They are described in more details in the following sections.

Collect (a subset of) the MIBs supported by the device.

Optionally annotate the MIBs with annotations to instruct Cisco NSO on how to talk to the device, for example ordering dependencies that are not explicitly modeled in the MIB. This step is not required.

Compile the MIBs and load them into Cisco NSO.

Configure Cisco NSO with the address and authentication parameter for the SNMP devices.

Optionally configure a named MIB group in Cisco NSO with the MIBs supported by the device, and configure the managed device in Cisco NSO to use this MIB group. If this step is not done, Cisco NSO assumes that the device implements all MIBs known to Cisco NSO.

See the makefile: snmp-ned/basic/packages/ex-snmp-ned/src/Makefile, for an example of the below description. Make sure that you have all MIBs available, including import dependencies and that they contain no errors.

Compiling and Loading MIBs

The ncsc --ncs-compile-mib-bundle compiler is used to compile MIBs and MIB annotation files into Cisco NSO load files. Assuming a directory with input MIB files (and optional MIB annotation files) exist, the following command compiles all the MIBs in device-models and writes the output to ncs-device-model-dir.

$ ncsc --ncs-compile-mib-bundle device-models --ncs-device-dir ./ncs-device-model-dir

The compilation steps performed by the ncsc --ncs-compile-mib-bundle are elaborated below:

Transform the MIBs into YANG according to the IETF standardized mapping ( [http://www.ietf.org/rfc/rfc6643.txt](http://www.ietf.org/rfc/rfc6643.txt)). The IETF defined mapping makes all MIB objects read-only over NETCONF.

Generate YANG deviations from the MIB, this basically makes SMIv2 read/write objects YANG config true as a YANG deviation.

Include the optional MIB annotations.

Merge the read-only YANG from step 1 with the read/write deviation from step 2.

Compile the merged YANG files into Cisco NSO load format.

This list summarizes the key features of SNMP NED:

Cisco NSO uses XML data internally (NETCONF)

SNMP NED YANG

MIBs: Translated to YANG

YANG model: Defines device XML config

ConfD converts between XML and SNMP (bidirectional)

#### Generic NED

is used in Cisco NSO to manage devices that do not support NETCONF, SNMP, or CLI modeling. This includes devices that rely on proprietary CLIs or protocols like REST, CORBA, XML-RPC, or SOAP. Similar to a CLI NED, a Generic NED must connect to the device, retrieve capabilities, apply changes, and sync the configuration.

Instead of sending CLI commands, a Generic NED receives a set of NedEditOp objects, each representing an operation like create, delete, or update, along with a keypath and optional value. Syncing configuration requires fetching the full device config and writing it into a transaction using the Maapi interface.

Once implemented, a Generic NED allows Cisco NSO to perform network-wide transactions just like NETCONF and CLI NEDs. However, rollback is challenging for devices that don’t support transactions, requiring reverse diff calculations to undo changes. Authentication also depends on the device protocol, sometimes requiring custom authentication methods.

NSO uses XML data internally (NETCONF)

Generic NED YANG

YANG model: Defines device XML config

Java code

For protocol implementation

ConfD converts between XML and DOM (bidirectional)

![](<../.gitbook/assets/Unknown image (845)>)

### Network Management Interface Simulator (ncs-netsim)

The **Network Management Interface Simulator (ncs-netsim)** program is a useful tool to simulate a network of devices to be managed by Cisco NSO. It makes it easy to test Cisco NSO packages towards simulated devices. All that you need is the Cisco NSO NED packages for the devices that you need to simulate. The devices are simulated with the Tail-f ConfD product. Every Cisco NSO instance comes with netsim simulation tool installed. Netsim is capable to simulate only the device management interfaces, Cisco NSO is managing a device as if it were real.

Optional add-on to NEDs

Simulates device's native CLI

Useful for development and testing

Lightweight: Enables simulation of many devices on a desktop

Uses YANG model of NSO NED and ConfD in reverse to simulate a device's CLI

| admin$ ncs-netsim --help |   |
| ------------------------ | - |

![](<../.gitbook/assets/Unknown image (846)>)

### NED development process

If there is a need to develop a new NED, the best way to go about is to contact Cisco NSO team. Most likely, the work required will need supervision and support from experienced NED developers.

### NSO ecosystem and ETSI NFV MANO

The figure illustrates the European Telecommunications Standards Institute (ETSI) Management and Orchestration (MANO) architecture that is used to enable an NFV service

The NFV Orchestrator (NFVO) layer where Cisco NSO can be deployed to orchestrate NFV services.

The VNF Manager (VNFM) layer where Cisco ESC can be deployed to control the lifecycle of VNFs. Alternatively, any other 3rd party system can be used to achieve a similar management goal.

The Virtualized Infrastructure Manager (VIM) is used to manage the virtual and optionally also physical infrastructure hosting and interconnecting the VNFs. The figure lists the following examples: OpenStack, VMware, Cisco APIC (for Cisco ACI solutions), Cisco Virtual Topology Controller (VTC) for Cisco Virtual Topology System (VTS) solutions, or any other 3rd party systems.

The last layer consists of the physical and virtual infrastructure that provides the end-functionality for the designed NFV services such as physical and virtual network devices.

![](<../.gitbook/assets/Unknown image (847)>)

![](<../.gitbook/assets/Unknown image (848)>)

The figure illustrates a solution where Cisco NSO is responsible for:

Network Functions Virtualization Infrastructure Software (NFVIS) consisting of several selected VNFs.

Service chains used to link individual components into an end-to-end service.

New branch/physical device registers with day-0 server (Cisco PnP).

Day-0 server provides initial config and registers branch/physical device in Cisco NSO.

Cisco NSO initiates VNF virtual machine creation via Cisco ESC Lite that is part of NFVIS.

Cisco NSO deploys service by configuring physical and virtual devices.

Firewall policies

VLANs

Access control lists ACLs

Port security (802.1X)

Quality of service (QoS)

![](<../.gitbook/assets/Unknown image (849)>)

### Cisco Elastic Service Controller (ESC)

**Cisco Elastic Service Controller (ESC)** is a Virtual Network Functions Manager (VNFM) that performs lifecycle management of VNFs. Cisco ESC provides agentless and multivendor VNF management by provisioning virtual services and monitoring their health. Also, it promotes agility, flexibility, and programmability in NFV environments. It provides the flexibility to define rules for monitoring, and associate actions that are triggered based on the outcome of these rules. Based on the monitoring results, Cisco ESC performs scale-in or scale-out operations on the VNFs. In the event of a VM failure, Cisco ESC also supports automatic VM recovery

Cisco ESC fully integrates with Cisco and other third-party applications. As a standalone product, the Cisco ESC can be deployed as a VNFM. Cisco ESC integrates with Cisco NSO to provide VNF management along with orchestration. Cisco ESC as a VNFM targets the Virtually Managed Services (VMSs) and all service provider NFV deployments, such as virtual video, Wi-Fi, authentication, and so on. Complex services include multiple VMs that are orchestrated as a single service with dependencies between them. These multiple VMs are managed as a single entity, such as VMS 1.0 and later.

Functionalities

VNF lifecycle management

VNF day-0 configuration

VNF license management

VM and service monitoring, recovery, and elasticity

Transaction resume and rollback

Coupled VM VNF management (VM affinity, startup order, manage VM interdependency)

## Cisco Catalyst Center (CCC)

**Cisco Catalyst Center (CCC)** (formerly DNACenter) is a Cisco SDN controller for enterprise networks—branch, campus, and WAN

It is a single pane of glass centralized management platform that serves as unified interface or dashabord to provide comprehensive visibility and control over various systems, applications, or network elements from a single location

This simplifies provisioning, automation, management, monitoring, and troubleshooting by consolidating multiple disparate tools and interfaces into a single, intuitive interface

Cisco Catalyst Center provides open programmable APIs for policy-based management and security through a single controller

Cisco Catalyst Center can program the network in an automated way, based on the application requirements, and it represents a basis for intent-based networking.

The controller consistently provisions network services and provides rich network information and analytics across all network resources: both LAN and WAN, wired and wireless, and physical and virtual infrastructures. This visibility allows you to optimize services and support new applications and business models. The controller bridges the gap between open, programmable network elements and the applications that communicate with them, automating the provisioning of the entire end-to-end infrastructure.

Cisco Catalyst Center provides a single dashboard for managing and controlling the enterprise network. It uses workflows to simplify provisioning of user access policies combined with advanced assurance capabilities. It also provides open platform APIs, adapters, and SDKs for integration with business applications and orchestrators.

![](<../.gitbook/assets/Unknown image (59)>)

The enterprise programmable network infrastructure sends data to the Cisco Catalyst Center virtual appliance. The appliance activates features and capabilities on your network devices using the Cisco Catalyst software. Everything is managed from the Cisco Catalyst Center dashboard.

Cisco Catalyst Center is a robust software solution that is designed to simplify and enhance network management. It offers a variety of deployment options to meet the diverse operational requirements of different organizations:

Cloud-based deployments: Cisco Catalyst Center Virtual Appliance can be deployed in major public cloud environments such as AWS (expected to be supported in Microsoft Azure and Google Cloud in the future) offering flexibility and scalability. This approach eliminates the need for on-premises hardware, reduces capital expenditure, and provides access to advanced cloud features.

On-premises virtualization: For organizations preferring on-premises solutions, Cisco Catalyst Center supports deployment on virtual environments like VMware ESXi (expected to support Microsoft Hyper-V, and KVM in the future). This enables customers to leverage existing virtual infrastructure, enhancing resource utilization and reducing costs.

Physical appliance: Cisco offers the option to deploy Cisco Catalyst Center as a physical appliance for customers who need dedicated hardware. This option is ideal for environments with strict security or performance requirements, ensuring maximum control and reliability.

## Application Centric Infrastructure (ACI)

**Application Centric Infrastructure (ACI)** is an SDN solution from Cisco for Data Centers, simply ACI is a Network policy-based automation model solution developed by Cisco.

Cisco Network Services Orchestrator (NSO) is industry-leading software for automating services across traditional and virtualized networks.

Cisco ACI is a shift in data center architecture to integrate physical and virtual elements in an open ecosystem model utilizing APIs, open standards, and Open Source elements

It is Cisco enhanced VXLAN, which enables the automation and orchestration of network infrastructure to meet the requirements of modern data centers and cloud environments.

ACI is a layer 3 fabric, where either OSPF or IS-IS is used as routing protocol to build the routing table.VXLAN is used for building Overlay Network.

![](<../.gitbook/assets/Unknown image (834)>)

![](<../.gitbook/assets/Unknown image (835)>)

### ACI components

#### Application Policy Infrastructure Controller (APIC)

**Application Policy Infrastructure Controller (APIC)** is the centralized management and automation platform that provides a single point of control for the entire infrastructure. It allows administrators to define policies and automate network provisioning, security, and optimization across the data center. Configuration is stored in XML or JSON format, these can be configured using APIs as well.

If all APICs go down, there will be no negative impact or outage in the environment because policy is already received by the fabric switches and APIC does not sit in the forwarding plane.

APIC is connected to Leaf Switch. Cisco recommends having minimum 3 APIC servers in odd numbers 3,5,7.

#### Spine and leaf switches

ACI uses spine and leaf Nexus 9000 switches in 2-Tier architecture. ECMP is in place

Spine switches are the core and they connect to the leaf switches, which, in turn, connect to the endpoints such as servers, VMs, ...

NXOS mode device will be managed individually

ACI Mode switch will be managed using APIC. Both modes are mutually exclusive i.e. switching between the mode require deleting of complete configuration.

#### Application Network Profile (ANP)

**Application Network Profile (ANP)** is a logical grouping of network policies that define how a particular application should be treated within the network. ANPs can include policies related to security, QoS, and network connectivity.

#### Endpoint Groups (EPGs)

**Endpoint Groups (EPGs)** are logical groupings of endpoints that share the same network policies. EPGs can include endpoints such as servers, virtual machines, or other network devices.

#### Contracts

**Contracts** define the communication policies between different EPGs within the network. Contracts can include policies related to security, QoS, and network connectivity.

### ACI operation

Define the Application Network Profile (ANP): The ANP defines the network policies required for the application. It includes policies related to security, QoS, and network connectivity.

Create Endpoint Groups (EPGs): EPGs are created to group the endpoints that share the same network policies

Define Contracts: Contracts are defined to specify the communication policies between different EPGs within the network.

Configure Spine and Leaf Switches: Spine and leaf switches are configured to provide connectivity between endpoints in the network.

Deploy the Application: Once the ANP, EPGs, contracts, and switches are configured, the application can be deployed. ACI automatically manages the underlying resources to ensure the application runs as intended

### ACI benefits

Simplified Network Management: ACI provides a centralized management platform that simplifies network management and reduces operational costs.

Automated Provisioning: ACI automates network provisioning, enabling organizations to rapidly deploy and scale applications.

Improved Visibility: ACI provides better visibility into network traffic and application performance, making it easier to troubleshoot issues.

Consistent Security: ACI provides consistent security policies across the entire network, ensuring that applications are protected from cyber threats.

Scalability: ACI is designed to scale to meet the requirements of modern data centers and cloud environments, making it an ideal solution for organizations of all sizes.

### Fabric discovery and protocols

**Discovery Protocol:** When a new ACI fabric is deployed, the leaf switches send out a discovery message to the spine switches to identify themselves. The spine switches then respond with their own discovery message, and the leaf switches store the spine switch information.

**CDP and LLDP:** The fabric uses to discover and exchange information about the network topology. This information is used to build a network topology map that is used by the APIC to manage the fabric.

VXLAN protocol is used to encapsulate and transport the traffic between endpoints in the ACI fabric. VXLAN provides a scalable way to extend Layer 2 networks over a Layer 3 infrastructure.

BGP is used by the spine switches to exchange routing information between each other. BGP is used to distribute the IP prefixes that are associated with the various EPGs in the fabric.

OpFlex is a protocol used by the APIC to communicate with the leaf switches to configure the network policies. The APIC sends policy information to the leaf switches using OpFlex, and the leaf switches translate the policies into configuration commands that are applied to the network devices.

**ACI Policy Model:** The ACI fabric uses a policy model to define and enforce network policies. The policy model is based on the concept of Application Network Profiles (ANPs) and Endpoint Groups (EPGs). ANPs define the policies for a specific application, and EPGs are used to group endpoints that share the same policies.

### Management options

**APIC GUI:** is the centralized management console for the ACI fabric. The APIC GUI provides a web-based interface to configure and manage the fabric. It offers a comprehensive view of the network topology, network policies, endpoints, and various other aspects of the fabric. The APIC GUI is user-friendly and can be accessed from anywhere with an internet connection.

**APIC REST API:** ACI also provides a REST API that allows for programmatic configuration and management of the fabric. The APIC REST API is widely used by developers and automation tools to automate network configuration and management tasks.

**CLI:** ACI also provides a Command Line Interface (CLI) to configure and manage the fabric. The CLI is similar to the traditional Cisco IOS CLI and can be used to perform low-level configuration tasks on the fabric.

Automation and Orchestration Tools: ACI is designed to work with various automation and orchestration tools, such as Ansible, Chef, Puppet, and Terraform. These tools can be used to automate network configuration and management tasks, making it easier to deploy, scale, and manage the ACI fabric.

Third-party Management Tools: ACI can also be integrated with third-party management tools, such as network monitoring and security tools. This integration allows for better visibility and control over the network, enabling more efficient management and troubleshooting.

### Virtual Device Context (VDC)

**Virtual Device Context (VDC)** is a feature provided by some network devices, particularly Cisco Nexus switches. VDC networking allows the physical switch to be partitioned into multiple virtual switches, each operating independently with its own set of resources and configurations. It enables network administrators to logically separate and manage different network environments within a single physical device.

## Service Provider Management Tools

### Cisco Crosswork Network Controller (CNC)

**Cisco Crosswork Network Controller (CNC)** is a transport SDN controller that empowers customers to simplify and automate intent-based network service provisioning, health monitoring, and optimization in a multivendor network environment with a common GUI and API. Cisco CNC simplifies operational workflows by consolidating both the service lifecycle and device management functions in a single integrated solution.

### Optical Network Management

To manage DWDM network solutions built on Cisco NCS platforms, an architecture consisting of two basic components is used.

**Site manager** – this is a software that manages hardware components at individual locations

An example of a site manager component is the SVO (Shelf Virtualization Orchestrator) for NSC2k or COSM for NCS1k

**Central controller** – this is a software providing central management of the overall solution with a global view of the network and services. The architecture of the central controller allows you to manage multiple types of site managers if needed (for example, when there is a combination of multiple types of hardware in one network).

The central controller component is solved using the Cisco Optical Network Controller (CONC) solution.

**Crosswork Hierarchical Controller** – this is a superior system enabling automatic management of heterogeneous networks across multiple domains – for example, via IP and optical networks and through various manufacturers. This software is not included in the offered solution.

### Hierarchy of all systems

![](<../.gitbook/assets/Unknown image (787)>)

### Cisco Transport Controller

**Cisco Transport Controller** serves as a foundational element for the management of optical transport networks. It is primarily responsible for configuration, provisioning, and monitoring of network elements. Operators use Transport Controller to set up connections, allocate wavelengths, and manage resources efficiently. The data and insights generated by Transport Controller become crucial inputs for higher-level planning and optimization.

![](<../.gitbook/assets/Unknown image (782)>)

### Cisco Transport Planner

**Cisco Transport Planner** is a powerful software tool designed to help you plan, design, and optimize your optical network infrastructure.

Cisco Transport Planner complements Cisco Transport Controller by providing advanced network planning capabilities. Network planners leverage Transport Planner to design and optimize optical transport networks. This includes capacity planning, topology optimization, and traffic engineering. The results of planning efforts are fed back into Transport Controller for implementation, creating a closed loop between planning and execution.

It is older software supporting older platforms like NCS2k

![](<../.gitbook/assets/Unknown image (783)>)

### Cisco Optical Network Planner

**Cisco Optical Network Planner** is a software tool used for designing, validating, and optimizing dense wavelength-division multiplexing (DWDM) optical networks

Graphical network design:

Optical Network Planner provides a GUI that allows users to easily create and modify network designs. Users can drag and drop nodes and links to represent network devices and connections.

Optical Network Planner plays a key role in the strategic planning of optical networks. It assists network planners in designing scalable and efficient networks. Optical Network Planner insights influence the decisions made in Transport Planner, guiding the network planning process. The integration between Optical Network Planner and Transport Planner ensures that the long-term vision aligns with the day-to-day operational requirements.

It the successor of CTP - it supports all NCS optical platforms

![](<../.gitbook/assets/Unknown image (784)>)

### Cisco Optical Network Controller

**Cisco Optical Network Controller** is a brain of the optical network that serves as the software-defined networking (SDN)-compliant domain controller within the Cisco Optical Management Systems suite (OMS)

CONC has a web user interface with several applications providing complete CRUD (create, read, update and delete) and NMS (Network Management System) services.

As the network scales and becomes more dynamic, Optical Network Controller steps in to provide centralized control and automation. Optical Network Controller enables dynamic network provisioning, performance monitoring, and fault management. It interacts with Cisco Transport Controller to implement real-time changes based on network conditions, ensuring optimal resource utilization and responsiveness to changing demands.

[https://www.cisco.com/c/en/us/products/collateral/optical-networking/network-convergence-system-1000-series/datasheet-c78-2558054.html](https://www.cisco.com/c/en/us/products/collateral/optical-networking/network-convergence-system-1000-series/datasheet-c78-2558054.html)

![Cisco Optical Network Controller 3.1 Configuration Guide - Use Cisco Optical Network Controller \[Cisco Optical Network Controller\] - Cisco](<../.gitbook/assets/Unknown image (785)>)

### Cisco Optical Site Manager (COSM)

Built into NCS1010 (or 1014) controller card

▪ Mechanical Layout & Inventory

▪ Alarm correlation & Notification

▪ Current PM up to last 32 bins

▪ OAM & Configurations

▪ Connection Verification

▪ Loopbacks

▪ PRBS

▪ OTDR

▪ OCM

▪ TCA

▪ Card & Port configs

## Assurance tools

**NetBox** solution for modeling and documenting modern networks. By combining the traditional disciplines of IP address management (IPAM) and datacenter infrastructure management (DCIM) and APIs and extensions, NetBox provides "source of truth" to power network automation

[https://github.com/netbox-community/netbox](https://github.com/netbox-community/netbox)

**ThousandEyes** is a SaaS product that offers network monitoring and diagnostic capabilities to analyze traffic patterns, identify performance, and troubleshoot issues with connectivity

As business traffic increasingly flows through the internet to cloud service providers, corporations often lack comprehensive visibility into the employee access patterns to the shared internal or cloud-based resources, so complete network monitoring is impossible because we cannot influence and monitor network traffic on the Internet

So the Thousdandeyes was designed to address these emerging cloud-based services that businesses rely on

ThousandEyes provides insights through a cloud-based graphical user interface (GUI) dashboard, allowing monitoring of corporate devices and providing data and performance statistics of their connections to the shared resources

ThousandEyes essentially enables corporations to see both inside-generated traffic from the offices and outside-generated traffic from the employees accessing resources from the "outside" internet

Thousandeyes employes **Synthetic monitoring** (proactive monitoring), which is a monitoring technique that is done by using a simulation or scripted recordings of network transactions

Behavioral scripts (or paths) are created to simulate an action or path that a customer or end user would take on a website or cloud application,

Those paths are then continuously monitored at specified intervals to measure overall performance such as response time and availability

Thousandeyes is installed as a plugin or software on the endpoints and corporate primary data center servers to track the activities

ThousandEyes also operates a global network of distributed software agents accessible via the internet.

Organizations can configure tests and measurements to be executed from both their internal software agents and ThousandEyes' distributed agents. These tests provide visibility into the end-to-end performance of networks, including internet routing, ISP performance, and cloud service provider performance.

![](<../.gitbook/assets/Unknown image (924)>)

![ThousandEyes Device Layer Review - RouterFreak](<../.gitbook/assets/Unknown image (925)>)

**IP Fabric** is The lightweight discovery tool utilizing SSH/Telnet/CDP/LLDP to quickly detect the current network state, including detailed data for each address and port.

**Zabbix** is an open-source SNMP-based monitoring tool supporting ICMP, TCP, and UDP

**Grafana** is an open-source visualization and monitoring platform that integrates with various data sources, including databases, time-series databases, and monitoring tools like Zabbix

The platform provides extensive customization options for dashboard design and layout, enabling users to tailor dashboards to their specific monitoring needs

**FlowMon** It is NetFlow/IPFIX-based monitoring tool, analyzyng network traffic in real-time

**Paessler Router Traffic Grapher (PRTG)** monitoring tool supporting SNMP,Netflow,WMI (Windows Management Instrumentation)

**Batfish** is an open source network validation tool that provides correctness guarantees for security, reliability, and compliance by analyzing the configuration of network devices.

Batfish does NOT require direct access to network devices. Nor does it use data plane probes (e.g. ICMP).

You feed Batfish configurations, it supports multiple vendors (e.g. AWS, Cisco, Arista...), and after you query it:

Can all my instances all reach the DNS server?

Does this specific VM have internet access?

Are all BGP sessions in ESTABLISHED state?

To find out more about Batfish visit their official website:

{% embed url="https://www.batfish.org/" %}

## Intent-based Assurance

Networks have become far too complex to manage with traditional service assurance. With 5G rollouts, SD-WAN, SASE, multi-cloud, and IoT, operators are managing millions of devices, links, and virtualized functions. Old models rely on engineers manually deciding where to place sensors, configuring telemetry, and mapping raw metrics (packet loss, jitter, delay) to something that resembles user experience. This doesn’t scale — especially when customers demand predictable end-to-end performance and fast remediation.

**Intent-Based Assurance (IBA)** is the evolution of service assurance to match the intent-based networking model. Instead of configuring probes and KPIs by hand, an operator expresses a service-level intent (for example: “Connect Site A and Site B with 99.9% availability and <50ms latency”). The IBA system then:

Translates that intent into network-level instrumentation — deciding where probes and sensors need to go.

Maps telemetry to KPIs that reflect actual service health and user experience, not just raw counters.

Monitors continuously, using both active probes (synthetic tests) and passive telemetry.

Predicts and alerts when an SLO/SLA is at risk, not just when it’s already violated.

Feeds results back into orchestration or automation systems to remediate issues automatically (closing the loop).

In short: IBA makes service assurance proactive, automated, and scalable, moving away from fragmented monitoring toward a unified, intent-driven system.

### Cisco Provider Connectivity Assurance (PCA) (formerly Accedian Skylight)

**Cisco Provider Connectivity Assurance (PCA)** solves the challenges of fragmented multidomain tools and lack of end-to-end visibility on service quality, and enables differentiated services based on quality of experience (QoE) and enhanced SLAs. Cisco Provider Connectivity Assurance delivers network-wide visibility and precise synthetic network and service testing for high-performance networks. Network and end-to-end service quality is visible in a single pane of glass for efficient operations and troubleshooting. Granular performance metrics from Cisco Provider Connectivity Assurance sensors can be correlated with third party data and combined with machine learning powered analytics for near real-time performance insights.

Designed for communications service provider, webscaler, global enterprise and federal or public sector networks with stringent performance requirements, Cisco Provider Connectivity Assurance enables

proactive service assurance for efficient troubleshooting and exceptional customer experience—all while lowering the cost of operations. Cisco Provider Connectivity Assurance provides continuous visibility of end-to-end network and service quality as well as per-segment visibility, all with microsecond precision performance data that’s needed to automate assurance.

#### How it works

Provider Connectivity Assurance orchestrates and fully automates monitoring and assurance capabilities through the Crosswork Network Automation platform. Crosswork Network Controller and Crosswork Network Services Orchestrator drive automation decisions based on real-time network performance information and proactive alerting. Here’s how it works:

● Assurance Sensors are deployed using Crosswork NSO as part of the automated workflow when new transport layers or VPNs are defined using service intent. Activation testing templates and other automated test sequences are used to activate service assurance.

● NSO triggers these templates using the NETCONF and YANG API to achieve closed-loop automation. This actively verifies that services work after being provisioned by NSO and continue to perform over the service lifecycle.

● Performance data and events can be further analyzed in Provider Connectivity Assurance or can be sent back to Crosswork Network Controller to correlate KPIs with other events coming from the infrastructure. This process supports SLA management, AI/analytics, databus, and end-customer SLA reporting portals.

Proven scalability (already deployed at Tier-1 carriers).

Multi-vendor support with standards-based telemetry.

Strong integration with Cisco’s Crosswork automation suite.

Flexible sensors (software + hardware).

Benchmarking network assurance tool that can be integrated with Cisco CNC and NSO, allowing it to close the loop - meaning that based on the performance it can signal to NSO or CNC to adjust the configuration or path

Patented In-service throughput testing (does not restrict operation)

Assurance Analytics and reporting dashboard/GUI is SaaS in a cloud can be used for both the SP operations to measure the link as well as other tenants like end-user so that they can also access the performance data analysis SLA assurance & QoS monitoring



Solution meant for business critical services/links such as:

Military

Healthcare

DCI

Federal

Tier 1 SP - Main PCA customers

Managed Service Providers

The Provider Connectivity Assurance (PCA) / Performance Assurance Sensors are not service routers, they don’t instantiate L2VPNs or L3VPNs. Their job is to:

Insert test packets into live services

They can tag packets (VLAN, MPLS label, IP header, DSCP, etc.) so the probes follow the same forwarding path as a specific customer service.

Example: send synthetic traffic over a given VLAN in an EVPN instance, or over a specific L3VPN VRF.

Measure KPIs on that service

Latency, jitter, frame loss, throughput.

Both one-time (turn-up tests like Y.1564/RFC 2544) and continuous (Y.1731/TWAMP).

Assurance sensor modules

In-line with service traffic or out of-line in a spare port (with no impact to customer service traffic)

Pri In-line - jeden port do subscribera e.g UNI/CE a druhy do site NNI/PE

Je i Softwarova verze jako container na routeru

![](<../.gitbook/assets/Unknown image (1520)>)

Network flow sensor: A Docker container hosting a software agent on a Cisco Cell Site Router (CSR).

PCA platform: The router monitors the desired interface and sends packets via the Switched Port Analyzer (SPAN) to the network flow sensor. The sensor analyzes and characterizes traffic flows, exporting metrics to the data platform for visualization and analysis.

[https://www.cisco.com/c/en/us/products/collateral/cloud-systems-management/provider-connectivity-assurance-sensors/provider-connect-assurance-user-exp-ds.html](https://www.cisco.com/c/en/us/products/collateral/cloud-systems-management/provider-connectivity-assurance-sensors/provider-connect-assurance-user-exp-ds.html)

#### FAQ

Q: Can Provider Connectivity Assurance monitor systems from other vendors?

A: Yes, the platform is vendor-agnostic and excels in multivendor environments, offering a unified view of network and

service performance. It generates numerous measurements and KPIs that are analyzed to enhance ecosystem insights

and inform actions. The platform can interwork with existing standards-based network elements, such as TWAMP, for

synthetic/active monitoring and can also ingest other data, e.g., Cisco device and infrastructure telemetry from

mechanisms like Model-drive Telemetry (MDT)

Q: How does Cisco Provider Connectivity Assurance interwork with Crosswork suite products such as CNC, NSO and HCO?

A: Cisco's controllers and orchestrators (CNC and NSO) have been integrated with Provider Connectivity Assurance so

that whenever a new service is set up, it is defined from the start of the service lifecycle with assurance in place. We

can then monitor the service 24/7 and flag issues or behaviors according to predefined policies, which can then be

turned into actions to be executed by the controller. These actions could include things such as a change of route, a

change of path, an addition of bandwidth, or a more granular monitoring session, among many other possibilities that

will vary according to the customer's use case.

Q: I already have ThousandEyes (TE) deployed but I am interested in Provider Connectivity Assurance. Can they work

together and if so, how?

A: Yes, Thousand Eyes and Provider Connectivity Assurance can coexist and together provide a more comprehensive

view of any service traversing the access, distribution, and core, extending out to the application in the cloud. Having

that telescopic, macro view that Thousand Eyes provides or the unowned and parts of the owned network, combined

with the microscopic detail that Provider Connectivity Assurance offers of the owned network, delivers a complete endto-end from user to cloud, and everywhere in between visibility perspective.

Cisco boasts a world-class control plane within the Crosswork Network Automation portfolio, which encompasses various solution elements necessary to adjust the configuration and settings of a network to maintain a desired state. Cisco and the Crosswork portfolio has now integrated Provider Connectivity Assurance to establish the feedback loop,

providing the instrumentation and analytics needed to measure the network's actual state and how its services are performing. This feedback is then relayed to Cisco's control plane, allowing it to automatically maintain the desired state in near real-time and continuously, before any impact on network and service quality affects customer experience.

What kinds of closed-loop scenarios and "action" items are you focused on?

A: Congestion scenario: Imagine a connectivity service experiencing congestion. The platform's feedback loop, which

measures the actual state, can detect this and signal back to the Crosswork suite to adjust the Committed Information

Rate (CIR) in near real-time, thus preventing any impact on customer experience without human intervention.

Low latency service scenario: In situations where low latency is required, Provider Connectivity Assurance would

provide real-time feedback to Crosswork about the latency of a specific service on a given circuit. Crosswork could then

make the decision to switch the service from a default latency circuit to one that is optimized for low latency.

Accedian, long known for network performance monitoring, has adapted its Skylight platform to deliver intent-based assurance. Skylight provides visibility across the entire service path — from user device, through access and core, to cloud.

Instrumentation: Skylight uses lightweight software agents (VMs, containers) and specialized hardware probes (like SFP modules with embedded FPGAs) for precision measurement where built-in telemetry is missing.

Service Modeling: Given a high-level service description (endpoints, SLO/SLA targets), Skylight figures out which sensors and tests are needed, deploys them, and automatically generates KPIs and alert thresholds.

Integration & APIs: Skylight integrates with automation systems (notably Cisco Crosswork), exposing REST/gNMI/Kafka APIs so assurance data can flow into orchestration platforms for closed-loop remediation.

Analytics: The platform uses a streaming analytics engine with ML for anomaly detection, correlation, and predictive insights. When end-to-end KPIs can’t be directly measured, Skylight synthesizes them from multiple sources.

User Experience: Operators get dashboards that focus on services and customer experience rather than raw counters, and end-customers can also view their own service health.

The AI/ML stuff mainly does that for example when there is high latency between two sites having like 5 routers in between, the AI is able to consolidate and flag it as one issue as these latency alerts pulled from all the nodes between the sites are related - normally 5 or more tickets would be generated because of that

All the analysis and probing data pulled from the devices are then tied to the metadata created by the user - like creating a region/city and associating the nodes that connecting two regions to the data pulled from the devices - forming one circuit - this allows to create the map of the devices and circuits so that the AI and ML can then correlate future issues and analysis based on the data associated with our metadata

Orchestrator will be integrated into Analytics

On the black belt you have sales/technical/deployment training for this solution

## Network Telemetry

**Telemetry** refers to automated remote collection of data and it's transmission to remote nodes, which uses these data for monitoring and analysis

**Streaming Telemetry** uses a subscription model to identify information sources and destinations, replaing the need for the periodic polling of network elements; instead a continuous stream of the requested data is being send each defined interval

When streaming telemetry is combined with YANG models as a Data Definition Language, it’s known as **Model Driven Telemetry (MDT)**

The data to be streamed is driven through subscription. Subscriptions allow applications to subscribe to updates (automatic and continuous updates) from a YANG datastore, and this subscription enables the publisher to push and, in effect, stream those updates.

### SNMP limitations

With SNMP, all requested data must be edited and sent at once - with Push based, the sending of individual data can be spread out, which reduces the load on the network and devices

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

## Network performance tools

A loopback in networking is when traffic sent into a device or interface is sent back out again, allowing tests of connectivity, throughput, or troubleshooting without needing a remote peer.

Selective Traffic Loopback means not all traffic is looped, only traffic that matches certain filters.

One way monitoring is better than two way (RTT), because with RTT you dont know in which direction you have a issue

### Active measurement / test protocols

TWAMP (RFC 5357) – Measures one-way/two-way delay, jitter, packet loss; used for end-to-end SLA enforcement.

ETH-DM (Ethernet Delay Measurement, Y.1731 PM) – Ethernet OAM extension to measure delay, delay variation, loss, availability.

ETH-LB (Ethernet Loopback, 802.1ag/Y.1731) – Used for fault isolation and connectivity testing at Ethernet layer.

ICMP Echo (Ping) – Simple test of reachability and RTT (round-trip delay).

UDP Echo – Sends UDP packets to test connectivity, latency, loss where ICMP may be filtered.

ETH-VS (Ethernet Vendor Specific) – Proprietary Ethernet OAM extensions; vendor-specific features.

TCP Throughput (RFC 6349) – Measures end-to-end TCP performance (throughput, efficiency, buffer delay).

Traceroute – Identifies path/hops and per-hop latency; useful for path visibility and fault localization.

#### Benchmarking and service activation

RFC 2544 – Lab/device benchmarking: throughput, latency, frame loss, back-to-back frames. Not suited for live networks.

Y.1564 (Ethernet Service Activation) – Service provider turn-up & SLA validation; verifies CIR/EIR, multi-service testing, sustained load.

### Operations, administration, and maintenance (OAM)

IEEE 802.1ag CFM – Ethernet OAM for fault detection: continuity check, loopback, linktrace.

ITU-T Y.1731 – Extends 802.1ag: adds performance monitoring (delay, jitter, loss, availability).

IEEE 802.3ah EFM OAM – First-mile OAM; monitors access link health, remote failure indication.

#### Other monitoring and management

IP SLA (Cisco proprietary) – Active measurement: latency, jitter, packet loss, MOS (voice QoS).

BFD (RFC 5880) – Fast fault detection (<50 ms) for routing adjacencies and links.

SNMP/RMON – Passive stats collection: interfaces, errors, utilization, traffic.

NETCONF/YANG Telemetry – Real-time, model-driven streaming of network state and performance for analytics.

#### Quick use-case mapping

Lab/benchmarking: RFC 2544.

Service activation & SLA: Y.1564, TWAMP, Y.1731 PM.

Fault management: 802.1ag CFM, ETH-LB, BFD.

Basic reachability: ICMP/UDP Echo, Traceroute.

Last mile monitoring: 802.3ah.

Enterprise monitoring: IP SLA, SNMP, Telemetry.

TCP performance validation: RFC 6349.

### E-OAM

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
