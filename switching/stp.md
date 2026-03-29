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

# STP

### Issues with redundant topology

LAN networks frequently utilize loop or redundant topologies in order to have always backup link to prevent any outage in case device or link failure

However this loop topology can be susceptible to a problem known as a Broadcast storm, which occurs when broadcast, unknown unicast or multicast frames are endlessly flooded between switches, which duplicates frames that circulate in the network, causing network congestion and network instability

A looped topology is often desired to provide redundancy, but looped traffic is undesirable

Example if below PC sends a broadcast frame, perhaps for ARP or to get IP from DHCP, the switch where the PC is connected floods it to other switches to find the destination device. Each switch that receives the broadcast frame replicates it to all connected devices, and this process repeats each switch in the network causing broadcast storm

And since Layer 2 has no mechanism as Layer 3 has TTL, the STP protol was developed

![](<../.gitbook/assets/Unknown image (672)>)

### Broadcast storm and CAM instability

Host A transmits the frame destined for host B on segment A.

Switch W receives the frame that is destined for host B, learns the MAC address of host A on segment A, and since it doesn't have the MAC of B in it's MAC table it floods it out to switches X and Y.

Switch X and switch Y both receive the frame from host A (via switch W) and correctly learn that host A is on segment 1 for switch X and on segment 2 for switch Y. Switch X and switch Y

then forward the frame to switch Z. Switch Z receives two copies of the frame from host A: one copy through switch X on segment 3 and one copy through switch Y on segment 4.

Assume that the first copy of the frame from switch X arrives first. Switch Z learns that host A resides on segment 3. Because switch Z does not know where host B is connected, it

forwards the frame to all its ports (except the incoming port on segment 3) and therefore to host B and also to switch Y.

When the second copy of the frame from switch Y arrives at switch Z on segment 4, switch Z updates its table to indicate that host A resides on segment 4. Switch Z then forwards the

frame to host B and switch X.

In this example where no loop prevention mechanism exists the result is that host B has received multiple copies of the frame, which can cause problems with the receiving application

directly on the host B.

Switches X and Y now change their internal tables to indicate that host A is on segment 3 for switch X and on segment 4 for switch Y. The copies of the initial frame from host A being

received on different segments of the switches results in MAC database instability.

Furthermore, if the initial frame from host A was a broadcast frame, then all switches forward the frames endlessly. Switches flood broadcast frames to all ports except the port on which the

frame was received. The frames then duplicate and travel endlessly around the loop in all directions. They eventually would use all available network bandwidth and block transmission of

other packets on both segments. This situation results in a broadcast storm.

![](<../.gitbook/assets/Unknown image (673)>)

### Spanning Tree Protocol (STP)

**Spanning Tree Protocol (STP)** solves the issues with loop topology. It uses Spanning-Tree Algorithm (SPA) which disables redundant data paths

![](<../.gitbook/assets/Unknown image (674)>)

A tree is a subgraph in which two or more nodes are connected by exactly one path, and the spanning means that the tree includes all nodes, so the tree spans across all nodes

Physical topology can be considered as the graph, logical topology can be considered as the sub-graph, where the nodes are connected by exactly one path to avoid loop topology

First STP 802.1D is commonly referred to as the Common Spanning Tree (CST)

![](<../.gitbook/assets/Unknown image (675)>)

STP behaves in the following way:

STP uses bridge protocol data units (BPDUs) for communication between switches. Two BPDUs are used: Configuration and Topology change BPDUs

STP forces certain ports into a blocked state so that they do not listen to, forward, or flood data frames.

The overall effect is that only one path to each network segment is active at any time.

If there is a connectivity problem with any active network segment, STP activates a previously inactive path, if one exists (changing the blocked port to the forwarding state).

To prevent Layer 2 loops in a network, STP uses a reference point called the root bridge. The root bridge is the logical center of the spanning tree topology. All paths that are not needed to

reach the root bridge from anywhere in the network are placed in STP blocking mode.

The root bridge is chosen with an election. In the original STP, each switch has a unique 64-bit bridge ID (BID) that consists of the 16-bit bridge priority and 48-bit MAC address as shown in the figure. The bridge priority is a number between 0 and 65535 and the default on Cisco switches is 32768.

### BPDU format

![Spanning-Tree Protocol | zartmann.dk](<../.gitbook/assets/Unknown image (676)>)

**Flags:** Used in response to a Topology change notification (TCN) BPDU

**Root Path Cost:** Identifies the cost from the transmitting switch to the root

**Sender Bridge ID:** Identifies the BID of the transmitting switch

**Port ID:** Identifies the transmitting port MAC address

### Bridge ID (BID)

**Bridge ID (BID)** identifies the bridge ID (BID) of each switch in STP topology

First part is a 2-byte Bridge Priority field (which can be configured) while the second part is the 6-byte MAC address of the switch (Base MAC address)

MAC address of the switch is called the base MAC address, sh ver | i Base

![](<../.gitbook/assets/Unknown image (677)>)

In order to accommodate the additional VLAN information, the Extended System ID field was introduced in new version of STP - PVST, RST and MST, to identify respective VLAN participating in STP, borrowing 12 bits from the original Bridge Priority

The default priority value is 32768 (the most significant bit set to 1) and the priority can be modified only in increments of 4096 (the least significant bit of the four priority bits)

![](<../.gitbook/assets/Unknown image (678)>)

### Enforcing the root bridge

The best way to prevent other devices from taking over the STP root role is to set the bridge priority to 0 for the primary root switch and to 4096 for the secondary root switch.

In addition, root guard should be used. The root bridge should be a swithc within distribution or core layer

### STP algorithm (high level)

#### 1) Root bridge election

When switches in a network are powered up, all of their ports start in the blocked state.

Each initially assumes role of a root , the process of root bridge election begins with the exchange and comparison of configuration Bridge protocol data unit (BPDU) messages sent each Hello time perior every 2 seconds on all ports to a special R/STP (reserved) multicast MAC address 0180.c200.0000 (0100.0ccc.cccd for R/PVST+)

The source MAC address is the MAC address of the port itself.

BPDUs contain information about the switch and its ports, including switch and MAC addresses, switch priority, port priority, and path cost

The switch with the lowest Bridge ID is elected as Root switch (bridge), and it serves as a backbone and logical center of STP topology, with all his ports having in a forwarding state called Designated ports (DP)

Superior BPDU if received BPDU from a neighboring switch contain lower bridge ID, lower path cost, Switches will discard the inferior BPDUs, and propagate only the superior

Inferior BPDU if received BPDU from a neighboring switch contain higher bridge ID, path cost, etc..

Only the root bridge will continue to generate new Configuration BPDU with root cost of 0 to advertise its root bridge status and to convey configuration information to other switches in the network. When non-root switches receives Configuration BPDU, they add the cost of their receiving port to the BPDU and then forward it to other switches out of their Designated ports

By default if the switch that is elected as a root bridge fails, the switch with the next lowest bridge ID takes over the role of a root bridge. Cisco enables the configuration of a primary and scondary root bridge.

![](<../.gitbook/assets/Unknown image (679)>)

| Port Role          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Root port          | This port exists on nonroot bridges. It is the switch port with the best path to the root bridge. Root ports forward traffic toward the root bridge and populate the MAC address table for network segments attached to that port. Only one root port is allowed per switch or per VLAN in Cisco PVST+.                                                                                                                                                                                                                                                                 |
| Designated port    | This port exists on root and nonroot bridges. For root bridges, all switch ports are designated ports. For nonroot bridges, a designated port is the switch port that will receive and forward frames toward the root bridge as needed. Only one designated port is allowed per segment. If multiple switches exist on the same segment, an election process determines the designated port, and the corresponding switch port begins forwarding frames for the segment. Designated ports populate the MAC address table for the network segment attached to that port. |
| Nondesignated port | The nondesignated port is a switch port that it is blocking data frames and is not populating the MAC address table with the source addresses of frames that are seen on that segment.                                                                                                                                                                                                                                                                                                                                                                                  |
| Disabled port      | The disabled port is a switch port that is shut down.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

| Switch(config)# spanning-tree vlan vlan-id priority value OR Switch(config)# spanning-tree vlan vlan-id root \[primary\|secondary] diameter | Default spanning tree configuration for Cisco Catalyst switches includes the following characteristics: PVST+ Enabled on all ports in VLAN 1 |
| ------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Switch(config)# spanning-tree mode {pvst \| mst \| rapid-pvst}                                                                              |                                                                                                                                              |
| show spanning tree                                                                                                                          |                                                                                                                                              |
| show spanning-tree interface1/0/1 detail                                                                                                    |                                                                                                                                              |

#### 2) Root port selection

Each nonroot switch determines a root port (RP). The root port is the port with the best path to the root bridge. The root path cost value is used in this calculation; it is the cumulative STP

cost of all links to the root bridge. The root port is the port with the lowest root path cost to the root bridge.

The STP path cost depends on the speed of the link

STP default port cost:

![](<../.gitbook/assets/Unknown image (680)>)

| switch(config)# spanning-tree pathcost method { long \| short } | The use of 32-bit metrics can be forced using |
| --------------------------------------------------------------- | --------------------------------------------- |

Each port has to transition through following states during the RB/RP election

If a switch port connects to another switch, the STP initialization cycle must transition from state to state to ensure a loop-free topology.

Blocking is the initial (nondesignated) state of all enabled switchports that only receives BPDU's, but the port is not forwarding any traffic to ensure that a loop is not created.

