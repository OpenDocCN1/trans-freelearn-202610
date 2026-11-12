DSO
DSO is the average number of days it takes a company to receive payment once
a sale has been made.

In the next section, we will understand metadata management – an essential component
of data governance.

# Metadata management

**Metadata** is *data about data*, meaning it provides the necessary context for data. For
example, you have a **Formula** field called **Days between Stage** defined in Salesforce to
calculate the number of days between each stage on the Opportunity object. The value
in the field is the data that needs to be managed but the fact that this field is of the type
**Formula** with a return type of **Number** and the actual formula definition to calculate the
result are all examples of metadata.

This is the technical aspect of metadata, but metadata encompasses the business definition
as well. In a comprehensive metadata document, you will notice that it answers the *five Ws*
and *the How* question. Let’s use our example from the previous section and see how we
would drill deeper to understand the metadata of this field in more detail:

+   • **Why?**: This is related to understanding the rationale of why we are storing this data.
+   This is the most important question to ask. For our example, the reason why we are
+   storing this data is so we can report on the number of days it takes to move between
+   stages.
+   
+   • **What?**: Another question we need to ask is the purpose of the field and the drivers
+   behind using it. This pertains to the business definition of the data and the business
+   rules applicable to it. For our example, the business definition could be *calculate the
+   number of days between the current stage and the previous stage for an Opportunity
+   record*. Another question answered is what are the business rules for this data? For
+   example, when the Opportunity record is in the Initial Discovery stage (the very
+   first stage in the Stage picklist), the value of this field will be blank.

Another aspect of this question is to ask what the acronyms or abbreviations for
this field are. For us, it could be **NoD (Stage)** referring to **Number of Days** for the
**Stage** field or **NDS** referring to **Number of Days** for the **Stage** field. The database
field name is also mentioned here, which in our example is Number_of_Days_
Stage__c, where **Field Type** is **Formula** with the return type as **Number** and 0
decimal places.