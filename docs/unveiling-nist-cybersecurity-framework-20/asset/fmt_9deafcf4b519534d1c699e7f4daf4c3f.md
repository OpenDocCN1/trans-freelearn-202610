
# Incident Analysis (RS.AN)

We also want to preserve the integrity of the timeline to ensure that it was not altered in any way. This will come in need when we begin to discuss the chain of custody of the document and prevent alterations of the document once it is completed. You will want to ensure that the timeline is as accurate as possible in case it is to be submitted to law enforcement.

To ensure the integrity of the document, we can also use a hashing algorithm on the document itself. As we discussed previously, a hash is a digital fingerprint of the document, and that fingerprint changes when the document is altered in any way. This will protect the integrity of the document. However, it will not cover the authenticity of the document. To do this, you will need to leverage a digital signature. A digital signature will prove both the authenticity and integrity of the document as the person signing the timeline will need to be in possession of the private encryption key.

The private key used to generate the digital signature must be protected at all costs. If you lose possession of the private key, anyone could impersonate you and digitally sign the document without your knowledge. The digital signature will then be verified by your public key. The public key can be shared with anyone and will need to be shared in order to verify the signature is valid.

## RS.AN-07

*Incident data and metadata are collected, and their integrity and provenance are preserved.*

The definition of metadata is data about data. Metadata includes everything that made up the event, which includes timestamps, IP addresses, sources, destinations, and the identity of the person who made the event. This information should also be captured and placed into the timeline to better understand the characteristics that made up the event. This information will come in handy for chain-of-custody requirements.

Ensure that all the logs that are generated go to a centralized logging solution such as a log correlation engine or SIEM. You want to have all of your logs going to a centralized system in order to gain a full picture of the events that occurred. Having this metadata as part of the overall logs will provide additional insight into the adverse event and how it occurred. Eventually, we will want to understand the root cause of the incident; this will help with understanding what happened.

## RS.AN-08

*An incident’s magnitude is estimated and validated.*

The determination of an incident’s magnitude should be part of your policies and procedures documentation. We can calculate the incident’s impact on the organization by leveraging FIPS 199 and how that applies to the confidentiality, integrity, and availability of IT resources. This classification is helpful when determining the magnitude of an incident and what you and your team should do in response. For example, you could have a database that is down that only affects a certain amount of users within the company. On the other hand, you could have a system-wide outage affecting all users within the organization. Which one would take precedence?

When filing the IR report, ensure that this has been documented for historical purposes. A report should include not only the description, the players involved, and a timeline, but also the magnitude of the impact the incident had. In the previous database example, you could have a low impact if the database outage only affected 5% of the workforce, whereas a high impact would be warranted in the event a database was down for the entire company. Additional information can be found here: \[*3*\].

We will also need to finalize the source or the root cause of the incident and how that impacted the company. Take this information and create new or alter current runbooks to reduce the time it takes to respond to future incidents. This is important as it will help reduce the amount of time it takes to respond to future incidents.

# Incident response reporting and communication (RS.CO)

This control family is all about getting the word out when an incident strikes. It is not just internal stakeholders that you will have to notify though. You will also need to notify external stakeholders and maybe even regulatory bodies. States and the federal government have breach notification laws whereby you are required to notify those affected by a certain date or within a certain time limit. This date could be one day to one month after determining whether it was a breach or not.

## RS.CO-02

*Internal and external stakeholders are notified of incidents.*

Your policies and procedures should state if and when you notify others of a data breach or other adverse event that has affected your company. There should be thresholds that dictate whom to speak with and when after an event has occurred. There also needs to be a determination of whether the event affected others and is considered a breach of confidential information.

For example, PCI 12.10 requires that you have a thorough IR plan in place and that it has been tested. The requirements also state that you must have a data backup process and a business continuity plan in place. Depending on your merchant, they also may have additional steps or requirements such as how much time you have to notify them of a breach.

Another example is that the **Securities and Exchange Commission** (**SEC**) introduced new rules that require publicly traded companies to send out notifications within four days of a material breach. This means that if your company has determined that a breach of sensitive information is considered material, you have four days from the time you deemed it material to send out a notification.

You may need to work with the executive leadership team, legal department, and outside counsel to assist in the determination of a data breach. Also, utilize law enforcement and the **Internet Crime Complaint Center** (**IC3.gov**) to notify the FBI when a breach or other adverse event has struck your organization.

## RS.CO-03

*Information is shared with designated internal and external stakeholders.*

For this control, you will need to rely on your policies and procedures along with any contracts or legal documents you may have with third parties. Each of these documents should have SLAs for how and when to communicate this information to all your stakeholders. You will also need to develop a crisis communication plan for how to speak with these various groups.

Review any contracts that you have with third-party vendors. The contracts will more than likely have verbiage around how to communicate with the vendor and when. As mentioned previously, several regulatory requirements state that you must notify them within a certain timeframe to be considered *in compliance*. You may face hefty fines if notification is beyond that timeframe. Vendors and customers may have similar language in their contracts. Be sure to have your legal department review them to ensure that you and your organization are maintaining their agreed-upon SLAs.

Your communications (or similar) department should also create a **Crisis Communication Plan** (**CCP**). This communication plan is to be used if and when a serious incident has occurred. Thresholds should be established in the policies and procedures documents for when to activate or use the plan. You may have several CCPs depending on the audience you are communicating with. Work with your communications department to ensure that these documents are created.

# Incident mitigation (RS.MI)

In this control family, we discuss how to mitigate or contain and eradicate the adverse event from the environment. We will need to work with third parties or trusted vendors in our environment possibly to perform techniques to understand our attack surface and how the event happened to begin with.

## RS.MI-01

*Incidents are contained.*
