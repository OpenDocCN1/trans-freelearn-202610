
## File storage

Salesforce has a growing number of ways to store file-based data, ranging from the historic Document tab, to the more sophisticated Content tab, to using the Files tab, not to mention Attachments, which can be applied to your Custom Object records if enabled. Each has its own pros and cons for end users and file size limits that are well defined in the Salesforce documentation. Going forward only Files will be supported by Salesforce and so use of this is recommended over other file storage options. For example, only the Files tab is supported in Lightning Experience.

From the perspective of application development, as with data storage, be aware of how much your application is generating on behalf of the user and give them a means of controlling and deleting that information. In some cases, consider whether the end user would be happy to have the option of recreating the file on demand (perhaps as a PDF) rather than always having the application to store it.

## Record identification, uniqueness, and auto numbering

Every record in Salesforce has an Id field, and with a few exceptions, also a Name field or DeveloperName for Custom Metadata Type objects. The Id field is unique, but is not human readable, meaning it cannot be entered or, for most of us on this planet, easily remembered!

The Standard and Custom Objects default value for the Name field is to provide a means for the user to enter their own textual description for the record, such as the race name or the driver name. The Name field is not enforced as being unique. Being the primary means by which the user identifies records, this can be a problem and is something that the end user needs to avoid. This aspect of the platform can be particularly problematic with accounts and contacts hence AppExchange has a number of so-called "Deduplication" applications you can try.

> The Database.merge and merge DML statements support the merging of accounts, leads, and contact records.

### Unique and external ID fields

When adding fields, you can indicate that the values stored within them are **unique** and/or **external identifiers**. For unique fields, the platform will automatically enforce this rule regardless of how records are created or updated, rejecting records with no unique values. By utilizing fields designated as external ID fields, you can simplify the data-loading process for users; this will be discussed later in this chapter in more detail. Both of these field modifiers result in a custom index being created, which can help optimize query performance.

> You can help users avoid creating duplicate records with the same name by ensuring that other identifying fields such as these are also added to the **lookup layout** as described in the previous chapter.

Let's add a **Unique** and **External Id** field to record the driver's Twitter handle. Perform the following steps in the packaging org to try this out:

- 1. Create a new **Text** field, **Twitter Handle** (`TwitterHandle__c`), with a length of **15** on the **Driver** object, and check the **Unique** and **External Id** checkboxes.
- 2. Utilize the **Driver** records entered in the previous chapters to update the fields and confirm that the system prevents duplicate Twitter handles. Lewis Hamilton's Twitter handle is LewisHamilton and Ruben's is rubarrichello.
- 3. Update the permission sets that have read and write permissions for the new field to **Race Management**, and the read-only permission to **Race Analytics**.

### Auto Number fields

The platform supports an auto sequence field type, which can be applied to the **Name** field during and after object creation (as long as the object has not been previously included as part of a prior package release upload). It can also be applied to a brand new field. The platform handles incrementing the sequence and ensures that there are no duplicates.

Apply **Auto Number** within the FormulaForce application to give a unique auto incrementing identifier to the **Race Data** records as they are inserted. Perform the following steps in the packaging org to try this out:

- 1. Create a new **Race Data** (`RaceData__c`) object; for **Record Name**, enter `Race Data Id`, and select **Auto Number** from the **Data Type** field.
- 2. In the **Display Format** field, enter `RDID-{00000000}` and `1` in the **Starting Number**. Finally, check the **Allow Reports** field.
- 3. Add a **Lookup** field called `Race` (`Race__c`) to relate records in this object to the existing `Race` object. Select the **Don't allow deletion of the lookup record that's part of a lookup relationship** option to prevent the deletion of race records if race data exists. We will add further fields to this object in a later chapter.
- 4. Create a tab for the **Race Data** object that we just created and add the tab to the **Race Management** and **Race Analytics** applications. By doing this, the tab and object will automatically be added to the package ready for the next upload.
- 5. Create a test record to observe the **Race Data Name** field value.

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_121_0.png)

- 6. Update the permission sets with the new object and fields, giving a read and write permission to **Race Management** and a read-only permission to **Race Analytics**.

While it is possible to add new custom fields to objects that have already been packaged and released to your customers, you cannot currently add new **Auto Number** fields to an object that has already been packaged, for example, the **Driver** object.

Apex tests creating test records that utilize Auto Number fields can be configured by an org setting to not increment the sequence number outside of the test, which would create gaps in the sequence from the end user's perspective. Access this setting in the subscriber org via the Options... button on the Apex Test Execution page under Setup as follows:

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_122_0.png)

The Display Format determines the formatting but not the maximum sequence number. For example, if a format of SEQ{0000} is used, and the next sequence number created is 10,000, the platform will display it as SEQ10000, ignoring the format digits. The maximum of an Auto Number field is stated in the Salesforce documentation as being a maximum length of 30 characters, of which 10 are reserved for the prefix characters. It also states that the maximum starting number must be less than 1,000,000,000.

The platform does not guarantee that there will be no gaps in an auto number sequence field. Errors in creating records can create gaps as the number assigned during the insert process is not reused. Thus, set expectations accordingly with your users when using this feature.

One workaround to creating an unbroken sequence would be to create a Custom Object with the Name field that utilizes the Auto Number field type, for example, Job References. Then, create a relationship between this object and the object records to which you require the sequence applied, for example Jobs. Then, write an Apex Scheduled job that inserts records on a daily basis into the Job References object for each Jobs record that is currently without a Job Reference relationship. Then, run reports over the Job References object. To ensure that no Job records are deleted, thus creating gaps, you can also enable the lookup referential integrity feature described in the following sections.

### Subscribers customizing the Auto Number Display Format

Be aware that Display Format cannot be customized once it is packaged and installed in the subscriber org. However, it is possible to create a custom formula field in the subscriber org to remove the prefix from the Name field value and reapply whatever prefix is needed. For example, the following formula will isolate the numeric portion of the Name field value and apply a new prefix specified in the formula (this could also be driven by a custom setting or conditional logic in the formula):

```
'MYID-' + MID(Name, 4, LEN(Name) + 4)
```

When this formula field is applied for a record whose Auto Number is RD-000000001, the formula would display as follows:

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_123_0.png)
