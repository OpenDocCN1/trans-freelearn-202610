
Customer 360 consists of four elements:

- **Customer 360 Data Manager**: This is the key component that allows companies to connect and provide a single source of truth of customer records across Salesforce and other enterprise systems. It does this by creating a single universal Salesforce ID that is used to represent a customer. This allows companies to provide a personalized experience to the customer and, internally, allows companies to get that unique insight into customer behavior through Salesforce Einstein.

- **Salesforce Identity for Customers**: This component allows companies to provide a uniform, consistent login experience to their customers. Rather than customers requiring multiple passwords to log in and access different company assets, such as mobile apps, websites, and e-commerce sites, customers can use one login with multi-factor authentication enabled and securely access these resources.

- **Customer 360 Audiences**: This creates multiple customer profiles across website visits, first-party IDs, emails, and suchlike, and allows companies to create customer journeys and gain AI-driven insights. **First-party IDs** are IDs associated with first-party cookies. First-party cookies are cookies (small text files) that are placed on a user’s device when they visit the site. First-party cookies are different to third-party cookies in that first-party cookies can only be placed by the website host you are visiting. For example, if you visit `google.com`, it can place cookies on your device but `youtube.com` cannot. First-party cookies are generally considered more privacy-safe.

- **Salesforce Privacy and Data Governance**: This allows companies to collect and enforce their systems’ customer privacy preferences. Part of this is also the data classification label that can now be applied to fields in Salesforce. This helps companies to ensure that they are compliant with privacy laws.

## Common Information Model (CIM)

CIM is an open source data model that standardizes data interoperability between systems. It is a data model that allows data sources with different formats and structures to be connected. It forms the backbone on which Customer 360 is architected. When data sources are connected to Customer 360, data, in essence, is mapped from those sources to a canonical data model.

Canonical Data Model (CDM): This is a data model that can contain all the data from different data models that source systems may have; for example, let's say we have MS Dynamics, SAP, and Salesforce in our systems landscape. All three will have their own way of storing data and in a CDM, we will be able to bring this data, translate it, store it, and easily map it back to the source systems. Therefore, in our preceding example, we will be able to bring in the data from MS Dynamics, SAP, and Salesforce, making it available for other systems to consume that data easily without having to do their own transformation or translation.

### Data lakes

Typically, when you are storing data in a data store, you have to structure it in a certain format. Data lakes are data stores that don't require data to be stored in any strict format and allow users to store structure as well as unstructured data.

In Salesforce, the CIM is exposed to the platform so you can run SOQL queries against it, build triggers, or create Lightning Web Components (LWCs). The strength of Customer 360 lies in the fact that there is no need to move massive amounts of data into a data lake; rather, the use of a unique reference back to the source allows the data to remain in place. This also means that there is no need to make a change to the source systems to accommodate integrations.

## Summary

We started off with the basics and reviewed master data and its importance in any organization, whether small or large. We reviewed the different types of data, especially comparing master data and reference data in the context of Salesforce. When most people talk about master data, they are usually referring to both these categories under the master data umbrella. We discussed at length what data is master data and the business problems that it helps us to solve. This should have provided you with an appreciation of why adequate resources and time should be spent in identifying master data with a solid plan to manage it properly for smooth and ongoing business operations.

We discussed Master Data Management (MDM), starting with its definition to establish a baseline understanding of the term, and then briefly discussed how an MDM program can be implemented. The discussion around different styles of implementing MDM provided a good understanding of which style would be appropriate for your organization, followed by a discussion of the criteria when choosing an MDM solution. We also discussed the Golden Record, its importance, and what the detractors of the Golden Record philosophy argue. This should have provided a balanced approach to the Golden Record when discussing it, keeping in mind its benefits and the perceived disadvantages that you can mitigate when implementing an MDM solution.

Lastly, we discussed the new offering from Salesforce called Customer 360 and its components. Customer 360 is Salesforce's attempt to provide a solution to master data issues that organizations face today, but it doesn't stop there. It takes it a step further and brings the customer into the conversation, too, by attempting to provide a unified login experience to the customer via Salesforce Identity for Customers and ensuring that customer data is properly governed and that requests from customers regarding their data that the organization possesses can be properly addressed via the Salesforce Privacy and Data Governance component of Customer 360.

In the next chapter, we will discuss data governance for enterprises and privacy acts such as the GDPR and the CCPA, along with discussing ways in which to assess our current state from a data governance perspective.

## Questions

- 1. Precision Printers (PP) has multiple CRMs in each region and has decided to implement an MDM system to consolidate records and arrive at the Golden Record. The project team has decided to use survivorship rules for selecting the record that would need to be kept during the merge operation. What do the data architect and the project team need to consider in this scenario?

- a) The team must define multiple survivorship rules, one for each system, to arrive at the Golden Record.

- b) Survivorship rules cannot be used for this scenario because they are not intended for duplicate management.

- c) The project team needs to define the specific criteria that can be used to arrive at the Golden Record.

- d) The project team needs to set up probabilistic rules to determine the Golden Record.

- 2. Alpha Big Box Inc. has multiple systems, each of which has been determined to be the system of record, having a portion of the master data related to accounts and contacts, in other words, data is entered or updated, and for those specific data points, the system where these data points are updated is considered as the system of record. A CRM is being transitioned to Salesforce that is defined as the source of truth for accounts and contacts going forward. An MDM has also been recently implemented that is now pulling data from Salesforce.

Users are finding that some records in the MDM have blank or incorrect values, such as Test Contact. What could be the root cause of the problem? (Choose two answers):

- a) Test data generated during User Acceptance Testing (UAT) in Salesforce still exists and has not been removed.

- b) The project team should turn off the MDM interfaces immediately, remove all the data from the MDM, and restart the integrations all over again.

- c) All the data has not yet been migrated to Salesforce and it has been prematurely defined as the system of record for accounts and contacts, causing the MDM to pull in those specific fields for some records.

- d) Only one system should always be the system of record; this configuration is invalid and the source of this problem.

- 3. Koolio Box Inc. is a consumer goods retailer that operates on a B2C model. It has 20 million person account records in Salesforce and the number is growing by 5% each year. Koolio's management is concerned that these are not unique records and, due to a number of data entry channels, such as web-to-case, web-to-lead, and outbound marketing efforts, the situation is going to get worse if nothing is done on a priority basis. They have hired a data architect to recommend a solution. What should the data architect recommend?

- a) Hire several interns and have them manually sift through the records in Excel and determine duplicate records. Delete those records from the system.

- b) Develop a custom solution that uses APIs and Lightning Web Components (LWCs) to assist in determining potential duplicates based on a limited number of criteria, for example, email and phone number.

- c) Use an AppExchange package to clean up the existing data and leverage it to ensure data quality on an ongoing basis.

- d) Use Dun & Bradstreet (D&B) services to clean up the data.