The switch then stays in this state for up to 20 seconds, as soon as the BPDU is received from other switch that already went from the blocking state to the listening (essentially switch that was powered up 20s+ earlier than this switch) it compares the received BPDU's with it's own parameters and determines whether the port would win the DP or RP election and based on that it will transition to the listening state and send BPDU to the neighboring switch to let him know his parameters or it will leave the port in blocking state.

Since the switchport will remain in the blocking state the neighboring switch will not receive any BPDU from the switch, leaving him to assume the DP/RP role

Listening state reaches port that didn't received BPDU with better parameters than it's own. It is thus elected as designated port that that can send or receive BPDUs.

The port remains 15second in this state to listen, whether it receives better or updated BPDU from the neighboring switch. If yes, it will determine if the port should be set back into blocking state or it will sucessfully transition to the learning state. If no updated BPDU has been received, the port transition to listening state

Learning in this designated 15second state a switchport begins to learn MAC addresses, but still forward only BPDUs and not any network traffic

Forwarding is the final designated port state in which the switch can forward network traffic

Disabled is in an administratively off state

Broken configuration or an operational problem on a port

The entire .1D STP initialization time takes about 30 seconds for a port to enter the forwarding state using default timers (50sec including the 20 transition from blocking to listening)

Tie-breakers for switch to determine the RP

* lowest cost to the root wins the selection
* lowest neighbor BID wins
* the lowest neighbor bridge ID wins
* the lowest neighbor port priority wins
* the lowest neighbor internal port number wins

![](<../.gitbook/assets/Unknown image (681)>)

The STP topology (forwarding x blocking ports) can be influenced by manipulating two values:

* Interface COST
* Interface PRIORITY

In the STP calculation process, COST has higher priority

| SwitchA(config-if)# spanning-tree vlan vlan port-priority <0-192> |   |
| ----------------------------------------------------------------- | - |
| SwitchA(config-if)# spanning-tree vlan vlan cost <1-200000000>    |   |

#### 3) Designated port selection

On each segment, a designated port is selected. This is again calculated based on the lowest root path cost. The designated port on a segment is on the switch with the lowest root path cost. On root bridges, all switch ports are designated ports. Each network segment will have one designated port.

The root ports and designated ports transition to the forwarding state and any other ports (called nondesignated ports) stay in the blocking state.

All other interfaces are placed in blocking state, called Non-Designated ports. DP transmits BPDUs, and the non-designated port receives BPDUs

Tie-breakers for switch to determine the DP

* port on a switch with lowest root cost wins the selection
* port on a switch with lowest BID wins
* lowest local port ID

![](<../.gitbook/assets/Unknown image (682)>)

![](<../.gitbook/assets/Unknown image (683)>)

Although the order of the steps that are listed in the diagrams suggests that STP goes through them in a coordinated, sequential manner, that is not actually the case. If you look back at

the description of each step in the process, you see that each switch is going through these steps in parallel. Also, each switch might adapt its selection of root bridge, root ports, and

designated ports as it receives new BPDUs. As the BPDUs are propagated through the network, all switches eventually have a consistent view of the topology of the network. When this

stable state is reached, BPDUs are transmitted only by designated ports. However, all blocking ports are continuously listening for BPDUs that are sent every 2 seconds. If a blocking port

stops receiving BPDUs, it will begin transition to the forwarding state.

There are two loops in the sample topology, meaning that two ports should be in the blocking state to break both loops. The port on Switch C that is not directly connected to Switch B (root

bridge) is blocked, because it is a nondesignated port. The port on Switch D that is not directly connected to Switch B (root bridge) is also blocked, because it is a nondesignated port.

The resulting STP tree can be seen in the preceding figure. STP has its strengths and weaknesses. The strength of STP is that it removes any possible Layer 2 loops found in the topology.

However, it does have a weakness, which is that all traffic must traverse the root bridge. If traffic needs to be sent from a source connected to Switch C, to a destination connected to

Switch D, traffic will be sent through the root bridge, Switch B. Similar to OSPF, STP trees are considered the shortest path taken in the network. In STP there is only one root and the

shortest path is calculated from that device. Meanwhile in OSPF, each router is considered its own root and the Shortest Path First tree is calculated from it. Due to this difference, in STP

there is only one tree per topology while in OSPF, there is a tree for each router participating in OSPF in the topology.

![](<../.gitbook/assets/Unknown image (684)>)

### STP timers

**Max Age:** determines how long a BPDU's information will remain valid (20s default), before ceasing to receive BPDUs to change the topology

**Message Age:** Indicates the age of the current BPDU

**Hello Time:** Identifies the time interval between generation of configuration BPDUs by the Root (2s default)

**Forward Delay:** Defines the time a switch port must wait in the listening and learning state (15sec default)

![](<../.gitbook/assets/Unknown image (685)>)

### Topology changes

