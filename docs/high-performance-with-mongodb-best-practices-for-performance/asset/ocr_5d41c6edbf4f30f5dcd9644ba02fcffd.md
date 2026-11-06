# Avoid common $lookup performance issues

The $lookup stage enables *joins* between collections, but it can be a performance bottleneck if not used with care. Understanding two *types* of $lookup is crucial:

**Local/foreign field lookups** (the *standard* form using `localField` and `foreignField`) compare fields between collections using regular `find` semantics. MongoDB executes these the same way as if you had a `$match` stage as the first *inner* pipeline stage, which uses the value of `localField` in `find` with the `{foreignField: {$eq: <localField-value>}}` condition when `localField` is a scalar, or `{foreignField: {$in: <localField-array>}}` when the `localField` value is an array. This allows all indexes on the `from` or foreign collection to be used the same way they are used in normal `find` commands with the `$eq` or `$in` operators.

> **Using the pipeline option for enhanced joins**
> 
> The pipeline option can be used with *local/foreign* syntax to add more stages to implement additional join conditions, reduce the size of returned documents (that go into an as array), and do any other processing that the join requires.

**Expressive lookups** (not using `localField` and `foreignField` and only using the pipeline option) usually specify the join conditions in an explicit `$match` stage that should be first in the pipeline, and can reference fields from the input documents (assigned to variables in the `let` option) in the sub-pipeline. When there is no condition depending on field(s) from the input documents, this results in an *uncorrelated* subquery, which is only executed once and then its results cached, rather than executing it once for each incoming document in the pipeline. This syntax should only be used when there is *no* equality join condition.

> **Standard versus expressive $lookup**
> 
> The only difference between standard and expressive lookups is that one of them includes `localField` and `foreignField` while the other one does not. They can both specify `pipeline` to add stages to the $lookup execution.