+   - The traffic generated was also captured by our sniffer. Instead of displaying all the traffic, I used a display filter `ftp.request.command==PAS` in order to view only traffic that might be malicious. The following screenshot shows what display filter I used to query malicious repetitive packets.

![](bbox=[0.122, 0.171, 0.93, 0.364])

Figure 7.16: FTP Brute Force attack traffic pattern

+   - It is easily identifiable that the traffic is malicious because, in a span of maximum 85 seconds (calculated using the time column), there were approximately 10 password attempts made. This does look dangerous, and activities of such kind should be monitored closely in order to protect your resources facing the Internet.

There is one more way through which you can point out such traffic patterns. The best advisable option using Wireshark is to create a different coloring scheme using the same display filter expression that we used in order to point out the malicious traffic even faster. Refer to the following screenshot where I did the same and created a different coloring scheme for both TELNET and FTP traffic:

![](bbox=[0.072, 0.693, 0.931, 0.81])

Figure 7.17: Coloring scheme for malicious traffic