If there is a topology change, that is when any port transitions to a different state (failed link), the switch sends an Topology change notification (TCN) BPDU toward the root bridge on it's root port to notify the root of a topology change on a downstream switch. After receiving a TCN, the Root Bridge sets the Topology Change (TC) flag in it's Configuration BPDU for the duration of Max Age + Forward Delay (35 seconds). When a switch receives a Configuration BPDU with the TC bit set, it shortens the MAC address aging timer to Forward Delay (15 seconds). MAC addresses of devices that don't communicate within 15 seconds will be flushed from the table. MAC addresses of communicating devices will be maintained.

This is to prevent sending traffic over a failed link Topology Change Acknowledgment (TCA) bit is on to acknowledge the TCN of sending switch

A switch that has sent a TCN will send one every hello interval until it receives a TCA

The total convergence time for legacy STP was from 30-50 seconds

Example 1

1. SW2 <> SW4 link goes down
2. SW4 send its's own BPDU to SW 5 and declares itself as the root bridge, because it is

not receiving the configuration BPDUs from the Root

3. SW5 receives the SW4's new BPDUs that is inferior to the BPDUs from SW1, so it ignores the SW4's new BPDU until the old BPDU information times out (Max Age Timer)
4. After the Max Age timer expires, SW5 finally reacts to SW4's inferior BPDUs

Its port becomes Designated, and it forwards SW1's superior BPDUs to SW4.

5. SW4 accepts SW1 as the Root Bridge again, and its port becomes a Root port

![](<../.gitbook/assets/Unknown image (686)>)

Example 2

If the SW3-SW4 link goes down, traffic is unaffected because it was already disabled

SW3 will send a TCN, SW1 will acknowledge and set TC flag on BPDUs

SW4 won't send a TCN; a port in the Blocking state moving to the Disabled state doesn't trigger a TCN

![](<../.gitbook/assets/Unknown image (687)>)

Example 3

If the SW2-SW4 link goes down, traffic will be affected until STP reconverges

SW2 will send a TCN out of its Root port (G0/0)

SW1 will acknowledge the TCN and set the TC flag on BPDlJs

SW4 will select G0/1 as its new Root port and send a TCN

SW3 will send a TCN to SW1 and acknowledge SW4's TCN

SW1 will acknowledge SW3's TCN

SW1 will continue to set the TC flag on BPDUs

SW4 will send a TCN again once G0/1 enters the Forwarding state

G0/1 needs to move through Listening and Learning after becoming SW4's RP

and same sequence occurs

![](<../.gitbook/assets/Unknown image (688)>)

Example 4

If the SW1-SW3 link goes down, traffic will be affected until STP reconverges

SW1 (Root bridge) won't send a TCN, instead it will set the TC flag on Configuration BPDUs

SW3, having lost its Root port, doesn't send a TCN and begins to declares itself the Root bridge in it's new BPDU's towards the SW4

Since for SW4 the SW3's BPDUs are inferior to SW1's, it will ignore them

After SW4 G0/1's Max Age timer expires, it will transition from original blocking state to listening>learning and then forwarding and forwards SW1's BPDU's to SW3

SW3 will accept SWI1as the Root bridge again

Reconvergence time: 50 seconds

![](<../.gitbook/assets/Unknown image (689)>)

### PortFast (edge ports)

allows a port to immediately transition to the Forwarding state upon being connected/enabled, bypassing the Listening and Learning states.

Applied on access ports, for end hosts to connect to the network immediately (or on a trunk, to connect servers with multiple virtual hosts) and should be never configured on a connection to another switch that would otherwise form a loop

Host reboots do not impact STP stability or cause a MAC address table flushes when portfast is configured

In RSTP the PortFast feature is known as an edge port concept. All ports directly connected to end stations cannot create bridging loops in the network. Therefore, the edge port directly transitions to the forwarding state, and skips the listening and learning stages. Unlike PortFast, an edge port that receives a BPDU immediately loses its edge port status and becomes a normal spanning-tree port.

{% hint style="info" %}
PortFast does not disable BPDUs on the port.
{% endhint %}

| (config-if)# spanning-tree portfast     |                                                     |
| --------------------------------------- | --------------------------------------------------- |
| (config)#spanning-tree portfast default | Enable portfast edge as default on all access ports |

### BPDU Guard

if the switch receives a BPDU on a PortFast-enabled port (another unauthorized switch connects to the PortFast-enabled port), it will set the port into errdisable state to prevent loop

| ASW(config-if)# spanning-tree bpduguard enable        |                                                    |
| ----------------------------------------------------- | -------------------------------------------------- |
| ASW(config)# spanning-tree portfast bpduguard default | Enable bpdu guard by default on all portfast ports |

Errdisable is a Cisco mechanism that automatically disables a port when a specific error condition occurs such as Loopback error, EtherChannel misconfig, excessive flapping

Manual recovery - Bouncing the port - with shutdown and no shutdown recovers the port from errdisable state #show errdisable recovery

| errdisable recovery cause all errdisable recovery interval \[30-86400 sec] | setting a timer for how long a port remains in the Errdisable state before it attempts to recover automatically |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |

### UplinkFast

blocking port is put into forwarding immediately after direct link to root bridge fails. Configured for access layer switches

Cross-Stack UplinkFast (CSUF) provides a fast spanning-tree transition across a switch stack, enabled automatically with uplinkfast

| DSW1(config)# spanning-tree uplinkfast |   |
| -------------------------------------- | - |

### BackboneFast

speeds up a STP convergence after indirect link failure e.g connection between other switches

RootLink Queries (RLQ) is used to query switch connected to it's RP to confirm the root bridge, avoiding waiting for the Max Age to expire

SW2 does not receive BPDU's from SW3, since it's Gi0/1 is in blocking state, it starts to declare itself as the root bridge to SW3

| spanning-tree backbonefast | If you use BackboneFast, you must enable it on all switches in the network. BackboneFast is not supported on Token Ring VLANs. This feature is supported for use with third-party switches. |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

![](<../.gitbook/assets/Unknown image (690)>)

1. SW1 G0/0, SW2 G0/0 enter the Disabled state

SW2 doesn't receive BPDU's from SW3, so SW2 doesn't know it can still reach the Root Bridge via GO/I.

2. SW2 declares itself the root bridge, starts sending inferior BPDUs to SW3 via G0/1
3. Upon receiving an inferior BPDU from SW2, SW3 sends a Root Link Query (RLQ) Request out of its Root Port.

The purpose is to check if the switch it thinks is the Root Bridge (SW1) is really the Root Bridge, or if SW2 is correct.

4. SW1 confirms that it is still the Root Bridge by sending an RLQ Response to SW3.
5. Upon receiving SW1's RLQ Response, SW3 makes G0/1 a Designated port in the Listening state, bypassing the Max Age timer.
6. SW3 forwards SW1's BPDUs to SW2

SW2 accepts SW1 as Root and makes G0/I its Root Port.

SW3 GI0/1 proceeds through Learning to reach

30 sec of downtime

### BPDU Filter

BPDUs are sent on all ports by default, even if they are PortFast-enabled. You should always run STP on your network switches to prevent Layer 2 loops. However, there are special cases where you may need to prevent BPDUs from being sent out.

This feature turns off sending BPDUs out of a port and ignores BPDU aswell (similar to passive interface, where we don't want to establish any relationship for the protocol with the neighboring device e.g. connection between customer and service provider)

| Switch(config-if)# spanning-tree bpdufilter enable        |                                                     |
| --------------------------------------------------------- | --------------------------------------------------- |
| Switch(config)# spanning-tree portfast bpdufilter default | Enable bpdu filter by default on all portfast ports |

![](<../.gitbook/assets/Unknown image (691)>)

### Root Guard

For optimal performance, it is preferable to position the root bridge within the distribution or core network layer. However, in standard STP, any bridge possessing the lowest bridge ID becomes the root bridge by default, thereby stripping administrators of the ability to specify its location.

Since there is no default mechanism to enforce a switch to remain a root bridge and whenever a switch with lowest bridge ID is connected to the network, it will assume the root bridge role, which may be undesirable and cause unpredictable network topology, The root guard is a feature that enforces the position of the chosen root bridge

Assures that the interface on which the root guard is enabled is set as the designated port and ignores any received superior BPDUs

Port is placed to root-inconsistent (blocked) state, if superior BPDU is received on that port, preventing traffic to be forwarded across this port

Root guard is best deployed toward ports that connect to switches that should not be the root bridge. Root guard is enabled using the spanning-tree guard root command in interface configuration mode.

### Bridge Assurance

provides an additional layer of verification to ensure that the BPDUs exchanged between switches are valid and that the network topology remains consistent. Is enabled on Designated ports and switches on both ends actively participate in Bridge Assurance. If one of the switches stops receiving BPDUs from its neighbor for a specific duration, it will infer that there may be a problem with the link or the neighbor

If Bridge Assurance detects that BPDUs are no longer being received on a designated port, it places that port into a "Bridge Assurance Inconsistent" state. In this state, the port effectively stops forwarding traffic, when issue is resolved, it transition back to forwarding state. This can happen when there is some intermittent loss in the network and BPDUs are not being delivered between switches

| Switch(config)# spanning-tree bridge assurance |   |
| ---------------------------------------------- | - |

### Loop Guard

by default, interfaces that are currently in "blocking" state can transition into forwarding when they no longer receive BPDUs, since STP functionality depends on continuous exchange of BPDU's.

Loop Guard is used to prevent unidirectional link loops that can occur when one side of a link stops receiving BPDUs while still forwarding traffic

Loop Guard monitors the BPDUs it receives on a designated port. If it stops receiving BPDUs, it places the port in a loop-inconsistent state, effectively blocking traffic on that port until it starts receiving BPDUs again. Loop guard works only on point-to-point links and should be enabled on all nondesignated ports and root ports

Loop guard should be activated on non-designated ports, specifically on root and alternate ports, across all active topologies. Since loop guard does not operate on a per-VLAN basis, the same trunk port may be designated for one VLAN while being non-designated for another.

| Switch(config-if)# spanning-tree guard loop     | configured on ports where the presence of root bridge is undesirable |
| ----------------------------------------------- | -------------------------------------------------------------------- |
| Switch(config)# spanning-tree loopguard default |                                                                      |

### Unidirectional Link Detection (UDLD)

similar to loop guard, monitoring bidirectional communication of fiber-optic or twisted-pair Ethernet cables connections, and disables the connection, if it is detects unidirectional communication. Both switches must support and have UDLD enabled on the respective ports

UDLD learns about other UDLD-capable neighbors by periodically echoing a hello packet sent to well-known MAC 01:00:0C:CC:CC:CC

UDLD packet contains own device/port id and neighbors device port id.

Message Interval: 15 seconds

Timeout Interval: 5 seconds

When the switch receives a hello message, it caches the information until the age time (hold time or time-to-live) expires

If the switch receives a new hello message before an older cache entry ages, the switch replaces the older entry with the new one

Neighboring ports should see their own device/port ID (echo) in the packets received from the other side

Link is considered unidirectional when the port doesn’t see its own device/port ID in the incoming UDLD packets

Normal mode operates passively, and generates only syslog without shutting down the port

Aggressive mode shuts down the port

Only Tx is broken on fiber-optic cable (interface still as up/up) on B and thus C assumes that port on B is in blocking state > So C will after MaxAge time out place port into forwarded (designated) and C is now sending BPDU's out of both connected ports and creating loop (B Rx is still in operation and forwarding it to the root)

errdisable recovery cause udld

The shutdown interface configuration command followed by the no shutdown interface configuration command restarts the disabled port or use #udld reset

![](<../.gitbook/assets/Unknown image (692)>)

| (config)# \[no] udld \[ aggressive \| enable | enabling udld globally                                       |
| -------------------------------------------- | ------------------------------------------------------------ |
| (config-if)# \[no] udld port \[aggressive]   | per port                                                     |
| udld message time                            | ## Modifying the UDLD message interval                       |
| udld reset                                   | ## Resetting all interfaces which have been shutdown by UDLD |
| show udld \[ \| vlan \| neighbors]           |                                                              |

### Storm control

prevents unicast, multicast, broadcast storms by measuring the input rate on a physical interface and allowing or denying the traffic based on defined thresholds

Port can be either temporarily or completely blocked. Configuration is done per interface for each traffic type individually (unicast, multicast, broadcast)

Rising Thresholds and Falling Thresholds are defined. Once the rising threshold is reached, traffic is either temporarily blocked until it reaches the falling threshold again or the port is completely put into err-disabled state and optionally a SNMP trap will be sent

Traffic is blocked for every NEXT one-second (1) interval until the threshold reaches and stays below the falling threshold

Storm Control is typically configured on Access Ports to prevent storms from even reaching the network

| (config-if)# storm-control \<unicast\|multicast\|broadcast\|unknown-unicast] level \<rising-in-%> \<falling-in-%>(config-if)# storm-control action \[trap \| shutdown] | can be specified as packet/bits per second or as rising % |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |

### Comparison of spanning tree variants

| Protocol    | Standard | Resources Needed | Convergence | Number of Trees        |
| ----------- | -------- | ---------------- | ----------- | ---------------------- |
| STP         | 802.1D   | Low              | Slow        | One                    |
| PVST+       | Cisco    | High             | Slow        | One for every VLAN     |
| RSTP        | 802.1w   | Medium           | Fast        | One                    |
| Rapid PVST+ | Cisco    | Very high        | Fast        | One for every VLAN     |
| MSTP        | 802.1s   | Medium or high   | Fast        | One for multiple VLANs |

STP assumes one 802.1D spanning tree instance for the entire bridged network, regardless of the number of VLANs. Because only one instance exists, the CPU and memory requirements for this version are lower than for the other protocols. However, because of only one instance, there is only one root bridge and one tree. Traffic for all VLANs flows over the same path, which can lead to suboptimal traffic flows. Because of the limitations of 802.1D, this version is slow to converge. When a change in the topology occurs at a switch (for example, a link goes down), it generates a Topology Change Notification (TCN) BPDU. It then sends this TCN BPDU to the root bridge. When the root bridge receives this TCN BPDU, it will send an update to all non-root switches informing them of the update. It also informs them to change the aging timer for MAC addresses to 15 seconds.

PVST+ is a Cisco enhancement of STP that provides a separate 802.1D spanning tree instance for each VLAN that is configured in the network. The separate instance supports features like PortFast, UplinkFast, BackboneFast, BPDU guard, BPDU filter, root guard, and loop guard to enhance security. Creating an instance for each VLAN increases the CPU and memory requirements but allows for per-VLAN root bridges. The use of PVST+ gives the administrator the ability to load balance traffic per VLAN. For example, the root bridge for VLAN 10 could be switch A and the root bridge for VLAN 20 could be switch B. Convergence of this version is similar to the convergence of 802.1D, meaning that the topology change mechanism is the same as 802.1D. However, convergence is per-VLAN.

RSTP, or IEEE 802.1w, is an evolution of STP that provides faster STP convergence. This version addresses many convergence issues, but because it still provides a single instance of STP, it does not address the suboptimal traffic flow issues. To support that faster convergence, the CPU usage and memory requirements of this version are slightly higher than the requirements of the original STP but lower than the requirements of PVST+. When a topology change occurs at a switch, it sends a TCN BPDU to its neighboring switches and flushes out the MAC Addresses in its MAC Address table. All switches receiving the TCN BPDU will repeat this until all switches have done so. Because switches do not wait for a BPDU from the root switch before aging out MAC Addresses, this causes the network to converge faster than in classic STP.

Rapid PVST+ is a Cisco enhancement of RSTP that uses PVST+. It provides a separate instance of 802.1w per VLAN. This version addresses both the convergence issues and the suboptimal traffic flow issues. However, this version has the largest CPU and memory requirements. The topology change mechanism in Rapid PVST+ is the same as in RSTP, except it is done per VLAN.

MSTP is an IEEE standard that is inspired by the earlier Cisco proprietary MISTP implementation. To reduce the number of required STP instances, MSTP enables the mapping of multiple VLANs into the same spanning-tree instance with a common root bridge. The Cisco implementation of MSTP provides up to 16 instances of RSTP (802.1w) and combines many VLANs with the same physical and logical topology into a common RSTP instance. Each instance supports PortFast, BPDU guard, BPDU filter, root guard, and loop guard security enhancements. The CPU and memory requirements of this version are lower than the requirements of Rapid PVST+ but are higher than the requirements of RSTP. The topology change mechanism in MSTP is similar to the one in RSTP. However, due to having multiple spanning-tree instances (MSTI), the calculation is done per MSTI.

### Rapid Spanning Tree Protocol (RSTP)

provides rapid convergence by decreasing forward delay time of port state transitions. Switches age out the BPDU information much more quickly

In standard STP, a switch waits 10 hello intervals (20sec), whereas RSTP switch considers a neighbor down if it misses 3 BPDU's (6sec), flushing all MAC's learned on that interface.

UplinkFast and Backbone fast functions are integrated into RSTP itself - doesnt have to be configured explicitly

Standard STP cost used 16-bit cost values, RSTP introduced the long costs - 32bit values, to accomodate links of greater speeds

RSTP is proactive and therefore negates the need for the 802.1D delay timers. RSTP supersedes 802.1D while remaining backward compatible. Much of the 802.1D terminology and most parameters remain unchanged. In addition, RSTP is capable of reverting to 802.1D to interoperate with traditional switches on a per-port basis, and negotiate port states on a peer switch basis, using a proposal and agreement process.

New BPDU flags

**Proposal Flag** The "Proposal" flag in a BPDU indicates that a switch is suggesting itself as the root bridge for a particular VLAN

When a switch with a lower Bridge ID wants to become the root bridge for a VLAN, it sets the Proposal flag in its BPDUs to notify the other switches of its intention.

**Agreement Flag** in a BPDU signifies that a switch has acknowledged and agreed to the Proposal made by another switch. When a switch receives a BPDU with the Proposal flag set and it decides to accept the proposal (i.e., it has a higher Bridge ID), it will set the Agreement flag in its subsequent BPDUs to indicate its agreement with the proposed switch as the root

![](<../.gitbook/assets/Unknown image (693)>)

New Port State

**Discarding** represents all three initial states - Disabled, Blocking and Listening.

The RSTP port states correspond to the three basic operations of a switch port: discarding, learning, and forwarding. There is no listening state as there was with STP. The listening and blocking STP states are replaced with the discarding state.

A port will accept and process BPDU frames in all port states.

The transition to Learning and eventually to Forwarding takes 15seconds combined

If the RSTP sync mechanism fails (if other side run older STP), the port spends 15 seconds in Discarding state and 15 seconds in learning (forward delay \*2) before moving to Forwarding

Renamed Non-Designated Port Roles - in stable discarding state

Root port, Designated port remains same

Alternate port in a discarding state that serves as a backup for the root port and takes over, when root port fails, which is equivalent to uplink fast, so there is no need to configure it

Backup is a port in a discarding state related to the shared segment - the same collision domain (Switch connected to Hub). This port receives BPDU from another interface on the same switch and activates, when primary DP fails

You will not encounter this port role in a modern network

![](<../.gitbook/assets/Unknown image (694)>)

RSTP differentiates three link types

**Edge** equivalent to PortFast - port connected to endpoints, immediately forwarding, unlike in portfast, the edge port transition to normal RSTP state if it receives BPDU.

Can be considered as sub-type of P2P and Shared. If port is full-duplex it will appear as P2P/Edge

**Point-to-point (P2P)** Full-duplex ports; connection between two switches

**Shared** Half-duplex ports; connection to a Hub

### Per-VLAN Spanning Tree Plus (PVST+) (Cisco)

Per-VLAN Spanning Tree Plus (PVST+) by Cisco decouples VLANs and STP instances, allowing one STP instance per VLAN, which facilitates load balancing by utilizing blocked ports as forwarding paths for different VLANs. However, this comes with a scalability trade-off: with 20 VLANs, there are 20 STP instances, each generating 20 BPDUs every 2 seconds on each trunk, significantly increasing bandwidth and CPU usage. Rapid PVST, an enhanced version of PVST+ incorporates principles for faster convergence

{% hint style="info" %}
The BPDUs for a specific VLAN are also tagged by the switch with the VLAN tag.
{% endhint %}

![](<../.gitbook/assets/Unknown image (695)>)

Load Sharing

Assign lower cost values to interfaces that you want selected first and higher cost values that you want selected last.

| Adjusting Port priorities                                     |                                                                                                                                    |
| ------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| SwitchC(config-if)# spanning-tree vlan 1-10 port-priority 16  | Vlan 1-10 will use interface 1 and blocking 2                                                                                      |
| SwitchC(config-if)# spanning-tree vlan 11-20 port-priority 16 | VLan 11-20 will use interface 2 and blocking 1                                                                                     |
| Adjusting Path cost                                           |                                                                                                                                    |
| SwitchC(config-if)# spanning-tree vlan 1-10 cost 30           | If your switch is a member of a switch stack, you must use path cost comand to select an interface to put in the forwarding state. |

### Multiple Spanning Tree Protocol (MSTP) (802.1s)

A single BPDU is generated which contains information for all MST instances (MSTI's)

We can map 10 VLANs to one instance and second 10 VLANs to second instance

In MSTP mode, the switch or switch stack supports up to 65 MST instances. The number of VLANs that can be mapped to a particular MST instance is unlimited.

MST region is grouping of switches with the same configuration and they appear as a single virtual switch to external switches

MSTP establishes and maintains two types of spanning trees:

Internal Spanning Tree (IST) (Instance 0) runs on all switches in an MST region and operates as a shared spanning tree for all VLANs that are not explicitly mapped to specific MSTIs

IST is the only spanning-tree instance that sends and receives BPDUs. All of the other spanning-tree instance information is contained in M-records, which are encapsulated within MSTP BPDUs

Common and Internal Spanning Tree (CIST) is a collection of the ISTs in each MST region, and the common spanning tree (CST) that encompasses the entire switched domain and interconnects the MST regions and single spanning trees

Switches only exchange and compare digest (hash) tables, which represents VLAN-to-instance maps with revision number and name. If digest differs that means the other switch is in different region (MST) and thus this port on local switch is Region Boundary (Boundary port)

![](<../.gitbook/assets/Unknown image (696)>)

In MST, a Main Root Bridge is elected for the entire CST, and a root bridge is elected for each MSTI. Additionally, there is a CIST regional root, commonly referred to as the IST Master, which serves as the root for the IST (Internal Spanning Tree) instance.

MST can interact with PVST+/RSTP environments by acting as a root bridge for all VLANs or ensuring that the PVST+/RSTP environment is the root bridge for all VLANs.

MST cannot be a root bridge for some VLANs and then let the PVST+/RSTP environment be the root bridge for other VLANs.

![](<../.gitbook/assets/Unknown image (697)>)

![](<../.gitbook/assets/Unknown image (698)>)

![](<../.gitbook/assets/Unknown image (699)>)

#### MST configuration

| SW1(config)# spanning-tree mode mst          |   |
| -------------------------------------------- | - |
| SW1(config)# spanning-tree mst root primary  |   |
| SW1(config)# spanning-tree mst configuration |   |
| SW1(config-mst)# name                        |   |
| SW1(config-mst)# revision                    |   |
| SW1(config-mst)# instance vlan               |   |

| SW1(config)# spanning-tree mst priority                    | To manipulate with root bridge priority If using the priority command you must set the ID in multiples of 4096. |
| ---------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| SW1(config)# spanning-tree mst root {primary \| secondary} |                                                                                                                 |
| show spanning-tree mst                                     |                                                                                                                 |
