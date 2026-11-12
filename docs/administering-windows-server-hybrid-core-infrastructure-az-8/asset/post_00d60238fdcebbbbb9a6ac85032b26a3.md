
## How does iSCSI work?

As a storage protocol, iSCSI operates within the Open Systems Interconnection (OSI) network stack model.

iSCSI commands sent between a server and a storage system are encapsulated in TCP/IP packets and sent over an Ethernet network as any regular network packet; the payload can be a SCSI command. The packets are then passed back up through the network stack, and the SCSI commands are stripped out and interpreted as local device SCSI commands by the storage system. Data transmitted between the storage system and client computers follow the same TCP/IP encapsulation.

This concept is shown in the following diagram:

![](img/cf35f46a8dd5f11e58349c1d81611b75_268_0.png)

Figure 7.4 – iSCSI encapsulation

This contrasts with the FC and FCoE SAN protocols, which are alternative methods of transporting the block data.

To identify initiators and targets, FC uses World Wide Names. For iSCSI addressing, IQN iSCSI Qualified Names, as defined in RFC-3271, are used.

The IQN has a limit of 255 characters, and the format is as follows:

- iqn.yyyy-mm.naming-authority:unique name
- iqn.2004-07.com.example:MBS_Target_1_SN_WC786556

Next, let's look at how it is implemented.

## How is iSCSI implemented?

iSCSI transmits information in IP packets over an Ethernet network infrastructure. This can be dedicated to storage traffic or sharing the data network but segregated by using virtual local area networks (VLANs) and Quality of Service (QoS) for performance.

To increase performance, an iSCSI host bus adapter (HBA) or TCP Offload Engine (TOE) card can be used to offload the IP packet processing from the server’s processors.

The following should be considered when implementing iSCSI:

- Workloads
- Network topology
- Performance
- Security
- High availability
- The skill sets of staff and cross-function team collaboration

We will look at some of these considerations in the following sections.

### Workloads

You should consider the use cases and types of workloads you need a storage system for in your environments, such as block, file, and object-based.

iSCSI target servers can provide access to block-based storage systems that are required by applications such as SQL Server, Exchange, and others.

The data is accessed at the block level, which refers to the data blocks and the SCSI commands; these commands and data are encapsulated in packets and sent over an Ethernet network to iSCSI initiators for client access.

iSCSI-connected storage systems can be heterogeneous; the iSCSI initiator is not dependent on the Windows OS, meaning support for multi-platform storage clients.

Diskless and network boot scenarios are also supported using differencing disks to save storage space.

### Network topology

You should consider whether a shared storage and data network topology or a network dedicated to storage will be implemented. This is important to evaluate in the planning stage as this can impact network performance and has security considerations. For shared data and storage networks, QoS should be configured for the network.

The minimum network speed should be 1 Gbps; a 10 Gbps, 40 Gbps, or 100 Gbps infrastructure can be used.

The following is a reference architecture for a shared storage and data network topology:

![](img/cf35f46a8dd5f11e58349c1d81611b75_270_0.png)

Figure 7.5 – iSCSI Ethernet network topology

In this preceding topology, an Ethernet network infrastructure is shared between storage and data traffic but segregated logically with separate VLANs for data traffic and storage traffic.

The following is a reference architecture for a dedicated storage network topology:

![](img/cf35f46a8dd5f11e58349c1d81611b75_270_1.png)

Figure 7.6 – iSCSI Ethernet network topology

In this preceding topology, an Ethernet network infrastructure is dedicated to storage traffic. The storage traffic is isolated physically by the dedicated storage switches.

### Security

You should consider using a dedicated network that is isolated. iSCSI does not support zoning, so the initiator and target are configured with password-based authentication for spoofing attack protection. To authenticate initiator connections, Challenge-Handshake Authentication Protocol (CHAP) is used; reverse CHAP is used to authenticate the iSCSI target.

