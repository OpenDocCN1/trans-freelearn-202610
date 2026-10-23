# Using pfSense to fake the DNS query

In order to have our lab pfSense firewall that we deployed as part of exercise 2 in Chapter 6, Passive Security Monitoring, send custom DNS responses, we will add a host override entry in the pfSense DNS resolver configuration. Follow along with the instructions to get a host override in place on our pfSense firewall:

+   1. Log in to the pfSense web portal (https://172.25.100.1/) and navigate to Services – DNS Resolver. Make sure the service is enabled, then scroll down to the bottom where the host and domain override sections are:

![](bbox=[0.115, 0.255, 0.85, 0.415])

Figure 11.59 – pfSense – DNS overrides

+   2. Click on Add in the Host Overrides section and enter the following details to create a custom DNS response:

![](bbox=[0.13, 0.485, 0.83, 0.795])

Figure 11.60 – pfSense – add DNS host override