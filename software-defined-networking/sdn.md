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

# SDN

### Overview

Traditional networks comprise several devices (for example, routers, switches, and WLCs) that are equipped with software and networking functionality:

The **data (or forwarding) plane** is responsible for the forwarding of data through a network device.

The **control plane** is responsible for controlling the forwarding tables that the data plane uses.

The **management plane** is integrated into the control plane.

Each device has a control and data plane. This means that all devices are equally smart and can make decisions on their own, since the control plane exists. Of course, the data plane is what is responsible for the actual packet forwarding. This network is now referred to as the traditional network, and is still the dominant network type deployed.

The data plane acts on the forwarding decisions.

The control and management planes learn/compute forwarding decisions.

The following trends in traditional networks present significant challenges:

There are more users and endpoints, therefore, more VLANs and subnets. It becomes more difficult to keep track of all those groups and segment them.

There are so many different types of users coming into the network that it is becoming more complex to configure. Multiple steps are required to give users credentials and support connectivity choices.

As users and devices move around the network, policy is not consistent. As a result, it is difficult to find users when they move around and troubleshoot issues.

With SDN, the network changes:

The control (and management) plane becomes centralized.

Physical devices retain data plane functions only.

### Software-Defined Networking (SDN)

**Software-Defined Networking (SDN)** refers to the set of techniques that are used to manage and change a network’s behavior through an open interface rather than closed-box methods

SDN centralizes control and management plane function into application called **Controller**, which runs on a dedicated device or as a virtual software on a server and acts as the brain of the network

The **controller** is a single pane of glass centralized management platform that serves as unified interface or dashabord to provide comprehensive visibility and control over various systems, applications, or network elements from a single location and interact programmatically with the infrastructure using APIs

The **data plane** are the individual network edge devices on each site, that perform forwarding and WAN connectivity to the individual site

This method decreases complexity, human error, and the time it takes to deliver a new service

With Software-Defined Networking (SDN), you can reduce the complexity of your network by using a standardized network topology and by building an abstract overlay network on top. In this way, you move from a single device view of the network (box-oriented) to a global, high-level view (network-oriented). This high-level view enables you to use abstractions and simplifications when provisioning new services. For example, the network operator configuring a virtual private network (VPN) for a remote office environment is not concerned (and should not be) with the physical layout of the network. The only requirement of the remote site and operator is that the network spans all geographic regions required for the VPN (for example, the Main Campus and Remote Office). The controller will figure out what needs to be provisioned. The prerequisite to this is that the controller is the central point of management and the "source of truth" for the configuration.

Using abstractions when managing your network also enables you to use standardized components. Software-Defined Networking (SDN) implementations typically define a standard architecture and Application Programming Interfaces (APIs) that the network devices use. To a limited degree, you can also swap a network device for a different model or a different vendor altogether. The controller will take care of configuration, but the high-level view of the network for the operators and customers will stay the same.

Simplification of configuration and automated management also directly results in operating expenses (OPEX) savings. Typically, the total cost of ownership (TCO) for a network in a 5-year span comprises about 30 percent capital expenditure (CAPEX) and about 70 percent OPEX. Manual service configuration and activation represent a significant chunk of OPEX.

Controller-based networking makes centralized policy easy to achieve. Networkwide policy can be easily defined and distributed consistently to the devices connected to the controller. For example, instead of attempting to manage access control lists across many individual devices, a flow rule can be defined on the central controller and pushed down to all the forwarding devices as part of the normal operations.

Compared to traditional networking, controller-based networking makes it easy to define special treatment for specific network traffic. Instead of adding complexity to the network through advanced mechanisms like policy-based routing, a traffic flow rule can be defined on the controller and pushed down to all the forwarding devices as part of normal operations. The largest benefit here is that there is a device, a controller, that has a unified view of the network in one location.

