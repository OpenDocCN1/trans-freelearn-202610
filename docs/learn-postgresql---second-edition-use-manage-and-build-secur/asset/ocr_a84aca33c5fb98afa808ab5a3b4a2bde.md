Another important piece of information in the actual part of the node output is the number of rows (i.e., how many tuples the node has produced), in a similar way to the estimation of a plain EXPLAIN. Why is there a need to again report the number of rows obtained by the node? Remember that EXPLAIN estimates, while EXPLAIN ANALYZE provides you with the effective number of tuples obtained by a node. When these two values are really different, by an order of magnitude or more, PostgreSQL is not able to correctly estimate the size of the result set and hence the best plan to choose to access the data – meaning it may choose a non-optimal access method.

Last, in the actual output, there is the number of loops, that is, the number the very same node has executed. Usually this number is 1, meaning the node has been executed only once, but in the case that the node belongs to a subquery, the value could be greater than 1. The timing and number of rows are related to a single loop execution; therefore, in order to get the final values, you need to multiply rows and time by loops.

At the very end of the command output, EXPLAIN ANALYZE provides overall time information, which includes the planning time, which is the time the optimizer has spent producing the best candidate access plan, and the execution time, which is the total time spent running the query (excluding the parsing and planning time).

Therefore, the preceding example took 0.163 milliseconds to “fetch data” and 0.052 milliseconds in deciding how to fetch the data, and the total time the query took was 0.219 milliseconds (a little more than 0.052 + 0.163 because some time is spent on resource allocation).

When the data to access is very small, the planning time takes longer than the execution time.

The execution time also includes time spent running BEFORE triggers, while AFTER triggers are not counted because their function is executed once the plan has completed.

The planning time, similarly, accounts only for the time spent in producing the best access plan, not the time required to process rules and writing of the statement, as well as parsing.

EXPLAIN ANALYZE always executes the query you want to analyze; therefore, in order to avoid side effects, you should wrap EXPLAIN ANALYZE in a transaction and roll back the work once the analysis is complete.