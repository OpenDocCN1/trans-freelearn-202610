# **Ownership skew**

This happens when a single user owns a large number of records, typically more than 10,000 records. For example, an integration user is assigned all the accounts from the external system. When the skewed owner exists in the role hierarchy, operations such as delete or owner changes cause the system to remove sharing from the old owner and all parent users within the hierarchy, and from users who have access through sharing rules. For this reason, ownership changes are the costliest transactional changes in the system.

If you do need to assign a large number of child records to a parent record, determine whether you can have multiple parent accounts that the child records can be distributed to, thereby avoiding the ownership skew scenario. Remember that every record in Salesforce should have an owner, irrespective of whether it’s a user or a queue.

In situations where ownership cannot be avoided, the skewed owner can be made such that they don’t have a role in the hierarchy. That way, the system doesn’t have to make any sharing calculations associated with the skewed user’s records.

# **Lookup skew**

This happens when a large number of records are associated with a single lookup record; for example, you have a custom quote object that has a lookup to a custom facility object that indicates where the repair for the component will be done. If there are an excessive number of quotes associated with a single facility record, you will run into lookup skew issues.

Lookups are basically foreign key relationships in database terms, and when a record is being inserted/updated, the associated lookup record must also be locked to maintain data integrity. This is the reason why lookup skews are not good for performance.