
Again, the output gives us valuable information for the host that was scanned.

We now know the details of every Windows OS running on our ICS network. This gives us a great start with our asset inventory list as we can parse this information into an Excel sheet or a database. Now, we can start filling in details such as installed software, files, users, and whatever else we can find out.

There are many more SMB scripts we can use, and many of them will have limited results on modern Windows versions. I will leave it up to you as a homework assignment to play with these scripts. We will close out this section on SMB with an Nmap scan that requests the user list from all our Windows machines on the ICS network.

## Users

Knowing what users run on a system is the first step for an attacker to crack credentials. So, if any of our machines makes this too easy to find out, we want to know about that. The smb-enum-users.nse Nmap script allows us to find out whether any of our systems are exposing user details. To run it on all our windows machines, we can use the following command:

```
pac@KVM0101011:~$ sudo nmap -iL ics_ips.txt -p 445 --open -T3 --script smb-enum-users
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-29 17:25 MDT
Nmap scan report for 172.25.100.100
Host is up (0.00020s latency).

PORT     STATE SERVICE
445/tcp  open  microsoft-ds
MAC Address: 00:02:BA:D9:A7:41 (Cisco Systems)

Nmap scan report for 172.25.100.105
Host is up (0.00017s latency).

PORT     STATE SERVICE
445/tcp  open  microsoft-ds
MAC Address: 00:13:20:03:3F:B8 (Intel Corporate)
...
Nmap scan report for 172.25.100.220
```

```
Host is up (0.00086s latency).

PORT     STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 00:0C:29:FA:9D:2F (VMware)

Host script results:
|_smb-enum-users: ERROR: Script execution failed (use -d to debug)

Nmap done: 16 IP addresses (16 hosts up) scanned in 0.79 seconds
```

None of our systems are returning user account information. This is a good thing. There are other ways an attacker can enumerate users from a Windows machine, though. One such way is via NULL sessions over RPC. Here is an example of the method being performed from our Kali Linux machine, targeting the OT-DC1 server:

```
pac@KVM0101011:~$ sudo rpcclient -U "" 172.25.100.100
Enter WORKGROUP\'s password:
rpcclient $> getusername
Could not initialise lsarpc. Error was NT_STATUS_ACCESS_DENIED
rpcclient $> enumdomusers
result was NT_STATUS_ACCESS_DENIED
rpcclient $> lsaquery
Could not initialise lsarpc. Error was NT_STATUS_ACCESS_DENIED
rpcclient $>
...
```

As you can see, although the server allows us to anonymously connect to the RPC service, it gives us an access denied error on any subsequent queries. RpcClient is a fantastic tool that can reveal all sorts of information, so I highly recommend that you play around with it and see whether any of the other systems on the network are more willing to reveal information.

Next, we will be switching gears and seeing how we can start interrogating industrial protocols/services, starting with Modbus.

## Exploring Modbus

As you may recall from our network scans, 172.25.100.21 showed TCP port 502 open, indicating a Modbus-capable service is present and active. Let's see what exactly is listening on that port by using Nmap scripts. If we do a search among the Nmap scripts, we will see that there is only a single script concerning Modbus:

```
pac@KVM0101011:~$ ls /usr/share/nmap/scripts/ | grep modbus
-rw-r--r-- 1 root 5.9K Jul 13 03:03 modbus-discover.nse
```

This one script is exactly what we need to figure out what is running on open TCP port 502. The following Nmap command can be used to unearth the application behind the open port:

```
pac@KVM0101011:~$ nmap -p 502 -T3 172.25.100.21 --script modbus-discover
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-31 15:13 MDT
Nmap scan report for 172.25.100.21
Host is up (0.00096s latency).

PORT     STATE SERVICE
502/tcp open  modbus
| modbus-discover:
|   sid 0x1:
|     Slave ID data: Pymodbus-PM-2.3.0\xFF
|_    Device identification: Pymodbus PM 2.3.0
MAC Address: 00:20:4A:64:1D:3F (Pronet Gmbh)

Nmap done: 1 IP address (1 host up) scanned in 0.40 seconds
```

Here, we can see that the service is Pymodbus PM 2.3.0, which is the Python Modubus implementation we have running on our lab equipment. If we look at the log on the machine running the Pymodbus service, we can find out how Nmap discovered the details shown previously:

