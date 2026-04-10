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

## Access Control Lists (ACL)

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

![](<../.gitbook/assets/Unknown image (723)>)

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

![](<../.gitbook/assets/Unknown image (724)>)

![](<../.gitbook/assets/Unknown image (725)>)

#### Infrastructure ACLs (iACLs)

Traffic from customers should only traverse service provider devices and should never be destined to the devices themselves. By implementing ACLs on the edge of the network, the infrastructure of the service provider can be protected by blocking traffic to the router interfaces of the service provider.

**Antispoofing ACLs in the inbound direction (from the service provider point of view)**: Protecting the rest of the Internet

The service provider should filter traffic coming from customers into the P-network (provider network), by allowing traffic to originate only from the assigned address space. This way, the service provider protects the rest of the Internet by preventing the customer from using spoofed IP addresses. Such filtering is described in RFC 2827 (Network Ingress Filtering: Defeating Denial of Service).

**Antispoofing ACLs in the outbound direction (from the service provider point of view)**: Protecting the customer's network.

The service provider should filter traffic that is destined to the customers by denying traffic that originates from customer-assigned address space. This way, the service provider protects the customer from IP spoofing attacks.

**RFC 1918 IP address filtering**: Packets that are destined to (or from) private IP addresses should not be seen in the Internet and should be filtered.

**Filtering based on security packages**: A service provider can provide residential users with different security packages, where traffic to the customer can be restricted for protection. For example, the service provider could offer packages with low, medium, and high security. In the low security package, all inbound and outbound traffic (from the customer point of view) is allowed, except the traffic just described. In a medium security package, all outbound traffic could be allowed and inbound traffic with a source port larger than 1023 could be allowed. In the high security packages, the outbound traffic could also be limited to the most-used traffic only (HTTP, HTTPS, DNS, ICMP, and so on).

![](<../.gitbook/assets/Unknown image (726)>)

#### ACL rules and guidelines

The standard Access-list is generally applied close to the destination, to prevent permitting or denying access to other resources for the source network that the ACL covers

This is because standard determines access only for the source IP, as shown in the below picture, if you would place deny to internet in standard ACL to the R1, the guest vlan couldn't access the internal servers

The extended Access-list is generally applied close to the source, in order to avoid unnecessary processing on next-hop routers

This is because the extended matches only specific source destination and application, so it won't interfere with communication to other sources and moreover it saves the processig requirements, since the packet that is meant to be denied for certain resource can be dropped by the first hop router instead of the last hop

![](<../.gitbook/assets/Unknown image (727)>)

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

![](<../.gitbook/assets/Unknown image (728)>)

When adding a new ACE (access-control list entry), it is essential to follow a structured approach using sequence numbers. If no sequence number is defined in the ACE, the ACE/rule is placed automatically as the last ACE - to the bottom of the list. By default, numbering starts with 10 and subsequent numbers are incremented by 10.

Your ACL should be organized to allow processing from the top down. Organize your ACL so that the more specific references to a network or subnet appear before the ones that are more general. Place conditions that occur more frequently before conditions that occur less frequently

![](<../.gitbook/assets/Unknown image (729)>)

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

![](<../.gitbook/assets/Unknown image (730)>)

Applying ACL to the interface to filter the traffic

![](<../.gitbook/assets/Unknown image (731)>)

#### Extended ACL (IPv4)

can filter Source IP, Destination IP and protocols (tcp, udp, icmp, etc..). Number ID range from 100-199 and 2000-2699)

It is very important to note that in this example the port numbers are destination port numbers because they come after the destination address (which in this case is represented by the keyword any). Port numbers that appear after the source address are source port numbers.

Ports can be also specified as keyword defining the application - insead of typing port 23, yo ucan type telnet, this is however limited only for certain applications/port numbers

![](<../.gitbook/assets/Unknown image (732)>)

| (config)# ip access-list extended \[100-199 \| 2000-2699 \| ACL\_NAME] (config-ext-nacl)# \[permit \| deny] \[protocol] \[source-ip \| any] \[mask] \[dest-ip \| any] \[mask] \[log] | Extended ACL can be also configured with name, called Named ACL, which is specified with letters instead of number range ID, giving ACL brief description to their intention The Named Access List configuration mode provides more flexibility in configuring and modifying ACL entries. if the wildcard is omitted in the matching criteria, wildcard mask of 0.0.0.0 is assumed keyword hosts automatically assumes definition of single host IP (/32) keyword any automatically assumes all possible addresses e.g 255.255.255.255 keyword log logs the ingress interface and MAC that tries to access the vlan/port or destination |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Example access-list extended 100 deny tcp any any eq 23 deny ip host 10.1.2.2 host 10.1.2.1 permit ip any any interface GigabitEthernet0/1 ip access-group 100 in                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |

| 10.10.10.0 0.0.0.254 | To match even hosts in subnet (mask .254 leave last bit to be always 0) |
| -------------------- | ----------------------------------------------------------------------- |

![](<../.gitbook/assets/Unknown image (733)>)

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

## Switch port security

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

## DHCP snooping

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

![](<../.gitbook/assets/Unknown image (734)>)

Note you will not see MAC addresses of a hosts with an active binding entry in a CAM table, since it is already contained in the binding table

| Switch(config)# ip dhcp snooping            | has to be enabled globally                                      |
| ------------------------------------------- | --------------------------------------------------------------- |
| Switch(config)# ip dhcp snooping \[vlan <>] | to enable for a vlan aswell, enable both the first and for vlan |
| Switch(config-if)# ip dhcp snooping trust   | Mark trusted interfaces (connected to legit DHCP servers)       |
| show ip source binding                      |                                                                 |
| show ip dhcp snooping \[trust \| binding]   | debug ip dhcp snooping \[events\|packet]                        |

![](<../.gitbook/assets/Unknown image (735)>)

DHCP on a Cisco Device Restriction/Note

When a DHCP server receives DHCP discover with Option 82, it expects that the giaddr of the relay agent is present, however when a DHCP snooping is configured on the switch, it inserts the DHCP option 82 to the discover request, but leaves the giaddr as 0.0.0.0 since it performs Layer 2 switch function. Such discover packet is dropped by the cisco DHCP server, since it cannot determine who the relay agent is when the the giaddr is 0.0.0.0

Go through the most straightforward way - when deploying the DHCP Snooping, do not initially modify anything regarding the Option 82

Verify whether your clients can receive their IP config via DHCP. If yes then there is nothing more to tweak. Otherwise, proceed further:

| ip dhcp relay information trust-all    | To fix this we have to enable on a DHCP server                                                                                                                                                                                                                                                                                                                                      |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| no ip dhcp snooping information option | This is not needed when above command is configured on the dhcp server cisco device This command is configured on the intermediary switch to disable the insertion of Option 82 DHCP option 82 is inserted by default into DHCP packets when DHCP snooping is enabled Cisco IOS acting as DHCP server can’t handle DHCP Option 82 and therefore sending it out needs to be disabled |

![](<../.gitbook/assets/Unknown image (736)>)

## Dynamic ARP Inspection (DAI)

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

## IP Source Guard (IPSG)

**IP Source Guard (IPSG)** blocks all traffic except DHCP until a valid binding exists. After DHCP creates a binding table entry, IPSG enforces the DHCP-assigned source IP and drops packets without a binding entry. This protects against IP spoofing attacks.

| Switch(config)# ip source binding \[MAC] \[VLAN] \[IP] binding interface <> | ## Creating a static IP-to-MAC binding entry                                                                                                                                             |
| --------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Switch(config-if)# ip verify source \[mac-check]                            | Enables IP source guard with source IP address filtering (second entry in snip) mac-check enables IP Source Guard with source IP address and MAC address filtering (first entry in snip) |
| show ip verify source                                                       |                                                                                                                                                                                          |

![](<../.gitbook/assets/Unknown image (737)>)

## Unicast Reverse Path Forwarding (uRPF)

**Unicast Reverse Path Forwarding (uRPF)** is a mechanism used to prevent IP spoofing attacks in networks.

URPF verifies that incoming packets are arriving on the expected interface for the source IP in the packet's header - comparison is being made against CEF

If the interface from which the packet is received does not match the expected interface for the source IP address, the packet is dropped

Strict mode requires that incoming packets have a source IP address that matches the expected interface based on the routing table e.g ithe interface is used to forward pakcets to the source.

Configure strict uRPF only where there is natural or configured symmetrical routing, meaning packets follow the same path in both directions (to and from the source) .

Internal interfaces are likely to have a routing asymmetry, that is, multiple routes to the source of a packet. This asymmetry can cause valid packets to fail the check and be dropped, therefore, you should not implement strict uRPF on interfaces that are internal to the network and implement it on the network edge, where spoofing is expected

Implementation of strict mode uRPF requires maintenance of a uRPF interfaces list for the prefixes. The uRPF interface list keeps track of interfaces that are expected to receive packets from specific source prefixes. These interfaces are determined by the routing table (prefix path). If multiple prefixes use the same interface, the router shares the interface entry to save resources

Note The behavior of strict uRPF varies slightly on the basis of platforms, the number of recursion levels, and the number of paths in Equal-Cost Multipath (ECMP) scenarios. A platform may switch to loose uRPF check for some or all prefixes, even though strict uRPF is configured. For example, if ECMP Path is eight or more, strict mode is converted to loose mode

![](<../.gitbook/assets/Unknown image (738)>)

Loose mode only requires that the source IP address of an incoming packet has a route in the routing table. If the source IP address has a route in the routing table, the packet is accepted, regardless of which interface it arrives on. This mode is less secure. Used mainly in the network core, where spoofing probability is much lower

Note If the entry points to the Null0 interface, the RPF check fails and packet is dropped - used in RTBH BGP

![](<../.gitbook/assets/Unknown image (739)>)

Note

Normally, a device might block packets it sends to itself (self-ping) because the source and destination are the same.

The allow self-ping option enables the router to accept packets where the source address matches its own address.

This is useful for testing or specific scenarios where the router needs to send and receive packets to/from itself

In some cases, traffic doesn’t have a specific route in the routing table and instead matches a default route (e.g., 0.0.0.0/0). A default route is used as a fallback when no exact match exists.

By default, uRPF might block such traffic because it cannot verify the source IP against a specific route.

The allow default option lets the router process packets that match the default route, as long as they come in through the interface associated with that default route

uRPF Restrictions

![](<../.gitbook/assets/Unknown image (740)>)

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

## Remote Triggered Black Hole Filtering (RTBH)

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

![](<../.gitbook/assets/Unknown image (741)>)

IOS-XR config

![](<../.gitbook/assets/Unknown image (742)>)

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

![](<../.gitbook/assets/Unknown image (743)>)

IOS-XR config

In the example, strict uRPF is enabled on the PE2 router on the GigabitEthernet0/0/0/0 interface using the ipv4 verify unicast source reachable-via rx command. A router that receives a packet will perform the routing table lookup for the source IP address. If the packet came from the attacker, the router will see that the routing table for the attacker IP addresses points to the 192.0.2.1 next-hop IP address, which in turn points to the null interface. The packet will be dropped because the packet was received through the other interface that would be used to forward the return traffic.

{% hint style="info" %}
Source-based RTBH requires uRPF because routing decisions are destination-based, so the static Null0 route only marks the attacker IP as invalid, while uRPF uses that information to actually drop packets based on their source address.
{% endhint %}

![](<../.gitbook/assets/Unknown image (744)>)

## Demilitarized Zone (DMZ)

**Demilitarized Zone (DMZ)** is a security zone between an internal network and the public Internet. It hosts services accessible from both sides (for example, web/email/DNS), while internal-only services (for example, databases) stay on private networks.

## Cisco Zone-Based Firewall (ZBFW)

**Cisco Zone-Based Firewall (ZBFW)** is an integrated stateful firewall feature in IOS. It reduces the need for a separate branch firewall while still providing stateful network security.

