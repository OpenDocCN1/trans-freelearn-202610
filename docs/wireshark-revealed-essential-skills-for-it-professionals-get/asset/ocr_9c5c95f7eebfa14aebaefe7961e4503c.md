# Getting ready

The first thing is to locate Wireshark. There are several options for this (see the following diagram):

+   1. When you suspect an attack that comes from the Internet, locate Wireshark after the firewall (1), and when you suspect that it crosses the firewall, locate it before (2).
+   2. When you suspect malicious traffic coming from a remote office, port mirror the traffic coming on the central line before (3) or after (4) the router. In this case, you can filter the suspicious traffic with IP networks to see patterns from different offices in order to isolate the problematic office.
+   3. You can also port mirror the traffic in the remote office before (7) or after (6) the routers.
+   4. When a PC or a server is the suspect, port mirror its port on the switch (5) or (8).

![](bbox=[0.07, 0.38, 0.81, 0.75])

Now, we will try to see what are the types of traffic that we should look out for, what are the types of traffic that are normal, and what traffic should be followed.

Before starting with the tests, make sure that you have an updated topology of the