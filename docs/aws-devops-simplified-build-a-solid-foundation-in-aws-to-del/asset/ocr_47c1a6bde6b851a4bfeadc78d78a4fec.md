Having worked with several software teams in my career, a common approach I’ve noticed is that observability and monitoring are always seen just in the light of technical metrics. Sometimes, software professionals are a bit disconnected from understanding the business impact their code has, and how exactly the end users consume the applications they develop daily. Observability, from my perspective, is all about *empathizing with the customer* and feeling what they feel, seeing what they see, and measuring what matters to them the most. It is only then that we can improve their experience, every single time they use our application. Of course, this in a way depends on the underlying technical metrics, but the approach should always be to work backward from the business goals that drive customer experience, and then derive relevant metrics that influence those goals. Building a technical strategy that is disconnected from your business requirements will result in an ineffective solution.

As a concrete example of an e-commerce platform, customers are least concerned about rising CPU and memory usage on the backend servers. However, if they are unable to complete an order in a reasonable time, they are going to be unhappy about it. So, it might be valuable to focus on the *order completion time* as a metric and work back from there to figure out if an increase in CPU utilization causes it.

In this chapter, we will cover the following topics to learn a few things about enabling the observability of your workloads running in AWS:

+   - What is observability?
+   - Key AWS offerings for monitoring and observability
+   - Best practices for building a solid observability strategy
+   - How to go about defining your observability strategy for workloads hosted in AWS
+   - Deploying a basic observability stack for our test application hosted in ECS

Let’s begin by understanding the core idea behind observability.

# What is observability?

Simply put, it’s all about understanding the current state a running system is in, from the work it is doing and the data that it is emitting. Developing a solid observability strategy is not a one-time thing and it will always have scope for optimizations as your business needs evolve. But, before you can even understand what is going on, it’s important to ensure that the system at least emits some data for us to be able to derive some reasoning out of it. But what kind of data?

Observability has three foundational pillars that allow you to convert data into information, and derive insights from that information, which ultimately leads to the actions that need to be taken:

+   - **Logs:** These are the *discrete events* that have occurred across several components in your systems while serving a customer request. It’s invaluable to store these logs in a centralized data store so that information can be securely extracted, and analyzed, as and when needed.