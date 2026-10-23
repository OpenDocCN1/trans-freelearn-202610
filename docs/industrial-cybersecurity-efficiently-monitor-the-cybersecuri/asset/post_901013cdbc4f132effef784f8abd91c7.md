
```
|    Anonymous access: READ
|    \172.25.100.100\NETLOGON:
|    warning: Couldn't get details for share: NT_STATUS_ACCESS_DENIED
|    Anonymous access: <none>
|    \172.25.100.100\USERS:
|    warning: Couldn't get details for share: NT_STATUS_ACCESS_DENIED
|    Anonymous access: <none>
```

```
Nmap done: 1 IP address (1 host up) scanned in 1.25 seconds
```

As you can see, the server is not allowing anonymous enumeration of file shares, which is a good thing! I ran the command on all the ICS IP addresses that might belong to a Windows machine, and only one returned some interesting results:

```
pac@KVM010101:~$ sudo nmap 172.25.100.220 -p 445 -T3 --script smb-enum-shares
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-29 16:33 MDT
Nmap scan report for 172.25.100.220
Host is up (0.00080s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 00:0C:29:FA:9D:2F (VMware)

Host script results:
| smb-enum-shares:
|   account used: guest
|   \172.25.100.220\ADMIN$:
|     Type: STYPE_DISKTREE_HIDDEN
|     Comment: Remote Admin
|     Anonymous access: <none>
|     Current user access: <none>
|   \172.25.100.220\C$:
|     Type: STYPE_DISKTREE_HIDDEN
|     Comment: Default share
|     Anonymous access: <none>
|     Current user access: <none>
|   \172.25.100.220\IPC$:
|     Type: STYPE_IPC_HIDDEN
|     Comment: Remote IPC
|     Anonymous access: READ
|     Current user access: READ/WRITE
|   \172.25.100.220\Shared:
|     Type: STYPE_DISKTREE
|     Comment:
|     Anonymous access: <none>
|_    Current user access: READ/WRITE
Nmap done: 1 IP address (1 host up) scanned in 0.62 seconds
```

As you can see, 172.25.100.220 allows Guest accounts to connect to some shares, with the most interesting being the folder called Shared. Let's see what is stored in there by using the smbclient.py script, which is installed on our Kali Linux machine.

