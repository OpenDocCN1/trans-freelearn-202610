
We like to describe Elasticsearch as a search and analytics engine. The analytics side of Elasticsearch plays a crucial role as it allows you to perform complex queries and aggregation on data.

However, the icing on the cake is Elasticsearch’s extensibility. In 2021, Elastic introduced the capability to index vectors. This means that Elasticsearch natively understands what a vector is and is optimized for the task, not letting users manage and force the data store to handle non-supported data types.

Beyond search, we would like to open your mind and see how we could apply vector search to other use cases, such as observability and cybersecurity.

## A primer on observability and cybersecurity

Vectors are not only important for tasks such as NLP but also important in fields such as observability and cybersecurity. They allow for efficient processing and analysis of large volumes of data.

Let’s pause for a second and think about the current state of observability. Today, operations teams have several tools in their toolbelt to accelerate the investigation process, such as anomaly detection, while mainly relying on unsupervised learning. Now, text classification could be a great benefit for signals such as logs, where the data is fairly unstructured and needs to be grouped into buckets. The outcome is a better analyst experience and accelerated root cause analysis. Now, let’s think about what a future state would look like. By vectorizing data – in other words, converting data points into vectors – it becomes much easier to identify patterns and anomalies.

In cybersecurity, vectors can be used to represent and analyze multiple types of data, such as network traffic, malware signatures, and user behavior.

We will address observability and cybersecurity in Chapters 7 and 8, respectively, and look at practical examples you can use and apply in your environment, after which we will extend them to other places where we see fit.

## Summary

This book is going to guide you through pragmatic, practical examples that you will be able to replicate for your needs, ranging from building a vector-powered search application to applying vectors to domains such as observability and security. It targets a large spectrum of practitioners but is more focused on the operations teams, which deal with specific challenges and could make good use of combining vectors and Elastic.

In this chapter, you learned about the key aspects of NLP, vector search, AI-based search, and its relative techniques. You were also shown pieces of code that you can run and adapt for your business applications. Lastly, you gained a high-level understanding of Elasticsearch’s role in this context as well as how to apply vector search beyond the search use case, such as in terms of observability.

In the next chapter, we will get started by looking at vectors in Elasticsearch. We will explore the current search methodology and see how vectors are augmenting and enhancing it. Furthermore, we will dive deep into the technicality of vector implementation in Elasticsearch.

## Getting Started with Vector Search in Elastic

Welcome to *Getting Started with Vector Search in Elastic*. In this chapter, we will understand the fundamental paradigm of search with Elastic and how vector search has emerged as a powerful tool for real-time, context-aware, accurate information retrieval.

In this chapter, we are going to cover the following topics:

- Search experience in Elastic before the addition of vector search
- The need for new representations such as vectors and **Hierarchical Navigable Small World (HNSW)**
- A new vector data type
- Different strategies to configure the mapping and challenges of storing vectors, getting a better perspective on how to optimize its implementation
- How to build queries including brute force, **k-nearest neighbors (kNN)**, and exact match as a resource

Whether you are a seasoned Elastic user or just getting started, this chapter will provide valuable insights into the power of vector search in Elastic. Let’s begin!

## Search experience in Elastic before vectors

Before the introduction of vector search in Elastic, the primary relevancy model was based on text search and analysis capabilities. Elasticsearch provides various data types (https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html) and analyzers (https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-analyzers.html) to provide efficient search. In this part, we will level-set and make sure we all have an understanding of what the “before state” looks like.

## Data type and its impact on relevancy

There are various data types in Elasticsearch, but for the purpose of this part, it wouldn’t be useful to go through them one by one. Instead, we will divide them into two categories: the type that directly drives the relevancy ranking and the types that indirectly influence the ranking. The goal is to understand how they are related to relevancy models.

The first type that we will look at is one that directly impacts relevancy ranking:

- **Text**: The `text` data type is the most crucial data type for the relevancy model in Elasticsearch. It is used to store and search textual data such as articles and product descriptions. Text data is analyzed using built-in analyzers, which break the text into tokens and perform operations such as lowercasing, stemming, and filtering.
- **Geo**: The `geo` data type is used to store and search geographic coordinates. It enables you to perform geo-based queries, such as finding documents within a specific distance or bounding box. Although not part of the text-based relevancy model, geo-based queries can help to narrow down search results and improve their relevance.

The second type of data types are ones that improve relevancy:

- **Keyword**: The `keyword` data type is used to store non-analyzed text data, typically used for filtering and aggregations. It helps to refine the search results by applying filters or aggregations, thereby improving the relevance of the results.
- **Numeric types (integer, float, double, etc.)**: These data types are used to store and search numeric data. Although they don’t directly impact the text-based relevancy model, they can be used to filter or sort search results, which can indirectly affect the relevancy of the results.
- **Date**: The `date` data type is used to store and search date and time data. Similar to numeric types, date data types can be used to filter and sort search results, impacting the overall relevancy indirectly.
- **Boolean**: The `boolean` data type is used to store `true/false` values. While it doesn’t contribute directly to the relevancy model, it can be used to filter search results, improving their relevance.

## The relevancy model

While this book is not an Elasticsearch cookbook and focuses on vector search, it is important to understand relevancy ranking models before diving into vectors, so that we can use them wherever required. We will see that building a hybrid search by combining vector search and “traditional” search is a technique that will improve the end user experience.

Elasticsearch itself went through iteration when it comes to the relevancy model, first with **term frequency-inverse document frequency (TF-IDF)** and now with BM25. Both are text retrieval algorithms used to rank documents based on their relevance to a query. However, they have key differences, which we will explore now.

## *TF-IDF*

To illustrate the TF-IDF concept, we will use a simple example with a small collection of three documents:

- **Document 1:** “I love vector search. Vector search is amazing.”
- **Document 2:** “Vector search is a method for searching high-dimensional data.”
- **Document 3:** “Elasticsearch is a powerful search engine that supports vector search.”

We want to calculate the TF-IDF score for the term `vector search` in each document.

First, we calculate the **term frequency (TF)** for each document; note that we apply TF on a **bi-gram** (a sequence of two adjacent words in a text – i.e., `vector` and `search` together) here:

Document 1: `vector search` appears twice out of 8 words: TF = 2 / 8 = **0.25**

Document 2: `vector search` appears once out of 9 words: TF = 1 / 9 = **0.111**

Document 3: `vector search` appears once out of 10 words: TF = 1 / 10 = **0.110**

Then, we calculate the **inverse document frequency (IDF)** for the term `vector search`.

We know that the number of documents containing the term `vector search` = 3.

And the total number of documents = 3.

When these are used in the formula, we get:

*IDF = log(3/3) = log(1) = 0*

Finally, we calculate the TF-IDF score for the term `vector search` in each document:

- **Document 1:** TF-IDF = TF * IDF = 0.25 * 0 = 0
- **Document 2:** TF-IDF = TF * IDF = 0.111 * 0 = 0
- **Document 3:** TF-IDF = TF * IDF = 0.110 * 0 = 0

In this specific example, the IDF value is 0 because the term `vector search` appears in all documents, making it a common term across the entire document collection. As a result, the TF-IDF scores for all documents are also 0. This illustrates how the IDF component in the TF-IDF algorithm penalizes common terms in the document collection, reducing their impact on the relevance scores.
