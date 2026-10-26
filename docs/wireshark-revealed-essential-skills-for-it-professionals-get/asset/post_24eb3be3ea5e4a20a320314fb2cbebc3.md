
![](img/da40f92928403866599198344271d63e_978_0.png)

- You can also have consecutive SYN and RST packets. When there is a port number that is opened, you will see the complete SYN/SYN-ACK/ACK when the scanner opens connection to the victim. This is illustrated in the following screenshot:

![](img/da40f92928403866599198344271d63e_978_1.png)

- 7. Always look for unusual traffic patterns. Too many ICMP requests, for example, are a good indication for scanning. Look for multiple ICMP requests to clients, ICMP timestamp request, ICMP in ascending or descending order, and so on. These patterns can indicate malicious scanning.
  - When you suspect a scan, click on the title of the destination (address), and you will get the packet list sorted by the destination address. In this way, it will be easier to see the scan patterns.
  - In the following screenshot, you see an example of this scenario:

![](img/da40f92928403866599198344271d63e_979_0.png)

8. In the case of application scanning, you can have various types of scans:

- **NetBIOS scans**: It looks for massive scanning of NetBIOS ports
- **HTTP**: It looks for SYN requests to HTTP port 80 with HTTP requests later on
- **SMTP**: It looks for massive scanning on the TCP port 25
- **SIP**: It looks for massive requests on port 5060

Other types of applications are scanned according to their port numbers

## How it works...

The majority of scanners work in several steps: ARP scanning, ICMP, and then TCP or UDP. The principle is simple:

- If the scanner is on the LAN, it sends an ARP broadcast to the entire LAN.
- The scanner sends ICMP requests. Some of the ICMP requests will be answered.
- When someone answers the ARP or ICMP request, it goes up to TCP and UDP and starts scanning the layer-4 ports. When the scanner finds out that a port is open, it starts with application scanning.
- In application scanning, the scanner sends commands to the applications, trying to get the application to answer, and in this way, try to break into it.

## There's more...

Most of the modern intrusion detection/prevention systems (IDS/IPS) in the last several years know how to deal with ICMP scans, TCP SYN scans, and various types of scans that generate massive traffic of standard, well-known attack patterns. In case you have such a system and you connect to the Internet with an ISP that has their systems, you are probably protected from these simple types of attacks.

These systems usually work in two ways:

- NetFlow/Jflow-based IDS/IPS that identifies massive traffic coming from several sources; they neutralize it by blocking it or changing the routing tables to disable these packets from getting to the ISP network
- Content-based IDS/IPS that looks at the traffic patterns and accordingly decides whether to forward it or not

Attacks coming from the internal network are not filtered by the external devices, and therefore, are even more common. There are more sophisticated types of attacks that will be discussed in the Locating smart TCP attacks recipe later in this chapter.

The way to prevent attacks coming from the Internet is to connect through an ISP with efficient IDS/IPS systems along with using one of your own. The way to prevent attacks coming from the internal network is to implement organizational security policy along with appropriate protection software such as antivirus and personal firewalls.

## See also

In the previous section, I've mentioned the issue of organizational security policy, that is, how to implement a set of rules for securing your organization. Further information on this subject is widely available on the Internet. Some interesting websites that cover this area are:

- http://www.cert.org/work/organizational_security.html
- http://www.praxiom.com/iso-17799-4.htm
- http://www.sans.org/reading-room/whitepapers/policyissues/1331.php
- http://www.sans.org/security-resources/policies/

## Discovering DoS and DDoS attacks

**Denial of Service (DoS)** and **Distributed Denial of Service (DDoS)** are attacks that intend to deny users from accessing network services. Services that can be denied to users can be:

- **Communication lines**: This will usually be done by generating traffic that floods and blocks the communications line
- **Applications and services (web services, mail services, and so on)**: This will usually be done by loading a server to a point at which it will not be able to serve clients' requests

DoS/DDoS attacks can be a result of scanning that we talked about in the previous recipe. The difference is that DoS/DDoS is a scan that slows down a server or a network in a way that denies user access.

In this recipe, we will see some common DoS/DDoS patterns, and learn how to identify and block them.

## Getting ready

DoS/DDoS are usually discovered when one of the network resources, that is, communications lines or servers becomes very slow and is also not functioning.

When you identify such a resource, connect Wireshark with port mirror to this device and start packet capture. In this recipe, we will go through some common DoS/DDoS attacks and their signatures.

## How to do it...

Connect Wireshark to the network with port mirror to the port of the resource that you suspect is exposed to DoS/DDoS. Usually, it will be a server that becomes very slow, a communication line that becomes very loaded, or any other resource that stops functioning or becomes very slow.

