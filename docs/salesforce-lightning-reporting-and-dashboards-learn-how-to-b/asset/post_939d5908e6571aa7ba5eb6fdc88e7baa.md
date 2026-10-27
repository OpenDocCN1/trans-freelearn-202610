
## History tracking on custom objects

For custom objects, you need to enable tracking in the object details before selecting the fields to track. Let's say we have a custom object named `Claim`. Perform the following steps:

1. Navigate to **Setup | Objects and Fields | Object Manager**.
2. Click on the **Claim** object link and click on the **Edit** button.
3. Enable **Track Field History** in optional features, and click on the **Save** button. This is shown in the following screenshot:

    ![](img/72d01bea5a9ccf02f9452aab7505d9de_347_0.png)

4. Select **Fields & Relationships** from the menu on the left-hand side.
5. Click on the **Set History Tracking** button, and select the fields you want to track.
6. Do the same with the standard object, for the **Text Area (Long)**, **Text Area (Rich)**, and **Picklist (Multi-Select)** fields—it will track changes only, not the old and new values.
7. Click on the **Save** button to save the changes.

## History objects

When you enable **Field History Tracking** for an object, at the backend, Salesforce will create a new object. The object name will end with **History**, for example, **AccountHistory**, **ContactHistory**, and so on for the standard object, and for the custom object it would be **__History**, for example, `Claim__History`.

When any changes happen to a tracked field in a record, Salesforce will create a new record in this object. For example, if you have five tracked fields in an object, and three of them change in one transaction, Salesforce will create three records in this object.

As a developer or admin familiar with SOQL, you can look into the object to query for the following:

- `CreatedById`: User who makes the change
- `CreatedDate`: Date of change
- `OldValue`: Shows the old value
- `NewValue`: Shows the new value
- `ParentId`: (for custom object), the record ID related to the changes
- `ObjectId` (for example: `AccountId` or `ContactId` for standard object)

You cannot create, update, or delete records in this object manually, you can only query. For a good reason, the storage usage for history objects is not counted as data storage.

## Reading the history tracking value

Once you enable history tracking, there are two ways to see the tracking value, not including using an API query. These two ways are discussed next.

### The history related list

Add a field history information related list to the object page layout. This information will be visible to all users assigned to the particular page layout, just in case you have multiple page layouts.

To add the history related list to the object page layout, open the page layout editor. In this exercise, let us take the Account object, so the related list for this would be Account History. Select Account History in Related Lists, and drag it to the page layout. This is shown in the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_349_0.png)

Once the history related list is added to the page layout, any update to the fields tracked will be shown here, as seen in the following screenshot under the Related tab (left arrow); but you can also add it as a related list in the Lightning component (right arrow):

![](img/72d01bea5a9ccf02f9452aab7505d9de_350_0.png)

Let us analyze the preceding screenshot:

- The history is sorted by date in descending order
- The first history shows when the record was created
- When updating more than one field in one transaction, it will track as two different items, but with the same date and time; see changes in Account Owner and Account Number
- Because Description is a long text area field, it will not track the original and new value, but just track that a change occurred

All objects will have the same label, and end with History, for example, Account History, Contact History, Claim History, and so on. But for the Opportunity object, it is an exception—it is called Opportunity Field History. Consider the following screenshot for a sample of Opportunity Field History:

![](img/72d01bea5a9ccf02f9452aab7505d9de_351_0.png)

The same is true for an API object name—it will be called **Opportunity Field History** instead of **Opportunity History**. The **Opportunity History** object is used to store **Opportunity Stage** information.

### History reports

Another option to get a history tracking value is using a report. The user just needs to have the **Run Report** permission, and the permission to access the object to run the report created.

To create the report, start by selecting a custom report type such as **Account History** report, as shown in the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_352_0.png)

By default, it will show **My accounts**, but you can modify it like a normal report. Add a filter, add groupings, change the report format, and add/remove fields of the report. If you look at the left panel for field availability, only fields from the **History Data** group will contain historical data, while other fields will show live data in Salesforce if you bring that field to the report. Consider the following screenshot for fields under **History Data**:

![](img/72d01bea5a9ccf02f9452aab7505d9de_352_1.png)

If you notice in the preceding screenshot, Parent ID for the **Account History** report type is **Account ID**. This will be the same for other objects; for example, if this is **Contact History**, the **Parent ID** would be Contact ID.

For a custom object, once you enable **Track Field History** in the object details, create a new custom report, and notice the report type ending with `History` is available for you to use, such as `Claim History`. Similar to the aforementioned **Account History**, you can use this report type as a normal report. Consider the following screenshot for the report:

| HISTORY ID | PARENT ID | ACCOUNT NAME | EDITED BY | EDIT DATE | FIELD / EVENT | OLD VALUE | NEW VALUE |
|---|---|---|---|---|---|---|---|
| 017B000002tGNh | 001B000000SWOQO | Electric Inc Limited | Alvin Lee | 5/24/2017 9:10 PM | Account Number | - | 8001 |
| 017B000002tGfz | 001B000000SWOQO | Electric Inc Limited | Johan Yu | 5/24/2017 9:10 PM | Account Owner | Johan Yu | Alvin Lee |
| 017B000002tGcL | 001B000000SWOQO | Electric Inc Limited | Johan Yu | 5/24/2017 9:09 PM | Description | - | - |
| 017B000002tGcK | 001B000000SWOQO | Electric Inc Limited | Johan Yu | 5/24/2017 9:08 PM | Account Name | Electric Inc | Electric Inc Limited |
| 017B000002tGyA | 001B000000SWOQO | Electric Inc Limited | Johan Yu | 5/24/2017 9:08 PM | Created | - | - |
| 017B000002tGVC | 001B000000SWOQJ | Big Inc. Limited | Johan Yu | 5/24/2017 9:07 PM | Description | - | - |
| 017B000002tGVB | 001B000000SWOQJ | Big Inc. Limited | Johan Yu | 5/24/2017 9:07 PM | Account Name | Big Inc. | Big Inc. Limited |
| 017B000002tGWM | 001B000000SWOQJ | Big Inc. Limited | Johan Yu | 5/24/2017 9:07 PM | Created | - | - |

## Limitations of using field history tracking

Some of the limitations have been explained previously, but let us summarize them here:

- By default, only a maximum of 20 fields can be tracked
- For **Text Area (Long)**, **Text Area (Rich)**, and **Picklist (Multi-Select)** types of fields, you cannot track the original and new value
- Not all standard objects can be tracked; for example, **User**, **Task**, **Event**, **Campaign**, and **Quote**
- Not all fields can be tracked; for example, the **Formula** field, **Roll-Up Summary**, auto-number fields, **Created By**, **Last Modified By**, and **Expected Revenue** fields on **Opportunity**

## Reporting on historical data with Reporting Snapshots

Can we have a report on data trending in Salesforce? Does Salesforce only report current data? Field history tracking will store old and new values whenever value changes in fields are detected. But to use it for trending would be too difficult, unless you export the data to an external database and analyze it with live data—it will not include deleted data either (if any).
