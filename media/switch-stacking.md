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

# Switch Stacking

### Overview

**Switch stacking** combines multiple switches into one logical device.

**Switch stack** is a set of multiple switches interconnected with special cabling to form a single logical unit managed as one device. It simplifies management. It increases capacity (port count) and improves redundancy. If one member fails, the stack can keep operating (assuming endpoints are dual-homed). An Advantage license is required for switch stacking.

**Stack Master** is the switch with the highest priority (configured 1-15; 1 is default). It controls the whole stack. The stack is managed through a single IP address.

All stack members share control and management plane, and they keep synchronized system-level configuration files such as CAM,IP,STP,VLAN and BID of master switch.

The interface-specific configuration of each stack member is associated with the stack member number. All members use the same SDM template as the master

The standby switch takes over, if primary master fails. When a stack member leaves the switch stack, the remaining stack members age out or remove all addresses learned by the former stack member. All stack members must run the same Cisco IOS software image and feature set to ensure compatibility between stack members

stack number/module number/port number

**Multichassis EtherChannel (MEC/MLAG)** allows port-channels to be configured across different switch units within a stack

![](<../.gitbook/assets/Unknown image (528)>)

### Verification and troubleshooting commands

| show module                                                  |                                        |
| ------------------------------------------------------------ | -------------------------------------- |
| show switch \[detail]                                        |                                        |
| show diagnostic events slot <>show diagnostic result slot <> | to perform diagnostic test on a module |

### Stack protocol version and compatibility

Each software image includes a stack protocol version. The stack protocol version has a major version number and a minor version number (for example 1.4, where 1 is the major version number and 4 is the minor version number) that determine the level of compatibility among the stack members.

Switch with the same major version number but with a different minor version number are considered partially compatible. When connected to a switch stack, a partially compatible Switch enters version-mismatch (VM) mode and cannot join the stack as a fully functioning member and generates syslog message of the incompatibility detail

Auto-Copy copies software from stack members to upgrade a switch in VM mode.

Auto-Extract searches for the needed software across the stack if it's not found in the stack member.

| show platform stack-manager all                                                                      | Display all stack information, such as the stack protocol version.                                                                                                                                                                                                                                                                                                                              |
| ---------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| archive copy-sw /destination-system \[member-number] /allow-feature-upgrade /overwrite /force-reload | This command will copy the IOS image from the master switch to incompatible Member of the stack, allow feature upgrades, overwrite the existing image, and force a reload of Member to activate the new image. If you download your image by using the copy tftp: boot loader command instead of the archive download-sw privileged EXEC command, the proper directory structure is not created |
| archive download-sw /rolling-stack-upgrade                                                           | Rolling stack upgrade feature upgrades the members one at a time, to avoid losing network connectivity                                                                                                                                                                                                                                                                                          |
| show switch stack-ports summary                                                                      | display a summary of the stack ports                                                                                                                                                                                                                                                                                                                                                            |

![](<../.gitbook/assets/Unknown image (529)>)

### VSS (Virtual Switching System) (EOL)

support up to 2 physical switches that can establish a single logical network entity, they are connected by ethernet cable

It supports devices that are geographically separated, which ensures redundancy and disaster recovery in the event of a failure at one location

**Virtual Switch Link (VSL)** is the MLAG (EtherChannel) between the two switches. It carries VSS control traffic and data traffic destined for the other switch.

**VSLP (Virtual Switch Link Protocol)** is used to establish and maintain VSL/VSS. It has two component protocols: **LMP** and **RRP**.

**LMP (Link Management Protocol)** performs the following functions:

Verifies link integrity by establishing bidirectional traffic forwarding.

Unidirectional links are rejected.

Exchanges switch IDs.

Before setting up the VSS you should configure one switch's ID as '1' and the other as '2'.

Exchanges other required information.

**RRP (Role Resolution Protocol)** performs the following functions:

Determines if the hardware and software versions of the switches are compatible.

Determines the active switch and standby switch.

Depending on the compatibility checks, the stack will come up in one of two modes:

RPR (Route-Processor Redundancy) mode the standby switch cannot forward traffic, but is available as a backup if the active fails

NSF/SSO mode the standby switch is fully initiliazed and can forward traffic

