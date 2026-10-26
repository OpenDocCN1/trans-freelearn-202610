
# There's more…

In this section, I would like to show some examples to get a better understanding of the NetBIOS protocols.

## Example 1 – application freezing

In the following screenshot, we see the reason for an application freeze:

![](img/da40f92928403866599198344271d63e_852_0.png)

In the example, we make the following observations:

- 1. A client with IP address that starts with 203 is trying to connect to `\\NAS01\SAM1M` on a server with an IP address `10.1.70.95`, and gets back a `STATUS_ACCESS_DENIED` error.
- 2. The client logs off and the server confirms it.
- 3. Since the applications waits, TCP is holding the connection with keep-alive messages.
- 4. After a while, the client sends disconnect requested that is approved by the server.
- 5. The application waits and TCP maintains the connection with keep-alives.
- 6. TCP closes the connection with `RST` (Reset).

What the customer saw here was an application freeze.

## Example 2 – broadcast storm caused by SMB

In one of my client's networks, I got an urgent call that a remote office was disconnected from the HQ. Some network details are as follows:

- The remote office addresses are on subnet 172.30.121.0/24, with a default gateway 172.30.121.254.
- The HQ addresses are on subnet 172.30.0.0/24. The connections between the remote offices and the centre are with L3 IP-VPNs over MPLS network.

To solve the problem, I did the following:

- 1. I tried to ping the servers in the HQ. I got no response.
- 2. I called the service provider that provides the lines to the centre, and they said that on their monitoring system they don't see any load on the line.
- 3. I pinged the local router, 172.30.121.254, and got no response. The meaning is that PCs on the LAN couldn't get to their local router, which is the default gateway.
- 4. I connected a Wireshark with port mirror to the router port, and I saw something like the following screenshot:

![](img/da40f92928403866599198344271d63e_853_0.png)

- 5. I saw that a huge amount of packets are generated within microseconds (1) by a host with IP address 172.30.121.1. The packets are broadcast (3), and the service that generated them is Write Mail Slot (5), which is sent by the SMB Mailslot protocol (4).
- 6. To get the picture of the number of packets, I used the IO Graphs feature. I got 5000 packets per second, that generated 10 Mbps that block the poor old router port (changing the router port to 100 Mbps or 1 Gbps wouldn't help. It would have been blocked too).
- 7. When I didn't find anything about it on Google or Microsoft, I started to stop services that I don't know, keeping track of what happened with the broadcast.

Eventually, the service that caused the problem was called LS3Bcast.exe. I stopped it, made sure it didn't come back and that was it.

![](img/da40f92928403866599198344271d63e_854_0.png)

## Analyzing database traffic and common problems

Some of you may wonder why I have added this section here. After all, databases are considered to be a completely different branch in the IT environment. There are databases and applications on one side and the network and infrastructure on the other side. It is correct since we are not supposed to debug databases; there are DBAs for this. But through the information that runs over the network, we can see some issues that can help the DBAs with solving the problem.

In most cases, the IT staff will come to us first because people blame the network for everything. We will have to make sure that the problems are not coming from the network and that's it. In a minority of the cases, we will see some details on the capture file that can help the DBAs with what they are doing.

## Getting ready

When the IT team come to us complaining about the "slow network", there are some things to do just to verify that it is not the case. Follow the instructions in the following section to make sure we avoid the "slow network" issue.

## How to do it...

In the case of database problems, follow these steps:

- 1. When you get complaints about the "slow network responses" start asking these questions:
  - Is the problem local or global? Does it occur only in the remote offices, or also in the center? When the problem occurs in the entire network, it is not a WAN bandwidth issue.
  - Does it happen the same for all clients? If not, there might be a specific problem that happens only with some users because only these users are running a specific application that causes the problem.
  - Is the communication line between the clients and the server loaded? What is the application that loads them?
  - Do all applications work slowly, or is it only the application that works with the specific database? Maybe some PCs are old and tired, or is it a server that runs out of resources?
- 2. When we are done with the questionnaire, let's start our work:
  1. Open Wireshark and start capturing packets. You can configure port mirror to a specific PC, to the server, to a VLAN, or to a router that connects to a remote office in which you have the clients.
  2. Look at TCP events (expert info). Do they happen on the entire communication link, on specific IP address/addresses, or on specific TCP port number/numbers? This will help you isolate the problem and verify whether it is on a specific link, server, or application.
- 3. If you see problems in the network, solve them as we learned in previous chapters. But, there are some network issues that can influence database behavior. In the following example, we see the behavior of a client that works with the server over a communication line with a roundtrip delay of 35 to 40 ms.
  1. We are looking at the TCP stream number 8 (1), and the connection started

### Tip

When measuring traffic on a connection to the Internet, you will get many retransmissions and duplicate ACKs to websites, mail servers, and so on. This is the Internet. In an organization, you should expect 0.1 to 0.5 percent retransmissions. When connecting to the Internet, you can expect much higher numbers.

with TCP SYN/SYN-ACK/ACK. I've set this as a reference (2). We can see that the entire connection took 371 packets (3).

![](img/da40f92928403866599198344271d63e_858_0.png)

2. The connection continues, and we see time intervals of around 35 ms between DB requests and responses.

![](img/da40f92928403866599198344271d63e_858_1.png)

- 3. Since we have 371 packets travelling back and forth, 371*35 ms gives us around 13 seconds. Add to this some retransmissions that might happen and some inefficiencies, and this leads to a user waiting for 10 to 15 seconds and more for a database query.
- 4. In this case, you should consult with the DBA on how to significantly reduce the number of packets that run over the network; or you can move to another way of access, for example, terminal server or web access.

4. Another problem that can happen is that you will have a software issue that will reflect in the capture file. If you have a look at the following screenshot, you will see that there are five retransmissions (1), and then a new connection is opened from the client side (3). It looks like a TCP problem but it occurs only in a specific window in the software. It is simply a software procedure that stopped processing, and this stopped the TCP from responding to the client (2).

![](img/da40f92928403866599198344271d63e_859_0.png)

## How it works...

Well, how databases work was always a miracle to me. Our task is to find how they influence the network, and this is what we've learned in this section.

## There's more...

When you right-click on one of the packets in the database client to the server session, a window with the conversation will open. It can be helpful to the DBA to see what is running over the network.

When you are facing delay problems, for example, when working over cellular lines over the Internet or over international connections, the database client to the server will not always be efficient enough. You might need to move to web or terminal access to the database.

An important issue is how the database works. If the client is accessing the database server, and the database server is using files shared from another server, it can be that the client-server works great; but the problems come from the database server to the shared files on the file server. Make sure you that know all these dependencies before starting with your tests.

And most importantly, make sure you have very professional DBAs in your friends. One day you will need them.
