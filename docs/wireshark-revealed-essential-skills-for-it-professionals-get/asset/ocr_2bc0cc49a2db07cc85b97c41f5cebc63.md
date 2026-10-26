# # Top five reasons for poor application performance

Generally speaking, performance issues can be attributed to one of the following five areas, in order of decreasing likelihood:

+   - Server processing time delay
+   - Application turns delay
+   - Network path latency
+   - Bandwidth congestion
+   - Data transport (TCP) issues

Client processing time is usually a relatively small component of overall response time—except perhaps for some compute-extensive desktop applications, which leaves the focus on the network and server environments and any performance-affecting application design characteristics.

# ## Preparing the tools and approach

As was done when preparing to troubleshoot a connectivity or functionality problem, you'll need to gather the right information about the application environment and problem domain. You'll also want to determine which tools you may need to use during the analysis: Wireshark, TAPs to facilitate packet captures, and any other analysis tools.

You will also need to determine where to perform the first packet capture:

+   - A client-side capture is the best place to begin a performance analysis effort. From this vantage point, you can view and verify what the user is complaining about, view any error messages presented to the user or evident in the packet capture, measure network round-trip times, and capture the performance characteristics to study within a packet capture without the need to use a capture filter so you know you won't miss anything.
+   - A server-side capture may be needed because a client-side capture may not be possible for a user that is at a long distance, or to analyze server-to-server transactions to backend databases or other data sources.
+   - A packet capture at some intermediate point in the network path may be needed to isolate the source of excessive packet loss/errors and the associated retransmissions.