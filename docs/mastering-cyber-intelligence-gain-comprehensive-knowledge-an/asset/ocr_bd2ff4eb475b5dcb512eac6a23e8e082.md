**Use Case: SolarWind Attack**
**Summary**: APT compromising public and private sectors from March 2020.
**Initial access**: DLL poisoning – supply chain in SolarWinds Orion platform's code by weaponizing `solarwinds.orion.core.businesslayer.dll`.
**C2 domains**: `avsvmcloud.com`, `deftsecurity.com` – mimic SolarWinds traffic (T1195.002). IP addresses: `20.140.0.1`, `13.59.205.66`, `65.153.203.68`.
**Evasion tactic**: T1027.003 – C2 traffic obfuscation using virtual private servers with victims' local IP addresses.
**Privilege escalation**: TA0004 tactic – authentication tokens and credentials addition in highly privileged directories.
**Persistence**: TA0003 tactic – adding login credentials and system process modification T1543.003.
**Command and control**: Web protocols – T1071.001.
**Data exfiltration**: Collect information – compromise the **Security Assertion Markup Language (SAML)**. Reference: https://bit.ly/3EuLjwW.

Given the preceding case, we see how intelligence can help reactive and proactive IR. The SolarWinds data breach is used as an example, and the MITRE ATT&CK TTPs are used as a reference.

# Threat intelligence and reactive response

IR is reactive by default. It means that the IR analysts come out to play when threats are detected. Typical reactive IR will work as follows:

+   1. The SIEM system flags an *alert* on the `avsvmcloud.com` domain from the proxy server logs. It indicates a potential indicator of compromise.
+   2. CTI analysts (manual or semi-automated) or the SIEM or TIP tool (automated and recommended) correlate the detected domain against external intelligence to *contextualize* the IOC. They can use platforms such as VirusTotal and malware feeds. They map the IOC to the concerned APT group. At this step, the CTI analysts extract other IOCs (IP addresses, hash values, and additional domains) associated with the group.
+   3. The system (SIEM, TIP, or SOAR) *prioritizes* the alert and tags it as critical, needing the full attention of the IR and CTI teams.