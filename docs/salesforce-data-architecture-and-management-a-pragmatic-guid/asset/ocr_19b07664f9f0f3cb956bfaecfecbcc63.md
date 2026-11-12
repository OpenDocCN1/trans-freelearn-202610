The Campaign Member object in the preceding screenshot has yet another field for email. So far, we have seen three **Email** fields on three different objects – Lead, Contact, and Campaign Member. Repeating the **Email** field three times would be considered unacceptable in a normalized database, but because Salesforce is tuned for read operations, it makes sense to have redundant fields, so a minimum number of joins are needed to return the query results and improve the query performance.

Another advantage of denormalization is that although duplicated, data is there for you to report on. Think of the example of the lead mapping process. In a nutshell, we have the data on the Lead object, which is mapped over to Account, Contact, and Opportunity, so when a Lead record is converted, data is copied over from the Lead object to the Account, Contact, and Opportunity objects. This gives us the ability to report on data in the Lead object. For example, what Lead sources (ad words, website, blog, and other) of Lead records returned the highest revenue on opportunities? How effective are the marketing qualification and sales qualification processes? And so forth...

# Design principles for data modeling

Design principles form the rules that you must adhere to and should not be violated unless there is a clear and compelling reason to do so. This is because they form the foundation that you design your system on. They are typically based on industry experiences, best practices, and learned experiences and provide a solid foundation on which to build your data model. Since Salesforce is optimized for read operations, understanding design principles will help us to deliver scalable and maintainable solutions in the long run. Here are some key principles to consider when designing the data model.

# User experience

You want your users to use the application you designed or built. If the user finds that they have to navigate to five different screens in order to retrieve one piece of data, that may impact buy-in from your customers and reduce system usage. A common mistake is to create too many fields on the object and place them on the page layout. You want to design your data model in a crisp manner and create fields that are needed. If you truly need many fields on the object, consider using record types, which we will talk about later. You can also use dynamic forms to control fields that appear on the UI when specified conditions are met.

# Analytics

Reporting is an important requirement and you want to make sure that your data model can support performant reports, dashboards, and list views. Since you cannot do joins in SOQL, you will need to write Apex to pull the data you need and marry it together.