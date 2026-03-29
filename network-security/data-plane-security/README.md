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

# Data Plane Security

### Access Control Lists (ACL)

**Access Control Lists (ACL)** are a tool to filter traffic based on source and destination IP. ACL's are used for Security - to permit or deny access, for NAT - to identify what addresses needs to be translated or QoS - to identify what

packets needs to be prioritized, or even identification of traffic that needs to be encrypted

One of the common applications of ACLs is traffic filtering. By default, a device does not have ACLs configured; therefore, a device does not filter traffic. For instance, traffic that enters the router is routed solely based on information within the routing table. However, when an ACL is applied to an interface, the router performs the additional task of evaluating all network packets against the ACL as they pass through the interface to determine if the packet can be forwarded.

When used for traffic filtering, ACLs provide these features:

Limit network traffic to increase network performance. For example, if a corporate policy does not allow video traffic on the network, ACLs that block video traffic could be configured and applied. This action would greatly reduce the network load and increase network performance.

Provide traffic flow control. For example, ACLs are used for traffic prioritizing or to limit certain type of traffic.

Provide a basic level of security for network access. ACLs can allow one host to access a part of the network and prevent another host from accessing the same part. For example, access to the Human Resources network can be restricted to specific users.

Filter traffic based on traffic type. For example, an ACL can permit email traffic but block all Telnet traffic.

An ACL is a sequential list of permit or deny statements, known as access control entries (ACEs) or ACL statements. When network traffic is processed by an ACL, the device compares packet header information against the ACE matching criteria. ACL statements are evaluated one by one, in a sequential order from the first to the last, to determine if the packet matches one of them. This process is called packet filtering.

The rules that define matching of IPv4 addresses are written using wildcard masks. As with subnet mask and an IPv4 address, a wildcard mask is a string of 32 binary digits. However, a wildcard mask is used by a device to determine which bits of the address to examine for a match.

Packet filtering starts at the top (lowest sequence) and proceeds sequentially down (higher sequence) until a matching pattern is identified

When adding ACL statements to the configuration, Cisco IOS Software automatically numbers each statement. By default, numbering starts with 10 and subsequent numbers are incremented by 10.

Once a match is found, the ACL applies the corresponding permit or deny action to the packet and stops further processing of the rules

Note Implicit deny statement at the bottom ensures that the packets or prefixes are denied by default

#### Inbound ACL (`ip access-group X in`)

Applied to traffic entering an interface from the wire.

Packets are filtered before the routing lookup/before they are routed to an outbound interface

Advantage: prevents unnecessary processing of unwanted packets,drop unwanted packets early, saving CPU and bandwidth.

Commonly used.

Best practice to block traffic as soon as it enters. Example: blocking unnecessary or malicious traffic at the edge.

#### Outbound ACL (`ip access-group X out`)

Applied to traffic leaving an interface.

Packets are routed first, then checked against the ACL before transmission.

Less common, but sometimes necessary.

Outbound ACLs are used when:

You want to control all routed traffic going into a specific segment

You need to filter traffic originated by the router itself. (e.g., management-plane protection).

Inbound placement would be too complex (too many ingress interfaces).

Sometimes used when the same inbound traffic can exit multiple interfaces, and you want to restrict only at one specific egress. Example:

Example with SVI:

| interface Vlan500 description \* \* \* SCADA-LAN \* \* \* ip address 10.88.122.1 255.255.255.192 ip helper-address 10.88.34.7 ip access-group SCADA31122\_OUT out |   |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

If you put an ACL inbound, it filters packets as soon as they enter the router on that interface.

So, if your router has:

VLAN100 (Corporate LAN)

VLAN200 (Engineering LAN)

VLAN300 (Remote Access VPN)

VLAN500 (SCADA LAN)

…and you want to protect SCADA (VLAN500) from all other sources, you’d need to:

Apply inbound ACLs on VLAN100, VLAN200, VLAN300 … each one blocking traffic to VLAN500.

That’s multiple ACLs to maintain.

If instead you put the ACL outbound on VLAN500, it filters packets just before they exit toward SCADA.

Now you only need one ACL on VLAN500.

It doesn’t matter where the traffic came from (Corporate, Engineering, VPN). If it’s trying to reach SCADA, it will be filtered at the single exit point.

It’s the single choke point for traffic entering the SCADA LAN.

Much simpler than configuring inbound ACLs on every other VLAN or WAN interface.

If you apply an ACL inbound on Vlan500 (the SCADA VLAN interface), you are filtering packets coming from the SCADA devices into the router.

Inbound on SCADA = controls what SCADA devices can talk to the router or to other networks.

Outbound on SCADA = controls what traffic is allowed to enter SCADA from anywhere else, because the VLAN500 is the exit interface for the packet

Another example:

The F0/0 interface is the default gateway of the Staff segment. It receives traffic from the Staff segment and forwards it to the Server segment. The F0/0 interface is the entry point of traffic.

The administrator applied the outbound ACL to filter the incoming traffic. The traffic of the Staff segment enters the F0/0 interface. It does not exit the F0/0 interface. An outbound ACL can't filter the incoming traffic.

The administrator can fix this problem in two ways. He can apply an inbound ACL to the F0/0 interface, or he can apply an outbound ACL to the F0/2 interface. The following image shows the first solution.

![](<../../.gitbook/assets/Unknown image (723)>)

#### Wildcard masking (inverse mask)

**Wildcard masking (inverse mask)** is a mask written inversely to a traditional mask:

Subnet mask (normal mask, e.g. 255.255.255.0 = /24):

1 bits = network bits (must match)

0 bits = host bits (don’t care for routing match, but used for host addressing)

Wildcard mask (used in ACLs, OSPF, etc., e.g. 0.0.0.255):

0 = must match

1 = don’t care

This is why a wildcard mask is often called an inverse mask — it flips the logic compared to a subnet mask.

2. Your Example

0.0.0.255 as a wildcard = “match the first 24 bits, ignore the last 8 bits.”

That effectively matches a /24 network.

In a subnet mask, all 1s (network bits) must be contiguous from the left.

