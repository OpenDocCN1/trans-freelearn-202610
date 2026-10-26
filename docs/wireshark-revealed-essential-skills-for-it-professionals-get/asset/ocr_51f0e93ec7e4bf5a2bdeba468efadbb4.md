columns configuration, as described in Chapter 1, Introducing Wireshark).

![](bbox=[0.171, 0.086, 0.928, 0.268])

+   2. You can see from here that `62.219.24.171` sends a packet with a sequence number of `120185105`, and then a packet with the sequence number `120186557`. When receiving these two packets, the client `10.0.0.7` tells the server to send him the next packet with ACK = `120188009`, after which the server sends the packet with the sequence number `120188009`, and the next packet with sequence number `120189461`, and so on.

You can see a diagram for this.

![](bbox=[0.122, 0.533, 0.737, 0.856])

# What are TCP retransmissions and what do they cause