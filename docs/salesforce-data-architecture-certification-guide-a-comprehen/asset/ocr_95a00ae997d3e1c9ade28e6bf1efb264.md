![](bbox=[0.411, 0.102, 0.555, 0.365])

Figure 8.3 – A diagrammatic representation of a Person Account

Now that we’ve covered Person Accounts, let’s learn how to associate a Contact record with more than one Account record in Salesforce.

# Exploring the Contacts to multiple Accounts feature

As you would expect, the Contacts to multiple Accounts feature allows a Contact record to be associated with more than one Account record. When Contacts to multiple Accounts is enabled, Contact records essentially have a primary Account record relationship and zero or more indirect Account relationships.

> **Important note**
> 
> Disabling the Contacts to multiple Accounts feature will effectively delete all indirect Account relationships for your Contact records.

When enabled, the Account object’s standard Contacts-related list can be replaced on page layouts with the **Related Contacts** list instead. Contact page layouts can have the **Related Accounts** list added to them. Person Accounts can have both related lists added to their page layouts.

> **Important note**
> 
> By default, any activities associated with a Contact record will roll up to the primary Account record for the contact. It is possible to disable this default behavior by unselecting the **Roll up activities to a contact’s primary account** option in **Activities Settings** in **Salesforce Setup**.