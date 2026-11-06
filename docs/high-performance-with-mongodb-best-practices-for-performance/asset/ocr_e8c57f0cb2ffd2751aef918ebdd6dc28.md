![](bbox=[0.22, 0.1, 0.82, 0.26])

Figure 3.3: Using indexes for efficient range queries and sorting

For a query on {field: {$gt: 205, $lt:290}}, we find our lower range, 205, and then traverse along the leaf nodes until we encounter a value greater than our upper range, 290. Note that we are getting these values in order from the index, so if the query had sort:{field:1}, we would get the values in sorted order “for free.” If the sort expression was on {field:-1}, we would just start by looking for 290, the upper value of the range first, and then scan the index right to left (i.e., backward rather than forward) till we got past 205, the lower boundary, which would give us the values in descending order.

What happens if we have an equality query on one field and a range query on another field? Let's look at the index on {category:1,price:1} and the {category:"books",price:{$lt:20}} query. We already saw that we can easily find documents with equality on category and range (or sort) on price:

```
"books",10
"books",13
"books",15
"books",22
"pens",2
"pens",5
"pens",12
"pens",15
```

Since the equality field comes first, we can navigate directly to the part of the sorted tree matching that value, and the second field remains in order, so applying a range filter is just as straightforward. But what happens if the index is on {price:1,category:1}, which means the range filter is on the first field, and equality is on the second field?