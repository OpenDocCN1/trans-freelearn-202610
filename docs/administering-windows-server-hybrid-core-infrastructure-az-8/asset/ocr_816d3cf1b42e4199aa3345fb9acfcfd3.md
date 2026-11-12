# # How does Storage Spaces work?

Storage Spaces is similar in concept to the traditional SAN storage model. In this model, physical disks from a storage enclosure are grouped/aggregated to provide a RAID pool. From this, we create LUNs, which are presented to a server as one or more volumes by initializing them, assigning a drive letter, and then formatting them with filesystem such as NTFS. This can be seen in the following diagram:

![](bbox=[0.26, 0.225, 0.735, 0.615])

Figure 7.7 – Traditional SAN concept

In the Storage Spaces model, physical disks are grouped from a pool of disk resources, and in this storage pool of physical disks, virtual disks are created; a virtual disk is equivalent to a LUN. These virtual disks can be presented to a computer, then initialized and brought online so that they can be formatted and have a drive letter assigned. A Storage Spaces virtual disk can be formatted with FAT32, NTFS, and ReFS.

Storage Spaces has the following components:

+   * **Storage pools**: These are physical disks that are combined to create a pool of disk resources from which virtual disks can be created. The disks in the pool can be of different types and sizes; the pools abstract the underlying physical disks, and much like in compute virtualization, the hardware layer is abstracted. The physical disk can only be a part of one pool.