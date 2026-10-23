# # Technical requirements

The following tools and resources are used throughout this chapter:

+   * **AlienVault OTX**: `https://otx.alienvault.com`

# # Threat intelligence explained

Threat intelligence is knowledge, the knowledge of threats that are relevant for your organization and your particular environment. Threat intelligence is the result of a methodical process where evidence-based analysis from a variety of data sources leads to actionable data that can help uncover or eradicate threats that are most likely to be encountered on your network and in your environment. Threat intelligence produces deep insight into adversaries that target your organization, industrial sector, or operational space, and how they perform their malicious **Tactics, Techniques, and Procedures (TTPs)**. This knowledge allows us to make more effective and precise decisions about how we go about securing our environment.

Many so-called threat intelligence sites will try to sell you what they call threat intelligence. By definition, this is not possible. Unless a company is highly familiar with your environment and can perform the process of transforming threat information into actionable intelligence data for you, the best they can offer is threat information. In this sense, threat information is merely a list of **Indicators of Compromise (IOCs)**. As discussed before, IOCs are technical bits of information that characterize a threat. They can be **Internet Protocol (IP)** addresses, domain names, filenames and locations, registry keys, file hashes, and more. IOCs make it possible to do threat detection, threat hunting, and the correlation of artifacts found within your environment to established and verified artifacts of malicious actors, actions, content, or activities.

As an example, let's say we identified a system in our environment communicating with `http[:]//148.251.82.21/Microsoft/Update/KS4567890[].php`. There is just something suspicious about this URL, isn't there? Well, if we do a quick lookup of the domain name or IP address on AlienVault's threat exchange site (`https://otx.alienvault.com/indicator/ip/148.251.82.21`), we can see that our suspicion is valid: