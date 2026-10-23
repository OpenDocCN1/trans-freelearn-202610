# Performance Tuning – Working with Data

In the previous chapter, we covered how to integrate Elasticsearch with **machine learning (ML)** models. In this chapter, we’ll focus on optimizing vector search performance in Elasticsearch.

The topics we will cover include the following:

+   - ML model deployment tuning techniques
+   - Estimating vector capacity for an Elasticsearch node
+   - Load testing with Elastic’s performance tool, Rally
+   - Troubleshooting slow **k-nearest neighbor (kNN)** search response times

By the end of this chapter, you will understand how to estimate the number of Elasticsearch nodes required for your use case, how to use performance benchmarking to ensure those estimates are in line with your requirements, and identify potential causes for slow kNN search response times.

Let’s begin by deploying a **natural language processing (NLP)** model.

# Deploying an NLP model

Deploying an NLP model requires selecting a deployment method that can handle the expected inference load during peak production while being efficient and secure. During the testing phase, the model can often be run locally if it is small enough. However, a production use case necessitates a production deployment strategy. The model can be deployed on virtual machines, through a cloud provider service, on a dedicated model hosting service such as Hugging Face, or, when possible, within the same service where the vectors will be used, such as Elasticsearch. Each method has its trade-offs between efficiency, latency, scaling, and management.