Logical unit number (LUN) masking is the same as FC, but the IQN is used for client identification instead of a WWPN. To enhance security, IPSec can provide end-to-end encryption.

### High availability

You should consider how redundancy will be provided. You can use Microsoft Multipath Input/Output (MPIO) or Multiple Connections per Session (MCS) as follows:

- MPIO: This multipathing software can provide redundant network paths to a target for multiple sessions to the same target. The multipathing software is responsible for choosing the paths to take.

- MCS: This provides the target with multiple TCP/IP connections from the initiators for the same iSCSI session.

In this section, we looked at how iSCSI is implemented and concluded the topic of implementing Windows Server iSCSI. The following section will look at implementing Storage Spaces and Storage Spaces Direct.

### Implementing Storage Spaces and Storage Spaces Direct

This section will introduce storage virtualization using Microsoft Storage Spaces and Storage Spaces Direct. The first technology we will introduce in this section will be Storage Spaces; we will compare it with Storage Spaces Direct in the next section and understand the differences.

### What is Storage Spaces?

Storage Spaces is a Microsoft storage virtualization technology that allows the local disks of a computer to be aggregated into an array of disks that can be used to create virtual disks. It is also a way to provide storage system redundancy through protection against drive failures. This can be used as an alternative to the traditional disk failure protection method of a Redundant Array of Independent Disks (RAID) solution.

When selecting a storage technology for a given scenario, Storage Spaces provides an inexpensive storage solution that is simple to implement and manage while being scalable and reliable.

## How does Storage Spaces work?

Storage Spaces is similar in concept to the traditional SAN storage model. In this model, physical disks from a storage enclosure are grouped/aggregated to provide a RAID pool. From this, we create LUNs, which are presented to a server as one or more volumes by initializing them, assigning a drive letter, and then formatting them with filesystem such as NTFS. This can be seen in the following diagram:

![](img/cf35f46a8dd5f11e58349c1d81611b75_272_0.png)

Figure 7.7 – Traditional SAN concept

In the Storage Spaces model, physical disks are grouped from a pool of disk resources, and in this storage pool of physical disks, virtual disks are created; a virtual disk is equivalent to a LUN. These virtual disks can be presented to a computer, then initialized and brought online so that they can be formatted and have a drive letter assigned. A Storage Spaces virtual disk can be formatted with FAT32, NTFS, and ReFS.

Storage Spaces has the following components:

- **Storage pools**: These are physical disks that are combined to create a pool of disk resources from which virtual disks can be created. The disks in the pool can be of different types and sizes; the pools abstract the underlying physical disks, and much like in compute virtualization, the hardware layer is abstracted. The physical disk can only be a part of one pool.

- **Storage spaces**: These are equivalent to RAID sets (pools) and are virtual disks in this terminology. They are created on the physical disks from the available space in the storage pools. A volume is a virtual disk partition and can have a drive letter assigned; volumes are the equivalent of LUNs on a SAN.

The relationship between these components can be seen in the following diagram:

![](img/cf35f46a8dd5f11e58349c1d81611b75_273_0.png)

Figure 7.8 – Storage Spaces components

Storage Spaces can provide the following functionality:

- **Storage tiers**: Different disk types and speeds can be used in a pool, such as **hard disk drives (HDD)** and **solid-state drives (SSD)**; USB drives can even be used to create a storage pool. Storage Spaces uses tiering to achieve capacity and performance balance. It automatically moves data that is more frequently accessed to faster disks and to slower disks for less frequently accessed data. Data is moved once a day at 01:00 A.M. You can configure which storage is used by files and pins as required.

- **Provisioning models**: Storage Spaces provides two provisioning models for virtual disks, as follows:

  - **Thick (fixed) provisioning**: This creates a fixed size at the point of creation.

  - **Thin provisioning**: This does not pre-allocate a size at the point of creation and utilizes just-enough and just-in-time creation. A trim process can reclaim unused allocated space.

A storage pool can have both *thin* and *fixed* provisioned virtual disks.
