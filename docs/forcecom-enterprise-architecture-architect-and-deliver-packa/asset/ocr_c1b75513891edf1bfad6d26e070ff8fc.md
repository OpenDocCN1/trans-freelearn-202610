# # Unique and external ID fields

When adding fields, you can indicate that the values stored within them are **unique** and/or **external identifiers**. For unique fields, the platform will automatically enforce this rule regardless of how records are created or updated, rejecting records with no unique values. By utilizing fields designated as external ID fields, you can simplify the data-loading process for users; this will be discussed later in this chapter in more detail. Both of these field modifiers result in a custom index being created, which can help optimize query performance.

> You can help users avoid creating duplicate records with the same name by ensuring that other identifying fields such as these are also added to the **lookup layout** as described in the previous chapter.

Let's add a **Unique** and **External Id** field to record the driver's Twitter handle. Perform the following steps in the packaging org to try this out:

+   1. Create a new **Text** field, **Twitter Handle** (`TwitterHandle__c`), with a length of **15** on the **Driver** object, and check the **Unique** and **External Id** checkboxes.
+   2. Utilize the **Driver** records entered in the previous chapters to update the fields and confirm that the system prevents duplicate Twitter handles. Lewis Hamilton's Twitter handle is LewisHamilton and Ruben's is rubarrrichello.
+   1. Update the permission sets that have read and write permissions for the new field to **Race Management**, and the read-only permission to **Race Analytics**.

# # Auto Number fields

The platform supports an auto sequence field type, which can be applied to the **Name** field during and after object creation (as long as the object has not been previously included as part of a prior package release upload). It can also be applied to a brand new field. The platform handles incrementing the sequence and ensures that there are no duplicates.

Apply **Auto Number** within the FormulaForce application to give a unique auto incrementing identifier to the **Race Data** records as they are inserted. Perform the following steps in the packaging org to try this out:

+   1. Create a new **Race Data** (`RaceData__c`) object; for **Record Name**, enter `Race Data Id`, and select **Auto Number** from the **Data Type** field.
+   2. In the **Display Format** field, enter `RDID-{00000000}` and `1` in the **Starting Number**. Finally, check the **Allow Reports** field.