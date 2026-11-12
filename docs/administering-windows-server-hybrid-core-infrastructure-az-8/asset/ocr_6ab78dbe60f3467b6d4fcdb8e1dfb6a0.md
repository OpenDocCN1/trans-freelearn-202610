The OS that runs on a VM is referred to as a guest OS. Hyper-V supports the following guest OS types:

+   - All supported versions of Windows

+   - CentOS, Red Hat Enterprise Linux, Debian, Oracle Linux, SUSE, Ubuntu, and FreeBSD

Where possible, generation 2 VMs should be used for the guests; this provides capabilities such as secure boot, shielded VMs, and hot add/remove virtual network adapters.

# Hyper-V networking

For communication on the network, the two primary components are as follows:

+   - Virtual network adapter

+   - Virtual network switch

The VM is configured with a virtual network adapter that connects to a port on the virtual network switch.

Two virtual network adapter types are supported:

+   - Legacy network adapter: These are only in generation 1 VMs and can be used for installation tasks using a network boot.

+   - Network adapter: Also called a synthetic adapter, it can be used with generation 1 and 2 VMs. It does not support the network boot for generation 1 VMs.

Three types of virtual network switches are supported:

+   - Private: This provides communication between VMs on the same Hyper-V host only; there is no communication between VMs and the Hyper-V host.

+   - Internal: This provides communication between VMs on the same Hyper-V host and between VMs and the Hyper-V host.

+   - External: This provides communication outside the host server, such as to the internet or another network.

A Virtual LAN (VLAN) can logically separate networks and partition traffic.

Virtual switches can be created and managed using Hyper-V Manager, Windows Admin Center, or PowerShell. The PowerShell cmdlet for this is New-VMSwitch.

# Nested virtualization

Nested virtualization allows an instance of Hyper-V to be installed on a VM, which then runs VMs created within this second layer of virtualization; the host VM runs as a virtualized Hyper-V host. The following diagram visualizes this concept: