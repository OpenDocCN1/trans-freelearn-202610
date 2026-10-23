# Step 2: Connecting Data Sources

Once your schema is defined, you can connect it to one or more data sources. AppSync supports a variety of data sources, including AWS Lambda, Amazon DynamoDB, Amazon Elasticsearch, and HTTP APIs.

# Step 3: Configuring Resolvers

You configure resolvers for each field in your schema that dictate how the data is fetched or written. These resolvers can be connected directly to your data sources or execute the business logic you define in AWS Lambda functions.

# Step 4: Deploying and Querying

After your GraphQL API is configured, AppSync automatically deploys it. Your client applications can then query it using standard GraphQL operations.

In the next section, you will learn more about the use cases for AWS AppSync.

# Integrating AWS AppSync with Amazon DynamoDB

AWS AppSync and Amazon DynamoDB are powerful for building scalable, serverless, real-time applications. AWS AppSync is a managed GraphQL service that simplifies application development by enabling developers to create a flexible API to securely access, manipulate, and combine data from one or more data sources. When paired with Amazon DynamoDB, a fast and flexible NoSQL database service, it allows for seamless data synchronization and management with minimal setup.

The next section provides an overview of how data management and flow are configured for AppSync.

# Data Management and Flow

+   - **Schema definition:** In AWS AppSync, you define a GraphQL schema that specifies how clients interact with the data. The schema includes types, queries, mutations, and subscriptions.
+   - **Resolver configuration:** For each operation defined in the schema (query, mutation, and subscription), you configure resolvers. A resolver determines how a GraphQL query or mutation translates to a data operation in DynamoDB.
+   - **Direct data source integration:** AppSync allows you to link GraphQL fields directly to DynamoDB items using **Velocity Template Language (VTL)** templates or AWS Lambda functions. This direct integration abstracts database operations and lets you perform CRUD operations directly on DynamoDB tables without middleware.

Next, you will learn more about queries and mutations.