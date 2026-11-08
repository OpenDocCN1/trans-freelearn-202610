# Using Apollo with TypeORM

After we've created the entities and inserted some test data, we'll look at how to integrate Apollo with TypeORM. In this section, we'll define our GraphQL resolvers, which are functions that will be called when we send queries and mutations to our GraphQL endpoint:

+   1. First, you need to change back the user and author fields on the Post, Comment, and Like types of our GraphQL schema, in the src/graphql/schema.graphql file, to the type of User instead of ID:
+   2. Additionally, change the type of the post field from ID to Post in the Comment and Like types:

```
author: User!
user: User!
```

```
post: Post!
```

Before implementing our schema resolvers, we need to create the types for ensuring the type safety of our resolvers' code. Fortunately, we don't have to do this manually. Instead, we can use GraphQL Code Generator, which is a tool for generating code from your GraphQL schema and operations with a simple CLI. For additional information about this tool, please refer to https://www.graphql-code-generator.com/.

Since those types are generated code, we'll place them in their own package and then import them from our resolvers.

Following that, we'll look at how to use Lerna to create a package and configure it to be able to import the generated types:

+   1. If your server is still running, stop it and execute the following commands to create a graphql-types package:

```
cd ../../
lerna create graphql-types
```

Note that you'll be asked some questions regarding your package. Accept the default values and press Enter.

A new graphql-types package will be created in ./packages/graphql-types.