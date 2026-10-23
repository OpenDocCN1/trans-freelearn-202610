```
List of targets contains 1 host(s)
8853:   Checking 172.25.100.254 ...
...
* Cisco by SNMP found ***
*System Description: Cisco Internetwork OS Software
IOS (tm) IES 2000 Software Version 15.2(4), RELEASE SOFTWARE (EA7)
...
Cisco-IOS Webserver found
  HTTP/1.1 401 Unauthorized
Date: Tue, 13 Apr 2020 00:57:07 GMT
Server: cisco-IES
Accept-Ranges: none
WWW-Authenticate: Basic realm="level_15_access"
...
401 Unauthorized
...
```

As you can see, `cisco-torch` can gather some relevant data on the network device we pointed at it. We can take that information and look up any exiting exploits:

![](bbox=[0.117, 0.531, 0.852, 0.626])

Figure 17.15 – Searchsploit results for the discovered IOS version

Now, let's discuss a conventional way to create a network map as we perform our pentest activities by using a tool called Grassmarlin.

# Grassmarlin network mapping

Here is a tip: while you are scanning (for example, performing the initial Nmap scan of the network), let Wireshark capture packets in the background. We can import the packet capture into a network mapping tool such as **Grassmarlin** (`https://github.com/nsacyber/GRASSMARLIN`), which provides us with a nice, graphical representation of the network we are attacking. A great way to map the network at hand is by running the following Nmap scan while having Wireshark sniff the interface we are using:

```
nmap -p- -A -oA ot-scan 172.25.100.0/24
```