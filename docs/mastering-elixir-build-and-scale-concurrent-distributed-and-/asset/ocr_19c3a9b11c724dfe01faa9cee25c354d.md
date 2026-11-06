# Schema or not – that is the question

So far, we've talked about schemas as if they were obligatory when one considers implementing the database access with Ecto. Before Ecto 2.0, this was true to some extent, given that many database queries without schemas were more verbose than their schema-powered counterparts, and some database operations were only possible if you chose the schema way of doing things.

Fortunately, those days are over, and you can now perform all operations and queries without a schema in place. As an example, imagine we have a `users` database table with `:username` and `:email` fields. You could insert two new entries on this table by calling the `Repo.insert_all/2` function and passing a list of lists, such as `[[username: "andre", email: "aa@elixir.pt"], [username: "daniel", email: "dc@elixir.pt"]]`, representing the two new rows to insert.

The same happens when you want to retrieve data from the database, and your queries happen to join a lot of tables. Does it make sense to try to fit all the results into the different schemas involved in the query? Probably not. It would be better to simply map the result to an Elixir standard data structure (say, a map), and call it a day.

The key takeaway here is that you can have more than one schema for the same database table, or you can have none. It depends on your needs, and fortunately Elixir makes it easy for you to change it afterwards. Imagine you had no schema for a given table and a query of yours was returning a list of maps. If you decide to add a `Foo` schema, with a minimal change, your query can now return a list of `Foo` structures. The existing pattern-matches for the previous list of maps will probably continue to work, given that an Elixir struct is really just a special map.

The `Storage.Media` schema that we defined will be backed by a database table. Let's see how we can create it using Ecto `migrations`.

# Migrations

As we've already seen, the purpose of our `Media` schema is to represent the data that we retrieve from the database in the Elixir world. Since we are using a relational database, data is persisted in database tables.

To create the tables, instead of executing a SQL script directly on the database, we will use Ecto migrations to apply every database schema change for us.