```
| serialNumber: 0x005eae98
| productCode: 166
| revision: 5.28
| status: 0x0030
| state: 0x03
|_ deviceIp: 172.25.100.11
MAC Address: 00:00:BC:5A:D0:56 (Rockwell Automation)
```

We can take that information to an online service such as CVE Details (https://www.cvedetails.com/) and do a search on 1756-en2t:

![](bbox=[0.115, 0.32, 0.85, 0.785])

Figure 7.8 – CVE Details – 1756-en2t results

Now, we can see whether there are any known issues with the device.

This is a very labor-intensive process and prone to missing vulnerabilities or the discovery of false positives. Therefore, next, we are going to look at how to automate this process with well-known vulnerability scanning tools.