Router interfaces are assigned to a specific zone. A zone establishes a security border on the network and defines acceptable traffic that is allowed to pass between zones

By default, interfaces in the same security zone can communicate freely with each other, but interfaces in different zones cannot communicate with each other

**Self Zone** is a system-level zone that includes the router’s own IP addresses used for management (SSH/SNMP) and control-plane protocols (EIGRP/BGP).

After a policy is applied to the self zone and another security zone, inter-zone communication must be defined

**The Default Zone**

system-level zone, and any interface that is not a member of another security zone is placed in this zone automatically

When an interface that is not associated in a security zone sends traffic to an interface that is in a security zone, the traffic is dropped

![](<../.gitbook/assets/Unknown image (745)>)

#### Example: Internet-facing router interface

| A zone needs to be created for the outside zone (the Internet). The self zone is defined automatically Zone security OUTSIDE description OUTSIDE Zone used for Internet Interface                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Define instpection class-map for traffic that is permitted or denied to be received from INET ip access-list extended ACL-IPSEC permit udp any any eq non500-isakmp permit udp any any eq isakmp ip access-list extended ACL-PING-AND-TRACEROUTE permit icmp any any echo permit icmp any any echo-reply permit icmp any any ttl-exceeded permit icmp any any port-unreachable permit udp any any range 33434 33463 ttl eq 1 ip access-list extended ACL-ESP permit esp any any ip access-list extended ACL-DHCP-IN permit udp any eq bootps any eq bootpc ip access-list extended ACL-GRE permit gre any any class-map type inspect match-any CLASS-OUTSIDE-TO-SELF-INSPECT match access-group name ACL-IPSEC match access-group name ACL-PING-AND-TRACEROUTE class-map type inspect match-any CLASS-OUTSIDE-TO-SELF-PASS match access-group name ACL-ESP match access-group name ACL-DHCP-IN match access-group name ACL-GRE | #show class-map type inspect \[class-name]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Define the inspection policy map, which applies firewall policy actions to the class maps defined in the policy map policy-map type inspect POLICY-OUTSIDE-TO-SELF class type inspect CLASS-OUTSIDE-TO-SELF-INSPECT inspect class type inspect CLASS-OUTSIDE-TO-SELF-PASS pass class class-default drop                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | The inspect policy map has an implicit class default that uses a default drop action. This provides the same implicit “deny all” as an ACL #show policy-map type inspect \[policy-name]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Apply a policy map to a traffic flow source to a destination zone-pair security OUTSIDE-TO-SELF source OUTSIDE destination self service-policy type inspect POLICY-OUTSIDE-TO-SELF                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Apply the security zones to the appropriate interfaces interface GigabitEthernet 0/2 zone-member security OUTSIDE                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | #show policy-map type inspect zone-pair                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Router also have to allow locally originated packets with a self-to-outside policy                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ip access-list extended ACL-DHCP-OUT permit udp any eq bootpc any eq bootps ! ip access-list extended ACL-ICMP permit icmp any any ! class-map type inspect match-any CLASS-SELF-TO-OUTSIDE-INSPECT match access-group name ACL-IPSEC match access-group name ACL-ICMP class-map type inspect match-any CLASS-SELF-TO-OUTSIDE-PASS match access-group name ACL-ESP match access-group name ACL-DHCP-OUT ! policy-map type inspect POLICY-SELF-TO-OUTSIDE class type inspect CLASS-SELF-TO-OUTSIDE-INSPECT inspect class type inspect CLASS-SELF-TO-OUTSIDE-PASS pass class class-default drop log ! zone-pair security SELF-TO-OUTSIDE source self destination OUTSIDE service-policy type inspect POLICY-SELF-TO-OUTSIDE                                                                                                                                                                                                      | drop \[log]: This default action silently discards packets that match the class map. The log keyword adds syslog information that includes source and destination information pass \[log]: This action makes the router forward packets from the source zone to the destination zone. Packets are forwarded in only one direction. A policy must be applied for traffic to be forwarded in the opposite direction. Useful for protocols like IPsec, ESP, and other inherently secure protocols with predictable behavior. inspect: offers state-based traffic control. The router maintains connection/session information and permits return traffic from the destination zone without the need to specify it in a second policy |

## .1X EAP MAB

Network access control at the access layer can be managed by using the IEEE 802.1X protocol to secure the physical ports where end users connect. A network where each user is verified before they access it is called an identity-based network.

Identity-based networking allows you to verify users when they connect to a switch port. Identity-based networking authenticates users and places them in the right VLAN, based on their identity. Should any users fail to pass the authentication process, their access can be rejected, or they might be simply put in a guest VLAN.

**802.1X (.1x)** is an IEEE standard that defines port-based access control (PBAC) for wired and wireless networks (a client can associate with an AP, but won’t be allowed network access until authenticated).

### Extensible Authentication Protocol (EAP)

**Extensible Authentication Protocol (EAP)** is an authentication framework integrated with 802.1X. It provides an encapsulated mechanism for transporting authentication parameters to ensure Port Level Security (PLS).

**EAP over LAN (EAPoL)** is a Layer 2 encapsulation protocol for wired and wireless networks.

The IEEE 802.1X standard allows you to implement identity-based networking based on a client-server access control model. The following three roles are defined by the standard:

**Supplicant** endpoint device that is requesting access. Software on the endpoint communicates and provides identity credentials through EAPoL to the authenticator

**Authenticator** acts as a proxy. It requests identity from the supplicant, verifies it with the authentication server, and relays the result back. If authentication succeeds, it opens access. If authentication fails, it blocks access. It also acts as an EAPoL/RADIUS translator and has no visibility into the EAP method in use.

**Authentication Server (AS)** validates endpoint identity and returns the authorization result based on a user database and policies.

Usually a RADIUS server (port 1812) like Cisco ISE, Windows Server AD, etc..

![](<../.gitbook/assets/Unknown image (96)>)

**Unauthorized** Initial state. Ingress traffic is blocked except for 802.1X authentication, CDP and STP packets.

**Authorized** Supplicant is successfully authenticated. All ingress traffic is allowed and flows normally.

Note If a port is configured as a Voice VLAN port, VoIP traffic and 802.1x packets are allowed before the supplicant is authenticated

| ## Reauthenticating a supplicant manually dot1x re-authenticate interface clear authentication session interface | also shut and unshut the port to re-initiate the authentication |
| ---------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |

### Host modes

**Single-host mode**: Only one host can be attached and authenticated on a port.

**Multi-host mode**: Multiple hosts can be attached to a port but only one host must authenticate for all other hosts to have access to the network.

**Multi-auth mode**: Multiple hosts can be attached to a port and all hosts must authenticate themselves to have access to the network.

### Operation (802.1X)

1. Session initiation:

When the authenticator notices a port coming up, it starts the authentication process by sending periodic EAP-request frames

The client sends a request to initiate the authentication or the authenticator detects a link on a port and initiates the authentication.

The supplicant can also initiate the authentication process by sending an EAPoL-start message to the authenticator

2.

Session authentication: The authenticator relays messages between the client and the authentication (RADIUS) server. The client sends the credentials to the RADIUS server.

The authenticator relays EAP messages between the supplicant and AS, copying the EAP message in the EAPoL frame to an AV-pair inside a RADIUS packet and vice versa until an EAP method is selected

3. Session authorization: The RADIUS server validates the received credentials and if valid credentials were submitted, the server sends a message to the authenticator to allow the client access to the port. If the credentials are not valid, the RADIUS server sends a message to the authenticator to deny access to the client.

If authentication is successful, the authentication server returns a RADIUS access-accept message with an encapsulated EAP-success message, then the authenticator opens the port

4. Session accounting: When the client is connected to the network, the authenticator collects the session data and sends it to the RADIUS server.
5. Session termination: When the client disconnects from the network, the session is terminated immediately.

![](<../.gitbook/assets/Unknown image (97)>)

### Configuration

| Switch(config)# aaa new-model                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | ## Enabling AAA services globally                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Switch(config)# dot1x system-auth-control                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | ## Enabling AAA dot1x authentication globally                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Switch(config)# radius server Switch(config-radius-server)# address \[ipv4 \| ipv6] auth-port acct-port Switch(config-radius-server)# key                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | ## Configuring a RADIUS server                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ## Configuring AAA dot1x authentication to use RADIUS Switch(config)# aaa authentication dot1x \[default \| ] group radius                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ## Enabling AAA dot1x authentication on an interface Switch(config)# interface Switch(config-if)# dot1x pae authenticator Switch(config-if)# authentication port-control auto                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | auto: The port is in an unauthorized state until the connected device initiates and successfully authorizes itself with the AAA server. force-unauthorized: The port is always unauthorized. Connected devices cannot initiate an authorization process. force-authorized: The port is always authorized and ready to use. No authorization is required. This is the default mode.                                                                                                                                                                                      |
| Example interface TenGigabitEthernet1/0/47 description AOCNHTWA05AP11 Wireless AP switchport access vlan 408 switchport mode access device-tracking attach-policy SSC\_IPDT authentication control-direction in authentication event fail retry 0 action authorize vlan 3024 authentication event server dead action authorize vlan 408 authentication event no-response action authorize vlan 3024 authentication event server alive action reinitialize authentication host-mode multi-domain authentication port-control auto authentication periodic authentication timer reauthenticate 36000 snmp trap mac-notification change added snmp trap mac-notification change removed mab dot1x pae authenticator dot1x timeout quiet-period 10 dot1x timeout tx-period 1 dot1x timeout supp-timeout 1 spanning-tree portfast |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Switch Flex-Auth with guest vlan fallback config                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| (config-if-range)#authentication priority dot1x mab                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Configure the authentication method priority on the switchports.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| (config-if-range)#authentication order dot1x mab                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Configure the authentication method order on the switchports.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| (config-if-range)#authentication event fail action authorize vlan vlan-id                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Specifies an active VLAN as an 802.1X auth fail VLAN                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| (config-if-range)#authentication event fail retry 4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Specifies a number of authentication attempts before a port moves to the auth fail VLAN. The range is 0 to 5, and the default is 2 attempts after the initial failed event.                                                                                                                                                                                                                                                                                                                                                                                             |
| (config-if-range)#authentication event server dead action reinitialize vlan <>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Configure the port to use a local VLAN when the RADIUS server is down                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| (config-if-range)#authentication event server dead action authorize voice                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | For voice, when RADIUS is down                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| (config-if-range)#authentication host-mode multi-auth                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Set the host mode of the port.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| (config-if-range)#authentication port-control auto                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Enables 802.1X authentication on the port                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| (config-if-range)#authentication violation restrict                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Configure the violation action. When an authentication violation occurs, such as when there are more MAC addresses than are allowed on the port, the default action is to put the port into an error-disabled state. Although this behavior may seem to be nice and secure, it can create an accidental denial of service, especially during the initial phases of deployment. Therefore, we will set the action to be restricted. This mode of operation will allow the first authenticated device to continue with its authorization and deny any additional devices. |

#### 802.1X template example

| template 802.11X ! template dot1x dot1x pae authenticator dot1x timeout tx-period 5 switchport mode access switchport nonegotiate switchport voice vlan 151 spanning-tree portfast edge mab access-session host-mode multi-domain access-session control-direction in access-session closed access-session port-control auto authentication periodic authentication timer reauthenticate server service-policy type control subscriber MAB-DOT1X description USER\_@NOMON ! interface GigabitEthernet6/26 ip device tracking maximum 65535 source template dot1x |   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

### EAP method types

EAP-MD5 // deprecated

Uses the MD5 message-digest algorithm to hide the credentials. Supplicant does not validate the AS to see if it is trustworthy.

This lack of mutual authentication makes it a poor choice as an authentication method

LEAP (Lightweight EAP) // deprecated

Clients must provide a username and password to authenticate

Mutual authentication is provided by both the client and server sending a challenge phrase to each other

EAP-FAST (EAP Flexible Authentication via Secure Tunneling)

developed by Cisco. There is no client or server certificates used in EAP-FAST. Consists of three phases:

PAC (Protected Access Credential) is generated and passed from the server to the client

PAC is similar to a secure cookie, stored locally on the host as “proof” of a successful authentication

Secure TLS tunnel is established between the client and AS

Inside of the secure (encrypted) TLS tunnel the client and server further authenticate/authorize the client

![](<../.gitbook/assets/Unknown image (98)>)

PEAP (Protected EAP)

Like EAP-FAST, PEAP involves establishing a secure TLS tunnel between the client and server. Instead of a PAC, the server has a digital certificate

The client uses this digital certificate to authenticate the server. The certificate is also used to establish a TLS tunnel

EAP-MS-CHAPv2 (Microsoft Challenge-Handshake Authentication Protocol) (PEAPv0) in this inner method, client’s credentials are sent to server encrypted within a session

EAP-GTC (PEAPv1) created by Cisco as an alternative to MSCHAPv2

![](<../.gitbook/assets/Unknown image (99)>)

EAP chaining supports machine and user authentication inside a single outer TLS tunnel

It enables machine and user authentication to be combined into a single overall authentication result

This allows the assignment of greater privileges to users who connect to the network using corporate managed device

EAP-TTLS

same as PEAP, supporting legacy Password Authentication Protocol (PAP) and Authentication Protocol (CHAP)

EAP-TLS (EAP Transport Layer Security)

most secure, but difficult to implement as it requires to install certificate for both, the AS and each client (signed by a Certificate Authority (CA))

Uses TLS Public Key Infrastructure (PKI) certificate authentication mechanism to provide mutual authentication

![](<../.gitbook/assets/Unknown image (100)>)

Local EAP

built on WLC meant for a small environment without RADIUS server

GUI: Security > Local EAP > Profiles; if RADIUS is still checked in Security > AAA > RADIUS > Authentication, then local EAP will then be used as a backup method, so uncheck RADIUS option and check EAP in WLANs > Security > AAA servers tab. You can create users by navigating to Security > AAA > Local Net Users

### MAC Authentication Bypass (MAB)

**MAC Authentication Bypass (MAB)** is a Port Level Security (PLS) feature that uses an endpoint MAC address for access control. It is commonly used as a backup mechanism to 802.1X authentication.

MAB allows ports to be dynamically enabled or disabled based on the MAC address of the endpoint that connects to the port

However, since MAC addresses can be easily spoofed, MAB-authenticated endpoints should be restricted to only communicate with necessary networks and services

#### Operation

1. Switch initiates authentication by sending an EAPoL identity request message to the endpoint every 30 seconds by default

After three timeouts (90 seconds by default), the switch determines that the endpoint does not have a supplicant and proceeds to authenticate it via MAB

2. Switch begins MAB by opening the port to accept a single packet from which it will learn the source MAC address of the endpoint

Packets sent before the port has fallen to MAB (during the IEEE 802.1x timeout phase) are discarded immediately and cannot be used to learn the MAC

3. After the switch learns the source MAC address, it discards the packet. It crafts a RADIUS access-request message using the endpoint’s MAC address as the identity. The RADIUS server

receives the RADIUS access-request message and performs MAC authentication

4. RADIUS server determines whether the device should be granted access to the network and, if so, what level of access to provide.

The RADIUS server sends the RADIUS response (access-accept) to the authenticator, allowing the endpoint to access the network

![](<../.gitbook/assets/Unknown image (101)>)

### Web Authentication (WebAuth)

Cisco switch always attempts 802.1x authentication first, followed by MAB, and if those methods fail, it proceeds to authenticate via WebAuth

This is an Open Authentication option, where client can associate right away, but must open a web browser to see and accept the terms for use and enter basic credentials

This type is used, when endpoints doesn't have .1x supplicants and might not know the MAC address to perform MAB (employees,contractors with misconfigured .1x settings, visitors or guests that need access to the Internet). The endpoint is assigned with IP address, DNS server, and default gateway using DHCP during authentication

Local Web Authentication (LWA)

a switch or WLC sends the login credentials on behalf of the user

Does not support VLAN assignment; it supports only ACL assignment, however cisco switches have the option to assign a guest VLAN to endpoints that don’t have an 802.1x supplicant in order to provide access to the internet for wired users

The database is either internal - stored on WLC or an external stored on RADIUS.

After authentication the WLC redirects the client via an external passthrough splash page, requiring client to acknowledge and accept redirect to the external RADIUS database

Cisco Central Web Authentication (CWA)

is a fallback method of authentication in which the host's Web browser is redirected to a CWA. Offers more advanced services than LWA

Config To create a WLAN with Open Authentication, first create a new WLAN and map it to the correct VLAN.

Go to the General tab and enter the SSID string, apply the appropriate controller interface, and change the status to Enabled. Security > Layer 2 tab > select None.

For passthrough: Security > Layer 3 tab and choose the Layer 3 Security type Web Policy. Security > Web Auth > Web Login Page > define text string that would display during session

### Enhanced Flexible Authentication (FlexAuth)

**Enhanced Flexible Authentication (FlexAuth)** allows using 802.1X and MAB concurrently so endpoints can be authenticated and brought online faster.

Priority and order can be assigned to each authentication method

Multi-Auth mode will allow virtually unlimited MAC addresses per switchport, requiring an authenticated session for every MAC address

Multi-Domain Authentication will allow a single MAC address in the data domain and a single MAC address in the voice domain per port

### Cisco Identity-Based Networking Services (IBNS) 2.0

**Cisco Identity-Based Networking Services (IBNS) 2.0** is an integrated solution offering authentication, access control, and user policy enforcement with a common end-to-end access policy for wired and wireless networks.

Components: Cisco ISE, Enhanced FlexAuth (Access Session Manager) ; Cisco Common Classification Policy Language (C3PL)

### Wireless security

#### Wireless security threats

Evil Twin Attacks

Attackers create fake access points with names similar to legitimate ones, tricking users into connecting to the malicious network. Once connected, the attacker can intercept sensitive data also called eavesdropping

Rogue Access Points

Attackers set up unauthorized access points, mimicking legitimate ones, to deceive users into connecting to them. Once connected, the attacker can eavesdrop, capture data, or launch further attacks on the unsuspecting users

#### Wi-Fi Protected Access (WPA)

**Wi-Fi Protected Access (WPA)** is a security standard for devices with wireless connections. It was developed by the Wi-Fi Alliance to provide stronger encryption and authentication than WEP.

WPA

TKIP (based on WEP) provides encryption/MIC and .1x authentication (Enterprise mode) or PSK (Personal mode)

WPA2

CCMP provides encryption/MIC and .1x authentication (Enterprise mode) or PSK (Personal mode)

WPA3

GCMP provides encryption/MIC. 802.1 X authentication (Enterprise mode) or PSK (Personal mode)

PMF (Protected Management Frames) protecting 802.11 management frames from eavesdropping (unauthorized real-time interception)

SAE (Simultaneous Authentication of Equals) protects the four-way handshake when using personal mode authentication

#### Modes

Enterprise mode

(802.1x + EAP-based authentication)

Personal mode

Pre-Shared Key (PSK) is a secret key string exchanged between Client and WAP, that is used to generate encryption keys for securing data transmission between them

Identity PSKs are unique pre-shared keys created for individuals or groups of users on the same SSID

No complex configuration required for clients, supported on most devices. The same simplicity of PSK, making it ideal for IoT and BYOD

Easy access revoke for a single device or individual, without affecting everyone else

Thousands of keys can easily be managed and distributed via the AAA server

#### Four-way handshake

allows an authenticator and a wireless client to establish an encrypted connection without having to reveal the pass key (Pairwise Master Key or PMK) to each other

Message 1

The wireless access point (WAP) sends an EAPOL-Key frame with nonce value (a random number that can only be used once in a given cryptographic exchange) and connection information to the client. The WAP’s nonce value is called ANonce

With this information, the client is able to derive the pairwise transient key (PTK), which is required to encrypt traffic between the client and the WAP.

Message 2

The client sends its own EAPOL-Key frame with SNonce (its own nonce value), RSN Element, MIC (message integrity code), and authentication to the WAP.

Message 3

After verifying message 2, the WAP sends the ANonce, RSN Element, another MIC, and the group temporal key (GTK) back to the client.

The GTK is used to protect broadcast and multicast frames.

Message 4: After verifying message 3, the client sends confirmation to the WAP that the temporal keys have been installed successfully.

![](<../.gitbook/assets/Unknown image (102)>)

WLC config: WLAN profile > Security > Layer 2 > Select WPA+WPA2 > uncheck lower ver and check AES/CCMP > PSK > PSK string ASCII

#### Certificates note (.pem)

Privacy Enhanced Mail (`.pem`) is a commonly used file format in cryptography and security-related applications. It is often used to store certificates, private keys, and other cryptographic data. In the context of DNAC and WLC, the `.pem` file may contain the necessary certificates and other cryptographic information from DNAC.

The WLC can read this .pem file and use the contained certificates for secure communication and authentication with DNAC during the onboarding process

## IP Security (IPsec)

**IP Security (IPsec)** is a framework of protocols combined to ensure the confidentiality, integrity, and authenticity of data transmitted between devices over unsecured networks such as the Internet

Confidentiality: by encrypting our data, nobody except the sender and receiver will be able to read our data.

Integrity: we want to make sure that nobody changes the data in our packets

By calculating a hash value, the sender and receiver will be able to check if changes have been made to the packet.

Authentication: the sender and receiver will authenticate each other to make sure that we are really talking with the device we intend to.

Anti-replay: even if a packet is encrypted and authenticated, an attacker could try to capture these packets and send them again

By using sequence numbers, IPsec will not transmit any duplicate packets.

Note ACL can be defined to limit the only the intended traffic that should be encrypted and sent through IPsec tunnel

### Internet Key Exchange (IKE)

**Internet Key Exchange (IKE)** is a protocol that ensures authentication between two endpoints by establishing IKE tunnels called security associations (SAs), that carries control and data plane traffic for IPsec

IKEv1 (ISAKMP)

Internet Security Association Key Management uses Oakley Protocol key exchange technique that provides PFS for keys, identity protection, and authentication

Skeme technique provides anonymity and quick key refreshment

Perfect Forward Secrecy (PFS) is a function for phase 2, where another DH exchanges are performed requiring additional CPU cycles, but creates greater resistance to crypto attacks

IKE builds the tunnels for us but it doesn’t authenticate or encrypt user data. We use two other protocols for this:

#### Authentication Header (AH) (not recommended)

AH offers authentication and integrity but it doesn’t offer any encryption. It protects the IP packet by calculating a hash value over almost all fields in the IP header. The fields it excludes are the ones that can be changed in transit (TTL and header checksum)

Does not support encryption and NAT-T; Uses protocol number 51 in the IP header creates a digital signature similar to a checksum to ensure that the packet has not been modified

#### Encapsulating Security Payload (ESP)

**Encapsulating Security Payload (ESP)** ensures that the original payload (before encapsulation) maintains data integrity and confidentiality by encrypting the payload and adding a new set of headers during transport across a public network. ESP supports NAT-T and is identified by the IP protocol number 50 i nthe IP header

Note it’s possible to use AH and ESP at the same time

### IPsec modes

**Tunnel mode** is the default mode, where the entire original IP packet is encrypted and a new IP header is added

This allows to create the overlay in order to establish VPN between sites and allow communication of multiple devices behind the IPsec gateway

**Transport mode** encrypts and authenticates only the packet payload

It routes based on the original IP headers, which saves 20bytes for the header in the contrary of Tunnel mode. So the transport mode is only used for communication of a single device that is generating and receiving the encrypted traffic 1to1 or host to host

Routers however automatically switch the mode to tunnel when a host behind it tries to send data that are matched to be encrypted by the ACL

![](<../.gitbook/assets/Unknown image (955)>)

