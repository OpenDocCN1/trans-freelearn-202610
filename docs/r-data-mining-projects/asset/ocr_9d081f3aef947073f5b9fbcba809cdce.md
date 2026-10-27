![](bbox=[0.18, 0.13, 0.81, 0.51])

# Apriori algorithm

Apriori algorithm uses a downward closure property, which states that any subsets of a frequent item set and also frequent item sets. The apriori algorithm uses level-wise search for frequent item sets. This algorithm only creates rules with one item in the right-hand side of the equation (RHS), which is known as consequent; left-hand side of the equation (LHS) known as antecedent. This implies that rules with one item in RHS and blank LHS may appear as valid rules; to avoid these blank rules, the minimum length parameter needs to be changed from 1 to 2:

```
> # Get the association rules based on apriori algo
> rules <- apriori(Groceries, parameter = list(supp = 0.01, conf = 0.10))
Parameter specification:
confidence minval smax arem aval originalSupport support minlen maxlen target ext
0.1 0.1 1 none FALSE TRUE 0.01 1 10 rules FALSE
Algorithmic control:
filter tree heap memopt load sort verbose
0.1 TRUE TRUE FALSE TRUE 2 TRUE
```