```
{
    try
{
        OutputStreamWriter output =
            new OutputStreamWriter(new FileOutputStream(new
                File("c:/output/output.xml")));
        webRowSet.writeXml(output);
    } catch (SQLException sqlException)
{
        for (Throwable e: sqlException)
{
            System.out.println("Error encountered: " + e);
        }
    }
    catch (IOException e)
{
    }
}
public void deleteRow(int row)
{
    try
{
        webRowSet.absolute(row);
        webRowSet.deleteRow();
    } catch (SQLException sqlException)
{
        for (Throwable e: sqlException)
{
            System.out.println("Error encountered: " + e);
        }
    }
}
public void insertRow(String journal, String publisher, String
            edition,
                    String title, String author)
{
    try
{
        webRowSet.moveToInsertRow();
        webRowSet.updateString(1, journal);
        webRowSet.updateString(2, publisher);
        webRowSet.updateString(3, edition);
        webRowSet.updateString(4, title);
        webRowSet.updateString(5, author);
```