We identify the compromised hosts' IP and names, as highlighted in the preceding screenshot (192.168.8.4, 192.168.8.14, and 192.168.8.23). These hosts have communicated with the C2 server (125.19.103.198). We also see a File Transfer Protocol (FTP) URL used during communication (ftp://admin.test@125.19.103.198:21/sys/files/?query=.../), with the object's Multipurpose Internet Mail Extension (MIME) set to DOC/RAR files (possible file exchange using FTP).

+   2. Compare the results of the different internal analyses. Ensure that Step 3 matches and confirms the findings in Step 1. For example, we see that timestamp patterns in Step 1 match the proxy server logs in Step 3. In that case, this is a good indication of a potential compromise and validation of the third-party report. We need to involve the incident response (IR) and forensics teams. However, there is still analysis we need for more evidence. A comparison between Step 1 and Step 3 is shown in the following screenshot:

![](bbox=[0.12, 0.42, 0.88, 0.68])

Figure 10.6 – Matching the network and proxy logs' timestamps