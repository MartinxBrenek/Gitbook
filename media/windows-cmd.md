# Windows CMD

Quick reference for common CMD and networking commands.

### Directories

| cd            | Change directory.           |
| ------------- | --------------------------- |
| dir           | List files and directories. |
| mkdir         | Create a directory.         |
| rmdir or rd   | Remove a directory.         |
| chdir or cd.. | Move up one directory.      |
| chdir /d path | Change drive and directory. |
| \| findstr    | \| grep alternative         |

### Files and text

copy: Copy files.

move: Move or rename files.

del or erase: Delete files.

ren: Rename files.

### System information

systeminfo: Display system information.

tasklist: List running processes.

taskkill: Terminate processes.

type: Display the contents of a text file.

notepad: Open a text file in Notepad.

find: Search for a text string in files.

more or less: View text file content page by page.

### User and security

net user: Manage user accounts.

gpupdate: Update Group Policy settings.

shutdown: Shutdown or restart the computer.

### Useful Run commands (Win + R)

msinfo32

dxdiag

### Startup folder path

The path to the folder where startup applications are located on Windows is:

{% code title="Startup folder" %}
```
C:\Users\brema3q\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```
{% endcode %}

### Networking

| ping                                                                                   | -t to run continuous ping Note Windows may not respond to the ICMP requests by default - due to its integrated firewall                                                       |
| -------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| pathping                                                                               | Pathping may take some time to complete, as it sends multiple packets to each hop along the path                                                                              |
| nslookup \<ip/hostname>                                                                | prints DNS information                                                                                                                                                        |
| ipconfig /all                                                                          | is used for displaying and managing IP configuration settings on network interfaces. Displays detailed information about all network interfaces.                              |
| ipconfig /renew                                                                        | Renews the DHCP lease for a network interface, requesting a new IP address.                                                                                                   |
| ipconfig /release                                                                      | Releases the DHCP lease for a network interface, relinquishing the current IP address.                                                                                        |
| ipconfig /flushdns                                                                     | Flushes the DNS resolver cache, which can be useful for clearing cached DNS records.                                                                                          |
| Netstat (Network Statistics)                                                           | is a command-line tool in Windows that provides details about active TCP/UDP network connections, listening ports, routing tables, interfaces and statistics of local machine |
| netstat -o                                                                             | To display the PID (Process ID) of each connection                                                                                                                            |
| netstat -e                                                                             | Displays Ethernet statistics. This may be combined with the -s (statistics)                                                                                                   |
| netstat -r                                                                             | To display the routing table                                                                                                                                                  |
| route print \[-4 -6]                                                                   | to view the routing table and interface id that uses given route, you can specify either IPv4 or IPv6 Note Lower metric values indicate more preferred routes                 |
| route add mask <>                                                                      | temporary command that is lost after reboot                                                                                                                                   |
| Netsh (Network Shell)                                                                  | is primarily used for configuring and managing network settings and services on a Windows system. It's a tool for making changes to the network configuration                 |
| netsh interface \[ipv4 \| ipv6]show \[ route \| interface \|subinterface \| neighbors] | to show interfaces, routes or neighbors for IPv4 and IPv6 subinterface shows MTU and in current incoming and outgoing traffic in bytes                                        |
| netsh interface ipv6> set address interface\_name address/prefix                       | to change IP address                                                                                                                                                          |
| netsh interface ipv4 add route 0.0.0.0/0 "Ethernet 2" 10.1.1.2                         | adds static default route to the next hop IP                                                                                                                                  |
| netsh interface \[ipv4 \| ipv6] show subinterfaces "Ethernet" mtu=1450                 | to change MTU                                                                                                                                                                 |
| netsh interface set interface "YOUR\_ADAPTER\_NAME" \[enable\|disable]                 | to enable/disable interface                                                                                                                                                   |

#### Interface index and metrics

The %xx following IPv6 address identifies the interface index that use this address, since the same IPv6 address can be used by multiple interfaces - such as link-local

Interface index is essentially numerical ID assigned to the windows adapter, which can be used instead of the adapter name such as "Ethernet 2" in the netsh show commands

![](<../.gitbook/assets/Unknown image (621)>)

Final metric is a sum of interface metric and metric, translated from router preference (High =16, Medium=256, Low=4096)

![](<../.gitbook/assets/Unknown image (622)>)

![](<../.gitbook/assets/Unknown image (623)>)

#### IP and interface configuration in PowerShell

| Information about network adapter:        | Get-NetAdapter Get-NetAdapterBinding –Name              |
| ----------------------------------------- | ------------------------------------------------------- |
| To activate IPv6 on the network interface | Enable-NetAdapterBinding –Name -ComponentID ms\_tcpip6  |
| To disable IPv6 on a network interface    | Disable-NetAdapterBinding –Name -ComponentID ms\_tcpip6 |

