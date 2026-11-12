
The goal should be to minimize these transformations as much as possible so as to have a reliable data restore. The overall recovery process should be viewed as a multi-step process and at each step, certain things must get done to ensure an accurate and reliable data restore. Here, we go into each of these in more detail:

- Establish your dataset: This is the first step and an important one. You want to make sure you have a very clear idea of what you want to restore and what the end result should look like. What are the from and to dates for the data restore? From your selected dataset, are there any exceptions to the records that shouldn't be restored? For example, when restoring Opportunity records, you want to restore only those that are in the pricing negotiation stage.

- Pre-processing: This step involves adjusting any settings that could cause data transformation or cause the restore process to fail; for example, turning off triggers or deactivating validation rules. Some of the things that can be done in this step are as follows:

    - Turning off validation rules: Turn off validation rules on the objects that you are trying to restore data for. That will help to speed up the process and eliminate any chances of the restore process failing due to the hard stops when validation rules are triggered.

    - Turning on audit fields: These refer to the CreatedDate, LastModifiedDate, CreatedBy, and LastModifiedBy fields, which are used to reflect when a record was initially created and updated. The last two fields reflect who created the initial record and the user who updated it last. When restoring data, you want to ensure that the Create Audit Fields permission is turned on so that data gets restored with the correct audit field values. This is important if you have, let's say, workflow rules or process builders running off of the CreatedDate field. Loading data with the current CreatedDate field, which is the default behavior, can impact your business processes. Keep in mind that the audit fields cannot be updated once entered and are only available through the APIs.

    - Synchronizing metadata: Often, a restore would fail because the data and metadata do not align between the backup and the organization in which the restore is being done. For example, an opportunity stage value was deactivated in the environment recently, whereas in the restore, the data still exists for that stage. At the time of restore, it will try to restore that stage, but when it cannot locate it in the environment, it will throw an error.

    - Turning off automation tools: Make sure to turn off automation tools such as workflow rules, process builders, flows, and triggers that can cause significant transformation when data is being restored.

    - Changing autonumber fields: This is another area that can cause the restored data to be transformed. Make sure to change your **Autonumber** fields to the **Text** type before starting the restore as that will ensure that the data gets written the same way it is in the restore.

- Data restore: This is the execution step where data will be written back into the organization. Having the right tool to do the job at this stage can be of tremendous help. Imagine if you had to do a simple restore for Accounts, Contacts, and Opportunities and had half a million records in total. This can be done via Data Loader, but it can turn out to be a very cumbersome and painful process.

- Post-processing: This step involves reverting those settings that were made in the pre-processing step, specifically, settings such as activating validation rules, turning on triggers, process builders, and flows. You will also want to flip the **Autonumber** field back to **Auto**, making sure that you give it the correct format and a starting value. You will also want to ensure that the **Create Audit Fields** permission is removed from the profile or the permission set.

- Verification: This is the final step and involves verifying your data restore. Barring the items that we know would have changed, for example, formula fields, you will want to focus on whether the total number of records from the backup and the restored data matches. Then you can filter down on the restored dataset and compare a few other fields, such as **Stage** if it is opportunities, the **Status** field for cases, and so on. Again, having a tool that assists with verification activities can be a huge time saver, otherwise, be prepared to spend hours verifying the restore before it can be declared successful and operations can return to normal.

Let's look at some tools we can use to help us with data backups and restores in the next section.

## Backup and recovery – tools of the trade

In this section, we will discuss some tools that are widely used for backing up and restoring data. As you will see, some of these are native and free from Salesforce, while others are third-party AppExchange applications. These are just a handful of tools and the list is not exhaustive by any means, nor is it an endorsement of any one tool. We have already discussed factors that may influence your decision-making, including costs, and those should be used as a guide to determine the most optimal tool for your organization. Note that I didn't say _best_ or _cheapest_, but rather _optimal_, which means balancing the needs of your organization with practical constraints such as costs, the nature and size of your business, and data volumes.

Let's start with the native tools from Salesforce.

## Data Export Service

This is a UI-based tool within **Setup** that allows you to either schedule or manually run backups of your data. It allows you to select the objects you want to back up and is limited to data only; metadata is not included in this. The on-demand export can be run every 7 days. Limitations include not being able to run an export from a sandbox environment and there is no restore mechanism, so you will need another tool to restore the generated .csv file to the org. Export files are held by Salesforce for 48 hours only (excluding weekends) and then they are deleted.

## Data Loader

This is another free tool from Salesforce that can be used for both backups and restores. You can run this on-demand or schedule it using the command-line interface. Data Loader is a powerful tool that supports the Bulk API and can be used to export up to 5 million records. Beyond that, Salesforce recommends using an AppExchange partner for backups and restores. It is also limited to data only and metadata is not covered. Other limitations include an inability to export attachments, so you will need to use Data Export Service or another tool for that, and you cannot export compound fields with Data Loader. Remember that compound fields are fields that are a combination of other fields, for example, address or location fields. Data Loader can export individual fields that make up the address or location fields.

## Odasava

This is an AppExchange partner tool that was designed by a **Salesforce Certified Technical Architect (CTA)** and is a robust tool that can be scaled and used in small to very large Salesforce organizations. It provides both data and metadata backup and restore capabilities and supports the Bulk API along with other Salesforce APIs.

## OwnBackup

This is another well-known backup tool that supports both data and metadata backups and restores. It also supports the Bulk API and other Salesforce APIs for backup and restores.

I have mentioned just a few here, but there are many other companies, including Skyvia and Spanning, that offer these services. Again, when evaluating these solutions, consider the factors we discussed earlier to ensure that your requirements will be adequately met.

## Summary

In this chapter, we started off by defining data and information, understanding why data is valuable, and then the benefits of data management. We took a deep dive into understanding how increasingly valuable data is becoming for organizations with the advent of machine learning and AI. We looked at some of the challenges associated with data management. This should have given you an appreciation of why data is critical and managing it effectively is an essential element for the continued success of an organization.

Next, we reviewed the data life cycle and an example within the context of Salesforce and the various operating models that are in use today to facilitate data management. We also discussed best practices associated with managing data and metadata in the context of Salesforce and what steps we can take as data architects to make data management easier. Equipped with this knowledge, you can start to gradually implement best practices and avoid the pitfalls that materialize when data is not properly taken care of.

In the last couple of sections, we reviewed backup and recovery in Salesforce, why it's important to implement processes, and explained, barring certain scenarios that relate to infrastructure management, how protecting data is the responsibility of the customer. We discussed why we should implement a robust and well-tested backup and recovery strategy and then reviewed the key considerations when choosing a backup solution. The discussion around Salesforce discontinuing the data restore service and the other reasons we discussed will hopefully have provided you with a list of good reasons to implement robust backup and restore plans. Ensure that these plans have also been through several dry-runs to iron out any wrinkles in the process.

In the next chapter, we will look at MDM, which is becoming critical as companies grow and their data volumes grow with them, along with a plethora of different systems that are part of the system landscape in a reasonable-sized company.
