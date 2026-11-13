
## macOS Practice Questions

### 6. The correct answer is **C**
The Mac operating system (beginning with Mac OS X in 1996) was developed as a custom operating system based on Berkeley Software Distribution (BSD) Unix. macOS is not based on Linux, Android, or ChromeOS.

### 7. The correct answer is **C**
The Keychain feature allows a user to store passwords and account information that can then be used to reduce the number of passwords that a user would otherwise need to remember. Mission Control is used to switch between applications and desktops. Spotlight is used for system-wide searches (such as searching for an application, email, or files) on a macOS machine. Dock is used to quickly navigate to applications and features.

### 8. The correct answer is **A**
The easiest way for this task to be accomplished is to locate the application and delete the application folder. None of the other options will give the desired result.

### 9. The correct answer is **A**
In the event that a Mac computer fails to boot, it is likely that one or more critical files on the hard disk are missing or damaged. To access the Recovery menu, `Command + R` must be pressed during the boot process. The recovery options can then be accessed to attempt to fix the error. Do not attempt to reinstall the operating system until all other options have been attempted. Important data files could be lost if they have not been backed up. The Recovery menu cannot be accessed by booting the computer from a DVD, not to mention that most modern Macs do not include a DVD drive. Safe mode is a feature of the Windows operating system.

### 10. The correct answer is **A**
To permanently delete the files and free up disk space, the Trash application should be emptied. Recycle Bin and Disk Cleaner are Windows features. Separating a disk into separate partitions with Disk Utility will not increase the available disk space.

### 11. The correct answer is **B**
The Dock is normally displayed at the bottom of the screen and is a convenient place through which to access features, applications, folders, and files. The Dock will display recently used applications by default but can be customized by dragging an item onto the Dock to create a persistent link. Mission Control allows users to switch between different workspaces. The Start menu is a Microsoft Windows feature. Toolbars are used to access features within an application.

### 12. The correct answer is **B**
System Preferences allows the configuration and customization of all common configuration preferences. Mission Control allows users to switch between different workspaces. Dock offers easy access to applications and files. Disk Utility is a system tool that handles disk-specific tasks on Mac computers.

### 13. The correct answer is **A, C**
To avoid an overly cluttered desktop due to a large number of running applications, it is possible for a Mac user to create additional desktops. These desktops are known as spaces. The user can switch between them either by using `Control + right or left arrow keys` or by adding and/or selecting another desktop in Mission Control. Desktops (or spaces) cannot be accessed directly from the Dock. `Command + spacebar` is used to call up the Spotlight search function.

## 7.11 Identify Common Features and Tools of the Linux Client/Desktop OS

### 1. The correct answer is **B, C**
To list files in a directory, the `ls` (list) command with the `-l` switch (for long listing) would be used to show all permissions for the contents of the directory. To show all possible options for the `ls` command, `ls --help` would be used. `sudo chown` would allow the ownership of the `Employee-Reports` file to be changed from sales group ownership to the `HumanResources` group. Looking at the other permissions that have been allocated, it seems likely that the sales group has incorrectly been given group ownership of the file. `dir` is a Windows command and would show the same output. `sudo chown` would allow the ownership of a file to be changed. `sudo chmod` would reset permissions on a file or directory. `pwd` (present working directory) will show the directory where commands are currently being processed. `sudo mv` will move a file to a different location.

### 2. The correct answer is **D**
Using the `sudo` command, a user can access the privileges of the superuser account. They will be prompted to type their own user password. It is also possible to switch user to an account that’s alive with the `su` command. For more information on both these commands, use the given command along with `--help` switch. `pwd` displays the present working directory. `grep` is used to search for text strings. `Samba` is an interoperability suite that allows Windows platforms to access shared files from a Linux computer.

### 3. The correct answer is **C**
The Linux `top` command shows real-time active processes. `PID` is the Process ID number, `USER` is the owner of the task, `PR` and `NI` show the priority of the task, and `VIRT` and `RES` show memory usage. Other important values are `%CPU`, which shows CPU usage, and `%MEM`, which shows the memory being used per process. `nano` is a text editor on Linux systems. `dig` is used to send DNS queries to a Name Server. `ps` is used to list running processes on a Linux system but does not display the details of resources being used to the same extent as the `top` command.

### 4. The correct answer is **D**
The `rm` command (also known as the remove command) is used to delete files and directories from a Linux system. The command supports additional switches, such as `-r` (recursive), that allow for directories to be deleted along with files. Type `rm --help` to see all options. `chmod` allows for permissions on files and directories to be changed. `man` is used to display the manual for a command (for example `man rm` will display verbose information for the remove command). `df` is the disk-free command and displays filesystem information.

### 5. The correct answer is **C**
The output from `df` shows the developer how much unused space remains on the storage location (hard disk). `ps` and `top` display the running processes. `ip` is used to configure settings for network interfaces.

### 6. The correct answer is **B**
`SSH` (secure shell) will allow the technician to access a remote terminal session using a secure link. `SSH` uses symmetric encryption to protect the confidentiality of any data transferred. `RDP` is used on the Windows operating system to access the desktop of a Windows host. `telnet` is not secure; a malicious user may be able to intercept the network traffic and access details of the data. The `cp` command can be used to copy files between different Linux hosts but is not secure.

### 7. The correct answer is **C**
The `man` command displays help in the form of a manual, usually offering much more detail than the corresponding `--help` command when typed after the `CLI` command. The other explanations are false.

### 8. The correct answer is **D**
Installing the `samba` service on the Linux Ubuntu server will allow Windows computers to use native services and applications to access files and printers on the Linux server in the same way that they connect to other Windows computers. Windows uses a Microsoft protocol called Server Message Block (`SMB`), also known as Common Internet File System (`CIFS`) to access shared content across a network. `samba` supports the same protocols. `telnet` is used to access the `CLI` of a remote system but does not allow files to be transferred between systems. Windows 10 workstations do not run the `samba` service, as they already support the network protocols needed to create a persistent connection to shared resources using a mapped drive. Windows File and Print Sharing is native to Windows operating systems and cannot be installed on Linux systems.
