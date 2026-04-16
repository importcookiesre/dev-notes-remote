
# What is Virtualization ?  

**Virtualization** is the process of creating a virtual (rather than actual) version of a computing resource, such as hardware platforms, storage devices, network resources, or operating systems. 
- It allows multiple virtual instances to run on a single physical system, enabling better resource utilization, scalability, and isolation.

> Instead of running one OS on one physical machine, you can run multiple **Virtual Machines (VMs)**, each with its own OS, on the same physical host using a **hypervisor**.

## Types of virtualication : 

1. Hardware virtualization ( or system virtualization ) : 
	- Emulates complete hardware allowing multiple operating systems to run concurrently on a host system.
	- Achieved using a **hypervisor** (e.g., VMware ESXi, Microsoft Hyper-V, KVM).
	- Two types:
	    - **Full Virtualization**: VMs are fully isolated and unaware they are on virtual hardware.
	    - **Paravirtualization**: Guest OS is aware it is being virtualized and can optimize performance.

2. Operating System Virtualization : 
	- Uses **containers** instead of full VMs (e.g., Docker, LXC).
	- Multiple isolated user-space instances run on the same OS kernel.
	- Lightweight and faster than hardware virtualization.

3. **Storage Virtualization**:
	- Combines multiple physical storage resources into a single logical storage pool.
	- Common in SAN (Storage Area Networks) and cloud storage environments.

4. Network Virtualization : 
5. Desktop Virtualization : 
6. Application Virtualization : 

# What is a hypervisor ? 

A **hypervisor**, also known as a **Virtual Machine Monitor (VMM)**, is a software layer or firmware that enables **virtualization** by allowing multiple **virtual machines (VMs)** to run on a single physical hardware host.

## Types of hypervisors : 

#### **1. Type 1 Hypervisor (Bare-Metal Hypervisor):**

- **Runs directly on physical hardware**
- Does not require a host operating system
- Common in **enterprise-grade virtualization**
- Provides **better performance and efficiency**

**Examples:**
- VMware ESXi
- Microsoft Hyper-V (Server Core version)
- Xen
- KVM (Linux Kernel-based Virtual Machine)
```text
Hardware
   |
[Type 1 Hypervisor]
   |
[Virtual Machines (each with Guest OS)]
```

#### **2. Type 2 Hypervisor (Hosted Hypervisor):**

- **Runs on top of a conventional operating system**
- Suitable for **desktop virtualization and development**
- Easier to set up but **less performant** than Type 1
**Examples:**
- Oracle VirtualBox
- VMware Workstation
- Parallels Desktop (macOS)
- QEMU (when run as a user application)

```text
Hardware
   |
[Host Operating System]
   |
[Type 2 Hypervisor]
   |
[Virtual Machines]
```