
```
ControlPoint, InstanceID=22820, SymbolType=196,
DataTypeValue=196, DataType=DINT, Array=0, Struct=0,
Size=0 AccessRight=None Internal=None Meta=None Scope0=None Scope1=None Bytes=None), LgxTag(TagName=Boiler2_PressureSetpoint, InstanceID=49109, SymbolType=196,
DataTypeValue=196, DataType=DINT, Array=0, Struct=0,
Size=0 AccessRight=None Internal=None Meta=None Scope0=None Scope1=None Bytes=None), LgxTag(TagName=Program:MainProgram.test, InstanceID=46883, SymbolType=193, DataTypeValue=193,
DataType=BOOL, Array=0, Struct=0, Size=0 AccessRight=None Internal=None Meta=None Scope0=None Scope1=None Bytes=None)],
Status=Success)
```

```text
In [6]: comm.Read('Boiler2_PressureSetpoint')
Out[6]: Response(TagName=Boiler2_PressureSetpoint, Value=10, Status=Success)

In [7]: comm.Write('Boiler2_PressureSetpoint', 200)
Out[7]: Response(TagName=Boiler2_PressureSetpoint, Value=[200], Status=Success)

In [8]: comm.Read('Boiler2_PressureSetpoint')
Out[8]: Response(TagName=Boiler2_PressureSetpoint, Value=200, Status=Success)
```

As you can see, the Pylogix module allows us to connect to the ControlLogix PLC and then look up all the tags in the running program. Once this has happened, we can pick one that we want to manipulate. The same Pylogix module then allows us to read and write the tag's value. We did this successfully. The following two screenshots show the PLC tag values before and after we manipulated the boiler pressure setpoint.

The value of the sensitive Boiler2_PressureSetpoint before we manipulated it via Pylogix is as follows:

![](img/c8ffa9959721097701584fcc95101362_288_0.png)

Figure 7.6 – Boiler2_PressureSetpoint before Pylogix manipulation

The value of the sensitive Boiler2_PressureSetpoint after we manipulated it via Pylogix is as follows:

![](img/c8ffa9959721097701584fcc95101362_288_1.png)

Figure 7.7 – Boiler2_PressureSetpoint after Pylogix manipulation

As you can see, even modern automation controllers suffer from legacy vulnerabilities. The ControlLogix platform, or more precisely the FactoryTalk environment (FactoryTalk security), allows us to set up strict control over who can program or even view controller data. However, FT security is cumbersome to set up and is often not implemented.

Finally, I wish to point out that I am not singling out PLCs/protocols – most major PLC brands and automation protocols suffer from the same inherently vulnerable design flaws that we saw with Modbus and EtherNet/IP, and they are easy to manipulate/interrogate by unauthorized individuals as soon as those individuals are on the right network segment (our IDMZ should help secure things). The major manufacturers have started to implement additional controls to mitigate some of the effects of these vulnerabilities. Things are looking up, but it will take a long time, if ever, before we see those fundamental design flaws in ICS equipment be completely remediated.

To finish our industrial protocol scanning activities, we will close this section by extracting information from a Siemens PLC.

## Scanning Siemens S7 (iso-tsap)

Siemens S7 PLCs use the `iso-tsap` protocol to communicate to other PLCs and a programming terminal. The protocol runs on a service that typically listens on port `102`. If you recall from our scan, we had one system with port `102` open. Let’s look at what system that was with the following command:

```
pac@KVM0101011:~$ cat ics-services.txt | grep 102 -B5

Nmap scan report for 172.25.100.20
Host is up (0.00055s latency).
Not shown: 65534 closed ports
PORT    STATE SERVICE VERSION
102/tcp open  iso-tsap Siemens S7 PLC
```

Now that we know `172.25.100.20` responds to queries on port `102`, let’s see what kind of information we can get from it. We are going to use another Nmap script, written to get information from S7 PLCs:

```
pac@KVM0101011:~$ ls /usr/share/nmap/scripts/ | grep -e s7

-rw-r--r-- 1 root root 9.8K Jul 13 03:03 s7-info.nse
```

Putting things together, we target the system with the following command:

