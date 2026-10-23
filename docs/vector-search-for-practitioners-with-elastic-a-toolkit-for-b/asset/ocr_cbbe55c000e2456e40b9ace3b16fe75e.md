Now, with this understanding of the evolutionary context, let’s delve into the specifics of how RAG, ELSER, and RRF can work together to further advance our search capabilities.

# Semantic search with ELSER

As we previously discussed, semantic search has transformed the way we understand and respond to user queries. ELSER is a valuable tool in this sphere, offering a unique way to represent and understand the content and context of user queries in a more in-depth manner.

We previously looked at how ELSER can onboard large content into a high-dimensional token space, creating a semantic map of sorts, to yield more contextually accurate results. However, how does ELSER come into play when we bring in elements such as RAG and RRF? Let’s find out.

# A recap of essential considerations for RAG

To successfully implement RAG in any search system, there are three key components we must consider: the document store, the retriever, and the generator. These elements interact to form the backbone of an effective RAG-enhanced search system.

# Document store

Elasticsearch serves as the document store in our search system, providing the platform where all searchable data resides. This data must be both comprehensive and diverse to accommodate the broad spectrum of information that it may receive from queries. Elasticsearch is known for its ability to handle diverse data types swiftly and efficiently, making it an ideal choice for this role.

Elasticsearch doesn’t only act as a passive repository but also plays an active role in delivering a successful search experience. With its powerful full-text search capabilities, efficient indexing, and scalability to handle large volumes of data, Elasticsearch forms a robust foundation to deploy a successful RAG-enhanced search system.

# Retriever

The retriever, in this context, operates between the user’s query and Elasticsearch, our document store. Here, ELSER, a pre-trained model, plays a pivotal role.

ELSER functions by expanding tokens from the document corpus indexed in Elasticsearch, enhancing its ability to understand and locate semantically relevant information based on user queries. It works hand-in-hand with RRF, a technique that blends lexical search and ELSER-based search.

RRF achieves this by creating sub-queries that assist in combining the precision of lexical search’s exact term matching with the enriched semantic understanding offered by ELSER. The integration of these two techniques allows for a more comprehensive and effective retrieval process, significantly boosting the relevance and precision of the search results in our RAG system.