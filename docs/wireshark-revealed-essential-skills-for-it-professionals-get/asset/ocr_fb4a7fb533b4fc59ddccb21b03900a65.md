the time to acknowledge every TCP sequence

What we've got is the time that it took to acknowledge every TCP packet.

+   8. Now, let's configure **IO Graphs** to see if there are TCP retransmissions, and why they happen:
+      - Use the same IO Graph with **Advance in Y Axis**, and configure the second line.
+      - The **Filter** field: **tcp.stream eq 0** (numbered 1 in the preceding diagram) to present a single stream.
+      - The calculation **COUNT FRAMES(*)** to see the average.
+      - The filter `tcp.analysis.retransmissions` to see the time to acknowledge every TCP packet.

![](bbox=[0.122, 0.339, 0.924, 0.644])

+   9. We can see from here that all retransmissions happened when there was a significant increase in the delay, so it is a delay problem. We cannot say from here if the delay is from the network, from the end device or from the application, so for isolating the problem check how retransmissions are distributed (see _Chapter 9, UDP/TCP Analysis_).
+   10. Retransmissions that are not due to increase in RTT are probably due to packet losses.