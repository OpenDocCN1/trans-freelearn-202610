
This brings us to a related misunderstanding that inequality queries (such as $ne) cannot use indexes effectively. That’s false! If you query for {status: {$ne: "done"}} against the same orders collection, it will be just as selective as querying for {status: "processing"}. The MongoDB query engine handles it slightly differently (it’s processed as a range query), but being highly selective, it is still very efficient, even though it will use a different query plan:

```
// For {status: {$ne: "done"}}, the index scan uses:
"indexBounds": {
  "status": [
    "[MinKey, \"done\")",
    "(\"done\", MaxKey]"
  ]
}
// For { status: "processing" }, the index scan uses:
"indexBounds": {
  "status": [
    "[\"processing\",\"processing\"]"
  ]
}
```

These are examples of output from the explain method, which we will talk about more later, in Chapter 12, Advanced Query and Indexing Concepts, where we start trying to debug our queries and validate how well our indexes help with performance. Knowing whether an operation is an equality or range query matters a lot to our next topic of ordering fields in compound indexes.

## Constructing compound indexes

Optimal compound indexes require understanding how different queries use them. A compound index indexes multiple fields in a defined order, and that order significantly affects how MongoDB uses the index.

A compound index in MongoDB includes multiple fields, and sorts documents first by the value of the first indexed field, then by the second, and so on. Internally, it’s organized like a sorted tree structure, enabling efficient traversal when the query matches the beginning of the index key pattern. For example, a compound index on {a:1, b:1, c:1} allows MongoDB to efficiently locate documents that match queries filtering on the following:

```
{ a: <value> }
{ a: <value>, b: <value> }
{ a: <value>, b: <value>, c: <value> }
```

This is known as **prefix matching**. The query must match the prefix of the index fields in order to be eligible to use the index. A query that skips over the first field, such as {b:<value>} or {c:<value>}, cannot use this index. In those cases, MongoDB performs a full collection scan.

Let’s take a look at a more practical example. Suppose you create a compound index such as this: {category:1,price:1,stock:1}.

This index works well for queries such as {category:"books"} or {category:"books",price: {$lt:20}}. Using a sorted list can help us to visualize how the index can be utilized:

```
"books",10,399
"books",13,205
"books",15,11
"books",22,0
"pens",2,175
"pens",5,42
"pens",12,109
"pens",15,98
```

When the list is sorted by category and then price, it’s easy to quickly find all books or all pens, and once we find a single category, it’s easy to find the "price less than $20" range and return those records in order by price.

However, you can see that if I just asked for all items less than $20 without specifying a category, we would have to go through every category value, which would make the index far less useful. This is because the query skips over the category field. Since the index is sorted first by category, MongoDB has no efficient way to isolate the relevant range of documents by price alone. And the index is completely unusable to answer a query such as "what items have stock 0". We may as well do a collection scan for that query.

Understanding how prefix matching works is important for designing compound indexes that align with your application’s most common query patterns.

## Equality queries

A common query pattern involves using equality on multiple fields. For example, suppose you’re storing addresses, mostly in the United States, where the following applies:

- country is usually "US" (low cardinality)
- state has about 50 possibilities (medium cardinality)
- zipString has over 42,000 unique values (high cardinality)

Let’s say we always query with equality on all three fields:

```
db.collection.find({country: "US", state: "NY", zipString: "10001"})
```

Which compound index would perform better?

- Option 1: { country: 1, state: 1, zipString: 1 }
- Option 2: { zipString: 1, state: 1, country: 1 }

Conventional wisdom, or your gut, might say Option 2 because the most selective field is ordered first. But it turns out that it wouldn’t make the queries run any faster. For queries with equality conditions on all fields, both indexes will perform equally well. This is an important concept to understand: when selecting on all fields, the B-tree for Option 1 and Option 2 will be the same size because they have to index the same number of documents; therefore, they will be the same depth, and finding a specific value will be the exact same O(log n) for both.

That said, you should still prefer Option 1 if all else is equal. When low-cardinality fields (such as country, which might only be "US" or "CA") appear first, MongoDB can compress long runs of repeated values more effectively. Because of prefix compression, placing low-cardinality fields first will significantly reduce the index size on both disk and in RAM:

```
"country_1_state_1_zipString_1": 620 KB
"zipString_1_state_1_country_1": 804 KB
```

That’s almost a 30% difference in size! Smaller indexes mean less disk usage, faster loads, and more efficient memory usage.

There may still be valid reasons to order fields a certain way. For example, the Option 2 index can serve queries on just zipString or on both zipString and state. So, there might be good use cases for choosing Option 2 over Option 1, but expecting “faster queries” with equality on all three fields is not one of them.

Things get more interesting (and more complex) when we start adding range queries and sort operations into the mix.

## Sorts and range queries

Indexes allow us to efficiently answer range queries because we can quickly find the value at one end of the range and then proceed through the index, getting documents in order till we reach the first value above our upper bound, as follows:

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_109_0.png)

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

The index would now look like this:

```
2,"pens"
5,"pens"
10,"books"
12,"pens"
13,"books"
15,"books"
15,"pens"
22,"books"
```

In this index, when we navigate to the index range where price is less than 20, we’ll find that category is all mixed. This means we’ll have to scan more index entries with a price less than 20, throwing away ones that don’t match the equality predicate, than we would with a different index structure. And what if we needed a sort on the second field after applying range on the first field? Again, because the values are not in order, we would need to do a lot more work in memory.

The same concept applies if we have equality and sorting. If the equality is on the first field in the compound index, then the second field values are still “in order” in the part of the index we have navigated to. But if sort is on the first field in the index, then while we can get those values in order while scanning the full index, we have to keep checking and throwing away the entries where the second field is not equal to our predicate.

What if we have all three: equality, range predicate, and a sort on another field? Let’s return to our example with three fields in the {category:1,price:1,stock:1} index and these values:

```
"books",10,399
"books",13,205
"books",15,11
"books",22,0
"pens",2,175
"pens",5,42
"pens",12,109
"pens",15,98
```
