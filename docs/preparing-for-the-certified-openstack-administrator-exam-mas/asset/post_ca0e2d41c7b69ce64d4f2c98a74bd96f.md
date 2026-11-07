
- **Compute nodes**: If the controller is considered the brain of the OpenStack cloud, then the compute nodes are the muscle. Compute nodes provide disk, CPU, and memory resources that are consumed by the instances. Each node contains a Type-1 hypervisor, as well as the Nova-compute daemon for talking directly to that hypervisor. There is also a Neutron plugin agent on these servers to assist the instances in getting plugged into the virtual network infrastructure.
- **Block storage nodes**: The block storage nodes provide Cinder block volumes to instances running in OpenStack. They typically run **Logical Volume Manager** (LVM), a popular Linux program for managing storage, as well as an iSCSI target daemon. When a user wants to attach a volume to an instance, an iSCSI connection is initiated from the instance's compute node to the block storage server where the volume resides.
- **Object storage nodes**: The object storage nodes make up the Swift object storage system. These nodes consist of commodity servers with cheap, high-capacity storage. The Swift daemons running on these servers are responsible for ensuring data replication and integrity across the entire object storage cluster.

## The all-in-one Newton environment

The provided virtual appliance contains all core OpenStack services on a single virtual machine. To download the appliance, please go to the official book repository at https://github.com/PacktPublishing/Preparing-for-the-Certified-OpenStack-Administrator-Exam.

See Figure 2.1. The appliance runs inside VirtualBox which runs on your host operating system. Figure 2.2 provides some additional information about the virtual appliance details. We will talk more about this as we progress through the chapters.

![](img/da864e0670c65edf6f5ab861ef9906b3_66_0.png)

Figure 2.1: The virtual appliance contains all core OpenStack services featured on the COA exam

![](img/da864e0670c65edf6f5ab861ef9906b3_67_0.png)

The Ceilometer Telemetry service is not a required learning objective on the exam and is not installed on the virtual appliance.

| Operating System | Ubuntu 16.04 |
| Operating System User Credentials | username: openstack
password: openstack |
| vCPU | 1VCPU |
| Memory | 6GB RAM |
| Disk | 80GB (dynamic) |
| Hostname | coa-aio-newton |
| VirtualBox Networking | Adapter1: NAT
Adapter2: Host-Only (vboxnet0) |
| Operating System Networking | eth0: 10.0.2.15
eth1: 192.168.56.56 |
| Database | MariaDB |
| AMQP | RabbitMQ |
| OpenStack Services | Keystone, Glance, Neutron, Nova, Cinder, Heat, Swift |
| Install Method / OpenStack Version | Ubuntu Cloud Archive - Newton |
| Horizon Dashboard URL | http://192.168.56.56/horizon |
| OpenStack User Credentials | username: admin
password: admin |
| Horizon Multi-Domain Support | Disabled |

Figure 2.2: Information about the virtual appliance included with this book

## Minimum hardware requirements

Although VirtualBox has minimal system requirements, the included all-in-one OpenStack Newton Virtual Appliance requires:

- 2 GHz or faster 64-bit (x64) processor with Intel VT-x or AMD-V support
- 6 GB available RAM
- 10 GB available hard disk space

![](img/da864e0670c65edf6f5ab861ef9906b3_68_0.png)

Most modern CPUs contain a hardware-assisted virtualization feature to help accelerate Type-1 and Type-2 hypervisors. Before importing the OVA, ensure that your system’s virtualization extensions are turned on in the BIOS or UEFI. Access your BIOS or UEFI firmware by entering the setup screen. Once inside, look for any of the following options and make sure they are turned on: Intel Virtualization Technology, Intel VT-X, AMD-V, Virtual Extensions, and so on. Save your settings and reboot the system.

## Downloading and installing VirtualBox

Once you are on a system that meets the minimum system requirements, proceed with the following steps:

- 1. Navigate to the VirtualBox downloads page at https://www.virtualbox.org/wiki/Downloads.
- 2. Download and install the latest version of VirtualBox for your host operating system.

## Configuring VirtualBox networking

