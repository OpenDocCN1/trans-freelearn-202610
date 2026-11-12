# # 11
# Asynchronous Processing
# and Big Data Volumes

This chapter covers two important aspects of developing an enterprise-level application on the Force.com platform. While asynchronous processing and Big Data volumes are not necessarily linked, the greater governor limits and processing power of the platform are mostly to be found in the async processing mode. However, making sure that your interactive user experience is still responsive when querying large volumes of data is also very important.

In this chapter, we will first review how to ensure SOQL queries are as performant as possible by understanding how to profile queries and make use of the standard and custom indexes. This will benefit both interactive and batch (or async) processes in your application as well as how they leverage native platform features such as reporting.

We will then take a look at the options, best practices, design, and usability considerations to move your application processing into the asyc mode, once again leveraging the **Service** layer as the shared backbone of your application, before finally understanding some thoughts and considerations when it comes to volume testing your application. We will cover the following topics in this chapter:

+   - Creating test data for volume testing
+   - Indexes, being selective, and query optimization
+   - Asynchronous execution contexts
+   - Volume testing