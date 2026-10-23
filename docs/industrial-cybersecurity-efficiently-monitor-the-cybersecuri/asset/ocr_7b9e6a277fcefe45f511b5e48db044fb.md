We can now take the media access control (MAC) address and look up the vendor via an online tool such as https://macvendors.com/, as illustrated in the following screenshot:

![](bbox=[0.295, 0.165, 0.7, 0.295])

Figure 16.13 – MAC address lookup

If we find any automation vendors, we know where to put our attention next. Additionally, we can perform port scanning on the found systems and see if there are any open ports that are typically found on automation devices, such as 2222, 44818, 502, and so on. The function to use within the implant is portscan, as illustrated here:

```
TEST_REPORTSSER\reports_user @ TEST_REPORTSSER (PID:5976)
PS 12> portscan 10.0.0.100
```

This shows us the open ports for 10.0.0.100 in the posh-server screen, as follows:

![](bbox=[0.12, 0.505, 0.86, 0.86])

Figure 16.14 – Running portscan implant function