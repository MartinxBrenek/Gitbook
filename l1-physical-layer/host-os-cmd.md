# Host OS CMD

## Windows

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

## Linux

### Directories

| cd             | Change directory.                                                                                                                                                                                                                                                                                                            |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ls             | List files and directories.                                                                                                                                                                                                                                                                                                  |
| mkdir:         | Create a directory.                                                                                                                                                                                                                                                                                                          |
| rmdir or rm -r | Remove a directory.                                                                                                                                                                                                                                                                                                          |
| pwd            | Print working directory.                                                                                                                                                                                                                                                                                                     |
| cd \~          | Navigate to the home directory.                                                                                                                                                                                                                                                                                              |
| tar -xzf       | used to archive and extract files **-x** → eXtract Tells tar to extract files from an archive. **-z** → gzip Filters the archive through gzip. Use this when the archive is compressed with .gz (i.e. a .tar.gz or .tgz file). **-f** → file Tells tar that the next argument is the name of the archive file to operate on. |

{% hint style="info" %}
If you run a binary from the current folder, prefix it with `./`. Example: `./iperf3`.
{% endhint %}

### File operations

| cp           | Copy files and directories.                                                                           |
| ------------ | ----------------------------------------------------------------------------------------------------- |
| mv           | Move or rename files and directories.                                                                 |
| rm           | Remove files and directories. To forcefully remove a non-empty directory, use: sudo rm -rf /etc/tibit |
| touch        | Create an empty file.                                                                                 |
| cat          | displays file content                                                                                 |
| more or less | View text file content page by page.                                                                  |
| nano vi      | Text editors for file editing.                                                                        |

### System information

| uname -a                                                                                                                                                                                                                                                                                                                                              | Display system information.                                                   |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| df                                                                                                                                                                                                                                                                                                                                                    | Display disk space usage.                                                     |
| top or htop                                                                                                                                                                                                                                                                                                                                           | Display system processes.                                                     |
| free                                                                                                                                                                                                                                                                                                                                                  | display memory usage.                                                         |
| whoami                                                                                                                                                                                                                                                                                                                                                | Display the current user.                                                     |
| useradd and userdel                                                                                                                                                                                                                                                                                                                                   | Add and delete users.                                                         |
| passwd                                                                                                                                                                                                                                                                                                                                                | Change user password.                                                         |
| chmod (Change Mode): Modifies the permissions (read, write, execute) of a file or directory for the owner, group, and others. It controls what actions can be performed on the file/directory. chown (Change Owner): Changes the ownership of a file or directory, i.e., who owns it (user and group). It controls who has access based on ownership. | <p>Change file permissions.<br><br>example: sudo chmod -R u+rw /etc/cisco</p> |

### Package management (common distros)

* `apt` (Debian/Ubuntu) — Package manager for installing software
* `yum` (Red Hat/CentOS) — Package manager for installing software

### Common shell operators

* `>`: Redirect stdout to a file (overwrite). Example: `command > file.txt`
* `>>`: Redirect stdout to a file (append). Example: `command >> file.txt`
* `2>`: Redirect stderr to a file. Example: `command 2> error.txt`
* `&>` or `2>&1`: Redirect stdout + stderr to a file. Example: `command &> output.txt`
* `|`: Pipe stdout of one command to another. Example: `command1 | command2`
* `<`: Use a file as stdin. Example: `command < input.txt`
* `&&`: Run next command only if previous succeeds. Example: `command1 && command2`
* `||`: Run next command only if previous fails. Example: `command1 || command2`

### Change system MTU

#### Temporary change

`sudo ifconfig [interface] mtu xxxx`

Replace \[interface] with the name of the network interface and xxxx with the desired MTU size.

#### Make it persistent

To make the change permanent, edit:

`sudo nano /etc/network/interfaces`

Find the line for the desired interface and add the following line: “mtu xxxx”

Replace xxxx with the desired MTU size.

Save the changes and exit the file.

Restart the network service by typing the following command: “sudo service network-manager restart”

### Router advertisements (IPv6) on Ubuntu

Use this to view the current setting:

`cat /proc/sys/net/ipv6/conf/all/accept_ra`

Output `"2"` indicates acceptance of RAs even when forwarding is enabled.

### Networking commands

