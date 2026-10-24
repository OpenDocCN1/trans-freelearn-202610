The first step is to break up the text string for each row into a list of words. This can be done across many different nodes all at the same time. You would write Python code that does this, and then provide it to Spark to send to each executor so it can use it. This would be the first Map step. The next step would be to take each word and put a number next to it, so it can be summed up later. The form would be (fantastic, 1). At this point, the number would always be 1 as the same word will be in the dataset as often as it is present. This is the second map step, and you would use the same procedure as we discussed earlier.

Next, the results should be ordered by the word across the cluster so it can be counted easier. This is a shuffle step. Then, each executor can add the numbers attached to each of the words together to get a total for the words it has on its plate. This reduces the rows to an individual word subtotal for each executor. Hence, this is called a Reduce step. Finally, all the subtotals are added together for an overall total for the cluster by each individual word in the final reduce step.

Taken together, this concept is called Map/Reduce. This is a very simple example, and in practice, systems such as Spark handle much of the details for you. But, the overall concept holds even at higher-level applications where map and reduce is not directly called upon. You should think how to break apart the work so it can be distributed, then aggregate it into a much smaller result set for further analytics.

# Using Spark for IoT data processing

Using our conceptual diagram from earlier in the chapter, Spark fits into both the analysis interaction and the execution engine areas. You can write code directly on the core Spark objects and also make use of the programming modules that come along with it. The following image helps to visualize the Spark module stack:

![](bbox=[0.28, 0.7, 0.72, 0.85])

Spark Module Stack. Source: Apache Spark