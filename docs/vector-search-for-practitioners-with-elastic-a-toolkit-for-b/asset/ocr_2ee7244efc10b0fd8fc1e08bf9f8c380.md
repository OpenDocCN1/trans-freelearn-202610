is that the vector captures the semantic meaning and the context as well, which ultimately serves the end user experience.

If you’re curious, in the situation where an e-commerce customer would search for “wireless headphones,” the organization would represent user queries as vectors in a high-dimensional space and use a pre-trained model such as *word2vec* to convert each word in the query into a vector representation. The combined result would represent the entire query. This enables us to compare the vector to the vector space built out of the product representation. The following is a 10-dimensional vector that gives us an idea of the representation:

```
[0.213, -0.178, 0.425, -0.067, 0.321, 0.195, -0.267, 0.132, -0.455, -0.033]
```

What’s interesting here is the fact that the query is suddenly converted into a space of possibility, which allows us to compare vector space, instead of keywords, to determine similarities. Does the product exist? What type of product is being searched for? The semantics that are extracted from a query are fascinating.

# *Applying ML*

Another level of AI-based search experience is training the data with models such as **convolutional neural networks (CNNs)**, **recurrent neural networks (RNNs)**, or **transformers**. This is done on the previously vectorized data to learn and understand patterns and relationships between data. The goal of this phase is to prepare the search application so that it can support NLP.

To continue our example of wireless headphones, using models such as CNNs or RNNs allows us to browse the graph of connections this specific article could have. In this case, think about features such as noise cancellation, battery life, whether they’re waterproof, or whether they stay in the wearer’s ears during their burpee session.

# *Integrating personalization*

Without personalization, the search, whether smarter than a keyword search or not, would stay cold. Personalization allows you to integrate context into the search based on past searches and similar searches. These are known as network profiling and user profiling, respectively.

The beauty of CNNs/RNNs/transformers is that the benefits don’t stop at what we mentioned previously – they help provide recommendations to users based on their past searches.

# *Getting into production*

We would love to tell you that AI-based search is solving the back and forth that a search application requires from a continuous optimization perspective between relevancy and latency performance, but that’s not the case.