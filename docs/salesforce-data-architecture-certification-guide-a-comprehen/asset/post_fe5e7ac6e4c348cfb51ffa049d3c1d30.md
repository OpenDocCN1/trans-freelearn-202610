
## Practice questions
- 1. Which Experience Cloud license type is required in B2C scenarios where access to reports is required?
- 2. Which Experience Cloud license type exposes the **Quote** object?
- 3. Which Experience Cloud license type is required for access to Knowledge?
- 4. Which Experience Cloud license type only facilitates record sharing through sharing sets?
- 5. Which Experience Cloud license type allows for the creation of dashboards?
- 6. Which Experience Cloud license types allow for sharing rules?
- 7. Which Experience Cloud license type allows for the creation of reports?
- 8. Which Experience Cloud license type exposes the **Lead** and **Opportunity** objects?
- 9. Which technology is used to surface external data in **Open Data Protocol (OData)** 2.0 or OData 4.0 format?
- 10. Which technology is used to process and execute long-running Apex operations across entire datasets on the Salesforce Platform?
- 11. Which technology is used to read, upload, or delete large datasets on the Salesforce Platform?
- 12. Which technology is used to surface data in one Salesforce instance to another Salesforce instance?
- 13. Customer 360 is a Salesforce solution designed to provide what?
- 14. Marketing Cloud and Salesforce are connected using what?
- 15. What is the recommended technology to bring together data from external data sources on the Salesforce Platform as part of Customer 360?

## Answers
- 1. Customer Community Plus
- 2. Partner Community
- 3. Customer Community
- 4. Customer Community
- 5. Partner Community
- 6. Partner Community and Customer Community Plus
- 7. Customer Community Plus
- 8. Partner Community
- 9. Salesforce Connect
- 10. Batch Apex
- 11. Bulk API
- 12. Salesforce to Salesforce
- 13. A 360° view of the customer
- 14. The Marketing Cloud connector for the Salesforce Platform
- 15. MuleSoft

## Further reading

Salesforce Experience Cloud licensing:

```
https://help.salesforce.com/s/articleView?id=users_license_types_communities.htm&type=5&language=en_US
```

Setting up sharing sets:

```
https://help.salesforce.com/s/articleView?id=sf.networks_setting_light_users.htm&type=5
```

Salesforce data storage allocations:

```
https://help.salesforce.com/s/articleView?id=sf.overview_storage.htm&type=5
```

CDP:

```
https://www.salesforce.com/ap/solutions/customer-360/
```

Salesforce to Salesforce:

```
https://help.salesforce.com/s/articleView?id=sf.business_network_intro.htm&type=5
```

Introduction to Bulk API 2.0:

```
https://developer.salesforce.com/docs/atlas.en-us.api_asynch.meta/api_asynch/asynch_api_intro.htm
```

## Data Governance

Being able to safeguard data on the **Salesforce** platform is a key part of the Salesforce Data Architect’s role. This chapter covers the concepts *behind building compliant solutions* and how the Salesforce platform offers features that can help in the technological underpinning of an enterprise **data governance** strategy.

In this chapter, we’ll cover the following topics:

- Considerations for implementing a compliant data model
- Enterprise data governance

While data governance may be perceived by some as a somewhat *dry* or boring topic, read on and you’ll find out why it is important when designing *scalable, secure* solutions on the Salesforce platform. Data governance accounts for 10% of the certification syllabus, so that’s some valuable points that can be attained when taking the exam.

This chapter is the culmination of several topics that are interwoven when it comes to data management, security, and governance. By the end of this chapter, you’ll understand the concepts and regulations behind a compliant data model, such as the **General Data Protection Regulation (GDPR)**, **Personally Identifiable Information (PII)**, and more, and be able to articulate both what enterprise data governance is and how it relates to the Salesforce platform.

Let’s begin by understanding the considerations for implementing a compliant data model.

## Considerations for implementing a compliant data model

The Salesforce Certified Data Architect credential syllabus talks about implementing a **compliant** data model in the context of data governance. As Salesforce Data Architects, it is on us to ensure that we can recommend an approach for designing a data model that conforms to the **compliance standards** that are set out in regulations such as the following:

- **General Data Protection Regulation (GDPR)**, European Union
- **California Consumer Privacy Act (CCPA)**, United States
- **Personal Information Protection Act (PIPA)**, Japan
- **Lei Geral de Proteção de Dados (LGPD)**, Brazil
- **Personal Information Protection and Electronic Documents Act (PIPEDA)**, Canada

This includes going over the various options available for identifying, classifying, and protecting sensitive data.

We'll look at one such regulation now, namely the GDPR.

### What is the GDPR?

The GDPR is the European data protection law that defines how personal information regarding EU individuals needs to be handled.

The GDPR is there to control how information is used by organizations or governments. Individuals have a legal right to know how their information is collected, processed, and stored. They have a right to request the information a company, government, or entity holds about them, and a right to be forgotten. If such a request is made, the organization holding the data is required to delete it, subject to the fair processing of data (which the person must have been made aware of when they freely opted into the company or entity collecting this information). Companies are legally required to collect information fairly, only use it for the specific purposes they need it for, store it securely, and erase it when it's no longer needed. If a company falls foul of the GDPR, the greater figure out of a maximum of €20 million or 4% of their annual global turnover is what the fine could be (this is according to the GDPR itself).

The impacts of complying with the GDPR from a software application perspective include ensuring that the application data model can record consent and privacy preferences. As we already know, Salesforce includes several standard objects to assist with recording preferences and consent (such as the Individual object). Additional custom objects can be added for specific requirements.

### The Individual object

According to the Salesforce documentation available at https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_individual.

htm, the `Individual` object represents a customer's data privacy and protection preferences. Data privacy records based on the Individual object store your customers preferences. Data privacy records are associated with related leads, contacts, person accounts, and users.

When the `Individual` object is enabled, it can be exposed to users by adding the `Individual` field to the **Lead**, **Contact**, and **Person Account** page layouts.

> **Important note**
> Enabling the `Individual` object: The `Individual` object is available if **Data Protection and Privacy** is enabled in Salesforce. This can be done by selecting **Make data protection details available in records** under **Data Protection and Privacy** in **Salesforce Setup**.

### *Dealing with GDPR data*

Personal data and sensitive personal data are covered by the GDPR and need to be **identified**, **classified**, and **protected**. How does Salesforce help address data protection? Well, Salesforce includes several features to assist with protecting data from unauthorized access, including the following:

- Classic and platform Shield Encryption
- Data masking
- Field-level security
- Sharing settings
- Event Monitoring
- Session security
- Field history tracking

Let's look at these topics in detail.

### *Classic encryption*

Included with a base license, **classic encryption** provides encryption for data at rest for custom fields (a distinct custom field type) using the 128-bit **Advanced Encryption Standard (AES)**. Users will need the **View Encrypted Data** profile permission enabled within their profile or via an assigned permission set to read encrypted field values. Classic encryption facilitates masking encrypted fields from users who don't have permission to read the values. They will be presented with a series of asterisks instead. Encrypted fields that use classic encryption cannot be searched or used in automation such as Flow, approval processes, workflow updates, and so on. Therefore, consider classic encryption when encrypting certain data fields that simply store data in a manner that is masked to those who shouldn't see it when viewing the record. If the field requires data to be encrypted at rest, needs to be searchable by users, or readable in automation scenarios such as Flow, consider Shield Encryption instead.