After the first 0 appears, no more 1s are allowed.

Example:

Valid subnet mask: 11111111.11111111.11111111.00000000 = 255.255.255.0

Invalid subnet mask: 11111111.11111111.11111101.00000000 = 255.255.253.0

There is no such regularity in wildcard masks—After the first 1 in a wildcard mask, subsequent bits can be either 0s or 1s, with no obligatory contiguity. Therefore, wildcard masks are much more flexible than the subnet masks.

The flexibility available with wildcard masks is to identify subsets, such as odd or even matches.

With wildcard masks you can define criteria for matching all bits of an IPv4 address, or you can define rules that require only parts of the reference address to match. Partial match requirement results in a range of addresses matching the criteria, such as IPv4 addresses of many subnets.

To match the desired range of addresses exactly, sometimes you will have to use more than one ACL statement. For example, to match a range of addresses from 172.16.16.0/24 to 172.16.32.0/24, you should use two entries with the following matching rules: 172.16.16.0 0.0.15.255 and 172.16.32.0 0.0.0.255.

![](<../../.gitbook/assets/Unknown image (724)>)

![](<../../.gitbook/assets/Unknown image (725)>)

#### Infrastructure ACLs (iACLs)

Traffic from customers should only traverse service provider devices and should never be destined to the devices themselves. By implementing ACLs on the edge of the network, the infrastructure of the service provider can be protected by blocking traffic to the router interfaces of the service provider.

**Antispoofing ACLs in the inbound direction (from the service provider point of view)**: Protecting the rest of the Internet

The service provider should filter traffic coming from customers into the P-network (provider network), by allowing traffic to originate only from the assigned address space. This way, the service provider protects the rest of the Internet by preventing the customer from using spoofed IP addresses. Such filtering is described in RFC 2827 (Network Ingress Filtering: Defeating Denial of Service).

**Antispoofing ACLs in the outbound direction (from the service provider point of view)**: Protecting the customer's network.

The service provider should filter traffic that is destined to the customers by denying traffic that originates from customer-assigned address space. This way, the service provider protects the customer from IP spoofing attacks.

**RFC 1918 IP address filtering**: Packets that are destined to (or from) private IP addresses should not be seen in the Internet and should be filtered.

**Filtering based on security packages**: A service provider can provide residential users with different security packages, where traffic to the customer can be restricted for protection. For example, the service provider could offer packages with low, medium, and high security. In the low security package, all inbound and outbound traffic (from the customer point of view) is allowed, except the traffic just described. In a medium security package, all outbound traffic could be allowed and inbound traffic with a source port larger than 1023 could be allowed. In the high security packages, the outbound traffic could also be limited to the most-used traffic only (HTTP, HTTPS, DNS, ICMP, and so on).

![](<../../.gitbook/assets/Unknown image (726)>)

#### ACL rules and guidelines

The standard Access-list is generally applied close to the destination, to prevent permitting or denying access to other resources for the source network that the ACL covers

This is because standard determines access only for the source IP, as shown in the below picture, if you would place deny to internet in standard ACL to the R1, the guest vlan couldn't access the internal servers

The extended Access-list is generally applied close to the source, in order to avoid unnecessary processing on next-hop routers

This is because the extended matches only specific source destination and application, so it won't interfere with communication to other sources and moreover it saves the processig requirements, since the packet that is meant to be denied for certain resource can be dropped by the first hop router instead of the last hop

![](<../../.gitbook/assets/Unknown image (727)>)

Only one ACL per direction and per protocol is allowed (e.g. one ACL can be created for inbound and applied and second for outbound and applied to the same interface, similarly one for IPv4 and one for IPv6), or you can use the same ACL and apply it in both directions, if it makes sense to do so.

Same ACL can be used multiple times (applied on a multiple interfaces)

An ACL applied to an interface does not filter traffic from a router (management traffic, routing protocol traffic, and so on). In case you would like to limit administrative access to the routes (SSH or Telnet), you should apply an ACL to vty lines instead of an interface

The figure shows a scenario in which ACL is used to deny access to the internet only to the host IPv4 address 10.1.1.101.

Traffic from other hosts within 10.1.1.0/24 is allowed.

Two ACL implementations are represented in the figure. The first uses a standard ACL 15. A standard ACL is applied at the point closest to the destination. That point is the Gi0/1 interface on the Branch router. Filtering should happen for the traffic exiting Gi0/1 interface, in the outbound direction. Note that PC2 traffic would reach the router, be processed to determine the outbound interface (it will be routed), and, only at the very exit, it will be discarded. The processing power and bandwidth of the router are used for both permitted and discarded traffic.

Note that another possible placement for standard ACL 15 would be the GigabitEthernet0/0 interface. For the traffic to be filtered, the direction would have to be inbound. However, this solution

would not only prevent host PC2 from accessing the internet but would also deny all communication between PC2 and the router.

The second implementation uses an extended ACL NOINTERNET PC2. An extended access list should be placed as close to the source of the denied traffic as possible. In the example, the denied traffic is the PC2 traffic. The closest point to PC2 is the Gi0/0 interface on the Branch router. It should filter traffic incoming to the router; therefore, the ACL should be applied in the

inbound direction. Traffic from PC2 will be discarded before it is routed, which saves the processing power and bandwidth of the router.

![](<../../.gitbook/assets/Unknown image (728)>)

When adding a new ACE (access-control list entry), it is essential to follow a structured approach using sequence numbers. If no sequence number is defined in the ACE, the ACE/rule is placed automatically as the last ACE - to the bottom of the list. By default, numbering starts with 10 and subsequent numbers are incremented by 10.

Your ACL should be organized to allow processing from the top down. Organize your ACL so that the more specific references to a network or subnet appear before the ones that are more general. Place conditions that occur more frequently before conditions that occur less frequently

![](<../../.gitbook/assets/Unknown image (729)>)

Using numbered configuration method, you cannot add or remove individual statements directly. Instead, you would have to first copy the entire access list, modify it in the text editor, delete it from the configuration and enter the modified ACL statements.

