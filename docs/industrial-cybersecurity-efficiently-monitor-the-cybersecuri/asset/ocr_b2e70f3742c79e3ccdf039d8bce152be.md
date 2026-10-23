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
```