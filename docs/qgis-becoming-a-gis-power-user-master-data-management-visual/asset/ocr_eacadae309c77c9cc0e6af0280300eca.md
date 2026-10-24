Add the following to the preceding code. This will perform the following functions:

+   - Import the PySpatiaLite module and connect to our sqlite3/SpatiaLite database
+   - Use the connection as a context manager, which automatically commits the executed queries and rolls back in case of an error
+   - To test that the connection is working, use the SQLITE_VERSION() function in a SELECT query and print the result

The following code, appended to the preceding one, can be found at c6/data/web/cgi-bin/db_test.py. Make sure that the path in the code for the SQLite database file matches the actual location on your system.

```
# Import the pySpatiaLite module
from pySpatiaLite import dbapi2 as sqlite3
conn = sqlite3.connect('C:\packt\c6\data\web\cgi-bin\c6.sqlite')

# Use connection handler as context
with conn:
    c = conn.cursor()
    c.execute('SELECT SQLITE_VERSION()')

    data = c.fetchone()
    print data
    print 'SQLite version:{0}'.format(data[0])
```

You can view the following results in a web browser at http://localhost:8000/cgi-bin/db_test.py.

```
(u'3.7.17',) SQLite version:3.7.17
```

The first time that the data is printed, it is preceded by a u and wrapped in single quotes. This tells us that this is a unicode string (as our database uses unicode encoding). If we access element 0 in this string, we get a nonwrapped result.

# The Python code for web access to SQLite through JSON

The following code performs the following functions:

+   - It connects to the database
+   - It issues a query to find the minimum distance location and field where the specified location and date are given
+   - It returns JSON with field value pairs based on the database result field names and values