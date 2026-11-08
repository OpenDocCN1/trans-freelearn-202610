
Next, add a user field to the Like type, as follows:

```
user: casual.random_element(usersIds),
```

Next, change the post fields of the Comment and Like types, as follows:

```
post: casual.random_element(postsIds)
```

We've added all these modifications to make sure the fields that point to users and posts are getting values from the usersIds and postsIds arrays.

Finally, you need to change the user and author fields on the Post, Comment and Like types in our src/graphql/schema.graphql file to the ID type instead of the User type:

```
author: ID!
user: ID!
```

This is only temporary for the sake of mocking realistic data relationships.

Now, head back to Apollo Studio and run the following query:

```
query {
  searchUsers { id }
  getPostsByUserId(userId:"93c9c019"){ id author }
  getCommentsByPostId(postId:"93c9c019"){ id author post }
  getLikesByPostId(postId:"93c9c019"){ id user post }
}
```

Make sure that you pass the right values for userId and postId as they will be different in your case since they are randomly generated values.

You should get linked results – that is, the user and post fields will contain values that are equal to the values that were generated for the posts' identifiers.

Now that we have realistic and linked data to test our API, we'll learn how to configure CORS. This will help us connect to our GraphQL server from the Angular frontend without getting blocked by the browser.

## Configuring CORS

Cross-origin resource sharing (CORS) refers to a mechanism by which we can allow or disallow other domains from requesting restricted resources on a web page that are being served from a specific domain.

We have already set up a server on port 8080 of our localhost machine and we'll be also running a development server of the Angular CLI from another port in the next few chapters. Different ports on localhost are considered different origins, so we need to configure CORS in our Express.js server; otherwise, the requests from our Angular frontend will be disallowed.

Fortunately for us, configuring CORS in Express.js is easy. Head to your Terminal and run the following command from the root of the server folder to install the cors module:

```
npm install cors
```

Now, you need to open the src/index.ts file and import the cors module:

```
import cors from 'cors';
```

Next, add the following line just below where you created an instance of the Express.js application:

```
app.use(cors());
```

This is the basic configuration of CORS in our Express.js server, which allows any other domains to request data from your server without restriction.

You can refer to the docs of the cors module at https://expressjs.com/en/resources/middleware/cors.html for more options for configuring CORS while protecting your server resources from non-authorized access.

## Summary

In this chapter, we started implementing our backend application, which will be exposing the GraphQL API. We created a project using Lerna to follow the monorepo approach to organizing our code base, which will contain both the server and client apps.

We installed Express.js and configured it with TypeScript and also GraphQL via Apollo Server.

Next, we created an initial GraphQL schema for our small social network application and configured Apollo Server with mocking. Then, we used the casual tool to use realistic values for our mock data.

Thanks to mocking, we were able to run a working GraphQL server by providing only a schema without implementing the resolvers.

This is a great feature that allows us to easily test the GraphQL API without running a complete API server. It also allows the frontend developer/team to start working on the features that require data to be fetched from the server as quickly as possible without waiting for the backend development to finish.

Next, we used Apollo Studio to send queries to our Apollo Server, which was able to return data that contains the shapes of our schema types.

Finally, we installed the `cors` module and configured CORS in our Express/Apollo Server to enable our frontend application, which will be running from a different port at localhost during development, to send requests without getting blocked due to the same-origin policy in web browsers.

In the next chapter, we'll continue our journey of building our backend application to expose a fully working GraphQL API with resolvers that fetches data from a real MySQL database.

## 3 Connecting the Database with TypeORM

In the last chapter, we learned how to set up a Node.js server with GraphQL support to implement a backend application. Additionally, we explained how to install Express.js and configure it with TypeScript and GraphQL. We used mocking to provide a working GraphQL server with Apollo Server before implementing the resolvers that should actually be responsible for fetching and adding data.

In addition, we set up Cross-Origin Resource Sharing (CORS) and used Apollo Studio to communicate with our GraphQL server.

In this chapter, we'll look at how to connect a MySQL database to our application using TypeORM, as well as how to create resolvers to get and save data from the database.

Now that we've built a backend application that can respond to GraphQL queries, we'll need to connect it to our database in order to store our data. We'll utilize TypeORM to abstract the database operations, allowing you to use any desired database management system for your app without changing the code. In addition, we will learn how to connect TypeORM with Apollo, as well as how to generate and seed our database tables.

We will discuss the following topics in this chapter:

- Creating a MySQL user and database
- Setting up TypeORM and MySQL in our server project
- Creating TypeORM entities
- Seeding test data in our database
- Using Apollo together with TypeORM

## Technical requirements

Before you can proceed with the steps in this chapter, you must first finish the steps in the previous chapter on how to set up a functional GraphQL server. Please refer to Chapter 2, Setting Up GraphQL with Node.js, Express.js, and Apollo, to learn how to build up a GraphQL server using Express.js and Apollo Server.

It is important to note that your work machine should have MySQL installed. Please refer to Chapter 1, App Architecture and Development Environment, for instructions.

You should also be familiar with the following technologies:

- TypeScript
- Node.js and npm
- SQL (including basic concepts such as tables, columns and their types, relationships, and primary and foreign keys)
- GraphQL basics (including resolvers, queries, and mutations)

If you want to learn more about SQL, please see https://www.khanacademy.org/computing/computer-programming/sql.

You should also go through TypeORM's official documentation, which is accessible at https://typeorm.io/#/

If you need a review on the fundamentals of GraphQL, please see https://graphql.org/learn/.

The code for this chapter may be found in the GitHub repository at https://github.com/PacktPublishing/Full-Stack-App-Development-with-Angular-and-GraphQL/tree/main/Chapter03.

## Creating a MySQL user and database

When we first installed MySQL on our development machine, it created a root user who can manage the database system. This user has complete access to the MySQL server, including all databases and tables.

Furthermore, beginning with MySQL v5.7, the root user can be authenticated via the auth socket plugin rather than a username and password, as allowed by the MySQL client used with TypeORM.

> **Important Note**
> 
> The name of the system user that launches the MySQL client and the MySQL user must be the same when using the auth socket plugin; moreover, you must execute mysql with sudo privileges to have access to your database(s).

For the reasons stated above, it is required to create a new user in MySQL and provide it with the right permissions. To do so, go through the following steps:

1. To begin, use the following command to connect to MySQL:
2. Once you've accessed the MySQL prompt, you may use SQL commands such as CREATE USER to create a new user with a password that utilizes the mysql native password plugin for authentication:
3. We must provide it with the necessary permissions using the following command:
4. Then, use the following command to reload all of the privileges:

```
sudo mysql
```

You'll be asked to enter a password. Enter your system's root password.

```
CREATE USER 'dbuser'@'localhost' IDENTIFIED WITH mysql_native_password BY 'p4ssw0rd';
```

In this case, we created a user called dbuser with the password p4ssw0rd.

```
GRANT ALL PRIVILEGES ON *.* TO dbuser@'localhost';
```

We have given the user all of the privileges (that is, full root access to everything in our database). This is not advised for security reasons. However, keep in mind that this is simply a test database.

```
FLUSH PRIVILEGES;
```
