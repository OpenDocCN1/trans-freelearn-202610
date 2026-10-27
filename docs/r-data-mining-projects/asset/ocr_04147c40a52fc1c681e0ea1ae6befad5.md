Support of X = number of transactions involving X / total number of transactions

+   - **Confidence**: Confidence always refers to the confidence of a rule. It can be defined as confidence of x => y. Hence, support (x U y) implies the number of transactions in the database containing both item set x and item y:
+   
+     Confidence of X => Y = support of (X U Y) / support of (X)
+   - **Lift**: Lift can be defined as the proportion of observed support to expected support:
+   
+     Lift of a rule X => Y = support of (X U Y) / support of (X) * support of (Y)

An association rule stands supported in a transactional database if it satisfies the minimum support and minimum confidence criteria.

Let's take an example to understand the concepts.

The following is an example from the Groceries.csv dataset from the arules library in R:

| Transaction ID | Items |
|---|---|
| 001 | Bread, banana, milk, butter |
| 002 | Milk, butter, banana |
| 003 | Curd, milk, banana |
| 004 | Curd, bread, butter |
| 005 | Milk, curd, butter, banana |
| 006 | Milk, curd, bread |

The support of item set X where X = {milk, banana} = proportion of transactions where milk and banana bought together; 4/6 = 0.67, hence, the support of the X item set is 0.67.

Let's assume Y is curd; the confidence of X => Y, which is {milk, banana} => {curd}, is the proportion of support (X U Y) / support(X); three products—milk, banana, and curd—are purchased together 2 times. Hence the confidence of the rule = (2/6)/0.67 = 0.5. This implies that 50% of the transactions containing milk and banana the rule are correct.

Confidence of a rule can be interpreted as a conditional probability of purchasing Y such that the item set X has already been purchased.