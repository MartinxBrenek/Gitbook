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

#### Action examples

![](<../.gitbook/assets/Unknown image (960)>)

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
