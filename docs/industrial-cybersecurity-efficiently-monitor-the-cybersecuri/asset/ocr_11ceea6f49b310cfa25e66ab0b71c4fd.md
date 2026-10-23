We can now identify the system under suspicion and either physically log on or try to connect via remote desktop. Either way, we are going to have to connect to the HMI-2 machine.

# Find the beaconing process – netstat

It is time we find out what exactly is performing the beaconing to very-malicious-website .com. Note that for this I used a remote desktop session to HMI-2.

Figuring out a beaconing process is a bit tough as there isn’t a continuous connection to uncover with a tool such as netstat (run from Command Prompt):

```
C:\Documents and Settings\Administrator>netstat -anb

Active Connections

Proto  Local Address      Foreign Address    State      PID
...
TCP    0.0.0.0:135        0.0.0.0:0          LISTENING  956
C:\\windows\\system32\WS2_32.dll
C:\\WINDOWS\\system32\RPCRT4.dll
c:\windows\\system32\rpcss.dll
C:\\WINDOWS\\system32\svchost.exe
-- unknown component(s) --
[svchost.exe]
...
TCP    172.25.100.220:1036 172.25.100.250:22 ESTABLISHED 1300
[ telnet.exe]
...
```

The output shows **ESTABLISHED** network connections to and from the HMI-2 computer (`telnet.exe` connected to `172.25.100.250` in the example). Netstat will also show connections that were just disconnected (**CLOSING** and **CLOSE-WAIT**) and **LISTENING** (connections for network services that might be running). See `https://www.ibm.com/support/knowledgecenter/en/SSTLBW_2.1.0/com.ibm.zos.v2r1.halu101/constatus.hf?cmmc=263,264,265,271,419,420,421#13` for more details on all the states netstat can report. However, a beacon connection is very short-lived and unless we are lucky enough to capture the connection to the command and control server in a connected state or shortly after disconnecting, we cannot use the netstat tool to find our culprit process.