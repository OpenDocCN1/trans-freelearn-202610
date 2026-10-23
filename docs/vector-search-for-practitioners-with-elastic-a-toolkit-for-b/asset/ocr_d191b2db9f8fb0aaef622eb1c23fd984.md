![](bbox=[0.17, 0.12, 0.82, 0.47])

Figure 10.1: Significance of Context measure as a function of relevancy with C, T, and A held constant

Elasticsearch comes with the significant advantage that it enables the users to fine-tune the relevancy. This plot shows that despite there being three factors in addition to relevancy that make up the SoC, the more controls on relevancy the data store gives, the more the significance of the dynamic context will increase.

# Dynamic Context Layer plugin vision—architecture and flow

In this section, we get to the heart of our project: the design and flow of the DCL plugin. We’ll lay out its structure, explaining how ChatGPT, Embedchain, and Elasticsearch work together. By understanding the underlying architecture and the steps of data flow, you’ll see how to integrate real-time data into a chatbot’s responses. We’ll discuss why certain design choices were made and their impact on the system’s functionality.

In any advanced system, clarity of structure and function is crucial. The DCL is no exception. To navigate through the mechanics of how ChatGPT interacts with Elasticsearch via Embedchain, we must first familiarize ourselves with the foundational components enabling this integration. Each component serves a unique purpose, collectively enabling our chatbot to dynamically retrieve, comprehend, and relay current information.