This single point of administration addresses the scalability problem where administrators are no longer required to touch each individual device to make changes to the environment. This concept is also not new as controllers have also been around for many years and used for campus wireless networking. Similar to the behavior between a Cisco Wireless LAN Controller (WLC) and its managed access points (APs), the controller provides a single point to define business intent or policy, reducing overall complexity through the consistent application of intent or policy to all devices that fall within the management domain of the controllers. For example, think about how easy it is to enable authentication, authorization, and accounting (AAA) for wireless clients using a WLC, compared to enabling AAA for wired clients (where you would need AAA changes on every switch if you are not using a controller).

With automated processes, the time to provision a new service or implement a change request is drastically reduced. What would previously take days or weeks to implement can be automated to run in hours, along with testing and verification. Lifecycle management is another important step in the automation process—from design and installation of the infrastructure components (day 0) to service enablement (day 1) to management and operations (day 2). Also, after the customer no longer needs the service, the resources that are used must be deallocated and the configuration of the devices must be cleaned up. Even with proper change management procedures, this process is tedious at best if performed manually. If the process is fully automated, you can make sure that the same configuration changes that were applied when provisioning the new service will be removed when it is deprovisioned.

The hybrid SDN option is a combination of the best of both schemas. In a hybrid SDN, the controller becomes an active part of the distributed network control plane, rather than a means to configure the network control plane behavior in devices. This solution also offers a centralized view of the network, giving an SDN controller the ability to act as the brain of the network.

### SDN layers

**Infrastructure layer:** Contains network elements (any physical or virtual device that deals with traffic).

**Control layer:** Represents the core layer of the SDN architecture. It contains SDN controllers, which provide centralized control of the devices in the data plane.

**Application layer:** Contains the SDN applications, which communicate network requirements towards the controller.

![](<../.gitbook/assets/Unknown image (994)>)

#### Northbound API (NBI)

**Northbound API (NBI)** is used to communicate between SDN controller and its management software (Network management system (NMS)) via REST APIs to provide an abstracted network view to upstream applications in the application layer.

#### Southbound API (SBI)

**Southbound API (SBI)** is used to communicate between the SDN controller and network devices via the NETCONF and RESTCONF or Openflow or APIs to control individual devices in the infrastructure layer

![](<../.gitbook/assets/Unknown image (995)>)

![](<../.gitbook/assets/Unknown image (996)>)

#### Integration API (westbound)

**Integration API (westbound)** in the context of Cisco DNA Center refers to an API that enables the system to publish network data, events, and notifications to external systems, as well as to consume information from connected third-party applications. In contrast, a Northbound API is typically used to allow external applications to retrieve network insights and interact with Cisco DNA Center

### Intent-based networking

**Intent-based networking** transforms a hardware-centric, manual network into a controller-led network that captures business intent and translates it into policies that can be automated and applied consistently across the network to help assure desired business outcomes

SDN is a foundational building block of intent-based networking. The good news for SDN practitioners is that intent-based networking addresses shortfalls of SDN. For example, the automated translation of business policies to IT (security and compliance) policies, automated deployment of these policies, and the assurance that if the network is not providing the requested policies, they will receive proactive notification. Intent-based networking adds context, learning, and assurance capabilities, by tightly coupling policy with intent. The "intent" enables the expression of both business purpose and network context through abstractions, which are then translated to achieve the desired outcome for network management. SDN is purposely focused on instantiating change in network functions.

The translation element enables the operator to focus on what they want to accomplish, and not how they want to accomplish it. The translation element takes the desired intent and translates it to associated network policies and security policies. Before applying these new policies, the system checks if these policies are consistent with the already deployed policies or if they will cause any inconsistencies.

Once approved, the new policies are then activated (automatically deployed across the network).

With assurance, an intent-based network performs continuous verification that the network is operating as intended. Any discrepancies are identified; a root-cause analysis can recommend fixes to the network operator. The operator can then "accept" the recommended fixes to be automatically applied, before another cycle of verification. The assurance does not occur at discrete times in an intent-based network. Continuous verification is essential since the state of the network is constantly changing. Continuous verification assures network performance and reliability.

Software-Defined Networking (SDN) allows network engineers to provision, manage, and program networks more rapidly, as it greatly simplifies automation tasks by providing a single point of administration for the programming of the infrastructure.

### Network programmability

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

### Zero-Touch Provisioning (ZTP)

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