```
root@ubuntu:/home/pac/workdir/python/modbus# python3 ./modbus-server.py
2020-08-31 14:13:19,327 MainThread      DEBUG    sync :347    Started thread to serve client at ('172.25.100.222', 60272)
2020-08-31 14:13:19,328 Thread-1         DEBUG    sync
:46          Client Connected [172.25.100.222:60272]
2020-08-31 14:13:19,329 Thread-1          DEBUG      sync
:199          Handling data: 0x0 0x0 0x0 0x0 0x2 0x1 0x11
2020-08-31 14:13:19,329 Thread-1          DEBUG      socket_framer
:147          Processing: 0x0 0x0 0x0 0x0 0x2 0x1 0x11
2020-08-31 14:13:19,329 Thread-1          DEBUG      factory
:137          Factory Request [ReportSlaveIdRequest: 17]
2020-08-31 14:13:19,330 Thread-1          DEBUG      sync      :229      send:
[ResportSlaveIdResponse(17, b'Pymodbus-PM-2.3.0', True)]-b'00000000001501111250796d6f646275732d504d2d322e332e30ff'
2020-08-31 14:13:19,332 Thread-1          DEBUG      sync
:199          Handling data:
2020-08-31 14:13:19,332 Thread-1          DEBUG      socket_framer
:147          Processing:
2020-08-31 14:13:19,332 Thread-1          DEBUG      sync
:54           Client Disconnected [172.25.100.222:60272]
2020-08-31 14:13:19,333 MainThread          DEBUG      sync
:347          Started thread to serve client at ('172.25.100.222', 60274)
2020-08-31 14:13:19,334 Thread-2          DEBUG      sync
:46           Client Connected [172.25.100.222:60274]
2020-08-31 14:13:19,334 Thread-2          DEBUG      sync
:199          Handling data: 0x0 0x0 0x0 0x0 0x5 0x1 0x2b 0xe 0x1 0x0
2020-08-31 14:13:19,335 Thread-2          DEBUG      socket_framer
:147          Processing: 0x0 0x0 0x0 0x0 0x5 0x1 0x2b 0xe 0x1 0x0
2020-08-31 14:13:19,335 Thread-2          DEBUG      factory
:137          Factory Request [ReadDeviceInformationRequest: 43]
2020-08-31 14:13:19,335 Thread-2          DEBUG      sync
:229          send: [ReadDeviceInformationResponse(1)]- b'00000000 001d012b0e0183000003000850796d6f646275730102504d0205322e332e30'
2020-08-31 14:13:19,337 Thread-2          DEBUG      sync
:199          Handling data:
2020-08-31 14:13:19,337 Thread-2          DEBUG      socket_framer
:147          Processing:
2020-08-31 14:13:19,338 Thread-2          DEBUG      sync
:54           Client Disconnected [172.25.100.222:60274]
```

Our Kali Linux machine (**172.25.100.222**) connects to the Modbus service on two occasions. The first time, we are asking for a `SlaveIDRequest` with a value of 17, followed by a request for `DeviceInformationRequest` with a value of 43. This is enough data for Nmap to give us the results we read earlier.

Although the information returned by Nmap (vendor, type, version, and revision) would be enough for us to figure out whether there are any known vulnerabilities for this system, I would like to point out that there are some out-of-the-box vulnerabilities baked into the Modbus protocol. These vulnerabilities stem from two architectural flaws in the Modbus protocol: it doesn't supply transport encryption and it doesn't mandate authentication for communications. With these two fundamental design flaws, an attacker can sniff the network, learn about (critical) memory registers, and read/write to and from those registers. To illustrate the impact of these vulnerabilities, the following example shows an attacker sniffing the read of a register (Marker word) on an industrial network via Wireshark:

![](img/c8ffa9959721097701584fcc95101362_279_0.png)

Notice how in the preceding screenshot, **172.25.100.222** can be seen requesting a register value in the nr. 4 packet and the Modbus service (**172.25.100.21**) sending a response in *packet 6* (highlighted in the preceding screenshot). Among the packet details, we can see the register’s number (**10**), as well as its value (**34**).

With this information, an attacker can now instruct the Modbus service to change this value to his or her liking. One possible tool we can do this with is **modbus-cli**, which will be explained next.