- 1. When a communication line becomes very slow, for example, a connection to the Internet, connect Wireshark with port mirror to this line.
- 1. Try to locate where the traffic comes from.
- 2. I've port mirrored the server, and this is what I got:

![](img/da40f92928403866599198344271d63e_985_0.png)

- 3. We see source addresses in the ascending order, generating traffic to the Internet address 94.23.71.12.

### Tip

When you look at the time column that is configured with "time since the previously displayed packet", you see that there are 11-12 micro-seconds between frames. When you see TCP-SYN coming at this rate, something is wrong. Check what it is!

- 4. Since the source addresses are unknown, I've checked their MAC address. What I got was:

![](img/da40f92928403866599198344271d63e_986_0.png)

5. The problem was that all source addresses came from a single MAC address; so I checked their MAC addresses, and all IP addresses came from a single MAC address, the MAC address of the server.

### Tip

### Conclusion

Check for SYN scans, and verify which IP and MAC addresses they are coming from. It can be that a worm is generating source addresses that are not the addresses of the host.

2. Another example can be a simple SYN scan that comes from a single attacker, as seen in the next illustration. Look for SYN and watch the port numbers that they are scanning. You might see:

- No response
- Reset packet
- SYN-ACK response

3. There can be various consequences to this type of attack:

- In case of no response or reset response, the attacked server is functioning well. In case the server answers with a SYN-ACK response, it might be a risk to the server.
- The risk is that if too many connections will be opened (SYN/SYN-ACK/ACK) or half opened (SYN/SYN-ACK), the server might get slow due to these connections.
- You can see a typical TCP SYN attack in the following illustration. A SYN

attack becomes DoS/DDoS when it blocks a communication line or loads a server to the point that it stops functioning.

| No. | Time | Source | Destination | Protocol | Info |
|---|---|---|---|---|---|
| 55372 | 0.000025 | 10.0.0.103 | 10.0.0.10 | TCP | 33928 > 1082 [SYN] Seq=0 Win=1024 Len=0 MSS=1460 |
| 55373 | 0.000025 | 10.0.0.103 | 10.0.0.10 | TCP | 33928 > 15003 [SYN] Seq=0 Win=1024 Len=0 MSS=1460 |
| 55374 | 0.000034 | 10.0.0.103 | 10.0.0.10 | TCP | 33928 > 6567 [SYN] Seq=0 Win=1024 Len=0 MSS=1460 |
| 55375 | 0.000025 | 10.0.0.103 | 10.0.0.10 | TCP | 33928 > 458 [SYN] Seq=0 Win=1024 Len=0 MSS=1460 |
| 55376 | 0.000026 | 10.0.0.103 | 10.0.0.10 | TCP | 33928 > 8383 [SYN] Seq=0 Win=1024 Len=0 MSS=1460 |
| 55377 | 0.000035 | 10.0.0.103 | 10.0.0.10 | TCP | 33928 > 2100 [SYN] Seq=0 Win=1024 Len=0 MSS=1460 |
| 55378 | 0.000025 | 10.0.0.103 | 10.0.0.10 | TCP | 33928 > 1721 [SYN] Seq=0 Win=1024 Len=0 MSS=1460 |
| 55379 | 0.000025 | 10.0.0.103 | 10.0.0.10 | TCP | 33928 > 8994 [SYN] Seq=0 Win=1024 Len=0 MSS=1460 |
| 55380 | 0.000025 | 10.0.0.103 | 10.0.0.10 | TCP | 33928 > 6699 [SYN] Seq=0 Win=1024 Len=0 MSS=1460 |
| 55381 | 0.000025 | 10.0.0.103 | 10.0.0.10 | TCP | 33928 > 10616 [SYN] Seq=0 Win=1024 Len=0 MSS=1460 |
| 55382 | 0.000025 | 10.0.0.103 | 10.0.0.10 | TCP | 33928 > 2381 [SYN] Seq=0 Win=1024 Len=0 MSS=1460 |
| 55383 | 0.000024 | 10.0.0.103 | 10.0.0.10 | TCP | 33928 > 55555 [SYN] Seq=0 Win=1024 Len=0 MSS=1460 |
| 55384 | 0.000025 | 10.0.0.103 | 10.0.0.10 | TCP | 33928 > 8193 [SYN] Seq=0 Win=1024 Len=0 MSS=1460 |
| 55385 | 0.000026 | 10.0.0.103 | 10.0.0.10 | TCP | 33928 > 10001 [SYN] Seq=0 Win=1024 Len=0 MSS=1460 |
| 55386 | 0.000025 | 10.0.0.103 | 10.0.0.10 | TCP | 33928 > 5904 [SYN] Seq=0 Win=1024 Len=0 MSS=1460 |

TCP SYN SCAN
