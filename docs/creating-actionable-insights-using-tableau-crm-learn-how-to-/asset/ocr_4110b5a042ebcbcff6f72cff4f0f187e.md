Here is the end result:

![](bbox=[0.117, 0.131, 0.851, 0.461])

*Figure 12.6 – The final task that was created and edited via TCRM*

Keep in mind that record actions in TCRM support these objects and global actions:

+   - Create and update object records.
+   - Log a call.
+   - Custom actions to trigger Lightning components, flows, and Visualforce pages.
+   - Lightning components that use Lightning events are supported in embedded dashboards but are not supported in Analytics Studio.
+   - Standard Chatter actions, excluding *File*.

Global quick actions and canvas apps are not supported in TCRM configured record actions. TCRM does not support actions assigned to a page layout for the user object.

As you saw briefly in *Chapter 10, To Code, or Not to Code*, TCRM also offers the advanced functionality of bulk actions, where a group of records can be actioned in Salesforce with just a few clicks. However, as this is an advanced developer feature, it will not be covered in this book.

Let's now understand how to trigger an automation in Salesforce from TCRM.