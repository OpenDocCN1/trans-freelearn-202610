Then, we learned how to connect a MySQL database to our web application using TypeORM and implemented the resolvers for communicating with the database. We exposed a working GraphQL API with resolvers that query and remove data from a real MySQL database.

We also utilized TypeORM to abstract the database operations, allowing developers to use any desired database management system for their application without changing the code. We learned how to connect TypeORM to Apollo, as well as how to generate and seed database tables.

Next, we added authentication and image uploads with Apollo Server to our GraphQL API and learned about the necessary concepts for adding authentication with Node.js, Express, and Apollo Server, as well as for handling image uploads.

We also added realtime support with authentication to our server application, which enabled us to communicate fresh data from the server to the client as soon as it becomes available. To do this, we leveraged Apollo Server's GraphQL subscriptions to push new comments and likes on users' posts from the server to the client, right at the moment when they are added.

After implementing the backend, we used the Angular CLI to create a new Angular project and reviewed many fundamental Angular concepts, such as modules, components, and services. We also learned how to use the Angular CLI to generate the artifacts that constitute the structure of the application.

Then, we utilized Angular Material in the project to provide Material design components to create a visually appealing application UI.

Finally, we integrated the frontend that we built using Angular with the backend using Apollo Client, which is designed to send GraphQL queries and mutations to the server to fetch and write data.

After developing and testing the application and adding any extra features that you want, you can build the production bundles and host them with your preferred hosting service. Please refer to the official documentation at `https://angular.io/guide/deployment` for more information about this process.

Thanks for taking the time to read this book and I hope you enjoyed it.