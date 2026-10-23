# Weaviate

Weaviate is an open source, GraphQL-based vector search engine that enables users to search for data using natural language queries. One of Weaviate’s strengths is its focus on semantic search, making it well-suited for applications where understanding the meaning of a query is important. It also supports schema-based data modeling, allowing users to define the structure of their data and the relationships between different entities. However, Weaviate’s focus on semantic search (vector or hybrid search) and its reliance on GraphQL may introduce complexity for some users, particularly those who are not familiar with these concepts.

# Elasticsearch

Elasticsearch v8+ from Elastic.co supports dense vector storage for ANN and exact match search, providing a scalable approach to vector search on large datasets. Unlike exact match nearest neighbor search, ANN trades perfect accuracy for speed, making it more efficient with high-dimensional vectors. Elasticsearch employs the Hierarchical Navigable Small World (HNSW) graphs ANN algorithm, boasting strong performance and widespread industry use. Built on Apache Lucene, this feature integrates seamlessly with other Elasticsearch functions, including filtering and hybrid search with BM25. However, Elasticsearch’s complexity may pose setup and configuration challenges for some users. Lucene’s dense_vector data type limitations also apply to Elasticsearch, and while it can handle massive vector datasets (in the billions), the memory requirements for ANN at such scales may be impractical.

Each of these vector databases has its unique strengths and weaknesses, depending on the specific use case and requirements. Pinecone excels in ease of use and maintenance, Vespa offers versatility with support for multiple search modalities, Milvus is designed for high performance and scalability, and Weaviate focuses on semantic search with natural language capabilities. Finally, Elasticsearch provides support for dense vectors and ANN search with seamless integration of its search features.

When selecting a vector database, developers should carefully consider their specific needs and evaluate each solution based on its features, performance, and ease of use.

# Use cases and domains of application

AI-based search is often overwhelming for a majority of users to get started with, mainly, we believe, because of the lack of standardization – there are a lot of options to get started. In this book, we believe that it would be useful for practitioners to understand where to start by targeting the more mature and adopted techniques, use cases, and domains of application.

We will directly address the scope of possibilities for search, navigating around the jargon carried by the complex space of ML, NLP, and deep learning.