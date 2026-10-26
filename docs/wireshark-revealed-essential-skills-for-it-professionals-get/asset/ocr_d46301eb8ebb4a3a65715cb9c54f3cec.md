# How to do it...

When you monitor internal traffic in your organization, the following things should be checked:

+   1. Traffic that is generated from known addresses (in the organization):
+      - **Normal:** This is the traffic from known addresses and address ranges
+      - **Suspicious:** This is the traffic from/to addresses that you don't know
+   2. Applications and port numbers:
+      - **Normal:** This includes standard port numbers, 80 (HTTP), 137/8/9 (NetBIOS), 3389 (RDP), 20/21 (FTP), 25,110 (Mail), 53 (DNS), and so on. Be sure of the applications that run over the network, and verify that these are the only port numbers that you see.
+      - **Suspicious:** This includes unusual port numbers, that is, port numbers that do not belong to applications that run on server (for example, RDP packets to web server).
+   3. TCP patterns:
+      - **Normal:** TCP SYN/SYN-ACK/ACK that indicates a connection establishment, single reset (RST) that indicates a fast connection tear-down, FIN/FIN-ACK packets that indicate a regular tear-down of a connection, standard packets, and acknowledgments
+      - **Suspicious:** Large amount of SYN packets that go to a single or multiple destinations or coming from multiple sources (usually in a scan pattern that will be described later in this chapter), unusual flags combination (RST/FIN, URG), and so on
+   4. Massive traffic to a single or multiple sites that you don't know about:
+      - **Normal:** Traffic patterns are usually not of fixed bandwidth. When you save or open files, browse the Internet, send or receive mails, or access a server with RDP, you see ups and downs.
+      - **Suspicious (in some cases):** Fixed bandwidth patterns can indicate that someone is connected to your device, but it can also indicate that someone is listening to the radio over the Internet (100-150 Kbps), watching video (in some cases), and so on. When you see a fixed bandwidth pattern of traffic, check what it is. A fixed bandwidth pattern is illustrated in the following screenshot: