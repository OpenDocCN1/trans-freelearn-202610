
## Getting ready

Connect Wireshark with a port mirror to the suspected link or server, and start capture.
Keep track of every window message you will see in the capture window.

## How to do it...

There are several types of window messages that you should be aware of:

### TCP Zero Window, Zero Window Probe, and Zero Window Violation

TCP Zero Window occurs when a receiver advertises a receive window size of zero (in the window field in the TCP header). This tells the sender to stop sending data because the receiver's buffer is full. This indicates a problem on the receiver that might be:

- A weak server that cannot allocate enough memory for the process
- A problem in the application that does not receive a sufficient buffer, so the TCP has to tell the sender to stop sending data
- An application that consumes too much memory so the operating system will limit the application resources

TCP **Zero Window Probe** is a message that is sent by the sender in order to see if the receiver's Zero Window condition still exists. This message works by sending the next byte of data to the receiver. If the receiver answers with window that is still zero, the sender doubles his timer before probing again.

The sender ignores the Zero Window condition of the receiver and sends additional bytes of data. TCP Zero Window Violation can indicate a TCP error or bug in the protocol stack.

In order to check what the problem is, check if these events are coming from:

- A specific end device (server or client) that will indicate a problem in the end device.
- A problem in a specific application that will indicate a general application problem.
- A problem when you do something specific in the application, for example, open a specific table, send a file to the printer, create a report, or anything else on the application. In this case, it is of course an application problem.

### TCP Window Update

TCP sends Window Update to the other side in a connection in order to indicate that it changed the buffer size, and is ready to accept higher or lower data rate (buffer size determines the throughput that the sender is allowed to send). This can happen in the case of:

- The TCP receiver recovers from the Zero Window condition, and asks the sender to start sending data again. In this case, you don't have to do anything about it, just check for the problem that caused Zero Window the first time.
- The TCP receiver changes the window's size frequently. In this case check what is disturbing the receiver. It can be an application problem, memory problem, or any other performance problems on the end devices.

If you see this kind of phenomena, there is nothing to worry about. This is how TCP works.

## TCP Window Full

This message is an indication that the sent packet will completely fill the receiver buffer on the receiver. This will happen when the receiver has not sent any ACK confirming the acceptance of the previous data, and therefore, this will be the last packet of data that the sender will send before accepting an ACK from the receiver.

On the receiver side, the moment it gets this packet, it will send a Zero Window message to the sender that will stop sending the data.

This event is triggered for the same reasons that trigger Zero Window. It is simply an indication to a non-responsive server or application. A typical example is shown in the following screenshot:

![](img/da40f92928403866599198344271d63e_718_0.png)

In the previous screenshot we see that:

- 1. Packet 183816, 192.168.2.138 tells 192.168.1.58 that the sender window is

## How it works...

The TCP Sliding Window mechanism works as follows:

- 1. After the connection is established, the sender sends data to the receiver, filling the receiver window.
- 2. After several packets, the receiver sends an ACK to the sender, confirming the acceptance of the bytes sent by it. Sending the ACK empties the receiver window.
- 3. This process is continuous when the sender is filling the window, and the receiving party empties it and sends confirmation of the information.
- 4. Increasing the receiver window size tells the sender to increase the throughput, and decreasing it tells him to decrease the throughput. It works according to the following WS/RTT rule (with some changes according to the TCP version):

![](img/da40f92928403866599198344271d63e_720_0.png)

## There's more...

You can also use the TCP throughput graphs and the IO graphs to view these problems. In the TCP throughput graphs, use the TCP trace graph, where the upper line indicates the window size, and its distance from the lower line indicates what is on the left-hand side of the window. No distance between them indicates a Zero Window.

![](img/da40f92928403866599198344271d63e_721_0.png)

A fixed distance between the lines (as shown in the preceding screenshot) indicates a good operation on the receiving side. When the lines are getting closer, it indicates that the sender is overwhelming the receiver. As long as lines are not overlapping, TCP will continue to send data.

## TCP resets and why they happen

During a normal operation, TCP will open a connection with SYN signals, and close the connection with FIN signals. One of the characters of TCP is the possibility to close a connection faster due to a problem or just for better efficiency.

In this recipe we will describe these cases, and how to understand exactly what happens, and if it is a regular condition or something went wrong.

## Getting ready

Connect Wireshark with a port mirror to the suspected link or server, and start capture. Keep track of every window message you will see on the capture window. TCP resets can be sent in several cases. Some point to the proper working of the protocol, and some suggest a failure or problem. In this recipe, we will get to the reasons for it, and try to point out the problems and how to solve them.

## How to do it...

Reset is a TCP signal that is sent in order to tell the receiver to break the connection. Reset is sent by setting the RST flag to a value of 1.

## Cases in which reset is not a problem

The standard way of closing a connection in TCP is by FIN and FIN-ACK signals. The problem is that in order to close a connection, you need four packets: FIN/ACK and ACK from one side, and the same from the other side. It can happen, for example, when you open a standard web page, tens of connections (the main page, news bars, commercials, pictures that are updated periodically, and so on) can be opened, and in order to close all of them you will need sometime hundreds of FIN and FIN-ACK packets standard way. In order to prevent it from happening, the web server will, in many cases, send you the requested data and then break the connection with reset. This is a standard thing to do, and it depends on the application.

## Cases in which reset can indicate a problem

There are some cases in which resets can indicate a problem (not necessarily a communication problem):

- A reset sent by a firewall: When you try to open a connection to a remote server, and don't get anything, you might see an RST signal coming back. This is a firewall blocking a connection. In the next screenshot, you can see that every SYN that is sent is replied to with an RST.

![](img/da40f92928403866599198344271d63e_724_0.png)

- Reset sent due to a problem on one of the sides: Here you can have many
reasons. Some of them are as follows:

- One of the reasons is the five consecutive retransmissions that are not replied to by an ACK. When the sender does not get any reply for the retransmissions, it will send a reset signal to the other side, telling it to break the connection.
- Another reason is a connection without any traffic on it for a few minutes (how many minutes is the operation system default). The side that opened the connection will usually send the reset (usually but not always, it depends on the implementation).
