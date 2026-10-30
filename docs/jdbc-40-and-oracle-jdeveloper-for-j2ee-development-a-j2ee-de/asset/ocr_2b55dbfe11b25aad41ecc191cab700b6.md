Next, we will modify the command button action method in the backing bean. Double-click on the command button in the Design tab. In the Bind Action property window select the default settings for Managed Bean and Method and click on OK. In the command button action method retrieve the input field values and update the database table for the specified catalog id. Add an import statement to the backing bean class for the java.sql package. In the commandButton1_action method load the Oracle JDBC driver oracle.jdbc.OracleDriver using the Class.forName method.

Create a Connection object using the getConnection method of DriverManager class. Create a Statement object from the Connection object using createStatement method. Specify the result set type as scrollable and result set concurrency as read-only. Retrieve the Input Text field values using getValue method. Update database table Catalog by running an UPDATE SQL statement using the executeQuery method. The commandButton1_action method is listed below.

```
```java
public String commandButton1_action() {
    // Add event code here...
    ResultSet rs=null;
    try{Class.forName("oracle.jdbc.OracleDriver");
        String url="jdbc:oracle:thin:@localhost:1521:ORCL";
        Connection connection =
        DriverManager.getConnection(url,"OE", "pw");
    Statement
    stmt=connection.createStatement(ResultSet.TYPE_SCROLL_INSENSITIVE,
    ResultSet.CONCUR_READ_ONLY);
        int catalogId=((Integer)(inputText0.getValue())).
intValue();
        String journal=(String)inputText1.getValue();
        String publisher=(String)inputText2.getValue();
        String edition=(String)inputText3.getValue();
        String title=(String)inputText4.getValue();
        String author=(String)inputText5.getValue();
        String href=(String)inputText6.getValue();
    String query="UPDATE OE.CATALOG SET
    JOURNAL="+"'"+journal+"'"+","+"PUBLISHER="+"'"+publisher+"'"+
    ","+"EDITION="+"'"+edition+"'"+","+"  TITLE=" +"'"+title+"'"+","+""
    AUTHOR="+"'"+author+"'"+"," + " HREF="+"'"+href+"'"+"WHERE
    CATALOGID="+"'"+catalogId+"'";
    stmt.executeUpdate(query);stmt.close();connection.close();}
    catch(SQLException e){System.out.println(e.getMessage());}
    catch(ClassNotFoundException e)
    {System.out.println(e.getMessage());}return null;}
```
```