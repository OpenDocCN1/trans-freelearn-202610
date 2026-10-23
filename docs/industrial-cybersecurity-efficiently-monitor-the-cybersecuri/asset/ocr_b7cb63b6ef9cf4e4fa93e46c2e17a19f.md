Disproving these three hypotheses or unearthing any systems that fall under these categories allows us to find typical malicious activities in an existing ICS network/environment.

The next three chapters are geared around performing these particular types of threat hunting exercises.

# What is needed to perform threat hunting exercises?

So, what does it take to perform a threat hunting exercise? The short answer to this question is information – information about your assets, the network connections that traverse between your assets, and the interactions users have with your assets. The more detailed the information is you have about these three aspects, the more successful your threat hunt will be.

Although the exact types of data and information needed for the job depend on the specifics of the threat hunt, having the following types of information readily available will help you get started in most scenarios. Along the way, additional information might be needed and will have to be obtained to keep the threat hunt exercise going or to expand its coverage.

# Network traffic logs

Because we are dealing with networked systems and equipment that we are performing threat hunting exercises on, the foremost valuable information we should have at our disposal is network traffic and access logs. Think of firewall logs, switch logs, network intrusion detection/prevention system logs, and so on. Oftentimes, it is one of the aforementioned network devices that will provide the initial indication (alert) that things are amiss. But during the hunting exercise, knowing specifics around what traverses the network can help in ways no other information can.

At a minimum, you should make sure you are capturing network connection information, along with any event logs your network appliances may be generating.

A network monitoring solution such as Zeek (previously known as Bro – https://zeek.org/) can efficiently and effectively capture large amounts of network traffic (meta) data and information. We discussed Zeek in *Chapter 6, Passive Security Monitoring*, and it is in my "don't leave a customer without one" category of security monitoring tools.