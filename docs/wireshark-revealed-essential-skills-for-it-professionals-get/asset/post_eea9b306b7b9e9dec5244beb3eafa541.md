
## Chapter 9. UDP/TCP Analysis

This chapter contains the following recipes:

- Configuring TCP and UDP preferences for troubleshooting
- TCP connection problems
- TCP retransmissions – where they come from and why
- Duplicate ACKs and fast retransmissions
- TCP out-of-order packet events
- TCP Zero Window, Window Full, Window Change, and other Window indicators
- TCP resets and why they happen

### Introduction

The goal of **Transmission Control Protocol (TCP)** and **User Datagram Protocol (UDP)** is to pass information between end applications, for example, from a web client to a web server, mail client to a mail server, and so on. This is done by providing identification to end applications and forwarding packets between them. These identifications are called port numbers, and a port number with its IP address is called a socket. In the following diagram you can see what happens when you open a connection from your browser to a web server. The web server listens on port 80 and you will open a connection, for example, from port 1024.

So, the server is listening to requests on port 80 and will send responses to you on port 1024.

![](img/da40f92928403866599198344271d63e_671_0.png)

While TCP is a reliable, connection-oriented protocol, UDP does not support connectivity and reliability, but simply transfers datagrams between two end processes.

#### Tip

There is an additional layer-4 protocol, which is called SCTP (Stream Control Transmission Protocol). This protocol can be considered as an improved version of TCP, and mostly used in a service provider's networks. SCTP is not included in the scope of this book.

In this chapter, we will focus on TCP, its behavior, various problems, and how to use Wireshark in order to isolate and solve them.

### Configuring TCP and UDP preferences for troubleshooting

In most cases you can use the default Wireshark parameters for TCP and UDP network analysis, but there are also some changes that can be configured. The changes will be configured in the **Preferences** window.

#### How to do it...

In this section we will see how to configure TCP and UDP preferences.

##### UDP parameters

Let's see some parameters that can influence the capture of UDP:

![](img/da40f92928403866599198344271d63e_675_0.png)

You can configure the following parameters in UDP:

- **Show UDP summary in protocol tree**: Mark this button if you want the UDP summary line to be shown in the protocol tree (set by default)
- **Try heuristic sub-dissectors first**: Try to decode a packet using the heuristic method before using a sub-dissector registered to the specific port
- **Validate the UDP checksum if possible**: Validates the UDP checksum
- **Collect process flow information**: Collects process flow information

By default only the first parameter is set. In most cases it is enough.

##### TCP parameters

![](img/da40f92928403866599198344271d63e_676_0.png)

You can configure the following parameters in TCP:

- **Show TCP summary in protocol tree**: Mark this button if you want the TCP summary line to be shown in the protocol tree (set by default).
- **Validate the TCP checksum if possible**: This feature can slow down performance. In most cases it is not required.
- **Allow subdissector to reassemble TCP streams**: This option is for stream analysis (set by default).
- **Analyze TCP sequence numbers**: When this is set, Wireshark analyzes sequence numbers and track phenomena such as retransmission, duplicate ACKs, and so on, which is one of the important features of Wireshark.
- **Relative sequence numbers**: When this is set, Wireshark will show you every TCP connection that starts from seq=0.
- **Track number of bytes in flight**: This setting enables Wireshark to track the number of unacknowledged bytes flowing on the network (set by default).
- **Calculate conversation timestamps**: This feature enables the calculations of TCP timestamps option.
- **Try heuristic sub-dissectors first**: Try to decode a packet using heuristic method before using a sub-dissector registered to the specific port.
- **Ignore TCP Timestamps in summary**: Ignore the timestamp option in the TCP header.
- **Do not call subdissector for error packets**: This option does not analyze erroneous TCP packets.

#### How it works...

There are some parameters in the TCP preferences that I would like to say a few words about.

Referring to **relative sequence numbers**, when you look at a TCP connection you see that it always starts with sequence numbers equal to zero. These are the relative numbers that are normalized to zero by Wireshark. The real numbers are numbers between 0 and 232, picked by the TCP process, which are difficult to follow. The TCP standard does not set any rule for picking this number.

The **calculating conversations timestamps** refers to the timestamp option of the TCP packet. The TCP timestamps option carries two 4-byte timestamp fields, as seen in the next diagram:

![](img/da40f92928403866599198344271d63e_678_0.png)

The problem that the timestamp option comes to solve is the sensitivity of TCP to delay variations. The solution, and written in RFC 1323, is to use TCP options in the following ways (for every TCP connection):

- The sender places a timestamp in each data segment that it sends (the Tsval field)
- The receiver reflects these timestamps in ACK segments (the Tsecr field)

Then, a single subtraction gives the sender an accurate RTT measurement for every ACK segment (which will correspond to every other data segment, with a sensible receiver). This mechanism is called **Round Trip Time Measurement (RTTM)**.

#### There's more…

UDP is a very simple protocol with a very simple header that includes only four fields: source port, destination port, packet length, and checksum. Checksum is used by the receiver to check whether to accept the packet or drop it. In case of a packet drop, there is no recovery mechanism. In some cases, the application will recover it (for example, DNS that sends the request again), and in some cases it won't.

TCP is more sophisticated. It is a connection-oriented, reliable protocol, with sequencing mechanism, flow, and congestion control. These subjects will be discussed later in this chapter.

SCTP is a reliable, connection-oriented protocol that allows the transfer of multiple streams per connection, optional bundling of multiple user messages into a single SCTP packet, support for cookies, multi-homing, and other mechanisms. It was initially developed for carrying signaling messages in cellular networks, and later implemented with other application protocols.

## TCP connection problems

When two TCP processes wish to communicate, they open the connection, send the data, and then close the connection. This happens when you open a browser to the Internet, connect from your mail client to the mail server, or connect with Telnet to your router or any other application that works over TCP.

When TCP opens the connection, it sends a request for open connection from the source port to destination port.

Some problems can occur during the establishment or closing of the application. Using Wireshark to locate and solve these problems is the goal of this recipe.

### Getting ready

If you experience one of the following problems, use Wireshark in order to find out what is the reason for it.

These problems can be of many types. Of these:

- You try to run an application and it does not work. You try to browse the Internet and you don't get any response.
- You try to use your mail but you don't have a connection to the mail server.
- Problems can be due to simple reasons, such as the server being down, the application is not running on the server, or the network is down somewhere on the way to the server.
- Problems can be also due to more complicated reasons, such as DNS problems, insufficient memory on the server that does not enable you to connect (due to high memory consumption by an application, for example), duplicate IPs, and many others.

In this recipe we focus on these GO/NO-GO problems that are usually quite easy to solve.
