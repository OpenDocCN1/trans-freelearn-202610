# # Managing Windows Server file servers

This section will look at the file-based storage solutions in Windows Server. We will start this chapter by looking at definitions and concepts to help set a baseline and foundation of knowledge that you can build from.

# ## What is a Windows Server filesystem?

When a disk is presented to a computer, for it to be used for stored data, volumes must be created for it, which you then format with a filesystem. In this section, we will examine the question of what is a filesystem.

A filesystem is a mechanism or structure that allows you to hierarchically organize files and their access. It contains directories and folders created on a volume. It allows you to control a naming convention and format that can be stored. It also provides security and protection through access control.

The following filesystem types can be used with Windows Server storage systems:

+   - File Allocation Table (FAT), FAT32, Extended File Allocation Table (exFAT)
+   - New Technology File System (NTFS)
+   - Resilient File System (ReFS)

In the following sections, we will look at these filesystems and the other filesystem technologies available on Windows Server.

# ## What are the FAT, FAT32, and exFAT filesystems?

FAT is the simplest but least secure filesystem used by Windows. It uses a volume-level table for filesystem objects; the root directory and tables are fixed on a formatted disk. For resiliency, two copies of a table are kept.

The maximum volume size for FAT is 4 GB; FAT32 and exFAT support a maximum of 64 GB for partitions.

There is no security mechanism with FAT or FAT 32, so you should not use these volume types on any Windows Server OS computers. External media such as USB flash drives are the intended use case for the FAT, FAT32, and exFAT filesystems.

# ## What is the NTFS filesystem?

NTFS is the standard Windows Server OS filesystem.

Unlike FAT, FAT32, and exFAT, NTFS security is built-in and provides file and folder access control lists (ACLs) to provide granular access to and control of the files and folder structure. NTFS also provides encryption and compression; a file or folder can only be encrypted or compressed, but not both.