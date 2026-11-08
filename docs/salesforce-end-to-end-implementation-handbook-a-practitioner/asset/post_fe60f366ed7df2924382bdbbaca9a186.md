
created, and lots of it. You will need and want to have the ability to archive data for several reasons:

- **Regulatory requirements**: Many regions, countries, and states only permit **personally identifiable information (PII)** data retention for a certain period, and only if there is a valid reason for the retention.
- **Usability**: Without regular archival of old or irrelevant data, users will unnecessarily have to sift through all the data clutter, decreasing the user experience and productivity, and increasing the risk of errors and poor customer experiences.
- **Performance**: If you don’t archive old or irrelevant data, when your users search for information, load record pages, or run reports, the load time will increase over time, diminishing the user experience and productivity.
- **Cost**: Storage is a big cost driver for cloud solutions. While you can purchase additional storage, having an *efficient and automatic* archival solution in place will quickly outweigh the saved storage space cost.
- **Backup and restore**: Whereas data and file archival is mostly relevant for old or irrelevant data, backup and restore is concerned with ensuring business continuity by making sure your critical operational Salesforce data is backed up regularly and easily restorable should data be unintentionally deleted, modified, or corrupted for any reason. There are robust Salesforce-specific **commercial off-the-shelf (COTS)** solutions available that provide these services – **OwnBackup** and **Odaseva** are the market-leading providers in the Salesforce ecosystem.
- **Accessibility management**: Offering a Salesforce solution that is accessible for people living with some form of disability is not only ethical but also smart for business. In many countries, it is a legal requirement that goods and services offered to customers must be accessible also to people with disabilities. Furthermore, being able to offer employment to people with disabilities will increase your talent pool – to the benefit of the people with disabilities as well as your company.

- **Speed/performance:** Everyone wants solutions that are quick and easy to use with no wait time. In some use cases – for example, a customer service call – having a customer contact page load time exceed a certain number of seconds may not be acceptable to the business, as it would result in a poor customer experience. Other key performance aspects are running reports and dashboards and searching for records. These are examples of non-functional requirements.
- **Reliability:** The percentage uptime of the system where users are able to log in and perform work is a characteristic of reliability. Although your company will be largely depending on Salesforce to ensure uptime, there are things you can do to minimize downtime. These include minimizing interruption of operations when deploying new features and building in exception handling to gracefully handle connection issues for your integrations.

Many non-functional requirements are effectively provided and, to some extent, controlled by Salesforce since – in addition to being a SaaS solution – it provides infrastructure and a platform for its customers. However, how you design, configure, and develop your Salesforce org has a major impact on its level of security and performance.

Tip!

Engage a technical architect to support you in defining the technical enablers for your Salesforce project.

Having covered some of the common technical enablers and non-functional requirements for Salesforce solutions, let’s look at data sources that are often integrated with Salesforce.

## Data sources

While your company’s ambition and intention may be for Salesforce to be the main system for your marketing, sales, and customer service teams, there will inevitably be other systems and applications in your company’s **system landscape**. These other systems and applications serve their own unique purpose and hold data that is sometimes relevant for users in Salesforce, while other times it may be Salesforce that needs to send information to an application to complete a process.

Let’s look at some systems that are often integrated with Salesforce:

- **Enterprise resource planning (ERP) system**: These systems are used for operational purposes, covering production planning, stock management, logistics planning, order processing, and invoicing.
- **Data warehouse (DWH)**: This is a database containing data from across the enterprise. It may have various levels of distribution and structure. For example, data lakes contain unstructured data for data scientists to model and leverage, whereas a highly structured and managed database can provide the basis for BI systems to provide a reporting layer to users.
- **Active Directory (AD)**: An AD often serves as an **identity provider** or **identity store** used in relation to *IAM* to centrally manage provisioning and **deprovisioning** and access level changes.
- **Website(s)**: Your company likely has at least one website describing your company and its products and services. The website may have a **contact form** for people to fill out with questions or to request information, which is then sent to your sales or customer service teams.
- **Content management system (CMS) and digital asset management (DAM) system**: These systems store assets used across a number of applications, including your website and email marketing solutions.
- **Product information management (PIM) system**: Stores information about your products (and services) used across a number of applications, including ERP, e-commerce stores, CRM, and product **configuration, pricing, and quoting (CPQ)** solutions.

Tip!

Review your workshop products of *systems* and *data* as a baseline for integrations with your Salesforce solution. Engage an architect to support you in determining which systems and data (external to Salesforce) need to be integrated with your Salesforce solution.

Next, we’ll look at how to summarize all the capabilities of your Salesforce solution.

## Summarizing the capabilities to be supported by your Salesforce solution

Having added the technical enablers, you now have a complete set of capabilities in scope for your Salesforce project. Let’s see how the business capability map looks for PME in the following figure:

![](img/307881206539c45e128c538a4b6912f8_65_0.png)

Figure 2.5 – Business capability map for PME’s Salesforce solution

*In addition to the imagined new business capabilities, PME – being a global company – identified the need to have a localized solution offered in multiple languages and supporting multiple currencies. PME is also keen on ensuring solid security, both in terms of user access to and in Salesforce, as well as what data they can access. PME also sees the value in regularly archiving and backing up data and files to comply with regulations and making sure the solution is scalable and performant, and ensuring business continuity. Finally, PME wants to ensure their solution can be used by their employees with disabilities – as well as by people with disabilities working for their customers.*

*PME identified – through workshops with end users as well as engaging with a technical architect – what key systems and data sources are required to work in conjunction with the future Salesforce solution.*

Having summarized the capabilities to be supported by your Salesforce solution, you are ready to move on to determining the high-level technical scope required for your Salesforce solution.

## Determining the high-level technical scope of your Salesforce solution

Until now, we have not spent a lot of energy on Salesforce features. That will change now as we progress to define the high-level technical scope of your Salesforce solution.

You have defined the business capabilities to be supported by your Salesforce solution. Now it’s time to understand what **hypothetical technical scope** is needed to support the business capabilities. It is *hypothetical* at this stage because you will undoubtedly have many assumptions and uncertainties. As you progress through the chapters in the