With named configuration method, modifying an ACL is significantly easier. Before you implement the modification, you need to know the sequence number of the statement you wish to add or remove. Modifications are implemented in the Named Access List configuration mode.

Specific statements cannot be overwritten using the same sequence number as an existing statement. The current statement must be deleted first, and then the new one can be added.

To add an entry from within Named Access List configuration mode, use one of the following commands, depending on whether you are modifying a standard or an extended access list:

For the standard ACL use the \[sequence-number] permit|deny source\_matching\_criteria command

For the extended ACL use the \[sequence-number] permit|deny protocol source\_matching\_criteria destination\_matching\_criteria command

To delete an entry, go to the Named Access List configuration mode. When deleting an entry for the numbered access list, use the ACL number as the name of the list you wish to modify. Again, you need to know the sequence number of the statement you wish to delete. To delete a statement, use the command no sequence-number.

### Port ACLs (PACLs)

**Port ACLs (PACLs)** are used on Layer 2 ports to filter only incoming traffic based on the specified IP or MAC address

#### Standard ACL (IPv4)

filters only based on Source IP (Number ID range from 1-99 and 1300-1999)

| (config)# access-list 1 permit {source\_address \[source\_wildcard]} (config-if)# ip access-group 1 {in\|out} | standard acl can be also configured with name - called Named ACL |
| ------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| show ip access-lists                                                                                          | to display all access-lists and matches that were enforced       |

![](<../../.gitbook/assets/Unknown image (730)>)

Applying ACL to the interface to filter the traffic

![](<../../.gitbook/assets/Unknown image (731)>)

#### Extended ACL (IPv4)

can filter Source IP, Destination IP and protocols (tcp, udp, icmp, etc..). Number ID range from 100-199 and 2000-2699)

It is very important to note that in this example the port numbers are destination port numbers because they come after the destination address (which in this case is represented by the keyword any). Port numbers that appear after the source address are source port numbers.

Ports can be also specified as keyword defining the application - insead of typing port 23, yo ucan type telnet, this is however limited only for certain applications/port numbers

![](<../../.gitbook/assets/Unknown image (732)>)

| (config)# ip access-list extended \[100-199 \| 2000-2699 \| ACL\_NAME] (config-ext-nacl)# \[permit \| deny] \[protocol] \[source-ip \| any] \[mask] \[dest-ip \| any] \[mask] \[log] | Extended ACL can be also configured with name, called Named ACL, which is specified with letters instead of number range ID, giving ACL brief description to their intention The Named Access List configuration mode provides more flexibility in configuring and modifying ACL entries. if the wildcard is omitted in the matching criteria, wildcard mask of 0.0.0.0 is assumed keyword hosts automatically assumes definition of single host IP (/32) keyword any automatically assumes all possible addresses e.g 255.255.255.255 keyword log logs the ingress interface and MAC that tries to access the vlan/port or destination |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Example access-list extended 100 deny tcp any any eq 23 deny ip host 10.1.2.2 host 10.1.2.1 permit ip any any interface GigabitEthernet0/1 ip access-group 100 in                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |

| 10.10.10.0 0.0.0.254 | To match even hosts in subnet (mask .254 leave last bit to be always 0) |
| -------------------- | ----------------------------------------------------------------------- |

![](<../../.gitbook/assets/Unknown image (733)>)

| 10.10.10.10.0.0.254                                               | To match each odd host in subnet                                                                                                                                                                                     |
| ----------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| access-list 101 permit ip any 192.168.0.1 0.0.255.0               | This access list will filter the 3rd octet (X.X.X.X)                                                                                                                                                                 |
| deny tcp host 209.165.200.255 host 209.165.201.25 eq 443          | eq specifies the port that has to be matched; this statement denies HTTPs as destination port                                                                                                                        |
| permit tcp host 209.165.200.255 eq 80 host 209.165.201.25         | this ACL statement permits port 80 to be used as source port                                                                                                                                                         |
| access-list 121 permit tcp host 1.1.1.1 host 10.9.9.9 established | allow the return traffic associated with already established sessions to pass through the firewall or network device. This ensures that the bidirectional communication is maintained and responses can pass through |
| access-list 122 permit icmp any any echo-reply                    | to permit receipt of responses to the switch that originated the pings                                                                                                                                               |
| access-list 122 permit icmp any any echo                          | to allow pings to the switch                                                                                                                                                                                         |

| ip access-list resequence                                | Note that a reload will resequence numbers in the ACL so that all numbers are multiples of 10. To initiate resequencing on your own and avoid reloading, use the access-list ACL name resequence command. |
| -------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ip access-list standard 1 no permit 192.168.0.0 OR no 20 | modifies an statemen within an access list. First you should go under the ACL sub-config mode and then execute the no form of the full statement or it's sequence number                                  |
| Router(config)# ip access-list resequence kmd1 100 15    | This example resequences an access list named kmd1. The starting sequence number is 100 and the increment is 15.                                                                                          |

### MAC ACLs

| Switch(config)# mac access-list extended \[ACL NAME] Switch(config-ext-macl)# permit host \[src-mac \| any] \[dst-mac \| any] Switch(config-if)# mac access-group \[ACL NAME] in | same syntax for IP as named ACL Prefer mode: PACL overrides other ACLs. This is the only allowed mode for trunks. Merge mode (default): PACLs, VACLs and RACLs are merged in the ingress direction. |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### VLAN ACLs (VACLs)

**VLAN ACLs (VACLs)** can filter traffic within a VLAN or that is routed into or out of a VLAN

Configured through access maps (multiple matches, single action). Implicit deny at the end of the VACL. MAC addresses can be filtered aswell (MAC ACL)

| Switch(config)# vlan access-map \[MAP NAME] Switch(config-access-map)# match \[mac \| ip] address \[ACL NAME] Switch(config-access-map)# action \[drop \| forward] \[log] Switch(config)# vlan filter \[MAP NAME] vlan-list | Access list to match with permit statement and is then allowed/denied in access-map match: for matching traffic action: is for the action on the matched traffic |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show vlan access-map                                                                                                                                                                                                        |                                                                                                                                                                  |
| show mac access-list extended                                                                                                                                                                                               |                                                                                                                                                                  |