![](<../.gitbook/assets/Unknown image (530)>)

![](<../.gitbook/assets/Unknown image (531)>)

### StackWise

**StackWise** supports up to 9 separate physical switches to be stacked into one stack "vertically". Switches are connected through StackWise Plus ports with proprietary cabling

When all switches in the stack are powered on, **SDP (Stack Discovery Protocol)** uses broadcast messages (via the stack connections) to discover the stack topology

After all switches in the stack have been discovered, switch numbers are determined. Switches must have a unique number (1 to 8).

After Discovery, an Active switch is elected. Switches that boot up within a 2-minute election window participate

The switch with the highest priority (default 1, highest 15) wins the election and assumes the Master role, if the priority is the same for all the switch with the lowest MAC wins

An election is not triggered when a new switch is added and they assume member role

#### Physical connection

Stacking Modules Required: Cisco 9200/9300 series switches do not come with stacking modules by default. You need to order them separately as a kit, which includes two modules and a cable for stacking.

Installation: Once you receive the stacking kit, remove the two panels at the back of the switch to install the modules. Each switch in the stack requires these modules.

Stacking Process: Connect the stacking cables between the modules of two switches, with the Cisco logo on the cable facing upwards. Stack additional switches by daisy-chaining the cables between them.

Complete the Stack Ring: On the final switch in the stack, connect the last cable from the bottom port of the last switch to the top port of the first switch to complete the stacking ring - as shown in the picutre below, the two switches are connectd with two cables to form a ring

Switch Configuration: Though not covered in the video, the stack allows configuration for setting the stack master and standby, and other management settings.

![](<../.gitbook/assets/Unknown image (532)>)

![](<../.gitbook/assets/Unknown image (533)>)

### StackPower

Cisco technology designed to share power across multiple switches in a stack. It allows connected switches to pool their power supplies, effectively creating a shared power system. If one switch in the stack loses its power supply, the other switches in the stack can compensate by supplying power to it. This enhances power redundancy and efficiency, ensuring that devices continue to operate even if there is a failure in one switch's power supply.

### StackWise Virtual

**StackWise Virtual** is a successor to VSS. It allows up to 8 separate physical switches to form a single virtual switch over Ethernet cables. It can connect switches "horizontally" over longer distances compared to physical stacking cables.

Supported on Catalyst 9400/9500/9600 series switches. They don't have dedicated stackwise modules, thus they are connected via usual ethernet ports.

3 links are needed - two links for etherchannel to create SVL and third is DAD link to prevent split brain issue

The switches in the stack are connected via the **SVL (StackWise Virtual Link)**

Link Management Protocol (LMP) is activated on each link of the StackWise Virtual link as soon as it is brought up online.

Periodic Hellos messages are exchanged to monitor and maintain the link health; integrity ensured by establishing bidirectional traffic forwarding, and rejects any unidirectional links

Control Traffic: Traffic used to establish and maintain the Stackwise Virtual domain.

Data Traffic: when traffic received on one member must be sent out of an interface on the other member

Control plane is active/standby, data plane is active/active

![](<../.gitbook/assets/Unknown image (534)>)

Frames sent across the SVL are encapsulated with a 64-byte StackWise Virtual Header.

![](<../.gitbook/assets/Unknown image (535)>)

### vPC (virtual Port Channel)

technology used for Cisco Nexus switches, but each switch is managed and configured independently (control and management planes are independent) with no preemption

It allows devices to connect to two separate Nexus switches as if they were connecting to a single logical switch, offering redundancy and improved network capacity

#### Switch cluster

is a set of Switch connected through their LAN ports, such as the 10/100/1000 ports.

### Stack membership and provisioning

A new, out-of-the-box Switch (one that has not joined a Switch stack or has not been manually assigned a unique stack member number) ships with a default stack member number of 1.

When it joins a Switch stack, its default stack member number changes to the lowest available member number in the stack.

Make sure that you power off the Switch that you add to or remove from the Switch stack

After adding or removing stack members, make sure that the Switch stack is operating at full bandwidth (64 Gb/s).

Press the Mode button on a stack member until the Stack mode LED is on. The last two right port LEDs (usually 10Ge SFP module ports) on all Switch in the stack should be green, if they are not, the stack is not operating at full bandwidth.

| #switch renumber                     | takes effect after reload and cannot be used when the switch is already provisioned                                                                                                                                                                                                         |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| #reload slot                         |                                                                                                                                                                                                                                                                                             |
| #switch priority                     | The new priority value takes effect immediately but does not affect the current active switchstack master. The new priority value helps determine which stack member is elected as the new active switchstack master when the current active switchstack master or the switch stack resets. |
| reload slot \<stack\_memebr\_number> | to reload specific member                                                                                                                                                                                                                                                                   |

As described in the hardware installation guide, you can use the Switch port LEDs in Stack mode to visually determine the stack member number of each stack member.

In the default mode Stack LED will blink in green color only on the stack master. However, when we scroll the Mode button to Stack option - Stack LED will glow green on all the stack members.

When mode button is scrolled to Stack option, the switch number of each stack member will be displayed as LEDs on the first five ports of that switch. The switch number is displayed in binary format for all stack members. On the switch, the amber LED indicates value 0 and green LED indicates value 1.

Example for switch number 5 (Binary - 00101):

First five LEDs will glow in below color combination on stack member with switch number 5.

Port-1 : Amber

Port-2 : Amber

Port-3 : Green

Port-4 : Amber

Port-5 : Green

Similarly first five LEDs will glow in amber or green, depending on the switch number on all stack members.

New switch member can be configured with offline configuration feature to be provisioned by the stack

You must change the stack-member-number on the provisioned switch before you add it to the stack, and it must match the stack member number that you created for the new Switch on the switch stack. The Switch type in the provisioned configuration must match the Switch type of the newly added Switch

