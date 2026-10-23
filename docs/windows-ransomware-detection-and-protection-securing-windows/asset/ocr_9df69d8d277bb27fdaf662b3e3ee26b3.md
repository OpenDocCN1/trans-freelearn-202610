+   - Boot
+   - Windows
+   - WinNT

However, if you have a different partition layout or data such as the domain controller’s database stored on another partition, for instance, it will get encrypted. Conti also skips some file extensions such as .exe, .dll, .sys, and .lnk. After it is done with the encryption, all files have a .CONTI extension, and within each folder, it also creates a ransom note.

# Sodinokibi/REvil ransomware

Sodinokibi/REvil is maybe the most prolific ransomware group on our list. They were the ones behind the infamous Kaseya VSA supply chain attack, and they were also behind the attacks on other large companies such as Travelex and JBS Foods. JBS Foods, which is also the world’s largest meat producer, ended up paying 11 million dollars to REvil to get access back to their data.

Like the other ransomware operators mentioned earlier, REvil has been known to use malware loaders such as IceID, as well as using different brute-force attacks and exploiting known vulnerabilities such as FortiOS VPN, Pulse VPN, BlueGate, Citrix, and Oracle WebLogic Server, to name a few.

They are also one of the ransomware operators that first started targeting VMware ESXi virtual machines. They used the built-in ESXCLI command line to force stop the virtual machines and then encrypt data directly at the VMware datastore level.

For one customer that I was working with that got hit with Sodinokibi, the initial point of entry was a compromised virtual machine (via RDP) in Azure, which was then used to access the virtual infrastructure.

Like the others, REvil also had a collection of scripts and utilities that they use to do reconnaissance of the network. One thing, however, that sets them a bit apart, is that they were able to restart virtual machines in safe mode with networking and still be able to run their payload. The advantage was that they were able to run their payload and disable any EDR services on the machines before rebooting back to default mode.

Fortunately, in early 2022, the Russian government arrested multiple key resources behind the REvil ransomware group on request from the US; you can read more about it here: https://www.wsj.com/articles/russia-says-it-raided-prolific-ransomware-group-revil-with-arrests-seizures-11642179589.

# LockBit ransomware

One of the most common ransomware groups at the time of writing is LockBit, which has impacted a lot of large organizations since its emergence back in 2019, such as Accenture, which was hit in late 2021.