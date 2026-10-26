# # Round-trip time graphs

**Round-trip time (RTT)** is the duration in which the ACK for a packet that is sent is received, that is, for every packet sent from a host, there is an ACK received (TCP communication), which determines the successful delivery of the packet. The total time that is consumed from the transfer of the packet to the ACK for the same is called round trip time. Follow these steps to create one for yourself:

+   - Select any TCP packet in your **packet list** pane.
+   - Navigate to **Statistics | TCP Stream Graph | Round Trip Time Graph**.
+   - The x axis represents the TCP sequence number and the y axis represents the RTT in seconds.
+   - Each plotted point on the graph represents the RTT of a packet. If you are not seeing anything in your graph, then you might have selected an opposite directional packet.
+   - RTT graphs are often used by network admins to identify any congestion or latency that can make your network perform slowly.
+   - To investigate further, just click on any plotted RTT dot in your graph, and Wireshark will point you to that specific packet in the list pane.

The following RTT graph represents normal web traffic, and at some points in the graph, latency can be observed: