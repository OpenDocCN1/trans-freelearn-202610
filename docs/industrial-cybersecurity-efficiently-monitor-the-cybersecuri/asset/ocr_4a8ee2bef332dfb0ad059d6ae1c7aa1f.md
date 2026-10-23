With that list, let’s find out whether we have any other compromises in our environment.

# Searching for network-specific indicators of compromise

We can use the Security Onion Kibana portal to search for IP addresses, URLs, and other network (packet) indicators of compromise. So, let’s open the Kibana portal and start with the first IOC, the IP address 222.222.222.222 that we discovered as being the target IP address for the beacons that the malware is sending out. The following steps show the process of tackling this:

+   1. We enter the IP address in the search box of the **Security Onion - Home** dashboard to see what kind of logs are associated with that IP address:

![](bbox=[0.117, 0.373, 0.844, 0.747])

Figure 11.62 – Kibana – 222.222.222.222 – overview