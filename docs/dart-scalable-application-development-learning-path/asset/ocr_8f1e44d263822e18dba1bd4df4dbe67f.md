# Working with Databases

Insert, update, and delete queries (coded in the methods with the same name) have to first go through the prepare stage and then the execute stage. Let's see a few examples. For insert, this query becomes as follows (refer to the sixth comment):

```
pool.prepare('insert into jobs (dbKey, type, salary, company,\n  posted, open) values (?, ?, ?, ?, ?, ?)').then((query) {})
```

An insert query on a table that contains an autoincrement field will return the value of that field in ${result.insertId}. For update, this query becomes as follows (refer to the seventh comment):

```
pool.prepare('update jobs set type = ?, salary = ?, company = ?,\n  posted = ?, open = ? where dbKey = ?').then((query) {})
```

The query for delete is as follows (refer to comment 8):

```
pool.prepare('delete from jobs where dbKey = ?').then((query) {})
```

The ? character represents values to be substituted in the SQL string. These values are placed in the specified order in the params list, which is given as an argument to query.execute. A query with multiple parameter sets can be executed with query.executeMulti(). If you need a number of queries to be executed as a whole (or all or none succeed), use the Transaction class from sqljockey in the following format:

```
pool.startTransaction().then((trans) {\n  trans.query('...').then((result) {\n    trans.commit().then(() {...});\n  });\n});
```

# There's more...

The complete API documents can be found at http://jamesots.github.io/sqljockey/docs/.

# Storing data in PostgreSQL

PostgreSQL is another popular open source SQL database. Dart can talk to PostgreSQL using the pub package postgresql by Greg Lowe (https://github.com/xxgreg/postgresql). In this section, we will demonstrate how to use this driver step by step. You can see it in action in the using_postgresql project.