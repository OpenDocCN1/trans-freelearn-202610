# How to do it...

When experiencing many TCP retransmissions, perform the following tests:

+   1. Check whether retransmissions are coming from the same application or from the same IP address. In this case, it is a slow application or a slow device and probably not a network delay issue.

If retransmissions are distributed between various applications and devices, it can be because of unstable line that causes network delays.

+   2. Configure a display filter tcp.analysis.retransmissions (numbered 1 in the following diagram). A list of all retransmissions in the packet list will appear.

![](bbox=[0.12, 0.33, 0.93, 0.7])

+   3. Down the packet details pane, expand the TCP Analysis Flags, and you will get:
+     - The time since the original packet is retransmitted (numbered 2 in the preceding diagram). In this case, 0.225003000 seconds.
+     - The packet that is retransmitted (numbered 3 in the preceding diagram). In this case, packet number 1779.

+   4. Usually the Retransmission Time Out (RTO) timer will be around 0.2 seconds for local connections, and up to 0.3 to 0.4 seconds for international connections. Start with assuming 0.2 seconds. Refer to the How it works... section in this recipe