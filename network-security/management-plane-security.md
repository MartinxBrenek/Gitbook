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

# Management Plane Security

### Securing the management plane

#### In-band management

**In-band management** uses the same network infrastructure as user data traffic to connect to devices via SSH or Telnet.

If the network itself is experiencing issues (congestion, outages), it can also affect the ability to manage devices using in-band connections

In-band management traffic travels on the same path as user data, potentially exposing it to security threats if not properly protected

An **in-band management interface** is a Cisco IOS XR Software physical or logical interface that processes management packets, and data-forwarding packets. An in-band management interface is also called a shared management interface.

#### Out-of-band management (OOBM)

**Out-of-band management (OOBM)** utilizes a separate physical connection specifically for management purposes. This can be a serial console port, a dedicated network management interface, or even a cellular connection.

OOB offers a more reliable way to manage devices even if the primary network is down or congested

Since OOBM uses a separate path, it provides better isolation from potential security threats on the main network

**OOB** refers to an interface that allows only the management protocol traffic to be forwarded or processed. An **OOB management interface** is defined by the network operator to specifically receive network management traffic. Management ports are OOB by default.

### Management Plane Protection (MPP)

**Management Plane Protection (MPP)** is a feature in Cisco IOS XR Software that restricts which interfaces can receive management packets. It lets you reserve a set of interfaces for management traffic, exclusively or shared with data. MPP changes the default interface behavior by not listening to all service traffic and allowing only explicitly configured management applications.

Before enabling the MPP feature you should understand the restrictions for implementing MPP:

Currently, MPP does not keep track of the denied or dropped protocol requests.

MPP configuration does not enable the protocol services. MPP is responsible only for making the services available on different interfaces. The protocols are enabled explicitly.

Management requests that are received on in-band interfaces are not necessarily acknowledged there.

Both the route processor (RP) and distributed route processor (DRP) Ethernet interfaces are by default out-of-band (OOB) interfaces and can be configured under MPP.

The changes made for the MPP configuration do not affect the active sessions that are established before the changes.

Currently, MPP controls only the incoming management requests for protocols, such as TFTP, Telnet, SNMP, SSH, NETCONF, and HTTP/HTTPS.

MPP does not support MIB.

In a Multiprotocol Label Switching (MPLS) VPN, the Virtual Routing and Forwarding (VRF) is globally configured for the ingress interface of the Provider Edge (PE) router. MPP applies the VRF on the interface filters. When an incoming packet from the core interface has a different VRF, then the MPP does not work in MPLS VPN.

The MPP protection feature, and all the management protocols under MPP, are disabled by default. When you configure an interface as either OOB or in-band, it automatically enables MPP. Therefore, this enablement extends to all the protocols under MPP.

Implementing the MPP feature provides these benefits:

Greater access control for managing a device than allowing management protocols on all interfaces.

Improved performance for data packets on non-management interfaces.

Simplifies the task of using per-interface access control lists (ACLs) to restrict management access to the device.

Fewer ACLs are needed to restrict access to the device.

Prevention of packet floods on switching and routing interfaces from reaching the CPU.

#### In-band or Out-of-band interface configuration example

![](<../.gitbook/assets/Unknown image (776)>)

### Securing access to privileged EXEC mode (IOS-XE)

You can secure a router or a switch by using passwords to restrict access. Using passwords and assigning privilege levels is a way to provide terminal access control in a network. It is a form of management plane hardening. You can establish passwords on individual lines, such as the console, and to the privileged EXEC mode.

Keep in mind that passwords are always case-sensitive.

**Privilege levels** enable you to control access to network devices and the services that are available when access is granted.

**Console port (cty)** #line con 0 Used for local system access using a console terminal

**Auxiliary port (aux)** #line aux 0 Used for remote access into the device through a modem

**Virtual terminal (vty)** #line vty 0 4 Virtual interfaces mediating remote Telnet and SSH connections into the device

**Password Type 4 up to 7** should not be used due to prone cracking the weak encryption used - use Type 8 or 9 where possible

**Type 0** passwords are not encrypted and shouldn't be used ( enable password )