### Time-based ACLs

**Time-based ACLs** use a time range to define when permit or deny statements are in effect. Prior to this feature, ACL statements were always in effect once applied. Both named and numbered access lists can reference a time range.

| ## Defining a time range for ACLs Router(config)# time-range Router(config-time-range)# absolute start [hh:mm](hh:mm) \[day] \[month] \[year] end [hh:mm](hh:mm) \[day] \[month] \[year] Router(config-time-range)# periodic \[days] [hh:mm](hh:mm) to [hh:mm](hh:mm)## Defining an IPv4 extended ACL with a time range Router(config)# ip access-list extended \[100-199 \| 2000-2699 \| WORD] Router(config-std-nacl)# \[permit \| deny] \[protocol] \[source-ip \| any] \[mask] \[dest-ip \| any] \[mask] \[log] time-range | multiple periodic commands are allowed; only one absolute command is allowed. |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------- |

Example

| time-range WEEKDAYS periodic weekdays 08:00 to 18:00 time-range WEEKENDS periodic weekend 00:00 to 23:59 ip access-list extended FTP\_RESTRICTION permit tcp any any eq 20 time-range WEEKDAYS permit tcp any any eq 21 time-range WEEKDAYS deny tcp any any eq 20 time-range WEEKENDS deny tcp any any eq 21 time-range WEEKENDS interface GigabitEthernet0/1 ip access-group FTP\_RESTRICTION in | FTP traffic will be permitted on weekdays and denied on weekends // When denying traffic to FTP server, set deny for both ports 20 (ftp-control) and 21 (ftp-data) |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

### Downloadable ACLs (dACLs)

**Downloadable ACLs (dACLs)** are dynamically assigned PACLs by RADIUS servers such as Cisco ISE. They overwrite other ACLs after successful authentication.

### IPv6 ACL traffic filters

As in case of IPv4, last implicit statement #deny ipv6 any any is activated after first explicit rule is set

Rules, options etc. very similar to IPv4

In contrast to IPv4:

Only named, extended ACL are available

Unlike IPv4 ACLs, IPv6 ACLs do not use wildcard masks. Instead, the prefix-length is used to indicate how much of an IPv6 source or destination address should be matched

Each IPv6 ACL contain 3 implicit commands at the end:

permit icmp any any nd-na

permit icmp any any nd-ns

deny ipv6 any any

ND communication must be allowed to obtain MAC<->IPv6 address binding

IPv6 ACL enables filtering based on presence or content of extension headers

| permit icmp any any nd-na permit icmp any any nd-ns deny ipv6 any any |   |
| --------------------------------------------------------------------- | - |

| permit icmp any any router-advertisement permit icmp any any router-solicitation | ICMPv6 RA/RS has to be explicitly permitted in ACL, as they are used for address resolution |
| -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |

| (config)# ipv6 access-list \[ACL NAME] (config-ipv6-acl)#\[sequence #] remark text (config-ipv6-acl)# \[permit \| deny] \[protocol] \[source-ipv6-prefix \| any \| host source-ipv6-address] \[dest-ipv6-prefix \| any \| host dest-ipv6-addr] \[port] | Defines an ACL rule <1-255> … An IPv6 protocol number X:X:X::\[/<0-128>] ... An IPv6 source or destination address/prefix Reconfiguration: Use the #no form of the specific rule to remove it and insert the updates rule if needed |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (config-if)# ipv6 traffic-filter \[ACL NAME] \[in \| out]                                                                                                                                                                                              |                                                                                                                                                                                                                                     |
| show ipv6 access-lists                                                                                                                                                                                                                                 |                                                                                                                                                                                                                                     |
| show run \| s ipv6 access                                                                                                                                                                                                                              |                                                                                                                                                                                                                                     |

NoteIPv6 Access-class vs IPv6 traffic-filter The difference depends on whether you want to filter IPv6 traffic sent _to_ the router or _through_ the router.

| ipv6 access-class {in \| out}   | filter IPv6 traffic destined to the router (locally originated traffic) - applied to vty |
| ------------------------------- | ---------------------------------------------------------------------------------------- |
| ipv6 traffic-filter {in \| out} | filter IPv6 traffic sent _to_ the router or _through_ the router (transit traffic)       |

Example

| CE# show running-config \| section ipv6 access ipv6 access-list Example\_ACL sequence 10 permit ipv6 2001:DB8:1::/64 any sequence 15 remark Permit all traffic from subnet 2001:db8:1::/64 sequence 20 deny ipv6 2001:DB8:2::/64 host 2001:DB8:3::1 log sequence 25 remark Deny traffic from subnet 2001:db8:2::/64 to host 2001:db8:3::1 | CE# show ipv6 access-list IPv6 access list Example\_ACL permit ipv6 2001:DB8:1::/64 any sequence 10 deny ipv6 2001:DB8:2::/64 host 2001:DB8:3::1 log sequence 20 |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Switch port security

If Layer 2 is compromised, then all layers above it are also affected. Layer 2 security solutions must be implemented to help secure a network.

Port security restricts access to a switch port based on the connecting devices MAC addresses. A port that is configured with port security accepts frames only from secure MAC addresses. The MAC addresses of legitimate devices are allowed access, while other MAC addresses are denied. Port security limits the number of valid MAC addresses allowed on a port. Enabling port security can be used to control unauthorized expansion of the network. Also, port security is the simplest and most effective method to prevent MAC address table flooding attacks.

by default, there is no limit to the number of MAC addresses a switch can learn on an interface, and all MAC addresses are allowed

Port-security provides Layer 2 security measure to limit the number of learned MAC addresses or specify the MAC address that can connect to the access port

Allowed MAC addresses are called secure addresses and are stored in the secure MAC address table. Depending on how the secure MAC address is learned by the switch

When a frame arrives on a port for which port security is configured, its source MAC address is checked against the secure MAC address table. If the source MAC address matches an entry in the table for this port, the device forwards the frame to be processed. Otherwise, the device does not forward the frame.

