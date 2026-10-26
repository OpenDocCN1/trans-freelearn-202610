# How it works...

In this case, things are simple. The TCP sender sends the packets to the receiver. These packets are numbered by their bytes. When a packet does not arrive in order, it is a problem that Wireshark notices. We can have two reasons for this:

+   - **A real problem**: In this case you will see retransmissions and duplicate ACKs that are TCP's response to packets that are received out of order
+   - **A capture problem**: In this case you will see only out-of-order packets, and since you don't see any response to the suspected lost and out-of-order packets, they probably are not