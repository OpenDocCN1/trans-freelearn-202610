# 9

# Fail Safe Mechanism –
Transactions

We have applied query object patterns to isolate query objects from the repository, which maintains query logic in one single place. We created a simple query object, then invented a repository to illustrate how query objects could be accommodated in the repositories. We then moved from queries to commands to perform real transactions using a similar pattern. Finally, we worked with expression trees to simplify and configure the conditions in a query execution.

Let's discuss how we could handle failure scenarios using transactions, how the transaction could be accessed between contexts, and also between different data access technologies:

+   - Is there any complex system that performs more than one data manipulation activity, then handles failures to maintain atomicity?
+     - Transactions allow us to handle atomicity in failure scenarios.
+   - Do we need to handle transactions in simple applications?
+     - Not required. The default behavior in EF would handle transactions only if all the changes were committed through SaveChanges.
+   - What if we need transactions to be maintained between multiple contexts in a complex system?
+     - We do have provision to maintain transactions between multiple database contexts.