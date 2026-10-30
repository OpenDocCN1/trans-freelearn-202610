Now, let's examine the most powerful and flexible tool at your disposal: security predicates.

# Understanding the concept and usage of security predicates

Data visibility in TCRM can be controlled using security predicates. As we mentioned earlier, row-level security for a dataset that is defined by a filter condition is called a predicate. Predicates can control data visibility based on these scenarios:

+   - **Role Hierarchy**: In the Salesforce org, predicates can filter the data based on the user's role, but only if they are logged in. A user can only view the records that are owned by them or by the people below them in the role hierarchy in Salesforce.
+   - **Manager Hierarchy**: In the User object, predicates can filter the data based on the Manager field in the User object. Therefore, the logged-in user may view the records that are owned by them, and by any user that reports under them in the manager hierarchy.
+   - **Logged-in User's Country**: The data can also be filtered based on the logged-in user's country, by the predicates. A user from one country can access the records that are owned by the users from the same country.
+   - **Opportunity Team and Accounts Team**: If data visibility needs to be governed depending on the logged-in users being part of the Opportunity Team and Accounts Team of the record owner, then predicates can be used for this as well.
+   - **User Territory**: Predicates can implement row-level security based upon User Territory.
+   - **A Combination of Various Requirements**: Defines a predicate based on a combination of these scenarios.

When defining a security predicate expression, you must use a valid syntax:

```
<dataset column> <operator> <value>
```

Here is an example:

```
'UserId' == "$User.Id"
```