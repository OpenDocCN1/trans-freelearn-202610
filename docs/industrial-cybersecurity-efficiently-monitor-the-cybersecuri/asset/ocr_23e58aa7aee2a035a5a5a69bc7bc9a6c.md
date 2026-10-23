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