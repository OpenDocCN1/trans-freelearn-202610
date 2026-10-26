+   1. An original large packet enters the NIC or the router with a packet size that needs to be fragmented.
+   2. The packet is fragmented into several parts depending on the original size.
+   3. For the fragmentation, we have these fields:
+      - **ID**: This is identical to the ID of the original packet
+      - **Bit 0**: Always 0
+      - **Bit 1 (DF Bit)**: 0 = May Fragment, 1 = Don't Fragment.
+      - **Bit 2 (MF Bit)**: Don't Fragment: 0 = Last Fragment, 1 = More Fragments
+      - **Fragment offset**: This indicates the number of bytes from the beginning of the original packet

In IPv4, the NIC itself can fragment the packet along with every router on the way to the destination.

In IPv6, fragmentation can be done only by the sender and not by the routers to the destination. In IPv6, fragmentation is implemented by the extension headers.