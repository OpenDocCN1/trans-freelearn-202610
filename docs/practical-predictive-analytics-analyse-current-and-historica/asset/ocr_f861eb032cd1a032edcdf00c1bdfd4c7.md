# The sample market basket

Each transaction numbered 1-10 listed previously represents a basket of items purchased by a shopper. These are typically all items that are associated with a particular transaction or invoice. Each basket is enclosed within braces {}, and is referred to as an itemset. An itemset is a group of items that occur together.

Market basket algorithms construct rules in the form of:

```
Itemset{x1,x2,x3 ...} --> Itemset{y1,y2,y3...}.
```

This notation states that buyers who have purchased items on the left-hand side of the formula (lhs) have a propensity to purchase items on the right-hand side (rhs). The association is stated using the Ã symbol, which can be interpreted as implies.

# Note

The lhs of the notation is also known as the antecedent, and the rhs is known as the consequence. If nothing appears on either the left-hand side or right-hand side there is no specific association rule for those items; however, it also means that those items have appeared in the basket.