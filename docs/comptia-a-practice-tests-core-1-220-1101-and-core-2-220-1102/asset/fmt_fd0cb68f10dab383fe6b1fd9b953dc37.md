
## 7.7 Given a scenario, apply application installation and configuration concepts

### Question 4
The correct answer is **A**. `Windows Defender Firewall` can be configured to block unwanted incoming and outgoing connections. This is the most effective way to prevent the use of a P2P file-sharing application. `Windows Virus & threat protection` will protect a `Windows` operating system from malware. There are legitimate P2P file-sharing applications that will be considered malware. `Windows Secure Boot` is designed to ensure a private key cannot be tampered with. `Windows BitLocker Drive Encryption` offers protection from unauthorized access to `Windows` hard disk drives.

### Question 5
The correct answer is **C**. Only `Administrator` accounts can modify `Windows Defender Firewall` settings. `Power Users` is a group that is provided for backward compatibility and allows the same privileges as a `Standard user`. `Guest Account` has the least privileges of all accounts, and `Standard users` do not have privileges to change system configuration settings.

### Question 6
The correct answer is **C**. The current IP address of `169.154.44.89` is an Automatic Private IP Address (APIPA), which is self-assigned by the client computer. This happens if the `DHCP` service is unable to respond to a client request. It is likely that there was a temporary service disruption. `ipconfig /renew` will indicate whether the `DHCP` service is available or if the issue needs to be escalated. `ipconfig /all` shows the current network configuration. `ipconfig /flushdns` is used to resolve DNS-related problems. `ipconfig /flushdhcp` is a non-existent option.

### Question 8
The correct answer is **B**. The configuration will need to include an IP address or hostname as the printer is connected directly to the network switch. Selecting a shared printer by name would only work if the printer was already installed and shared by an existing computer on the network. The other options would not give the desired result as the printer is not wireless or connected via USB.

### Question 1
The correct answer is **C**. The `x86` architecture describes a processor that has 32 registers. This architecture means that a maximum of `4GB` of RAM can be accessed by the system. To access any additional RAM purchased would require the system to be upgraded to an `x64-bit` architecture. Installing `Ubuntu Linux` will not affect the amount of memory that can be accessed. Installing a larger hard drive will not help, and the addition of more RAM on the existing `x86` architecture will not be effective.

### Question 2
The correct answer is **B**. While the exact requirements of the `CAD` application are unknown and the base hardware seems reasonably sufficient, the exception to this is the graphics card. `CAD` applications place heavy demands on the graphics hardware and will benefit from dedicated hardware comprising a multi-core Graphics Processing Unit (`GPU`) and dedicated video RAM. As the existing hardware is of a high standard, there is no obvious requirement to upgrade anything but the integrated graphics.

### Question 3
The correct answer is **C**. 32-bit applications are able to run on 64-bit `Windows` using a compatibility setting provided by the `Windows on Windows (WOW)` architecture. They will run in a similar fashion as they would on a native 32-bit host, with the same memory restrictions (`4 GB RAM` at most). Apart from the memory restrictions, all other functionalities will still be supported, including multi-tasking and multi-threading.

### Question 4
The correct answer is **B**. In some cases, an application makes a call to the operating system to get the current version and checks to see if that matches its requirements. Often, the application fails simply because the value is not the same as its requirements when the application is told that the OS is `Windows 10`, but it expected `Windows 8`. A quick fix is to try and trick the application into believing it is running on `Windows 8` by selecting that as the compatibility setting. Updating the application to the latest version may be a long-term option, but it doesn’t offer a quick fix. `Safe mode` is used to troubleshoot faulting drivers and services but is unlikely to offer a solution to the technician’s current problem.

### Question 5
The correct answer is **B**. A hardware token can store a unique key that is used to authorize the launch of the `CAD` software. Where the value of a single license may run into several thousand dollars, this is an effective way to ensure the software is not fraudulently used. None of the other options fully protect the developers from unscrupulous users.

## 7.8 Explain common OS types and their purposes

### Question 1
The correct answer is **A**. The technician must find a filesystem compatible with both operating systems. `FAT32` is compatible with many common operating systems, including `Linux`, `macOS`, and `Windows`. `ext4` is used on `Linux`. `NTFS` and `exFAT` do not have the same wide support as `FAT32` and may not be readable by some `Linux` operating systems.

### Question 2
The correct answer is **A**. An End of Life (`EOL`) operating system no longer receives updates or security patches from the vendor. The issue is not an incompatible operating system as it still works on the hardware platform. The issue raised here is more about security and reliability issues; the fact that it is a 32-bit OS is not significant. A vendor may offer extended support for important customers (usually requiring the customer to pay for this extended support). In this instance, extended support is not being offered.

### Question 3
The correct answer is **C**. `ChromeOS` is a lightweight OS based on the popular `Chrome` browser. There are many examples of versions of minimalist OSs, such as `Windows IoT` and `Linux Lite`, while `Apple` is rumored to be testing a Lite version of `macOS` for iPads. None of these are based on a browser-based interface, however.

### Question 4
The correct answer is **C**. `Windows` offers the best solution to standardize all servers, workstations, and mobile devices across a single vendor OS. `Windows Server` offers centralized datacenter services, `Windows 11 Enterprise` can be used for managed desktops, and `Windows Mobile` can be employed for handheld devices. `Linux` is used to deliver datacenter services and is a very capable server-based platform. There are also user-friendly desktop editions such as `Ubuntu`/`Red Hat`, but these editions are not supported on smartphones or tablets. `iOS` is the `Apple` operating system used on the iPhone and iPad. `ChromeOS` is desktop only.

### Question 5
The correct answer is **D**. `Linux` operating systems use the Extended Filesystem (`ext`), the first edition of which was created around 1992 and supported storage limits of `2GB`. `ext2` was followed by `ext3` in 2001, which supported a maximum partition size of `16TB`. `ext4` was introduced around 2008 and supports a maximum partition size of `1EB` (`1,000,000 TB`). The `ext4` filesystem also supports online defragmentation, which is important to maintain efficient file access. `FAT32` is not used as a core filesystem on `Linux`. It is intended for removable media and has a maximum volume size of `32GB`. `ext3` will not support online defragmentation or the volume size required. `NTFS` is a `Windows` filesystem.
