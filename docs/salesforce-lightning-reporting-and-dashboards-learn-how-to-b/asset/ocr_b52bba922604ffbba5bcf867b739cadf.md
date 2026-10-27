Remember, with field tracking the field value is not tracked until the object is enabled and the fields are selected for tracking. This means that if you enable tracking the **Account Name** today, you will not be able to track **Account Name** changes made before today.

History tracking is not just to track when you edit a record, but also when you create a new record, including records that are created when a **Lead** is converted. The field history entry will be marked as **Created** when you create a new record, or **Created by lead convert** when the record is converted from a **Lead** record; both, the old value and the new value will be blank.

# # History tracking on standard objects

History tracking supports both standard and custom objects. But not all standard objects are supported by field tracking; only the following standard objects support history tracking: **Accounts, Articles, Assets, Campaigns, Cases, Contacts, Contracts, Contract Line Items, Entitlements, Leads, Opportunities, Orders, Order Products, Products, Service Contracts, and Solutions.**

To enable history tracking on a standard object, such as **Account**, perform these steps:

+   1. Navigate to **Setup | Object and Fields | Object Manager | Accounts | Fields & Relationships**, and click on the **Set History Tracking** button.
+   2. Check the **Enable Account History** checkbox.
+   3. Select the fields you want to track.
+   4. Notice that **Description** will be tracked for changes only. This is because **Description** is a **Text Area (Long)** field, the same when you track fields with the type of **Text Area (Long)**, or **Text Area (Rich)**, and **Picklist (Multi-Select)**.
+   5. Click on the **Save** button to save the changes.