Statically Configured MAC Addresses MAC addresses can be manually configured and associated with a specific port

Dynamic secure MAC addresses: MAC addresses that are dynamically learned from devices that connect to the port and are not specified manually. The maximum number of such addresses accepted on a port is configured. This port security configuration is used when you care only about how many MAC addresses are permitted to use the port, rather than which MAC addresses are permitted. Dynamically learned MAC addresses that are not secure are stored in the MAC address table until they age out. However, dynamic secure MAC addresses do not age out by default. Instead, they are removed when the switch restarts or the port goes down. Dynamic secure MAC addresses are not stored in the running configuration.

Sticky MAC Addresses When sticky MAC addresses are enabled, the switch dynamically learns the MAC addresses of devices connected to the port and associates them with the port

These MAC addresses are saved in the switch's running configuration, allowing the switch to maintain a list of authorized MAC addresses.

Maximum allowed MAC addresses that can be configured ranges from 1 to 4097

Violation Modes

Shutdown mode (default) when a violation occurs, the switch puts the port into an error-disabled state, effectively shutting down the port

Manual intervention is required to bring the port back up

Protect mode when a violation occurs (e.g., a new MAC address is detected on the secured port), the switch does not forward traffic from the violating source

Restrict mode similar to Protect mode, Restrict mode also does not forward traffic from the violating source. However, it increments a violation counter. This mode allows administrators to monitor and take action based on the violation counter without completely blocking the port

| Switch(config-if)# switchport port-security Switch(config-if)# switchport port-security violation | Change the switchport mode from the default Dynamic Trunking Protocol (DTP) dynamic auto mode to either access or trunk. You can configure port security only on static access ports or trunk ports. When an interface is in the default mode, it cannot be configured as a secure port. In interface configuration mode, use the switchport mode { access \| trunk } command to set the mode to either access or trunk, or use the switchport nonegotiate command to disable DTP. |
| ------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Switch(config-if)# switchport port-security maximum <1-to-4097>                                   | specifies the limit to how many MAC addresses can be connected to the port                                                                                                                                                                                                                                                                                                                                                                                                         |
| Switch(config-if)# switchport port-security mac-address xxxx.xxxx.xxxx                            | add a specific MAC address that can be connected to the port                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Switch(config-if)# switchport port-security mac-address sticky                                    | the first connected MAC address to the port will be written to the running configuration of the switch and used for security check                                                                                                                                                                                                                                                                                                                                                 |
| switchport port-security aging type {absolute \| inactivity}                                      | You can also specify how dynamic secure MAC addresses age, by configuring aging type and aging time; the default is that they do not age. Aging type can be absolute or based on inactivity. When you configure absolute aging, all the dynamically learned secure addresses age out when the aging time expires. When you configure inactivity aging, the aging time defines the period of inactivity after which all the dynamically learned secure addresses age out.           |
| show switchport port-security                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

### DHCP snooping

Since the initial DHCP Discover message sent by the client is broadcast, any server connected to the network can offer an IP configuration to the client, even malicious DHCP server that can perform man in the middle attack

Because the DHCP server provides the DHCP client with server IP addresses, such as the IP address of one or more DNS servers, an attacker can convince a DHCP client to do its DNS lookups through its own DNS server, and can therefore provide its own answers to DNS queries from the client. This in turn allows the attacker to redirect network traffic through itself, allowing it to eavesdrop on connections between the client and network servers it contacts, or to simply replace those network servers with its own

**DHCP snooping** is a Layer 2 security feature that lets switches intercept and verify DHCP messages between clients and servers. It helps prevent rogue DHCP servers from providing malicious IP configurations (after examining Ethertype, switches can inspect Layer 3 and Layer 4 headers).

Switch builds a dynamic DHCP snooping Binding table and maintains the bindings from legitimate DHCP, such as MAC and IP addresses, lease time, binding type (dynamic or static), VLAN and port information of the host it was assigned to. This binding table is then used to validate DHCP messages and ensure that they are coming from legitimate DHCP servers.

For DHCP snooping to work, each switch port must be labeled as trusted or untrusted.

Trusted ports allow legitimate DHCP server messages (Offer, Ack) to pass through unrestricted

Untrusted ports are those ports that are not explicitly configured as trusted

From a DHCP snooping perspective, untrusted access ports should not send any DHCP server responses, such as DHCPOFFER, DHCPACK, or DHCPNAK. The switch will drop all such DHCP packets.

If a DHCP message is received from an untrusted port, or if the binding table does not contain a valid entry for the MAC address in the message, the switch will drop the message

All access ports should be labeled as untrusted, except the port to which the DHCP server is directly connected.

All interswitch ports should be labeled as trusted.

All ports pointing towards the DHCP server (that is, the ports over which the reply from the DHCP server is expected) should be labeled as trusted.

![](<../../.gitbook/assets/Unknown image (734)>)

Note you will not see MAC addresses of a hosts with an active binding entry in a CAM table, since it is already contained in the binding table

| Switch(config)# ip dhcp snooping            | has to be enabled globally                                      |
| ------------------------------------------- | --------------------------------------------------------------- |
| Switch(config)# ip dhcp snooping \[vlan <>] | to enable for a vlan aswell, enable both the first and for vlan |
| Switch(config-if)# ip dhcp snooping trust   | Mark trusted interfaces (connected to legit DHCP servers)       |
| show ip source binding                      |                                                                 |
| show ip dhcp snooping \[trust \| binding]   | debug ip dhcp snooping \[events\|packet]                        |

![](<../../.gitbook/assets/Unknown image (735)>)

DHCP on a Cisco Device Restriction/Note

When a DHCP server receives DHCP discover with Option 82, it expects that the giaddr of the relay agent is present, however when a DHCP snooping is configured on the switch, it inserts the DHCP option 82 to the discover request, but leaves the giaddr as 0.0.0.0 since it performs Layer 2 switch function. Such discover packet is dropped by the cisco DHCP server, since it cannot determine who the relay agent is when the the giaddr is 0.0.0.0

Go through the most straightforward way - when deploying the DHCP Snooping, do not initially modify anything regarding the Option 82

