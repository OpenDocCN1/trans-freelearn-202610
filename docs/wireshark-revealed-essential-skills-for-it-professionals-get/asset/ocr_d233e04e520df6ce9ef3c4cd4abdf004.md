# How to do it...

In the case of database problems, follow these steps:

+   1. When you get complaints about the "slow network responses" start asking these questions:
+      - Is the problem local or global? Does it occur only in the remote offices, or also in the center? When the problem occurs in the entire network, it is not a WAN bandwidth issue.
+      - Does it happen the same for all clients? If not, there might be a specific problem that happens only with some users because only these users are running a specific application that causes the problem.
+      - Is the communication line between the clients and the server loaded? What is the application that loads them?
+      - Do all applications work slowly, or is it only the application that works with the specific database? Maybe some PCs are old and tired, or is it a server that runs out of resources?
+   2. When we are done with the questionnaire, let's start our work:
+      1. Open Wireshark and start capturing packets. You can configure port mirror to a specific PC, to the server, to a VLAN, or to a router that connects to a remote office in which you have the clients.
+      2. Look at TCP events (expert info). Do they happen on the entire communication link, on specific IP address/addresses, or on specific TCP port number/numbers? This will help you isolate the problem and verify whether it is on a specific link, server, or application.
+   3. If you see problems in the network, solve them as we learned in previous chapters. But, there are some network issues that can influence database behavior. In the following example, we see the behavior of a client that works with the server over a communication line with a roundtrip delay of 35 to 40 ms.
+      1. We are looking at the TCP stream number 8 (1), and the connection started

# Tip

When measuring traffic on a connection to the Internet, you will get many retransmissions and duplicate ACKs to websites, mail servers, and so on. This is the Internet. In an organization, you should expect 0.1 to 0.5 percent retransmissions. When connecting to the Internet, you can expect much higher numbers.