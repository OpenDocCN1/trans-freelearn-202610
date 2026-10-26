# # Dynamic Host Configuration Protocol Version 6

**Dynamic Host Configuration Protocol Version 6 (DHCPv6)** is the IPv6 version of DHCP. Since IPv6 doesn't use broadcasts, DHCPv6 clients use the multicast address for `All_DHCP_Relay_Agents_and_Servers (ff02::1:2)` to locate DHCPv6 servers or relay agents.

# ## Wireshark DHCPv6 filters

**Capture filter(s):** `port 546` (DHCPv6 is between ports `546` and `547`; either will work)

**Display filter(s):** `dhcpv6 dhcpv6.msgtype == 1` (DHCPv6 Solicit message)