![](<../.gitbook/assets/Unknown image (956)>)

### IKE phase 1

In IKE phase 1, two peers will negotiate about the encryption, authentication, hashing and other protocols that they want to use and some other parameters that are required. In this phase, an ISAKMP (Internet Security Association and Key Management Protocol) session is established. This is also called the ISAKMP tunnel or IKE phase 1 tunnel.

The collection of parameters that the two devices will use is called a **SA (Security Association)**

The IKE phase 1 tunnel is only used for management traffic. We use this tunnel as a secure method to establish the second tunnel called the IKE phase 2 tunnel or IPsec tunnel and for management traffic like keepalives. Once IKE phase 2 is completed, we have an IKE phase 2 tunnel (or IPsec tunnel) that we can use to protect our user data

Step 1 Negotiation

The peer that has traffic that should be protected will initiate the IKE phase 1 negotiation. The two peers will negotiate about the following items:

1. Authentication to authenticate peers

RSA signatures are cryptographic system used for mutual authentication between peers with public-keys.

It uses digital certificates to authenticate the identity of each peer

Pre-Shared Key locally configured key used as a credential for mutual authentication between peers

1. Hashing to verify the integrity

Message Digest 5 (MD5) A one-way, 128-bit hash algorithm used for data authentication. Weak

Secure Hash Algorithm (SHA) A one-way, 160-bit hash algorithm used for data authentication

SHA-1 HMAC (Hash Message Authentication Code) is Cisco version, providing protection against MitM attacks

1. Diffie-Hellman (DH) generates shared secret symmetric keys used by two VPN peers for symmetrical algorithms, such as AES

The DH exchange itself is asymmetrical and CPU intensive, and the resulting shared secret keys that are generated are symmetrical

DH group refers to the length e.g strength of the key (modulus size) used for DH key exchange

1. Encryption to encrypt the packets

Data Encryption Standard (DES) A 56-bit symmetric data encryption algorithm, however weak

Triple DES (3DES) runs the DES algorithm three times with three different 56-bit keys. Weak

Advanced Encryption Standard (AES) symmetric encryption based on the Rijndael algorithm. Supporting key lengths of 128 bits, 192 bits, or 256 bits

1. Lifetime how long does the IKE phase 1 tunnel stand up? the shorter the lifetime, the more secure it is because rebuilding it means we will also use new keying material. Each vendor uses a different lifetime, a common default value is 86400 seconds (1 day). Only parameter that does not have to match on both peers

Step 2: DH Key Exchange

Once the negotiation has succeeded, the two peers will know what policy to use. They will now use the DH group that they negotiated to exchange keying material. The end result will be that both peers will have a shared key.

Step 3: Authentication

The last step is that the two peers will authenticate each other using the authentication method that they agreed upon on in the negotiation. When the authentication is successful, we have completed IKE phase 1. The end result is a IKE phase 1 tunnel (aka ISAKMP tunnel) which is bidirectional. This means that both peers can send and receive on this tunnel.

The three steps above can be completed using two different modes:

#### Main mode (MM)

more secure with longer negotiation

MM1: first message that the Initiator sends to a Responder. One or multiple SA proposals are offered and the responder needs to match one of the them for this phase to succeed

The SA proposals include different transform sets

MM2: sent from the responder to the initiator with the SA proposal that it matched

MM3: initiator starts the DH key exchange as agreed in previous steps

MM4: responder sends its own key to the initiator > established encryption for ISAKMP SA

MM5: The initiator starts authentication by sending the peer router its IP address

MM6: The responder sends back a similar packet and authenticates the session > ISAKMP SA established fully

**SPI (Security Parameter Index)** this is an 32-bit identifier so the receiver knows to which flow this packet belongs.

#### Aggressive mode (AM)

only requires three messages to establish the security association. It’s quicker than main mode since it adds all the information required for the DH exchange in the first two messages. Main mode is considered more secure since identification is encrypted, aggressive mode does this in clear-text.

AM1: initiator sends all the information contained in MM1 through MM3 and MM5 at once

AM2: send information contained in MM2, MM4, and MM6.

AM3 The initiator starts authentication by sending the peer router its IP address

### IKE phase 2

After a secure tunnel is established in Phase 1, the next step in setting up the VPN is to negotiate the IPSEc security parameters that will be used to protect the actual data and messages within the tunnel. Rather than negotiate each encryption and authentication protocol individually, the protocols are grouped into Transform Set

Phase 2 establishes separate unidirectional IPsec SAs for securing traffic in each direction (in and out) separately within the IPsec tunnel

There is only one mode to build the IKE phase 2 tunnel which is called quick mode

Just like in IKE phase 1, our peers will negotiate about a number of items:

IPsec Protocol: do we use AH or ESP?

Encapsulation Mode: transport or tunnel mode?

Encryption: what encryption algorithm do we use? DES, 3DES or AES?

Authentication: what authentication algorithm do we use? MD5 or SHA?

Lifetime: how long is the IKE phase 2 tunnel valid? When the tunnel is about to expire, we will refresh the keying material.

(Optional) DH exchange: used for PFS (Perfect Forward Secrecy).

The IPsec Phase 2 can be configured either using crypto map or IPsec profiles, which is new more simplified and organized configuration

#### Quick mode

QM1: The initiator (either peer) can start multiple IPsec SAs in a single exchange message. Includes phase 1 agreed-upon algorithms, as well as what traffic is to be encrypted or secured

QM2: This message from the responder has matching IPsec parameters

QM3: unidirectional IPsec SAs between peers established

QM\_IDLE indicates that SA remains authenticated and may be used for subsequent QM exchange for additional IPsec SAs

![R1 R2 data through IPsec tunnel](<../.gitbook/assets/Unknown image (957)>)

### IPsec VTIs (Virtual Tunnel Interface)

In the old method, an extended ACL must be defined to match which traffic will be encrypted

Since we use GRE as the encapsulation protocol for all IP packet, then we used an ACL to match the GRE packet sourced from 1.1.1.1 destined to 2.2.2.2, because all traffic that goes through the tunnel will encapsulated with the Public IP header defined in the tunnel source and tunnel destination command under the tunnel interface.

Then after setting this ACL, we need the popular crypto map for phase 2 IPsec, under the crypto map, we had to assign the ACL to set address 100 command and set peer 2.2.2.2 command, and the transform set using the set transform-set command, finally we apply the crypto map on the physical interface.

Now, why moving to Tunnel Protection or IPsec Profile?, simply because when we use IPsec with GRE (GRE over IPsec), there are many duplicate configuration:

1. The set peer 2.2.2.2 command under the crypto map has the same meaning as the tunnel destination 2.2.2.2 command under the tunnel interface.
2. The second duplication is for the ACL, previously using the old method crypto ACL, we need to identify the GRE packet and associate this ACL to crypto map using the match address 100, the ACL + match address 100 have the same meaning as the Tunnel source 1.1.1.1 and Tunnel destination 2.2.2.2 commands

With IPsec Profile, you associate the transform-net then you apply the IPsec Profile on the Tunnel interface and thats all

There are is no need of ACL or peer statement, all these information's are already there in the tunnel interface

| GRE over IPSec Crypto map access-list 100 permit gre host 1.1.1.1 host 2.2.2.2 ! crypto ipsec transform-set TS esp-aes esp-sha-hmac ! crypto map TEST set peer 2.2.2.2 match address 100 set transform-set TS ! int G0/0 ip address 1.1.1.1 255.255.255.0 crypto map TEST ! int tunnel 1 ip add 172.16.1.1 255.255.255.0 tunnel source G0/0 tunnel destination 2.2.2.2 | Tunnel Protection (IPsec Profile) crypto ipsec transform-set TS esp-aes esp-sha-hmac ! crypto ipsec profile TEST set transform-set TS ! int tunnel 1 ip add 172.16.1.1 255.255.255.0 tunnel source G0/0 tunnel destination 2.2.2.2 tunnel protection ipsec TEST |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IKEv1 Site-to-site with IPsec profile /w PSK                                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                 |
| 1. ISAKMP policy for IKE SA crypto isakmp policy                                                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                                                                 |
| encryption {des \| 3des \| aes \| aes 192 \| aes 256}                                                                                                                                                                                                                                                                                                                  |                                                                                                                                                                                                                                                                 |
| hash {sha \| sha256 \| sha384 \| md5}                                                                                                                                                                                                                                                                                                                                  |                                                                                                                                                                                                                                                                 |
| authentication {rsa-sig \| rsa-encr \| pre-share}                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                                                                                                 |
| group {1 \| 2 \| 5 \| 14 \| 15 \| 16 \| 19 \| 20 \| 24}                                                                                                                                                                                                                                                                                                                |                                                                                                                                                                                                                                                                 |
| lifetime                                                                                                                                                                                                                                                                                                                                                               |                                                                                                                                                                                                                                                                 |
| 2. Specifies PSK with Peer IP in global crypto keyring \[NAME] pre-shared-key address key                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                 |
| 3. Configure the ISAKMP Profile match identity address keyring                                                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                                 |
| 4. Configure Phase 2 in global crypto ipsec transform-set                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                 |
| mode \[tunnel \| transport]                                                                                                                                                                                                                                                                                                                                            | tunnel is recommended                                                                                                                                                                                                                                           |
| 5. Define IPsec profile crypto ipsec profile                                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                 |
| set transform-set                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                                                                                                 |
| set pfs {group15 \| group16 \| group19 \| group20 \| group24}                                                                                                                                                                                                                                                                                                          |                                                                                                                                                                                                                                                                 |
| set security-association lifetime seconds                                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                 |
| set isakmp-profile                                                                                                                                                                                                                                                                                                                                                     |                                                                                                                                                                                                                                                                 |
| 6. Configure VTI interface and apply profile to it - same as IKEv2                                                                                                                                                                                                                                                                                                     |                                                                                                                                                                                                                                                                 |

### IKEv2

require only 4 messages and less bandwidth to establish an IPsec SA; supports EAP authentication; IKEv2 has built-in support for NAT traversal

IKEv2 doesn’t have a main or aggressive mode for phase 1 and there’s no quick mode in phase 2.

It still has two phases though, phase 1 is called the IKE\_SA\_INIT and the second phase is called IKE\_AUTH. Only four messages are required for the entire exchange

CREATE\_CHILD\_SA exchange is for any additional IPsec SAs

Additional features in compare to v1:

Elliptic Curve Digital Signature Algorithm (ECDSA-SIG) newer and more efficient alternative to public keys

Next generation encryption (NGE) meets the security and scalability requirements for the next two decades. Asymmetric authentication (peers can use different authentication methods)

Anti-DoS IKEv2 detects whether an IPsec router is under attack and prevents resources consumption

#### IKEv2 site-to-site with IPsec profile (PSK)

**Phase 1 configuration**

| 1. IKEv2 Proposal crypto ikev2 proposal \[NAME1] encryption aes-cbc-256 integrity <.. \| sha512>group 14                                                                                                                   | recommended only AES the higher the better 14:2048 bit                                                                                                                                                                               |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 2. IKEv2 Policy crypto ikev2 policy \[NAME2] match fvrf \[WAN-vrf-name] match address local \[local\_WANIP] proposal \[NAME1]                                                                                              | >>>> used in VRF-Aware (F-VRF) deployment                                                                                                                                                                                            |
| 3. IKEv2 Keyring crypto ikev2 keyring \[NAME3] peer \[NAME4] address pre-shared-key                                                                                                                                        |                                                                                                                                                                                                                                      |
| 4. IKEv2 Profile crypto ikev2 profile \[NAME5] match fvrf \[WAN-vrf-name] match identity remote address authentication remote pre-share authentication local pre-share keyring local \[NAME3] dpd \[on-demand \| periodic] | >>>> used in VRF-Aware (F-VRF) deployment Dead Peer Detection (DPD) is used to check the availability of the IKEv2 peer. This helps to ensure that the VPN tunnel is still active and can detect if the peer is no longer responding |

