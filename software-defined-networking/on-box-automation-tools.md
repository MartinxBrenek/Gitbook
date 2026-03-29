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

# On box Automation tools

### Embedded Event Manager (EEM)

**Embedded Event Manager (EEM)** IOS built-in tool that allows to use scripting and build software applets that can automate many tasks

Scripts can automatically execute, based on the event happening on a managed network device

EEM Applets are composed of multiple building blocks // if-then statement logic

#### Components

**EEM server:** Consists of event detectors/publishers and event subscribers. Triggers a subscriber when a detector/publisher sends a notification about an interesting event (defined by the event subscribers)

**Event detectors/publishers:** Monitoring the device for events happening (defined by the event subscribers) and notify the EEM server in case of an interesting event.

**Event subscribers (Applet):** Scripts that get triggered through the EEM server by an event that happened at one of the event detectors/publishers.

**Policy Director:** The policy director is responsible for coordinating and managing the applets. It ensures that the right applet is triggered when a registered event occurs.

#### Possible detectors/publishers

**Interface:** Allows interface parameters to be monitored (eg. threshold violation).

**Routing:** Allows routing events to be monitored (eg. routes)

**SNMP:** Allows MIB objects to be monitored.

**Syslog:** Allows syslogs to be monitored for a specific pattern/string.

**Timer:** Allows actions to be executed based on a specific time (eg. cron job).

**Track:** Allows tracking objects to be monitored (eg. up/down state).

![](<../.gitbook/assets/Unknown image (959)>)

#### Event example

| event manager applet Loopback0 event syslog pattern "Interface Loopback0.\* down" period 1 action 1.0 cli command "enable" action 2.0 cli command "config terminal" action 3.0 cli command "interface loopback0" action 4.0 cli command "shutdown" action 5.0 cli command "no shutdown" action 5.5 cli command "show interface loopback0" action 6.0 syslog msg "I've fallen, and I can't get up!" action 7.0 mail server 10.0.0.25 to neteng@yourcompany.com from no-reply@yourcompany.com subject "Loopback0 Issues!" body "The Loopback0 interface was bounced. Please monitor accordingly. "$\_cli\_result" | // this will include all issues commands in debug // #debug event manager all // if AAA command authorization is being used, it is important to include the event manager session cli username username // |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### Action examples

| event manager environment filename Router.cfg event manager environment tftpserver tftp://10.1.200.29/ event manager applet BACKUP-CONFIG event cli pattern "write mem.\*" sync yes action 1.0 cli command "enable" action 2.0 cli command "configure terminal" action 3.0 cli command "file prompt quiet" action 4.0 cli command "end" action 5.0 cli command "copy start $tftpserver$filename" action 6.0 cli command "configure terminal" action 7.0 cli command "no file prompt quiet" action 8.0 syslog priority informational msg "Configuration File Changed! TFTP backup successful." Manual Execution of EEM Applet (that is stored in flash) event manager applet Ping event none action 1.0 cli command "enable" action 1.1 cli command "tclsh flash:/ping.tcl" Router# event manager run Ping Router# more flash:ping.tcl foreach address { 192.168.0.2 192.168.0.3 192.168.0.4 192.168.0.5 192.168.0.6 } { ping $address} | disables the IOS confirmation mechanism that asks to confirm a user’s actions no automatic event that the applet is monitoring to view content of script |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |

![](<../.gitbook/assets/Unknown image (960)>)

#### SNMP OID sampling (event example)

| event snmp oid <> get-type {exact \| next} entry-op operator entry-value <> \[exit-comb {or \| and}] \[exit-op operator] \[exit-val exit-value] \[exit-time exit-time-value] poll-interval <> | + oid: Specifies the SNMP object identifier (object ID) + get-type: Specifies the type of SNMP get operation to be applied to the object ID specified by the oid-value argument. + next – Retrieves the object ID that is the alphanumeric successor to the object ID specified by the oid-value argument. + entry-op: Compares the contents of the current object ID with the entry value using the specified operator. If there is a match, an event is triggered and event monitoring is disabled until the exit criteria are met. + entry-val: Specifies the value with which the contents of the current object ID are compared to decide if an SNMP event should be raised. + exit-op: Compares the contents of the current object ID with the exit value using the specified operator. If there is a match, an event is triggered and event monitoring is reenabled. + poll-interval: Specifies the time interval between consecutive polls (in seconds) |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### More examples

| track 10 interface Fo0/2/4 line-protocol ! event manager applet shut\_wan event track 10 state down action 1.0 cli command "enable" action 1.1 cli command "conf t" action 1.2 cli command "interface TenGigabitEthernet0/1/0" action 1.3 cli command "shut" action 1.4 syslog msg "WAN TenGigabitEthernet0/1/0 interface byl vypnut kvuli nedostupnosti LAN" action 1.5 cli command "end" event manager applet unshut\_wan event track 10 state up action 1.0 cli command "enable" action 1.1 cli command "conf t" action 1.2 cli command "interface TenGigabitEthernet0/1/0" action 1.3 cli command "no shut" action 1.4 syslog msg "WAN TenGigabitEthernet0/1/0 interface byl zapnut - LAN je opet dostupna" action 1.5 cli command "end" |   |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

| event manager applet LARGECONFIG event cli pattern "show running-config" sync yes action 1.0 puts "Warning! This device has a VERY LARGE configuration and may take some time to process" action 1.1 puts nonewline "Do you wish to continue \[Y/N]" action 1.2 gets response action 1.3 string toupper "$response" action 1.4 string match "$\_string\_result" "Y" action 2.0 if $\_string\_result eq 1 action 2.1 cli command "enable” action 2.2 cli command "show running-config" action 2.3 puts $\_cli\_result action 2.4 cli command "exit" action 2.9 end | When you use the sync yes option in the event cli command, the EEM applet runs before the CLI command is executed. The EEM applet should set the \_exit\_status variable to indicate whether the CLI command should be executed (\_exit\_status set to one) or not (\_exit\_status set to zero). With the sync no option, the EEM applet is executed in background in parallel with the CLI command. |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Construct a script that changes the routing from gateway 1 to gateway 2 from 11:00 p.m. to 12:00 a.m. (2300 to 2400) only, daily.

![](<../.gitbook/assets/Unknown image (961)>)

10\*\*\* means 1 minute after 0 hour regardless of what day or month it is

abcde

a=minute (0-59)

b=hour (0-23)

c=day of month (1 - 31)

d=month (1 - 12) January is 1

e=day of week (0 - 6) Sunday is 0

### Command Scheduler (KRON)

**Command Scheduler (KRON)** allows customers to schedule fully-qualified EXEC mode CLI commands to run once, at specified intervals, at specified calendar dates and times, or upon system startup

Command Scheduler has two basic processes. A policy list is configured containing lines of fully-qualified EXEC CLI commands to be run at the same time or same interval. One or more policy lists are then scheduled to run after a specified interval of time, at a specified calendar date and time, or upon system startup. Each scheduled occurrence can be set to run either once only or on a recurring basis.

#### Configuration guide

[https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/ntw-servs/b-network-services/m\_cns-cmd-sched.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/ntw-servs/b-network-services/m_cns-cmd-sched.html)

#### Example

| kron occurrence OCCU\_BCKP at 22:00 Sun recurring policy-list BACKUP\_CONF ! kron policy-list BACKUP\_CONF cli write memory |   |
| --------------------------------------------------------------------------------------------------------------------------- | - |
