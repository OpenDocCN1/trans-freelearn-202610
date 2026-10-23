
# msinfo32

The first technique for grabbing a large variety of information from a Windows machine is centered around a tool that has been built into Windows OSes since the Windows XP days: msinfo32.exe.

To invoke this tool, we simply open the Run dialog box with Windows Key + R and type in msinfo32:

![](img/c8ffa9959721097701584fcc95101362_307_0.png)

Figure 7.20 – Starting msinfo32

Hit Enter. This will bring up msinfo32.exe – the System Information application:

![](img/c8ffa9959721097701584fcc95101362_307_1.png)

Figure 7.21 – Microsoft System Information utility

As you can see, the System Information tool has sections on Hardware, Software, Components, and Internet Settings. You can click through and browse all the subsections and even search for certain information.

The following areas are of particular interest to us.

## System Summary

The initial view the System Information tools opens with gives us details about the computer:

![](img/c8ffa9959721097701584fcc95101362_308_0.png)

Figure 7.22 – msinfo32 – System Summary

There is OS information, BIOS details, hardware summaries, and environment variables to be discovered on this page.

## System Drivers

If we navigate to **Software Environment | System Drivers**, we can see all the drivers that are present on the system:

| System Summary | Name | Description | File | Type | Started |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Hardware Resources** | \| | | | | | | | | |
| **Components** | \| | | | | | | | |
| **Software Environment** | \| | | | | | | | |
| System Drivers | \| | | | | | | | |
| Signed Drivers | \| | | | | | | | |
| Environment Variables | \| | | | | | | | |
| Print Jobs | \| | | | | | | | |
| Network Connections | \| | | | | | | | |
| Running Tasks | \| | | | | | | | |
| Loaded Modules | \| | | | | | | | |
| Services | \| | | | | | | | |
| Program Groups | \| | | | | | | | |
| Startup Programs | \| | | | | | | | |
| OLE Registration | \| | | | | | | | |
| Windows Error Reporting | \| | | | | | | | |
| Internet Settings | \| | | | | | | | |
| | | | | | | | | |
| | ide | Floppy Disk Controller Driver | C:\windows\system32\drivers\iide.sys | Kernel Driver | No |
| | fastfat | FastFAT | c:\windows\system32\drivers\fastfat.sys | File System Driver | No |
| | dmload | dmload | c:\windows\system32\drivers\dmload.sys | Kernel Driver | Yes |
| | dmio | Logical Disk Manager Driver | c:\windows\system32\drivers\dmio.sys | Kernel Driver | Yes |
| | dmboot | dmboot | c:\windows\system32\drivers\dmboot.sys | Kernel Driver | No |
| | disk | Disk Driver | c:\windows\system32\drivers\disk.sys | Kernel Driver | Yes |
| | compbatt | Microsoft Composite Battery Driver | c:\windows\system32\drivers\compbatt.sys | Kernel Driver | Yes |
| | cmibatt | Microsoft AC Adapter Driver | c:\windows\system32\drivers\cmibatt.sys | Kernel Driver | Yes |
| | cdrom | CD-ROM Driver | c:\windows\system32\drivers\cdrom.sys | Kernel Driver | Yes |
| | cd21 | Cd21 | c:\windows\system32\drivers\cd21.sys | File System Driver | Yes |
| | cdudaud | Cduaduo | c:\windows\system32\drivers\cduaduo.sys | Kernel Driver | No |
| | ccbd2k | ccbd2k | c:\windows\system32\drivers\ccbd2k.sys | Kernel Driver | No |
| | beep | Beep | c:\windows\system32\drivers\beep.sys | Kernel Driver | Yes |
| | audstub | Audio Stub Driver | c:\windows\system32\drivers\audstub.sys | Kernel Driver | Yes |
| | atmarp | ATM ARP Client Protocol | c:\windows\system32\drivers\atmarp.sys | Kernel Driver | No |
| | alapi | Standard IDE/ESDI Hard Disk Control... | c:\windows\system32\drivers\alapi.sys | Kernel Driver | Yes |
| | asycmaca | RAS Asynchronous Media Driver | c:\windows\system32\drivers\asycmaca.sys | Kernel Driver | No |
| | agm440 | Intel A3P Bus Filter | c:\windows\system32\drivers\agm440.sys | Kernel Driver | Yes |
| | afd | AFD | c:\windows\system32\drivers\afd.sys | Kernel Driver | Yes |
| | acpiec | ACPIEC | c:\windows\system32\drivers\acpiec.sys | Kernel Driver | No |
| | aepi | Microsoft ACPI Driver | c:\windows\system32\drivers\aepi.sys | Kernel Driver | Yes |
| | vmvmmid1 | Memory Control Driver | /*?AC_Virtual_ACPI_Common File*/vmware\vdrv | Kernel Driver | Yes |
| | aibddsk | Aibddsk | Not Available | Kernel Driver | No |
| | abp450v5 | abp450v5 | Not Available | Kernel Driver | No |
| | aodpl160m | aodpl160m | Not Available | Kernel Driver | No |
| | aha154x | Aha154x | Not Available | Kernel Driver | No |
| | aic79ud2 | aic79ud2 | Not Available | Kernel Driver | No |
| | aic79x | aic79x | Not Available | Kernel Driver | No |
| | aide | Aide | Not Available | Kernel Driver | No |
| | amimnt | amimnt | Not Available | Kernel Driver | No |
| | asc | asc | Not Available | Kernel Driver | No |
| | asc3550p | asc3550p | Not Available | Kernel Driver | No |
| | asc3550 | asc3550 | Not Available | Kernel Driver | No |
| | atdisk | Atdisk | Not Available | Kernel Driver | No |
| | cd20mnt | cd20mnt | Not Available | Kernel Driver | No |
| | charger | Charger | Not Available | Kernel Driver | No |
| | cmdide | cmdide | Not Available | Kernel Driver | No |

