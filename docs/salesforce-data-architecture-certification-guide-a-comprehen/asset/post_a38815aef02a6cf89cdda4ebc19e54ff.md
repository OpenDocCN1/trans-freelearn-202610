
Next, there are full-blown external software packages that support data and metadata backup and restoration, often integrating with SFDX for metadata operations. An intricate understanding of these software packages is beyond the scope of the exam (and therefore this book), but they all automate and facilitate automated sandbox provisioning and backup and restoration of data and metadata, and often integrate with automated workflows as part of a **Continuous Integration/Continuous Deployment (CI/CD)** pipeline.

Let’s now summarize what we’ve covered in this chapter.

## Summary

In this chapter, we looked at the ways in which you can back up and restore both data and metadata when working with the Salesforce platform. We started by breaking down what the difference between data and metadata is before moving on to the tools available from a native or platform standpoint when it comes to backing up and restoring metadata and data.

Next, we moved on to looking at AppExchange and external software that facilitates backup and restoration, covering what they do to facilitate and even speed up this task. Lastly, we looked at some best practices when exporting and importing data.

In the next chapter, we’ll explore Territory Management to conclude our learning material.

## Practice questions

Test your understanding of the concepts covered in this chapter by attempting the following practice questions:

- 1. Which Salesforce feature allows for selected metadata changes to be migrated from one environment to another?
- 2. What is the name of the standalone application that facilitates data imports and exports to and from the Salesforce platform?
- 3. Which report export format supports CSV, XLS, and XLSX?
- 4. Which facility allows for monthly backups?
- 5. What format is the file from a monthly backup?
- 6. What is metadata?
- 7. What metadata backup facility is used to create a production-like instance of Salesforce?
- 8. Which two metadata backup and restore technologies work with local or source-controlled copies of metadata?
- 9. What does CSV stand for?
- 10. In a CSV file, the first row of data is the what?

## Answers

How did you get on answering the practice questions? Check your answers here:

- 1. Change sets
- 2. Data Loader
- 3. Details Only
- 4. The Data Export service
- 5. A zip file containing several CSVs
- 6. Data that describes data
- 7. Sandbox refreshes
- 8. SFDX and the Ant Migration Tool
- 9. Comma-Separated Value
- 10. The field names to which the values in the following rows pertain

## Further reading

For further information about the concepts and topics covered in this chapter, please refer to the following resources:

- *Best practices to backup Salesforce data:* https://help.salesforce.com/s/articleView?id=000334121&type=1
- *Export data using Data Loader:* https://help.salesforce.com/s/articleView?id=sf.exporting_data.htm&type=5
- *Export data using report exports:* https://help.salesforce.com/s/articleView?id=sf.reports_export.htm&type=5
- *Export data using weekly backups:* https://help.salesforce.com/s/articleView?id=sf.admin_exportdata.htm&type=5
- *Create a Sandbox:* https://help.salesforce.com/s/articleView?id=sf.data_sandbox_create.htm&type=5
- *Ant Migration Tool Guide:* https://developer.salesforce.com/docs/atlas.en-us.daas.meta/daas/meta_development.htm
- *SFDX:* https://developer.salesforce.com/tools/sfdxcli

Under the hood, Enterprise Territory Management is a way of sharing. By that nature, Salesforce utilizes a rules engine, as determined by how you set up Enterprise Territory Management to perform sharing and associated recalculations on the fly.

Before we get into the specifics of how Enterprise Territory Management is set up, let’s cover some terminology that is specific to Enterprise Territory Management, as follows:

- **Territory**
- **Territory Type**
- **Territory Type Priority**
- **Territory Model**
- **Territory Model State**
- **Territory Hierarchy**

We’ll cover each of these terms, beginning with the definition of a territory.

## Territory

A **territory** can be thought of as a grouping of account records and the users in your Salesforce instance that work with those account records. Territories are created from territory types. Territories can be hierarchical. For example, it’s entirely possible to have a territory called the United States, with Northwest, Northeast, Southwest, Southeast, and Central Territories as children. Territory records contain several key pieces of information, such as the following:

