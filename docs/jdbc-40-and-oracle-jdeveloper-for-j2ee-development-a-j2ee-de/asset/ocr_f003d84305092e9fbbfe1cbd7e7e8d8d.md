An SQLException in a JDBC application might be chained to other SQLExceptions and a developer would be interested in retrieving the chained exceptions.
In JDBC 3.0 the chained exceptions and the chained causes of the exceptions had to be retrieved by invoking the getNextException() and getCause() methods recursively.

```
```java
catch(SQLException e)
{
while(e != null)
{
System.out.println("SQLException Message:" + e.getMessage());
Throwable t = e.getCause();
while(t != null)
{
System.out.println("SQLException Cause:" + t);
t = t.getCause();
}
e = e.getNextException();
}
}
```
```

JDBC 4.0 has added support for the Java SE chained exception facility also called the cause facility. The support for the Java SE chained exception facility is implemented with following new features.

+   - Four new constructors in the SQLException class that have the Throwable cause as one of the parameters.
+   - SQLException class supports the enhanced for-each loop introduced in J2SE 5.0 to retrieve the chained exceptions and chained causes without invoking the getNextException() and getCause() methods recursively.
+   - The getCause() method supports non-SQLExceptions.

Chained exceptions and chained causes may be retrieved using the enhanced for-each loop as follows:

```
```java
catch(SQLException sqlException)
{
for(Throwable e : sqlException )
{
System.out.println("Error encountered: " + e);
}
}
```
```