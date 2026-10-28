# Dynamic Social Networks

In previous chapters, we’ve considered networks that don’t change over time. Their geometry remains unchanged, and spreading processes and importance metrics calculated on them don’t vary, either. However, this is not the case for many real-world networks, where new connections are forged and old connections break.

In this chapter, we’ll consider networks that change over time and how these changes impact network structure and spreading processes. For instance, within a social network, users can join or deactivate accounts. The strength of their ties can change as they interact with other users’ content. All these actions drive network changes over time, which impact how processes such as information exchange across a network function as hubs appear and disappear or bridges forge across different hubs. We’ll consider simulation data and trial data collected from wildlife observations and hypothetical epidemics spreading across populations of crocodiles and blue herons, respectively.

By the end of this chapter, you’ll understand how networks change over time and what these impacts mean for spreading processes and network metrics. You’ll also know how to wrangle dynamic datasets into networks and analyze time slices. These methods scale to larger populations and longer time frames, so you’ll be equipped to work with population models at scale by the end of this chapter.

In this chapter, we will cover the following topics:

+   - Social networks that change over time
+   - A deeper dive into spreading on networks
+   - Example with evolving wildlife interaction datasets

# Technical requirements

In this chapter, we will be using a Jupyter notebook to run our dynamic network examples. You can also run this code on a cloud platform that supports Jupyter notebooks, such as Amazon Web Services (AWS) or Google Cloud Platform (GCP). We will be using a Jupyter notebook run on a laptop computer, and it is not necessary to have cloud access to process the examples in this chapter.