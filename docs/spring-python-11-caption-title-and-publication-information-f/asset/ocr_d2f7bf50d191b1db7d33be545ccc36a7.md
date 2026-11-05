# Replacing multiple lines of query code with one line of Spring Python

Spring Python has a convenient utility class called DatabaseTemplate that greatly simplifies this problem.

+   1. Let's replace the two lines of import and connect code from the earlier example with some Spring Python setup code.

```
from springpython.database.factory import Sqlite3ConnectionFactory
from springpython.database.core import DatabaseTemplate

conn_factory = Sqlite3ConnectionFactory("/path/to/sqlite3db")
dt = DatabaseTemplate(conn_factory)
```

At first glance, we appear to be taking a step back. We just replaced two lines of earlier code with four lines. However, the next block should improve things significantly.

+   2. Let's replace the earlier coded query with a call using our instance of DatabaseTemplate.

```
return dt.query_for_object("SELECT COUNT(*) FROM ARTICLE")
```

Now we have managed to reduce a complex 14-line block of code into one line of Spring Python code. This makes our Python code appear as simple as the original SQL statement we typed in the database's shell. And it also reduces the noise.

# The Spring triangle—Portable Service Abstractions

We saw this diagram earlier in the book, as an illustration of the key principles behind Spring Python.

![](bbox=[0.33, 0.64, 0.66, 0.79])