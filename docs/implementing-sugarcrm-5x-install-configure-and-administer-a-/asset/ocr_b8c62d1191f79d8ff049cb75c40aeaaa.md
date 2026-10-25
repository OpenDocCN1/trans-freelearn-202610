# # SugarCRM basics: Data relationships

Before we get too deep into specific functional areas of the application, it is important
to understand certain aspects of the system's internal relationships and hierarchy.

Throughout this book we have referred to _Accounts_, _Contacts_, _Opportunities_, and other
data entities that make up a CRM system. We have also discussed that some of the
main objectives of a CRM system are to store this type of information and also easily
demonstrate relationships among them. It is this philosophy that allows a CRM
system to not only provide you with information about the sales that are pending,
but also the clients they relate to. In order to accomplish this, rules dictating the
manner in which data entities are interrelated, or can be connected, must exist within
the CRM system. To better understand SugarCRM, we must first examine these rules
that help build its hierarchy.

Within the world of SugarCRM, an Account is the top level entity. All other data
items, including Contacts, Opportunities, Cases, and so on can all be easily linked to
an Account by default. You are also permitted to link as many of those items as you
like to any given Account. This functionality allows multiple contacts to be linked
to a single account and is commonly referred to as a one-to-many relationship.

The following illustration depicts the hierarchy:

![](bbox=[0.211, 0.473, 0.797, 0.586])

While lower level data entities are easily linked to Accounts, they are limited in the
manner in which they can be linked to each other. A Case, for example, expects to
be linked to an Account by default. It can be associated with a Contact only after
it is first linked to an Account. This example highlights an area that may require
customization depending on whether your business uses a B2B or B2C model.

In addition, there are some relationships that can only be established in a single
manner. A good example is that of a Task and an E-mail. It is possible to establish
a relationship between the two, but in order to do so; you must access the e-mail in
question and establish the relationship there because attempting to do the reverse
is not allowed. Were you to try it by accessing the Task, you would be left with the
impression that it was not possible to establish the relationship.