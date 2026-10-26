# How it works...

Let's see the regular operation of TCP, and what are the causes for problems that might happen.

# Regular operation of the TCP Sequence/Acknowledge mechanism

One of the mechanisms that is built into TCP is the retransmission mechanism. This mechanism enables the recovery of data that is damaged, lost, duplicated, or delivered out of order.

This is achieved by assigning a sequence number to every transmitted byte, and expecting an acknowledgment (ACK) from the receiving party. If the ACK is not received within a timeout interval, the data is retransmitted.

At the receiver end, the sequence numbers are used in order to verify that the information comes in the order that it was sent. If not, rearrange it to its previous state.

This mechanism works as follows:

+   1. At the connection establishment, both sides tell each other what will be their initial sequence number.
+   2. When data is sent, every packet has a sequence number. The sequence number indicates the number of the first byte in the TCP payload. The next packet that is sent will have the sequence number of the previous one, plus the number of bytes in the previous packet, plus one (in the next screenshot).
+   3. When a packet is sent, the RTO (Retransmission Timeout) counter starts to count the time from the moment it was sent.

# Tip

The Retransmission Timeout timer is based on the Van Jacobson congestion avoidance and control algorithm, which basically says the TCP is tolerant to high delays, but not to fast delay variations.

+   4. When the receiver receives the packet, it answers with an ACK (Acknowledge) packet that tells the sender to send the next packet. In the following screenshot you will see how it works:
+      1. You can see from here that 10.0.0.7 is downloading a file from 62.219.24.171. The file is downloaded via HTTP (the Wireshark window was configured to show tcp.seq and tcp.ack from the Edit | Preferences