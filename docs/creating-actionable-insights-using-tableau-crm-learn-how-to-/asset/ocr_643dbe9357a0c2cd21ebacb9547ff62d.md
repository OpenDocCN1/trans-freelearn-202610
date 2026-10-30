# A deeper understanding of how and why to flatten
your data

One of the most common use cases for flattening your data is to prepare data for use in security predicates—that is, data concerning role hierarchy is used to decide who sees what in a dataset or dashboard, based upon hierarchy data pertaining to the logged-in user.

See *Chapter 7, Security in Tableau CRM*, for more information about security predicates.

You will now learn about the hierarchy data type and how it can be used with a flatten transformation to prepare data for TCRM security purposes.

# Role hierarchy data type in Salesforce

The role hierarchy data type is associated with the standard Salesforce **User** object. It relates users to each other in a different hierarchy as an alternative to the standard manager user field.

The characteristics of the role hierarchy data type are outlined here:

+   - It is only allowed on the **User** object.
+   - It is a specialized version of the lookup relationship, in that it automatically self-references back to the **User** object, and it is similar to the standard lookup in that you can only create up to 40.
+   - In the user **interface (UI)**, a hierarchical field includes a dropdown of user types (for example, **User**, **Partner User**, **Customer Portal User**) that limits the lookup field to those users.
+   - Hierarchical fields support filters.
+   - Users selected in the hierarchy cannot be any users below that user in the hierarchy or the same user on the current record.
+   - It is supported in approval processes. Salesforce allows us to select from the standard user manager field or any hierarchical fields when configuring an approval process and determining the next approver. Also, you can create a new hierarchical field on the fly while configuring the approval process, without having to build it in advance.