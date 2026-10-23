
> **Important note**
> Classic encryption is effectively *free*: it is available with any base license, so it doesn’t incur an extra fee.

## Shield Encryption

Available for an additional fee, **Shield Encryption** provides encryption at rest using 256-bit **AES**. While Shield Encryption doesn’t provide data masking of values, it does allow for the encryption of both standard and custom fields, not requiring a special field type. When configuring the fields, encryption parameters can be set. Fields encrypted using Shield Encryption can be searched and used in automation facilities such as **Flow**. Additionally, **Salesforce Shield** allows you to encrypt **Files and Attachments**. You can read more about the differences between classic encryption and Shield Encryption at https://developer.salesforce.com/docs/atlas.en-us.securityImplGuide.meta/securityImplGuide/security_pe_vs_classic_encryption.htm.

> **Important note**
> Shield Encryption works on standard objects: classic encryption doesn’t extend to standard objects. Therefore, if the need to encrypt data on standard objects arises, you’ll need to invest in Shield Encryption.

## Field-level security

As covered in *Chapter 2, Data Modeling and Database Design*, **field-level security** is set at the profile and permission set levels, and simply determines if users should be able to see a field, read the values, or edit the values in that field. Field-level security is a de facto way to determine if users should have access to a field, and if they should, whether that access is read-only or read/write.

## Sharing settings

As also covered in *Chapter 2, Data Modeling and Database Design*, **sharing settings** such as **Org-Wide Defaults (OWDs)** can be used to state the default level of access users should have to a Salesforce instance’s data.

## Event Monitoring

As part of the Salesforce Shield suite, **Event Monitoring** enables access to an API-only object called **EventLogFile**, the **LoginEvent** object, **Transaction Security**, and an **Event Monitoring Analytics** app.

With Event Monitoring enabled, it is possible to understand events and act on them, either through the API or by visually looking at event types using a Tableau CRM app. When acting on the events using the API, it is possible to determine usage such as impossible journeys. Impossible journeys are logins or page requests close together in time yet from locations far apart, making access from those locations within a specific time impossible and potentially malicious.

You can read about how to enable Event Monitoring at `https://help.salesforce.com/s/articleView?id=000339868&type=1.`

## Session security settings

It’s possible to configure settings related to the security of a users session for when they are accessing Salesforce to safeguard access to your org data held on the platform.

Session settings cover a wide range of topics, including **timeout settings, caching, multi-factor authentication,** and **high-assurance sessions** for accessing sensitive data.

You can read more about session security at `https://help.salesforce.com/s/articleView?id=sf.admin_sessions.htm&type=5.`

## Field history tracking

Lastly (but by no means least), **field history tracking** is a powerful tool for understanding who made what change to a custom subset of object fields. Certain field changes may be required for tracking by your company – for example, who changes the **Stage** picklist on **Opportunities**, recording the old and new value, who made the change, and when. Field-level security allows for 20 fields per object to be tracked *out-of-the-box* with Salesforce, although Shield (which incurs a separate subscription/charge) can raise this limit to 60 fields per object. Additionally, with Shield, the field history data is archived after 18 months and stored for 10 years.

You can read more about field history tracking at `https://help.salesforce.com/s/articleView?id=sf.tracking_field_history.htm&type=.`

Now that we’ve looked at how to implement a compliant data model (looking at the various tools and techniques available on the Salesforce platform to do so), let’s turn our attention to enterprise data governance.

## Enterprise data governance

As data architects, we must look at the various approaches and considerations when designing an enterprise data governance strategy for implementation within an organization.

To understand this better, the following diagram shows the components of an enterprise data governance strategy:

![](img/7cca8452e237cf62a2d0e00a080d3fe9_96_0.png)

Figure 5.1 – Components of an enterprise data governance strategy

An enterprise data governance strategy contains several key things, including the following:

- **Policies and standards for data:**
  - What external policies, rulings, or standards must data adhere to? For example, is the business a financial services organization, and therefore are there regulatory body requirements that need to be considered for the governance of data?
  - What internal policies must data adhere to? For example, are there internal policies relating to PII?

- **Ownership and accountability of data:**
  - Who owns the data?
  - Who is accountable for the data?
- **Rules for data:**
  - What rules are in place surrounding the data?
  - What rules need to be put in place?
  - Who oversees those?
- **Definitions of data:**
  - Where is the data dictionary defined, stored, and managed?
- **Data monitoring:**
  - What monitoring is in place for the data?
  - What systems need to be monitored?
  - What is the monitoring for?
- **Measurement of data usage:**
  - What measures or **Key Performance Indicators (KPIs)** is data usage measured against?

Having introduced these enterprise data strategy components, it should be clear that the questions posed contribute to the importance of having a sound data governance strategy in the first place. Given how valuable data is to an organization (and the laws and regulations that ensure it is treated as such), governing that data from collection to definition, usage, monitoring, and measurement is paramount to safeguarding what some may argue is its most valuable asset.

Now, let’s take a look at some roles and responsibilities involved in data governance.

## Roles and responsibilities

An enterprise data governance framework may contain different roles and responsibilities, which is why each organization will have differing requirements. Each company needs to implement a suitable data governance model based on its specific requirements.

Let’s visualize the roles as they would sit in a data governance framework:

![](img/7cca8452e237cf62a2d0e00a080d3fe9_98_0.png)

Figure 5.2 – An enterprise data governance role framework

There is typically commonality in the following role across data governance frameworks:

- The steering committee
- Data owner
- Data steward

Let’s look at each role in more detail.

## Steering committee

A **steering committee** is a forum of senior management, those accountable for certain business functions (such as Sales or Service), and C-level individuals (almost certainly the **Chief Information Officer (CIO)** and sometimes the **Chief Information Security Officer (CISO)**).

This committee has several responsibilities, which include setting the data governance strategy, working with the data stewards, and ensuring the data governance function is held to account (which may include various programs or projects of work that have specific deliverables, outcomes, and deadlines).

## Data owner

A **data owner** is a person or team responsible for ensuring information within a data domain is governed across the systems in the organization’s IT enterprise and business functions, such as sales. Data owners may be part of the steering committee (if the data owner is also a C-level member or in charge of a business function, such as Service). Data owners are typically responsible for the following:

- Approving the data definitions (the descriptions or metadata about the data).
- Ensuring the accuracy of information across the entire enterprise.
- The quality of the data.
- Contributing to, reviewing, refining, and approving the business approach to master data management. This can include reviewing or defining plans and other additional work items or artifacts that affect the master data management approach and its implementation.
- Working with the other data owners to collaborate on data issues across the enterprise and resolve them.
- Working with the steering committee, particularly to input on solutions (where the data owners have SME knowledge to offer). Additionally, they must give input on the selection and use of software and define and review policies or other regulatory requirements of their data domain (an example of a financial services organization in the United Kingdom is the **Financial Conduct Authority (FCA)**, which has strict requirements around handling data).

## Data steward

**Data stewards** are accountable for the day-to-day management of the data in the enterprise. They are the **Subject Matter Experts (SMEs)** who can both understand and communicate the meaning, definitions, and uses of the data in their business domain (and additionally, other related information across their business line). Data stewards work with other data stewards across the business to make decisions around the governance of the business data.

In a nutshell, data stewards are responsible for the following:

- Being the SMEs for their particular data domain
- Identifying, working through, and resolving issues with their and other data stewards data
- Reporting to the data owner and other applicable stakeholders within a particular data domain
- Working across the entire business to ensure that their data is managed and understood by other data stewards and data owners
