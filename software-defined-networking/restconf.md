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

# RESTCONF

### Representational State Transfer Configuration Protocol (RESTCONF)

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
