The single document returned had all three values in the search list in array "a", which is why we see that out of the three tested results, two were dropped as duplicates. This shows the extra work performed, which you can already see because nReturned is 1 while keysExamined is 3. The extra work is not because the index doesn’t efficiently store all the values we want to return; it’s because of the extra work done to eliminate duplicate results. While arrays are a powerful way of representing complex structures in your documents, it’s important to be aware of the extra cost some queries can have when dealing with indexed array fields.

# Challenges with null and $exists

In MQL, the query {a: null} matches all documents where a is either absent or a is present and equal to null. This includes cases where a is an array that contains null as well as some more complex scenarios.

But in all of these cases, because both missing and null are returned by this query, the B-tree indexes store the value null to indicate that a value is either explicitly null or it’s missing; that is, it is not there at all. So, indexes can be used to easily check that a field is not equal to null, but it cannot be used to answer $exists:true or $exists:false queries. That’s because the document would still need to be checked to see whether the field exists with a null value, or whether it’s not there at all. The only exception to this is sparse indexes, which only index values that exist (including null) and, therefore, can answer the $exists:true predicate directly from the index without needing to check additional details in the document itself.

Here are several explain fields when an index on a is used to check db.coll.find({a: {$ne: null}}):

```
stage: 'IXSCAN',
keyPattern: { a: 1 },
indexBounds: { a: [ '[MinKey, null)', '(null, MaxKey]' ] }
```

The index bounds show that the query is handled as a range query, which matches values less than null and greater than null.

All of this is essential to understand to write queries that are correct, but it’s also important to understand when indexes can and cannot be used as effectively as you might expect. In summary, if your application has a query that must check for the existence of a field, a sparse index can be used.