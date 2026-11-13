
## 6. Android OS Overview

The correct answer is **C**. Android is an open source (that is, royalty free) operating system developed by Google. It was initially released in 2008 and, in 2023, is currently on version 13 (codename Tiramisu). Android OS is based on a Linux kernel and supports many of the security features used in mainstream Linux distributions. iOS is a proprietary Apple-based mobile operating system originally used exclusively on Apple hardware, specifically the code base for iPhones, iPods, and iPads. SUSE Linux is not used on mobile devices or smartphones; it is primarily deployed on datacenter servers. Windows Mobile uses a modified version of the Windows desktop OS.

## 7.9 Given a Scenario, Perform OS Installations and Upgrades in a Diverse OS Environment

### Question 1

The correct answer is **D**. Pre-Boot Execution Environment (PXE) allows a computer to boot from the Network Interface Card (NIC) and access services on the network. In this case, the technician could use the media to create a Windows Deployment Server and perform the upgrades from a central location. Booting from a Solid State Drive (SSD) does not meet the requirements of installing over the network. Optical drives and flash drives can be used to install a single image but do not solve the problem of upgrading multiple computers across the network.

### Question 2

The correct answer is **D**. An unattended installation allows a central deployment image to be made available alongside an automated setup file (or unattended script). Once the installation is started, the technician can leave it to complete on its own. A remote network installation does not meet the requirements for an automated installation. A repair installation allows for a faulting computer to be reimaged back to a standard build configuration. A clean installation does not describe an automated process.

### Question 3

The correct answer is **C**. The GUID Partition Table (GPT) disk format allows partition sizes of up to 18 EB (that is, 18.8 million TB). A GPT drive can also contain as many as 128 separate partitions. ext4 is not supported on Windows; it is used on Linux filesystems. Master Boot Record (MBR) partitions are limited to a maximum size of 2TB and support a maximum of four partitions per disk. Apple File System (APFS) is not supported natively on Windows computers.

### Question 4

The correct answer is **D**. An in-place upgrade will allow the customer to keep all supported applications and retain their settings. It is important to check that in-place upgrades are supported for the version of Windows they are currently using. A clean install will not retain applications or settings. A repair installation will reset the image back to default for the current build of Windows on the customer’s system and is intended as a troubleshooting option. An unattended install does not necessarily mean that an upgrade will be selected.

### Question 5

The correct answer is **C**. Windows 10 was the first Windows operating system to support feature upgrades (Windows 11 also supports feature upgrades). Typically released every six months, feature upgrades include additional features and functionality. In previous releases of Windows, new features and functionality were delivered with service packs or interim releases, such as the upgrade of Windows 8.0 to 8.1. Windows 7 and Windows 8.1 do not support feature upgrades.

### Question 6

The correct answer is **C**. An image deployment allows for a customized build of the operating system (along with any required applications) to be created. The image can be hosted on a Windows Deployment Server, and all new installations will use the standard centralized image. An upgrade will retain current applications and settings (some existing desktop computers may have different applications installed and be configured differently). Clean installations do not always ensure the same settings will be applied. A repair installation is used to set a Windows computer back up on a standard image as a troubleshooting step.

### Question 7

The correct answer is **B**. When installing images from a central server across the network, it is expected that the overall traffic on the network segment will increase. It is important that any unacceptable load on the network is identified. The impact on users will only be a concern if the network is overwhelmed with additional traffic. DHCP will need to provide IP addresses to 30 workstations at a time (during the build), so this should not add any great burden to the DHCP server. The impact on production workstations will only be a negative concern if the network is experiencing unacceptable volumes of additional traffic during the build process.

### Question 8

The correct answer is **B, C**. Unified Extensible Firmware Interface (UEFI) replaces the original Binary Input Output System (BIOS) seen in computer systems since the original 1980s PC systems. UEFI is needed to support GPT disks. Windows 11 installation requires the presence of a Trusted Platform Module (TPM) to support some of the security features now standard in the operating system. Windows 11 requires 4GB RAM and a 1GHz 64-bit processor in addition to TPM 2.0 and UEFI support.

### Question 9

The correct answer is **B**. A repair installation allows for boot issues, such as missing or corrupt startup files, to be fixed. The technician will be presented with the Advanced Options menu if Windows cannot boot and the Automatic Repair function has failed. This menu provides options including System Restore, System Image Recovery (if you have a backup), or to run a Startup Repair. Unattended, image deployment, and remote installations are not intended as troubleshooting steps.

### Question 10

The correct answer is **B**. The Windows PC Health Check App is a free Microsoft download and allows a Windows computer to be assessed prior to an upgrade. Following this assessment, the app will provide you with a list of system requirements for the given upgrade and state whether or not these are met by the system in question. While the other options do provide useful information for technicians, they do not present an easy-to-read upgrade report.

## 7.10 Identify Common Features and Tools of the macOS/Desktop OS

### Question 1

The correct answer is **C**. The standard file extensions for a Mac installation file is DMG (from Disk iMaGe) and replaces the much older IMG file format. MSI and EXE file formats are used by Windows computers. APK files are Android Package files and are used to deploy applications on devices running the Android operating system.

### Question 2

The correct answer is **B**. The Mac Finder application allows the user to view files on all supported media. Keychain is used to support single sign-on (SSO) and allows transparent authentication across multiple sites. Mission Control is used to switch between different workspaces on a Mac. Explorer is a Windows feature.

### Question 3

The correct answer is **B**. The Mac feature used to back up and restore data files is Time Machine. File History and Windows Backup are features supported only on Windows operating systems. Mission Control is not used to back up or restore files.

### Question 4

The correct answer is **A**. The Force Quit menu allows unresponsive applications to be terminated. Task Manager, Task List, and Control Panel are all features available only on Windows computers.

### Question 5

The correct answer is **C**. FileVault allows a Mac hard drive to be encrypted using 128-bit or 256-bit Advanced Encryption Standard (AES). Once the feature has been enabled, the user must supply their password before the system can boot up. If no password is provided, then the disk will remain encrypted. iCloud is used to synchronize data from Apple devices to cloud-based storage. Keychain is used to manage multiple identities when accessing different sites. BitLocker is a disk encryption technology used on Windows operating systems.
