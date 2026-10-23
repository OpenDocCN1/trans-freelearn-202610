Putting things together, we target the system with the following command:

```
pac@KVM0101011:~$ nmap -T3 -p 102 --script s7-info 172.25.100.20
Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-01 16:29 MDT
Nmap scan report for 172.25.100.20

Host is up (0.0013s latency).
PORT     STATE SERVICE
102/tcp open  iso-tsap
| s7-info:
|   Module: 6ES7 315-2EH14-0AB0
|   Basic Hardware: 6ES7 315-2EH14-0AB0
|   Version: 3.2.6
|   System Name: SNAP7-SERVER
|   Module Type: CPU 315-2 PN/DP
|   Serial Number: S C-C2UR28922012
|_  Copyright: Original Siemens Equipment
MAC Address: 00:1C:06:00:BC:37 (Siemens Numerical Control, Nanjing)
Service Info: Device: specialized
Nmap done: 1 IP address (1 host up) scanned in 0.41 seconds
```

As you can see, Nmap once again manages to deliver the information we need to expand our asset database or use in order to discover system vulnerabilities.

That's it for the discovery section! You can now take the information you've learned here and look for known vulnerabilities.

# Manual vulnerability verification

As an example of this, let's take the Rockwell Communications adapter that was discovered running at 172.25.100.11:

```
44818/tcp open  EtherNet-IP-2
| enip-info:
|   type: Communications Adapter (12)
|   vendor: Rockwell Automation/Allen-Bradley (1)
|   productName: 1756-EN2T/B
```