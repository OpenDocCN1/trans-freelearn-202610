
## Discovering unusual traffic patterns

In this recipe, we will learn what are usual and unusual traffic patterns and how to distinguish between them.

### Getting ready

The first thing is to locate Wireshark. There are several options for this (see the following diagram):

1. When you suspect an attack that comes from the Internet, locate Wireshark after the firewall (1), and when you suspect that it crosses the firewall, locate it before (2).
2. When you suspect malicious traffic coming from a remote office, port mirror the traffic coming on the central line before (3) or after (4) the router. In this case, you can filter the suspicious traffic with IP networks to see patterns from different offices in order to isolate the problematic office.
3. You can also port mirror the traffic in the remote office before (7) or after (6) the routers.
4. When a PC or a server is the suspect, port mirror its port on the switch (5) or (8).

![](img/da40f92928403866599198344271d63e_959_0.png)

Now, we will try to see what are the types of traffic that we should look out for, what are the types of traffic that are normal, and what traffic should be followed.

Before starting with the tests, make sure that you have an updated topology of the network that includes:

- Servers' IP addresses and LANs' IP address ranges
- Routers, switches, and other communications equipments' IP addresses and topology
- Security devices—firewalls, **Intrusion Detection Systems / Intrusion Prevention Systems (IDSs/IPSs)**, **Web Application Firewalls (WAF)**, database and application firewalls, antivirus systems, and any other device that has an IP address and generates, filters, or forwards network traffic
- What are the applications that work over the network including TCP/UDP port numbers and IP addresses of software

### How to do it...

When you monitor internal traffic in your organization, the following things should be checked:

1. Traffic that is generated from known addresses (in the organization):
   - **Normal:** This is the traffic from known addresses and address ranges
   - **Suspicious:** This is the traffic from/to addresses that you don't know
2. Applications and port numbers:
   - **Normal:** This includes standard port numbers, 80 (HTTP), 137/8/9 (NetBIOS), 3389 (RDP), 20/21 (FTP), 25,110 (Mail), 53 (DNS), and so on. Be sure of the applications that run over the network, and verify that these are the only port numbers that you see.
   - **Suspicious:** This includes unusual port numbers, that is, port numbers that do not belong to applications that run on server (for example, RDP packets to web server).
3. TCP patterns:
   - **Normal:** TCP SYN/SYN-ACK/ACK that indicates a connection establishment, single reset (RST) that indicates a fast connection tear-down, FIN/FIN-ACK packets that indicate a regular tear-down of a connection, standard packets, and acknowledgments
   - **Suspicious:** Large amount of SYN packets that go to a single or multiple destinations or coming from multiple sources (usually in a scan pattern that will be described later in this chapter), unusual flags combination (RST/FIN, URG), and so on
4. Massive traffic to a single or multiple sites that you don't know about:
   - **Normal:** Traffic patterns are usually not of fixed bandwidth. When you save or open files, browse the Internet, send or receive mails, or access a server with RDP, you see ups and downs.
   - **Suspicious (in some cases):** Fixed bandwidth patterns can indicate that someone is connected to your device, but it can also indicate that someone is listening to the radio over the Internet (100-150 Kbps), watching video (in some cases), and so on. When you see a fixed bandwidth pattern of traffic, check what it is. A fixed bandwidth pattern is illustrated in the following screenshot:

Wireshark IO Graphs: Tel Hashomer station warm 1-JAN-2013 Test 02 --- LS38cast-EXE.pcapng

5. Broadcasts:
   - **Normal**: NetBIOS broadcasts, ARP broadcasts (not too many), DHCP (not too many), application broadcasts (usually once every several seconds and more), and so on
   - **Suspicious**: Tens, hundreds, or thousands and more broadcasts per second per device
6. DNS queries and responses:
   - **Normal**: A standard query-response pattern up to several tens per second per client, occasionally
   - **Suspicious**: Massive amount of DNS queries and/or responses, responses without queries, and so on

### How it works...

Network forensics is quite similar to what you see in police dramas on television. Something is going wrong; so, you go to the crime scene (this is your network) and look for evidence (these are the traces that are left in the network).

What you look for are the things that do not match the crime scene (your network), things that are left behind (unusual traffic patterns), fingerprints, and DNA (patterns that can identify the attacker).

In the following recipes, we will get to the details of various types of attacks and abnormalities that can indicate that a crime was committed, and we will see how to isolate the problems and solve them.

Some common attacks that can come from the network are:

- **Viruses**: These are small programs that attack your computer and try to cause damage. Viruses should be discovered and fixed by antivirus software.
- **Worms**: These are usually programs that attempt to replicate themselves across the network. There is a major impact on resource consumption, for example, bandwidth consumption and CPU load. The important thing is that the moment you fix the problem, everything will go back to normal.
- **Denial of Service (DoS) and Distributed DoS (DDoS)**: These are attacks that deny access to network resources. These types of attacks are usually very easy to discover since they have a distinct behavior that can be located easily.
- **Man-in-the-middle attacks**: These are attacks in which the attacker intercepts messages and then retransmits them. In this way, the attacker can eavesdrop on the traffic or change it before it gets to the destination.
- **Scanning**: There are various types of scans ranging from simple ICMP scans that usually are a form of DDoS, TCP scans that send, for example, SYN requests on various port numbers in order to try and open connections to services running on a server, and also application scans that try to connect to applications running on your servers.
- **Application-layer attacks**: These are attacks that target applications on your servers by intentionally causing a fault in a server's operating system or applications.

In the following recipes, we will see each of them (and some more).

### There's more...

An important indication that something went wrong is when a server, a PC, a communication link, or any other entity on the network becomes slow without any logical reason. For example:

- When a server becomes slow, check for hardware and software issues, check for network problems, but also check if someone is attacking it
- When a link from a remote office to the center becomes slow, it can be because of the load (constant or sudden), but it can also be because of an attack that blocks it (usually DoS/DDoS)
- When a PC becomes slow, it can be because it is doing something that you know about, but there is not just one possibility, check for the things you don't know

It is important to mention here that there are various systems that can protect us from attacks; a few of them are listed as follows:

- **Firewalls**: They protect unauthorized traffic from getting into specific areas. Firewalls can be located on the connection to the Internet, before the organization servers, between organization areas, and even as personal firewalls on every PC.
- **Network Access Control (NAC)**: These systems allow only authorized users to connect to the network. When connecting an unauthorized device to the network, you will see that the link on the device will be turned on and immediately off, and the unauthorized device will be blocked on the MAC layer.
- **IDS/IPS**: These systems can identify intrusion patterns and block them. There are usually two lines of defense here—one at the ISP network and one at the customer premises. IDS/IPS can be a dedicated device located between the firewall and the Internet or an additional software on the firewall.
- **Web Application Firewalls (WAF), Application Firewalls, Database Firewalls, and other application protection devices**: This group of products are layer-7 protection devices that look inside the applications and forward or block application layer attacks.
- **Web Filters and Mail Filters**: These are devices that scan mail and/or web content and forward only those messages and traffic that are allowed.

The features mentioned above can come as different devices, software on Virtual Machines (VMs), or features on the same device.
