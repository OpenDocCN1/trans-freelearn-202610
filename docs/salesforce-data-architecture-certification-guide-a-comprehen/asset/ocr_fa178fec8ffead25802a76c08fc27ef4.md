For the requirement relating to legal professionals that deal with multiple companies, the `AccountContactRelation` object can be used.

Lastly, the requirement for teams of users dealing with corporate clients is possible using the `AccountTeamMember` object.

While the object model focuses purely on Accounts and Contacts (since no OWDs are included), it’s possible to see how several business requirements can be achieved in a straightforward manner using standard Salesforce objects.

Now that we’ve covered an example that brings everything together, let’s revisit a performance issue that affects Accounts and Contacts: Account Skew.

# Revisiting Account Skew

As introduced in *Chapter 2, Data Modeling and Database Design*, **Account Skew** is the result of an Account record containing 10,000 or more child records. This causes performance and record locking issues.

When updates are made to the child record of an Account, Salesforce locks the parent Account record to maintain record integrity in the database. Updating a large number of child records under the same Account record may cause issues since each of those child record operations will be trying to lock the parent’s Account record.

Additionally, when updates are made to an Account record that affects sharing, such as a seemingly harmless action such as changing the record owner, then all sharing for that Account will have to be recalculated. This goes for all child records of the account. This can cause many inter-connected processes to fail since sharing rules, Role Hierarchy calculations, and other sharing operations will have to take place. If any of these operations lock a record or take too long, contention may occur.

# Mitigating Account Skew

The simple rule for mitigating Account Skew is to distribute child records across multiple parent Account records. Remember, 10,000 is the maximum advised number of child records for a particular account record before Account Skew may occur.

With Account Skew revisited, let’s summarize what we’ve covered in this chapter.

# Summary

In this chapter, we dug deep into the Account and Contact model, understanding its position at the center of the wider data model for many core Salesforce cloud products. First, we looked at the nuances involved in enabling Person Accounts and looked at the Contacts to multiple Accounts feature. The wider data model that surrounds the Account and Contact objects was also explored. Finally, we revisited Account Skew to understand its importance in terms of performance.