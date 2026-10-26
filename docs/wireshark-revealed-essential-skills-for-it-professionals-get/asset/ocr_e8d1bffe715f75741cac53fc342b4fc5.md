# DNS slow responses

How will you know that this is the problem?

+   1. When you are browsing the Internet and getting very slow responses, perform the following steps:
+      1. Port mirror the connection to the Internet, and check if you have any bottleneck on the way to the Internet. You can use the IO graphs for this purpose, as described in Chapter 5, Using Advanced Statistics Tools.
+      2. Verify that you don't have a significant number of retransmissions or duplicate ACK's indicating a connection problem.
+      3. Verify that you don't have any window-related problem, such as zero window or window full.
+   2. If answers are no for the preceding checks, it might be a DNS problem. You can have DNS problems in two cases:
+      - When working in your organization
+      - When connecting to the Internet
+   3. These issues can be resolved in two ways:
+      - When facing problems in your organization, port mirror the switch port that is connected to the DNS server
+      - When facing problems with the Internet, port mirror the switch port that connects your organization to the Internet
+   4. Watch the DNS response time that you get. There are several ways to locate the problem, and they are given as follows:
+      - The simplest way is to right-click on a packet from a DNS query stream, choose Follow UDP Stream, and then check the time between the query and response.
+      - Another way is to use IO graphs for this purpose. In the IO Graphs window, choose Advanced in the Y Axis configuration and configure the filter dns.time with AVG(*) in the Graph lines. Refer the following screenshot: