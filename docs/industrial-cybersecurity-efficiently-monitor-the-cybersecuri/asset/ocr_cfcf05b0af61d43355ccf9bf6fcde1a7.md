# Technical requirements

The following tools and resources are used throughout this chapter:

+   - **Notepad++:** https://notepad-plus-plus.org/
+   - **Autoruns:** https://docs.microsoft.com/en-us/sysinternals/downloads/autoruns
+   - **munin:** https://github.com/Neo23x0/munin
+   - **VirusTotal API key:** https://www.virustotal.com/gui/join-us
+   - **YARA:** https://virustotal.github.io/yara/
+   - **CrowdResponse:** https://www.crowdstrike.com/resources/community-tools/crowdresponse
+   - **yara_scanner:** https://github.com/tsale/yara_scanner
+   - **psexec.exe:** https://docs.microsoft.com/en-us/sysinternals/downloads/psexec

# Forming the malicious or unwanted applications threat hunting hypothesis

As we discussed in Chapter 10, Threat Hunting, threat hunting exercises are geared around hypotheses. Typically, hypotheses follow or reflect a discovered security incident or some form of an alert from an automated security monitoring system or a finding from a security analyst. The threat hunting exercise we will be performing in this chapter works a bit differently; the environment we are going to perform threat hunting activities in is unfamiliar or new to us. For example, our company purchased a new production plant with an existing ICS network infrastructure they are looking to tie to the existing company infrastructure, and we want to make sure that it is safe to do so. Another scenario would be when you have run your ICS network for ages but never seriously looked at the state of its security posture; in this case, these "unprovoked" types of threat hunting exercises can quickly and accurately give you an idea of malicious activity or content in your ICS environment.