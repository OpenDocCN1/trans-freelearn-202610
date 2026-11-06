# $facet

$facet processes multiple aggregation pipelines within a single stage on the same set of input documents. There is a misconception that the $facet stage does some aggregation magic to parallel-process a pipeline into multiple branches. That’s not really what happens at all. The simplest way to think about $facet is this: every document flowing through the pipeline up to that point is sent to each branch of $facet. This means that if one branch needs only the first 10 documents, but another needs 1 million, then all 1 million documents will be sent to every $facet branch.

$facet is an excellent candidate when you want to do extremely similar processing in each branch, for instance, when computing both counts by category and counts by price range of all incoming documents. It is not at all a good candidate when the processing you want to do in its branches is very different. One example would be wanting to return the first 10 documents as well as the total count of documents. In such cases, it’s much better to use $unionWith, which runs two (or more) pipelines while preserving all the optimizations that can be performed on each of them.

# $where, $function, $accumulator, and mapReduce

When run on the server, JavaScript will always be slower than expressing the same logic using native MQL syntax. In addition, JavaScript is inherently a security risk for MongoDB servers and has been deprecated with the intention of eventually removing it altogether to eliminate that risk.

For faster execution and better security, avoid using any JavaScript in your MongoDB queries or aggregations.

> **JavaScript execution: Server versus mongosh shell**
> 
> This doesn’t apply to the mongosh shell, which expands and interprets all the JavaScript before sending it to the server. This warning is specifically for any and all JavaScript that is meant to be executed on the database server itself.

# $text

If you wanted to do a full-text search before the MongoDB Atlas full-text search feature was introduced, your options were either using a $text operator, which relied on legacy MongoDB text indexes, or sending your data to another system. Since the launch of Atlas Search, development on native legacy text indexes has been stopped, and it’s fully expected that this feature will eventually be deprecated in favor of Lucene-based search. This newer search capability will be available in the MongoDB Community version as well as MongoDB Atlas.