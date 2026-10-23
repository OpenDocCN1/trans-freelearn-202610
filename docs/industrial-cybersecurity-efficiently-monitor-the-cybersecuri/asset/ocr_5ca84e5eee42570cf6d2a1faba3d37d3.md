19. The next couple of screens have us specify IP address details such as static versus DHCP; the IP address if we choose static, netmask, gateway; the domain name; an finally, the DNS server for the network we are on. Configure this according to your setup (I am using a static IP address of 172.25.100.250). The process is illustrated in the following screenshot:

![](bbox=[0.16, 0.21, 0.84, 0.35])

Figure 6.24 – Exercise 1: Installing Security Onion – management interface DNS search domain

After entering the configuration details for the management interface, the installer will initialize the network.

20. Next, we need to specify the physical adapter that will be used as a monitoring (sniffing) interface. Select the remaining interface and hit Ok. The process is illustrated in the following screenshot:

![](bbox=[0.16, 0.5, 0.84, 0.7])

Figure 6.25 – Exercise 1: Installing Security Onion – selecting the monitoring interface

21. On the next install screen, select Automatic as the operating system patch schedule to allow Security Onion to update certain parts of the appliance. Hit Enter to continue.

22. Now, we need to specify the HOME_NET IP address range. This should be set to cover the range of the industrial network (172.25.100.0/24 for the lab network) and allows Security Onion to differentiate between internal and external network addresses, used for several secrecy tools and checks. Set the IP address range and hit Enter to continue.