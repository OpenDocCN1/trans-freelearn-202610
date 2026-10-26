port scanner on the other side to display statistics. If the port was open, the malicious user will be notified with SYN and ACK (refer to the following screenshot), which signify that our machine is open to a connection over the port 21, and this might become an entry point to the user's malicious attacks.

![](bbox=[0.07, 0.16, 0.92, 0.29])

Figure 5.3: Port 21 open, an entry point for malicious attacks

Take a look at Frame 45, where the client sent a SYN request to the server at 172.16.136.1, and by this time, the port was open so our server sent SYN and ACK packets (Frame 46), acknowledging the connection initiation attempt with a positive confirmation that the server is open to connection over port 21.

There can be various scenarios other than this half-open scan (the scan shown in the preceding screenshot is called half open because the client who initiated the connection attempt, would never complete the connection by sending ACK, which the server will be expecting). If your basics regarding the packet behavior, connection initiation, completion process, TCP headers, flags in packets, and SEQ-ACK numbers are clear, then it would be quite easy for you to point out any unusual form of traffic that is flowing around. There is no such automated tool that can point out these abnormalities until you customize your environment about how to react or alarm you to such traffic anomalies. These are some traffic patterns that you can expect to happen on a regular basis.