- Assigned users to the territory
- Any assigned accounts
- Territory/Account Assignment Rules (the characteristics of an Account that mean that it will be assigned to this territory)

Next, let’s look at the definition of a territory type.

## Territory Type

**Territory Types** are used to organize and create Territories based on criteria such as account size, account billing state, or named accounts. When Territories are created, they are created from a territory type. Territory Types do not appear on the territory model Hierarchies that are created as part of the Territory Management setup.

For example, a territory called *UK County-Based Accounts* could be created whereby territories can be created based on the **UK County** value in the **state** picklist of the Account records.

With territory type now understood, let’s turn to territory type Priority.

## Territory Type Priority

**Territory Type Priority** is a custom scheme you define, and it is used to assist when creating territory records from the territory types you’ve defined. For example, you may have territory type Priority values of 1 through 5 to denote the priority of the territory record when it is created to align with the sales strategy of your business. This could mean that the Southwest Territory is given a priority of 1 as it is where your business expects to see the most growth this coming financial year. Essentially, Territory Type Priority is used to ensure that the right territory types are prioritized when creating new territory records.

Now that we have covered the definition of Territory Type Priority, let’s explore the territory model.

## Territory Model

The territory model is the representation of a complete territory management system, as defined by you for your Salesforce instance. You can have multiple territory models, but only one can be active at any given time. Salesforce provides a nice graphical hierarchy for territory models, which assists us in visualizing how Territories are organized, including the hierarchies. territory models allow you to test how Account and User record assignments will work, and as such allow you to try seeing how implementing a certain territory model will affect things before activating it.

> **Important note**
> 
> Salesforce Enterprise Edition only allows two territory models to exist at any one time. Performance and Unlimited editions can have four territory models. The same limits apply to Salesforce Sandbox environments that are created from a production instance utilizing one of these Salesforce editions. Remember only one territory model can be active at a time.

With territory model covered, let’s now look at the definition of a territory model state.

## Territory Model State

Territory Models are assigned a **state** value to quickly allow the status of a given territory model. There are three values available, as follows:

- **Planning:** All territory models default to this value. It can be thought of as signifying the territory model is being developed/is in draft status.
- **Active:** The territory model is active. Remember, only one territory model can be active at any given time.
- **Archived:** A previously active territory model is archived as another territory model has been activated. Once a territory model has been archived, it cannot be reactivated.

> **Important note**
> 
> When a territory model is archived (or you decide to delete a territory on an active territory model), the **Territory** field on Opportunity records is set to a blank value on Opportunity records that had a previously active territory assigned.

When activating or archiving a territory model, there may be errors that occur. If so, the status of the territory model will be set to either **Activation Failed** or **Archiving Failed**. Salesforce will send you an email containing details as to why the failure occurred.

Next, let’s look at the Territory Hierarchy.

## Territory Hierarchy

A **Territory Hierarchy** shows a structured view of the territory model. The Territory Hierarchy is the main interaction point with the territory model and is the place where Territories can be created, edited, and deleted. It’s also where assignment rules can be executed, and Opportunities can be assigned to Territories.

With the Territory Hierarchy definition covered, let’s delve deeper into how Enterprise Territory Management is set up.

## Implications of territory management on data model design and sharing records

**Territory management** essentially facilitates the automatic sharing of records based on a rule set, as defined by the territory model that has been activated in your Salesforce instance. How **Organization-Wide Defaults (OWDs)** are set up will have an impact on the way the territory management implementation for your organization works. This is because the sharing afforded by territory management and the assignment of Accounts and Opportunities may or may not offer any further record access in addition to the sharing already in place. In such cases where there is no difference in the sharing of records, territory management essentially becomes a tool for organizing records. Territory management report types can be utilized to provide reporting based on Accounts and Opportunities being assigned to users, as per the active territory model.

In instances where the default Account and Opportunity model is set to **Private**, then territory management can be used to set the access level for Account and Opportunity records that are shared with users. For example, Accounts and Opportunities may be **Private** as per the configured OWDs for your Salesforce instance. Territory Management can be used to configure record access to be Read or Read and Edit on Opportunity records that are shared with users.
