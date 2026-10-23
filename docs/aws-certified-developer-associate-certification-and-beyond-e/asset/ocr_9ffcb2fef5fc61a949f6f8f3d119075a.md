+   3. Next, you will be prompted to confirm the delete process by typing `permanently delete` in the textbox in the **Permanently delete objects?** section.
+   4. Next, click the **Delete objects** button.
+   5. Once the objects have been deleted, navigate back to the Terminal/Command Prompt window where you ran the `sam deploy` command. Ensure you are in the same folder.
+   6. Run the `sam delete --no-prompts` command.
+   7. SAM will now perform a stack delete operation. This will take a few minutes to complete.
+   8. If you navigate back to CloudFront and Amazon S3, you should notice that the resources have been deleted.

# Summary

In this chapter, you learned about AWS SAM, an open source framework that simplifies the development, testing, and deployment of serverless applications on AWS. SAM uses AWS CloudFormation to help build underlying infrastructure that supports your serverless application. It provides a syntax for expressing functions, APIs, and databases, allowing you to define and manage your IaC.

You also discovered the integration of AWS AppSync with Amazon DynamoDB, showcasing the powerful combination these services offer for building scalable, serverless, and real-time applications. By defining the GraphQL schema in AWS AppSync and configuring resolvers that map GraphQL operations directly to data operations in DynamoDB, you learned about the seamless interaction for efficient data synchronization and management without the need for intermediate layers, making it a highly effective solution for developing modern applications.

With AppSync, you can run queries and mutations to interact with DynamoDB, utilizing direct data mappings and AWS Lambda for more complex logic. Using real-time capabilities facilitated by GraphQL subscriptions, you discovered how AppSync can deliver live updates to client applications as data changes in the database.

This integration simplifies the backend infrastructure and accelerates development processes, allowing you to focus more on creating compelling user experiences rather than managing database operations and data flow. Whether you are building mobile apps, web applications, or complex systems with multiple data sources, the combination of AppSync and DynamoDB offers a scalable and efficient solution to meet modern application demands.