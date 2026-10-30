```
```java
webRowSet.insertRow();
} catch (SQLException sqlException)
{
    for (Throwable e: sqlException)
    {
        System.out.println("Error encountered: " + e);
    }
}
public void updateRow(int rowUpdate, String journal, String
    publisher,
    String edition, String title, String author)
{
    try
{
    webRowSet.absolute(rowUpdate);
    webRowSet.updateString(1, journal);
    webRowSet.updateString(2, publisher);
    webRowSet.updateString(3, edition);
    webRowSet.updateString(4, title);
    webRowSet.updateString(5, author);
    webRowSet.updateRow();
} catch (SQLException sqlException)
{
    for (Throwable e: sqlException)
    {
        System.out.println("Error encountered: " + e);
    }
}
}
public String[] readRow(int rowRead)
{
    String[] resultSet = null;
    try
{
    resultSet = new String[5];
    webRowSet.absolute(rowRead);
    resultSet[0] = webRowSet.getString(1);
    resultSet[1] = webRowSet.getString(2);
    resultSet[2] = webRowSet.getString(3);
    resultSet[3] = webRowSet.getString(4);
    resultSet[4] = webRowSet.getString(5);
} catch (SQLException sqlException)
{
```
```