smbclient.py is an example script for the Impacket suite of Python classes, and it is aimed at working with network protocols (https://github.com/SecureAuthCorp/impacket). The Impacket suite has a variety of example scripts available that can be used to interact with dozens of networking protocols. This particular script allows us to interact with a Windows Server Message Block (SMB) service. SMB is what implements file and folder sharing on Windows machines:

```
pac@KVM0101011:~$ smbclient.py -no-pass guest@172.25.100.220
Impacket v0.9.21 - Copyright 2020 SecureAuth Corporation

Type help for list of commands
# shares
IPC$
ADMIN$
C$
Shared
# use Shared
# ls
drw-rw-rw-      0 Sat Aug 29 16:43:15 2020 .
drw-rw-rw-      0 Sat Aug 29 16:43:15 2020 ..
drw-rw-rw-      0 Sat Aug 29 16:34:34 2020 Very-secret-files
# cd Very-secret-files
# ls
drw-rw-rw-      0 Sat Aug 29 16:34:34 2020 .
drw-rw-rw-      0 Sat Aug 29 16:34:34 2020 ..
-rw-rw-rw-      0 Sat Aug 29 16:34:34 2020 company-passwords.txt
# get company-passwords.txt
# exit
pac@KVM0101011:~$ ls
total 1.9M
-rw-r--r-- 1 pac 0 Aug 29 16:46 company-passwords.txt
...
```

Dissecting this command, we use smbclient.py to connect to 172.25.100.220 using the guest account (guest@172.25.100.220). We omit sending a password with -no-pass.

In the preceding output, after we logged on to the 172.25.100.220 SMB service with the smbclient.py script, we enumerated the shares that had been configured (shares). Next, we connected to the shared folder (use Shared), performed a directory listing (ls), changed directories to the Very-secret-files directory (cd Very-secret-files), and downloaded the company-passwords.txt file (get company-passwords.txt). We then exited the smbclient.py script (exit) and listed the directory contents of the local (Kali machine) directory to verify that we got the goods.

Full disclosure, the file was a dud:

```
pac@KVM0101011:~$ cat company-passwords.txt
nice try
```

This exercise did not yield us any sensitive files, but it did show us that a simple misconfiguration opens the doors for someone to collect potentially sensitive data via the SMB shares that are over-exposed.

Next, we will look at finding out the OS version via SMB.

## OS detection

In the list of scripts that Nmap has, we saw a script for OS discovery called smb-os-discovery.nse. Let's put this script into action and see what we can find out about our OT-DC1 server at IP address 172.25.100.100:

```
pac@KVM0101011:~$ sudo nmap 172.25.100.100 -p 445 -T3 --script smb-os-discovery
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-29 16:53 MDT
Nmap scan report for 172.25.100.100
Host is up (0.00036s latency).

PORT     STATE SERVICE
445/tcp  open  microsoft-ds
MAC Address: 00:02:BA:D9:A7:41 (Cisco Systems)

Host script results:
| smb-os-discovery:
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: OT-DC1
|   NetBIOS computer name: OT-DC1\x00
|   Domain name: OT-Domain.local
|   Forest name: OT-Domain.local
|   FQDN: OT-DC1.OT-Domain.local
|_  System time: 2020-08-29T15:53:57-07:00

Nmap done: 1 IP address (1 host up) scanned in 0.41 seconds
```

That is quite a lot of information being provided by that one script. We now know that OT-DC1 is running on Server 2016, standard edition, revision 14393. Additionally, we can see that the server is part of the OT-Domain.local domain and that the local time is 2020-08-29T15:53:57-07:00.

We can run this script against all the servers that have port 445 open (using the Nmap-open flag) and find out the OS types and revisions for all of them:

```
pac@KVM0101011:~$ sudo nmap -iL ics_ips.txt -p 445 --open -T3 --script smb-os-discovery
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-29 17:09 MDT
Nmap scan report for 172.25.100.100
Host is up (0.00024s latency).

PORT     STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 00:02:BA:D9:A7:41 (Cisco Systems)

Host script results:
| smb-os-discovery:
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: OT-DC1
|   NetBIOS computer name: OT-DC1\x00
|   Domain name: OT-Domain.local
|   Forest name: OT-Domain.local
|   FQDN: OT-DC1.OT-Domain.local
|_  System time: 2020-08-29T16:09:33-07:00
```

Other interesting results can be seen for the 172.25.100.105 host:

```
Nmap scan report for 172.25.100.105
Host is up (0.00024s latency).

PORT     STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 00:13:20:03:3F:B8 (Intel Corporate)

Host script results:
| smb-os-discovery:
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: FT-DIR1
|   NetBIOS computer name: FT-DIR1\x00
|   Domain name: OT-Domain.local
|   Forest name: OT-Domain.local
|   FQDN: FT-DIR1.OT-Domain.local
|_ System time: 2020-08-29T16:09:33-07:00
```

Notice how the Nmap script's output verifies this host to be a Windows Server 2016 machine.

Let's look at the output for host 172.25.100.110:

```
Nmap scan report for 172.25.100.110
Host is up (0.00023s latency).

PORT     STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 00:16:6F:7E:D4:30 (Intel Corporate)

Host script results:
| smb-os-discovery:
|     OS: Windows Server 2016 Datacenter 14393 (Windows Server 2016 Datacenter 6.3)
|     Computer name: FT-DIR2
|     NetBIOS computer name: FT-DIR2\x00
|     Domain name: OT-Domain.local
|     Forest name: OT-Domain.local
|     FQDN: FT-DIR2.OT-Domain.local
|_ System time: 2020-08-29T17:09:33-06:00
```

We can see similar results here, including the OS flavor and specifics about the functionality of this host's land details, such as the domain and forest it is part of.

The next host we will look at is 172.25.100.201:

```
Nmap scan report for 172.25.100.201
Host is up (0.00019s latency).

PORT     STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 08:60:6E:FC:78:0F (Asustek Computer)

Host script results:
| smb-os-discovery:
| OS: Windows 10 Pro 15063 (Windows 10 Pro 6.3)
| OS CPE: cpe:/o:microsoft:windows_10::-
| Computer name: Workstation1
| NetBIOS computer name: WORKSTATION1\x00
| Domain name: OT-Domain.local
| Forest name: OT-Domain.local
| FQDN: Workstation1.OT-Domain.local
|_ System time: 2020-08-29T17:09:33-06:00
```

The Nmap SMB script managed to pull even more details from this machine, such as the system’s time and NetBIOS name. The final script scan results we will look at are for the 172.25.100.220 host:

```
...

Nmap scan report for 172.25.100.220
Host is up (0.00068s latency).

PORT   STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 00:0C:29:FA:9D:2F (VMware)

Host script results:
| smb-os-discovery:
| OS: Windows XP (Windows 2000 LAN Manager)
| OS CPE: cpe:/o:microsoft:windows_xp::-
| Computer name: HMI-2
| NetBIOS computer name: HMI-2\x00
| Workgroup: WORKGROUP\x00
|_ System time: 2020-08-29T17:09:32-06:00

Nmap done: 16 IP addresses (16 hosts up) scanned in 0.80 seconds
```
