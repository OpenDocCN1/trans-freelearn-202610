# ## Creating threat intelligence data out of threat information

Creating threat intelligence data out of threat information is the cornerstone activity of threat intelligence efforts. It is also the most challenging part of the process. A good approach is to start by eliminating threat information that is not relevant to your environment. As an example, if you do not have Siemens equipment, delete any threat information that you pulled in from your threat information sources that is related to Siemens, or add a relevance attribute to your threat information and assign a low score to any Siemens-related information. On the opposite side, for equipment brands and vendors you do use, add a higher relevance scoring, tied to the criticality of the equipment.

The next step in the threat intelligence creation process would be to start adding additional data, that is, additional attributes to the threat information that is most relevant to you. Add information such as WHOIS data, DNS and reverse DNS results, news articles, peer discussions, additional artifacts, and more. Don’t forget to include information that details how to address, prevent, or detect a threat.

The MISP framework we discussed in the previous section lends itself well to threat intelligence creation activities.

That is all for our discussion of threat intelligence. There is more to the topic but because most of those deeper discussions would be highly dependent on your organization, sector, business, and environment, I will leave that as an exercise to you. Let’s next look at a way that threat information can enhance your security monitoring activities by adding a threat information feed to the Security Onion VM we built in *Chapter 6, Passive Security Monitoring*.

# ### Exercise – Adding an AlienVault OTX threat feed to Security Onion

As an example of what threat information can add to overall security monitoring effectiveness, we shall add a threat feed to our Security Onion deployment. As mentioned throughout this chapter, threat feeds, or IOC feeds, by themselves are not threat intelligence; however, adding a threat feed to your SIEM does allow you to perform some rudimentary threat intelligence activities.