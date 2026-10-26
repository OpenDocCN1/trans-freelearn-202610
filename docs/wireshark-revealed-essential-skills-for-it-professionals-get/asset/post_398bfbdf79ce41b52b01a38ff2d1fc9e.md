
- The traffic generated was also captured by our sniffer. Instead of displaying all the traffic, I used a display filter `ftp.request.command==PAS` in order to view only traffic that might be malicious. The following screenshot shows what display filter I used to query malicious repetitive packets.

![](img/da40f92928403866599198344271d63e_1290_0.png)

Figure 7.16: FTP Brute Force attack traffic pattern

- It is easily identifiable that the traffic is malicious because, in a span of maximum 85 seconds (calculated using the time column), there were approximately 10 password attempts made. This does look dangerous, and activities of such kind should be monitored closely in order to protect your resources facing the Internet.

There is one more way through which you can point out such traffic patterns. The best advisable option using Wireshark is to create a different coloring scheme using the same display filter expression that we used in order to point out the malicious traffic even faster. Refer to the following screenshot where I did the same and created a different coloring scheme for both TELNET and FTP traffic:

![](img/da40f92928403866599198344271d63e_1290_1.png)

Figure 7.17: Coloring scheme for malicious traffic

There are various other application layer protocols (HTTP, SSH, SMTP, and so on) that fall prey to these brute forcing techniques and might result in heavy losses for corporate infrastructures. In order to make these services secure, you can force encryption over the service that you are configuring and use strong password policies, such as an alphanumeric password with minimum length. You can also enforce a password change policy at a regular intervals, such as 30 days or something. Last but not least, you can make the employees aware of such activities. Any form of social engineering attacks executed against an employee can leverage the attacker to gain access to the infrastructure more easily.

## Inspecting malicious traffic

In some previously mentioned topics, we have witnessed a few scenarios that generated malicious traffic. Some of the common protocols, such as HTTP, DNS, ARP, IRC, that are seen in the list pane can carry malicious traffic. So, knowing about the malware traffic analysis is definitely an important skill every network and security professional should be well versed with. In today's digital world, various advances have been made. Yet, threats including malware infection persist. Every organization should consider threats of such nature to be critical. For illustrating the threats that are caused due to various malicious traffic, I have configured a few things in my virtual lab. The traffic generated because of the activities between the client and the server would be captured in parallel, which we will use to analyze later. Refer to the following screenshot:

![](img/da40f92928403866599198344271d63e_1292_0.png)

Malwares are supposed to perform a couple of tasks once installed on the victim's machine, such as passing on the secret content to the person in command, receiving commands from the server, and infecting and corrupting systems. Even if you have the best security solutions installed in your infrastructure, you are still open to wide attack vectors, including malware infections.

Now, we have understood the basics of how malicious traffic is being generated, and we also have a clear image of the infrastructure that we will work with. So, without wasting even a second more, let's go ahead and start the process. Follow these steps if you want to replicate the scenario in your own virtual lab:

- You require three machines connected to the same LAN. Make sure they are able to talk to each other, that is, verify the connectivity.
- On the IP address `192.168.1.106` stays a legitimate website, which the client is habituated to visit. However, this time, the client is not aware of the infection that causes redirection to another webserver. Refer to the following screenshot of the legitimate server:

![](img/da40f92928403866599198344271d63e_1293_0.png)

Figure 7.18: Legitimate website

- To simulate the redirection, I have configured my Apache server running on 106 to redirect every request coming to IP `192.168.1.100` and download the `efg.exe` malware from there.
- So, next time the client visits the website running at `192.168.1.106`, it gets redirected to a new webserver address, which directly asks the client to run a file named `efg.exe`. Refer to the following screenshot:

Figure 7.19: Client gets redirected to IP 192.168.1.100 and is asked to run the application.

- If the client clicks on **Run** they might not be aware of the dangerous effects the malware can pose to the client's machine and the network client is a part of. The publisher of the application is not verified, so the browser is not able to verify it. This results in giving an unknown publisher error. If the client still proceeds and clicks on **Run**, the malware will be installed. Refer to the following screenshot:

![](img/da40f92928403866599198344271d63e_1294_0.png)

![](img/da40f92928403866599198344271d63e_1294_1.png)

# Figure 7.20: Unknown publisher error

- Now, let's suppose that, if the client hits run, then the malware will be downloaded to the client's machine. It will be executed later on, thus creating a connection back to the command and control center.
- If the connection back to the attacker was successful, then without the knowledge of the client, the attacker can copy files, delete files, take screenshots, take webcam snaps, record voice through the mic, corrupt system files, and so on. You might have heard of various malwares such as ransom wares, spywares, and adwares.
- The whole traffic generated because of all these activities is being captured. Let's take a look at it. Instead of showing you the traffic, I assembled the TCP stream first between the client and the legitimate server.
- To understand the way our malware works, we need to look at more details, which can be presented to us by Wireshark. Refer to the following screenshot that shows the assembled TCP stream:

![](img/da40f92928403866599198344271d63e_1295_0.png)

# Figure 7.21: TCP stream between the client and real (compromised) server

As you can clearly see, the client is trying to visit the webserver, and the request is being forwarded with HTTP redirection to the new address `192.168.1.100`, trying to download the malicious file.

- Once the client gets a redirection response, the client again initiates a three-way handshake with the new server and tries to download the file. After a couple of packets were exchanged between the hosts in the later frames, the clients received a `200 OK` status message, suggesting successful download of the malware.

![](img/da40f92928403866599198344271d63e_1296_0.png)

In the following screenshot, you can see that the malware signature can be easily recognized by any `IDS/IPS` in place:

![](img/da40f92928403866599198344271d63e_1297_0.png)

Figure 7.22: Malware signature

The `GET` request was initiated by the client in search of cfg.exe, to which the server responded with a `200 OK` status message. Later, you can see the known malware signature starting with the characters `MZ` followed by some random character. A quick Google search regarding the same will reveal its behavior and pattern. Our search also reveals that it is an executable file, as Wikipedia states 16/32 bit DOS executable files can be identified by the letters `MZ` at the beginning of the file in ASCII. Refer to the following screenshot:

Until this point, it's clear that this is a Windows executable file which might be malicious.

Moving on with our investigation regarding the malicious file, I would like to export the `efg.exe` file using Wireshark.

1. Go to **File | Export Objects | HTTP**. You will see a dialog similar to the one shown here:

![](img/da40f92928403866599198344271d63e_1298_0.png)

Figure 7.23: Exporting HTTP objects

2. Now, to export the file, you need to select the conversation that states the name of the file along with it. Then click on **Save As** and save the file at a location of your choice.

3. The best option would be to upload this file to websites such as [http://www.virustotal.com](http://www.virustotal.com), which will cross examine the PE-executable file with numerous antivirus software online and will show you a detailed analytical report. Refer to the following screenshot:
