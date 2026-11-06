Let’s say we always query with equality on all three fields:

```
db.collection.find({country: "US", state: "NY", zipString: "10001"})
```

Which compound index would perform better?

+   - Option 1: { country: 1, state: 1, zipString: 1 }
+   - Option 2: { zipString: 1, state: 1, country: 1 }

Conventional wisdom, or your gut, might say Option 2 because the most selective field is ordered first. But it turns out that it wouldn’t make the queries run any faster. For queries with equality conditions on all fields, both indexes will perform equally well. This is an important concept to understand: when selecting on all fields, the B-tree for Option 1 and Option 2 will be the same size because they have to index the same number of documents; therefore, they will be the same depth, and finding a specific value will be the exact same O(log n) for both.

That said, you should still prefer Option 1 if all else is equal. When low-cardinality fields (such as country, which might only be "US" or "CA") appear first, MongoDB can compress long runs of repeated values more effectively. Because of prefix compression, placing low-cardinality fields first will significantly reduce the index size on both disk and in RAM:

```
"country_1_state_1_zipString_1": 620 KB
"zipString_1_state_1_country_1": 804 KB
```

That’s almost a 30% difference in size! Smaller indexes mean less disk usage, faster loads, and more efficient memory usage.

There may still be valid reasons to order fields a certain way. For example, the Option 2 index can serve queries on just zipString or on both zipString and state. So, there might be good use cases for choosing Option 2 over Option 1, but expecting “faster queries” with equality on all three fields is not one of them.

Things get more interesting (and more complex) when we start adding range queries and sort operations into the mix.

# Sorts and range queries

Indexes allow us to efficiently answer range queries because we can quickly find the value at one end of the range and then proceed through the index, getting documents in order till we reach the first value above our upper bound, as follows: