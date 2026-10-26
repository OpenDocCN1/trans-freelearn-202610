What I got when I connected Wireshark to a remote site (as illustrated below) was many ICMP requests (3), coming from the LAN 192.168.110.0 (1) to random destinations (2). Was it random?

![](bbox=[0.17, 0.133, 0.927, 0.357])

Also, look at the time between packets. If scanned, it will usually be very short.

When you go to Statistics | Conversations, you will see something interesting:

![](bbox=[0.17, 0.508, 0.927, 0.836])

When we sort the table by address A (1), we see a pattern of ICMP requests