With these business requirements, we first need a plan. Map this process end to end conceptually. These are the steps we need to take to enable this business requirement:

+   1. Create a Salesforce object (add a tab and assign it to an app so that users with access can access the records).
+   2. Create an Apex sharing reason.
+   3. Make the previously created object private.
+   4. Create a record-triggered flow on this new object:
+      A. Unshare with old owners.
+      B. Share with the new owners.
+   5. Save and activate your flow.

Let’s review these steps in detail with screenshots.

# Step 1 – Create a Salesforce object

This is the object that holds the contact’s sensitive information records.

To create a new object in Salesforce, go to Setup | Create | Objects | New Custom Object.

We created a new custom object with labels and fields as listed next. This custom object holds sensitive information and should be shared only with the contact owner and account owner as per the requirements:

+   - Custom object label = **Contact Sensitive Info**
+   - Custom fields:
+     - **Contact Name**
+     - **Contact Sensitive Info Name**
+     - **Data of Birth**
+     - **Pass Code**
+     - **signature verification image**
+     - **Tax ID**