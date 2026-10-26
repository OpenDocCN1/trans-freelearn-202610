# There's more...

In cases where the capture time is important, and you wish to capture data on one interface or more, and be time-synchronized with the server you are monitoring, you can use **Network Time Protocol (NTP)** to synchronize your Wireshark and the monitored servers with a central time source.

This is important in cases when you want to go through the Wireshark capture file in parallel to a server logfile, and look for events that are shown on both. For example, if you see retransmissions in the capture file at the same time as a server or application error on the monitored server, you will know that the retransmissions are because of server errors and not because of the network.

The Wireshark software takes its time from the OS clock (Windows, Linux, and so on) For configuring the OS to work with a time server, go to the relevant manuals of the operating system that you work with.

In Microsoft Windows7, configure it as follows:

+   1. Go the **Control Panel**.
+   2. Choose **Clock, Language, and Region**.
+   3. Under **Date and Time**, Choose **Set the time and date** and change to the **Internet time** tab.
+   4. Click on the **Change Settings** button.
+   5. Change the server name or the IP address.

# Note

In Microsoft Windows7 and later versions, there is a default setting for the time server. As long as all devices are tuned to it, you can use it as any other time server.

NTP is a network protocol used for time synchronization. When you configure your network devices (routers, switches, FWs, and so on) and servers to the same time source, they will be time synchronized to this source. The accuracy of the synchronization depends on the accuracy of the time server that is measured in levels or stratums. The higher the level, the more accurate it will be. Level 1 is the highest. Usually you will have levels 2 to 4.

NTP was first standardized in RFC 1059 (NTPv1), and then in RFC 1119 (NTPv2); the common versions in the last years are NTPv3 (RFC1305) and NTPv4 (RFC 5905).