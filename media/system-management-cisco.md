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

# System Management  - Cisco

#### Internetworking Operating System (IOS)

**Internetworking Operating System (IOS)** is a proprietary operating system that runs on Cisco network devices

Like many common end devices (such as laptops, servers, and mobile phones), network intermediary devices need an operating system to function. An operating system is the software that manages hardware resources, for example, memory allocation and input/output functions, and manages interaction among different hardware components. Many Cisco devices run Cisco IOS Software. The operating system software includes basic networking functions, but it may also include advanced features, such as management and security services, quality of service (QoS), or call processing features. Examples of devices that use Cisco IOS Software are routers, LAN switches, small wireless access points (APs), and so on. The main function of Cisco IOS Software is to provide network features and functions.

Networking devices run particular versions of the Cisco IOS Software. The IOS version depends on the type of device being used and the required features. While all devices come with a default IOS and feature set, it is possible to upgrade the IOS version and feature set to obtain additional capabilities.

The portion of the operating system that interfaces with applications and the user is a program known as a shell. Unlike common end devices, Cisco network devices do not have a keyboard, monitor, or mouse device to allow direct user interaction. However, users can interact with the shell using their own computer and accessing a CLI or a GUI. The figure illustrates the examples of CLI-based and GUI-based access to a shell.

When using a CLI, the user interacts directly with the system in a text-based environment by entering commands on the keyboard at a command prompt. The system executes the command, often providing textual output. The CLI requires very little overhead to operate. But the user must know the underlying structure that controls the system.

GUIs may not always be able to provide all features that are available in the CLI. Some tasks will require you to use the CLI because they are not supported in the GUI.

Cisco network devices support the option of remote management access via CLI or GUI. This allows the user to interact with devices even when not directly connected to the device. There are many different options for remote management, depending on device type and function.

Most devices offer a dedicated Management Interface which is used to connect the device to the management subnet. This is commonly used in small and medium-sized networks, where you can manage devices in standalone fashion with a user interacting directly with the device. In large networks, you can use a centralized management solution to simplify operations. Instead of a user interacting with devices directly, they interact with a network controller, such as Cisco Catalyst Center or Cisco Wireless LAN Controller. The controller then communicates information to network devices.

Each routing protocol or networking feature is implemented as a program or process running within the operating system. Configuring a specific protocol or feature activates its corresponding code and algorithm, allowing the router to perform tasks like exchanging routing information, calculating paths, and updating the routing table

Any device running some kind of OS (mostly linux) can be configured to be an router with it's own features - Cisco IOS is however cisco private OS designed specifically for switching/routing

Unix is a family of operating systems with different versions and variations, while Linux is a specific operating system kernel that is commonly used as the core component in many Unix-like operating systems known as Linux distributions

#### 32-bit/64-bit Operating System (OS)

Refers to how much bits can an operating system process at a time. With a 64-bit operating system, the system can address a much larger amount of memory (RAM) compared to a 32-bit system. While 32-bit systems are limited to addressing up to 4 gigabytes (GB) of RAM, 64-bit systems can theoretically address up to 16 exabytes (EB) of RAM

By upgrading your device to the latest available/recommended version, the highest system stability and system error correction can be achieved.

Cisco recommends minimizing the number of software releases that are deployed in any network environment and establishing a software strategy that indicates which releases and images will be used by different devices that are deployed throughout the environment

To maximize operational efficiency, it is ideal to use the same software release on devices that have similar hardware and feature deployments

#### System and hardware show commands

| **show processes cpu \[history \| sorted] \[platform]**                                            | to verify cpu utilization                                                                                                 |
| -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **show process memory \[platform]** **show memory \[platform]** **show memory statistics history** | To check memory utilization platform keyword shows the entire Kernel statistics, while without nly the IOS CPU statistics |
| **show crash**                                                                                     |                                                                                                                           |
| **show inventory**                                                                                 | to check what hardware is part of the device                                                                              |
| **show environment**                                                                               |                                                                                                                           |
| **show facility-alarm status**                                                                     | displays all alarms incling those that are indicated by LED light                                                         |
| **show platform \[resources \| hardware \| diag \| software \| qfp]**                              |                                                                                                                           |
| **show platform software status control-processor**                                                |                                                                                                                           |
| **show event manager history events**                                                              |                                                                                                                           |

### Command-line interface (CLI)

**Command-line interface (CLI)** is a clear-text based user interface (UI) used to interact with network or computer devices

Network device CLI can be access via console (line con) port or if the device is already configured and connected to the network, we can access it's virtual management interface (line vty) remotely through Telnet or SSH

Cisco IOS Software is designed as a modal operating system. The term modal describes a system that has various modes of operation. Each mode has its own set of commands and command history and is intended for usage for a specific group of tasks. The CLI uses a hierarchical structure for the modes. This hierarchy starts with the least specific command mode or higher-level mode and proceeds with more specific or lower-level command modes. A more specific command mode can be entered from the less specific mode, which precedes it in the hierarchy.

To enter commands into the CLI, type in or copy and paste the entries within one of the several console command modes. Each command mode is indicated with a distinctive visual prompt. The term prompt is used because the system is prompting you to make an entry. Pressing enter instructs the device to parse and execute the command.

Each command mode has a name and a distinctive visual prompt by which it can be recognized. By default, every prompt begins with the device name. Following the device name, the remainder of the prompt uses special characters and words to indicate the mode. As you use commands and change the operation mode, the prompt changes to reflect the current context. To enter a command, you can either type them in or copy and paste the entries. Once you are done, press Enter and the device will parse and execute the command if the command was entered correctly.

The example in the figure shows a CLI prompt switch>. The device in the example is named switch, and the operating CLI mode is indicated by the greater-than sign (>).

As a security feature, to limit the commands that a user can view and execute, Cisco IOS Software separates CLI sessions into two primary access levels:

User EXEC: Allows a person to execute only a limited number of basic monitoring commands.

Privileged EXEC: Allows a person to execute all device commands, for example, all configuration and management commands. This level can be password protected to allow only authorized users to execute the privileged set of commands.

* User EXEC mode

Cisco IOS Software has various modes that are hierarchically structured. The highest hierarchy is the user EXEC mode. It is followed by the privileged EXEC mode. From the privileged EXEC mode, you can proceed to the global configuration mode and from there to more specific configuration modes such as interface configuration mode and router configuration mode, as shown below.

* Privileged EXEC mode

Configuration mode

Subconfiguration modes (such as, interface, router)

* Cisco IOS Software and IOS XE Software apply the commands immediately.
* Cisco IOS XR Software has a two-stage commit system.

Because these modes have a hierarchy, you can only access a lower-level mode from a higher-level mode. For example, to access Global Configuration Mode, you must be in the Privileged EXEC mode. Each mode is used to accomplish particular tasks and has a specific set of commands that are available in this mode. Interface-specific configuration commands are available only in the Interface Configuration Mode. To access interface configuration commands, your full path through operation mode hierarchy would be: User EXEC Mode > Privileged EXEC Mode > Global Configuration Mode > Interface Configuration Mode. All commands that you enter and execute in Interface Configuration Mode apply only to the device interface you chose to configure.

You can tell the operation mode that you are in by looking at the prompt at the beginning of the line. Normally when you connect to a device, you are allowed access to the User EXEC Mode. In User EXEC Mode, you can change the console connection settings, perform basic connectivity tests, and display system information, but you cannot configure the device. To leave the User EXEC Mode (to close the console connection), you can use either the logout, exit, or the quit commands.

You do not have to return to global configuration mode in order to move to a different configuration mode. Rather, you can enter another configuration mode by typing the appropriate command at any configuration mode prompt.

| enable             | Enters privileged mode, providing access to advanced configuration and management commands. Exec is usually referred to privileged mode                                            |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| configure terminal | Moves to global configuration mode, where you can configure settings for various features and protocols on the device.                                                             |
| show               | Allows examination of device health status, interface details, routing information, and other operational parameters                                                               |
| debug              | Enables debugging messages, providing real-time monitoring of device processes and troubleshooting capabilities.                                                                   |
| ?                  | When typed after a command or in configuration mode, it displays a list of possible completions or available commands to assist in the configuration process                       |
| exit               | to exit the specific config mode                                                                                                                                                   |
| end                | to get out of the config mode into the exec                                                                                                                                        |
| terminal length 0  | disables output pagination so command results display continuously without pausing for user input - so that you don’t have to click space or enter to print the rest of the output |

![](<../.gitbook/assets/Unknown image (483)>)

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">show running configuration [all]</td><td valign="top"><p>Shows running configuration</p><p>Note If you encounter situation that command that is recognized in CLI, but not being saved to the configuration after pressing enter, it is possible that this command is default and thus not shown in configuration. You can verify with keyword all</p><p>use show run &#x3C;technology> or shrun | s &#x3C;technoogy> to view configuration of specific technology or protocol e.g show run vrf show run int &#x3C;int></p></td></tr><tr><td valign="top">copy running-config startup-config</td><td valign="top">to save running config to startup (making it permanent) - Writes data to non-volatile memory (NVRAM).</td></tr><tr><td valign="top">write mem OR wr</td><td valign="top">efficient alternative to copy</td></tr><tr><td valign="top">show history</td><td valign="top">display a list of previously entered commands in the current session. You can then use the arrow keys to navigate through the command history</td></tr><tr><td valign="top">show configuration history</td><td valign="top">display recent configuration changes</td></tr><tr><td valign="top">banner [exec | login | motd] ^ text ^</td><td valign="top"><p>use any delimiter (",$,^ or #), then press enter, insert the banner text and insert again the delimiter symbol, and press enter (MOTD - message of the day)</p><p>This MOTD banner is displayed to all terminals that are connected and is useful for sending messages that affect all users (such as impending system shutdowns).</p><p>Router1(config)# banner motd “</p><p>$(hostname) will be under maintenance on May 4th. Connectivity issues may be present.</p><p>“</p></td></tr><tr><td valign="top">show clock</td><td valign="top"></td></tr></tbody></table>

{% hint style="info" %}
If a command works in the CLI but doesn’t show up in `show running-config`, it may be a **default** command. Use `show running-config all` to confirm.
{% endhint %}

## Telnet

**Telnet** is a protocol built for remote access between remote systems. Client (the terminal application) and a Telnet server (the accessed network device)

Telnet can be used to test whether a TCP port is open on a remote device, but it cannot be used to test UDP ports.

\#telnet

| telnet 10.33.76.13 /source-interface \<VLAN/SVI> | to verfiy if the port is open and accepting connections from a switch |
| ------------------------------------------------ | --------------------------------------------------------------------- |

## SSH (Secure Socket Shell)

**SSH (Secure Socket Shell)** is Telnet's more secure successor for terminal connections. It uses asymmetric encryption for the contents of all messages within the SSH connection

| ssh \<IP/DNS>Example: ssh 10.1.1.1 Ssh admin@10.1.1.1 Ssh admin@N540X-16Z4G8Q2C-A | to log on a specific IP - or telnet if configured                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| --------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ssh -o KexAlgorithms=diffie-hellman-groupxx-shaxx user@IP                         | SSH common issues To fix "Unable to negotiate with 10.7.254.2 port 22: no matching key exchange method found. Their offer: diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1" - based on the mentioned parameters adjust the arguments when initiating ssh connection (where the xx is the number in the error - in this example group 14 and sha1) If it asks also for cipher use: ssh -o KexAlgorithms=diffie-hellman-group1-sha1 -o Ciphers=aes128-cbc mbrenek@IP                       |
|                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Configuration                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| hostname                                                                          | each configuration line is crucial and required for SSH to run                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ip domain-name martin.com                                                         | specifies the domain name of the cisco device itself. One of the things this is used for is security certificate generation for IPSEC, SSH or HTTPS access. The certificate will be generated using the entries you put in for hostname and domain name and is used on a router so you the router can do DNS lookups ip domain list <> <> <> // Each of the domains that you specified in the above command will be tried in turn.                                                                                                 |
| username martin secret martin                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| crypto key generate rsa modulus                                                   | RSA keys are used for public-key cryptography, which is a cryptographic system that uses a pair of keys: a public key and a private key. The public key can be freely distributed, while the private key must be kept secret. The modulus size of an RSA key determines its strength. A larger modulus size makes the key more difficult to crack. The recommended modulus size for RSA keys has increased over time as computing power has increased. Currently, the recommended modulus size for RSA keys is 2048 bits or higher |
| ip ssh version 2                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| line vty 0 15                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| transport input ssh                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| login local                                                                       | tells the router to use local credentials to authenticate inbound ssh connection                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

![](<../.gitbook/assets/Unknown image (484)>)

If SSH doesn't work despite configuration is complete

Try to upgrade the IOS to match on both devices, since there might be crypto bugs that won't allow devices to establish ssh connection

OR

Try to configure on the client and/or server side all available authentication methods:

\#ip ssh client algorithm mac hmac-sha1 hmac-sha2-256 hmac-sha2-256-etm@openssh.com hmac-sha2-512 hmac-sha2-512-etm@openssh.com

\#ip ssh client algorithm encryption 3des-cbc aes128-cbc aes128-ctr aes128-gcm aes128-gcm@openssh.com aes192-cbc aes192-ctr aes256-cbc aes256-ctr aes256-gcm aes256-gcm@openssh.com

\#ip ssh client algorithm kex curve25519-sha256 diffie-hellman-group14-sha1 diffie-hellman-group14-sha256 diffie-hellman-group16-sha512 ecdh-sha2-nistp256 ecdh-sha2-nistp384 ecdh-sha2-nistp521

\#ssh client algorithms cipher aes256-cbc aes256-ctr aes192-ctr aes192-cbc aes128-ctr aes128-cbc aes128-gcm@openssh.com aes256-gcm@openssh.com 3des-cbc

\#ssh server algorithms cipher aes256-cbc aes256-ctr aes192-ctr aes192-cbc aes128-ctr aes128-cbc aes128-gcm@openssh.com aes256-gcm@openssh.com 3des-cbc

#### IOS config

| show ip ssh                                  |                                                                                                    |
| -------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| show users                                   | to check active management sessions                                                                |
| show line                                    |                                                                                                    |
| show login                                   |                                                                                                    |
| clear line <>                                | to terminate active session                                                                        |
| login                                        | to switch from user on device to another user                                                      |
| login on-failure log or login on-success log | to log successful and failed login attempts of users through ssh use to no form to disable logging |
| show ip ports all                            | to display all open ports running on a device                                                      |