```
pac@KVM0101011:~$ nmap -T3 -p 102 --script s7-info 172.25.100.20
Starting Nmap 7.80 (https://nmap.org) at 2020-09-01 16:29 MDT
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

As you can see, Nmap once again manages to deliver the information we need to expand our asset database or use it in order to discover system vulnerabilities.

That's it for the discovery section! You can now take the information you've learned here and look for known vulnerabilities.

## Manual vulnerability verification

As an example of this, let's take the Rockwell Communications adapter that was discovered running at 172.25.100.11:

```
44818/tcp open  EtherNet-IP-2
| enip-info:
|   type: Communications Adapter (12)
|   vendor: Rockwell Automation/Allen-Bradley (1)
|   productName: 1756-EN2T/B
| serialNumber: 0x005eae98
| productCode: 166
| revision: 5.28
| status: 0x0030
| state: 0x03
|_ deviceIp: 172.25.100.11
MAC Address: 00:00:BC:5A:D0:56 (Rockwell Automation)
```

We can take that information to an online service such as CVE Details (https://www.cvedetails.com/) and do a search on 1756-en2t:

![](img/c8ffa9959721097701584fcc95101362_291_0.png)

Figure 7.8 – CVE Details – 1756-en2t results

Now, we can see whether there are any known issues with the device.

This is a very labor-intensive process and prone to missing vulnerabilities or the discovery of false positives. Therefore, next, we are going to look at how to automate this process with well-known vulnerability scanning tools.

## Scanning for vulnerabilities

As we discussed in Chapter 6, Passive Security Monitoring, there are tools we can implement that will use packet capturing (sniffing) technology to detect the systems on our ICS network and start indexing the services and versions/revisions of those services, allowing the tool to detect vulnerabilities. As we are operating around a layered defense model (defense in depth), we should not rely on just one tool or technique to keep us safe. This passive tool is only as good as the data we (can) feed it. If our OT-IDS is not seeing packets for something, it cannot verify vulnerabilities. Therefore, we should make sure to periodically verify things by scanning the network for vulnerabilities. In this section, we will discuss three common tools that can help us perform vulnerability scanning, namely, Nmap, Nikto, and Nessus.

### Using Nmap to scan for vulnerabilities in network services

Nmap can be used to scan for vulnerabilities that might be present in network services. Vulnerability scanning via Nmap is implemented via scripts. Let’s take a look at the available vulnerability scanning scripts with the following command:

```
pac@KVM0101011:~$ ls /usr/share/nmap/scripts/ | grep -e vuln
-rw-r--r-- 1 root root 6.9K Jul 13 03:03 afp-path-vuln.nse
-rw-r--r-- 1 root root 5.8K Jul 13 03:03 ftp-vuln-cve2010-4221.nse
-rw-r--r-- 1 root root 6.9K Jul 13 03:03 http-huawei-hg5xxx-vuln.nse
-rw-r--r-- 1 root root 4.2K Jul 13 03:03 http-vuln-wnr1000-creds.nse
-rw-r--r-- 1 root root 6.9K Jul 13 03:03 mysql-vuln-cve2012-2122.nse
-rw-r--r-- 1 root root 8.7K Jul 13 03:03 rdp-vuln-ms12-020.nse
-rw-r--r-- 1 root root 4.0K Jul 13 03:03 rmi-vuln-classloader.nse
-rw-r--r-- 1 root root 6.4K Jul 13 03:03 rsa-vuln-roca.nse
-rw-r--r-- 1 root root 4.1K Jul 13 03:03 samba-vuln-cve-2012-1182.nse
-rw-r--r-- 1 root root 5.2K Jul 13 03:03 smb2-vuln-uptime.nse
-rw-r--r-- 1 root root 7.4K Jul 13 03:03 smb-vuln-conficker.nse
-rw-r--r-- 1 root root 5.3K Jul 13 03:03 smb-vuln-ms07-029.nse
-rw-r--r-- 1 root root 5.6K Jul 13 03:03 smb-vuln-ms08-067.nse
-rw-r--r-- 1 root root 5.6K Jul 13 03:03 smb-vuln-ms10-054.nse
-rw-r--r-- 1 root root 7.1K Jul 13 03:03 smb-vuln-ms10-061.nse
-rw-r--r-- 1 root root 7.2K Jul 13 03:03 smb-vuln-ms17-010.nse
-rw-r--r-- 1 root root 15K Jul 13 03:03 smtp-vuln-cve2010-4344.nse
-rw-r--r-- 1 root root 7.6K Jul 13 03:03 smtp-vuln-cve2011-1720.nse
```
