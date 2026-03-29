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

# DNS

## Overview

**Domain Name System (DNS)** is a decentralized naming system for computers, services, or any resource connected to the Internet or a private network.

It translates human-readable domain names, also called **Fully Qualified Domain Names (FQDNs)**, like `google.com`, into IP addresses.

For IPv4, hostname-to-address mapping is an `A` record.

For IPv6, hostname-to-address mapping is an `AAAA` record.

Without DNS, you would have to remember the IP address of every host you want to reach.

DNS uses a distributed database hosted on several servers around the world to resolve names associated with IP addresses.

The DNS protocol defines an automated service that matches resource names with numeric network addresses.

### DNS hierarchy

DNS operates in a hierarchy. Root servers delegate to TLD servers, which delegate to authoritative servers. This system ensures that no single server needs to store every domain name/IP address pair, improving scalability and reliability.

**Top-level domain (TLD)** is the topmost domain in the hierarchical DNS of the Internet, or simply the last part of a domain name, such as `.com`, `.cz`, or `.org`.

### DNS namespace

**DNS namespace** represents all names present in the DNS system. Names, such as [www.google.com](http://www.google.com/), are referred to as DNS domains (domain names). DNS domain names are organized hierarchically in a multi-level architecture.

The architecture begins with a root name, which is common to all names. The root name then branches into multiple branches, with each branch starting with a different top-level domain name.

Each domain can have subdomains, which in turn can have subdomains of their own.

The complete name of the resource follows this structuring. Each level has its own name and the complete domain name is composed by aggregating names of all levels.

### Domain registration

**Domain name registration** assigns a specific domain name to a particular resource and sets up relationships between names and addresses. The name registration process ensures that each name is unique.

To ensure uniqueness, name registration is regulated and supervised. The Internet Corporation for Assigned Names and Numbers (ICANN) operates the internet’s DNS. The registration process itself is not centralized but is shared among multiple authorized entities, called registries and registrars. Within DNS registration, authorized entities record assigned domain names and corresponding data into a database. DNS database is distributed among these different authorities. At the same time, the DNS information is stored and made available on DNS servers.

### Domain name resolution

**Domain name resolution** is the process in which a resource name is resolved into a resource IP address. Domain name resolution happens on devices that are connected to the internet hundreds or thousands of times a day.

Users are usually unaware of the name resolution process. The process is integrated into the client programs, such as web browsers, email clients, or FTP clients. The clients accept domain name input by end users, use it to create name resolution requests, and send the requests to servers. Servers, on the other end, accept name resolution requests, look up the answers, and send the answers to the clients.

### Resolution process

In DNS, the resolution is implemented as a client and server process. It involves several entities of the system—DNS client, DNS servers of various types, and DNS distributed database. Clients and servers communicate by using the DNS protocol. The name resolution relies heavily on the information provided in the registration process. The process of resolution is also closely related to how the name space is structured. To find the information, the name resolution process resolves the given domain name one level at a time, until the final level is reached.

1. When a host types [www.google.com](http://www.google.com/) to the browser to access the internet web-service, it first performs a lookup into its cache memory, which is found in windows by typing in cmd ipconfig/displaydns
2. If no record found, it proceeds to send a query to the DNS server
3. The DNS server performs a lookup into it's database to find A record (IPv4-to-hostname), if no record found it initiates Iterative query (asking another DNS server) to other DNS servers in a following order: root domain servers, top-level domain servers and authoritative servers until the IP address is obtained
4. If a record is found it replies with a message containing the IP-to-hostname record back to the host. The host then proceeds to save this record into its own cache, so it can initiate subsequent communications, without querying the DNS server

Note if you want to manually add an DNS entry to the windows host: [How to Edit Hosts File on Windows](https://phoenixnap.com/kb/windows-hosts-file)

![](<../.gitbook/assets/Unknown image (1511)>)

![](<../.gitbook/assets/Unknown image (1512)>)

![](<../.gitbook/assets/Unknown image (1513)>)

### Common DNS record types

DNS doesn’t just handle domain-to-IP mapping.

#### Common records

* `A`: hostname → IPv4 address
* `AAAA`: hostname → IPv6 address
* `CNAME`: alias one name to another
* `MX`: mail exchanger for a domain
* `PTR`: IP address → hostname (reverse DNS)

#### Reverse DNS (PTR)

**PTR records** translate an IP address to a domain name.

IPv4 PTR records are stored under the reversed IP, plus `.in-addr.arpa`.

For example, the PTR record for the IP address `192.0.2.255` would be stored under `255.2.0.192.in-addr.arpa`.

`.in-addr.arpa` is used because PTR records are stored within the `.arpa` top-level domain in DNS. `.arpa` is a domain used mostly for managing network infrastructure, and it was the first top-level domain name defined for the Internet.

#### DNS server roles

Root servers: know about top-level domains (TLDs) like `.com`.

TLD servers: know about second-level domains (SLDs) like `networkchuck.com`.

Authoritative servers: return the IP address for the specific domain or subdomain (like `academy.networkchuck.com`).

Recursive DNS servers (like Google’s public DNS) help by querying other DNS servers to retrieve the needed IP address. They may also cache the results to respond more quickly to future requests.

**Reverse DNS lookup** is a DNS query for the domain name associated with a given IP address.

### DNS in Cisco IOS

| ip name-server | to configure DNS severs; multiple DNS servers can be specified in one line |
| -------------- | -------------------------------------------------------------------------- |
| ip dns server  | enables dns on cisco device                                                |
| show hosts     | to view dns-related database                                               |

The no ip domain lookup command is usually seen in configurations. By default, any single word entered on a command line that is not recognized as a valid command is considered as a hostname by the router, and the router will by default try to telnet to that hostname. This is extremely annoying, especially when you do a simple typo, as the router will try to translate that typo into an IP address. If you do not have a DNS server configured, the command line will stall for several seconds until the DNS request times out.

Quite frankly, it does not make much sense to have both ip name-server and no ip domain lookup configured. The no ip domain lookup tells the router to stop interacting with any DNS servers entirely. Having a DNS server configured is then a useless thing because it is not going to be used, anyway.

What could be considered a more proper way of doing things, however, is this: Have the DNS server configured using the ip name-server command, and at the same time, on all lines (con 0, aux 0, vty 0 15), deactivate the automatic action of telnetting into all "words" that look like hostnames:

| line con 0 transport preferred none line aux 0 transport preferred none line vty 0 15 transport preferred none |                                                                                                                                                                                                             |
| -------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| no ip domain-lookup                                                                                            | disable IP Domain Name System hostname translation, which improves the show command response time (when you mistype any word or something in CLI and press enter, the system won't try to resolve the word) |

### Windows DNS commands

| ipconfig /flushdns       | Flushes the DNS resolver cache, which can be useful for clearing cached DNS records.                                                                                                                                                                                                                                                                         |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| nslookup \<domain\_name> | An easy way to observe DNS in action can be performed in a command window in Microsoft Windows, Apple Mac OS X, or your favorite Linux distribution. When the command window is open, enter nslookup [www.google.com](http://www.google.com/). This command queries DNS to resolve the domain name into IP address. The result will appear below your query. |

### Packet capture examples

#### Host-to-local DNS (pcap)

![](<../.gitbook/assets/Unknown image (1514)>)

IPv4 A record response

![](<../.gitbook/assets/Unknown image (1515)>)

IPv6 AAAA record response

![](<../.gitbook/assets/Unknown image (1516)>)

### DNS security

Standard DNS queries are unencrypted and vulnerable to attacks, such as DNS spoofing or hijacking, where attackers intercept or manipulate DNS traffic. This can redirect users to malicious websites.

**DNS over HTTPS (DoH)** encrypts DNS queries by tunneling them through an HTTPS connection, which makes it harder for third parties to intercept or modify the queries.

Instead of sending DNS queries over plaintext UDP port 53, DoH wraps DNS queries in HTTPS and sends them over port 443 (the same port used for secure web traffic). This makes it indistinguishable from normal web traffic, providing both encryption and obfuscation.

**DNS over TLS (DoT)** is another method that encrypts DNS queries, but it does so by using TLS (Transport Layer Security) rather than HTTPS.

DoT wraps DNS queries in TLS encryption and sends them over a dedicated port (port 853). Like DoH, it ensures DNS queries are encrypted but doesn’t use the same obfuscation techniques as DoH.

**DNSCrypt** is another protocol that encrypts DNS queries between the user's device and the resolver.

DNSCrypt uses elliptic-curve cryptography to encrypt DNS queries, ensuring that no one can eavesdrop or modify the DNS traffic.

**DNS-based Authentication of Named Entities (DANE)** builds on DNSSEC to provide a way of verifying TLS certificates through DNS.

DANE allows domain owners to publish their TLS certificates in DNS using DNSSEC. When a client connects to a service, it can use DNSSEC to verify that the certificate presented by the server matches the one published in DNS.

DANE depends on DNSSEC, so it shares the same challenges of DNSSEC adoption and complexity.

#### DNSSEC (Domain Name System Security Extensions)

**DNSSEC (Domain Name System Security Extensions)** is an extension of the Domain Name System (DNS) that increases its security. DNSSEC provides users with the assurance that the information they obtain from the DNS was provided by the correct source, is complete, and its integrity has not been compromised in transit. DNSSEC ensures the trustworthiness of data obtained from the DNS

If the DNS service is not secured with DNSSEC, it provides a potential attacker with several places where it is possible to disrupt communication and falsify data. By changing domain name data, the attacker will affect the functioning of other Internet services, which he can abuse with this intervention.

If someone manages to spoof a numeric address, the user will unknowingly end up in a completely different location and will not connect to the service they were expecting.

The attacker can then, for example:

obtain other people's e-mails

obtain passwords, access codes or payment card details, etc. using fake websites

bypass anti-spam protection in DNS and send spam

forge messages and information on websites

redirect or eavesdrop on telephone calls made over the Internet.

In most cases, the user has no chance of knowing that something malicious is happening. Thanks to the implementation of DNSSEC, the user will gain confidence that the information he obtained from the DNS was provided by the correct source, is complete and its integrity was not compromised during transmission. DNSSEC ensures the credibility of the data obtained from the DNS.

#### Operation (high-level)

DNSSEC introduces DNS asymmetric cryptography – i.e. using one key to encrypt and another key to decrypt the content. A similar principle is the basis of the better-known message encryption using PGP or signing emails with an electronic signature. In the case of DNSSEC, the domain holder generates a pair of private and public keys. He then uses his private key to electronically sign the technical data about his domain that he enters into the DNS. The public key can then be used to verify the authenticity of this signature. To make this key available to everyone, the holder publishes it for his domain with a superior authority, which for all .cz domains is the .cz domain registry. At the .cz domain registry level, technical data in the DNS is also signed and the public key for this signature is again passed on by the registry administrator to the superior authority. This creates a chain that ensures the credibility of the data, as long as none of its elements are broken and all electronic signatures agree.

### DNS in IPv6

The core concept of DNS is unchanged from IPv4

1. In a dual-stack case, an IPv4-enabled and IPv6-enabled application query the DNS to resolve an domain name.
2. The DNS sends the query response containing all available records.
3. The application on the host will connect to the supported and preferred IP address (IPv6 is by default preferred in new applications). If the connection fails, it will attempt to connect to the other IP version.

DNS server for IPv6 maintains mapping between IPv6 address and a hostname – called “AAAA” record (4 times longer than IPv4)

This means that it is possible to use IPv4 as the network protocol to connect to a DNS server and resolve an IPv6 (AAAA) record

IPv6 reverse maps use a sequence of nibbles separated by dots with the suffix “.IP6.ARPA” as defined in RFC 3596.

For example, the reverse lookup domain name corresponding to the address 2001:db8:1234:1a00:1:2:3:4 would be 4.0.0.0.3.0.0.0.2.0.0.0.1.0.0.0.0.0.a.1.4.3.2.1.8.b.d.0.1.0.0.2.ip6.arpa.

Note If Windows does not obtain any DNS server IPv6 addresses, it configures default legacy site-local addresses fec0:0:0:ffff::1, fec0:0:0:ffff::2, and fec0:0:0:ffff::3, similar to how APIPA works for IPv4

![](<../.gitbook/assets/Unknown image (1517)>)

DNS64

![](<../.gitbook/assets/Unknown image (1518)>)

### Multicast DNS (mDNS)

**Multicast DNS (mDNS)** is a service that allows devices on a local network to automatically discover each other and resolve hostnames to IP addresses without requiring a traditional Domain Name System (DNS) server.

### Related concepts

#### Proxy servers

**Proxy servers** are an intermediary server between a client (app or web browser) and another server

When a client requests a resource (web page or file) from another server, the request is first sent to the proxy server, which then forwards the request to the target server on behalf of the client.

The target server responds to the request, and the response is sent back to the proxy server, which then relays the response to the client.

Reverse Proxy Protect servers instead. So the client doesn't know to which server it is connected to

Benefits

Improving performance: By caching frequently accessed resources, a proxy server can reduce the amount of network traffic and improve the speed of access to resources.

Filtering content: Proxy servers can be configured to filter content based on various criteria, such as IP address, domain name, URL, or content type.

This can be used to block access to specific websites, restrict access to certain types of content, or prevent access to malicious sites.

Anonymizing requests: Proxy servers can be used to hide the client's IP and other identifying information

Load balancing: Proxy servers can distribute incoming requests across multiple servers, which can improve performance and ensure high availability.

#### Load balancers (LB)

**Load balancers (LB)** distribute network traffic across multiple servers or resources to ensure optimal resource utilization, high availability, and improved performance. Load balancers are commonly used in web applications, database clusters, and Content delivery networks (CDNs).

Vendors: Cisco, F5 Networks, Citrix, and Nginx are some popular vendors offering load balancing solutions.

![](<../.gitbook/assets/Unknown image (1519)>)

#### Application Layer Gateways (ALG)

**Application Layer Gateways (ALG)** are specialized network devices or software modules that operate at the application layer of the OSI model. They intercept and modify application-specific traffic to provide various functionalities, such as:

NAT (Network Address Translation): Translating private IP addresses to public IP addresses and vice versa.

Firewalling: Blocking or allowing specific types of traffic based on rules.

Quality of Service (QoS): Prioritizing or limiting certain types of traffic.

Application-Specific Features: Providing features tailored to specific applications (e.g., FTP, HTTP, VoIP).

#### Chromecast (example)

**Chromecast** is a small device developed by Google that lets you stream media (like videos, music, or photos) from your phone, tablet, or computer to a TV or speakers. You plug it into your TV's HDMI port, connect it to your Wi-Fi, and then "cast" content to it using apps like YouTube, Netflix, or Spotify. It supports mDNS (Bonjour) for device discovery.

#### mDNS (Bonjour) across VLANs

The configuration enables device discovery across different network segments (VLANs) using Cisco routers or switches. Normally, devices like printers, Chromecasts, or Apple AirPlay devices use mDNS (Multicast DNS)—also known as Bonjour in Apple terms—to find each other on the same network. But mDNS traffic doesn’t naturally cross VLAN boundaries, so devices in VLAN 3 can’t see devices in VLAN 6.

This setup turns a Cisco router or switch into an mDNS gateway, allowing it to forward mDNS traffic between VLANs (e.g., VLAN 3, 6, and 17 in your example). As a result:

A Chromecast in VLAN 3 can be discovered by a phone in VLAN 6.

A printer in VLAN 17 can be seen by a laptop in VLAN 3.

Apple devices using AirPlay can find each other across VLANs.

Eliminates VLAN Barriers: Without this, devices in different VLANs can’t discover each other using mDNS, which is a problem in segmented networks (common in offices, schools, or homes with multiple VLANs for security or organization).

Replaces Clunky Alternatives: The common workaround is to use a Linux VM running Avahi to reflect mDNS traffic, but that’s messy—you need a VM with an interface in each VLAN. The Cisco solution is cleaner and uses existing hardware.

Practical Example: If you’re in an office with a printer in VLAN 3 and employees in VLAN 6, this configuration lets those employees find and print to the printer without needing to be in the same VLAN.

How It Works (Simplified):

The Cisco device listens for mDNS traffic (like “Hey, I’m a printer!”) in each VLAN.

It forwards that traffic to the other VLANs, so devices in different VLANs can “hear” each other.

The redistribute mdns-sd option makes it automatically broadcast mDNS traffic across VLANs, but you need to be careful to avoid loops if another reflector is running.