Figure 7.23 - msinfo32 - System Drivers

I typically sort the view by **File** (path) and see whether there are any oddball location drivers that have been installed. For example, having a kernel driver start from the `C:\windows\temp` folder is highly suspicious. Malware will sometimes register kernel drivers to attain persistence on a system and then compare the list of installed/started drivers as a routine check.

## Services

Processes (applications) that have been configured to automatically start when the system boots or a user logs in are called services. We can pull up the currently configured list of services by navigating to Software Environment | Services:

![](img/c8ffa9959721097701584fcc95101362_310_0.png)

Malware will often create a service as part of the infection process, to establish persistence on the system so that the malware starts along with the computer. Comparing snapshots of installed and running services over time, or against similar systems, often reveals infections by malware.

## Running processes

Processes (or tasks) are applications (program files) mapped in memory that run code and perform tasks. So, if we look at our processes, we can get a picture of the tasks our system is performing. To get a list of processes that were running at the time the System Information tool grabbed the information, we can go to Software Environment | Running Tasks:

![](img/c8ffa9959721097701584fcc95101362_311_0.png)

Figure 7.25 – msinfo32 – Running Processes (Tasks)

Again, it is common practice to sort this list by File (path) to identify any strange locations the processes are started from.

Comparing running process dumps over time or between similar systems can reveal infections by malware or other unwanted software. This is particularly true for ICS systems where computers tend to run the same set of applications for long periods of time. Changes to the set of running processes stand out like a sore thumb.

## Startup Programs

Along with services, Windows provides a variety of methods we can use to automatically start an application at system boot or user login. The System Information tool shows applications that start automatically by means of some of those methods. We can view these applications by navigating to **Software Environment | Startup Programs**:

![](img/c8ffa9959721097701584fcc95101362_311_1.png)

Figure 7.26 – msinfo32 – Startup Programs

This view shows the program's name, the start up command, the user account being used by the program, and the start method being used to automatically start the program. Again, grabbing snapshots of Startup Programs and comparing them over time or against similar systems can reveal all kinds of evil.

## Export report

To create a record of this information so that we can compare it at a later date, we can export the System Information report via the File | Export menu option. Make sure that you have selected the System Summary location before exporting, as this guarantees all the information is exported:

![](img/c8ffa9959721097701584fcc95101362_312_0.png)

Figure 7.27 – msinfo32 – Export report

And with that, we now have a record of the system information and system state of the HMI-1 computer. You should store it in a secure location on the network for safe keeping and easy retrieval.
