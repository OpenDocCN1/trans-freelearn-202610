By using the `InMemoryUserDetailsService` in an alternative application context, it is easy to create a whole host of user accounts that have all the permutations of roles, privileges, good and bad passwords needed to checkout the services. CherryPy is especially easy to test, because it doesn't require running the web container. Since CherryPy conveniently maps URLs to methods, it is easy enough to call the methods directly from a test harness rather than verify the web handling machinery of CherryPy. And swapping the data access layer with either stubs or mocks makes it easy to isolate business logic.

All these things help to build a high confidence automated test suite. With the core of our application easily subjected to automated testing, it now becomes easy to decide whether or not to use an automated web container test kit for the web layer. It is also an option to test the 3rd party security system through automated integration testing.

By removing the need to code security APIs by hand inside our business logic and also decoupling the web layer, it becomes much easier to automate testing, leading to a more powerful system that meets expectations.

# Configuring SQL-based security

We configured our wiki application with startup security by using `InMemoryUserDetailsService`. It's easy to upgrade to database-managed security by simply swapping that component out with `DatabaseUserDetailsService`.

In the `SpringWikiAppContext`, we just need to change `user_details_service` to:

```
@Object
def user_details_service(self):
    service = DatabaseUserDetailsService()
    service.dataSource = self.factory()
    return service
```

In this example, we have left out the coding of `factory()`. This is simply a connection factory used to create a `DatabaseTemplate` to talk to the database, as covered in the earlier SQL chapter.

`DatabaseUserDetailsService` defines the following queries to look up user data and granted authorities:

+   - To lookup users: `SELECT username,password,enabled FROM users WHERE username = ?`
+   - To lookup authorities: `SELECT username,authority FROM authorities WHERE username = ?`