| ip address show                                      | Show IP addresses and configuration for all interfaces.                                           |
| ---------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| sudo ip addr add 192.168.1.200/24 dev eth0           | Add the new IP address or use sudo ifconfig eth0 192.168.1.200 netmask 255.255.255.0              |
| sudo ip link set dev eth0 up                         | Bring the interface up (if it's down) or use sudo ifconfig eth0 up                                |
| sudo systemctl restart network OR sudo netplan apply | Restart the networking service:                                                                   |
| ip route show                                        | ip -6 route show                                                                                  |
| route -n                                             | Display the routing table without resolving hostnames.                                            |
| ping                                                 | Send ICMP echo requests to test network connectivity.                                             |
| traceroute                                           | Trace the route packets take to a network host.                                                   |
| netstat -an                                          | Display all network connections, routing tables, and interface statistics.                        |
| ifconfig                                             | Display network interface information.                                                            |
| ss                                                   | Display socket statistics, similar to netstat but more flexible.                                  |
| hostname                                             | Show or set the system's hostname.                                                                |
| dig                                                  | Perform DNS lookups for IP addresses or hostnames.                                                |
| nslookup                                             | Interactively query Internet name servers.                                                        |
| route add default gw                                 | Add a default gateway to the routing table.                                                       |
| iptables -L                                          | List firewall rules (IPv4).                                                                       |
| tcpdump                                              | Capture and display network traffic on a specified interface.                                     |
| sshd restart                                         | Restart the OpenSSH daemon (secure shell server).                                                 |
| telnet                                               | User interface for the TELNET protocol (not secure, for demonstration only).                      |
| scp                                                  | Securely copy files between remote hosts.                                                         |
| wget                                                 | Download files from web servers.                                                                  |
| curl                                                 | Transfer data with URL syntax, often used for APIs.                                               |
| iptraf                                               | Interactive color monitor for IP LAN traffic.                                                     |
| iftop                                                | Monitor bandwidth usage on a network interface in real-time.                                      |
| nmap                                                 | Network scanner to identify devices and services on a network.                                    |
| lsof -i :80                                          | List open files, including network connections (listening on port 80).                            |
| ethtool                                              | Display or change Ethernet card settings.                                                         |
| arp -a                                               | Display the Address Resolution Protocol (ARP) cache.                                              |
| arp-scan -l                                          | scans arp                                                                                         |
| ss -s                                                | Summarize socket statistics by protocol.                                                          |
| hostnamectl status                                   | Get the system hostname and related settings.                                                     |
| resolvconf -u                                        | Update DNS resolver configuration files.                                                          |
| mtr                                                  | Network diagnostic tool that combines traceroute and ping.                                        |
| iwconfig                                             | Configure wireless network interfaces (deprecated, use ip link for newer systems).                |
| nc -l 8080                                           | Listen for incoming connections on TCP port 8080 (netcat, for educational purposes only).         |
| scp                                                  | Securely copy files between remote hosts (same as No. 16).                                        |
| ssh-keygen                                           | Generate, manage, and convert SSH authentication keys                                             |
| ss -t -a                                             | Show detailed information on all sockets, including TCP connections.                              |
| tcpdump -i eth0 tcp port 80                          | Capture and display packets on a specific interface (eth0) for TCP traffic on port 80.            |
| route add -net                                       | Add a new route to the routing table, specifying network address, subnet mask, and gateway.       |
| nmcli connection show                                | Show information about available network connections managed by NetworkManager.                   |
| dig +short A google.com                              | Perform a DNS lookup and display only the IP address (A record) for google.com.                   |
| nload                                                | Visually represent incoming and outgoing network traffic.                                         |
| iperf -c server\_ip                                  | Measure TCP and UDP bandwidth performance between your machine and a server.                      |
| fping -a -g                                          | Quickly ping multiple hosts specified by a range and gateway.                                     |
| iftop -n                                             | Display network bandwidth usage on an interface without hostname resolution.                      |
| route del -net                                       | Delete a route from the routing table by specifying network address and subnet mask.              |
| tcpdump -A -i eth0                                   | Capture and display packets in ASCII format on a specific interface (eth0).                       |
| netcat -zv                                           | Test network connectivity by opening a connection and verifying remote host response.             |
| nmtui                                                | Text-based user interface for managing NetworkManager connections.                                |
| ethtool -s eth0 speed 100 duplex full                | Change the speed and duplex settings of an Ethernet interface (eth0) to 100 Mbps and full duplex. |
| ss -l                                                | Show only listening sockets (those waiting for incoming connections).                             |
| host                                                 | Perform a DNS lookup for an IP address or hostname.                                               |
| nmcli device wifi list                               | List available Wi-Fi networks detected by the system.                                             |
| ipcalc                                               | subnetting calculator                                                                             |
| nmcli device wifi list                               | List available Wi-Fi networks detected by the system.                                             |

### IP and interface configuration in Linux

#### Add an address

```
ifconfig inet6 add /

or

sudo ip addr add \<IP\_ADDRESS>/ dev
```

#### Delete an address

```
sudo ip addr del \<IP\_ADDRESS>/ dev
```

#### Add a route

```
route –A inet6 add gw
```

DNS servers are added to the /etc/resolv.conf file

```
nameserver 2001:db8:100::53
```

#### Basic configuration (ip tool)

new tool ip:

```
ip -6 addr add / dev
```

#### Delete an address (ip tool)

```
ip -6 addr del / dev
```

#### Add a route (ip tool)

```
ip –6 route add via
```

#### Show routes

```
ip -6 route show
```

#### Neighbor discovery

**Show neighbor cache**

```
ip -6 neigh show \[dev ]
```

**Add static neighbor entry**

```
ip -6 neigh add lladdr dev
```

**Delete static neighbor entry**

```
ip -6 neigh del lladdr dev
```

#### Privacy extensions

sysctl.conf

```
net.ipv6.conf.all.use\_tempaddr = 2

net.ipv6.conf.default.use\_tempaddr = 2
```

or for specific interface

```
net.ipv6.conf.wlan0.use\_tempaddr = 2

net.ipv6.conf.eth0.use\_tempaddr = 2
```

### Edit IP address in Ubuntu (netplan)

Edit the netplan config:

`sudo nano /etc/netplan/01-netcfg.yaml`

```yaml
network:

version: 2

ethernets:

ens160: # Replace with your interface name

dhcp4: no

addresses:

* 10.19.55.22/24 # Static IP/subnet

gateway4: 10.19.55.1 # Gateway

nameservers:

addresses: \[8.8.8.8, 8.8.4.4] # DNS servers (optional)
```

then restart the network service

`sudo systemctl restart network`

OR

`sudo netplan apply`

### IPv6 on Linux

On Linux, the system can be configured to prefer IPv6 over IPv4 or the other way around by adjusting the settings in the sysctl.conf file, or more recently, using the net.ipv6.conf.all.prefer\_ipv6 sysctl parameter.

| Linux: /etc/gai.conf | Adjusting preference of an specific IPv6 address |
| -------------------- | ------------------------------------------------ |

You can list and manage tables using commands like nft list tables to view existing tables or nft add table to create new ones. Tables can be configured for different types of network traffic like IPv4 (ip), IPv6 (ip6), or both (inet).

### nftables

![](<../.gitbook/assets/Unknown image (629)>)

#### Chains

Chains organize rules in a table. They define when packets are processed (incoming, outgoing, routed).

**Chain types**

* `filter`: Default. Checks packets for allow/block.
* `nat`: Network Address Translation
* `route`: Routing decisions

**Hooks**

Hooks define when a chain should process packets:

* `prerouting`: Before routing
* `input`: Packets destined to the system
* `forward`: Packets being routed through the system
* `output`: Packets originating from the system
* `postrouting`: After routing

![](<../.gitbook/assets/Unknown image (630)>)

#### Rules

Rules define actions for packets based on match conditions. You can add, modify, or delete rules to control traffic.

**Rule action**

The action is what happens when a rule matches a packet:

* `accept`: Allow the packet
* `drop`: Block the packet
* `queue`: Send the packet to user space
* `continue`: Continue checking other rules
* `return`: Exit the chain
* `jump`: Jump to another chain
* `goto`: Go to another chain, skipping remaining rules

![](<../.gitbook/assets/Unknown image (631)>)

#### IPv6 firewall configuration with nftables

nftables is a modern framework in Linux for handling network packet filtering and firewall configuration, essentially unifies the older iptables, ip6tables, arptables, and ebtables frameworks.

**Tables**

* Table view: `nft list tables` or `nft list table <family> <table>`
* Modify tables: `nft (add | delete | flush) table <family> <table>`

The table type determines which network protocols the table will work with, and the available types are: ip (default), arp, ip6, inet (hybrid for IPv4 and IPv6 traffic)

![](<../.gitbook/assets/Unknown image (632)>)

**Chains**

* Chain operations: `nft (delete | list | flush) chain <family> <table> <chain>`
* Chain creation: `nft (add | create) chain <family> <table> <chain>`

\[ { type hook \[device ] priority ; \[policy ;] } ]

Type can be filter, route, nat. IPv6 supports all types.

Hook indicates the phase in which the packet to be processed is: prerouting, input, forward, output, postrouting.

Priority indicate the priority according to which the lists are processed

Policy can be either accept or drop.

![](<../.gitbook/assets/Unknown image (633)>)

**Rules**

* Rule creation: `nft add rule <family> <table> <chain> ...`
* Insert a rule: `nft insert rule <family> <table> <chain> ...`

\[position ]

* Modify a rule: `nft replace rule <family> <table> <chain> <handle> ...`

\[handle ]

* Delete a rule: `nft delete rule <family> <table> <chain> <handle>`

\[handle ]

Match is a condition of the given packet processing rule

Statement is the action that a rule performs when it is triggered: accept, drop, queue, continue, return, jump , goto

![](<../.gitbook/assets/Unknown image (634)>)