**Type 5** MD5 hashing algorithm is used to encrypt, use whenever possible ( enable secret )

**Type 7** uses a Cisco proprietary Vigenere cypher encryption algorithm, easily cracked and shouldn't be used - use keyword secret instead of password to avoid using type 7

**Type 8** Password-Based Key Derivation Function 2 (PBKDF2) with a SHA-256 hashed secret

**Type 9** SCRYPT hashing algorithm, providing enhanced protection against brute-force attacks - enabled by using secret keyword

**Note:** Username-based authentication with password is recommended only as a fallback to an AAA authentication

| username <> secret <> | if you use password instead of secret you would have to specify the password in a given hash, or if you specify it as plain text it will show like that in running config which is undesirable, so always use secret in order to encrypt the plain text password for a username |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

![](<../.gitbook/assets/Unknown image (777)>)

You can also add a further layer of security to any plaintext passwords in your configuration, which is particularly useful when the configuration is viewed, or when it is stored elsewhere, such as on a TFTP server.

| service password-encryption | encrypts plain text passwords in #sh run. However, note that service password encryption uses type-7 obfuscation, which is not very secure. There are several tools and web pages available that convert a type-7 protected password into a plaintext string. |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Privilege levels and role-based access control (RBAC)

**Privileged** - having special rights, advantages

**Privilege level 0** Includes the disable, enable, exit, help, and logout commands

**Privilege level 1 (default)** in user EXEC mode (R1>). Configure terminal is not availableAdditional privilege levels ranging from 2 to 14 can also be configured to provide customized access control

