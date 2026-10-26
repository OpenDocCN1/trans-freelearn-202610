![](bbox=[0.073, 0.064, 0.927, 0.439])

# ## Wireshark ICMP filters

**Capture filters(s):** `icmp`

**Display filter(s):** `icmp icmp.type==8 || icmp.type==0 (pings) icmp.type==5 && icmp.code==1 (host redirects)`