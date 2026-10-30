```
public void myTopLevelJDBCMethod()
{
    Connection c = null;

    /* All JDBC objects would be method level objects that ensure thread-safety and
    prevent connection leaking. Define the connection object before the JDBC 'try'
    block.*/
    try
    {
        /* The above is the JDBC try block for the method. All the JDBCs' for this method in
        the block is connected directly from the DataSource in the try block. Do not connect
        it from any method that has kept a connection, and is sharing it for repeated use.*/
        c = myDataSource.getConnection();

        /* You can pass the connection or sub-objects to sub-methods but none of these
        methods must expect to keep or use the objects they receive after their method call
        completes (eg).*/
        DoSomethingFancyWith(c);

        /* Use Prepared or Callable Statements. They are usually faster, because we cache
        them transparently with the pool.*/
        PreparedStatement p = c.prepareStatement(...);
        ResultSet rs = p.executeQuery();
        ProcessResult(myrs);

        /* Close JDBC objects in the proper order: first resultset,; then statement; and then
        connection.*/
        rs.close();

        /* Always close result sets as soon as possible (ASAP) at the levels they were
        created*/
        p.close();

        /* Always close statements ASAP at the levels they were created. When the JDBC is
        processed in the try-block, close the connection:*/
        c.close();

        /* Always close connection-ASAP in the same method and the block that was used
        to create or obtain it.*/
        c = null;
```