**Privilege level 15** highest privileged EXEC mode (R1#), where all CLI show and configure commands are available

| username NOC privilege <> algorithm-type scrypt secret cisco123                                                                                                                                     | username <> algorithm-type {md5 \| sha256 \| scrypt} secret {password}                                                                                                                                                                     |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| privilege exec level 5 configure terminal privilege configure level 5 interface privilege interface level 5 shutdown privilege interface level 5 no shutdown privilege interface level 5 ip address | Allow those commands to be executed when logged as level 5 configured in global config mode                                                                                                                                                |
| show privilege                                                                                                                                                                                      |                                                                                                                                                                                                                                            |
| line vty                                                                                                                                                                                            | enters line config mode. range can be specified - Example: line vty 0 15 - configuring lines from 0 to 15 If there is no need for more than five vty lines, you can configure only 0-4                                                     |
| login local                                                                                                                                                                                         | enable local password authentication; use login password to specify the password for logging via telnet/ssh                                                                                                                                |
| transport input { telnet \| ssh \| all \| none }                                                                                                                                                    | specifies which protocol to be used for inbound connections to the VTY line of the device, only ssh recommended                                                                                                                            |
| exec-timeout {minutes} {seconds}                                                                                                                                                                    | terminates an EXEC session after specified timeout of inactivity - default is 10minutes                                                                                                                                                    |
| absolute-timeout 10                                                                                                                                                                                 | terminates session even if the connection is being used at the time of termination                                                                                                                                                         |
| logout-warning 20                                                                                                                                                                                   | sets the warning before logging out of terminal                                                                                                                                                                                            |
| access-class \<ACL\_num> in                                                                                                                                                                         | pre-defined ACL to be defined to restrict access to inbound VTY connections                                                                                                                                                                |
| line console 0                                                                                                                                                                                      | enters console line config                                                                                                                                                                                                                 |
| transport output ssh                                                                                                                                                                                | Configure the line console 0 to only allow outbound ssh connections initiated from the console 0 interface Outgoing connections typically refer to a scenario where a device initiates a connection to another device using Telnet or SSH. |
| transport input none                                                                                                                                                                                | block any inbound connection or reverse Telnet into console port                                                                                                                                                                           |
| privilege level 15                                                                                                                                                                                  | Sets the privilege level for the console line to 15                                                                                                                                                                                        |
| no exec                                                                                                                                                                                             | users won't be able to establish incoming connections to that line, and even if a connection is established, they won't be able to execute commands                                                                                        |
| login local                                                                                                                                                                                         | refrain to use password as the login to the console - instead use login loca to use Type 8 or 9 password                                                                                                                                   |
| show line                                                                                                                                                                                           |                                                                                                                                                                                                                                            |

Remember to always configure a password when using the login command or a username and password when using the login local command, before closing the console session. Entering only the login or login local command in the configuration of the console line will result in the console terminal being inaccessible if other methods of accessing the device were not configured.

### Authentication, Authorization, and Accounting (AAA)

In a small network, local authentication is often used. When you have more than a few user accounts in a local device database, managing those user accounts becomes more complex. For example, if you have 100 network devices, adding one user account means that you have to add this user account on all 100 devices in the network. Also, when you add one network device to the network, you have to add all user accounts to the local device database to enable all users to access that device

**Authentication, Authorization, and Accounting (AAA)** refers to a security architecture for distributed systems that controls who can access which services and tracks resource usage. It is a framework that supports and provides these services:

**Authentication** is the process of verifying and confirming a user's identity before granting access to the network. There are several ways to identify user, the simplest and most straightforward being a username and password dialog. Among more advanced methods, the challenge and response method provides authentication and increased security, because it does not exchange user identification data across the connection. Users can authenticate using certificates as well—using something they have (the certificate) and something that they know (the PIN or other passphrase)

**Authorization** defines the access privileges and restrictions to be enforced for an authenticated user

**Accounting** collects information about the user activity and resource consumption. It allows you to log user logins, commands executed by the user, session durations, bytes transferred, and so on. This information can then be used for billing, auditing and reporting purposes

AAA provide the primary framework through which you set up access control on your routers and switches. AAA services provide a higher degree of scalability than the line-level and privileged EXEC authentication commands alone

When AAA accounting is activated, the network devices report all user activities to the RADIUS or TACACS+ security server, depending on the security method implemented.

AAA uses protocols, such as RADIUS, TACACS+, or Kerberos, to handle security functions. AAA is meant as the primary and recommended method for access control on Cisco devices. However,

Cisco IOS Software and Cisco IOS XE Software have additional features for simple access control, in case you do not need or want to configure AAA:

Local username authentication

Console and line password authentication

Enable password authentication

The problem with local implementations of AAA is that they do not scale well. Service provider environments have multiple Cisco routers. Maintaining local databases for each Cisco router for this size of network is not feasible. One or more AAA servers such as Cisco Identity Services Engine (ISE) systems (servers or engines) can manage the entire user and administrative access needs for an entire corporate network using one or more databases.

![](<../.gitbook/assets/Unknown image (778)>)

#### Remote Authentication Dial-In User Service (RADIUS)

**RADIUS** is an open IETF standard AAA protocol for applications such as network access or IP mobility that was developed by Livingston Enterprises. RADIUS works in both local and roaming situations and is commonly used for accounting purposes.

The RADIUS protocol hides passwords during transmission between the router and RADIUS server, even with Password Authentication Protocol (PAP), using a rather complex operation that involves Message Digest 5 (MD5) hashing and a shared secret. However, the rest of the packet is sent in plaintext.

RADIUS combines authentication and authorization as one process. After users are authenticated, they are authorized as well. RADIUS uses UDP ports 1645 or 1812 for authentication and UDP ports 1646 or 1813 for accounting.

Cisco use UDP port 1645 for authentication and authorization and 1646 for accounting. Industry standard use UDP 1812 for authentication and 1813 for accounting

Supports EAP for .1x authentication and is used primarily for secure network access, while TACACS for network device access control

#### Terminal Access Controller Access-Control System Plus (TACACS+)

**TACACS+** is a Cisco enhancement to the original TACACS protocol. Despite its name, TACACS+ is an entirely new protocol that is incompatible with any previous version of TACACS. TACACS+ is defined in RFC 8907.

TACACS+ provides separate AAA services. Because TACACS+ separates authentication and authorization, it is possible to use TACACS+ for authorization and accounting while using another method of authentication. The extensions to the TACACS+ protocol provide more types of authentication requests and response codes than were in the original specification. TACACS+ offers multiprotocol support, such as IP. Normal TACACS+ operation encrypts the entire body of the packet for more secure communications and utilizes TCP port 49.