IOS-XR config [https://community.cisco.com/t5/networking-knowledge-base/ssh-configuration-on-cisco-ios-xr/ta-p/3146439](https://community.cisco.com/t5/networking-knowledge-base/ssh-configuration-on-cisco-ios-xr/ta-p/3146439)

| line default transport input ssh | Restricting management protocol to SSH only |
| -------------------------------- | ------------------------------------------- |

## Device Memory types

RAM (Random Access Memory) is a volatile type of memory, meaning it loses its contents when the device is powered off or restarted. It is used to store active processes, operational data, and the running configuration that the CPU actively uses. There are two main types of RAM. Dynamic RAM (DRAM) is built from cells composed of a capacitor and a transistor, and requires periodic refreshing to retain data—hence the term "dynamic." Static RAM (SRAM), on the other hand, does not require refreshing. It is faster and more reliable but also more expensive and offers lower data density, which is why it is typically used as CPU cache memory.

The RAM is the memory component that contains the running configuration. When the device starts, the system copies the startup configuration to the RAM. RAM does not retain stored information when the device is rebooted or powered off. Changes made to the running configuration should be copied to the startup-config file in the NVRAM.

ROM (Read-Only Memory) is non-volatile and contains microcode like the bootstrap program and basic diagnostics (like Power on self-test (POST) microcode), which is used to test the basic functionality of the switch hardware to determine which components are present.

Its contents cannot be modified under normal conditions. NVRAM (Non-Volatile RAM) retains information after the device is rebooted and is used to store the startup configuration file (startup-config), which is loaded during boot.

Flash memory is a non-volatile storage medium used to store the Cisco IOS software image, backup configuration files, and other system-related data. It retains its contents after a reboot and is writable, unlike ROM. Bootflash is a dedicated portion of flash memory, or sometimes a separate chip, used to store the bootloader or boot image. This image is critical for initializing the main IOS image during system startup.

EEPROM (Electrically Erasable Programmable Read-Only Memory) is a type of non-volatile ROM that allows data to be erased and rewritten at the byte level. It is typically used to store low-level system information, such as firmware versions, hardware IDs, and serial numbers. EEPROM is not usually accessed or modified by the administrator during normal operations.

External storage devices, such as USB flash drives, can be used with Cisco equipment for tasks like IOS upgrades, configuration backups, or log file storage. These devices can be accessed via the command-line interface using commands like dir usb0: or dir usb1:, depending on the USB slot in use.

### Cisco IOS Integrated File System (IFS)

This system allows you to create, navigate, and manipulate files and directories on a Cisco device. The directories that are available depend on the platform. The Cisco IFS feature provides a single interface to all the file systems that a Cisco device uses, including these systems:

#### Flash memory file systems

Network file systems such as TFTP, Remote Copy Protocol (RCP), and FTP

Any other memory available for reading or writing data, such as NVRAM and RAM

An important feature of the Cisco IFS is the use of the URL convention to specify files on network devices and the network. The URL prefix specifies the file location.

Commonly used prefix examples include the following:

flash: The primary flash device. Some devices have more than one flash location, such as slot0: and slot1:. In such cases, an alias is implemented to resolve flash: to the flash device that is considered primary.

nvram: NVRAM. Among other things, NVRAM is where the startup configuration is stored.

system: A partition that is built in RAM that holds, among other things, the running configuration.

tftp: Indicates that the file is stored on a server that can be accessed using the TFTP protocol.

ftp: Indicates that the file is stored on a server that can be accessed using the FTP protocol.

scp: Indicates that the file is stored on a server that can be accessed using the Secure Copy Protocol (SCP).

Directories and subdirectories can be used to organize files in manageable containers. A preceding slash (/) character indicates the root directory, and the slash character is also used to separate directory names from the directory’s contents. Individual files have names, which must be unique within the directory in which they are stored. URLs are used to specify files and they provide full specification of a locally stored file, including the prefix, directory path, and filename.

URLs that specify remote files can be more complex. After the prefix, a server location (IP address or resolvable hostname) must be specified. For protocols that require user authentication, usernames and passwords may be specified. If they are not specified in the URL, they will need to be specified interactively by the application.

The Catalyst 9000 switches running the IOS XE Fuji 16.9.1 and above support using an external USB 3.0 SSD (only official Cisco USB Drives are supported). This provides extra storage space mainly for application hosting. However, it can also be used as a general-purpose storage device. This USB 3.0 SSD cannot be used to boot images, emergency install images, or upgrade the internal flash. It is also not meant to be a replacement for the flash memory. The USB 3.0 SSD device from Cisco is shipped as a raw device and must be formatted. It supports all EXT-based file systems, such as EXT2, EXT3, and EXT4. To format your USB 3.0 SSD device, first connect it to the switch and then use the following command: format usbflash1:{ ext2 | ext3 | ext4 | secure}

| dir:                                    | Displays the contents of a directory.                                                                                                                                                                                                                                                   |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| copy:                                   | Copies files between two locations, such as from the device's flash memory to a TFTP server or vice versa.                                                                                                                                                                              |
| delete:                                 | Deletes files from the device's flash memory.                                                                                                                                                                                                                                           |
| delete /force /recursive bootflash:/IOS | to remove anything if delete or rmdir won't work                                                                                                                                                                                                                                        |
| mkdir                                   | Creates a new directory on the device's flash memory.                                                                                                                                                                                                                                   |
| rmdir                                   | Deletes a directory from the device's flash memory.                                                                                                                                                                                                                                     |
| rename:                                 | Renames a file or directory on the device's flash memory.                                                                                                                                                                                                                               |
| more:                                   | Displays the contents of a file on the terminal screen, one page at a time.                                                                                                                                                                                                             |
| edit:                                   | Opens a text editor that allows you to create or modify a file on the device's flash memory.                                                                                                                                                                                            |
| fsck                                    | check and repairs disk                                                                                                                                                                                                                                                                  |
| copy usb: bootflash:                    | // to copy usb files (such as IOC image) to bootflash and install IOS image install all \<IOS\_name.bin>                                                                                                                                                                                |
| show flash:                             | Displays the contents of the device's flash memory, including files and directories.                                                                                                                                                                                                    |
| show file systems                       | provides insightful information, such as the amount of available and free memory and the type of file system and its permissions. Permissions include read only (indicated by the "ro" flag), write only (indicated by the "wo" flag), and read and write (indicated by the "rw" flag). |

## Device booting process

Perform POST: This event is a series of hardware tests that verifies that all components of a Cisco router are functional. During this test, the router also determines which hardware is present. Power-on self-test (POST) executes from microcode that is resident in the system read-only memory (ROM).

Load and run bootstrap code: Bootstrap code is used to perform subsequent events such as finding Cisco IOS Software at all possible locations, loading it into RAM, and running it. After Cisco IOS Software is loaded and running, the bootstrap code is not used until the next time the router is reloaded or power-cycled.

Locate Cisco IOS Software: The bootstrap code determines the location of Cisco IOS Software that will be run. Normally, the Cisco IOS Software image is located in the flash memory, but it can also be stored in other places such as a TFTP server. The configuration register and configuration file, which are located in NVRAM, determine where the Cisco IOS Software images are located and which image file to use. If a complete Cisco IOS image cannot be located, a scaled-down version of Cisco IOS Software is copied from ROM into RAM. This version of Cisco IOS Software is used to help diagnose any problems and can be used to load a complete version of Cisco IOS Software into RAM.

Load Cisco IOS Software: After the bootstrap code has found the correct image, it loads this image into RAM and starts Cisco IOS Software. Some older routers do not load the Cisco IOS Software image into RAM but execute it directly from flash memory instead.

Locate the configuration: After Cisco IOS Software is loaded, the bootstrap program searches for the startup configuration file in NVRAM.

Load the configuration: If a startup configuration file is found in NVRAM, Cisco IOS Software loads it into RAM as the running configuration and executes the commands in the file one line at a time. The running configuration file contains interface addresses, starts routing processes, configures router passwords, and defines other characteristics of the router. If no configuration file exists in NVRAM, the router enters the setup utility or attempts an autoinstall to look for a configuration file from a TFTP server.

Run the configured Cisco IOS Software: When the prompt is displayed, the router is running Cisco IOS Software with the current running configuration file. You can then begin using Cisco IOS commands on the router.

![](<../.gitbook/assets/Unknown image (485)>)

Cat9k rommon syntax

| switch: boot flash:\<image\_name.bin> |                                               |
| ------------------------------------- | --------------------------------------------- |
| switch: dir                           |                                               |
| switch: set                           | displays all current variables for the switch |

Possible errors:

"MAC\_ADDR ROMMON variable is not set, rebooting", then add the MAC address with switch: set MAC\_ADDR=00:B1:E3:5B:F3:00 (use the MAC address of the switch chassis)

(Out of resources) proceed to upgrade the device with the lowest IOS possible, then upgrade it to the nearest next major release > for example load the switch with the IOS-XE 16.9.X, then upgrade to the 17.3.1, then to the latest recommended release such as 17.12

If the bootloader/rommon version stays same as the previous IOS-XE release and the switch won't boot to the new software withou manually entering boot, proceed to upgrade rommon with #upgrade rom-monitor capsule golden switch

### ROMMON (ROM Monitor)

is embedded Cisco firmware similar to BIOS that handles device boostrapping, diagnostic procedures including password recovery, software upgrades, and configuration settings.

IOS boot sequence is a process performed after an Cisco IOS device is powered on

The IOS device performs a power-on self-test (POST) to test its hardware components and choose an IOS image to load

To get into rommon you must interrupt the booting process > press ctrl+c or Ctrl + Break or Ctrl + Shift + 6 or ctrl^

Or you can configure the device to get into rommon after you reload it:

| conf t boot manual exit write memory reload | use the no form of the command to boot atuomatically (there is no config-register command in latest IOS-XE releases anymore) |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |

| rommon 1 > dev              | dir equivalent                                                                                                                                                          |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| rommon1 > dir bootflash:    |                                                                                                                                                                         |
| rommon 1 > boot <> ios      | or type only boot to use the existing ios image                                                                                                                         |
| rommon 1 > reset            | to reboot the router                                                                                                                                                    |
| rommon 1 > confreg \[XxXXX] | to change configuration register - if confreg command is not supported, just boot the switch and reconfigure it in global configuration mode - conf t > config-register |

### Configuration Register (Config register)

The bootstrap code checks the boot field of the configuration register. The configuration register is a 16-bit value; the lower 4 bits are the boot field. The boot field tells the router how to boot up. The boot field can indicate that the router looks for the Cisco IOS image in flash memory, or looks in the startup configuration file (if one exists) for commands that tell the router how to boot, or looks on a remote TFTP server. Alternatively, the boot field can specify that no Cisco IOS image will be loaded, and the router should start a Cisco ROM monitor (ROMMON) session.

The bootstrap code evaluates the configuration register boot field value as described in the following list. In a configuration register value, the "0x" indicates that the digits that follow are in hexadecimal notation. A configuration register value of 0x2102, which is also a default factory setting, has a boot field value of 0x2—the far right digit in the register value is 2 and represents the lowest 4 bits of the register.

If the boot field value is 0x0, the router boots to the ROMMON session.

If the boot field value is 0x1, the router searches flash memory for Cisco IOS images.

If the boot field value is 0x2 to 0xF, the bootstrap code parses the startup configuration file in NVRAM for boot system commands that specify the name and location of the Cisco IOS Software image to load. (Examples of boot system commands will follow.) If boot system commands are found, the router sequentially processes each boot system command in the configuration, until a valid image is found. If there are no boot system commands in the configuration, the router searches the flash memory for a Cisco IOS image.

If the router searches for and finds valid Cisco IOS images in flash memory, it loads the first valid image and runs it.

If it does not find a valid Cisco IOS image in flash memory, the router attempts to boot from a network TFTP server using the boot field value as part of the Cisco IOS image filename.

After six unsuccessful attempts at locating a TFTP server, the router starts a ROMMON session.

The procedure for locating the Cisco IOS image depends on the Cisco router platform and default configuration register values. The procedure that is described here applies to the Cisco Integrated Services Routers.

[https://www.cisco.com/c/en/us/support/docs/switches/catalyst-9300-series-switches/216850-configuration-register-equivalent-clis-i.html](https://www.cisco.com/c/en/us/support/docs/switches/catalyst-9300-series-switches/216850-configuration-register-equivalent-clis-i.html)

![](<../.gitbook/assets/Unknown image (486)>)

| Operation                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Cisco IOS config-register value                                                                                                                                                                                                             | Equivalent Cisco IOS XE CLI                                                                          |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Boot normally this is the default configuration register value for most Cisco devices instructing the device to load the startup configuration file from NVRAM and boot the IOS image                                                                                                                                                                                                                                                                                                                                      | 0x2102, 0xF                                                                                                                                                                                                                                 | Switch(config)#no boot manual                                                                        |
| Boot to rommon This setting boots the device using the first image in flash memory without loading the startup configuration. Used during the password recovery process.                                                                                                                                                                                                                                                                                                                                                   | 0x0,0x2120, 0x2100                                                                                                                                                                                                                          | Switch(config)#boot manual                                                                           |
| Enable Break/Disable Break                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 0x2120/ residual register values                                                                                                                                                                                                            | Switch(config)#\[no] boot enable-break                                                               |
| Setting Baud/ Console line speed                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | 0x102, 0x2101, 0x2102, 0x2142 : 9600 baud rate 0x1202 : 1200 baud rate 0x2120, 0x2122, 0x2124 : 19200 baud rate 0x2902 : 4800 baud rate 0x2922 : 38400 baud rate 0x3122 : 57600 baud rate 0x3922 : 115200 baud rate 0x3902 : 2400 baud rate | Switch(config)#line console 0 Switch(config-line)#speed ? <0-4294967295> Transmit and receive speeds |
| Ignore startup This setting causes the device to bypass the startup configuration when booting up It is often used to recover passwords because it allows you to access the device without loading the startup configuration. The router boots without any configuration so you can #erase startup-configuration and reload the router to delete previous configuration And don’t forget to put the config-register to the default value #config-register 0x2102 , so it can boot and retain the configuration as expected | 0x2142                                                                                                                                                                                                                                      | Switch(config)#system ignore startupconfig                                                           |
| Ignores break Prevents console users from interrupting boot with a BREAK key, so the device will boot normally                                                                                                                                                                                                                                                                                                                                                                                                             | 0x102, 0x2101, 0x2102, 0x2122, 0x2124, 0x2142, 0x2902, 0x2922, 0x3122, 0x3902, 0x3922                                                                                                                                                       | Switch(config)#\[no] boot manual Switch(config)#\[no] boot enable-break                              |
| Disable password recovery                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | 0x102                                                                                                                                                                                                                                       | Switch(config)#system disable password recovery                                                      |

| show version  | to view system version and all other software related information                                                           |
| ------------- | --------------------------------------------------------------------------------------------------------------------------- |
| show boot     | shows the current boot sequence. It shows the order in which the device will attempt to boot and load its operating system. |
| show platform | detailed information about all hardware within the device and their status                                                  |

Before router reboot > verify that there is no difference between running and startup config #sh arch conf diff - to prevent loss of any unsaved changes

{% hint style="info" %}
Don’t copy-paste a `boot system ...` line from an old router blindly. If the image filename does not exist on the new device, it can boot-loop.
{% endhint %}

### Password Recovery

Reboot the router

During the booting process press Ctrl+C to get into ROMMON (or break command in the terminal)

In rommon type confreg 0x2142 or 0x142

Type #reset - to reboot the router

Once you get to the Router>

go to enable and run copy startup-config running-config to load the config

Then go to the configuration mode and remove the existing password or login configuration and add desired user credentials and write it to the startup with #wr

{% hint style="info" %}
To fully sanitize a router (remove configuration), you can use:
{% endhint %}

| #write erase                                     | Erases NV memory - efficient version of erase command, erase is more targeted/specific                                                                                                    |
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| #erase \[/all \| nvram:\| startup configuration] | /all: Purpose: Erases all files in NVRAM nvram: Purpose: Erases the files in the NVRAM filesystem startup-config: Purpose: Specifically erases the startup configuration stored in NVRAM. |

{% hint style="info" %}
`erase ...` requires a reboot to take effect.
{% endhint %}

C9k password recovery [https://www.cisco.com/c/en/us/support/docs/switches/catalyst-9200-series-switches/223262-perform-password-recovery-on-catalyst.html](https://www.cisco.com/c/en/us/support/docs/switches/catalyst-9200-series-switches/223262-perform-password-recovery-on-catalyst.html)

### Loading Cisco IOS Configuration Files

When a Cisco router locates a valid Cisco operating system image file in the flash memory, the Cisco operating system image is normally loaded into RAM to run. Image files are typically compressed, so the file must first be decompressed. After the file is decompressed into RAM, it is started.

For example, when Cisco IOS Software begins to load, you may see a string of hash signs (#), as shown in the figure, while the image decompresses.

The Cisco IOS image file is decompressed and stored in RAM. The output shows the boot process on a router.

After the Cisco IOS Software image is loaded and started, the networking device must be configured to be useful. If there is an existing saved configuration file (startup-config) in NVRAM, it is executed. If there is no saved configuration file in NVRAM, the device typically either begins autoinstall or enters the setup utility. An example below illustrates the procedure in a Cisco IOS router.

The router loads and executes the configuration from NVRAM. If no configuration is present in NVRAM, it prompts for an initial configuration dialog (also called setup utility or setup mode).

If the startup configuration file does not exist in NVRAM, the router may search for a TFTP server. If the router detects that it has an active link, it sends a broadcast searching for a configuration file across the active link. This condition will cause the router to pause.

The setup utility prompts the user at the console for specific configuration information to create a basic initial configuration on the router, as shown in this example:

\--- System Configuration Dialog ---

Would you like to enter the initial configuration dialog? \[yes/no]:

If you type "yes" at this stage, the setup utility prompts you for basic information about your router and network, and it creates an initial configuration file. Since the result configuration is basic, typically you would type "no" at this point and continue with a manual configuration.

Ensure that the downloaded cisco image is not corrupted by comparing the md5/sha checksum on the cisco software page with the verify md5/sha command on the router

Cisco IOS Image and/or Rommon Upgrade

Check how much free space is available on the switch. This will save you a lot of time even before starting to think about upgrading the switch.

Check the requirements to upgrade to the most recent recommended version.

Read the release notes of the software version you plan to upgrade to.

Schedule a maintenance window to upgrade the switch if it is online (even if it's a stack, VSS, or vPC).

If you'll be onsite, upload the software image using a USB drive or transfer to the switch using a T/FTP server.

Backup the current software image and the configuration of the switch.

If the switch is online, have a list of services or applications to test before starting the upgrade - You'll need to validate that those services are still working after the upgrade.

Install the new software image.

Monitor the upgrade process. Some upgrade processes are interactive, which means that you'll be required to confirm or deny certain operations.

If the upgrade was successful, verify services running on the switch and confirm that it is operating as expected

Check whether the services you identified before the upgrade are still working.

Save the configuration!

#### Best Practises

If you suggest customer to upgrade IOS - check whether all services and features that the customer is using is supported in the new IOS version as it might also have different syntax

If you want to configure different IP on interface without losing access to the device, for example that you are connected to the device on the old IP address, you can configure the new address as #ip add <> secondary and test access via ssh

It is a best practise to run reload in xx when you do configuration changes in case you mistype any command the router will reboot with the previous config

if you sucessfully configure the router you can cancel the reload and copy running config to the startup

If you copy txt file with config to the running config it will merge both current running and the config in the txt file, so make sure to compare the uploaded running config with the original config txt file. These commands can also be used to delay the reload to be performed in a specific time

\#reload at <>

\#reload in <>

\#reload cancel

#### Process

| Download the correct software IOS version for your router model and requirements with ROMMON file from the official Cisco website                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------- |
| Exec# show version > to check the current IOS version and ROMMON version #show platform to display additional platform information. Verify that there is space in the bootflash #show bootflash: Plug in the flash drive and verify that the IOS and ROMMON is present on #show usb0:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                 |
| #show file systems                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | To check the specific identifier for the plugged external usb drive. It can be usbflash0 usb0 or usbflash1 usb1 |
| Copy new ROMMON to bootflash: directory #copy usb0: bootflash:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                                 |
| Verify MD5 checksum of ROMMON package between cisco software page and the copied ios on the router #verify bootflash:/\[package name]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                 |
| Install new ROMMON #upgrade rom-monitor bootflash:/\[package name] \<R0 \| all > and perform reboot #wr #reload #show platform to verify that the new ROMMON is running                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |                                                                                                                 |
| Copy new IOS to bootflash: #copy usb0:/\[IOS name bootflash] bootflash: #verify bootflash:/\[IOS name] to verify MD5 hash, the integrity of the new IOS image. There should be displayed "Embedded hash verification successful" or compare the MD5 checksum between Cisco software page (where you downloaded the IOS) and the generated by the cisco device                                                                                                                                                                                                                                                                                                                                                                                                                                                            |                                                                                                                 |
| <p>Now perform either budnled or install mode (Bundle mode (Discontinued after release 17.18)) Configure the router to boot with in the bundled mode with the new IOS Router(config)#boot system bootflash:/[new IOS name] Note: In case there is existing ios image run #no boot and remove if required to free up space with #rm , otherwise leave it as second boot statement for backup:<br>#boot system bootflash:/[new IOS name] #boot system bootflash:/[old IOS name] Copy to startup config: Router(config)#write memory or Router#copy running-config startup-config and Reload the router #reload<img src="../.gitbook/assets/Unknown image (487)" alt=""> Note: if you encounter issue that the router won't boot after upgrade of universalk9 image, try to upgrade to npe version or other IOS version</p> |                                                                                                                 |
| If dealing with large configuration instead of manually inserting lines through console, copy the configuration as a txt file from your flash to the nvram:/startup-config - #copy usb0:/\[FILENAME.txt] startup-config then copy the startup to running config (Reload the router if needed) - Encountered with long ACL on MSP replacement of C8500 Keep in mind to test the syntax prior uploding such configuration file to the device - the current IOS may have a different syntax for a certain features (e.g when you upgrading older hardware with older software IOS to the new hardware with new IOS)                                                                                                                                                                                                         |                                                                                                                 |
| Verify that the purchased licenses are enabled with show license <>If they are not use (config)#license boot level <>Save the configuration to ensure the license features are retained: Router(config)#write memory or Router#copy running-config startup-config Reboot the router and check that licence has been enabled #sh ver #show license <>                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |                                                                                                                 |

This next example specifies a TFTP server as a source of a Cisco IOS image, with a ROMMON session as the backup:

Branch(config)# boot system tftp://c2900-universalk9-mz.SPA.152-4.M1.bin

Branch(config)# boot system rom

{% hint style="info" %}
Cisco devices typically support only FAT32 USB media.
{% endhint %}

![](<../.gitbook/assets/Unknown image (488)>)

### Managing Cisco IOS Images and Device Configuration Files

As a network grows, keeping track of all of the Cisco IOS Software images and configuration files running on your devices is key.

Production internetworks usually span wide areas and contain multiple routers. For any network, it is important to retain a backup copy of the Cisco IOS Software image in case the system image in the router becomes corrupted or is accidentally erased.

Widely distributed routers need a source or backup location for Cisco IOS Software images. Using a network TFTP server allows image (and configuration) uploads and downloads over the network. Storing Cisco IOS Software images and configuration files on a central TFTP server enables you to control the number and revision level of the files that must be maintained. The network TFTP server can be another router, a workstation, or a host system.

You can copy Cisco IOS image files from a TFTP, RCP, FTP, or SCP server to the flash memory of a networking device. You may want to perform this function to upgrade the Cisco IOS image, or to use the same image as on other devices in your network.

You can also copy (upload) Cisco IOS image files from a networking device to a file server by using TFTP, FTP, RCP, or SCP protocols, so that you have a backup of the current IOS image file on the server.

The protocol you use depends on which type of server you are using. The FTP and RCP transport mechanisms provide faster performance and more reliable delivery of data than TFTP. These improvements are possible because the FTP and RCP transport mechanisms are built on and use the TCP/IP stack, which is connection-oriented.

Device configuration files contain a set of user-defined configuration commands that customize the functionality of a Cisco device.

Device configurations can be loaded from the following components:

NVRAM

Terminal

Network file server (for example, TFTP, SCP, and others)

startup-config Stores the initial configuration used anytime the switch reloads Cisco IOS; is stored in NVRAM

running-config stores the currently running configuration commands. This file changes dynamically when someone enters commands in configuration mode; is stored in RAM

When a configuration is copied into RAM from any source, the configuration merges with or overlays any existing configuration in RAM, rather than overwriting it. New configuration parameters are added, and changes to existing parameters overwrite the old parameters. Configuration commands that exist in RAM for which there are no corresponding commands in NVRAM remain unaffected. In contrast, copying a configuration from any source into the startup configuration file in NVRAM will overwrite the startup configuration file in NVRAM.

## Trivial File Transfer Protocol (TFTP)

is a simpler file transfer protocol designed for basic file transfers between two systems

TFTP/FTP are based on Client-server model where FTP client has to be installed to access the FTP server running on a remote machine.

It lacks authentication, encryption and directory listing capabilities. Does not provide comprehensive error recovery mechanisms.

TFTP is often used for transferring firmware or configuration files to or from networking devices. TFTP uses UDP and operates on port 69

When the client sends the first message to the server, the destination port is UDP 69 and the source is a random ephemeral port

This random port is called a 'Transfer Identifier' (TID) and identifies the data transfer.

The server then also selects a random TID to use as the source port when it replies, not 69.

When the client sends the next message, the destination port will be the server's TID, not 69.

{% hint style="info" %}
Telnet is not a valid way to test TFTP. Telnet uses TCP, while TFTP uses UDP/69.
{% endhint %}

![](<../.gitbook/assets/Unknown image (489)>)

TFTP config on server side

| tftp-server ip tftp source-interface Lo0 | Enable the TFTP server on the source router by specifying the file in directory that is eligible to be accessed by tftp clients. Important: on Linux/Windows you set the server root directory in the TFTP config. Clients then request only the filename (not the full path).![](<../.gitbook/assets/Unknown image (490)>)![](<../.gitbook/assets/Unknown image (491)>) |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

{% hint style="info" %}
On Linux/Windows TFTP servers, you typically configure the TFTP root directory once. Clients should request only the filename, not the full path.
{% endhint %}

## File Transfer Protocol (FTP)

is a more comprehensive protocol that supports features such as authentication, directory listing, improved error handling, and is bidirectional - able to transfer files in both directions. Two TCP communication channels (ports) are established for the FTP connection.

The first is used to establish command channel on port 21 where the client sends the FTP commands to perform FTP functions (full list is at the bottom of this section)

The second is to establish data channel on port 20 used for the data transfer

FTP Modes

### Active FTP

The client uses a random port to send a command (PORT command) to the server’s port 21. This command tells the server which data port on the client-side it should connect to.

The server then use the source port 20 reach the destination port specified in PORT command to establish a connection with the client

This process is called Active because the client is actively specifying the port number it prefers the server to connect to. In an Active FTP, the server is the one initiating the connection, following the command of the client

This is an older version, that is rarely used, due to the

![](<../.gitbook/assets/Unknown image (492)>)

### Passive FTP

This is the newer FTP version, that addresses major issue with FTP in modern networks. Since all modern networks have firewall in their environment, which usually don't permit outside devices to initiate connections to inside network, preventing the active FTP outside server to establish TCP connection on a random destination port that might be blocked by the firewall

In Passive FTP, the client uses a random source port to send a command (PASV command) to port 21 of the server.

The server responds with an acknowledgement and provides the IP and port number that it wants the client to initiate the data connection on

The client then initiates the new TCP connection for the data connection on a random source port to the provided destination port to send data to the server

![](<../.gitbook/assets/Unknown image (493)>)

## Secure File Transfer Protocol (SFTP)

uses a secure channel to transfer files, providing encryption for both authentication and data transfer

It is designed to address the security shortcomings of traditional FTP, including the clear transmission of usernames, passwords, and data

Authentication in SFTP is handled using SSH key pairs or other methods supported by the SSH protocol. This enhances security by eliminating the need to transmit passwords in clear text. SFTP usually operates over port 22 by default. This is the default port for SSH, the underlying protocol used by SFTP

| FTP Config on client side                                                                               |                                                                                                                                                                                                        |
| ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| no ip ftp passive                                                                                       |                                                                                                                                                                                                        |
| ip ftp username <>                                                                                      |                                                                                                                                                                                                        |
| ip ftp password <>                                                                                      |                                                                                                                                                                                                        |
| ip ftp source-interface Lo0                                                                             | Loopback interface provides a stable and reliable source IP for network communications, it ensures consistent FTP connections regardless of changes to states or configurations of physical interfaces |
| copy tftp: running-config Address or name of remote host \[]? Server IP Destination filename?           | format: copy to specify credentials ftp://user:pass@/test.txt however the intro prompt asks for it anyway to upload IOS to the FTP: copy running-config ftp:                                           |
| FTP config on server side ip ftp server ip ftp username <>ip ftp password <>ip ftp source-interface Lo0 |                                                                                                                                                                                                        |

## SCP (Secure Copy Protocol)

is a secure network protocol used to transfer files between systems over the network via an SSH connection

| username <> privilege 15 secret <>crypto key generate rsa modulus 2048 ip domain-name <>  |                                                                                                               |
| ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| aaa new-model aaa authentication login default local aaa authorization exec default local | SSH and AAA must be configured for SCP to work to configure default basic AAA to prompt for local credentials |
| ip scp server enable                                                                      | to enable scp                                                                                                 |
| copy bootflash: scp:                                                                      | for cisco CLI \[vrf ] can be specified in case vrf is in use                                                  |
| scp                                                                                       | for linux shell                                                                                               |
| scp local\_file.txt user@remote\_host:/path/to/destination                                | Copy a file from the local system to a remote system:                                                         |
| scp user@remote\_host:/path/to/source\_file.txt local/directory/                          | Copy a file from a remote system to the local system:                                                         |
| copy scp: stby-bootflash: vrf Mgmt-intf                                                   |                                                                                                               |

![](<../.gitbook/assets/Unknown image (494)>)

| scp C:\Users\mbrenek\Downloads\asr920igp-15\_6\_46r\_s\_rommon.pkg cisco@10.19.55.224:flash:asr920igp-15\_6\_46r\_s\_rommon.pkg scp C:\Users\mbrenek\Downloads\PonCntlInit.json cisco@10.19.55.10:harddisk:PonCntlInit.json scp C:\Users\mbrenek\Downloads\PonCntlInit.json cisco@10.19.55.10:/var/xr/disk1/PonCntlInit.json scp C:\Users\mbrenek\Downloads\Cisco\_Routed\_PON\_25.3.1\_Release.zip cisco@10.19.55.21:/home/cisco scp cisco@10.19.55.10:harddisk:ems.pem C:\Users\mbrenek\Downloads\ems.pem scp cisco@10.19.55.10:/misc/config/grpc/ems.pem ./ | Windows Powershell support scp, don't forget to specify the destination file name if you want to download it to the specific folder - can be named as the source You can also specify only the source and it will be downloaded to the default folder of the SCP server or into your user folder |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

![](<../.gitbook/assets/Unknown image (495)>)

![](<../.gitbook/assets/Unknown image (496)>)

If you still have issues to transfer files iwth the error Connection to 10.19.55.143 closed by remote host. use the -O option and the destination file name aswell ref:

[https://www.cisco.com/c/en/us/support/docs/troubleshooting/220371-scp-from-clients-on-openssh9-0-to-ios-xe.html](https://www.cisco.com/c/en/us/support/docs/troubleshooting/220371-scp-from-clients-on-openssh9-0-to-ios-xe.html)

| scp -O C:\Users\mbrenek\Downloads\ONT.xml alef@10.19.55.143:flash:ONT1.XML |   |
| -------------------------------------------------------------------------- | - |

![](<../.gitbook/assets/Unknown image (497)>)

The error message you're seeing indicates a "Host key verification failed" issue when trying to use SCP (Secure Copy Protocol) with OpenSSH. This typically happens when the host key of the remote server has changed, the system has been replaced, or your local system doesn't recognize the host key, potentially indicating a man-in-the-middle attack or a legitimate change in the server's configuration. Here's how you can resolve it:

| ssh-keygen -R 10.19.55.21 |   |
| ------------------------- | - |

![](<../.gitbook/assets/Unknown image (498)>)

## HTTP for File transfer

| HTTP server config on cisco router                | show ip http server status                      |
| ------------------------------------------------- | ----------------------------------------------- |
| username user privilege 15 password 0 pass        |                                                 |
| hostname                                          |                                                 |
| ip domain-name                                    |                                                 |
| crypto key generate rsa general-keys modulus 2048 | // self-signed SSL certificate                  |
| ip http secure-server                             | Configure the router to use the SSL certificate |
| ip http secure-active-session-modules all         |                                                 |
| ip http secure-trustpoint local-self-signed       |                                                 |
| ip http server                                    | // enables http server                          |
| ip http bind-address                              | // to listen on specific IP address             |
| ip http authentication local                      | // enables authentication                       |
| ip http path <>                                   |                                                 |
| on client side                                    |                                                 |
| ip http client source-interface Loopback0         |                                                 |
| ip http client username user                      |                                                 |
| ip http client password 0 pass                    |                                                 |

## PSCP (PuTTY Secure Copy)

is a secure file transfer client for Windows. It is part of the PuTTY suite of tools named pscp.exe

It is a command line application - meaning u cannot open it by double clicking on it, you have to drag the icon of it from folder into the command prompt and then type:

| pscp username@remote-server:/path/to/source-file C:\path\to\destination-directory |   |
| --------------------------------------------------------------------------------- | - |

![](<../.gitbook/assets/Unknown image (499)>)

![](<../.gitbook/assets/Unknown image (500)>)

## Cisco IOS WebUI

Even though the management of Cisco IOS devices, such as switches and routers, is usually performed via the CLI, these devices also support a WebUI that can be easily enabled. WebUI provides a GUI method of interacting with the device using your web browser.

The Cisco IOS WebUI feature set differs between device types. Some devices, such as the Cisco Catalyst 9800 Wireless LAN Controller, allow you to completely configure your wireless network using the WebUI, while switches and routers might only support a few main configuration and monitoring options.

The Cisco WebUI consists of the following categories, found in the main menu:

The Dashboard provides insight into the overall health and operation of your network device. While the dashboard can be customized, it will only support features specific to the device. For example, the Wireless LAN Controller (WLC) dashboard includes a widget for the number of active access points, whereas a switch dashboard includes a widget for Power Over Ethernet (PoE) consumption.

The Monitoring tab is useful for observing the operation of the device. The available monitoring options will vary depending on the device you are using. For instance, on a switch, you can monitor port status and traffic, while on a wireless controller, you can monitor wireless clients.

The Configuration tab is used for setting up your device. It serves as the WebUI equivalent of the CLI configure terminal mode.

The Administration tab allows you to perform administrative tasks such as creating new users or upgrading the device's IOS image.

The Troubleshooting tab is designed for diagnosing issues with your device. Here, you can view logs, perform pings and traceroutes, and even capture packets to generate a packet capture (PCAP) file, which you can download and inspect using your preferred network analyzer, such as Wireshark.

The Troubleshooting tab is designed for diagnosing issues with your device. Here, you can view logs, perform pings and traceroutes, and even capture packets to generate a packet capture (PCAP) file, which you can download and inspect using your preferred network analyzer, such as Wireshark.

### Enabling Cisco IOS WebUI

Enabling the Cisco IOS WebUI is a straightforward process. The prerequisite is that the device already has an IP address and a configured username and password. After that, you simply need to start the HTTP and HTTPS servers and enable authentication.

switch(config)# username admin privilege 15 secret cisco123

switch(config)# ip http server

switch(config)# ip http secure-server

switch(config)# ip http authentication local

{% hint style="info" %}
Although the config supports both HTTP and HTTPS, disable HTTP. Use HTTPS only for device WebUI access.
{% endhint %}

Once the WebUI is enabled, you can access it via your browser by navigating to [https://device-ip-address](https://device-ip-address/). By default, the standard HTTP and HTTPS ports (80 and 443) are used.

If you want to verify that the WebUI server is running using CLI, you can use the show ip http server status command.

While most engineers still prefer using the CLI to configure network devices due to its robustness and comprehensive command set, the Cisco IOS WebUI offers an alternative method for device management. You can use both the CLI and Cisco IOS WebUI simultaneously, choosing the best tool for the task at hand. For example, configuring ACLs or VLANs may be more straightforward with the CLI, whereas monitoring events and upgrading the IOS image might be more conveniently done through the Cisco IOS WebUI.

## Software Packaging

Cisco IOS Software or Cisco NX-OS Software image is an executable file that contains one or more feature sets for a specific platform.

Cisco IOS Software images are typically distributed in binary format, as denoted by the .bin filename extension. These binary image files contain the compiled code and data that the

TAR stands for "tape archive," and it's a common archive format used in Unix and Linux systems.

When a Cisco IOS Software image is distributed in the .tar format, it's essentially an archive that contains one or more binary image files along with associated data and metadata

Metadata is a set of data that describes and gives info about other data

Train provides a vehicle for delivering software with a specific set of features to a specific set of platforms. As such, it consists of individual software releases

Universal Software Images: Certain switches and routers, including Cisco Catalyst 3560-E/X, 3750-E/X, 4500E Supervisor Engine 7-E Modules, and ISR G2 Routers, ship with a single, universal Cisco IOS Software image. This image contains all available features, and administrators can enable specific feature sets using licenses.

universalk9: Includes all Cisco IOS Software features, including strong payload cryptography features like IPsec VPN, SSL VPN, and Secure Unified Communications.

universalk9\_npe (No Payload Encryption): Excludes strong payload cryptography features to satisfy export and import requirements for countries that do not support strong cryptography

Within each universal software image, features are grouped into feature sets. Administrators activate specific feature sets by using technology package licenses via Cisco Software Activation licensing keys

### Software and Hardware Lifecycle

First Customer Shipment (FCS): This marks the initial availability of the release to customers.

End-of-Sale Announcement: Six months before the actual end-of-sale date, Cisco publicly announces the upcoming end-of-sale for the release. Administrators are advised to begin evaluating alternative releases and develop migration plans when this announcement is made.

End of Sale (EoS): This is the last day when customers can order the release through Cisco's point-of-sale mechanisms, and it's also the last day when manufacturing shipments of Cisco hardware include the release.

End of Software Maintenance (EoSW): This marks the last day when Cisco Engineering may release any final software maintenance updates or bug fixes for the release. After this date, development and support cease, and support is provided through supported successor releases.

End of Security Vulnerability Support (EoSV): This date is the last day when Cisco Engineering may release fixes addressing security vulnerabilities and issues, specific to the release and train. In some cases, security fixes may be provided for a brief time after the end of software maintenance.

Last Date of Support: The Cisco Technical Assistance Center (TAC) provides service and support for the release until this date. Afterward, all support services for the release become unavailable, and it reaches end-of-life status.

End-of-Life (EoL): After the last date of support, the release becomes obsolete. It is no longer sold, manufactured, improved, repaired, maintained, or supported. Detailed end-of-sale

and end-of-life information can be found in the End-of-Life Policy.

A manufacturer may also declare end-of-sale because certain components are no longer manufactured or the contract with suppliers is cancelled.

![](<../.gitbook/assets/Unknown image (501)>)

### Important communications

Security advisory: Addresses security concerns directly impacting Cisco products and may require customer action.

Deferral advisory: Announces removal of a problematic software image, urging customers to migrate to a replacement.

Software advisory: Provides solutions for defects; customers impacted should consider upgrading to a replacement image.

### Firmware

Cisco Integrated Management Controller (CIMC) firmware is a management module, which is built into the motherboard. A dedicated ARM-based processor, separate from the main server CPU, runs the CIMC firmware. The system ships with a running version of the CIMC firmware. You can update the CIMC firmware, but no initial installation is needed.

CIMC is the management service for the E-Series Servers. CIMC runs within the server. You can use CIMC to access, configure, administer, and monitor the server.

BIOS Firmware initializes the hardware in the system, discovers bootable devices, and boots them in the provided sequence. It boots the operating system and configures the hardware for the operating system to use. BIOS manageability features allow you to interact with the hardware and use it. In addition, BIOS provides options to configure the system, manage firmware, and create BIOS error reports. The system ships with a running version of the BIOS firmware. You can update the BIOS firmware, but no initial installation is needed.

### Operating System or Hypervisor

The main server CPU runs on an operating system such as Microsoft Windows, Linux, or Hypervisor. You can purchase an E-Series Server with a preinstalled operating system such as Microsoft Windows or VMware vSphere Hypervisor TM, or you can install your own operating system

## IOS-XE

Older Cisco IOS was a monolithic operating system running directly on hardware, with shared process memory

Monolithic - refers to something that is whole, unified and undivided where all components and functions are integrated into a single whole

Since Cisco IOS is replaced with XE and XR, the below software packages are renamed

![](<../.gitbook/assets/Unknown image (502)>)

IOS-XE is an evolution of IOS with distributed architecture, separating the data plane and control plane, optimized for enterprise networks, offerring programmability REST/NETCONF and SDWAN features, Python scripting, Preboot execution for [ZTP/PnP, Guest Shell Linux Containers (LXCs)](onenote:SDN.one#ZTP%20and%20Guest%20Shell\&section-id={2DC2FCEA-3B33-4FFE-8240-49F35B5E0C99}\&page-id={BF2E0122-F446-486A-AF5E-DE6FA82896BE}\&end\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE) securely host third-party Linux applications.

One of its significant enhancements is support for symmetric multiprocessing (SMP). This allows the operating system to take advantage of multiple CPU cores, improving performance and scalability by distributing processes across multiple processors.

Processes can be statefully upgraded or restarted without taking down the device.

Features can be deployed and rolled back in minutes without changing the underlying software image

Much of the code base is the same between Cisco IOS and IOSd daemon running on a Cisco IOS XE router, while Cisco IOS XR was built from the ground up with advanced architecture and features focusing on reliability and modularity of the system. This is one of the reasons for the different command syntax structure between the two flavors

IOS XE architecture is based on the Linux kernel, which in turn controls all the aspects of the OS. Module drivers reside on top of the kernel, as does a common infrastructure module. Cisco IOSd is the main Cisco IOS process, running as an application on the Linux kernel. Other Cisco IOS subsystems run as separate processes, giving the whole system more resilience to failing components

![](<../.gitbook/assets/Unknown image (503)>)

IOS daemon (IOSd) is responsible for handling many core processes and services within the operating system such as process management, resource allocation, config mgmt

daemon refers to a background process or program that runs continuously, usually without direct user interaction, to perform various tasks or services on a computer system

![](<../.gitbook/assets/Unknown image (504)>)

Device mode boots the router using the local image. If there is no configuration file present. The device uses DHCP to determine whether the device should enter Zero Touch Provisioning (ZTP) or Network Plug and Play (PnP) automated provisioning mode, based on options in the DHCP offer. The DHCP options available for these modes are 67 and 43

ZTP mode automates the process of installing or upgrading software images and installing configuration files on Cisco devices that are deployed in a network for the first time. When a device boots up and does not find the startup configuration, the device enters the ZTP mode. The device locates a DHCP server, bootstraps itself with its interface IP address, gateway, and Domain Name System (DNS) server IP address, and enables Guest Shell. The device then obtains the IP address or URL of an HTTP or TFTP server through DHCP Option 67 and downloads the Python script to configure the device

Network PnP mode Upon receiving DHCP Option 43, the device initiates a connection to the PnP server, such as Cisco DNA Center. This action triggers a predefined workflow that can install a certificate, upgrade the Cisco IOS XE Software version, and apply the initial configuration to the device

Preboot Execution Environment (PXE) mode is an open standard for network booting. It allsows the device to boot an image that is hosted on an external server. The PXE server is automatically detected by the device, using the standard PXE interface. The Cisco IOS XE image can then be downloaded to the device using HTTP, FTP, or TFTP protocol

### Bundle mode (Discontinued after release 17.18)

(Consolidated) is traditional way where IOS is stored in .bin file unbundled and installed in each boot process and loaded to RAM, resulting in increased booting process and RAM utilization, since all packages, even unused are activated. This enabled you in the past to delete the IOS from the flash while the system was running without affecting the switch while it was in a running state. You will not be able to apply an SMU if the switch is in Bundle Mode

When installing Cisco IOS® XE on a device, there are two options for installation and boot: Install Mode and Bundle Mode. After Release 17.18. Bundle Mode (.bin file) will be discontinued, and Install Mode (packages.conf file) will be the only option supported.

#### Upgrade Process

| boot bootflash:/ | configure traditional boot statement for router to pull IOS rom the flash and perform reload |
| ---------------- | -------------------------------------------------------------------------------------------- |

To convert from install to bundle just remove the bootflash:/IOS/packages.conf and configure the boot \<ios.bin>

![](<../.gitbook/assets/Unknown image (505)>)

#### Rommon Upgrade

Cisco IOS XE 17.3.2 and later versions do not require a ROMMON upgrade prior to installing a new IOS image

IOS XE has an integrated ROMMON, which means it's part of the operating system itself. This eliminates the need for a separate ROMMON upgrade

During migration to Cisco IOS XE version 17.x.x, the ROMMON auto-upgrade and FPGA upgrade completes in two reloads. ROMMON has to be upgraded on 920 first

The router updates the FPD images automatically as part of the Cisco IOS boot process.

In case you are upgrading from lower IOS-XE versions, upgrade the ROMMON first - \[in Cisco system section]\(onenote:#Cisco System\&section-id={58B48AF9-0B19-4885-B24D-492AA8D605CF}\&page-id={80496B1B-8459-432C-941A-7186DC378467}\&end\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE/Fundamentals.one) #upgrade rom-monitor bootflash:/\[package name] R0

### Install mode

(Sub-package) is the default recommended mode, where .bin file is unpacked into sub-packages (.pkg and packages.conf), each pkg/module of the monolithic .bin file can be pre-installed and activated directly on the flash, so that the device doesn't have to process and load the entire .bin file during the boot process, improving boot time and transitions between software versions

However this approach requires 2x image size available space in bootflash memory since the .bin file is unpacked directly on it

Provisioning files (package.conf) manages the boot process when the router is configured to boot in sub-packages

The provisioning file manages the bootup of each individual sub-package

Provisioning files are extracted automatically when individual sub-package files are extracted from a consolidated package

![](<../.gitbook/assets/Unknown image (506)>)

#### Upgrade process with Install command

this is the new interactive automation script of install mode, however it will reboot the router during the process

If you want only to unpack during business hours and reboot after business hours use the following guide

| no boot system                                              | to remove previous boot                                                                                                                                                                                                                                                                                                                                                                                             |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| boot system bootflash:/packages.conf                        | configure router to boot the provisioning file packages.conf                                                                                                                                                                                                                                                                                                                                                        |
| install add file bootflash:\<ios.bin> \[activate] \[commit] | activate keyword activates all packages during the script process, without needing to activate .pkg separately after installation commit keyword disable the automatic rollback, where device in case of any issues with the new software reverts to the previous IOS image after 10 minutes Note you should put there basic config including license boot XX to perform install mode - idk why, encountered in MSP |
| show install active                                         | to check installed and active versions                                                                                                                                                                                                                                                                                                                                                                              |
| show version running                                        | Show all active platform software versions (packages)                                                                                                                                                                                                                                                                                                                                                               |
| show license \[status \| all]                               |                                                                                                                                                                                                                                                                                                                                                                                                                     |
| more packages.conf                                          | to check what packages are included and the type of the original .bin - npe or universal                                                                                                                                                                                                                                                                                                                            |

#### IOS Upgrade with multiple Route Processors (RP's)

To minimize network downtime and router have redundant hardware route processors we can perform software upgrade of RP one at a time

IOS image has to be copied to both active (bootflash:) and standby (stby-bootflash:) RSP, if you use the install command it copies the ios from active automatically during the process else you can copy manually from active

boot statement can be configured only for main bootflash:

Reload the standby RP first to apply the new IOS #hw-module slot R1 reload (use the show redundancy and show platform to see which is active)

After you see the standby RP in status ‘ok’, you can do a switchover to it redundancy force-switchover - resulting in a short downtime

A few seconds after you do the switchover, you should see the active RP being reloaded with the new software image and the standby RP becoming the active RP.

scp C:\Users\mbrenek\Downloads\asr920igp-15\_6\_46r\_s\_rommon.pkg alef@10.19.55.203:flash:asr920igp-15\_6\_46r\_s\_rommon.pkg

copy scp: stby-bootflash: vrf Mgmt-intf

| request platform software package install \[ node \| rp <> ] file bootflash:\<file.bin> |                                                                                          |
| --------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| request platform software package expand file <.bin>                                    | this unpack the entire .bin ios image in case only certain packages have to be installed |
| request platform software package clean file bootflash:                                 | equivivalent to install remove inactive - remove unused .pkg                             |

### In-Service Software Upgrades (ISSU)

is a HA feature that upgrades the router software with no outage on the control plane and with minimal outage on the forwarding plane

The router should have redundant route processor with SSO and NSF setup, so that one RP can be upgraded by ISSU, while the secondary takes over the control over control and data plane.

It is supported on both XE and XR, but it is not supported when upgrading between major releases from 16.x to 17.x

![](<../.gitbook/assets/Unknown image (507)>)

## IOS-XR

IOS-XR is designed to meet the needs of service providers, running on top of QNX or Linux, working on different coding with different syntax supported on Cisco NCS Series NCS540, NCS560, NCS5000, NCS5500, and NCS6000, CRS Series, ASR 9000 Series and XRv 9000 virtual router platform

The Cisco IOS XR kernel handles core system functions, such as process and management, interrupts, and scheduling

Other system functions become services and run above the kernel. User or client applications also run above the kernel, with the kernel acting as a sort of traffic director.

For example one route processor would primarily handle BGP, and another would handle MPLS operation, while providing process level redundancy for one another

All processes outside the kernel are individually restartable in case of crashes or other issues.

The architecture of Cisco IOS XR Software is a layered, where each layer in the architecture performs a separate set of tasks:

Control This plane distributes routing tasks and management of the Routing Information Base (RIB) in participating route processors; different routing processes can be running on different physical units.

Data This plane maintains the Forwarding Information Base (FIB) changes across the participating nodes, letting the router perform as a single forwarding entity.

Management This plane controls the operation of the router as a single networking element.

Layers communicate with each other through the kernel, using a standard message-passing API.

Cisco IOS XR Software kernel have protected process memory space, where each process has a virtual memory space, so one process cannot corrupt the memory of another

All processes can be check-pointed, so that if a process fails, it can be restarted quickly or the redundant process can take over faster.

The planes (data, control, and management), applications, and processes are separated, so that the failure of one module has no influence on the modules of the other layers

Failure of process within one software plane does not affect other processes or applications within that plane

XR is released in modular packages. A package contains the components that support a specific set of features or functions, such as routing, security, or modular services card (MSC)

Unlike Cisco IOS Software, where feature sets are defined at image build time and remain static while the system is in operation, Cisco IOS XR Software can dynamically load and unload software packages that deliver one or more features. In addition, Cisco IOS XR Software packages are created in versions and can be upgraded or patched as necessary to add features or resolve problems, which allows system enhancement and maintenance to take place without requiring a system restart or disrupting traffic traversing the system

Secure domain routers (SDRs) XR supports logical router partitioning into multiple SDRs

All SDRs in one system share common components like power supply and fan trays while being individually assigned other hardware such as line cards and route processors

Package installation envelopes (PIEs)

are nonbootable files that contain a single package or a set of packages (called a composite package or bundle). Because the files are nonbootable, they are used for adding software package files to a running router

Adding a package to the router does not affect the operation of the router. It only copies the package files to a local storage device on the router, which is known as the boot device (such as the CompactFlash drive). To make the package functional on the router, you must activate it for one or more cards. To upgrade a package, you activate a newer version of the package. When the automatic compatibility checks have been passed, the new version is activated and the old version is deactivated

{% hint style="info" %}
IOS XR doesn’t have **User EXEC** (`>`). The default is **EXEC** (`#`).
{% endhint %}

![](<../.gitbook/assets/Unknown image (508)>)

### Two-Stage Configuration Commit Process

This feature serves two purposes:

Batch command entry – allows entering a sequence of commands at once, which is useful if a connection could drop after the first command, preventing further configuration.

Configuration safeguard – lets you set a time by which the configuration must be committed. If the device becomes unreachable before the commit, unconfirmed changes are automatically discarded.

Stage 1: Making Configuration Changes

When you enter the global configuration mode using the configure command, a new target configuration session is created. The target configuration allows you to enter, review, and verify configuration changes without impacting the running configuration. The target configuration is not a copy of the running configuration; it is an overlay of uncommitted configuration changes in addition to the running configuration.

When you first enter the configuration mode, the target configuration is identical to the running configuration. The running and target configurations are also identical after each successful commit operation.

Target configurations can also be saved to a disk as nonactive configuration files. These saved files can be loaded, further modified, and committed later.

Stage 2: Making Configuration Changes Persistent

When you exit configuration mode, the router will ask if you want to commit the target configuration changes. The changes in the target configuration do not become part of the running configuration until you enter the commit command. When you commit a target configuration, the changes in the target configuration are merged with the running configuration to create a new running configuration.

If you end a configuration session without saving your changes to the running configuration with the commit command, you are prompted to do so

### Configuration Rollback

Configuration changes in Cisco IOS XR Software can be reverted to any previous commit point in a process. This action is called configuration rollback

Each commit generates a record with a commit ID or label, which is a rollback point - can store up to 100 rollback points

Each point is dated and timestamped and lists the user who committed it. You can display the configuration changes that were made at each point

The database is a recovery and convenience feature; it permits you to go back to a previously working configuration

{% hint style="info" %}
Most rollback/commit commands must run in the **config** level. They won’t work in sub-modes.
{% endhint %}

| commit                                   | upon each configuration change, the IOS XR requires to confirm it with "commit" command                                           |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| show configuration commit changes last 1 | to display last committed changes                                                                                                 |
| abort                                    | will cancel all the configuration that is entered in the current configuration session and will exit to EXEC mode                 |
| pwd                                      | to view in which sub-config mode you currently are                                                                                |
| show install active                      | To determine which Cisco IOS XR Software packages are active on a device                                                          |
| (config)# show commit changes diff       | to display current uncomitted changes (don't use "do" before the command)                                                         |
| show configuration failed <>             | to check individual errors that prevent to commit a change                                                                        |
| rollback configuration last 1            | to rollback to the previous configuration before the last commit, we can specify to which configuration state we want to rollback |
| commit replace                           | to erase running configuration                                                                                                    |
| show media location all                  | space utilization                                                                                                                 |
| show memory summary                      | to show the DRAM e.g. physical memory allocation and availability                                                                 |

### 32-bit and 64-bit IOS-XR

Cisco IOS XR Software exists in two versions: 32-bit Cisco IOS XR Software, which is known as classic or cXR, and 64-bit Cisco IOS XR Software, which is known as eXR. The classic IOS XR version primarily runs on older Cisco devices, including the Cisco 12000 Series Routers, Cisco CRS Series Routers, and Cisco ASR 9000 Series Routers

The 64-bit Cisco IOS XR Software runs on newer Cisco devices, including the Cisco NCS Series Router, and on the versatile Cisco ASR 9x00 Series Routers

Model-Driven Programmability to programatically automate and collect the configuration of network devices including [ZTP](onenote:SDN.one#ZTP%20and%20Guest%20Shell\&section-id={2DC2FCEA-3B33-4FFE-8240-49F35B5E0C99}\&page-id={BF2E0122-F446-486A-AF5E-DE6FA82896BE}\&end\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE) - iPXE and app hosting

iPXE is an enhanced version of PXE, it enables network boot for a device that is offline - FTP/HTTP/TFTP boot image download capability, IPv4/6 support, SLAAC,DHCPv6,URI

Model-driven streaming telemetry is a new approach for network monitoring. Data is streamed from network devices continuously by using a push model and provides near real-time access to operational statistics. Applications can subscribe to specific data items they need by using standard-based YANG data models over NETCONF-YANG. Cisco IOS XR Software streaming telemetry allows data to be pushed off the device to an external collector at a much higher frequency and more efficiently. It also allows streaming of data upon changes happening on the devices

In contrast to the traditional use of the pull model in SNMP and syslog, where the client requests data from the network, does not scale when requiring real-time data

SNMP polling can often take 5-10 minutes, and CLIs are unstructured and prone to change, which can often break scripts

![](<../.gitbook/assets/Unknown image (509)>)

### RPM Package Manager (RPM)

RPM packages as the new package format starting with Cisco IOS XR Release 6.0, versus the PIE format used by the 32-bit IOS XR Software

Flexible packaging is a method of breaking down the Cisco IOS XR Software into modules and providing them as RPMs (packages)

Delivering packages as RPMs enables easier and faster system updates.

The base software contains only the required packages, while the optional packages are provided separately as installable RPMs. You can choose and install the services that you want by choosing the required RPMs.

Packages can be placed in a reachable repository and accessed via the network, using FTP/Secure File Transfer Protocol (SFTP)/Secure Copy Protocol (SCP)/TFTP or HTTP, or pre-staged on the device. Third-party packages can be installed with RPM or YUM inside the Shell. Install commands are a wrapper around YUM. Both YUM and install commands provide package dependency verification and resolution.

Yellowdog Updater Modified (YUM) is a free and open-source command-line package-management utility for computers running the Linux operating system and using the RPM

| install update or install upgrade | to install IOS XR packages |
| --------------------------------- | -------------------------- |
| yum install                       | to install linux packages  |

![](<../.gitbook/assets/Unknown image (510)>)

### VM-based Cisco IOS XR vs Container-based Cisco IOS XR

Guest Shell Linux Containers (LXCs) are considered lightweight since they do not need their own kernel as they borrow services from the host operating system and generally require less resources. The platforms that support the LXC architecture do not support SMU and [ISSU](https://onenote/#IOS-XE\&section-id={58B48AF9-0B19-4885-B24D-492AA8D605CF}\&page-id={BA710271-F92A-442A-81A3-38C509B76485}\&object-id={DC5F6F8F-B1EE-0E44-2D0C-21E4D6AC45EC}&3C\&base-path=https://d.docs.live.net/b03dd2dfb2522723/Documents/CCIE/L1.one)

VMs emulate hardware and the host operating systems must have their own kernel, but the VM architecture does provide ISSU support

LXCs and VMs provide isolation between instances, so that Two containers or two VMs cannot interfere with one another.

![](<../.gitbook/assets/Unknown image (511)>)

### Control Plane and Admin Plane VMs

Control plane VM also known as the XR container, is the most significant part of Cisco IOS XR 64-bit Software

It contains all of the routing protocol information, the CLI, the system database, and many other processes

The control plane VM is itself a mini Yocto Linux environment that is composed of a multitude of packages.

Yocto is not a Linux distribution but rather a building environment that helps create a Linux distribution.

Linux OS Interacts directly with the underlying hardware or VM, Provides kernel services for the containers, libraries, tools, and utilities to launch, monitor, and maintain containers

The control plane VM runs two types of packages:

Packages developed by Cisco for core network functions (BGP, MPLS, etc.)

Yocto packages for standard Linux tools and libraries (bash, python, tcpdump, etc.)

Administrator plane VM provides services that were originally provided by the administrator mode of IOS XR Software

In this VM are processes that are responsible for performing system diagnostics, monitoring environment variables, and managing hardware components

It is the first VM to be booted by the host and is responsible for the start of all the other VMs in the system, including the control plane VM

Like the control plane VM, the admin plane VM is a mini Yocto Linux environment.

Third-Party Containers

You can create your own container on Cisco IOS XR Software and host applications within the container. The applications can be developed using any Linux distribution. This solution is well suited for applications that use system libraries that are different from that provided by the Cisco IOS XR root file system, without interrupting or interacting with the IOX-XR software

![](<../.gitbook/assets/Unknown image (512)>)

To access the control plane LXC or VM use SSH port 22

To access the control plane guest operating system

To directly access the control plane guest operating system via SSH port 57722, you must first enable the sshd\_operns service from within the control plane guest operating system

| run  | or ssh on port 57722                                                                                                     |
| ---- | ------------------------------------------------------------------------------------------------------------------------ |
| bash | enters linux shell in cisco IOS XR. In older IOX-XR release you have to use #run and then #ip netns exec global-vrf bash |

To access the admin plane

| admin | Here you can jump into conf t and configure SDRS or control individual card slots (to turn power on and off) |
| ----- | ------------------------------------------------------------------------------------------------------------ |

![](<../.gitbook/assets/Unknown image (513)>)

| uname -a                     | in a bash shell, verify the Linux kernel version |
| ---------------------------- | ------------------------------------------------ |
| cat /proc/cpuinfo or meminfo | to get the processor and RAM information         |

### IOS XR 7

new evolution of IOS XR 64-bit, that got rid of the virtualization of each individual XR process and runs the IOS XR again on top of the linux. This makes the system faster and lighter

The virtualization layer is still there available to support 3rd party containers

{% hint style="info" %}
Don’t confuse **IOS XR 7** (architecture) with **IOS XR 7.x** (release train).
{% endhint %}

![](<../.gitbook/assets/Unknown image (514)>)

### IOS-XR software and RPM install and upgrade

use installation document provided with the .iso file in the software central - it is a separate downloadable document

[https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5xx/system-setup/24xx/b-system-setup-cg-24xx-ncs540/understanding-software-modularity-and-installation.html#id\_121027](https://www.cisco.com/c/en/us/td/docs/iosxr/ncs5xx/system-setup/24xx/b-system-setup-cg-24xx-ncs540/understanding-software-modularity-and-installation.html#id_121027)

### Sonic

open-source Network Operating System (NOS) created by microsoft, that based on Debian Linux distribution that uses Switch Abstraction Interface (SAI) API that ontrols forwarding elements (ASIC, NPU)

This 3rd party NOS is supported on the Cisco 8k fixed series routers meant for DC (with the X-X-O PID) for the DC. The protocol support is limited so it is meant for simple DC routing

Cisco provides Base Support Package (BSP) that controls board, fans, drivers, FPD, LED, etc.

![](<../.gitbook/assets/Unknown image (515)>)

### Licensing

is always related to Smart Licensing communication with Cisco Licensing Server Directly or Smart Satellite Server or License reservation (PAK like, no communication, military, isolated systems)

Traditional licenses are purchased for individual feature (L3VPN, L2VPN, BGP, …)

Flexible Consumption Model (FCM = Vortex) software licence is purchased for either essential or advantage features (L2/L3/peering/core+agg/DC) set per 10GE or per 100GE port capacity, or it can be purchased for the entire chassis for specific platforms

### Flexible Consumption Model (FCM)

is licensing model for IOS-XR based platforms, allowing customer to purchase a platform and pay only for actively used capacity (per port and it's active capacity) and set of features

Customers can flexibly pay for additional port capacity to unlock other ports as their network grows (pay-as-you-grow)

The licenses are tracked on My Cisco Entitlements (MCE) platform that provides a consolidated view of all assets and entitlements, including services, subscriptions, licenses, and devices in the Smart Account

It is comprised of three parts

1. Hardware device
2. Right To Use (RTU) perpetual software component involve two software suites, Essentials Software and Advantage Software

Essential Software licenses include the IOS XR comprehensive suite of routing and management services

Advantage Software licenses are an extension of Essential license and include all features of Essentials Software licenses with additional advanced routing and management services.

The RTU is purchased for individual port capacity of a device - If customer requires Essentials license for a 100g port it is purchased as one Essential Right-to-Use 100G

Licenses remain available and valid, even when the hardware was destroyed or sold

3. Software Innovation Access (SIA) is additional subscription that provides customers with new software releases and feature upgrades and permits to share the perpetual RTU software licenses across the entire FCM network for a given Smart Account from a common license pool. SIA subscription for 3 year is mandatory when purchasing a device and RTU's

{% hint style="info" %}
If **SIA** expires, software upgrades and license transfers are blocked. Renewal may include penalties.
{% endhint %}

### FCM 2.0

Applies only to devices of new releases of 8k series

New 3rd license level - Network Premiere

In FCM 1.0 each platform 8k,ASR and NCS have their own set of RTUs

In FCM 2.0 licenses are shared between devices regardless of the platform or role

SIA is purchased per device, not per port in FCM 2.0

One SIA per fixed centralized system and One SIA per line card in a distributed system

![](<../.gitbook/assets/Unknown image (516)>)

| <p>Router(config)# license smart flexible-consumption enable<br>Router(config)# commit</p> | To enable Flexible Consumption model licensing on routers running Cisco IOS XR |
| ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| Device# show running-config license smart flexible-consumption enable                      | To verify the Flexible Consumption Model configuration:                        |
| show license platform summary                                                              | To verify the device compliance status                                         |

#### Systems supporting IOS XR FCM

8000 Series Routers

NCS 5700 Series Routers

NCS 5500 Series Routers

ASR 9000 Series Routers

NCS 560 Series Routers

NCS 540 Series Routers

{% hint style="info" %}
Part numbers (PNs) with `SYS` are delivered with **FCM**.
{% endhint %}

### License Feature support

[https://salesresources.cisco.com/app?ContentId=8725f7fc-c940-4be0-87d3-687fdd2e580e#/doccenter/1d1918e9-b5b0-4428-b8fc-87e02ad44156/doc/%252Fddc0cee617-d96b-085b-ab11-08fdbc97cfda%252FdfM2Y2MGY5YjEtOWQ2OS00MGNkLTg4ZTAtNmE2ODMzOGMzNWZl%252CPT0%253D%252CUG9ydGZvbGlv%252FdfNzI3MmU0NWQtZDk2ZC00NWQwLTk0OWQtOWE3MjI2YmQwMDZk%252CPT0%253D%252CRGF0YSBTaGVldHM%253D%252Flf4c805e6f-83a3-42b4-adbd-24bd85562dba//?mode=view\&searchId=f44abda3-85c1-4575-bba6-b4a4923a6296](https://salesresources.cisco.com/app?ContentId=8725f7fc-c940-4be0-87d3-687fdd2e580e#/doccenter/1d1918e9-b5b0-4428-b8fc-87e02ad44156/doc/%252Fddc0cee617-d96b-085b-ab11-08fdbc97cfda%252FdfM2Y2MGY5YjEtOWQ2OS00MGNkLTg4ZTAtNmE2ODMzOGMzNWZl%252CPT0%253D%252CUG9ydGZvbGlv%252FdfNzI3MmU0NWQtZDk2ZC00NWQwLTk0OWQtOWE3MjI2YmQwMDZk%252CPT0%253D%252CRGF0YSBTaGVldHM%253D%252Flf4c805e6f-83a3-42b4-adbd-24bd85562dba//?mode=view\&searchId=f44abda3-85c1-4575-bba6-b4a4923a6296)

Release Naming Conventions for Cisco IOS XR Software

![](<../.gitbook/assets/Unknown image (517)>)

Encryption support The image includes support for strong cryptography features, including payload encryption, as indicated by the k9designation. If an image does not include support for these features, the k9 designation is omitted from the image

If an image does not include support for strong encryption of payloads, the npe (no payload encryption) designation is prepended to the software release value in the name

### Software Maintenance Updates (SMUs)

An SMU is a software patch that is installed on the Cisco IOS XR device. The concept of an SMU applies to all Cisco IOS XR hardware platforms.

A Cisco IOS XR SMU is an emergency point fix, which is positioned for expedited delivery and which addresses a network that is down or a problem that affects revenue.

When the system runs into a software deficiency (bug), Cisco can provide a fix for the particular problem in the base current Cisco IOS XR release. This is a substantial difference from the classic Cisco IOS software, which has no capability to apply a single fix in the base current release.

An SMU is built on a per release and per component basis and is specific to the platform. This means that an SMU for a CRS router cannot be installed on an ASR 9000 router. An SMU built for Cisco IOS XR software Release 4.2.1 cannot be applied to a system with Cisco IOS XR software Release 4.2.3. An SMU built for a P image cannot be used on a system built for a PX image.

SMUs are provided for urgent, “showstopper” issues only. The fix provided by the SMU is then integrated into the subsequent Cisco IOS XR software maintenance release. Cisco strongly encourages you to upgrade to the subsequent maintenance release.

SMUs are Package Installation Envelope (PIE) files that are similar in functionality and installation to the feature PIEs for manageability (MGBL), Multiprotocol Label Switching (MPLS), and Multicast

Cisco Software Manager (CSM) provides Cisco IOS XR SMU recommendations to users and reduces the effort that it takes for you to manually search, identify, and analyze SMUs that are needed for a device. The CSM can connect to multiple devices and provide SMU management for multiple Cisco IOS XR platforms and releases.

Full Guide

[https://www.cisco.com/c/en/us/support/docs/ios-nx-os-software/ios-xr-software/116332-maintain-ios-xr-smu-00.html](https://www.cisco.com/c/en/us/support/docs/ios-nx-os-software/ios-xr-software/116332-maintain-ios-xr-smu-00.html)

## IOS XE and XR CLI Editors

### nano editor: - default for IOS-XE

Ctrl-S: Save current file

Ctrl-O: Offer to write file ("Save as")

Ctrl-X: Close buffer, exit from nano

Ctrl-H: Delete character before cursor

Ctrl-D: Delete character under cursor

Ctrl-Shift-Del: Delete word to the left

Ctrl-Del: Delete word to the right

Alt-Del: Delete current line

### VIM editor:

Left, down, up, and right arrows: Move cursor left, down, up, right.

h, j, k, l: Move cursor left, down, up, right.

i: Start editing at the cursor position.

a: Start editing after the cursor position.

ESC: Stop editing (return to command mode).

x: Delete the character at cursor position.

dd: Delete a line.

u: Undo a single action.

CTRL+C then :qa! and enter to abort any changes and exit vim

ESC followed by :w: Save changes.

ESC followed by :q: Exit and commit saved changes.

After exiting the editor, you will be asked to save and commit the changes.

### Emacs editor:

Ctrl-F: Move cursor forward (right).

Ctrl-B: Move cursor backward (left).

Ctrl-N: Move cursor to next line (down).

Ctrl-P: Move cursor to previous line (up).

Ctrl-E: Move to the end of the line.

Ctrl-A: Move to the start of the line.

Backspace: Delete character to the left of the cursor.

Ctrl-D: Delete character to the right.

Ctrl-X followed by Ctrl-S: Save changes.

Ctrl-X followed by Ctrl-C: Exit and commit saved changes.

### vi editor

| vi /etc/php-fpm.d/zabbix.conf | to edit a file                          |
| ----------------------------- | --------------------------------------- |
| insert button                 | to start edit                           |
| esc button then :wq           | to exit editing and close the vi editor |
| systemctl restart php-fpm     | apply the change                        |

## System monitoring

is essential for maintaining optimal network performance. It allows network engineers to gather real-time data, identify issues, and ensure that the network operates efficiently. This process involves creating a performance baseline, which serves as a reference point for troubleshooting and evaluating network health.

Enterprises need proactive network management to detect and address anomalies. A central Network Management System (NMS) is critical for this, using protocols such as Syslog and Simple Network Management Protocol (SNMP). These protocols enable network devices to report events to a central server, creating a holistic view of network activities. To ensure accurate event correlation, timestamps and synchronization across all network devices is crucial.

### System logging

Syslog is a protocol that allows a device to send event notification messages across IP networks to event message collectors. By default, a network device sends the output from system messages and debug-privileged EXEC commands to a logging process. The logging process controls the distribution of logging messages to various destinations, such as the logging buffer, console line, terminal lines, or a syslog server, depending on your configuration. Logging services enable you to gather logging information for monitoring and troubleshooting, to select the type of logging information that is captured, and to specify the destinations of captured syslog messages.

During operation, network devices generate messages about different events. These messages are sent to an operating system process. This process is responsible for sending these messages to various destinations, as directed by the device configuration. Logging messages are also sent to the console by default. Even if the global logging process is disabled, logging messages are

nevertheless sent to the console. You can decide about the severity level of the logged messages and their destination.

You can set the severity level of the messages to control the type of messages that the consoles display and where the messages are displayed. You can configure the device to add time stamps to log messages, and to set the syslog source address, to enhance real-time debugging and management.

You can access logged system messages by using the device CLI or by saving them to a syslog server. The switch or router software saves syslog messages in an internal buffer.

You can remotely monitor system messages by viewing the logs on a syslog server or by accessing the device through Telnet, Secure Shell (SSH), or through the console port.

Administrators usually see syslog messages on the router console.

Message Discriminator is a syslog processor, it is associated with a syslog session and binds that session to a transport connection.

Can be used to filter and sort syslog messages based on type of message, the source of the message, or the severity of the message

The syslog receiver is commonly called syslogd, syslog daemon, or syslog server. Syslog messages can be sent via UDP (port 514) or TCP (port 6514).

UDP/514 syslog

TCP/6514 syslog over TLS

UDP/6514 syslog over DTLS

![](<../.gitbook/assets/Unknown image (518)>)

![](<../.gitbook/assets/Unknown image (519)>)

PRI (priority)

Priority is an 8-bit number and its value represents the facility and severity of the message. The three least significant bits represent the severity of the message (with 3 bits, you can represent eight different severity levels), and the upper 5 bits represent the facility of the message.

You can use the facility and severity values to apply certain filters on the events in the syslog daemon.

The priority and facility values are created by the syslog clients (applications or hardware) on which the event is generated. The syslog server is just an aggregator of the messages.

Facility

Syslog messages are broadly categorized based on the sources that generate them. These sources can be the operating system, process, or an application. The source is defined in a syslog message by a numeric value.

These integer values are called facilities. The local use facilities are not reserved; the processes and applications that do not have preassigned facility values can choose any of the eight local use facilities. As such, Cisco devices use one of the local use facilities for sending syslog messages.

By default, Cisco IOS Software-based devices use facility local7. Most Cisco devices provide options to change the facility level from their default value.

| Numerical Code | Facility                                 |
| -------------- | ---------------------------------------- |
| 0              | Kernel messages                          |
| 1              | User-level messages                      |
| 2              | Mail system                              |
| 3              | System daemons                           |
| 4              | Security/authorization messages          |
| 5              | Messages generated internally by syslogd |
| 6              | Line printer subsystem                   |
| 7              | Network news subsystem                   |
| 8              | UUCP subsystem                           |
| 9              | Clock daemon                             |
| 10             | Security/authorization messages          |
| 11             | FTP daemon                               |
| 12             | NTP subsystem                            |
| 13             | Log audit                                |
| 14             | Log alert                                |
| 15             | Clock daemon (note 2)                    |
| 16             | Local use 0 (local0)                     |
| 17             | Local use 1 (local1)                     |
| 18             | Local use 2 (local2)                     |
| 19             | Local use 3 (local3)                     |
| 20             | Local use 4 (local4)                     |
| 21             | Local use 5 (local5)                     |
| 22             | Local use 6 (local6)                     |
| 23             | Local use 7 (local7)                     |

### Severity Levels

The log source or facility (a router or mail server, for example) that generates the syslog message specifies the severity of the message using single-digit integers 0-7.

The severity levels are often used to filter out messages which are less important, to make the amount of messages more manageable. Severity levels define how severe the issue reported is, which is reflected in the severity definitions in the table.

| 0 | emergency (System unusable)                    |
| - | ---------------------------------------------- |
| 1 | alert (Immediate action needed)                |
| 2 | critical events                                |
| 3 | error events                                   |
| 4 | warning events                                 |
| 5 | notification events                            |
| 6 | informational events                           |
| 7 | debug messages (Appears during debugging only) |

Time Stamp

The time stamp field is used to indicate the local time, in MMM DD HH:MM:SS format, of the sending device when the message is generated.

For the time stamp information to be accurate, it is good administrative practice to configure all the devices to use the Network Time Protocol (NTP). In recent years, however, the time stamp and hostname in the header field have become less relevant in the syslog packet itself because the syslog server will time stamp each received message with the server time when the message is received, as well as the IP address (or hostname) of the sender, taken from the source IP address of the packet.

A correct sequence of events is vital for troubleshooting in order to accurately determine the cause of an issue. Often an informational message can indicate the cause of a critical message. The events can follow each other by milliseconds.

Hostname

The hostname field consists of the host name (as configured on the host) or the IP address. In devices such as routers or firewalls, which have multiple interfaces, syslog uses the IP address of the interface from which the message is transmitted.

Many people can get confused by "host name" and "hostname." The latter is typically associated with a Domain Name System (DNS) lookup. If the device includes its "host name" in the actual message, it may be (and often is) different than the actual DNS hostname of the device. A properly configured DNS system should include reverse lookups to help facilitate proper sourcing for incoming messages.

MSG (message text)

The message is the text of the syslog message, with additional information about the process that generated the message.

The syslog packet size is limited to 1024 bytes.

seq no:timestamp: %facility-severity-MNEMONIC:description

Example

\*Jan 18 03:02:42: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to down

Jan 18 03:02:42 – the timestamp

%LINEPROTO – the source that generated the message. It can be a hardware device (e,g. a router), a protocol, or a module of the system software

5 – the severity level

UPDOWN – the unique mnemonic for the message

Line protocol on Interface GigabitEthernet0/0, changed state to down – the description of the event

This table explains some of the facility codes that you may see in a Cisco IOS Software syslog message:

| Code      | Facility                        |
| --------- | ------------------------------- |
| LINEPROTO | Line Protocol                   |
| LINK      | Data Link                       |
| OSPF      | Open Shortest Path First (OSPF) |
| CDP       | Cisco Discovery Protocol        |
| SYS       | Operating System                |

While logging to the console is enabled by default, it is very expensive in terms of CPU resources on a Cisco IOS device. The reason is that the console is a character-by-character serial device. Each character that is displayed to the console requires a CPU interrupt. As such, it is common to disable logging to the console when logging to a centralized syslog server is configured.

Configuration

[https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/syst-mgmt/b-system-management/m\_log-local-nonvol-0.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/syst-mgmt/b-system-management/m_log-local-nonvol-0.html)

| #terminal monitor                                                                                                                                                                                                                                                    | Cisco IOS doesnt print log messages to a terminal session over IP by default. This command enables it. use terminal no monitor to disable                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| (config)# logging userinfo                                                                                                                                                                                                                                           | To enable the logging of user information                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| \[no] logging console                                                                                                                                                                                                                                                | Enable/disable logs and debugs to be printed to the console CLI; it is enabled by default                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| \[no] logging monitor \<severity\_lvl>                                                                                                                                                                                                                               | Enable/disable logging for vty line If severity level 0 is configured, it means that only emergency-level messages will be displayed. For example, if severity level 4 is configured, all messages with severity levels up to 4 will be displayed (Emergency, Alert, Critical, Error, and Warning).                                                                                                                                                                                                                                                                                                                                                                                                    |
| (config)# logging buffered                                                                                                                                                                                                                                           | configures the logging buffer max size with additional options. Default buffer size for log messages in routers is typically 4096 bytes The optional severity-level argument limits the logging of messages to the buffer to those no less severe than the specified level. debugging keyword will also store debugging logs into the logging buffer                                                                                                                                                                                                                                                                                                                                                   |
| logging \[alarm \| monitor \| trap]                                                                                                                                                                                                                                  | when you specify certain logging severity it will log lower severity levels (e.g when logging for debugging e.g 7, it will log all other, when only alert e.g 2 it will log alert and emergency) alarm logs alarm messages monitor logs monitor messages trap limits the syslog severity sent to external server best practise logging alarm informational logging trap debugging                                                                                                                                                                                                                                                                                                                      |
| (config)# logging host                                                                                                                                                                                                                                               | specifies the destination server/host IP, where logs are sent for monitoring                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| logging history                                                                                                                                                                                                                                                      | Changes the default level of syslog messages stored in the history file and sent to the SNMP server.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| logging history size <>                                                                                                                                                                                                                                              | Specifies the number of syslog messages that can be stored in the history table. The default is to store one message. The range is 0 to 500 messages.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| logging facility                                                                                                                                                                                                                                                     | The logging facility command basically tells the syslog server where to put the log message. You configure the syslog server with something like: local7.debug /var/adm/local7.log Now, when you use the "logging facility local7" on your device, all messages with severity "debug" or greater should be saved in /var/adm/local7.log.                                                                                                                                                                                                                                                                                                                                                               |
| (config)# logging persistent \[url harddisk: / directory ] \[size filesystem-size ] \[filesize logging-file-size ] Example best practise logging persistent size 6400000 filesize 512000                                                                             | Writes logging messages from the memory buffer to the specified directory on the device’s bootflash or a harddisk. Values are specified in bytes Before logging messages are written to a file on the bootflash or harddisk, the Cisco software checks to see if there is sufficient disk space. If not, the oldest file of logging messages (by timestamp) is deleted, and the current file is saved. The filename format of log files is log\_MM:DD:YYYY::hh:mm:ss. For example: log\_11:26:2012::01:01:41. The defaults for this command are as follows: url: bootflash:/syslog Filesystem-size: 10% of total disk space. Logging-file-size: 262144 Example: /bootflash/syslog/log\_20240901-104332 |
| (config-line)# logging synchronous level \<all \| 0-7>                                                                                                                                                                                                               | By default, messages are printed at any time, possibly disrupting the user's current command. This command disables this default behaviour and waits until the user's current command and its output is completed before printing any log messages                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Service timestamps                                                                                                                                                                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| (config)# service timestamps \[log \| debug] \[uptime \| datetime] \[msec \| year \| localtime \| show-timezone] Best practise service timestamps debug datetime msec localtime show-timezone year service timestamps log datetime msec localtime show-timezone year | configure the system to time-stamp debugging or logging messages to correlate events to the exact date and time, when it occured uptime (Optional) Time stamp with time since the system was rebooted. This is the default in UTC - use datetime instead datetime Time stamp with the date and time msec Include milliseconds in the date and time stamp. localtime Time stamp relative to the local time zone. show-timezone Include the time zone name in the time stamp year Include year in timestamp                                                                                                                                                                                              |
| (config)# service sequence-numbers                                                                                                                                                                                                                                   | Additionally sequence numbers can be configured (disabled by default) This is especially useful when several messages occur at the same time to see the exact sequence in which the messages occurred                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| show logging                                                                                                                                                                                                                                                         | If the buffer size gets exceeded, the oldest messages will be removed                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| show buffers                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |

Syslog Server

| tail -f | displays new file entries as they arrive to the console, for live tshoot |
| ------- | ------------------------------------------------------------------------ |

![](<../.gitbook/assets/Unknown image (520)>)

| origin-id            | applies the router hostname to logs                                                  |
| -------------------- | ------------------------------------------------------------------------------------ |
| sequence-num-session | applies timestamp to syslog, in case syslog server is unable to apply it by himself; |

![](<../.gitbook/assets/Unknown image (521)>)

### Debug

is a Cisco IOS command that provides real-time CLI logging output of operation performed by specific protocol or feature running on a device

Debugging packets is dangerous. Packet debugging triggers process switching of the multicast packets, which is CPU intensive. Also, packet debugging can produce huge output which can hang the router completely due to slow output to the console port. Before you debug packets, special care must be taken in order to disable logging output to the console, and enable logging to the memory buffer. In order to achieve this, configure no logging console and logging buffered debugging

The results of the debug can be seen with the show logging command.

If connecting via the VTY lines (ssh or telnet) you will need to use the #terminal monitor command to be able to view them

Conditional Debugging is a technique employed to narrow down and return only relevant debug information to the console or syslog server by implementing ACL or debug conditions

| debug condition interface <>    | example: debug condition ip address 192.168.1.1                         |
| ------------------------------- | ----------------------------------------------------------------------- |
| undebug condition 1 undebug all | show debug condition Always remember to disable debugs when you're done |

![](<../.gitbook/assets/Unknown image (522)>)

### Configuring Configuration Change Notification and Logging (Archive)

The Configuration Change Notification and Logging (Config Log Archive) feature allows the tracking of configuration changes entered on a per-session and per-user basis by implementing an archive function. This archive saves configuration logs that track each configuration command that is applied, who applied the command, the parser return code (PRC) for the command, and the time the command was applied. This feature also adds a notification mechanism that sends asynchronous notifications to registered applications whenever the configuration log changes.

The Cisco IOS configuration archive is intended to provide a mechanism to store, organize, and manage an archive of Cisco IOS configuration files to enhance the configuration rollback capability provided by the configurereplace command. Before this feature was introduced, you could save copies of the running configuration using the copyrunning-config destination-url command, storing the replacement file either locally or remotely. However, this method lacked any automated file management.

Configuration

[https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/syst-mgmt/b-system-management/m\_cm-config-logger-0.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/syst-mgmt/b-system-management/m_cm-config-logger-0.html)

| (config)# archive                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| (config-archive)# log config                                               | Enters configuration change logger configuration mode.                                                                                                                                                                                                                                                                                                                                                                                                                               |
| (config-archive)# logging enable                                           | Enables the logging of configuration changes - disabled by default                                                                                                                                                                                                                                                                                                                                                                                                                   |
| (config-archive)# logging size <>                                          | Specifies the maximum number of entries retained in the configuration log. Valid values for the entries argument range from 1 to 1000. The default value is 100 entries. When the configuration log is full, the oldest entry is deleted every time a new entry is added. Note If a new log size is specified that is smaller than the current log size, the oldest log entries are immediately purged until the new log size is satisfied, regardless of the age of the log entries |
| (config-archive)#hidekeys                                                  | Suppresses the display of password information in configuration log files                                                                                                                                                                                                                                                                                                                                                                                                            |
| (config-archive)#notify syslog                                             | Enables the sending of notifications of configuration changes to a remote syslog.                                                                                                                                                                                                                                                                                                                                                                                                    |
| show archive log config \[end-number]                                      | Use this command to display configuration log entries by record numbers. If you specify a record number for the optional end-number argument, all log entries with record numbers in the range from the value entered for the number argument through the end-number argument are displayed. use keyword all to display all logged lines                                                                                                                                             |
| show archive config differences system:running-config nvram:startup-config | compares two configuration files. In this example it compares the running and startup config. You can use the abbreviated version sh arch conf diff keyword incremental-diffs compares the specified file with the running config and generates a list of the configuration lines that do not appear in the running configuration file                                                                                                                                               |
| show run diff                                                              | equivalent to #show arch conf diff on a switch                                                                                                                                                                                                                                                                                                                                                                                                                                       |

Entries from the configuration log can be cleared in one of two ways. The size of the configuration log can be reduced by using the logging size command, or the configuration log can be disabled and then reenabled with the logging enable command.

| (config)# logging purge-log buffer days 90 time 15:45 | This feature allows you to delete the entries from the logging buffer automatically after a configurable time. The following example shows how to enable automatic log deletion to retain only 90 days old data. The deletion of logs will take place at the specified time, which is 15:45. |
| ----------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Configuration Replace and Configuration Rollback

Allows you to revert to a previous configuration state, effectively rolling back configuration changes.

Allows you to replace the current running configuration file with the startup configuration file. After you replace the file, you must reload the device for your configuration changes to take effect.

Allows you to revert to any saved Cisco IOS configuration state.

Simplifies configuration changes by allowing you to apply a complete configuration file to the router, where only the commands that need to be added or removed are affected.

When using the configure replace command as an alternative to the copy source-url running-config command, increases efficiency and prevents risk of service outages by not reapplying existing commands in the current running configuration. After you replace the file, you must reload the device for your configuration changes to take effect.

Configuration Rollback Confirmed Change feature enables an added criterion of a confirmation to configuration changes. This functionality enables a rollback to occur if a confirmation of the requested changes is not received in a configured time frame. Command failures can also be configured to trigger a configuration rollback.

Ref [https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/syst-mgmt/b-system-management/m\_cm-config-rollback-0.html](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/syst-mgmt/b-system-management/m_cm-config-rollback-0.html)

| (config)# archive                           | Enters archive configuration mode.                                                                                                                                                                                                                                                                                                                              |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Device(config-archive)# path flash:myconfig | Specifies the location and filename prefix for the files in the Cisco IOS configuration archive. Note If a directory is specified in the path instead of file, the directory name must be followed by a forward slash as follows: path flash:/directory/ The forward slash is not necessary after a filename; it is only necessary when specifying a directory. |
| (config-archive)# maximum <>                | (Optional) Sets the maximum number of archive files of the running configuration to be saved in the Cisco IOS configuration archive. Default is 10                                                                                                                                                                                                              |
| Device(config-archive)# time-period <>      | (Optional) Sets the time increment for automatically saving an archive file of the current running configuration in the Cisco IOS configuration archive. The minutes argument specifies how often, in minutes, to automatically save an archive file of the current running configuration in the Cisco IOS configuration archive.                               |
| Device# archive config                      | Saves the current running configuration file to the configuration archive. Before using the archiveconfig command, you must configure the path command to specify the location and filename prefix for the files in the Cisco IOS configuration archive.                                                                                                        |

Performing a Configuration Replace or Configuration Rollback Operation

| configure replace \[nolock ] \[list ] \[force ] \[ignorecase ] \[reverttrigger \[ error] \[ timer minutes ] \|time minutes ] | Replaces the current running configuration file with a saved Cisco IOS configuration file. After you replace the file, you must reload the device for your configuration changes to take effect. The target -url argument is a URL (accessible by the Cisco IOS file system) of the saved Cisco IOS configuration file that is to replace the current running configuration, such as the configuration file created using the archiveconfig command. The list keyword displays a list of the command lines applied by the Cisco IOS software parser during each pass of the configuration replace operation. The total number of passes performed is also displayed. The force keyword replaces the current running configuration file with the specified saved Cisco IOS configuration file without prompting you for confirmation. The time minutes keyword and argument specify the time (in minutes) within which you must enter the configureconfirm command to confirm replacement of the current running configuration file. If the configureconfirm command is not entered within the specified time limit, the configuration replace operation is automatically reversed (in other words, the current running configuration file is restored to the configuration state that existed prior to entering the configurereplace command). The nolock keyword disables the locking of the running configuration file that prevents other users from changing the running configuration during a configuration replace operation. The reverttrigger keywords set the following triggers for reverting to the original configuration: error --Reverts to the original configuration upon error. timer minutes --Reverts to the original configuration if specified time elapses. The ignorecase keyword allows the configuration to ignore the case of the confirmation command. |
| ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| configure revert {now \|timer{ minutes \|idle minutes \}}                                                                    | (Optional) To cancel the timed rollback and trigger the rollback immediately, or to reset parameters for the timed rollback, use the configurerevert command in privileged EXEC mode. now --Triggers the rollback immediately. timer --Resets the configuration revert timer. Use the minutes argument with the timer keyword to specify a new revert time in minutes. Use the idle keyword along with a time in minutes to set the maximum allowable time period of no activity before reverting to the saved configuration.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| configure confirm                                                                                                            | (Optional) Confirms replacement of the current running configuration file with a saved Cisco IOS configuration file. After you replace the file, you must reload the device for your configuration changes to take effect.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| configure replace nvram:startup-config                                                                                       | to revert to the Cisco IOS startup configuration file. force keyword to override the interactive user prompt                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| show archive                                                                                                                 | Use this command to display information about the files saved in the Cisco IOS configuration archive                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| show configuration lock                                                                                                      | The running configuration lock is automatically cleared at the end of the configuration replace operation                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |

Configuration Logger Persistency feature implements a quick-save mechanism so that the time to save changes from the startup configuration is proportional to the size of the incremental changes (with respect to the startup configuration) that need to be saved. The persisted commands from the Cisco IOS configuration logger will be used as an extension to the startup configuration. The saved commands, which are used as an extension to the startup configuration, provide a quick-save ability. Rather than saving the entire startup-config file, Cisco IOS software saves just the commands entered since the last startup-config file was generated.

| Router(config)# archive                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Router(config-archive)# log config                                | Enters archive configuration-log configuration mode.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Router(config-archive-log-cfg)# logging persistent auto           | Example: Router(config-archive-log-cfg)# logging persistent auto Enables the Configuration Logger Persistency feature: The auto keyword specifies that each configuration command will be saved automatically to the Cisco IOS secure file system. The manual keyword specifies that you can save the configuration commands to the Cisco IOS secure file system on-demand. To do this, you must use the archivelogconfigpersistentsave command. Note To enable the loggingpersistentauto command, you must have disk0: configured and an external flash card inserted on the router. |
| Router(config-archive-log-cfg)# logging persistent reload         | Sequentially applies the configuration commands saved in the configuration logger database (since the last writememory command) to the running-config file after a reload.                                                                                                                                                                                                                                                                                                                                                                                                            |
| Router(config-archive-log-cfg)# logging persistent size threshold | Specifies the disk space size for writing log messages in the configuration logger database; triggers an alert on the console or syslog server when the log size exceeds the threshold (specified in percentage).                                                                                                                                                                                                                                                                                                                                                                     |
| Router(config-archive-log-cfg)# logging size 10                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| show archive log config persistent                                | This command displays the persisted commands in the configuration log. The commands appear in a configlet format. The following is sample output from this command:                                                                                                                                                                                                                                                                                                                                                                                                                   |
| clear archive log config persistent                               | This command clears the configuration logging persistent database entries. Only the entries in the configuration logging database file are deleted. The file itself is not deleted because it will be used to log new entries. After this command is entered, a message is returned to indicate that the archive log is cleared.                                                                                                                                                                                                                                                      |
| archive log config persistent save                                | This command saves the configuration log to the Cisco IOS secure file system. For this command to work, the archivelogconfigpersistentsave command must be configured.                                                                                                                                                                                                                                                                                                                                                                                                                |
| debug archive log config persistent                               | This command turns on the debugging function. A message is returned to indicate that debugging is turned on.                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |

### IP Event Dampening

is a Cisco feature that is used to manage the impact of network events (such as flapping links or network loops) on network performance

When network events occur, the devices generate a large number of syslog messages and SNMP traps, which can overwhelm the network management systems and consume bandwidth

IP Event Dampening suppresses log messages and traps by temporarily disabling the affected interfaces or routes, and then gradually reintroducing them back into the network.

The IP Event Dampening feature provides a configurable set of parameters that allows network administrators to control the duration, threshold, and suppression levels of events

### To set up text logging of CLI session output into txt file in your PC

For Putty

change settings -- logging

select printable output only

on the file name box choice the filename you want to give to the file

For Securecrt

file ->log session open a file dialog box where you can choice name and dir

To save every established session to folder

C:\Users\mbrenek\OneDrive - ALEF\Documents\Logs%S\_%H\_%Y%M%D\_%h%m%s.txt

\*\*\*\* connect %H\_%Y%M%D\_%h%m%s \*\*\*\*

![](<../.gitbook/assets/Unknown image (523)>)

### Simple Network Management Protocol (SNMP)

is an application-layer protocol that provides a message format for communication between managers and agents. SNMP provides a standardized framework and a common language that is used for monitoring and managing devices in a network

Routers and other network devices keep statistics about the information of their processes and interfaces locally. SNMP on a device runs a special process that is called an agent. This agent can be queried, using SNMP, to obtain the values of statistics or parameters. By periodically querying, or "polling," the SNMP agent on a device, statistics can be gathered and collected over time by a network management system (NMS). NMS can be software that runs on the management computer and gathers SNMP data by querying managed devices

This data can then be processed and analyzed in various ways. Averages, minimums, or maximums can be calculated, the data can be graphed, or thresholds can be set to trigger a notification process when they are exceeded. The NMS polls devices periodically to obtain the values of the MIB objects that it is set up to collect.

Remember, the SNMP manager periodically polls SNMP agents, which means that you will always receive the information on the next agent poll. Depending on the interval, this could mean a 10-minute delay.

The SNMP framework consists of these elements:

SNMP manager: A system that controls and monitors the activities of managed devices/hosts using SNMP. The most common managing system is an NMS, such as Cisco Evolved Programmable Network (EPN) Manager.

SNMP agent: A software component within a managed device/host that maintains the data for the device and reports this data, as needed, to managing systems. The agent resides on the network device (router, access server, or switch).

Management Information Base (MIB): All versions of SNMP utilize the concept of the MIB. The MIB organizes configuration and status data into a tree structure

The SNMP agent contains MIB object variables and the SNMP manager can request or change the values. A manager can get a value from an agent or store a value in the agent. The agent gathers data from the MIB, the repository for information about device parameters and network data. The agent can also respond to manager requests to get or set data. An MIB is a collection of information that is organized hierarchically. The information is then accessed using a protocol such as SNMP. Object IDs (OIDs) uniquely identify managed objects in an MIB hierarchy. OIDs can be depicted as a tree, the levels of which are assigned by different organizations. Top-level MIB OIDs belong to different standard organizations. Vendors define private branches, including managed objects for their own products.

For example, system identification data is located under 1.3.6.1.2.1.1. Some examples of system data include the system name (OID 1.3.6.1.2.1.1.5), system location (OID 1.3.6.1.2.1.1.6), and system uptime (OID 1.3.6.1.2.1.1.3).

OIDs belonging to Cisco are numbered as follows: .iso (1).org (3).dod (6).internet (1).private (4).enterprises (1).cisco (9).

Interface Index (ifIndex) is a unique identifier assigned to each interface on a network device, enabling SNMP to track and analyze interface-specific data.

For most software, the ifIndex is the name of the interface.

SNMPv3 supports the SNMP Engine ID’ Identifier, which uniquely identifies each SNMP device entity

![](<../.gitbook/assets/Unknown image (524)>)

The SNMP framework supports the following operations:

SNMP Get: get, get-next, or get-bulk operations are used to retrieve MIB object variables from SNMP agent.

SNMP Set: set operation is used to modify MIB object variables.

SNMP Notifications: Notifications can mean improper user authentication, restarts, link status (up or down), MAC address tracking, closing of a TCP connection, loss of connection to a neighbor, or other significant events.

Traps are unsolicited messages alerting the SNMP manager to a condition on the network.

Informs are traps that include a request for confirmation of receipt from the SNMP manager. When router sends informs, the SNMP server has to acknowledge the Informs. If the server does not send ACK, then router will send inform again.

![](<../.gitbook/assets/Unknown image (525)>)

![](<../.gitbook/assets/Unknown image (526)>)

SNMP communication between the agent and manager on UDP port 161

SNMP traps sent from the agent to the manager on UDP port 162

SNMP can be used not only for monitoring but also for configuring network devices through writable MIB objects. SNMP operates using MIBs (Management Information Bases), which define various device parameters as Object Identifiers (OIDs). Some of these OIDs are read-only, used for monitoring, while others are read-write, allowing configuration changes. The SNMP SET command enables modifications to writable MIB objects, allowing actions such as enabling or disabling interfaces, updating IP addresses, changing VLAN settings, or even modifying security configurations.

SNMP is implemented in three versions: Simple Network Management Protocol version 1 (SNMPv1), Simple Network Management Protocol version 2c (SNMPv2c), and Simple Network Management Protocol version 3 (SNMPv3). SNMPv2c introduced a bulk retrieval mechanism and a more detailed error message reporting to management stations. The bulk retrieval mechanism supports the retrieval of tables and large quantities of information, minimizing the number of round trips that are required.

| SNMP Version | Security                                              | Bulk Retrieval Mechanism |
| ------------ | ----------------------------------------------------- | ------------------------ |
| SNMPv1       | Plaintext authentication with community strings       | No                       |
| SNMPv2c      | Plaintext authentication with community strings       | Yes                      |
| SNMPv3       | Strong authentication, confidentiality, and integrity | Yes                      |

| Operation | Message            | SNMP Version | Description                                                                                                                                                                          |
| --------- | ------------------ | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| get       | GetRequest-PDU     | 1, 2c, 3     | Retrieves a value from a specific variable.                                                                                                                                          |
| get-next  | GetNextRequest-PDU | 1, 2c, 3     | Retrieves a value from a variable within a table.                                                                                                                                    |
| get-bulk  | GetBulkRequest-PDU | 2c, 3        | Retrieves large blocks of data, such as multiple rows in a table, that would otherwise require the transmission of many small blocks of data.                                        |
| set       | SetRequest-PDU     | 1, 2c, 3     | Stores a value in a specific variable.                                                                                                                                               |
| trap      | Trap-PDU           | 1, 2c, 3     | An unsolicited message that an SNMP agent sends to an SNMP manager when some event has occurred.                                                                                     |
| inform    | InformRequest-PDU  | 2c, 3        | SNMP manager needs to acknowledge the message that an SNMP agent sends when some event has occurred.. If the message is unacknowledged, it can be sent again by SNMP agent.          |
| response  | GetResponse-PDU    | 1            | Replies to a GetRequest-PDU, GetNextRequest-PDU, and SetRequest-PDU sent by an SNMP manager.                                                                                         |
|           | Response-PDU       | 2c,3         | Replies to a GetRequest-PDU, GetNextRequest-PDU, GetBulkRequest-PDU, SetRequest-PDU, and InformRequest-PDU sent by an SNMP manager. Replies to an inform-request sent by SNMP agent. |

![](<../.gitbook/assets/Unknown image (527)>)

{% hint style="info" %}
SNMPv1 does not provide acknowledgments between device and server.
{% endhint %}

The current SNMP version is SNMPv3. The precedent versions, SNMPv1 and SNMPv2 are considered obsolete and should not be used

Communities are created to permit or deny level of access to SNMP data

Read-only (RO): Gives read access to authorized management stations to all objects in the MIB except the community strings but does not allow write access.

Read/write (RW): Gives read and write access to authorized management stations to all objects in the MIB but does not allow access to the community strings.SNMPv3

primarily introduced security features such as confidentiality, integrity, and authentication of messages between SNMP managers and agents.

The SNMP security level defines the cryptographic security services that are applied to an SNMP session. There are three SNMP security levels:

noAuth: Authenticates SNMP messages using a community string.

Auth: Authenticates SNMP messages using either Hashed Message Authentication Code (HMAC) with Message Digest 5 (MD5) or Secure Hash Algorithm 1 (SHA-1).

Priv: Authenticates SNMP messages by using either Hashed Message Authentication Code-Message Digest 5 (HMAC-MD5) or Secure Hash Algorithm (SHA). Encrypts SNMP messages using Data Encryption Standard (DES), Triple Data Encryption Standard (3DES), or Advanced Encryption Standard (AES).

Elements

SNMP View defines what data are available to SNMP users

SNMP User is associated with an SNMP Group, where the user is added to define the access privileges and views, also includes specifying the level of encryption and authentication

SNMP Group is a collection of users assigned with the same level of permissions and level of access

As messages are created, they are given a special key that is based on the EngineID. The key is shared with the intended recipient and used to receive the message

Payload of the SNMP message is encrypted to ensure the confidentiality and integrity of the SNMP communication

Features

User-based Security Model (USM) provides data integrity and authentication for messages exchanged between the SNMP manager and agent

View-Based Access Control Model (VACM) is responsible for message processing models and access control in SNMP

It determines which SNMP objects can be accessed by specific users or groups and defines the level of access granted

Configuration

[https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/snmp/configuration/xe-17-x/snmp-xe-17-book/nm-snmp-cfg-snmp-support.html](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/snmp/configuration/xe-17-x/snmp-xe-17-book/nm-snmp-cfg-snmp-support.html)

| v2c config                                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| access-list 99 permit host 10.12.34.16                                                                                                                                                                                                                                                                                                                                                                               |                                                                                                |
| snmp-server community Public RO 99                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                |
| snmp-server community Private RW 99                                                                                                                                                                                                                                                                                                                                                                                  |                                                                                                |
| snmp-server enable traps bgp                                                                                                                                                                                                                                                                                                                                                                                         | enables bgp traps                                                                              |
| snmp-server ifindex persist                                                                                                                                                                                                                                                                                                                                                                                          | instructs the SNMP agent to maintain the interface index after reboot or interface changes     |
| show snmp \[group \| user]                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                |
| debug snmp packet                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                |
| v3 config                                                                                                                                                                                                                                                                                                                                                                                                            |                                                                                                |
| snmp-server engineID local 8000000903001234567890                                                                                                                                                                                                                                                                                                                                                                    | is a unique identifier for a network device used to authenticate and authorize SNMPv3 messages |
| snmp-server group MYGROUP v3 priv read MYVIEW write MYVIEW access MYACL                                                                                                                                                                                                                                                                                                                                              | specifies group                                                                                |
| snmp-server user MYUSER MYGROUP v3 auth sha PASSWORD123 priv aes 128 PASSWORD456                                                                                                                                                                                                                                                                                                                                     | specifies user                                                                                 |
| snmp-server view MYVIEW iso included                                                                                                                                                                                                                                                                                                                                                                                 | create an SNMP view named "MYVIEW" that includes the entire ISO OID subtree                    |
| snmp-server access MYACL ro public                                                                                                                                                                                                                                                                                                                                                                                   | configure RO access to the SNMP view "MYACL" for the SNMP community string "public"            |
| SNMP Verification The snmpwalk command recursively pulls data from the MIB tree, starting from the specified location. For example, you could use it to show which interfaces exist on a router. The snmpwalk command essentially performs a whole series of get-next requests automatically for you and stops when it returns results that are no longer inside the range of the OID that you originally specified. |                                                                                                |
| snmpwalk -v 2c -c                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                |
| snmpwalk -v3 -l AuthNoPriv -u -A                                                                                                                                                                                                                                                                                                                                                                                     |                                                                                                |
| show snmp \[stats \| alarm-history]                                                                                                                                                                                                                                                                                                                                                                                  |                                                                                                |

You can also use the snmpset command to reset the interface. In this example, the no shutdown command was issued on Serial2/0 via the snmpset command.

root@NMS:\~$ snmpset -v 3 -u test-user -l authPriv -a SHA -A auth-pass -x AES -X

priv-pass 10.1.10.1 1.3.6.1.2.1.2.2.1.7.9 i 1

IF-MIB::ifAdminStatus.9 = INTEGER: up(1)

The tools that are mentioned above can be found here: [https://cfnng.cisco.com/](https://cfnng.cisco.com/).

### How to push config (telnet) to CPE when you are unable to access CPE

Make sure snmp is working with snmpwalk, otherwise you won't push anything

Create txt file with the config you want to apply to the router:

enable

conf t

line vty 0 4

transport input all

end

wr

Then push it via snmpset -c -v 1 <.OID.IP.FTPIP> s ]
