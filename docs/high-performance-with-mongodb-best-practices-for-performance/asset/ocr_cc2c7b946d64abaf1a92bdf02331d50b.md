+   - Timestamps and/or dates
+   - MongoDB’s automatically generated object IDs (because they include a timestamp component as the first four bytes)

In contrast, when you index a random value, access patterns will span the entire B-tree, eventually requiring the entire index to be loaded into memory, making it much less efficient. Types of random values that can force the entire index to stay in RAM include the following:

+   - UUID
+   - Most string values (names, usernames, etc.)
+   - Any hashed value

So, while index size matters, how you access that index matters even more. Designing your data access patterns thoughtfully can significantly reduce memory usage and boost performance.

# Common misconceptions about indexes in MongoDB

There are a few common myths about MongoDB (and NoSQL in general). One is that MongoDB is so fast that it doesn’t need indexes. This might only hold true if your dataset is so tiny that everything fits into RAM and every query is fast, even though it’s doing a full collection scan. This would never be the case for a real production system. Another myth is that every field is automatically indexed. In MongoDB, only the primary key (the _id field) is automatically indexed. To achieve acceptable performance in production environments, you need to analyze your query patterns and create the right secondary indexes accordingly.

# Types of indexes in MongoDB

MongoDB offers multiple types of indexes to optimize different query patterns. The majority of them use B-trees, but not all of them. This chapter will focus on the ones that do use B-trees as their underlying structure.

![](bbox=[0.14, 0.75, 0.22, 0.84])

> Field versus attribute in index types
> 
> The type of index can refer to either what fields you are indexing or what attributes you assign to the index. The former specifies the fields that will be stored in the B-tree; the latter is more about attributes of the index as a whole.

Let’s take a look at some of the more common index types.