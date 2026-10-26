# Basic network connectivity

A few simple tests can confirm that basic network connectivity is working, or reveal a routing issue or another issue that needs to be addressed by the network support team.

Capturing and analyzing the ICMP packets sent and received during the following tests can be revealing; although, the test results themselves are often telling enough:

+   - Ping the user's default gateway using the default gateway IP address obtained from using `ipconfig /all` (Windows) or `ip addr show` (Linux) to confirm that the user workstation has basic connectivity on the local network.
+   - Ping the hostname or URL of the target server. If this fails (request timed out message), try to ping other hosts or URLs. If necessary, inspect the DNS and/or ICMP responses in a packet capture of these tests to determine the nature of the failure. Otherwise, take note of the average round trip times.
+   - If a ping works to the default gateway but pinging other targets fails, a traceroute to a target server can reveal where in the network path connectivity ceases to function or is blocked.

# Note

The traceroute command-line utility in Windows is `tracert`, whereas for traceroutes on Linux/Unix and Mac OS X machines, the command is `traceroute`. To do a traceroute in Windows, open a Command Prompt (CMD) window and type `tracert <hostname or IP Address of target>`. In most other environments, open a terminal window and type `traceroute <hostname or IP address of target>`.

If you can ping the target server and network connectivity is functioning, you can move on to the next step in the troubleshooting process. If not, be aware that some hosts may be configured to not respond to ICMP ping requests, and/or ICMP is blocked by a firewall between the user and server for security reasons. So, the inability to ping a device is not necessarily a sign of a network problem. Traceroute results should help determine how far and to what extent network connectivity is functioning in the path towards the target server; testing to other targets should be revealing as well.

An example of pinging a default gateway, then a URL, and finally performing a traceroute to the target URL is depicted in the following screenshot: