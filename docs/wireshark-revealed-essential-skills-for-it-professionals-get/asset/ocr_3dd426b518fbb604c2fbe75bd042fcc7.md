# How to do it...

Connect Wireshark to the LAN switch attached to the monitored device, and configure port mirror to the device from which you suspect the problem is coming. Go through the following steps:

+   1. In case of user complains, configure the port mirror for monitoring the user device.
+   2. In case of a general problem in the network, configure port mirror to the DNS server:
+      - When the DNS server is configured on the internal server, configure port mirror on the server
+      - When the DNS server is configured on external server, configure port mirror to the link that connects you to the Internet