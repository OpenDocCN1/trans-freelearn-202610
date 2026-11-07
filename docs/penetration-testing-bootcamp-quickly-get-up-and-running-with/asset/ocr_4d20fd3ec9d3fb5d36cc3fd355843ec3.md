# 2

# Information Gathering

With all the information that was received from the scoping meeting, it is now time to not only validate that information, but also learn as much as you can from your own information gathering research. The goal is to gain as much knowledge as possible about the network and systems before starting to scan for vulnerabilities and then exploiting them.

In this chapter, you will learn to use various tools to start and map out the network and systems and then enumerate your findings. The more information you can get from this phase, the easier it will be to find vulnerabilities and exploits. This step can save you a lot of time later in the lab. For example, if you learn that a web server is a Microsoft Windows 2012 server, you can utilize this information for a better understanding of how to approach the exploitation phase. Without this information, you may try a bunch of exploits against this server but they will not work because they are not meant for a Windows 2012 server.

With all that being said, there is always the temptation to speed through this phase after you find some systems you would like to probe deeper into. You must, at all costs, resist this action. You must spend a good portion of your allocated penetration-testing timeframe during this phase. The actual time spent here will depend on the overall engagement time. I cannot emphasize enough; this is probably the most important phase in the overall penetration test. The more the time you spend understanding the environment and targets, the less the time you will waste in other phases of the penetration test.

In this chapter, we will discuss the following topics:

+   - Various tools in the toolbox
+   - Whois, dnsmap, and DNSRecon
+   - Nmap
+   - P0f
+   - Firewall Dotdotpwn