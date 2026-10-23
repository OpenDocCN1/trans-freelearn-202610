We like to describe Elasticsearch as a search and analytics engine. The analytics side of Elasticsearch plays a crucial role as it allows you to perform complex queries and aggregation on data.

However, the icing on the cake is Elasticsearch’s extensibility. In 2021, Elastic introduced the capability to index vectors. This means that Elasticsearch natively understands what a vector is and is optimized for the task, not letting users manage and force the data store to handle non-supported data types.

Beyond search, we would like to open your mind and see how we could apply vector search to other use cases, such as observability and cybersecurity.

# A primer on observability and cybersecurity

Vectors are not only important for tasks such as NLP but also important in fields such as observability and cybersecurity. They allow for efficient processing and analysis of large volumes of data.

Let’s pause for a second and think about the current state of observability. Today, operations teams have several tools in their toolbelt to accelerate the investigation process, such as anomaly detection, while mainly relying on unsupervised learning. Now, text classification could be a great benefit for signals such as logs, where the data is fairly unstructured and needs to be grouped into buckets. The outcome is a better analyst experience and accelerated root cause analysis. Now, let’s think about what a future state would look like. By vectorizing data – in other words, converting data points into vectors – it becomes much easier to identify patterns and anomalies.

In cybersecurity, vectors can be used to represent and analyze multiple types of data, such as network traffic, malware signatures, and user behavior.

We will address observability and cybersecurity in Chapters 7 and 8, respectively, and look at practical examples you can use and apply in your environment, after which we will extend them to other places where we see fit.

# Summary

This book is going to guide you through pragmatic, practical examples that you will be able to replicate for your needs, ranging from building a vector-powered search application to applying vectors to domains such as observability and security. It targets a large spectrum of practitioners but is more focused on the operations teams, which deal with specific challenges and could make good use of combining vectors and Elastic.

In this chapter, you learned about the key aspects of NLP, vector search, AI-based search, and its relative techniques. You were also shown pieces of code that you can run and adapt for your business applications. Lastly, you gained a high-level understanding of Elasticsearch’s role in this context as well as how to apply vector search beyond the search use case, such as in terms of observability.

In the next chapter, we will get started by looking at vectors in Elasticsearch. We will explore the current search methodology and see how vectors are augmenting and enhancing it. Furthermore, we will dive deep into the technicality of vector implementation in Elasticsearch.