# 3

# Indexes

What’s the most common database operation? It’s a query, or a read of a single document or several documents. We insert documents once, we update them sometimes, and we read them a lot. So, it’s important to ensure that those reads are as fast as they can possibly be.

In this chapter, you’ll learn how to use indexes to improve the performance of your queries. You’ll start by understanding what indexes are and how MongoDB uses them. Then, you’ll explore query plans and how to interpret them to identify slow or inefficient operations. You’ll also practice creating and using indexes to speed up queries and see how MongoDB chooses which index to use. By the end of this chapter, you’ll be able to make informed decisions about when and how to use indexes to optimize performance.

This chapter will cover the following topics:

+   - Indexes and the types of indexes MongoDB supports
+   - Cardinality, selectivity, and how they affect indexes
+   - Best practices for optimizing compound indexes
+   - Why partial indexes and covered index queries are important for performance

# Introduction to indexes

Let’s begin by looking at a basic example. Say you have a collection of orders, and you want to find the order with the ID value of 295:

```
db.orders.find({ orderId: 295 })
```

Without an index, MongoDB has to examine every document in the collection to check whether it has an orderId field, and then see whether its value is 295. For a collection with millions of documents, this can be painfully slow. It results in a time complexity of O(n), meaning the time it takes grows linearly with the number of documents.