Verify whether your clients can receive their IP config via DHCP. If yes then there is nothing more to tweak. Otherwise, proceed further:

| ip dhcp relay information trust-all    | To fix this we have to enable on a DHCP server                                                                                                                                                                                                                                                                                                                                      |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| no ip dhcp snooping information option | This is not needed when above command is configured on the dhcp server cisco device This command is configured on the intermediary switch to disable the insertion of Option 82 DHCP option 82 is inserted by default into DHCP packets when DHCP snooping is enabled Cisco IOS acting as DHCP server can’t handle DHCP Option 82 and therefore sending it out needs to be disabled |

![](<../../.gitbook/assets/Unknown image (736)>)

### Dynamic ARP Inspection (DAI)

**Dynamic ARP Inspection (DAI)** prevents ARP spoofing attacks (ARP cache poisoning). These occur when an attacker sends falsified gratuitous ARP messages to associate their MAC address with a victim IP.

DAI is an ingress only feature that intercepts ARP packets and validates IP-to-MAC association with the dhcp snooping binding table

If the ARP packet contains MAC address that is already active on a different port, it drops the packet

DAI can also be configured to drop ARP packets when the IP addresses in the packet are invalid or when the MAC addresses in the body of the ARP packet do not match the addresses specified in the Ethernet header. Rate limit on untrusted ports is by default: 15pps

In a typical network DAI configuration, configure all access switch ports that are connected to host ports as untrusted and all switch ports that are connected to other switches as trusted. With this configuration, all ARP packets entering a switch from another switch bypass the security check, which is safe because all switches validate the ARP packets as they are sent by hosts that are connected to untrusted access ports.

Enable DHCP snooping globally.

Enable DHCP snooping on selected VLANs.

| Switch(config)# ip arp inspection vlan     | DAI can be configured on a per-VLAN basis.                                                                                             |
| ------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| Switch(config-if)# ip arp inspection trust | applied on the port, where legitimate network equipment is connected. It can be applied on access,trunk, EtherChannels and PVLAN ports |
| show ip arp inspection                     | use debug ip dhcp snooping to see DAI events                                                                                           |

### IP Source Guard (IPSG)

**IP Source Guard (IPSG)** blocks all traffic except DHCP until a valid binding exists. After DHCP creates a binding table entry, IPSG enforces the DHCP-assigned source IP and drops packets without a binding entry. This protects against IP spoofing attacks.

| Switch(config)# ip source binding \[MAC] \[VLAN] \[IP] binding interface <> | ## Creating a static IP-to-MAC binding entry                                                                                                                                             |
| --------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Switch(config-if)# ip verify source \[mac-check]                            | Enables IP source guard with source IP address filtering (second entry in snip) mac-check enables IP Source Guard with source IP address and MAC address filtering (first entry in snip) |
| show ip verify source                                                       |                                                                                                                                                                                          |

![](<../../.gitbook/assets/Unknown image (737)>)

### Unicast Reverse Path Forwarding (uRPF)

**Unicast Reverse Path Forwarding (uRPF)** is a mechanism used to prevent IP spoofing attacks in networks.

URPF verifies that incoming packets are arriving on the expected interface for the source IP in the packet's header - comparison is being made against CEF

If the interface from which the packet is received does not match the expected interface for the source IP address, the packet is dropped

Strict mode requires that incoming packets have a source IP address that matches the expected interface based on the routing table e.g ithe interface is used to forward pakcets to the source.

Configure strict uRPF only where there is natural or configured symmetrical routing, meaning packets follow the same path in both directions (to and from the source) .

Internal interfaces are likely to have a routing asymmetry, that is, multiple routes to the source of a packet. This asymmetry can cause valid packets to fail the check and be dropped, therefore, you should not implement strict uRPF on interfaces that are internal to the network and implement it on the network edge, where spoofing is expected

Implementation of strict mode uRPF requires maintenance of a uRPF interfaces list for the prefixes. The uRPF interface list keeps track of interfaces that are expected to receive packets from specific source prefixes. These interfaces are determined by the routing table (prefix path). If multiple prefixes use the same interface, the router shares the interface entry to save resources

Note The behavior of strict uRPF varies slightly on the basis of platforms, the number of recursion levels, and the number of paths in Equal-Cost Multipath (ECMP) scenarios. A platform may switch to loose uRPF check for some or all prefixes, even though strict uRPF is configured. For example, if ECMP Path is eight or more, strict mode is converted to loose mode

![](<../../.gitbook/assets/Unknown image (738)>)

Loose mode only requires that the source IP address of an incoming packet has a route in the routing table. If the source IP address has a route in the routing table, the packet is accepted, regardless of which interface it arrives on. This mode is less secure. Used mainly in the network core, where spoofing probability is much lower

Note If the entry points to the Null0 interface, the RPF check fails and packet is dropped - used in RTBH BGP

![](<../../.gitbook/assets/Unknown image (739)>)

Note

Normally, a device might block packets it sends to itself (self-ping) because the source and destination are the same.

The allow self-ping option enables the router to accept packets where the source address matches its own address.

This is useful for testing or specific scenarios where the router needs to send and receive packets to/from itself

In some cases, traffic doesn’t have a specific route in the routing table and instead matches a default route (e.g., 0.0.0.0/0). A default route is used as a fallback when no exact match exists.

By default, uRPF might block such traffic because it cannot verify the source IP against a specific route.

The allow default option lets the router process packets that match the default route, as long as they come in through the interface associated with that default route

uRPF Restrictions

![](<../../.gitbook/assets/Unknown image (740)>)

Because uRPF requires the routing table to handle both destination and source lookups, it effectively doubles the workload on the routing table. Half of the routing table is conceptually allocated for each type of lookup.

This doesn't mean the size of the routing table physically changes—it refers to the increased computational demand and potential impact on performance

Note uRPF configuration may vary slightly from platform and software version based on platform restrictions. Please refer to the proper documentation for the platform and software version you are configuring uRPF on

IOS-XR Configuration

