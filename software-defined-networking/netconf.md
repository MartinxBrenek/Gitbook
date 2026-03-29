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

# NETCONF

### Network Configuration Protocol (NETCONF)

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

To test with python script:

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
