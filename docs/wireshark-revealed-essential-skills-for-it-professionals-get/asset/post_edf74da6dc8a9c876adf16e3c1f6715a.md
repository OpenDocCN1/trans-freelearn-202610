
![](img/da40f92928403866599198344271d63e_1326_0.png)

Figure 8.4: Duplicate ACK

With these real-life examples, I expect that you have understood the behavior of TCP error recovery features more precisely.

## The flow control mechanism

This is another feature used by the TCP protocol to avoid any data loss during the transmission. Using flow control, the sender syncs the transmission rate with the receiver's buffer space with a motive to avoid any future data loss. Consider a scenario where the recipient has a buffer space of 1,000 bytes available at an instance, and the sender side is capable of sending up to 5,000 bytes per frame. Now, using this information, both the hosts have to sync their window size to 1,000 bytes only to avoid any data loss. Refer to the following figure that shows this feature:

![](img/da40f92928403866599198344271d63e_1327_0.png)

The preceding figure depicts the way both the communicating hosts negotiate the window size for transmission purpose. Observe the behavior, beginning from the frame with SEQ 1 where Host 2 responds with ACK 2 to specify that the frame was successfully received.

Next, **HOST 1** tries to increase the transmission rate to two frames and sends them with **SEQ 2** and **3**. **Host 2** responds with **ACK 4**, which denotes that both frames were successfully received. Similarly, we succeed in increasing the rate to three frames.

Next, **HOST 1** increases the rate to 4 and tries sending packets with **SEQ 7, 8, 9,** and **10**. This time, **HOST 2** responds with **ACK 10**, which means that **Host 2** receiving the window size can afford maximum 3 frames at an instance, and the sending side should adjust to it.

Next time, when **Host 1** transmits, the windows size would be set to 3 frames, which the recipient can afford to process on his/her end. The window size is not set to a permanent value; it can vary until the whole transmission is completed, and the whole process is called the **TCP sliding window** mechanism and is used to avoid data loss during a transmission.

Think about what would happen if the recipient side is left with no buffer space, that is, 0 bytes. It can handle at some moment during the transmission. What will the TCP do in such case? Will the communication channel drop or the TCP will come up with something more reliable.

Yes, the TCP has another data loss recovery feature called the **Zero window** notification. Here, the recipient side sends a Windows update packet set to 0 bytes and asks the sender to halt the transmission of frames. In response, the sending side will understand the situation and respond with a **Keep Alive** packet that is sent at a particular duration while waiting for the next Window Update packet from the client. Refer to the Figure 8.6 that illustrates the same.

**HOST 1** starts communicating after the three-way handshake process has been completed. After a few packets get transmitted successfully, the receiving side buffer space gets filled up with other resources, so **HOST 2** responds with a Zero Window packet telling **Host 1** to halt sending packets until further notice. Accepting the **Host 2** zero window packet, **Host 1** starts transmitting **Keep Alive** packets in order to keep the connection active and waits for further notice. Once **Host 1** receives the new window size and ACK for the frames that were transmitted, it will start sending the data packets again in accordance with the receiver's buffer space.

## Troubleshooting slow Internet and network latencies

The discussion that we had on delays observed in the list pane can be categorized in two categories: the normal/acceptable delays and the unacceptable delays. Yes, you heard me right, there are some forms of delay that are acceptable, and you should not waste any precious time of yours in troubleshooting any of those cases.

Assign a category to your current scenario on the basis of the test results that you have obtained from the client site (try to put sniff packets from the complaining client's perspective) into one of the following categories: wire latency, client latency, and server latency. Seeing your scenario with the perspective of one of these cases will assist you in solving the problem with a more process-oriented approach, hence making the task less complex, which will end up getting sorted out in lesser time with lesser resources.

Before you start troubleshooting such scenarios, I would highly recommend that you change the default list pane view by customizing the existing time column (customize the time value to `seconds since Previous Displayed Packet`), which would work as a column to figure out latency issues, that is, it will show you the total amount of time between two related packets in a sequence. Refer to the following figure to customize the time column.

To further elaborate the best practices that are followed, I will discuss a step-down approach, which you can use as part of your checklists. Make sure that you understand one thing clearly: tracking an issue can be quite critical on a server side because you may see thousands of packets flying in and out per seconds. This can be really messy and would only end up in making the whole problem more intense. Looking at thousand of packets to figure out the source of slow Internet connection doesn't sound feasible. So, the best option would be to filter out things, prioritize them, and look at the problem from the client's end first.

![](img/da40f92928403866599198344271d63e_1331_0.png)

Figure 8.7: Customizing the time column

- Starting your investigation at the client's end makes it much simpler because you won't be dealing with several packets that may not be relevant to your scenario. On the other side, if there is even a hairline chance that you won't be able to see the packets that are relevant to you, this might make the troubleshooting experience a bit challenging.
- Apart from all the challenges that you might face at the client's end, the first thing you should ask your client is to replicate the problem if possible, or if the problem is occurring in a time-based manner, then you should wait at the client's end in order to witness and understand the scenario. The ultimate goal should be to capture the relevant packets and get a crystal clear understanding of the problem that the client is facing from their perspective.
- Now, when you have the trace file in hand, you can look at the process where the client is trying to connect to the server: the whole process where the client issues a DNS query with an objective to attain a server's logical location over the Web. If the local DNS cache already holds the IP address of the server, then you might not observe any DNS packets; instead, a direct SYN packet would be seen in the list pane sent to the server to initiate the independent connection. What you need to

make sure here is that if the DNS queries are seen in the list pane, then the round trip time should be low, as expected (approximately less than or equal to 150 ms).

![](img/da40f92928403866599198344271d63e_1332_0.png)

The next would be the three-way handshake packet that you will be observing in the list pane. The best option would be to isolate the communicating hosts that can help you in eliminating any further communication. You can just right-click on the communication and create a filter as illustrated in Figure 8.8

- Once you have filtered out the problematic connection between the hosts, the next task would be to observe the total time. The time between duration when the SYN packet was sent and the corresponding SYN/ACK packet was received. This can be compared with the baseline that you already have to come up with a variance that could help you in pointing out whether the connection is slow or is working fine. Refer to the following screenshot that illustrates the same:

![](img/da40f92928403866599198344271d63e_1332_1.png)

Figure 8.8: The time between the SYN and SYN/ACK packets

- As you can see, the time between the SYN and SYN/ACK packets is relatively low, and this seems to be a good working connection. This kind of connections can be helpful while you are designing a baseline for your network. At a later point in time, the same can be used to compare with problematic scenarios. Refer to the