[https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/security/24xx/configuration/guide/b-system-security-cg-cisco8000-24xx/implementing-urpf.html](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/security/24xx/configuration/guide/b-system-security-cg-cisco8000-24xx/implementing-urpf.html)

IOS-XE configuration

[https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/ip-routing/b-ip-routing/m\_urpf-acl-sup.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/ip-routing/b-ip-routing/m_urpf-acl-sup.html)

| Router(config-if)# \[ip \|ipv6] verify unicast source reachable-via \[any \| rx] | keyword rx sets Stric mode and any sets Loose mode Optionally an ACL can be applied. If the packet fails uRPF the ACL will be examined If there’s a permit statement it will be forwarded anyway. If there’s a deny statement it will be finally dropped. Failed uRPF packets are always logged even if they were permitted by an ACL |
| -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip traffic                                                                  |                                                                                                                                                                                                                                                                                                                                       |
| show ip int <> \| sec verify                                                     | to display per-interface statistics about uRPF drops and suppressed drops                                                                                                                                                                                                                                                             |
| show mls cef ip rpf                                                              |                                                                                                                                                                                                                                                                                                                                       |
| show prm server profile-status software                                          |                                                                                                                                                                                                                                                                                                                                       |

URPF with snmp trap configuration example

| ip verify drop-rate compute window 60        | window size determines the time interval during which packets are monitored to calculate the drop rate                                                                                             |
| -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ip verify drop-rate compute interval 60      | specifies how often the drop rate is recalculated within the defined window size                                                                                                                   |
| ip verify drop-rate notify hold-down 60      | prevents excessive notifications by specifying a duration during which the router will not send any additional notifications to the SNMP server, even if the drop rate remains above the threshold |
| interface Gi1                                |                                                                                                                                                                                                    |
| ip verify unicast source reachable-via rx    | enables URPF; router verifies the source address of incoming packets by ensuring that it has a route back to the source via the receiving (rx) interface                                           |
| ip verify path-reverse                       | not necessary, however used in conjunction with command above and does the same thing                                                                                                              |
| ip verify unicast notification threshold 800 | sets the threshold for generating SNMP notifications when URPF drops packets                                                                                                                       |
| snmp trap ip verify drop-rate                | enables SNMP trap notifications for drop-rate events for URPF                                                                                                                                      |
| snmp-server enable traps                     | enable traps on the router + configure snmp aswell snmp-server host community <> version 2c                                                                                                        |
| snmp-server enable traps ipverify            | specifically enables the SNMP trap notifications for the "ipverify" (URPF) group                                                                                                                   |

### Remote Triggered Black Hole Filtering (RTBH)

**Remote Triggered Black Hole Filtering (RTBH)** is a technique that uses routing protocol updates to manipulate route tables at the network edge (or elsewhere) to drop undesirable traffic before it enters the service provider network.

One of many methods to:

Mitigate the damaging effects of DDoS attack

Black hole (drop) traffic destined to the IP address or addresses being attacked

Filter the infected host traffic at the edge of the network closest to the source of the attack

Method for quickly dropping undesirable traffic at the edge of the network, based on either source addresses or destination addresses by forwarding it to the null0 interface (undemanding from the point of view of the CPU/HW router)

**Null0** is a pseudointerface that is always up and can never forward or receive traffic.

Signaling Router

Dedicated router (or UNIX workstation running BGP) that is installed at the NOC exclusively for the purpose of triggering a black hole

Must have an iBGP peering relationship with all the edge routers

If using route reflectors, it must have an iBGP relationship with the route reflectors in every cluster

Configured to redistribute static routes to its iBGP peers

#### Destination-based RTBH

1. The setup (preparation)

| Signaling\_router# router bgp 65535 ... redistribute static route-map static-to-bgp ... ! route-map static-to-bgp permit 10 match tag 66 set ip next-hop 192.0.2.1 set local-preference 200 set community no-export set origin igp ! route-map static-to-bgp permit 20 | Signaling router is also configured to redistribute static routes to its iBGP peers iBGP running among all routers Signaling router doesn’t have to accept any prefix Once the trigger advertisement is sent to the iBGP peers, you need to make sure that the peers prefer this route to one that they already have for the destination network. One way is to set a local-preference value greater than the default value of 100 and set the origin to igp. The preferred method is to set a path with a higher value for local-preference and a lower value of origin (IGP is lower than Exterior Gateway Protocol \[EGP], which is lower than incomplete). The route policy also adds a no-export community to the redistributed routes to prevent advertisement of the routes outside the autonomous system of the service provider. |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PE\_routers# ! ip route 192.0.2.1 255.255.255.255 Null0 ! interface Null0 no ip unreachables                                                                                                                                                                           | PEs must have a static route for an unused IP address space. For example, 192.0.2.1/32 (reserved for TEST-NET - such purpose) is set to Null0                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

2. The trigger

| Signaling router# ! ip route 172.19.61.1 255.255.255.255 Null0 Tag 66 | An administrator adds a static route to the trigger, which redistributes the route by sending a BGP update to all its iBGP peers, setting the next hop to the target destination address under attack as 192.0.2.1 All traffic to the target will now be forwarded to Null0 at the edge and dropped |
| --------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

3. The withdrawal

When the threat no longer exists, the administrator must manually remove the static route from the trigger, which sends a BGP route withdrawal to its iBGP peers

![](<../../.gitbook/assets/Unknown image (741)>)

IOS-XR config

![](<../../.gitbook/assets/Unknown image (742)>)

#### Source-based RTBH

In the source-based RTBH implementation, traffic coming from the IP addresses of the attacker is discarded on the service provider edge. It requires knowledge of the attacker IP addresses. However, in many DDoS attacks, the attacker IP addresses are difficult to determine because the number and variety of devices performing the attack can be very large due to worm infections.

With destination-based black holing, all traffic to a specific destination is dropped once the black hole has been activated, regardless of where it is coming from

Obviously, this could include legitimate traffic destined for the target

Implementation of source-based black hole filtering depends on Unicast Reverse Path Forwarding (URPF)

Loose URPF checks the packet and forwards it if there is a route entry for the source IP of the incoming packet in the router FIB.

