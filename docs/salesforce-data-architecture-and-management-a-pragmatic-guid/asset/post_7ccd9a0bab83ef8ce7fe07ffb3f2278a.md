
Data security is concerned with protecting your data from unauthorized access and malicious use. The way this is achieved varies from organization to organization and depends on the volume, type of data, and other factors that will drive the need for appropriate tooling and procedures. Data privacy, on the other hand, deals with policies and processes that define how you can collect, use, share, and discard data that is in your purview. Compared to data security, it is more universal since the laws apply to a wider base. However, there may be laws or rules that are imposed on certain industries or associations that the business is part of. For example, the PCI Security Standards Council is a private entity, but it imposes rules regarding data that member organizations must follow to gain or maintain their compliance with the standard.

## Understanding the business risks

As a data architect, you must be cognizant that when organizations start collecting and processing personal data, there are inherent risks that the organization is undertaking that must be properly understood to put risk mitigation strategies in place. It is your responsibility as a data architect to point these out so appropriate team members (for example, the privacy law officer) can be pulled into these conversations so the risks are discussed and appropriate action is taken to address them.

Keep in mind that the privacy laws are not there to stifle businesses but rather to attempt to balance the need for data by businesses versus respecting and protecting individuals' data privacy. Here are a few common risks that businesses are generally exposed to when dealing with personal data:

- **Lack of control around data access**: Some organizations lack control around who can access personal data and whether it's required for them to access it based on their job function; that is, access to personal data must be based on a *need-to-know* basis. This also ties into ensuring processes are in place that when the authorized individual leaves the company, their access to personal information is discontinued immediately to prevent any untoward data incidents.

- **Lack of processes to deal with requests**: Privacy laws such as GDPR require organizations to remove personal data as part of the *right to be forgotten* clause of the law. Not having processes in place to respond to or completely remove the information as required by the law can result in the organization being exposed to heavy penalties.

- **Transparency around sharing data**: Some laws require that the organization must spell out how the personal data collected is used, including sharing with third parties. Failure to do so can result in the company being found to be in violation of the law.

- **Responding to data loss**: Companies usually will tend to disclose whether there has been a theft or unauthorized access of personal data, but some laws mandate that victims are informed in a timely manner. This requires the company to have the appropriate processes and procedures in place to react to such situations.

- **Application and system vulnerability**: Insufficient security around systems and web applications, whether on-premises or in the cloud, can result in data breaches. There has been a number of breaches in the last few years, some involving very large, well-known businesses. For example, about 148 million users were impacted when a breach happened at Equifax in 2017. In a breach at Adobe in 2013, about 152 million users were impacted because encryption was weak and although passwords were stored with encryption, password hints had been stored in plain text.

Another aspect of this is to protect data not only at rest but also in transit between systems and applications. There are technical standards that when fully complied with can ensure the secure transmission of data.

- **Collect more data than needed**: Organizations should focus on collecting only the data that is required. The more data you collect, the more responsibility you take to ensure that the data is properly stored and processed. If a business offers chiropractor services, it can collect data for the patient that is used for diagnostic purposes, but if it decides to collect data about the family of the patient, it then has more data than it needs to ensure is properly protected. The rule of thumb should be to only acquire data that you truly need for your business process. If you anticipate needing the data in the future, your business processes and technical architecture should be designed in a way to acquire and protect that additional data properly.

- **Using data for things other than its purpose**: When collecting data, ensure that your organization is fully transparent and clear with the individual on how the data will be used. Using data for one purpose but then repurposing it for something else can represent a compliance violation. Privacy laws usually require the extent of personal data collection and how it's used. Laws such as GDPR require that when requested, the organization must inform the requestor of all the information it has on the individual. This obviously requires policies and procedures in place to ensure responses are given in a timely manner because failure to do so can result in non-compliance with the law.

Now that we have discussed the importance of data privacy and why it's important for businesses, let's do a deep dive into GDPR. Privacy laws such as GDPR and CCPA are complicated and require specialized expertise and tools to properly implement in organizations. The following discussions are not meant to be exhaustive but should be viewed as a means to introduce you to some of the key components of the laws and set a foundation for deeper discovery at a later time.

## Global Data Protection Regulation (GDPR)

For a number of years now, Europe has led the way with privacy laws. It introduced the initial EU Data Protection Directive that came into law in 1995 that required companies and organizations to properly protect personal data and have a clear, legitimate use for that data. Recently, though, in 2018, a more comprehensive privacy law called **GDPR** came into effect. GDPR is more comprehensive in nature than its predecessor and significantly expands the rights of individuals to privacy and obligates organizations that meet the defined criteria to comply with this law.

Its primary purpose is to give individuals control of how their personal data can be used and to consolidate and provide a single privacy act to make it easy for organizations to implement it. It is worth noting that GDPR applies to organizations beyond the borders of the EU as long as they are dealing with data belonging to EU citizens, which means that whether the organization has a physical presence in the EU is irrelevant. Violating GDPR laws can result in hefty fines of 20 million euros or 4% of annual global revenue, whichever is greater depending on the extent of the breach and the damage it caused.

In the next few sub-sections, we will try to understand what GDPR requires, and then in another section, we will discuss the CCPA.

### Understanding GDPR terms

Before we can do a deep dive into understanding what GDPR requires and how we can use Salesforce for compliance, let's first understand some of the terms used in GDPR:

- **Data subject**: A person who can be identified from some attribute such as name, location data, or social security number.

- **Personal data**: Any and all information related to a data subject that is associated with that data subject, for example, age, gender, or physical address.

- **Sensitive personal data**: Personal data associated with a data subject that identifies their political views, religious affiliation, health information, or ethnicity, for example, a member of such and such party, or a fingerprint scan.

- **Controller**: This is an entity that determines the purpose of processing personal data. For instance, an employer collecting data for the purpose of providing employment to an individual becomes the controller of that data.

- **Processor**: Any entity that processes personal data based on the direction of the controller. For example, Salesforce becomes the processor of data when the employer enters the demographic details of the candidate into Salesforce.

- **Pseudonymous data**: This is considered data that cannot be tied to a data subject without tying it to additional information that is stored elsewhere. For example, when the candidate visits the company pages, the website tracks the candidate and stores their IP address, which, when linked with other information available about the candidate, can result in identifying the data subject.

Let's discuss the key principles that form the underlying requirements of GDPR.

### The underlying foundation of GDPR requirements

There are many requirements in the GDPR privacy law. Understanding the underlying principles will help you understand the essence of these requirements and, to an extent, the intent of the law. Keep in mind, though, that you should consult with a qualified person who has the necessary knowledge and experience in this area before interpreting the law yourself. Fines for violations under GDPR are hefty and negatively impact the reputation of a business:

- **Fairness and transparency**: Organizations must process personal data fairly and transparently. In our earlier example of the candidate applying for a job, the employer must be transparent on what information is being collected about the candidate and also how that information will be used by the employer.

- **Purpose of data collection**: This relates to collecting personal data only for the specified purpose and it cannot be used for any other purpose other than the expressed explicit purpose stated at the time of data collection. For example, a data gathering company that sells data for individuals gathers candidate data for a job posting. The personal data collected is for hiring purposes and therefore cannot be sold for commercial gain.

- **Data minimization**: This principle states to collect personal data only to the extent that it is needed and relevant to the stated purpose. For example, the employer cannot ask whether the candidate owns their home, what vehicles they drive, and their plate numbers as those details are not relevant to the decision-making process in most cases. A common example of this is phone apps where some of those apps require more access than is needed for the purpose of the application.
