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

# ARP

### Address Resolution Protocol (ARP)

In modern networks, applications communicate using IP addresses (Layer 3), but actual data transmission occurs at Layer 2 (Ethernet, Wi-Fi, etc.). Since Ethernet frames require MAC addresses to be forwarded over a local network, there must be a mechanism to map an IP address to a MAC address before any communication can take place.

For this purpose an automatic mechanism called **ARP** is used to send a request message for the MAC address of the owner of the IP address

**ARP** is a L2 protocol that serves as a bridge between the L2 and L3 layers and dynamically translates IP address to MAC address and vice versa

All devices in the network maintain ARP cache/table, where they store all resolved all MAC-to-IP address mappings, so that they can construct layer 2 header for subsequent packets without initiating again the broadcast ARP process. Because ARP is a Layer 2 protocol, its scope is limited to the LAN.

Each entry, or row, of the ARP table, has a pair of values—an IPv4 address and a MAC address.

If no device responds to the ARP request, then the original packet is dropped because a frame to put the packet in cannot be created without the destination MAC address.

![](<../.gitbook/assets/Unknown image (709)>)

### ARP process

The sender first check their ARP cache/table. If the MAC address is not in the cache, they send an ARP request as a broadcast with destination IP 255.255.255.255 and MAC FFFF:FFFF:FFFF inquiring about the owner of the MAC address of a given IP in a local subnet. The device owning the IP responds with an unicast ARP reply, updating the requester's ARP cache.

Now the sender can construct the layer 2 header and send the data directly to the owner/destination IP.

If the IP is outside of the local network, the host must first resolve the MAC of it's default gateway, which will respond in the same manner.

[The default gateway/router then receives such packet and determines the next-hop router to reach to the destination. Once it determines the next-hop, it overwrites the destination MAC address with the next-hop router MAC and sends it. The process is repeated until the packet reaches the final destination IP.](https://onenote/#Routing\&section-id={6EC9C333-3E04-41C1-8C01-0E8D85C128A8}\&page-id={23330675-9420-47AE-8368-54C860A78BB9}\&object-id={52581BD7-7745-0862-3434-AF316314E545}&7C\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE/L3.one)

{% hint style="info" %}
ARP request is the reason to why first ping to specific IP address fails, because the device initially needs to perform the ARP to resolve what MAC address the destination IP owns
{% endhint %}

![](<../.gitbook/assets/Unknown image (710)>)

| arp -a                  | to check ARP table on Windows PC The device creates and maintains the ARP table dynamically, adding and changing address relationships as they are used on the local host. The entries in an ARP table expire after a while; the default expiry time for Cisco devices is 4 hours. Other operating systems (Windows, macOS) might have a different value; Windows uses a random value between 15 and 45 seconds. This timeout ensures that the table does not contain information for systems that may be switched off or moved. When the local host wants to transmit data again, the entry in the ARP table is regenerated through the ARP process. |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show arp \| show ip arp | to check ARP table on Cisco IOS device; ARP entries will age out after 4hours in Cisco IOS. Cisco IOS adds random jitter between 0 to 30minutes to the timeout counter of each ARP entry to prevent all ARP entries from expiring at the same time, causing ARP storm, that floods the network with ARP requests                                                                                                                                                                                                                                                                                                                                      |
| arp arpa                | to manually configure ARP entry                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| clear arp               | clears the arp table, the device will send a unicast ARP request to try to refresh each entry before removing it from the table                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

### ARP packet format

![](<../.gitbook/assets/Unknown image (711)>)

The host's MAC address appears twice. Once in the Ethernet frame as a source and once in the payload field (Info). It appears in the source field because the request message is a broadcast sourced from the host. However, the destination cannot learn the MAC address from the frame field because it is discarded during the decapsulation process. Therefore, the MAC address of the

host is also put in the ARP payload, so the ARP protocol on the destination device can retrieve the MAC address and store it in its ARP cache.

![](<../.gitbook/assets/Unknown image (712)>)

Hardware type (HTYPE)

This field specifies the network link protocol type. Example: Ethernet is 1.

Protocol type (PTYPE)

This field specifies the internetwork protocol for which the ARP request is intended. For IPv4, this has the value 0x0800. The permitted PTYPE values share a numbering space with those for EtherType.\[2]\[3]

Hardware length (HLEN)

Length (in octets) of a hardware address. Ethernet address length is 6.

Protocol length (PLEN)

Length (in octets) of internetwork addresses. The internetwork protocol is specified in PTYPE. Example: IPv4 address length is 4.

Operation

Specifies the operation that the sender is performing: 1 for request, 2 for reply.

Sender hardware address (SHA)

Media address of the sender. In an ARP request this field is used to indicate the address of the host sending the request. In an ARP reply this field is used to indicate the address of the host that the request was looking for.

Sender protocol address (SPA)

Internetwork address of the sender.

Target hardware address (THA)

Media address of the intended receiver. In an ARP request this field is ignored. In an ARP reply this field is used to indicate the address of the host that originated the ARP request.

Target protocol address (TPA)

Internetwork address of the intended receiver.

![](<../.gitbook/assets/Unknown image (713)>)

### Reverse Address Resolution Protocol (RARP)

**Reverse Address Resolution Protocol (RARP)** is requesting the IP for a known MAC address, which can happen in diskless booting scenarios where a computer needs to obtain its IP address and other network configuration information (such as subnet mask, default gateway, etc.) from a server. During the boot process, the computer sends out a RARP request containing its MAC address to request its IP address from a RARP server.

### Gratuitous ARP (GARP)

**Gratuitous ARP (GARP)** is an ARP reply message sent without being requested with an ARP request serving as a mechanism to inform the network about a change

It is sent to broadcast MAC address (unlike the regular ARP reply that is unicast) to update ARP table of all hosts

This can happen when router is announcing an interface MAC address state change, when failover between redundant devices in FHRP occurs

It updates the switches MAC address tables and host ARP tables.

![](<../.gitbook/assets/Unknown image (714)>)

### Proxy ARP

When a device wants to communicate with another device on a different network segment, it relies on its default gateway (usually a router) to forward the traffic

In such cases, proxy ARP allows a router to answer ARP requests where the target IP address that is in a different network, that the router can reach. It is enabled by default

IP local proxy ARP allows the router to respond to ARP requests on behalf of devices in the same subnet as the device that sent the ARP request.

So instead of the traffic going directly between hosts in the same subnet, traffic will be sent to the router first

![](<../.gitbook/assets/Unknown image (715)>)
