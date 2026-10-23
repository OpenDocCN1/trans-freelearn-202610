Now, let’s learn how to provide context to the relationship between related Accounts and Contacts.

# AccountContactRole

To represent the actual role that a particular Contact record plays against an Account record, such as Decision Maker, Buyer, and so on, the AccountContactRole object is a lightweight way of associating Contacts in a way that provides context.

Imagine that you are a salesperson using Salesforce and are selling to a particular company (Account). Knowing who (the Contact records) does what (the AccountContactRole records) makes selling more targeted and helps ensure you can have the right conversations with the right people.

The AccountContactRole object is a junction object, in that it contains lookups to the Account and Contact objects – that is, the AccountId and ContactID fields, respectively. As you’d expect, it is also the object that’s used to capture the role the Contact performs. This is represented by the Role field.

This junction object can be represented in a data model with Account and Contact as follows:

![](bbox=[0.26, 0.43, 0.68, 0.75])

Figure 8.5 – AccountContactRole as it fits within the standard Salesforce data model (OWDs intentionally omitted)

With the AccountContactRole object covered, let’s look at Account Teams.