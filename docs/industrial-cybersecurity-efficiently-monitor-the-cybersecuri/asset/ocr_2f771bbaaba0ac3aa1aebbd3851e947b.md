- services:

```
msf6 > services
Services
========

host          port  proto  name    state  info
----          ----  -----  ----    -----  ----
172.25.200.11  68    udp                open
172.25.200.11  80    tcp    www     open
172.25.200.11  161   udp    snmp    open
172.25.200.11  319   udp                open
172.25.200.11  320   udp                open
172.25.200.11  2222  udp                open
172.25.200.11  44818 tcp    unirp c open
172.25.200.11  44818 udp                open
172.25.200.12  68    udp                open
172.25.200.12  80    tcp    www     open
172.25.200.12  161   udp    snmp    open
172.25.200.12  319   udp                open
172.25.200.12  320   udp                open
172.25.200.12  2222  udp                open
172.25.200.12  44818 tcp    unirp c open
172.25.200.12  44818 udp                open
172.25.200.15  80    tcp    www     open
172.25.200.15  44818 tcp    unirp c open
172.25.200.20  44818 tcp                open
172.25.200.21  502   tcp                open
```

Figure 17.21 – Discovered services (ports)

- vulns:

```
msf6 > vulns
Vulnerabilities
=============

Timestamp           Host           Name                                   References
---------           ----           ----                                   ----------
2021-04-01 21:16:37 UTC 172.25.200.21 Common Platform Enumeration (CPE)      NSS-45590
2021-04-01 21:16:37 UTC 172.25.200.21 Nessus Scan Information                NSS-19506
2021-04-01 21:16:37 UTC 172.25.200.21 IP Protocols Scan                      NSS-14788
2021-04-01 21:16:37 UTC 172.25.200.21 Device Type                            NSS-54615
2021-04-01 21:16:37 UTC 172.25.200.21 OS Identification                      NSS-11936
2021-04-01 21:16:37 UTC 172.25.200.21 ICMP Timestamp Request Remote Date Disclosure CVE-1999-0524,CWE-200,NSS-10114
2021-04-01 21:16:37 UTC 172.25.200.21 TCP/IP Timestamps Supported            NSS-25220
2021-04-01 21:16:37 UTC 172.25.200.21 Traceroute Information                 NSS-10267
```

Figure 17.22 – Discovered vulnerabilities