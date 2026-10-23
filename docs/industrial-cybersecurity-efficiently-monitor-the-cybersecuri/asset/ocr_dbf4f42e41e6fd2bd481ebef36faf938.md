The next Volatility plugin we will cover shows us sockets - sockets:

```
remnux@remnux:~/workdir$ vol.py -f HMI-2.vmem
--profile=WinXPSP2x86 sockets
Volatility Foundation Volatility Framework 2.6.1
Offset(V)      PID  Port  Proto Protocol          Address
Create Time
---------- ---------- ------ ------ ------------ ------------
--- --- 
0x822d4b40      708    500    17 UDP             0.0.0.0
2020-09-24 01:00:26 UTC+0000
0x81b4d9a8     1152   1900    17 UDP
172.25.100.220 2020-09-24 01:00:31 UTC+0000
0x822c4e98        4    445     6 TCP             0.0.0.0
2020-09-24 01:00:17 UTC+0000
0x81b68390      956    135     6 TCP             0.0.0.0
2020-09-24 01:00:18 UTC+0000
0x81f9e6e8        4    139     6 TCP
172.25.100.220 2020-09-24 01:00:18 UTC+0000
...
0x81b5fe98      708   4500    17 UDP             0.0.0.0
2020-09-24 01:00:26 UTC+0000
0x81b62570     1300   1036     6 TCP
172.25.100.250          2020-09-26 17:15:42 UTC+0000
0x821a7128        4    445    17 UDP             0.0.0.0
2020-09-24 01:00:17 UTC+0000
```

The command shows us (active) network connections (sockets) the HMI-2 computer had established at the time we created the memory image. Most are internal (loopback) connections, but we can also see a connection to 172.25.100.250 (the Security Onion VM) over TCP port 22 (SSH), created at 9/26 at 17:15:42, by process with ID 1300 (the telnet.exe process we saw earlier).

A second way to find active connections in the memory dump file is with the connections command:

```
remnux@remnux:~/workdir$ vol.py -f HMI-2.vmem
--profile=WinXPSP2x86 connections
Volatility Foundation Volatility Framework 2.6.1
Offset(V)  Local Address          Remote Address
Pid
```