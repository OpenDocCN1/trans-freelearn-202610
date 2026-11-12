- **Changing autonumber fields**: This is another area that can cause the restored data to be transformed. Make sure to change your **Autonumber** fields to the **Text** type before starting the restore as that will ensure that the data gets written the same way it is in the restore.

+   * **Data restore**: This is the execution step where data will be written back into the organization. Having the right tool to do the job at this stage can be of tremendous help. Imagine if you had to do a simple restore for Accounts, Contacts, and Opportunities and had half a million records in total. This can be done via Data Loader, but it can turn out to be a very cumbersome and painful process.

+   * **Post-processing**: This step involves reverting those settings that were made in the pre-processing step, specifically, settings such as activating validation rules, turning on triggers, process builders, and flows. You will also want to flip the **Autonumber** field back to **Auto**, making sure that you give it the correct format and a starting value. You will also want to ensure that the **Create Audit Fields** permission is removed from the profile or the permission set.

+   * **Verification**: This is the final step and involves verifying your data restore. Barring the items that we know would have changed, for example, formula fields, you will want to focus on whether the total number of records from the backup and the restored data matches. Then you can filter down on the restored dataset and compare a few other fields, such as **Stage** if it is opportunities, the **Status** field for cases, and so on. Again, having a tool that assists with verification activities can be a huge time saver, otherwise, be prepared to spend hours verifying the restore before it can be declared successful and operations can return to normal.

Let's look at some tools we can use to help us with data backups and restores in the next section.

# Backup and recovery – tools of the trade

In this section, we will discuss some tools that are widely used for backing up and restoring data. As you will see, some of these are native and free from Salesforce, while others are third-party AppExchange applications. These are just a handful of tools and the list is not exhaustive by any means, nor is it an endorsement of any one tool. We have already discussed factors that may influence your decision-making, including costs, and those should be used as a guide to determine the most optimal tool for your organization. Note that I didn't say _best_ or _cheapest_, but rather _optimal_, which means balancing the needs of your organization with practical constraints such as costs, the nature and size of your business, and data volumes.

Let's start with the native tools from Salesforce.