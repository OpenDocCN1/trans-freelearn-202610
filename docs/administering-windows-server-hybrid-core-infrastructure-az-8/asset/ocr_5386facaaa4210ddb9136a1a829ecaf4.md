# # Implementing and managing DHCP

This section will look at managing on-premises IP addresses using the DHCP network service.

The first skills area we will look at in this section is what is DHCP?

# ## What is DHCP?

DHCP can be considered a network service component of a DNS-DHCP-IPAM (DDI) solution; we also cover DNS and IPAM in this chapter.

Concisely, DHCP is a network management protocol used to simplify IP address management through auto-assigning IP addresses and network information.

DHCP is an extended version of Bootstrap Protocol (BOOTP). The difference between the two is that BOOTP provides static configuration of IP addresses, and DHCP provides automatic configuration of IP addresses.

Without DHCP, there would be a need for manual network IP address configuration on the network interface for the required network settings and manual assignment of IP addresses; this is highly inefficient and error-prone.

With regards to DHCP version 6, this provides IPAM in IPv6 networks. Stateful and stateless configurations are supported.

The DHCP server assigns the IPv6 addresses and network settings in stateful configurations. In stateless configurations, the router assigns the IP addresses, and the DHCP server assigns only the network settings.

# ## How does DHCP work?

DHCP works on the traditional client/server relationship model. Windows computers run the DHCP client service, and when set to Obtain an IP address automatically, they will communicate with a DHCP server to receive their network information. This setting is found in the Internet Protocol Version 4 (TCP/IPv4) Properties dialog, as shown in the following screenshot: