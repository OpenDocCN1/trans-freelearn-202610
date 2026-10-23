+   4. Let’s imagine there is an active territory model that’s granting selected users read/write access to Account records belonging to a Territory. The Account record has a Master/Detail relationship with a custom object that contains child records associated with the Account record. What access will the users have to the custom object records?
+   5. True or false? Territory Management Models can be tested before they are activated.
+   6. True or false? Out of the box, only Account records can be assigned using territory management in a declarative manner.
+   7. How are selected (that is, using a filter) Opportunity record assignments configured using territory management?
+   8. In its simplest definition, what does territory management do?
+   9. With territory management, if a user is assigned to Accounts belonging to a particular Territory with a Read-Only access level, what access do they have to the parent Territory?
+   10. What are the three territory model states?

# Answers

How did you get on answering the practice questions? Check out your answers here:

+   1. Enterprise Territory Management.
+   2. The Territory Hierarchy.
+   3. Territory Type Priority.
+   4. Read/Write.
+   5. True.
+   6. True
+   7. They aren’t configured as such. Territory Management Opportunity Assignment works by editing an Apex class.
+   8. It shares Account and Opportunity records based on criteria associated with the records to select sets of users.
+   9. Read-Only, as territory management propagates sharing access upwards.
+   10. Planning, Active, and Archived.

# Further reading

To learn more about territory management, check out the *Territory Management Implementation Guide* at https://resources.docs.salesforce.com/latest/latest/en-us/sfdc/pdf/salesforce_implementing_territory_mgmt2_guide.pdf.