![](<../.gitbook/assets/Unknown image (624)>)

**Adapter IP address information (including configuration)**

Get-NetIPAddress –InterfaceAlias

**Add a new IP address (including configuration)**

New-NetIPAddress –InterfaceAlias -IPAddress -PrefixLength -DefaultGateway

**Remove an IP address (including configuration)**

Remove-NetIPAddress –InterfaceAlias -IPAddress

**IP address modification (including configuration)**

Set-NetIPAddress –InterfaceAlias \<modification, e.g. – IPAddress >

**Advanced network interface features**

Get-NetAdapterAdvancedProperty –InterfaceAlias

**Modify advanced network interface properties**

Set-NetIPAddress –Name \<modification, e.g. – IPAddress >

### macOS

#### CLI commands (similar to GNU/Linux)

ifconfig: Is used for manual configuration of IP parameters on interfaces.

route: Is used for configuring the routing table.

netstat: Shows network status, routing table, and interface statistics.

#### Enable/disable IPv6

networksetup -setv6automatic : Enables IPv6 on a specific interface.

networksetup -setv6off : Disables IPv6 on a specific interface.

![](<../.gitbook/assets/Unknown image (625)>)

### IPv6 on Windows

The preference between IPv4 and IPv6 is typically controlled by the system's Address Selection Policy Table (ASPT), which prioritizes one protocol over the other based on specific configurations. This is generally done by modifying system settings or policies for IPv6 preference.

For example:

In Windows, the preference for IPv6 or IPv4 can be adjusted by the "Prefer IPv4 over IPv6" setting in the network adapter settings (Control Panel -> Network and Sharing Center -> Change adapter settings -> Properties -> IPv6 settings).

| netsh interface ipv6 set prefixpolicy | These configurations control which IPv6 addresses are preferred when multiple IPv6 addresses are available on the system. They allow you to prioritize certain types of IPv6 addresses (e.g., global addresses, link-local addresses, or unique local addresses) over others, |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Scripting

#### Ping a list of IP addresses

To perform a ping operation for a list of IP addresses in Windows Command Line (CLI) and identify which ones do not respond, you can use a combination of batch scripting and the ping command. Here’s a step-by-step guide:

1. Create a List of IP Addresses

First, create a text file that contains the list of IP addresses you want to ping. Each IP address should be on a new line. Let’s name this file ip\_list.txt

2. Write a Batch Script

Create a batch script to read each IP address from the file, ping it, and log the results. You can create a batch file with the following content:

```bat
@echo off

setlocal enabledelayedexpansion

rem Specify the input file and the output file

set "inputFile=ip\_list.txt"

set "outputFile=unresponsive\_ips.txt"

rem Clear the output file if it exists

> "%outputFile%" echo List of unresponsive IPs:

rem Loop through each line in the input file

for /f "tokens=\*" %%i in (%inputFile%) do (

echo Pinging %%i...

rem Perform the ping command and check for a successful response

ping -n 1 %%i | find "Reply from" >nul

if errorlevel 1 (

echo %%i >> "%outputFile%"

echo %%i did not respond.

) else (

echo %%i responded.

)

)

echo Done. Check "%outputFile%" for the list of unresponsive IPs.
```

3. Save and Run the Batch Script

Save the above script as ping\_check.bat.

Place ping\_check.bat in the same directory as ip\_list.txt.

Open Command Prompt and navigate to the directory containing the script.

Run the script by typing ping\_check.bat and pressing Enter.

#### What the script does

The script reads each IP address from ip\_list.txt.

It pings each IP address once.

It checks if the response contains "Reply from", indicating that the ping was successful.

If a ping fails, it logs the IP address to unresponsive\_ips.txt

then run it by executing: C:\Users\mbrenek\Documents\ping\_check.bat

![](<../.gitbook/assets/Unknown image (626)>)

![](<../.gitbook/assets/Unknown image (627)>)

![](<../.gitbook/assets/Unknown image (628)>)

### Simple Service Discovery Protocol (SSDP)

The Simple Service Discovery Protocol (SSDP) is a network protocol based on the Internet protocol suite for advertisement and discovery of network services and presence information. It accomplishes this without assistance of server-based configuration mechanisms, such as Dynamic Host Configuration Protocol (DHCP) or Domain Name System (DNS), and without special static configuration of a network host. SSDP is the basis of the discovery protocol of Universal Plug and Play (UPnP) and is intended for use in residential or small office environments. It was formally described in an IETF Internet Draft by Microsoft and Hewlett-Packard in 1999. Although the IETF proposal has since expired (April, 2000),\[1] SSDP was incorporated into the UPnP protocol stack, and a description of the final implementation is included in UPnP standards documents.\[2]\[3]\[4]

[https://en.wikipedia.org/wiki/Simple\_Service\_Discovery\_Protocol](https://en.wikipedia.org/wiki/Simple_Service_Discovery_Protocol)
