The SolarWinds IT Asset Management solution will not only characterize any endpoint and inventory installed software and applications – it will also trend the resource utilization of a system over a long period of time, allowing for correlation and anomaly detection.

# Any modifications to files, registry, settings, and so on?

Being able to identify changes that have been made to files, settings, and a system's configuration, and then being able to narrow down those changes to data, users, or applications, can help narrow down the who, what, where, and when around a system compromise.

A tool such as Sysinternal's Sysmon (https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon) will record interesting changes that have been made to the system and can also send detection logs to an SIEM with the help of tools such as Wazuh (https://wazuh.com/). Sysmon and Wazuh were discussed in Chapter 9, Visualizing, Correlating, and Alerting.

# Sysinternals

If you haven't gotten the hint yet, Sysinternal's tools are tremendously helpful in getting all kinds of information about a Windows system. I only mentioned a few here, but there are many more that can aid you in a large variety of administrative, investigative/forensics, and maintenance activities. Head on over to https://docs.microsoft.com/en-us/sysinternals/ and become familiar with the many excellent (and free) tools the site has to offer.

This concludes our discussion around the tools and methods that can help us investigate, interrogate, and inventory endpoints and the software and applications they run. Next, we will look at the network side of things.