**Phase 2 configuration**

| 5. IPSEC Transform set crypto ipsec transform-set mode \[tunnel \| transport]                                                                                                                                                       | tunnel is recommended                                                                                                                                                                                                                                                                                                  |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 6. IPSEC Profile crypto ipsec profile \[NAME7] set transform-set \[NAME6] set ikev2-profile \[NAME5] set pfs {group15 \| group16 \| group19 \| group20 \| group24} set security-association lifetime \[days\|seconds\|kilobytes> <> | optional optional                                                                                                                                                                                                                                                                                                      |
| 7. Configure VTI interface and apply profile to it interface Tunnel1 tunnel mode ipsec \[ipv4\|ipv6] tunnel protection profile ipsec \[NAME5] tunnel source tunnel destination ip address \<tunnel\_ip>                             |                                                                                                                                                                                                                                                                                                                        |
| Optional MTU adjustment                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                        |
| ip mtu 1400 ip tcp adjust-mss 1360                                                                                                                                                                                                  | Optional MTU adjustments configures Pre-fragmentation under tunnel intf                                                                                                                                                                                                                                                |
| (config)# crypto ikev2 fragmentation mtu 1400                                                                                                                                                                                       | Configures the IKEv2 fragmentation The MTU size refers to the IP or UDP encapsulated IKEv2 packets                                                                                                                                                                                                                     |
| (config)# crypto ipsec fragmentation after-encryption                                                                                                                                                                               | (Optional) enables fragmentation of packets after encryption, which ensures that the entire packet, including the IPsec headers, are considered when determining whether to fragment the packet. This is important because if fragmentation is performed before encryption, the IPsec headers will be left unencrypted |

### Verification

| show crypto isakmp sa                    | Showing IPsec/ISAKMP (IKEv1) Phase 1 security associations                     |
| ---------------------------------------- | ------------------------------------------------------------------------------ |
| show crypto ipsec sa \[detail]           | Showing IPsec Phase 2 security associations and packets that were de/encrypted |
| show crypto ikev2 \[sa\|stats]           | Showing IPsec/IKEv2 Phase 1 security associations                              |
| show crypto session \[detail \| brief]   | General check of the peers                                                     |
| show crypto engine accelerator statistic |                                                                                |
| clear crypto ikev2 sa remote             |                                                                                |
| clear crypto \[session \| remote ]       | to reset associations                                                          |
| Debug crypto ikev2 error                 |                                                                                |
| Debug crypto ikev2 internal              |                                                                                |
| Debug crypto ikev2 packet                |                                                                                |

![](<../.gitbook/assets/Unknown image (958)>)

## Media Access Control Security (MACsec)

**Media Access Control Security (MACsec)** is per-hop MAC-layer (link-layer) encryption. It provides line-rate encryption at Ethernet port speeds (1/10/40/100Gbps) bidirectionally regardless of packet size by executing the encryption function in the PHY. Unlike IPsec, which is typically performed on a centralized ASIC, MACsec is enabled per-port with no performance impact.

It can be configured on the link between switch-to-switch or switch-to-host. The traffic is then unencrypted as it is processed internally within the switch. This allows the switch to look into the inner packets for things like SGT tags to perform packet enforcement or QoS prioritization

Note MACsec was primarily designed to be used in conjunction with IEEE 802.1X

![](<../.gitbook/assets/Unknown image (908)>)

For a router capable of forwarding terabits of traffic, IPsec encryption will be the bottleneck and limiting factor of maximum throughput of the device. For example, if a router has multiterabit forwarding capabilities, and ten 100-GE ports require encryption at line-rate, the MACsec solution offers 1 Tbps of AES-256 encryption on each port, regardless of the packet size, so the overall encryption throughput utilizing MACsec can leverage the full forwarding capability of the router, while also offering encryption of each bit on the Ethernet wire.

While MACsec offers a new set of high-speed encryption capabilities, IPsec is now, and will remain, a vital element to network designs, offering an extremely agile design option when IP (public or private) is the transport available. MACsec offers network designers another option when Ethernet can be leveraged as the endto-end WAN/Metro transport and high-speed encryption is vital to the overall business requirement.

### 802.1AE header (MACsec tag)

16-byte MACsec Security Tag field (802.1AE header) and a 16-byte Integrity Check Value (ICV) field are added.

MACsec EtherType (first two octets): Set to 0x88e5, designating the frame as a MACsec frame

Tag Control Information/Association (TCI/AN) (third octet) Number field, designating the version number and if confidentiality or integrity is used on its own

Short Length (SL) (fourth octet): field designating the length of the encrypted data

Packet Number (octets 5–8): The packet number for replay protection and building of the initialization vector

Secure Channel Identifier (SCI) (octets 9–16): for classifying the connection to the virtual port

![](<../.gitbook/assets/Unknown image (909)>)

MACsec offers complete transparency as it does not deal with the content of the Ethernet frame, but only encrypts it and transmits it. In other words, MACsec doesn't examine what's inside the frame – there could be an IP packet, MPLS label, VLAN tag or anything else, but MACsec doesn't see it and encrypts the entire frame as a single unit

When Cisco says "line rate MACsec", this is after accounting for MACsec overhead of 32 B

### Security Association Protocol (SAP)

**Security Association Protocol (SAP)** is Cisco proprietary and used only between Cisco switches. It has many limitations, so MKA is recommended.

### MACsec Key Agreement (MKA)

The purpose of MKA is to provide a method for discovering MACsec peers and negotiating the security keys needed to secure the link

It is a MACsec keying mechanism that provides the required session keys and manages the required encryption keys. Supported on both Switch-to-host and Switch-to-switch connections

MKA and MACsec are implemented after successful authentication using certificate-based MACsec or Pre Shared Key (PSK) framework

#### MKA terminology

**Connectivity Association (CA)** A security relationship between MACsec-capable devices on a LAN or WAN.

**Master Session Key (MSK)** – generated during EAP exchange. Supplicant and authentication server uses the MSK to generate the CAK and CKN. MSK is not used when MKA Pre-Shared-Key is configured.

**Connectivity Association Key (CAK)** – used by MKA to derive a transient session key called the SAK. Is either a manually entered Pre-shared Key, derived from the MSK if an EAP method is used or a key delivered from a MKA Key Server. The CAK is a long-lived master key used to generate all other keys used for MACsec.

**Connectivity Key Name (CKN)** – used as a container for storing the CAK. CKN is transmitted across the wire in clear text to the peer to assist the peer in validating the CAK

**Key Encrypting Key (KEK)** Used to protect the MACsec keys (SAK).

**Secure Association Key (SAK)** – a key derived from the CAK and used to encrypt data sent between devices

**Key Server (KS)** Responsible for selecting and advertising a cipher suite and generating the SAK.

**Secure Channel Identifier (SCI)** – a concatenation of the MAC Address and the Virtual Port ID. Virtual Port ID can be determined from the IF-ID column.

**Two methods to derive encryption keys:**

When Pre-shared Keys (PSK) are used the pre-shared key (PSK) is equal to the connectivity association key (CAK) and the connectivity association key name (CKN) must be manually entered and is stored in the device’s configuration. The CAK is then used to generate the rest of the MACsec encryption keys (ICK, KEK, and SAK)

When 802.1x EAP-TLS is utilized the master session key (MSK) is generated as a by-product of the EAP Authentication process. The CAK is then derived from the MSK. Unlike the pre-shared key method

where the connectivity association key name is manually entered, the CAK is also derived from the MSK. As withthe pre-shared key method, the remainder of the MACsec keys derived from the CAK

Note the .1x method requires:

Require Certificate Authority

ISE 2.0 +

802.1x AAA config

can use local switch user

CAK and SAK are not visible for user

Device certificates (SUDI) does not have any direct role in generating of secret keys

![](<../.gitbook/assets/Unknown image (910)>)

![](<../.gitbook/assets/Unknown image (911)>)

### Operation

MACsec frames are encrypted and protected with an integrity check value (ICV). When the switch receives frames from the MKA peer, it decrypts them and calculates the correct ICV by using session keys provided by MKA. The switch compares that ICV to the ICV within the frame. If they are not identical, the frame is dropped. The switch also encrypts and adds an ICV to any frames sent over the secured port

The EAP framework implements MKA as a newly defined EAP-over-LAN (EAPOL) packet. EAP authentication produces a master session key (MSK) shared by both partners in the data exchange. Entering the EAP session ID generates a secure connectivity association key name (CKN). The switch acts as the authenticator for both uplink and downlink; and acts as the key server for downlink. It generates a random secure association key (SAK), which is sent to the client partner. The client is never a key server and can only interact with a single MKA entity, the key server. After key derivation and generation, the switch sends periodic transports to the partner at a default interval of 2 seconds.

The packet body in an EAPOL Protocol Data Unit (PDU) is referred to as a MACsec Key Agreement PDU (MKPDU). MKA sessions and participants are deleted when the MKA lifetime (6 seconds) passes with no MKPDU received from a participant. For example, if a MKA peer disconnects, the participant on the switch continues to operate MKA until 6 seconds have elapsed after the last MKPDU is received from the MKA peer.

Note The MSK is delivered in the RADIUS vendor-specific attributes (VSAs) MS-MPPE-Send-Key and MS-MPPE-Recv-Key. Along with the MSK, the authentication server sends an EAP key identifier that is derived from the EAP exchange and is delivered to the authenticator in the EAP Key-Name attribute of the Access-Accept message.

![](<../.gitbook/assets/Unknown image (912)>)

![](<../.gitbook/assets/Unknown image (913)>)

### Rekey

MACsec periodically performs rekey of the SAK based on the rekey period. The rekey period is calculated according to the speed of the port or it can be manually set

It is recommended to configure keys such that there is overlap between the lifetime of the keys so that CAK rekey is successful and there is a seamless transition between the Keys/CA (without any traffic loss or session restart)

show mka sessions can show the rekey counter, the Pairwise CAK rekyes counter indicate that the CAK has been changed

### Fallback key

The Fallback Key feature establishes an MKA session with the pre-shared fallback key whenever the primary pre-shared key (PSK) fails to establish a session because of key mismatch. This feature prevents downtime and ensures traffic hitless scenario during CAK mismatch (primary PSK) between the peers. The purpose of the fallback key chain is to act as a last resort key. The fallback key feature is only applicable for PSK based MKA or MACsec sessions.

### Replay protection window size

Replay protection is a feature provided by MACsec to counter replay attacks. Each encrypted packet is assigned a unique sequence number and the sequence is verified at the remote end. Frames transmitted through a Metro Ethernet service provider network are highly susceptible to reordering due to prioritization and load balancing mechanisms used within the network.

A replay window is necessary to support use of MACsec over provider networks that reorder frames. Frames within the window can be received out of order, but are not replay protected. The default window size is set to 64. Use the macsec replay-protection window-size command to change the replay window size. The range for window size is 0 to 4294967295.

The replay protection window may be set to zero to enforce strict reception ordering and replay protection.

### MKA high availability

feature adds support for Platforms which support SSO redundancy mode. The feature allows to preserve the existing MKA sessions

### MACsec eXtended Packet Numbering (XPN)

MACsec uses Packet Numbers (PN) to ensure no packet is repeated. Every MACsec frame contains 32-bit PN number and it is unique for a given SAK. Upon PN exhaustion, SAK rekey takes place to refresh the keys. Since 32 bits minimum-sized IEEE 802.3 frames can be sent in approximately 3 minutes at 10 Gb/s, this can force SAK rekey.

For higher capacity links like 40 Gb/s, PN will exhaust within few seconds which will bring overhead of frequent SAK rekey to the control plane. XPN feature in MKA/MACsec eliminates this frequent SAK rekey problem that may occur in high capacity links.

XPN cannot be supported on 9400

MACSEC XPN Link work only if the devices on both sides of the link support XPN

When XPN is used, the PN value for MACsec frame would be logically of 64 bit. MACsec frame would contain the lowest 32 bits only and most significant 32 bits would be maintained by the peer itself i.e. both sending and receiving peer. The most significant 32 bits of the PN will be incremented at receiving end when the MSB of LLPN for respective peer is set and the MSB of PN value received in MACsec frame has zero value. By this way, both sending and receiving peer would maintain same PN value without changing MACsec frame structure. Since we are using 64 bit PN value, so it will require years to exhaust the PN and hence the frequent SAK rekey will get eliminated.

### Switch-to-host (downlink) MACsec

Encryption on a link between an endpoint and a switch. The encryption between the endpoint and the switch is handled by the MKA keying protocol. This requires a MACsec-capable switch and a MACsec-capable supplicant on the endpoint (such as Cisco AnyConnect). The encryption on the endpoint may be handled in hardware (if the endpoint possesses the correct hardware) or in software, using the main CPU for encryption and decryption. On cisco switch we can configure encryption manually per port or dynamically as an authorization option from Cisco ISE. If ISE returns an encryption policy with the authorization result, the policy issued by ISE overrides anything set using the switch CLI

### Switch-to-switch (uplink) MACsec

Encryption on a link between switches with 802.1AE. By default, uplink MACsec uses Cisco proprietary SAP encryption. The encryption is the same AES-GCM-128 encryption used with both uplink and downlink MACsec. Uplink MACsec may be achieved manually or dynamically. Dynamic MACsec requires 802.1x authentication between the switches.

On ingress the MACsec frame is decrypted in the PHY prior to performing all ingress functions (MPLS label imposition, queuing, scheduling, access control lists \[ACLs], etc.). On egress, the process is reversed such that Layer 2–Layer 7 services are performed prior to MACsec encryption of the frame, which is done on the PHY

### WAN MACsec

The WAN MACsec offering is standards based but offers additional capabilities not found in earlier MACsec capabilities. More specifically, MACsec can be leveraged by enterprise customers over public carrier Ethernet offerings, allowing customers to adapt to the public carrier Ethernet service offering and capabilities (or restrictions).

New enhancements for WAN MACsec include .1q tag in clear

#### 802.1Q tag in the clear

This enhancement offers the ability to expose the 802.1Q tag outside the encrypted MACsec header. Exposing this field offers a multitude of design options with MACsec

While offering high-speed encryption, the multipoint use case exposes limitations and impracticalities in recent MACsec-offered solutions. Why: Because earlier MACsec solutions did not offer the ability to expose the 802.1Q tag in the header, requiring a physical Ethernet connection on the central site, per branch. This was not a realistic design due to complexity of cabling, cost of each port, and “box” real estate required in the router to terminate this 1-to-1 remote site to physical-port requirement.

As described earlier, and as shown in Figure 10, the original MACsec header format encoded the 802.1Q tag as part of the encrypted payload, thus hiding it from the public Ethernet transport, which also limited the topologies and network design options that could be leveraged when transporting Ethernet frames over public or private

Ethernet.

One of the primary use cases designers are looking to leverage with this new “tag in the clear” capability is the ability to build hub/spoke networks with WAN MACsec over public Ethernet Virtual Private Line (E-LINE) services.

It should be noted that while Cisco WAN MACsec solution can leverage tag in the clear for virtual segmentation of connections, these tags can also leverage the 802.1p bits carried in that tag, for QoS service offerings. Without this capability, the QoS offerings will be much more coarse and typically very limited.

![](<../.gitbook/assets/Unknown image (914)>)

![](<../.gitbook/assets/Unknown image (915)>)

### Use cases

WAN MACsec is used mainly for Secure High-Speed Data Center

Cloud Interconnection or Secure High-Speed Branch Router Backhaul

Carrier Ethernet Transport

To elaborate further, Metro Ethernet Forum (MEF) standards dictate a specific set of well known MAC addresses deemed as “for me” frames to the carrier Ethernet forwarders—meaning transit Carrier Ethernet switches consume the frames containing these MAC addresses into their control plane for processing. Current MACsec and MKA implementations leverage an EAP over LAN (EAPoL) packet for MKA key negotiation and these EAPoL MAC addresses fall under the MEF “well known” MAC addresses for consumption. This means that customers deploying

MACsec over a public Carrier Ethernet transport that operate this Ethernet service with Carrier Ethernet switches that consume these EAPoL frames, cannot leverage MACsec across these providers.

To mitigate this problem, Cisco introduced the ability for the operator deploying WAN MACsec to change the EAPoL destination address and/or EtherType to an address that is defined in the provider’s bridge as “uninteresting.”

The “eapol destination-address” command allows the operator to change the destination MAC address of an EAPoL packet that is transmitted on an interface towards the service provider Ethernet transport

Network designers, for example, can now leverage this ability to apply a logical Layer 3 subinterface per remote site that is a subrate of bandwidth from say the 10 GE PHY, offering subrate capabilities from the physical bandwidth. The subinterface can support E-LINE or E-LAN services and can also support hierarchical traffic shaping to align with the prescribed subrate interface

When planning for a smaller number of remote branches, it's crucial to consider the Security Association (SA) scale limitations of the PHY layer for MACsec. Each physical Ethernet interface that supports MACsec has a vendor-defined limit on the number of SAs it can handle.

For example, on the Cisco ASR 1001-X, each 10GE PHY interface supports up to 64 SAs. However, to accommodate hitless key rollover, this number is effectively halved, reducing the practical limit to 32 branch sites per interface. In contrast, on the ASR 9000, a 100GE interface can support up to 256 SAs, highlighting that SA capacity is hardware-dependent and a key consideration when designing WAN MACsec deployments.

Important Note: The SA limit applies per interface, not per router. A hub site router can scale beyond these limitations by leveraging multiple 10GE interfaces.

Secure IP/MPLS and Metro Ethernet Backbone Networks

The per-hop encryption of MACsec offers high-speed per link encryption, while offering complete transparency to the functions of an IP/MPLS architecture

MACsec is being leveraged as the encryption recommendation for newly offered segment routing

capabilities, for all of the reasons listed previously, specifically offering 100-Gbps encryption while remaining transparent to the segment routing control and data plane functions and service requirements needed per hop.

Secure PE-CE Links for Managed Private IP VPN Transport

In the case of an service provider (SP)-managed service, SPs could leverage WAN MACsec to offer customers a secure encrypted PE to CE backhaul link to the provider cloud (e.g. PE device). The SP could extend this security service end to end if the SP expanded MACsec into their IP/MPLS MPLS backbone

It would eliminate the need for the CE to deploy IPsec overlay solutions (DMVPN or GET VPN). This type of deployment would greatly reduce the complexity for the end customers, not having to deploy a secure IP VPN overlay, while reducing operating expenses (OpEx) on the SP side and expanding the SP service catalog to their end customers

Hybrid Design Using WAN MACsec with IPsec

Consider the hybrid design example in Figure 20. In a typical 2-tier design, the option would be to leverage WAN MACsec in the IP/MPLS backbone (regional hubs and DC edge routers) where links speeds could target 10-100 Gbps. The branch locations, typically requiring lower speed links but higher volume of locations, can leverage IPsec with DMVPN or Cisco IWAN, to take advantage of the higher scale site termination IPsec and DMVPN offers.

This hybrid encryption design approach leverages the strengths of each encryption technology, with IPSec targeting higher scale SAs with lower encryption throughput, and MACsec optimizing the solution through extremely high-speed, lower-scale SAs and transparency for MPLS labels, Segment Routing, without the need for MPLS over GRE tunnels.

![](<../.gitbook/assets/Unknown image (916)>)

### Comparing MACsec to IPsec

While WAN MACsec is that de facto high-speed solution moving forward, it should not be thought of as a replacement for IPSec, but rather another set of tools in the encryption tool bag moving forward, and in some cases, deployed in combination with IPsec in larger scale deployments

● MACsec supports line-rate encryption performance (100 Gbps+), regardless of the MTU and packet size

● MACsec is transparent to upper layer protocols (IPv4/v6, MPLS labels)

● IPsec is extremely flexible from an underlying transport perspective (completely agnostic)

● IPsec supports massive scale (DMVPN moving beyond 4000 connections) from an SA termination perspective

● MACsec support will be dictated by the hardware’s Ethernet PHY capabilities

![](<../.gitbook/assets/Unknown image (917)>)

[https://www.cisco.com/c/dam/en/us/td/docs/solutions/Enterprise/Security/MACsec/WP-High-Speed-WAN-Encrypt-MACsec.pdf](https://www.cisco.com/c/dam/en/us/td/docs/solutions/Enterprise/Security/MACsec/WP-High-Speed-WAN-Encrypt-MACsec.pdf)

### Configuration (switch-to-switch MACsec)

![](<../.gitbook/assets/Unknown image (918)>)

<\<MACsec\_C9k.pptx>>

| Configure key chain for PSK                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| key chain $key\_chain\_name macsec                                                                                                                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| key                                                                                                                                                                                                                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| cryptographic-algorithm aes-256-cmac                                                                                                                                                                                                                                                                              | Encryption algorithm for MKA Control packets                                                                                                                                                                                                                                                                                                                                                                                                                              |
| key-string <32/64 hex>                                                                                                                                                                                                                                                                                            | depends on type of algorithm used 256 requires 64, for 128 32 is enough                                                                                                                                                                                                                                                                                                                                                                                                   |
| Configure MKA policy                                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| mka policy $policy\_name                                                                                                                                                                                                                                                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| macsec-cipher-suite gcm-aes-128                                                                                                                                                                                                                                                                                   | Encryption algorithm for Data packets                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Configure macsec on interface                                                                                                                                                                                                                                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| int <>                                                                                                                                                                                                                                                                                                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| macsec access-control should-secure                                                                                                                                                                                                                                                                               | Should-Secure (default): The switch attempts MKA. If MKA succeeds, the switch sends and receives encrypted traffic only. If MKA times out or fails, the network device permits unencrypted traffic. Must-Secure: The network device attempts MKA. If MKA succeeds, only encrypted traffic is sent or received. If MKA times out or fails, the connection is treated as an authorization failure by terminating the session and retry authentication after a quiet period. |
| macsec network-link                                                                                                                                                                                                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| mka policy $policy\_name                                                                                                                                                                                                                                                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| mka pre-shared-key key-chain $key\_chain\_name                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Example key chain KEY macsec key CAFE cryptographic-algorithm aes-256-cmac key-string 1234567890123456789012345678901212345678901234567890123456789012 mka policy MACSEC macsec-cipher-suite gcm-aes-256 interface TenGigabitEthernet1/1/3 macsec network-link mka policy MACSEC mka pre-shared-key key-chain KEY | Note one mka policy can be utilized by multiple ports                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ROLLBACK interface no macsec network-link no mka policy $policy\_name no mka pre-shared-key key-chain $key\_chain\_name ! no mka policy $policy\_name no key chain $key\_chain\_name macsec                                                                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| show mka \[policy \| session \| statistics]                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| show macsec interface                                                                                                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |

## Virtual Private Network (VPN)

The easiest and cheapest way to achieve mutual communication and data exchange today is to use the Internet

Even some companies use the Internet to communicate between their branches or offices spread all over the country, because it is much less expensive than to bring your own cable across the republic, or the whole country, to connect company offices

But the connection via the Internet is very dangerous, because anyone can connect through it and by various ways of attack can influence or eavesdrop on the flow of data, which can cause companies great financial damage, or gain your private data

This is why various technologies have been invented to encrypt data and establish secure connections, thus preventing almost any threat to the Internet.

One of them is the concept of **Virtual Private Network (VPN)**, which allows private networks to communicate with each other.

**Virtual Private Network (VPN)** is an overlay network that allows private networks to communicate with each other across a shared or untrusted network such as the Internet.

It aims to provide the same policies and services as a private network, even though they won't own the network and won't control it as much

Without VPN, the MITM attack can spoof the website that is accessed even when HTTPS is used

With a VPN, internet traffic is encrypted and routed through the VPN server, preventing others on the network from discerning the websites being visited, even if they don't use HTTPs

![diagram](<../.gitbook/assets/Unknown image (1742)>)

### Remote-access VPN

**Remote-access VPN** is designed to establish a secure connection between an individual user's device (such as a laptop) and the corporate network.

This type of VPN is commonly used by mobile workers or remote employees who need to access corporate resources from outside the office.

VPN client is a software application or device that enables users to establish a secure connection with a remote Network Access Server (NAS)/VPN Server to access private network over the Internet

VPN server's public IP address is configured in the VPN client application as well as authentication details (username/password,certificate), and encryption settings

The VPN client initiates a handshake with the VPN server, exchanging credentials and negotiating encryption algorithms.

Upon successful authentication, secure tunnel is established between client and server, using tunneling protocols such as IPsec, SSL/TLS , or OpenVPN, granting authorization to the client to access the VPN. The VPN server assigns a private IP from the corporate IP space to the VPN client, so that it can be identified within the VPN

The VPN client software dynamically creates a Virtual Adapter/NIC on the user's device, acting like a regular network interface but routes traffic through the VPN tunnel

The VPN client encrypts all outgoing data using algorithms like AES and sends it through Virtual NIC. Same applies for receiving traffic - the VPN client decrypts it and processes it

Cisco AnyConnect SSL VPN: Allows remote users to access corporate networks from anywhere on the Internet through an SSL VPN client

Clientless Cisco SSL VPN: Allows the user to securely access the corporate LAN from anywhere using a web browser

Once the user is authenticated, the VPN gateway establishes a secure SSL/TLS connection with the user's web browser - other advantage is that the SSL/TLS port is usually open on any kind of firewalls

VPN Concetrator is a dedicated hardware used by large companies that can manage thousands of concurrent VPN connections, and perform authentication and tunnel encryption

![](<../.gitbook/assets/Unknown image (1743)>)

Connection established from SOHO 192.168.0.0/24 to VPN 10.32.0.0/24. The VPN server public IP is 193.239.0.2

![](<../.gitbook/assets/Unknown image (1744)>)

![](<../.gitbook/assets/Unknown image (1745)>)

![](<../.gitbook/assets/Unknown image (1746)>)

![](<../.gitbook/assets/Unknown image (1747)>)

### Site-to-site VPN

for site-to-site VPN there must be a router on each site, that can use technology such as GRE with IPsec to connect two sites via Internet

#### Generic Routing Encapsulation (GRE)

**Generic Routing Encapsulation (GRE)** is a tunneling protocol that creates a logical connection on top of a physical one.

It is used to allow to connect remote sites over an transport underlay network such as Internet, as if they were directly connected by P2P link

Can be used to create VPNs, tunnel traffic through a firewall/ACL or to connect discontiguous networks.

GRE encapsulation is identified in the IP header as IP protocol 47, thus it does not use TCP or UDP

Original IP packet is encapsulated into new GRE, that adds it's new header, which contains the remote endpoint IP address as the destination

The GRE tunnel is up as long as there is underlay IP connectivity between the remote sites

GRE was originally created to provide transport for non-routable legacy protocols such as Internetwork Packet Exchange (IPX) across an IP network

Standalone GRE lacks inherent security features, which is why it is commonly utilized in conjunction with IPsec to enhance the security of encapsulated traffic

![](<../.gitbook/assets/Unknown image (1748)>)

![](<../.gitbook/assets/Unknown image (1749)>)

![Anatomy Of GRE Tunnels | Packet Pushers](<../.gitbook/assets/Unknown image (1750)>)

![](<../.gitbook/assets/Unknown image (1751)>)

| Configuration                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| interface Tunnel0                        | Also referred to as Virtual Tunnel Interface (VTI), is routable interface used to terminate tunnels (including IPsec)                                                                                                                                                                                                                                                                                                                                                                                   |
| tunnel source 100.64.1.1                 | or we can specify the WAN source interface Gi0/1                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| tunnel destination 100.64.2.2            | WAN IP of the destination device                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ip address 192.168.100.1 255.255.255.255 | This is the IP that we want to use to establish connection to the remove device                                                                                                                                                                                                                                                                                                                                                                                                                         |
| tunnel mode gre \[ip\|ipv6]              | this is the default mode of tunnels - not required to be configured                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| bandwidth \[1-10000000]                  | optional; for best-path calculation or QoS                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| keepalive \[seconds \[retries]]          | default is 10sec with 3 retries                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ip mtu mtu <>                            | optional; GRE tunnel additional IP header, so MTU have to be adjusted accordingly to avoid fragmentation that worsens network performance                                                                                                                                                                                                                                                                                                                                                               |
| tunnel ttl <1-255>                       | Traceroute does not display all the hops in the underlay In the same fashion, the packet’s time-to-live (TTL) is encapsulated as part of the payload The original TTL decreases by only one for the GRE tunnel, regardless of the number of hops in the transport network                                                                                                                                                                                                                               |
| tunnel path-mtu-discovery                | When you enable the PMTUD on a GRE tunnel, the GRE packets are sent with the DF bit set and the router responds to the incoming ICMP destination unreachable messages with the reduction of the tunnel MTU size. The decreased MTU can only be inspected with the show interface command. Regardless of the tunnel PMTUD algorithm, the router fragments or rejects the tunneled packets if their size exceeds the IP MTU size configured on the tunnel interface with the ip mtu configuration command |

#### Recursive routing issue

the tunnel source interface IP address should not be advertised into a GRE tunnel, because when the router learns the destination IP address for the tunnel interface through the tunnel itself, it removes the previous entry for the tunnel destination IP address from the routing table, making the tunnel’s destination unreachable and triggers Midchain syslog

\*Jul 9 07:59:50.611: %ADJ-5-PARENT: Midchain parent maintenance for IP midchain out of Tunnel11 - looped chain attempting to stack

\*Jul 9 07:59:54.151: %TUN-5-RECURDOWN: Tunnel11 temporarily disabled due to recursive routing

![GRE Tunnel and Exposed Underlay Routing](<../.gitbook/assets/Unknown image (1752)>)

![Running Routing (EIGRP) On GRE Tunnel Overlay](<../.gitbook/assets/Unknown image (1753)>)

Foxtrot13 is now learning the IP address (10.4.14.2) used as its destination IP address for the tunnel via EIGRP over the tunnel

![Steps for understanding why tunnel went down due to recursive routing](<../.gitbook/assets/Unknown image (1754)>)

Note An VPN Tunnel within specific VRF can use tunnel source IP/interface and tunnel destination of an interface that is not directly associated with the VRF

Example: This tunnel in vrf CZ-DC1-VRF001 use tunnel source IP from an interface in global vrf (to further explain this scenario, this tunnel is to establish P2P connection with a tunnel destination to establish eBGP)

![](<../.gitbook/assets/Unknown image (1755)>)

### FlexVPN

Interoperable with non-Cisco implementations and therefor works with 3rd party devices

Unified CLI for configuring different VPN types:

Site-to-Site

Hub-to-Spoke

Spoke-to-Spoke

Remote Access

### Group Encrypted Transport VPN (GET VPN)

Designed for any-to-any tunnel-less VPNs using the original IP header for routing encrypted traffic across ISP MPLS or private WANs

### Dynamic Multipoint VPN (DMVPN)

**Dynamic Multipoint VPN (DMVPN)** is a hub-and-spoke overlay routing architecture used to build a secure and scalable dynamic VPN over public or private networks such as the Internet.

Each spoke router establish only a single tunnel to the hub router, that enables spokes to create on-demand tunnels for direct communication between them

When new spokes are added to the network, they can simply establish a dynamic tunnel to the hub router without any additional configuration on the hub router

DMVPN is Non-broadcast Multiple Access (NBMA) network, where NMBA address refers to the public IP of a DMVPN router

![](<../.gitbook/assets/Unknown image (1756)>)

#### Components

GRE is used to encapsulate tunnel's private IP inside public IP address. Used only for point-to-point (pure hub-to-spoke)

Multipoint GRE (mGRE) destination IP address for a tunnel interface is not specified as it is being learned dynamically with NHRP, allowing to establish full mesh between routers

(Optional) Routing Protocol an IGP is chosen to exchange routes between Hub and Spoke

(Optional) IPsec to encrypt the GRE-encapsulated traffic

Next Hop Resolution Protocol (NHRP) allows Hub to as an ARP server, that resolves the next-hop address for spoke routers

Next Hop Client (NHC) = Spoke, Register themselves to the server and report their own NBMA address (public IP address)

Next Hop Server (NHS ) = Hub, keeps track of all NBMA addresses (public IP addresses) in its NHRP cache

![DMVPN | zartmann.dk](<../.gitbook/assets/Unknown image (1757)>)

We can use DMVPN in three different ways. They are called phases

#### Phase 1 (obsolete)

was the original implementation of DMVPN. It’s based only around the Hub and spoke model

VPN tunnels are created only between spoke and Hub sites. Traffic between spokes must traverse the hub to reach any other spoke, so the Hub is still in the data plane

Summarization can be performed on the Hub and advertised to spokes, to conserve space in their RIB

Problems

When traffic arrives at the hub, it needs to be decapsulated. It will then be encapsulated again and sent to destination spoke = huge overhead

Split-horizon has to be disabled on the hub tunnel interface to make route advertisements between spokes work

![](<../.gitbook/assets/Unknown image (1758)>)

#### Phase 2 (obsolete)

adds mGRE to hub and spoke routers, so they can communicate directly without having traffic traversing via Hub, so the Hub is now only in the control plane

When a spoke router wants to communicate with another spoke router in the network, it sends an NHRP request to the hub router. The Hub examines it's NHRP mapping table to find the NBMA address for a destination spoke. The Hub will forward the Resolution request to the destination spoke router. Destination spoke router caches the information in the request, and sends an NHRP Resolution Response directly to the source spoke router. Response contains the NBMA address of the destination spoke router, so spokes can find themselves to form dynamic tunnel between them and torn it down when no longer needed. Each spoke maintains permanent static tunnel with the Hub

![](<../.gitbook/assets/Unknown image (1759)>)

First traceroute flows through the hub to terminate dynamic tunnel to destination spoke

![](<../.gitbook/assets/Unknown image (1760)>)

Second traceroute flows directly to the spoke via dynamic tunel

![](<../.gitbook/assets/Unknown image (1761)>)

Limitation of Phase 2

For example Spoke1 will receive route 10.2.0.0/24 with the next hop of 10.0.0.4 even though the route has been “relayed” via the hub. This installs a special CEF entry for 10.0.4.0/24 with the next hop set to 10.0.0.4 and marked as “invalid”. At the same time, the CEF adjacency for 10.0.0.4 is marked as “glean” meaning it needs L3 to L2 lookup to be performed, which is performed by NHRP, after an initial packet is being sent to 10.0.4.0/24

![](<../.gitbook/assets/Unknown image (1762)>)

This invalid CEF entry makes the initial NHRP request packet to be routed using process switching to the Hub, which in turn responds and completes the CEF entry for the spoke

The second main problem with the DMVPN Phase 2 is that when we summarize on the Hub, and since Spoke1 has the summary route 10.0.0.0/8 in the RIB, it will not send NHRP request anymore as it already knows that it should pass any traffic to the Hub, so the traffic to other spoke (Spoke2) will always pass through the Hub

So in order to maintain Phase 2 ability to establish direct communication between the spokes, they have to preserve the next-hop and have unsummarized specific entries for their delegated networks, thus the spoke has to maintain all specific routes in it's RIB, consuming memory. This limits the scalability in large networks

![](<../.gitbook/assets/Unknown image (1763)>)

![](<../.gitbook/assets/Unknown image (1764)>)

#### Phase 3

allows summarization of routes at the Hub, reducing the size of routing table size and improves DMVPN scalability. This phase is also called NRHP Override (NHO)

A spoke will no longer start with an NHRP Resolution Request, instead, a spoke will simply start sending traffic to the hub

The Hub then sends the source spoke an NHRP Redirect message (similar to ICMP redirect message; it is also referred to as Traffic indication) indicating that i doesn't own the network and redirects the source spoke router to the NBMA of the destination spoke. The Redirect message also completes the CEF entry for the Spoke.

Then spoke proceeds to send the NHRP request directly to the destination spoke

The destination spoke receives the request, making him learn about the source spoke, and sends the NHRP resolution to the source spoke to form a dynamic tunnel

| #redirect | is configured on the hub                                                                                                                                                                                              |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| #shortcut | is configured on the spoke with summary route to destination spoke networks with hub as a next hop shortcut command enables the NHRP to dynamically insert the new route or overwrite existing route in routing table |

![](<../.gitbook/assets/Unknown image (1765)>)

![](<../.gitbook/assets/Unknown image (1766)>)

The RIB stays with the summary/default route, and the specific routes are now maintained in the FIB (this is the intentional DMVPN conflict between FIB and RIB)

However when that particular prefix is not summarized at the Hub, it is present in the RIB with % tag - next hop override

![](<../.gitbook/assets/Unknown image (1767)>)

![](<../.gitbook/assets/Unknown image (1768)>)

#### DMVPN and dynamic routing

Multicast can only travel between hub and spoke. This is because DMVPN is an NBMA network, that doesn’t support multicast or broadcast

The only reason we can get it working to the hub is because of the nhrp multicast command we add to the tunnel interface.

As there’s no multicast on spoke-to-spoke tunnels, there is no traditional IGP running between spokes. Routes are learned through the hub router

So, guideline number 1 is, don’t try to run a routing protocol between spokes. Leave it to NHRP to work out the routing for you

Generally avoid phase 2 with dynamic routing, causing the large RIB for each specific route and avoid OSPF since it is difficult to enforce the required hierarchy

![](<../.gitbook/assets/Unknown image (1769)>)

**With OSPF**

as the hubs have a single tunnel interface, spokes that connect to it all have to be in the same area, which limits scalability. This is due to Phase 2 does not support VRF's that allows for the creation of separate routing domains, enabling the spokes to be associated with different OSPF areas

Network type point-to-multipoint is recommended because of no DR/BDR election

Network type broadcast can be used, but then DMVPN is limited to two hubs and the priority must be explicitly configured (255 on the hubs, 0 on the spokes) so that no spoke router becomes the DR/BDR, which woud cause entire DMVPN design to bring down (Since the Hub would lose his DR position and bring down all the tunnels)

with all other OSPF network types you would destroy purpose of dmvpn, since you would have to manualy map each spoke in a topology

**With EIGRP**

In DMVPN, the tunnel interface is used for everything, including learning routes and advertising them back through the same tunnel.

However, split-horizon would block this, and Spoke-2 will never get the route, so the simplest solution is to disable #no ip split-horizon eigrp {AS} on the tunnel interface on hub

Phase 1 and Phase 3 have been designed to bypass the split horizon limitation, enabling the advertisement of summary or default routes from the hub without being blocked

Since the split-horizon is bypassed, you will find that the hub updates the route with it’s own IP as the next-hop IP

This introduces a new problem, especially noticeable in phase 2. If the hub is the next hop for all routes, then all traffic will flow via the hub

This effectively disables spoke-to-spoke tunnels, which is the whole point of phase 2

To remedy this, use #no ip next-hop-self eigrp {AS} on the hub tunnel interface. This prevents the hub from making itself the next hop, allowing spoke-to-spoke tunnels

**With BGP**

iBGP - not optimal. Usable only if all the spokes are part of your organization. Hub as Router Reflector

In phase 1 or phase 3, the hub routers will need to be the next-hop. This is simply configured with the next-hop-self command

In phase 3, even though the hub is the next hop, NHRP shortcuts still enable spoke to spoke communication

eBGP suitable for multi-tenancy deployment with hubs as part of core AS

Hub in one AS and all other spokes in same AS > phase 1 or 3 have to be used to avoid spokes to drop route advertisements from other spokes (BGP loop prevention mechanism to drop advertisements from same AS).

As the hub originates the route, the spoke ASN, is not in the path, and therefore the route is not dropped. Or we can have spokes in different AS to avoid this issue

![](<../.gitbook/assets/Unknown image (1770)>)

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

### Dual / Multi-Hub setup

* The second hub must be registered to the first hub as spoke.
* For each hub, there must be a separate ip nhrp nhs command configured on the spoke(s).

### DMVPN with IPsec

* IPsec should be applied to the tunnel interface to encrypt traffic passing through the tunnel.

### Per-Tunnel QoS

Used to apply QoS on a per hub-to-spoke-tunnel basis. It cannot be applied on dynamic spoke-to-spoke tunnels. Tunnels must be flapped to apply QoS config.

Example commands:

```
Router(config-if)# nhrp map group <group-name> service-policy output <policy-name>  # Per-Tunnel QoS on hub
Router(config-if)# nhrp group <group-name>                                    # Per-Tunnel QoS on spoke
Router# show policy-map multipoint
```

### MPLS over DMVPN

* Used for traffic segmentation (e.g. overlapping address space).
* Based on NHRP instead of LDP (because LDP uses keep-alives and spoke-to-spoke tunnels would never be terminated).
* Configure mpls nhrp on all tunnel interfaces which belong to the DMVPN solution (hub and spokes).
* Configure iBGP VPNv4 between spokes and hub.
* Hub only: summarize routes for each VRF.

Example command:

```
Router(config-if)# mpls nhrp   # Enabling MPLS NHRP on all DMVPN interfaces
```

### ODR (On-Demand Routing)

* Cisco proprietary.
* With ODR enabled in DMVPN, spokes can learn routes dynamically from hub without every spoke learning all other spokes — reduces control traffic.
* Enable ODR on the hub and on each spoke router.
* Biggest concern: CDP timing. CDP frames are sent every 60 seconds. Dropped routes are marked invalid after three CDP intervals (180s) and removed after 4 intervals (240s) — these timers may need adjustment to avoid poor convergence.

### DMVPN with IPv6

* IPv4 only: use only ip nhrp commands.
* IPv4 over IPv6: the ip nhrp nhs must be mapped to the IPv6 NBMA address.
* IPv6 only: use only ipv6 nhrp commands.
* IPv6 over IPv4: the ipv6 nhrp nhs must be mapped to the IPv4 NBMA address.
* For IPv4 over IPv6, configure the tunnel mode:

```
tunnel mode gre multipoint ipv6
```

***

### NHRP Flags

<details>

<summary>NHRP Flags and meanings</summary>

* Authoritative: NHRP information was obtained from a NHS.
* Implicit: NHRP information was obtained/learned from a NHRP resolution request or packet.
* Local: NHRP mapping entry for a network that is local to this router.
* NAT: Remote device supports NHRP NAT extensions (allows dynamic spoke-to-spoke tunnels to/from spokes behind a NAT router).
* Negative: The requested NBMA address for a NHRP mapping could not be obtained.
* (no socket): Router won’t set up IPsec encryption for this mapping because there’s no data traffic that uses this tunnel.
* Registered: NHRP mapping entry was created from a received NHRP registration request.
* Router: NHRP mapping entries for a remote router itself to access a network/host behind it.
* Unique: NHRP mapping entry can’t be overwritten by a different NHRP mapping entry with the same tunnel address but different NBMA address.
* Used: Data packets are being process-switched for the NHRP mapping.

</details>

***

### Configuration Phases (Phase 1 → Phase 2 → Phase 3)

### Phase 1 — Point-to-Point GRE (Hub + Spokes)

Hub (example Tunnel0):

```
interface Tunnel0
 ip address 10.0.0.1 255.255.255.0
 tunnel source 22.22.22.1             ! WAN interface public IP (NBMA)
 ip nhrp network-id 1                 ! Domain ID; must match across routers
 tunnel mode gre multipoint
 ip nhrp map multicast dynamic
 ip mtu 1400
 ip tcp adjust-mss 1360
 ip nhrp authentication cisco
 tunnel key 100
```

Spoke (example Tunnel0):

```
interface Tunnel0
 ip address 10.0.0.2 255.255.255.0
 tunnel source 21.21.21.1
 tunnel destination 22.22.22.1       ! Phase 1 uses p2p GRE; destination is hub NBMA
 ip nhrp network-id 1
 ip nhrp nhs 10.0.0.1 nbma 22.22.22.1 multicast
 ip nhrp map multicast 22.22.22.1
 ip mtu 1400
 ip tcp adjust-mss 1360
 ip nhrp authentication cisco
 tunnel key 100
```

Notes:

* MTU/MSS adjustments listed above are recommended (IPv4: MTU 1400, MSS 1360; IPv6 MSS 1340).
* NHRP authentication must match on hub and spokes for mutual authentication.
* Tunnel key is optional but must match across routers if used.

````
{% endstep %}

{% step %}
## Phase 2 — mGRE (Spoke: remove tunnel destination)

On Spoke:
```text
interface Tunnel0
 no tunnel destination 22.22.22.1
 tunnel mode gre multipoint
````

This enables dynamic spoke-to-spoke tunnels (mGRE). \{% endstep %\}

\{% step %\}

### Phase 3 — Optimizations (redirect/shortcut)

On Hub:

```
interface Tunnel0
 ip nhrp redirect
```

On Spoke:

```
interface Tunnel0
 ip nhrp shortcut
```

Useful commands:

```
show dmvpn [detail]
show ip nhrp [shortcut | redirect]
debug NHRP
debug dmvpn detail crypto
```

\{% endstep %\} \{% endstepper %\}

***

### Other NHRP commands

* ip nhrp registration no-unique\
  Used to allow NHRP registration without requiring a unique registration for each source IP address. Useful when multiple devices behind NAT share a common public IP.

***

### IPsec + Routing Example (Hub)

Example configuration (Hub) that demonstrates applying IPsec protection to the tunnel and EIGRP routing.

```
! Hub - Interfaces
int Loopback0
 ip address 1.1.1.1 255.255.255.255

interface GigabitEthernet1
 ip address 10.10.255.254 255.255.255.0
 no shut

interface Tunnel0
 ip address 192.168.1.254 255.255.255.0
 no ip redirects
 ip mtu 1400
 no ip next-hop-self eigrp 100
 no ip split-horizon eigrp 100
 ip nhrp authentication ccnp123
 ip nhrp network-id 1
 ip tcp adjust-mss 1360
 tunnel source GigabitEthernet1
 tunnel mode gre multipoint
 tunnel key 100
 tunnel protection ipsec profile PROFILE

! IKE / ISAKMP
crypto isakmp policy 1
 encryption aes
 hash sha512
 authentication pre-share
 group 20

crypto isakmp key ccnp123 address 0.0.0.0 0.0.0.0

! IPsec transform/profile
crypto ipsec transform-set IPSEC esp-aes esp-sha512-hmac
 mode transport

crypto ipsec profile PROFILE
 set transform-set IPSEC
 set pfs group24

! EIGRP
router eigrp 100
 network 192.168.1.0 0.0.0.255
 network 1.1.1.1 0.0.0.0
```

***

### Useful "show" and debug commands

* show dmvpn \[detail]
* show ip nhrp \[shortcut | redirect]
* show policy-map multipoint
* debug NHRP
* debug dmvpn detail crypto

***

### Other example snippets / notes

* Use ip nhrp map group with service-policy on a per-tunnel QoS basis:

```
Router(config-if)# nhrp map group <group-name> service-policy output <policy-name>
```

* To enable MPLS over DMVPN interfaces:

```
Router(config-if)# mpls nhrp
```

* Example: ip nhrp registration no-unique

```
ip nhrp registration no-unique
```

***

End of document.