If the router does not have an FIB entry for the source IP address, or if the entry points to Null0, the Reverse Path Forwarding (RPF) check fails, and the packet is dropped

It utilizes the same discard route with redistribution of the static route containing the malicious source IP address. The signaling router then sends the iBGP update to the PE routers.

1. The setup (preparation)

Trigger is also configured to redistribute static routes to its iBGP peers

PEs must have a static route for an unused IP address space. For example, 192.0.2.1/32 is set to Null0

Loose URPF must be configured on all external facing interfaces at the edges (PEs)

2. The trigger

Add a static route to the trigger „source IP of the attacker “ to 192.0.2.1

All traffic from the source IP will fail the loose URPF check at the PEs and therefore will be dropped

3. The withdrawal

Remove the static route from the trigger

![](<../../.gitbook/assets/Unknown image (743)>)

IOS-XR config

In the example, strict uRPF is enabled on the PE2 router on the GigabitEthernet0/0/0/0 interface using the ipv4 verify unicast source reachable-via rx command. A router that receives a packet will perform the routing table lookup for the source IP address. If the packet came from the attacker, the router will see that the routing table for the attacker IP addresses points to the 192.0.2.1 next-hop IP address, which in turn points to the null interface. The packet will be dropped because the packet was received through the other interface that would be used to forward the return traffic.

![](<../../.gitbook/assets/Unknown image (744)>)

### Demilitarized Zone (DMZ)

**Demilitarized Zone (DMZ)** is a security zone between an internal network and the public Internet. It hosts services accessible from both sides (for example, web/email/DNS), while internal-only services (for example, databases) stay on private networks.

### Cisco Zone-Based Firewall (ZBFW)

**Cisco Zone-Based Firewall (ZBFW)** is an integrated stateful firewall feature in IOS. It reduces the need for a separate branch firewall while still providing stateful network security.

Router interfaces are assigned to a specific zone. A zone establishes a security border on the network and defines acceptable traffic that is allowed to pass between zones

By default, interfaces in the same security zone can communicate freely with each other, but interfaces in different zones cannot communicate with each other

**Self Zone** is a system-level zone that includes the router’s own IP addresses used for management (SSH/SNMP) and control-plane protocols (EIGRP/BGP).

After a policy is applied to the self zone and another security zone, inter-zone communication must be defined

**The Default Zone**

system-level zone, and any interface that is not a member of another security zone is placed in this zone automatically

When an interface that is not associated in a security zone sends traffic to an interface that is in a security zone, the traffic is dropped

![](<../../.gitbook/assets/Unknown image (745)>)

#### Example: Internet-facing router interface

| A zone needs to be created for the outside zone (the Internet). The self zone is defined automatically Zone security OUTSIDE description OUTSIDE Zone used for Internet Interface                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Define instpection class-map for traffic that is permitted or denied to be received from INET ip access-list extended ACL-IPSEC permit udp any any eq non500-isakmp permit udp any any eq isakmp ip access-list extended ACL-PING-AND-TRACEROUTE permit icmp any any echo permit icmp any any echo-reply permit icmp any any ttl-exceeded permit icmp any any port-unreachable permit udp any any range 33434 33463 ttl eq 1 ip access-list extended ACL-ESP permit esp any any ip access-list extended ACL-DHCP-IN permit udp any eq bootps any eq bootpc ip access-list extended ACL-GRE permit gre any any class-map type inspect match-any CLASS-OUTSIDE-TO-SELF-INSPECT match access-group name ACL-IPSEC match access-group name ACL-PING-AND-TRACEROUTE class-map type inspect match-any CLASS-OUTSIDE-TO-SELF-PASS match access-group name ACL-ESP match access-group name ACL-DHCP-IN match access-group name ACL-GRE | #show class-map type inspect \[class-name]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Define the inspection policy map, which applies firewall policy actions to the class maps defined in the policy map policy-map type inspect POLICY-OUTSIDE-TO-SELF class type inspect CLASS-OUTSIDE-TO-SELF-INSPECT inspect class type inspect CLASS-OUTSIDE-TO-SELF-PASS pass class class-default drop                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | The inspect policy map has an implicit class default that uses a default drop action. This provides the same implicit “deny all” as an ACL #show policy-map type inspect \[policy-name]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Apply a policy map to a traffic flow source to a destination zone-pair security OUTSIDE-TO-SELF source OUTSIDE destination self service-policy type inspect POLICY-OUTSIDE-TO-SELF                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Apply the security zones to the appropriate interfaces interface GigabitEthernet 0/2 zone-member security OUTSIDE                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | #show policy-map type inspect zone-pair                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Router also have to allow locally originated packets with a self-to-outside policy                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ip access-list extended ACL-DHCP-OUT permit udp any eq bootpc any eq bootps ! ip access-list extended ACL-ICMP permit icmp any any ! class-map type inspect match-any CLASS-SELF-TO-OUTSIDE-INSPECT match access-group name ACL-IPSEC match access-group name ACL-ICMP class-map type inspect match-any CLASS-SELF-TO-OUTSIDE-PASS match access-group name ACL-ESP match access-group name ACL-DHCP-OUT ! policy-map type inspect POLICY-SELF-TO-OUTSIDE class type inspect CLASS-SELF-TO-OUTSIDE-INSPECT inspect class type inspect CLASS-SELF-TO-OUTSIDE-PASS pass class class-default drop log ! zone-pair security SELF-TO-OUTSIDE source self destination OUTSIDE service-policy type inspect POLICY-SELF-TO-OUTSIDE                                                                                                                                                                                                      | drop \[log]: This default action silently discards packets that match the class map. The log keyword adds syslog information that includes source and destination information pass \[log]: This action makes the router forward packets from the source zone to the destination zone. Packets are forwarded in only one direction. A policy must be applied for traffic to be forwarded in the opposite direction. Useful for protocols like IPsec, ESP, and other inherently secure protocols with predictable behavior. inspect: offers state-based traffic control. The router maintains connection/session information and permits return traffic from the destination zone without the need to specify it in a second policy |
