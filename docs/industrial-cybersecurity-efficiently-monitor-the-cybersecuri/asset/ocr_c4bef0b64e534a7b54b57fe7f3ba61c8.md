Running the enip-enum script against 172.25.100.12 resulted in almost identical details, but running the script against 172.25.100.23 revealed some noteworthy information:

```
pac@KVM0101011:~$ sudo nmap -T3 172.25.100.23 -p 44818 --script enip-info
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-31 20:17 MDT
Nmap scan report for 172.25.100.23
Host is up (0.001s latency).

PORT      STATE SERVICE
44818/tcp open  EtherNet-IP-2
| enip-info:
|   type: Programmable Logic Controller (14)
|   vendor: Rockwell Automation/Allen-Bradley (1)
|   productName: 1999-56 Our Test Controller
|   serialNumber: 0x006c061a
|   productCode: 54
|   revision: 20.11
|   status: 0x3160
|   state: 0xff
|_  deviceIp: 0.0.0.0
MAC Address: 00:03:12:0B:02:AC (TRsystems GmbH)

Nmap done: 1 IP address (1 host up) scanned in 0.42 seconds
```

What we are seeing here is Python's cpppo module, which we installed on this machine as part of the lab setup (see Appendix 1), in action. The server service that is running on that machine is completely customizable, so we can return any information we want by specifying so in the cpppo.conf file.

So, we now have all the information we need to be able to search for vulnerabilities and feed them into our asset management system. Before we move on, let's look at how EtherNet/IP suffers from the same vulnerabilities as Modbus and most other industrial communications protocols, including a lack of confidentiality (cleartext protocol) and a lack of authentication (anyone can manipulate data). An attacker can sniff the network to identify key data/tags to use them in their attack. A particularly handy Python module, Pylogix, makes detecting this information even more straightforward by allowing us to ask the controller what tags are in its memory. We are going to look at Pylogix next.