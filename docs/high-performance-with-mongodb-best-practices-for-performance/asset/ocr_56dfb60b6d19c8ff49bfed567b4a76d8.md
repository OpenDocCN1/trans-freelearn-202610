```
nReturned: Long('45'),
    executionTimeMillisEstimate: Long('2')
}, {
    '$set': { x: '$y' },
    nReturned: Long('45'),
    executionTimeMillisEstimate: Long('2')
}, {
    '$set': { a: '$b' },
    nReturned: Long('45'),
    executionTimeMillisEstimate: Long('2')
}, {
    '$project': { a: true, c: { '$add': [ '$d', '$e' ] }, _id: false },
    nReturned: Long('45'),
    executionTimeMillisEstimate: Long('2')
} /* ... */ ]}
```

Here, you can see the stages executed. The first stage is called $cursor, though it looks like it’s executing the $match stage. It shows you which fields it’s returning in the transformBy field, and you can see that the $set and $project stages took basically no time at all, as each stage shows cumulative time spent up to itself, which means each of the non-$cursor stages took 0 milliseconds to execute.

# Optimization techniques

Let’s look at various optimization techniques and how they can help your pipeline run faster.

# Filter data early

While MongoDB automatically reorders stages, understanding the principles behind these reorderings can help you write more performant pipelines. The general idea is to reduce the volume of data as early as possible:

+   - Place $match as early as possible and make sure it can be supported by an index, which will ensure that only necessary documents will be brought into the pipeline to begin with, reducing the number of documents that need to be read from the cache (or worse, from disk) and processed
+   - Place $sort as early as possible in the pipeline and make sure it can be supported by an index