| switch stack-member-number provision type                             |                                                                                               |
| --------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| session stack-member-number                                           | access specific members. The stack member number is appended to the system prompt (Switch-2#) |
| switch stack-member-number stack port port-number {enable \| disable} | Manually Enabling/Disabling a Stack Port                                                      |
| #pnpa service reset                                                   | removes the configuration files as well as the stack-related information                      |

### Potential issues

Adding powered-on Switch (merging) causes the stack masters of the merging Switch stacks to elect a stack master from among themselves. The re-elected stack master retains its role and configuration and so do its stack members. All remaining Switch, including the former stack masters, reload and join the Switch stack as stack members. They change their stack member numbers to the lowest available numbers and use the stack configuration of the re-elected stack master. The new stack master becomes available after a few seconds. In the meantime, the Switch stack uses the forwarding tables in memory to minimize network disruption. The physical interfaces on the other available stack members are not affected during a new stack master election and reset

Removing powered-on stack members causes the Switch stack to divide (partition) into two or more Switch stacks, each with the same configuration. This can cause an IP address

configuration conflict in your network. If you want the Switch stacks to remain separate, change the IP address or addresses of the newly created Switch stacks.

#### Split brain and dual-active detection (DAD)

If the VSL between two switches fails, one switch does not know the status of the other. Both switches could change to the active mode, causing a dual-active situation in the network with duplicate configurations (including duplicate IP addresses and bridge identifiers). The network might go down.

#### vPC split-brain scenario

Suppose you have two Cisco Nexus switches, switch A and switch B, configured with a vPC. The vPC is configured with two links, one from each switch, that are bundled together into a single logical port channel. The vPC is used to connect servers or other network devices to the data center network.

If there is a failure in the network infrastructure that connects the two switches, such as a cable or a switch failure, the two switches can become isolated from each other.

In this scenario, the vPC can become split-brain, where each switch believes it is the primary switch and continues to forward traffic to its connected devices.

If a server is connected to the vPC and is configured to use both links in the vPC, it may receive duplicate packets or may not receive packets at all, depending on which switch it is communicating with.

#### EtherChannel fix (dual-active prevention)

To prevent a dual-active situation, the core switches send PAgP protocol data units (PDUs) through the remote satellite links (RSLs) to the remote switches. The PAgP PDUs identify the active switch, and the remote switches forward the PDUs to core switches so that the core switches are in sync. If the active switch fails or resets, the standby switch takes over as the active switch. If the VSL goes down, one core switch knows the status of the other and does not change its state.

### StackWise Virtual staging

#### 48-port variant

**1.Přečíslování druhého switche na 2**

switch renumber 2

**2.Nastavení priority switche 1**

switch priority 15

**3.Nastavení domain – je třeba použít ID dle excelu per lokalita**

device(config)#stackwise-virtual

domain 15

Uložit a rebootovat oba switche.

**4.Nastavení StackWise Virtual Link na všech propojích a na obou switchích**

switch 1:

int twe1/0/47

stackwise-virtual link 1

des SWV SVL 1

int twe1/0/48

stackwise-virtual link 1

des SWV SVL 2

switch 2:

int twe2/0/47

stackwise-virtual link 1

des SWV SVL 1

int twe2/0/48

stackwise-virtual link 1

des SWV SVL 2

Uložit config a restartovat oba switche.

**5.Nastavení Dual-Active-Detection Link**

int twe 1/0/46

des SWV DAD

stackwise-virtual dual-active-detection

int twe 2/0/46

des SWV DAD

stackwise-virtual dual-active-detection

**6. Nastavení hostname -> dle tabulky -> hostname u C9500 je vždy s 01 nakonci. 02 slouží jen jako label na zařízení.**

hostname U-CV-RIEG-DSW01

#### 24-port variant

**1.Přečíslování druhého switche na 2**

switch renumber 2

**2.Nastavení priority switche 1**

switch priority 15

**3.Nastavení domain – je třeba použít ID dle excelu per lokalita**

device(config)#stackwise-virtual

domain 15

Uložit a rebootovat oba switche.

**4.Nastavení StackWise Virtual Link na všech propojích a na obou switchích**

switch 1:

int twe1/0/23

stackwise-virtual link 1

des SWV SVL 1

int twe1/0/24

stackwise-virtual link 1

des SWV SVL 2

switch 2:

int twe2/0/23

stackwise-virtual link 1

des SWV SVL 1

int twe2/0/24

stackwise-virtual link 1

des SWV SVL 2

Uložit config a restartovat oba switche.

**5.Nastavení Dual-Active-Detection Link**

int twe 1/0/22

des SWV DAD

stackwise-virtual dual-active-detection

int twe 2/0/22

des SWV DAD

stackwise-virtual dual-active-detection

**6. Nastavení hostname -> dle tabulky -> hostname u C9500 je vždy s 01 nakonci. 02 slouží jen jako label na zařízení.**

hostname U-CV-RIEG-DSW01

### StackWise RMA

1. Switch must be connected first outside of the stack, IOS needs to be updated to the same version as other switches (bundle mode, no install)
2. Renumber Switch to 1 if not done already.
3. Assign lesser priority to the new one and then turn it off, connect it to the rack, turn it on
4. New Switch should now sync the config from the stack master, you can change priority back to original state to make 1 master again.

Step 1: Power up the switch and wait for it to boot.

* DO NOT attach any stacking cable.

{% hint style="info" %}
Attaching a stack cable to a powered switch will cause the entire stack to reboot.
{% endhint %}

Step 2: Renumber the new switch accordingly with the old (faulty) member switch

Set the stack member number to the original stack numbering scheme (in configuration mode):

switch current-stack-member-number renumber new-stack-member-number

Verify the priority issuing the 'show switch detail' command.

If the priority is higher than one, change it to one:

switch stack-member-number priority priority-number

Step 3: Ensure that "software auto-upgrade enable" has been configured on the main stack so that if the new switch doesn't have the same IOS firmware as the master, it will be up/downgraded.

Step 4: Unplug the new (replacement) switch from power; make sure it's dead.

Step 5: Plug the stack wise cable in.

Step 6: The new (replacement) switch will boot and downloads its new firmware from the master if required and join the stack.

Step 7: The new (replacement) switch will upload the existing configuration from the master switch

Step 8: Check the new stack: #show switch #show interfaces status

### IOS mode and licensing alignment

When used in conjuction with StackWise:

all switches in the stack should be configured with the same mode of ios = install or bundle

all switches in the stack should have the same license as the active switch

when upgrading the IOS in install mode, simply upgrade the active switch, and all switches will be upgraded

when upgrading the IOS in bundle mode, the IOS file must be copied to each switch in the stack
