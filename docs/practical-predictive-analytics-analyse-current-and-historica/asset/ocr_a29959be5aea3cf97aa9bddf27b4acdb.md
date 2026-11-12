# # Evaluating the accuracy of a rule

Three main metrics have been developed that measure the importance, or accuracy of an association rule: support, confidence, and lift.

# ## Support

Support measures how frequently the items occur together. Imagine having a shopping cart in which there can be a very large number of combinations of items. Some items that occur rarely could be excluded from the analysis.
When an item occurs frequently you will have more confidence in the
association among the items, since it will be a more popular item. Often your
analysis will be centered around items with high support.

# ## Calculating support

Calculating support is simple. You first calculate a proportion by counting
the number of times that the items in the rule appear in the basket divided by
the total number of occurences in the itemsets:

# ## Examples

+   * We can see that for the first rule (index #63), {bottled water} and
+   {tropical fruit} appear together in the same transaction in two
+   different transactions (2 and 3), therefore the support for that rule is 2/10
+   or 20%
+   
+   * The last rule shown, rule (index #3) has a support of 0.10, since soda
+   with rolls/buns only appears once in the 10 transactions (1/10)

# ## Confidence

Confidence is the conditional probability that the event on the right-hand side