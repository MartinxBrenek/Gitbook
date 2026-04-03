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

# Virtualization

### Overview

**Virtualization** is a technology that allows us to create multiple independent machines (VM's) or containerized operating systems (containers) on a single physical server

**Virtual Machine (VM)** is a software emulation of a physical server with an operating system (OS)

### Advantages of VMs

VMs and containers increase the overall efficiency and cost-effectiveness of a server by maximizing the utilization of the available hardware resources, which reduces equipment costs and efficiencies in space, power and cooling

Migration from one server to another with no downtime

If a server fails, the VMs can be spun up on other servers in the network

Elasticity to scale up or scale down and increase or decrees capacity on demand

Opportunities to test and deploy new innovative services virtually and with lower risk

![](<../.gitbook/assets/Unknown image (1585)>)

### Hypervisors

**Hypervisor** is a software that creates VMs and performs the hardware abstraction that allows multiple VMs to run concurrently

Type 1 runs directly on the system hardware

Type 2 requires a host OS to run. Typically used by client devices

Example products:

VMware

VMware ESXi is the hypervisor itself, running directly on the server hardware and enabling the creation and management of virtual machines.

VMware vCenter Server is a centralized tool for managing ESXi guests and virtual machines, providing advanced features such as clustering, vMotion, HA (High Availability), and more.

VMware vSphere is the name given to the entire virtualization platform, which includes both ESXi and vCenter Server and other components.

Breakdown of Key Components:

ESXi The hypervisor that runs on physical servers to host VMs.

vCenter Server The centralized management tool for multiple ESXi hosts. Enables advanced features like vMotion, HA, DRS, etc.

vSphere Client GUI (web-based) interface to manage vCenter and ESXi.

vSphere The full suite of VMware’s virtualization technologies, including ESXi, vCenter, and other tools like vSphere Replication and Lifecycle Manager.

Hyper-V is similar to the VMware - Owned by: Microsoft Part of: Windows operating systems (e.g. Windows Server, Windows 10/11 Pro/Enterprise)

VirtualBox Owned by: Oracle Corporation

KVM (Kernel-based Virtual Machine) Owned by: Open-source community, maintained under Linux Foundation

![](<../.gitbook/assets/Unknown image (1586)>)

### Containers and orchestration

**Container** is an isolated environment where containerized applications run. **Container image** is a file created by a container engine that includes the application code along with its dependencies needed to run the app Rkt, Docker, and LXD are container engines

Docker is a containerization platform that allows you to package an application and all its dependencies into a lightweight, portable container. Unlike a virtual machine (VM), which includes a full OS, a Docker container shares the host system's kernel but has its own isolated user space, including dependencies, configurations, and libraries.

**Kubernetes** is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications.

### Virtual switching

**Virtual Switch (vSwitch)** enables VMs to communicate with each other within a virtualized server and with external physical networks through the physical network interface cards (pNICs)

Distributed virtual switching feature that aggregates vSwitches together from a cluster of virtualized servers and treats them as a single distributed virtual switch

This facilitates the configuration of each vSwitch for each newly created VM or container

![](<../.gitbook/assets/Unknown image (1587)>)

### Network Functions Virtualization (NFV)

**Network Functions Virtualization (NFV)** is an architectural framework that decouple network functions from hardware-based appliances and have them run in software on standard x86 servers

**ETSI Management and Orchestration (MANO)** defines the framework for management and orchestration of the virtualized infrastructure in data centers (compute, storage, network, and VMs).

**Virtual network function (VNF)** is the virtual or software version of an network function

**NFV infrastructure (NFVI)** is all the hardware and software components that comprise the platform environment

**Virtualized Infrastructure Manager (VIM)** is responsible for managing and controlling the NFVI hardware and virtualized resources (compute, storage, network)

Manages the entire lifecycle (setup, maintenance and teardown) of the infrastructure and creates Service chaining that chains VNFs together to provide more complex NFV service/solution

![](<../.gitbook/assets/Unknown image (1588)>)

**Element manager (EM)** responsible for management of an individual VNF. It handles fault detection, performance monitoring, configuration management and software updates

**NFV orchestrator** is responsible for creating, maintaining, and tearing down VNFs. EM+orchestrator = MANO

### Operations Support System (OSS) / Business Support System (BSS)

**Operations Support System (OSS)** is a platform operated by service providers for large enterprise networks, it maintains network inventory, provision new services, configure network devices, and resolve issues

**Business Support System (BSS)** is a combination of product management, customer management, revenue management (billing), and order management systems that are used to run the SP’s business operations.

OSS operates in tandem with BSS to improve the overall customer experience.

![](<../.gitbook/assets/Unknown image (1589)>)

In NFV solutions, the data traffic has two different patterns:

North-south traffic is sent from VNF of the server through a physical NIC (pNIC) and vice versa = user data

East-west traffic between VNFs (service chained) = statistics/communication between devices

### Hardware fundamentals (packet I/O)

**Input/output (I/O)** The communication between a computing system (server) and the outside world.

**Input** is the data received by computing system, and **output** is the data sent from it.

**I/O device** A peripheral device such as a mouse, keyboard, monitor, or NIC.

**Interrupt request (IRQ)** A hardware signal sent to CPU by an I/O device to notify the CPU when it has data to transfer

This is interaction between I/O devices and CPU (typing on keybord, clicking on mouse or modem transferring data) CPU saves its current state, temporarily stops what it’s doing, and runs an interrupt handler routine associated to the device. The interrupt handler determines the cause of the interrupt, performs the necessary processing, performs a CPU state restore, and issues a return-from-interrupt instruction to return control to the CPU so that it can resume what it was doing before the interrupt

**Interrupt handler** is included in each I/O device's driver.

**Device driver** A computer program that controls an I/O device and allows the CPU to communicate with the I/O device

A NIC is an example of an I/O device that requires a driver to operate and interface with the CPU.

**Direct memory access (DMA)** A memory access method that allows an I/O device to send or receive data directly to or from the main memory, bypassing the CPU, to speed up overall computer operations.

**Kernel** (“core” in German) is a program that is the central (core) part of an OS. It directly manages the computer hardware components, such as RAM and CPU, and provides system services to applications that need to access any hardware components, including NICs and internal storage. Because it is the core of an OS, the kernel is executed in a protected area

of the main memory (kernel space) to prevent other processes from affecting it.

**Non-kernel processes** are executed in a memory area called the user space, which is where applications and their associated libraries reside.

In non-virtualized environments, data traffic is received by a pNIC and then sent through the kernel space to an application in the user space.

In a virtual environment, there are pNICs and virtual NICs (vNICs) and a hypervisor with a virtual switch in between them

The hypervisor and the virtual switch are responsible for taking the data from the pNIC and sending it to the vNIC of the VM/VNF and finally to the application.

The addition of the virtual layer introduces additional packet processing and virtualization overhead, which creates bottlenecks and reduces I/O packet throughput.

Interrupts add a lot of overhead because any activity the CPU is doing must be stopped,the state must be saved, interrupt must be processed, and original process must be restored so that it can resume what it was doing before the interrupt

![](<../.gitbook/assets/Unknown image (1590)>)

**Virtual Network Computing (VNC)** is a technology that allows users to remotely access and control computers or servers over a network. It enables users to view the graphical desktop interface of a remote computer and interact with it as if they were physically present at the machine. VNC operates by transmitting keyboard and mouse input from the client to the server, and sending screen updates from the server to the client, allowing for real-time interaction with the remote system. It is commonly used for remote technical support, system administration, and accessing desktop applications running on remote machines

### Open vSwitch DPDK (OVS-DPDK)

**Open vSwitch DPDK (OVS-DPDK)** employs a Poll Mode Driver (PMD) that continuously polls the physical Network Interface Card (pNIC) for incoming data, bypassing the kernel and the need for interrupts.

This approach allows for efficient packet processing without the overhead of interrupt handling

To achieve this, 1 or more CPU cores are dedicated to polling and handling incoming data, optimizing data plane performance

Peripheral Component Interconnect (PCI) passthrough used to map a pNIC directly to a single VNF

#### Single-Root I/O Virtualization (SR-IOV)

**Single-Root I/O Virtualization (SR-IOV)** enhancement to PCI that allows multiple VNFs to share the same pNIC. It enhances I/O performance by enabling direct assignment of virtual functions (VF) to virtual machines (VMs) or containers, bypassing the hypervisor layer for data plane traffic and as a result, the I/O overhead in the software emulation layer is diminished and achieves network performance that is nearly the same performance as in nonvirtualized environments

![](<../.gitbook/assets/Unknown image (1591)>)

![](<../.gitbook/assets/Unknown image (1592)>)

### Data traffic reception and processing workflow

Step 1. Data traffic is received by the pNIC and placed into an Rx queue (ring buffers) within the pNIC.

Step 2. The pNIC sends the packet and a packet descriptor to the main memory buffer through DMA. The packet descriptor includes only the memory location and size of the packet.

Step 3. The pNIC sends an IRQ to the CPU.

Step 4. The CPU transfers control to the pNIC driver, which services the IRQ, receives the packet, and moves it into the network stack, where it eventually arrives in a socket and is placed into a socket receive buffer.

Step 5. The packet data is copied from the socket receive buffer to the OVS virtual switch.

Step 6. OVS processes the packet and forwards it to the VM. This entails switching the packet between the kernel and user space, which is expensive in terms of CPU cycles.

Step 7. The packet arrives at the virtual NIC (vNIC) of the VM and is placed into an Rx queue.

Step 8. The vNIC sends the packet and a packet descriptor to the virtual memory buffer through DMA.

Step 9. The vNIC sends an IRQ to the vCPU.

Step 10. The vCPU transfers control to the vNIC driver,which services the IRQ, receives the packet, and moves it into the network stack, where it eventually arrives in a socket and is placed into a socket receive buffer.

Step 11. The packet data is copied and sent to the application in the VM.

### Cisco Enterprise Network Functions Virtualization (ENFV)

**Cisco Enterprise Network Functions Virtualization (ENFV)** solution for branch offices that replaces physical FWs, routers, WLC, load balancers with virtual devices running in a single x86 platforms

Reduces the number of physical devices at the branch, resulting in efficiencies in space, power, maintenance and cooling

Reduces the need for technician site visits to perform hardware installations or upgrades for individual device

New services, critical updates, VNFs, and branch locations deployed in minutes

Components

Virtual Network Functions and Applications

Supports Viptela vEdge, cEdge, Cisco Integrated Services Virtual Router (ISRv), NGFWv, ASAv, vWLCs, vWAAS and third party, PaloAlto, Fortinet, Linux Server, Windows Server that can be installed on uCPE

Management and Orchestration (MANO)

Centralizes management through Cisco DNA Center, which simplifies designing, provisioning, updating, managing, and troubleshooting network services and VNFs.

Network profiles are created and assigned to branches that matches the requirements.

Plug and Play (PnP) provisioning provides a way to automatically and remotely provision and onboard new network devices

Network Function Virtualization Infrastructure Software (NFVIS)

**Network Function Virtualization Infrastructure Software (NFVIS)** is based on standard Linux with additional functions for virtualization, VNF lifecycle management, monitoring and device programmability

NFVIS is supported on

Cisco Enterprise Network Compute System (ENCS)

Cisco Cloud Services Platforms

Cisco 4000 Series ISRs with a Cisco UCS E-Series blade

UCS C-Series

![](<../.gitbook/assets/Unknown image (1593)>)

Service providers see NFV as a way to unlock new revenues by creating new services more easily and bringing them to market more quickly. They also expect to lower operational expenses (OpEx), by automating the end-to-end lifecycle of service creation, provisioning, and assurance. The primary factor in unlocking this potential is NFV orchestration: a way to assemble, recombine, and manage all elements of their virtualized environments in an automated way.

### OpenStack

**OpenStack** is an open-source project for infrastructure virtualization. OpenStack is mostly deployed as Infrastructure as a Service (IaaS). It has a very aggressive development cycle (around 6 months) and does not pay attention to the backward compatibility. You can get the code directly from [https://www.openstack.org/](https://www.openstack.org/), but it will be the same as if you used Linux directly from the source ( [https://www.kernel.org/](https://www.kernel.org/)). A more viable option is to use one of the prepacked repositories of Linux distributions or from the companies, which are repackaging OpenStack

![](<../.gitbook/assets/Unknown image (1594)>)

#### Components

**Dashboard service (Horizon)** is a web-based user interface for the management of the OpenStack services. It also serves as a Cisco Unity Inbox in the landscape of ETSI MANO.

**Identity service (Keystone)** is the authentication and authorization service for the OpenStack and supports username and password, or token-based authentication, to verify credentials. It also defines token for each OpenStack component.

**Compute service (Nova)** is the cloud computing fabric, which manages and automate compute resources. It supports different virtualization technologies, most commonly the KVM is used for that purpose.

**Networking service (Neutron)** provides connectivity to the other OpenStack components. Open vSwitch is used in the latest version of OpenStack as a chosen solution for Layer 2 network virtualization, it supports NetFlow, sFlow, Internet Protocol Flow Information eXport (IPFIX), Remote Switched Port Analyzer (RSPAN), Link Aggregation Control Protocol (LACP), 802.1ag. In addition to the Layer 2 networking feather the subnet management, DHCP, routers, firewalls, and VPN are supported.

**Image service (Glance)** is providing the service for storing VM images and volume snapshots, it supports raw, aki, ami, ari, iso, qcow2, vhd, vdi, vmdk, and ovf image formats. The images are used as a template from which the new VMs are instantiated.

**Block storage service (Cinder)** is providing virtual storage for virtual machines in the system. For the VMs, which are running on the Horizon interface, a user can add or expand disk space for it.

**Object storage (Swift)** is a scalable and distributed object storage system, where you can store your files, videos, images, virtual machine backups, and other unstructured data.

![](<../.gitbook/assets/Unknown image (1595)>)

Projects are used for creation and management of instances, they are also known as tenants or accounts. Inside the project tab, you can access the following categories:

Compute tab: You can create, launch, stop pause, reboot, or create snapshots for instances. Manage images (create, edit, and delete) and launch instances from images and snapshots)

Volume tab: You can manage (view, create, edit, and delete) volumes, backups, and snapshots

Network tab: View the network topology. Manage networks, routers, security groups, and IP addresses.

Admin tab is used, for example, for volume, flavor, image, and network management on a global scale. Inside the admin tab you can access the following categories:

Compute tab: You can create, launch, stop pause, reboot, or create snapshots for instances. Manage images (Create, edit, and delete) and launch instances from images and snapshots

Volume tab: You can manage (view, create, edit, and delete) volumes, snapshots, and volume types.

Network tab: You can view the network topology. Manage networks, routers, and IP addresses.

System tab: You can view information about the services, manage metadata information.

The Identity tab is used for project and user management (view, create, enable, delete, and so on).

### App-hosting

**App-hosting** allows users to deploy and execute various third-party automation, management or monitoring applications directly on the networking device within it's containerized environment, so that it is leveraging its computing resources and integrating applications with network services

#### Guest Shell

**Guest Shell** is a feature available on Cisco devices like routers and switches that enables the deployment of Linux containers directly on the device.

It provides a secure and isolated environment where network engineers can run custom scripts, applications, or utilities without compromising the integrity of the device's operating system. Supported only on IOS-XE platforms (version 16.5 “Everest” or higher)

Allows network engineers to automate tasks directly on the network device without the need for external servers or devices.

Facilitates integration with third-party applications and tools by providing a Linux environment.

Enhances troubleshooting capabilities by enabling the use of familiar Linux-based utilities and commands
