htm, the `Individual` object represents a customer's data privacy and protection preferences. Data privacy records based on the Individual object store your customers preferences. Data privacy records are associated with related leads, contacts, person accounts, and users.

When the `Individual` object is enabled, it can be exposed to users by adding the `Individual` field to the **Lead**, **Contact**, and **Person Account** page layouts.

> **Important note**
> Enabling the `Individual` object: The `Individual` object is available if **Data Protection and Privacy** is enabled in Salesforce. This can be done by selecting **Make data protection details available in records** under **Data Protection and Privacy** in **Salesforce Setup**.

# *Dealing with GDPR data*

Personal data and sensitive personal data are covered by the GDPR and need to be **identified**, **classified**, and **protected**. How does Salesforce help address data protection? Well, Salesforce includes several features to assist with protecting data from unauthorized access, including the following:

+   - Classic and platform Shield Encryption
+   - Data masking
+   - Field-level security
+   - Sharing settings
+   - Event Monitoring
+   - Session security
+   - Field history tracking

Let's look at these topics in detail.

# *Classic encryption*

Included with a base license, **classic encryption** provides encryption for data at rest for custom fields (a distinct custom field type) using the 128-bit **Advanced Encryption Standard (AES)**. Users will need the **View Encrypted Data** profile permission enabled within their profile or via an assigned permission set to read encrypted field values. Classic encryption facilitates masking encrypted fields from users who don't have permission to read the values. They will be presented with a series of asterisks instead. Encrypted fields that use classic encryption cannot be searched or used in automation such as Flow, approval processes, workflow updates, and so on. Therefore, consider classic encryption when encrypting certain data fields that simply store data in a manner that is masked to those who shouldn't see it when viewing the record. If the field requires data to be encrypted at rest, needs to be searchable by users, or readable in automation scenarios such as Flow, consider Shield Encryption instead.