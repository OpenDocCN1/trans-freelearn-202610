Now, let’s explore the wider object model that surrounds the Account and Contact objects.

# Exploring objects associated with Accounts and Contacts

Despite having an explicit relationship between each other, where multiple Contact records can belong to the same Account, there are a couple of additional objects that allow for context to be given to the relationship between the records of these two objects.

Now, let’s explore the various Account and Contact object associations, starting with AccountContactRelation.

# AccountContactRelation

We described the Contacts to multiple Accounts feature earlier in this chapter, thus covering how a single Contact record can be associated with more than one account. Well, the AccountContactRelation object is the underlying data model entity that facilitates this Salesforce feature.

The AccountContactRelation object essentially acts as a junction object between Account and Contact. Some standard fields are used to provide context about the relationship a Contact has to an Account. These can include things such as Start/End dates for when the Contact relationship with the Account started and ended. They may also include the Roles they may play on the Account, where more than one can be selected. Other fields may include denoting information such as if the relationship is active, and if the relationship is to the Contacts primary account (using the IsDirect field).

The underlying data model for this entity can be visualized as follows:

![](bbox=[0.28, 0.6, 0.7, 0.9])

Figure 8.4 – AccountContactRelation as it fits within the standard Salesforce data model (OWDs intentionally omitted)