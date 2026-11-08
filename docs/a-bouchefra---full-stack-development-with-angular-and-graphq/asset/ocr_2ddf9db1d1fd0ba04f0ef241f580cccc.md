So we've built the entities required to establish our social application database, as well as the associations between them. Next, we'll go over how to populate the database tables with test data, which will allow us to easily test the functionality of our resolvers in the next sections.

# Seeding test data

So far, we've built database entities that let TypeORM construct SQL tables. Let's now enter some information into the database. You could do this manually, but it would be exhausting; we need to automate it sufficiently so that we can have it up and running quickly.

We can accomplish this by utilizing the typeorm-seeding package to create factories and seeders for our entities, which simplifies the process. Perform the following steps:

+   1. Return to your Terminal and begin by installing the package with the following command:
+   2. Install the type definitions of the Faker library and create the src/database/, database/seeds/, and database/factories/ folders:
+   3. Add the following scripts to the package.json file to invoke the seed commands:
+   4. Add the following commands to the package.json file to make it easier for you to drop and sync the database:

```
npm install typeorm-seeding
```

```
npm install -D @types/faker
```

```
cd src/ && mkdir database
```

```
cd database && mkdir seeds factories
```

```
"scripts": {
  "seed:config": "ts-node ./node_modules/typeorm-seeding/dist/cli.js config",
  "seed:run": "ts-node ./node_modules/typeorm-seeding/dist/cli.js seed",
  [...] 
}
```

```
"scripts": {
  "schema:drop": "ts-node
```