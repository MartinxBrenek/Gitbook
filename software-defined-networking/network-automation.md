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

# Network Automation

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

[https://www.batfish.org/](https://www.batfish.org/)
