This instructs Nmap to scan every port (`-p-`) of every system on the `172.25.100.0/24` subnet and collect any data (`-A`), saving the results in the four main formats by the name of `ot-scan` (`-oA ot-scan`).

Once we are done, save the Wireshark output to a **packet capture (PCAP)** file and import it into Grassmarlin:

+   1. Start Grassmarlin and select **File | Import Files**.
+   2. Click **Add Files** and navigate to the saved PCAP file.
+   3. Click **Import Selected**.

![](bbox=[0.139, 0.309, 0.862, 0.856])

Figure 17.16 – Grassmarlin – Import PCAP