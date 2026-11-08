
5. Exit the current MySQL prompt:

```
exit;
```

6. Use the following command to log in as the new user:

```
mysql -u dbuser -p
```

Enter the password given to the user, in other words, p4ssw0rd.

7. Finally, to create a database, use the following command:

```
create database socialdb;
```

That's all! We have a user who goes by the name dbuser, with the p4ssw0rd password and a socialdb database. These credentials will be used to set up TypeORM in the following section.

## Setting up TypeORM and MySQL

In this section, we'll install and configure TypeORM in our server project.

TypeORM is a Node.js and TypeScript Object Relational Mapper (ORM). It lets you build and query database tables with TypeScript and high-level programming structures such as classes, decorators, and functions, rather than SQL commands.

Return to your Terminal. To start the server, make sure you're within the server's project and then perform the following commands:

```
cd ngsocial/packages/server/
npm start
```

Ascertain that you are inside the server's project. After that, execute the following commands in another Terminal:

```
cd ngsocial/packages/server/
npm install typeorm reflect-metadata mysql
```

The reflect-metadata library is essential in providing support for the TypeScript decorators required by TypeORM.

Next, open the tsconfig.json file and make the following changes:

```
{
"compilerOptions": {
"target": "es6",
"module": "commonjs",
"rootDir": "./src",
"outDir": "./dist",
"esModuleInterop": true,
"strict": true,
"experimentalDecorators": true,
"emitDecoratorMetadata": true,
"strictPropertyInitialization": false
}
}
```

We added the experimentalDecorators and emitDecoratorMetadata attributes and set them to true. We also added strictPropertyInitialization and set it to false.

Because decorators are currently an experimental feature, the experimentalDecorators attribute provides experimental support for them. The emitDecoratorMetadata attribute tells the compiler to emit the metadata necessary for decorated declarations. To produce decorator metadata, we must import the reflect-metadata package into our TypeScript code.

The strictPropertyInitialization attribute instructs the compiler whether strict property initialization checks should be enabled or disabled in classes.

Then, open the src/index.ts file and begin modifying the imports, as shown here:

```
import express, { Application } from 'express';
import { ApolloServer } from 'apollo-server-express';
import schema from './graphql/schema';
import cors from 'cors';
import 'reflect-metadata';
import { createConnection, Connection } from 'typeorm';
```

Then, edit the rest of the code as follows:

```
const connection: Promise<Connection> = createConnection();
connection.then(() => {
    startApolloServer();
}).catch(error => console.log("Database connection error: ", error));
```

```ts
async function startApolloServer() {
  const PORT = 8080;
  const app: Application = express();
  app.use(cors());
  const server: ApolloServer = new ApolloServer({ schema });
  await server.start();
  server.applyMiddleware({
    app,
    path: '/graphql'
  });
  app.listen(PORT, () => {
    console.log('Server is running at http://localhost:${PORT}');
  });
}
```

To provide support for the decorators, we imported the reflect-metadata library. Following that, we imported the createConnection function from the TypeORM package and deleted several symbols, such as context, which is no longer required in this part.

The reflect-metadata package is a utility library that enhances the TypeScript decorators' capabilities. This is essential for TypeORM to function correctly.

The mocks object was then discarded, and the line where the Apollo Server is created was changed to remove the mocks parameter. We don't need mocks in this part because we'll be creating actual resolvers.

We used the createConnection function to create a TypeORM connection and then invoked the then() method of the returned promise. Next, in the body of the promise's then() method, we invoked the code to build the Express and Apollo Server. This ensures that the server is only started after a successful connection to the MySQL database.

If a problem occurs, a database connection error message containing the error details will be printed to the console.

Because we have already launched our server in a previous step, we'll get the Database connection error: TypeORMError: No connection options were found in any orm configuration files error after saving the file.

To resolve this issue, use the following command to create the ormconfig.json file in the server/ folder:

```
touch ormconfig.json
```

Then, in the ormconfig.json file, add the following contents:

```
{
  "type": "mysql",
  "host": "localhost",
  "port": 3306,
  "username": "dbuser",
  "password": "p4ssw0rd",
  "database": "socialdb",
  "synchronize": true,
  "logging": false,
  "entities": [
    "src/entity/**/*.ts"
  ],
  "migrations": [
    "src/migration/**/*.ts"
  ],
  "subscribers": [
    "src/subscriber/**/*.ts"
  ]
}
```

This is the TypeORM configuration file, where we add the database connection configuration settings, such as the database type (in our instance, mysql), the database host and port, the database name, the username, and the password.

After that, if everything is properly configured, stop and restart the development server using the following command:

```
npm start
```

So, we've now configured TypeORM. Following that, we'll go over how to create the database entities for our social application.

## Creating TypeORM entities

In this part, we'll create entities that represent our social network database. An entity is a TypeScript class that has been annotated with the @Entity() decorator that corresponds to a database table.

When working with a database using plain SQL rather than an ORM, you begin by creating database tables. With TypeORM, we must also begin by creating entities.

Add the @Entity() decorator to the class definition and the @Column (or equivalent) decorator to each property in the class to create an entity. These annotations provide TypeORM with the information it needs to create the database table and columns.

We can also use the following decorators to establish associations between the entities:

- OneToOne: This is used to create a one-to-one relationship between two objects.
- JoinColumn: This specifies which part of the relationship contains the join column.
- OneToMany: This specifies a one-to-many relationship between objects.
- ManyToOne: This specifies a many-to-one relationship between objects.
- ManyToMany: This is used to establish a many-to-many relationship among objects.
- JoinTable: This specifies the join columns of a many-to-many relationship.

Create a subdirectory named entity/ in the src/ folder by running the following command:

```
mkdir src/entity
```

We can simply think about our database structure based on the schema structure because we already defined our GraphQL schema types based on our app requirements.

As a result, the following entities will be necessary in our case:

- User
- Post
- Comment
- Like
- Notification

To define an entity, we must first create a TypeScript file under the entity/ folder. A basic entity is made up of one or more columns, one of which should be the primary column.

Begin by creating an src/entity/User.ts file and then adding the following entity:

```
import { Entity, Column, PrimaryGeneratedColumn, CreateDateColumn } from 'typeorm';

@Entity()
export class User {
    @PrimaryGeneratedColumn() id: number;
    @Column() fullName: string;
    @Column("text", { nullable: true }) bio: string;
    @Column({ unique: true }) email: string;
    @Column({ unique: true }) username: string;
    @Column() password: string;
    @Column({ nullable: true }) image: string;
    @Column({ nullable: true }) coverImage: string;
    @Column({ default: 0 }) postsCount: number;
    @CreateDateColumn() createdAt: Date;
}
```

As previously stated, @Entity() instructs TypeORM to match the class to a database table. To make it easier for TypeORM to discover the entity, we place it in the entity/ folder. This was previously configured in the ormconfig.json file's entities array.

To produce the matching database's table columns, we just use the @Column decorator to decorate the class property that you wish to make into a column.

TypeORM will map a string to a varchar-like type (depending on the database type) and a number to an integer-like type by default. We may also modify the default type by supplying the required type to the column decorator. It's worth noting that we utilized the text type for the bio column rather than the limited varchar type.

TypeORM will build database tables with the same names as the entities; however, if you want to use a different table name, you may give it in the @Entity decorator — for example, @Entity("mytablename").
