
![](img/da40f92928403866599198344271d63e_1220_0.png)

I filtered the packets listed to show only DHCP packets using the udp.port==67 filter; as a result, only DHCP packets will be listed in the list pane.

## The TFTP

The Trivial File Transfer Protocol (TFTP) is a lightweight version of the FTP that is used to transfer between hosts. Unlike the FTP protocol, TFTP does not ask users for any credentials. A TFTP uses a UDP as a transport mechanism. Most commonly, a TFTP is used in LAN environments, and when dealing with manageable devices such as switches and routers, network administrators do use TFTP servers to take a back up of configuration files and to update the firmware running in those devices. A TFTP is also used by security professionals to transfer files from their system to yours in order to escalate the privileges (gaining more rights on a compromised system).

I have a TFTP server running at 192.168.1.106 and a client running at 192.168.1.104. There is a text file abc.txt that I've created on the server, and the client will try to download the same. And our sniffer in place will capture the traffic that is generated.

![](img/da40f92928403866599198344271d63e_1221_0.png)

The traffic generated due to the transaction that takes place between two hosts is successfully captured and the packets corresponding to it are shown in the following figure:

![](img/da40f92928403866599198344271d63e_1221_1.png)

Now, let's see what each pointer signifies:

+   - This shows that the transfer of the packet is initiated as soon as the client requests the abc.txt file. The request frame can be seen in the list pane.
+   - As discussed, a TFTP uses a UDP for a transport mechanism. The related details for the request are shown in the details pane, which states that the request was

initiated from a ephemeral port number from the client destined to port 69 on the server (69 is a well-known port to the TFTP protocol).

+   - The request was specific to the `abc.txt` file that is also shown in the details pane in the TFTP protocol section.

You must be wondering about the acknowledgement packets that are shared between the two hosts. As we discussed, a UDP is an unreliable form of communication, so why are we seeing ACKs in a UDP? The reason is that the TFTP server I am using has some kind of inbuilt reliability feature. Even on the client side, over the standard console, after initiating the request, I received quite interactive messages from the server, such as the file of size 3 bytes has been transferred successfully, and various other details were listed along with the message. The interesting thing to know here is that port 69 was only involved in the first packet, and the rest of the packets were sent and received by the acknowledging feature that the server is embedded with. So, the statement that some protocols use a UDP as a transport protocol and have their own inbuilt feature to ensure delivery is true, as we have just witnessed.

## Unusual UDP traffic

Suppose that the resource we are looking for is not available on the server. How will traffic look like then? Refer to the following screenshot to understand this:

![](img/da40f92928403866599198344271d63e_1223_0.png)

As seen in the preceding screenshot, the client requested an invalid resource that the server wasn't able to locate and hence returned with an error code and the summary message `File not found`. The same message was shown over the standard console to the client.

Sometimes, it is also possible that the server daemon may not run and the client may request a certain resource. In such cases, the client would receive the `ICMP destination unreachable` error with the error code 3. Refer to the following figure for the same:

![](img/da40f92928403866599198344271d63e_1223_1.png)

Now, we will see what each pointer signifies:

+   - The server returned with an `ICMP destination unreachable` message when the TFTP server daemon was not functional
+   - The client received an error code of type `3`
+   - The details regarding the request were mentioned in the reply under the UDP protocol section, which stated that the request was sent to port `69`, which was currently nonfunctional
+   - The requested resource was shown under the TFTP protocol section

Unusual DNS requests are also very often seen when a client initiates a request to look for name servers associated with an address. It would look similar to the one shown in the following figure:

![](img/da40f92928403866599198344271d63e_1224_0.png)

Now, we will see what each pointer signifies:

+   - **1:** As seen in the list pane, the client at `192.168.1.106` initiated a request to look for the address `8.0.0.0` and received a response in `Frame 2`
+   - **2:** The request was sent to the default gateway that holds the DNS cache
+   - **3:** The gateway responded with a `No such name` error

There can be multiple scenarios where you will see unusual traffic related to a UDP. The most important thing to look for is TFTP traffic, which might be generated because of the TFTP client in your network. It may be malicious traffic that you would like to make a note of.

## Summary

TCP is a reliable form of communication that has features like a three-way handshake and a tear down process ensures the connection is reliable and interactive.

A TCP header is 20 bytes long and consists of various fields such as source and destination port, `SEQ` and `ACK` numbers, offset, window size, flag bits, checksum, and options. The presence of various flags and header fields let the sender and receiver be sure about the delivery as well as the integrity of the data being sent.

The `SEQ` and `ACK` numbers are used by TCP-based communications to keep track of how much data is being sent across between the hosts taking part.

A UDP is a connection-less protocol that is a nonreliable means of communication over IP, where the lost and discarded packets are never recovered. A UDP does provide us with faster transmission and easier creation of sessions. A UDP header is 8 bytes long, which has very few fields such as source and destination port, packet length, and checksum. At the end application, the data is appended.

Common protocols such as DHCP, TFTP, DNS, and RTP mostly use a UDP as a transport mechanism, and these services are some of the major services that we deal with in our everyday life. To make the connection reliable, some of these protocols support their own version of acknowledging features that comes inbuilt.

In the next chapter, you will learn the basics of wireless traffic, how to decrypt wireless traffic, and the anomalies that may follow.

## Practice questions

+   1. **Q.1** List at least five differences between TCP and UDP protocols.
+   2. **Q.2** Capture a three-way handshake and tear down packets using your own FTP server.
+   3. **Q.3** Explain the purpose of window scaling and checksum offloading and state their corresponding significance in terms of TCP communications.
+   4. **Q.4** In what way can TCP-based communication can recover from a packet loss or unexpected termination? Imitate any scenarios that can generate such traffic.
+   5. **Q.5** Create a display filter to show only TCP FIN and ACK packets sent to your machine from your default gateway in the list pane.
+   6. **Q.6** What is the difference between the absolute and relative numbering system used by Wireshark in order to keep track of packets?
+   7. **Q.7** What is the purpose of the options field at the end of the TCP header and what kind of arguments does it contain?
+   8. **Q.8** There is one more way through which you can create filters to view a packet with a specific flags set. Without providing the HEX equivalent, figure out what it is and how you can filter a packets set with a PSH flag set using the same technique.
+   9. **Q.9** Find out why the length of data can only be 65507 bytes while working with a UDP.
+   10. **Q.10** What kind of packets you will see in a list pane if the server daemon for a TFTP is not running?
+   11. **Q.11** Try performing a zone transfer on your locally configured DNS and capture the traffic for analysis. What interesting facts did you notice about the packets? Explain them in brief.