- 1. Launch the VirtualBox application.
- 2. From the VirtualBox menu bar, select Preferences.
- 3. From the VirtualBox Preferences menu, select the Network icon.
- 4. Verify that Host-only Networks is highlighted.

### Mac/Linux

- 1. If you can see **vboxnet0** on the list, highlight and click on the tool icon.
- 2. If you do not see **vboxnet0**, click on the + icon. Next, highlight and click on the tool icon. Click on OK to save your settings. See Figure 2.3.

![](img/da864e0670c65edf6f5ab861ef9906b3_69_0.png)

Figure 2.3: Adding the vboxnet0 host-only network to VirtualBox on Mac or Linux

### Windows

- 1. If you can see **VirtualBox Host-Only Ethernet Adapter** on the list, highlight and click on the tool icon.
- 2. If you do not see **VirtualBox Host-Only Ethernet Adapter** on the list, click on the + icon. See *Figure 2.4*.

![](img/da864e0670c65edf6f5ab861ef9906b3_70_0.png)

Figure 2.4: Adding the VirtualBox Host-Only Ethernet Adapter to VirtualBox on Windows

## Confirming a host-only network IP address

Verify that your vboxnet0 or VirtualBox Host-Only Ethernet Adapter looks like Figure 2.5.

![](img/da864e0670c65edf6f5ab861ef9906b3_71_0.png)

Figure 2.5: vboxnet0 or VirtualBox Host-Only Ethernet Adapter should have 192.168.56.1

If you are currently using your host-only networks for other virtual machines in VirtualBox, you can create a new host-only adapter. Verify that the new host-only adapter looks like Figure 2.5. After importing the virtual appliance, right-click on the appliance and select Settings. Navigate to the Network section and select this host-only network for Adapter 2.

## Importing the appliance

- 1. From the VirtualBox menu bar, select Import Appliance. This will be under the File menu in Windows.
- 2. Click on the folder icon to browse your local drive.
- 3. Select coa-aio-newton.ova and click on Import or Open.
- 4. Click on Continue to proceed with installation.
- 5. Click on **Import**.
- 6. Once importing has completed, verify that the virtual machine has the proper network settings. See *Figure 2.6*.

![](img/da864e0670c65edf6f5ab861ef9906b3_72_0.png)

Figure 2.6: The virtual appliance should automatically have two adapters assigned

- 7. Start up the virtual machine by highlighting the appliance and clicking on **Start**. See *Figure 2.7*.

![](img/da864e0670c65edf6f5ab861ef9906b3_72_1.png)

Figure 2.7: Select start to power on the virtual appliance

![](img/da864e0670c65edf6f5ab861ef9906b3_72_2.png)

If you receive an error when attempting to start the virtual machine, verify you meet the aforementioned minimum system requirements. Also verify that virtualization extensions are turned on in the BIOS or UEFI.

- 8. Give the appliance time to boot. You will know it's up and running once you see the Ubuntu login prompt. See *Figure 2.8*.

![](img/da864e0670c65edf6f5ab861ef9906b3_72_3.png)

Figure 2.8: The virtual appliance is up and running once you see the login prompt

Although you are free to use the VirtualBox console for interacting with your newly imported OpenStack environment, you should SSH into the appliance from a terminal client on your host operating system.

## SSH into the appliance

Using a terminal client to SSH into the virtual machine appliance is the best way to work through the objectives in this book. Once connected, you can easily copy and paste commands from this book into the terminal client window.

![](img/da864e0670c65edf6f5ab861ef9906b3_73_0.png)

If you are not able to SSH directly to the instance, you may be connected to a corporate or school VPN that is rerouting LAN traffic. Disconnect from the VPN and attempt to reconnect. Custom SSH proxy configurations can also cause problems connecting and should be disabled or configured to ignore LAN traffic.

### Mac/Linux

- 1. Open up your built-in terminal.
- 2. Run the following:
- 3. When prompted for a password, enter openstack.

```
$ ssh openstack@192.168.56.56
```

### Windows

- 1. If you do not already have a terminal client installed, download Putty at http://putty.org.
