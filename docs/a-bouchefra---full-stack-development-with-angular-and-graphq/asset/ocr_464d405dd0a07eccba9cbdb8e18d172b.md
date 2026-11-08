# 3
Connecting the Database with TypeORM

In the last chapter, we learned how to set up a Node.js server with GraphQL support to implement a backend application. Additionally, we explained how to install Express.js and configure it with TypeScript and GraphQL. We used mocking to provide a working GraphQL server with Apollo Server before implementing the resolvers that should actually be responsible for fetching and adding data.

In addition, we set up Cross-Origin Resource Sharing (CORS) and used Apollo Studio to communicate with our GraphQL server.

In this chapter, we'll look at how to connect a MySQL database to our application using TypeORM, as well as how to create resolvers to get and save data from the database.

Now that we've built a backend application that can respond to GraphQL queries, we'll need to connect it to our database in order to store our data. We'll utilize TypeORM to abstract the database operations, allowing you to use any desired database management system for your app without changing the code. In addition, we will learn how to connect TypeORM with Apollo, as well as how to generate and seed our database tables.