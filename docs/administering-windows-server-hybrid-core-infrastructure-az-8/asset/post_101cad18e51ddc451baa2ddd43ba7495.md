
The term we use for these virtualized physical machines is **virtual machine (VM)**. VMs are the virtualization of a computer’s physical memory and processor resources; physical computers are emulated in software. This allows a physical machine’s resources to be subdivided and have a portion allocated to each virtual machine. Each VM runs its own isolated OS, the same as a physical server. The OS accesses virtual compute resources such as **virtual processors (virtual CPU or vCPU)** and virtual memory.

The next section will look at the relationship between physical computers and VMs.

## Comparing virtual and physical machines

To understand the virtualization model over the physical model, we will briefly explore the physical hardware model of operation.

Historically, an organization operated on a model where, typically, the application and dedicated instance of an OS were mapped one-to-one to physical hardware. This was not very efficient in terms of costs or operations and also not very scalable since each app or new workload requested by the business required a new physical server. This led to scale-out server sprawl with increased rack space and associated costs.

This traditional physical approach model is outlined in the following diagram:

![](img/cf35f46a8dd5f11e58349c1d81611b75_320_0.png)

Figure 9.1 – Physical server app deployment model

This traditional app delivery approach with a one-to-one relationship can benefit from the virtualization model. Virtualization addresses the following key challenges of the traditional physical hardware model:

- Utilization
- Scale
- Cost

The concept of virtualization is based on *hardware abstraction*, and for compute virtualization, the *hypervisor* is the technology layer that enables this hardware abstraction.

*Abstraction* involves removing dependencies and ignoring or filtering out some aspects that are no longer relevant so that what is important can remain the focus. In this hardware abstraction context, the VM is no longer dependent on the hardware layer it runs on when we abstract it (that is, when we remove the hardware from the equation); we no longer need that layer or need to concern ourselves with it.

This means that the value of virtualization is that we can do more with less by utilizing resources at more efficient costs and economies of scale.

We can achieve these efficiencies and optimizations by running multiple VMs from one physical server. This also means less rack space, fewer physical assets to cool, power, and maintain, and possibly less human capital. Technology can be seen as a more tangible value creation center, not a cost center.

The nutshell version is that when we look at the preceding diagram (*Figure 9.1*), we can deliver more apps with fewer physical servers through virtualization, which translates to a lower cost to deliver and operate those apps for the business.

Each VM shares the hardware resources from the underlying physical server (*referred to as the host*) that it is located on. These can be referred to as host resources. The subdivision of the host resources means we can create many virtual processors and memory resources to be assigned to the host VM. The following diagram illustrates this virtualization approach:

![](img/cf35f46a8dd5f11e58349c1d81611b75_321_0.png)

Figure 9.2 – Virtualization app deployment model

The diagram shown in *Figure 9.1* illustrates that we deployed two physical servers to run two applications with virtualization. In the example shown in *Figure 9.2*, we can deploy one physical server to run four applications; this is the benefit and value of virtualization. The underlying physical server host resources will determine how many VMs can be run... *your mileage may vary*.

In this section, we compared physical computers and VMs and learned the value and benefits of virtualization. In the next section, we will introduce Hyper-V.

## What is Hyper-V?

Now that we have understood virtualization's value and benefits, we will introduce Hyper-V as the enabler.

*Hyper-V is Microsoft's hypervisor, a piece of virtualization software and the technology layer that enables this hardware abstraction.*

Example use cases for Hyper-V are as follows:

- Physical computer infrastructure consolidation
- Remote Desktop Virtualization/Virtual Desktop Infrastructure (VDI) implementations
- Test and development environments
- Private and hybrid cloud infrastructure environments, including **Azure Stack Hub** and **Azure Stack Hyper-Converged Infrastructure (HCI)** implementations

The following diagram outlines the Hyper-V system architecture:

![](img/cf35f46a8dd5f11e58349c1d81611b75_322_0.png)

Figure 9.3 – Hyper-V system architecture

The preceding diagram shows that interaction with the physical machine layer is passed through the hypervisor software. The physical machine OS is known as the host OS, and the virtual machine OS is known as the guest OS.

In this section, we introduced Hyper-V as the virtualization software layer that allows us to create and run VMs. In the next section, we will learn how to implement and manage Hyper-V.

## Implementing and managing Hyper-V

**Hyper-V can be installed as a server role** in Windows Server using Server Manager, PowerShell, or Windows Admin Center (WAC).

The PowerShell `Install-WindowsFeature` cmdlet can be used to install the role locally or on remote hosts. The PowerShell `Get-WindowsFeature` cmdlet can then be run to verify the installation of Hyper-V. We will look at the installation steps in the Hands-on exercises section.

When installing the Hyper-V role, it should be the only server role that's installed on the host server; you should not install roles such as Domain Services or a File Server role.

The Server Core installation of Hyper-V has the following benefits over the full GUI OS installation option:

- Minimized hardware resources and better utilization for OS and guest VMs
- Minimized OS resources running and better utilization for OS and guest VMs
- Fewer software updates
- Smaller attack/vulnerable surface area; fewer services are running
- Streamlined deployment and remote management

The system hardware requirements for implementing Hyper-V are as follows:

- 64-bit processor
- **Second-Level Address Translation (SLAT)**
- VM Monitor Mode extensions
- **Intel Virtualization Technology (Intel VT) or AMD Virtualization (AMD-V)**
- Hardware-enforced **Data Execution Prevention (DEP)**
- Intel XD bit and AMD NX bit

The Hyper-V hosts should have sufficient processors and memory resources to support the number of VMs required to run on each host. Sufficient storage that is fast enough, network speed, and capacity should also be planned.

The OS that runs on a VM is referred to as a guest OS. Hyper-V supports the following guest OS types:

- All supported versions of Windows
- CentOS, Red Hat Enterprise Linux, Debian, Oracle Linux, SUSE, Ubuntu, and FreeBSD

Where possible, generation 2 VMs should be used for the guests; this provides capabilities such as secure boot, shielded VMs, and hot add/remove virtual network adapters.

## Hyper-V networking

For communication on the network, the two primary components are as follows:

- Virtual network adapter
- Virtual network switch

The VM is configured with a virtual network adapter that connects to a port on the virtual network switch.

Two virtual network adapter types are supported:

- Legacy network adapter: These are only in generation 1 VMs and can be used for installation tasks using a network boot.
- Network adapter: Also called a synthetic adapter, it can be used with generation 1 and 2 VMs. It does not support the network boot for generation 1 VMs.

Three types of virtual network switches are supported:

- Private: This provides communication between VMs on the same Hyper-V host only; there is no communication between VMs and the Hyper-V host.
- Internal: This provides communication between VMs on the same Hyper-V host and between VMs and the Hyper-V host.
- External: This provides communication outside the host server, such as to the internet or another network.

A Virtual LAN (VLAN) can logically separate networks and partition traffic.

Virtual switches can be created and managed using Hyper-V Manager, Windows Admin Center, or PowerShell. The PowerShell cmdlet for this is New-VMSwitch.

## Nested virtualization

Nested virtualization allows an instance of Hyper-V to be installed on a VM, which then runs VMs created within this second layer of virtualization; the host VM runs as a virtualized Hyper-V host. The following diagram visualizes this concept:
