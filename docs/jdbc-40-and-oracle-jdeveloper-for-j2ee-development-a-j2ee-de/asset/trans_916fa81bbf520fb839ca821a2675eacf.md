
![](img/553e44bc30e70d0d8e58278a7ece8da8_147_0.png)

在“多数据源”表中，单击“新建”按钮以创建新的多数据源。在“配置多数据源”窗口中，指定数据源名称和 JNDI 名称。选择算法类型为故障转移或负载均衡，然后单击“下一步”。

![](img/553e44bc30e70d0d8e58278a7ece8da8_147_1.png)

在“**选择目标**”窗口中，通过将多数据源部署到其上，选择 **examplesServer** 或其他任何服务器，然后单击“下一步”。

![](img/553e44bc30e70d0d8e58278a7ece8da8_148_0.png)

在“**选择数据源类型**”窗口中，为 XA 数据源选择 XA 驱动程序，或为非 XA 数据源选择非 XA 驱动程序，然后单击“**下一步**”。由于我们使用非 XA JDBC 驱动程序创建了 Oracle 数据源，请选择非 XA 驱动程序。在“**添加数据源**”窗口中，将数据源从“**可用**”列表添加到“**已选**”列表。如果需要新的数据源，请单击“**创建新的数据源**”按钮，然后单击“**完成**”。

![](img/553e44bc30e70d0d8e58278a7ece8da8_148_1.png)

新数据源已配置并添加到“**多数据源**”表。单击“**激活更改**”按钮以使数据源可供应用程序使用。

![](img/553e44bc30e70d0d8e58278a7ece8da8_149_0.png)

可以通过选择多数据源链接来配置多数据源。部署多数据源的目标可以通过“**目标**”选项卡进行配置。多数据源中的数据源可以通过“**配置**”选项卡中的数据源链接进行配置。多数据源 JNDI 名称可以在“**配置 | 通用**”窗口中修改。“**算法类型**”指定用于选择获取连接的数据源的算法。如果算法类型为“**故障转移**”，连接请求始终发送到列表中的第一个数据源。如果第一个数据源与 DBMS 失去连接，则连接请求将依次发送到列表中的下一个数据源，直到获得连接或到达数据源列表末尾。如果算法类型为“**负载均衡**”，连接请求的负载将在列表中的数据源之间平均分配。如果选择负载均衡，则连接请求也提供连接故障转移。这些连接请求将被发送到列表中的不同数据源，直到建立连接或到达数据源列表末尾。

多数据源还提供“**繁忙时故障转移请求**”、“**故障转移回调处理程序**”和“**测试频率**”设置。对于使用故障转移算法的多数据源，当选择“**繁忙时故障转移请求**”时，如果数据源中的所有连接都忙，则连接请求将发送到数据源池中的下一个数据源。“**故障转移回调处理程序**”指定用于处理回调的应用程序类，该回调在多数据源准备向另一个数据源发送故障转移连接请求时发送。“**测试频率**”指定测试连接的间隔（以秒为单位）。如果连接失败，则关闭并重新打开连接。如果连接再次失败，则关闭连接。

WebLogic Server 9.x 还支持数据源工厂。应用程序范围的连接池使用 JDBC 数据源工厂提供默认连接池值。通过单击“服务 | JDBC | 数据源工厂”链接来配置数据源工厂。在“数据源工厂”表中单击“新建”。在“创建新的 JDBC 数据源工厂”窗口中，指定数据源工厂名称、用户名和密码。这些用于登录数据库、连接 URL、驱动程序类名称和工厂名称，然后单击“确定”。数据源工厂已创建，可以部署到目标服务器节点。数据源工厂名称在管理控制台和配置文件 config.xml 中使用。工厂名称在部署描述符中使用。

## JDBC 性能调优

为了获得最佳稳定性和性能，请配置池在启动期间创建所需的所有连接，并无限期保留它们。因此，将初始容量设置为等于最大容量，这对于每个执行线程都是相同的。WebLogic Server 数据源提供的连接池通过为 JDBC 应用程序提供连接池来提高性能。无需为每个客户端打开和关闭连接。

“保留时测试连接”会在将连接提供给客户端之前测试连接，并可能降低性能。为防止频繁的连接测试，请设置连接池属性“Seconds to Trust an Idle Pool Connection”。此属性指定已知已成功使用的连接不使用 SQL 查询进行测试的秒数。当在外部客户端 JVM 中使用 RMI 通过 WebLogic 执行 JDBC 到 DBMS 时，通过在配置数据源时选择“启用行预取”并设置最佳预取大小，可以提高数据源性能。行预取通过从服务器向外部客户端预取多行来提高性能。不建议通过 WebLogic 在外部客户端中使用行预取。在 WebLogic JVM、JSP、servlet、EJB 等中执行 JDBC 要快得多。语句缓存通过重用语句而不是创建新语句来提高连接测试的性能。语句缓存在连接池配置中指定。

以下是无风险 WebLogic JDBC 编码风格的理想标准。池化速度很快，最好以快速的每次调用方式使用：

```
/* This is how you should make any of your top-level methods that will produce JDBC, work for any of your applications. */
```

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
        ProcessResult(rs);

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

        /* Set the connection to null, so that the finally block below knows that it has been taken care of.*/
    }
    catch (Exception e )
    {
        /* Do whatever, according to your needs. You would not have a catch block, if it is not needed.*/
    }
    finally
    {
        /* Always have the above finally block. A finally block is crucial to ensure that the connection is closed and returned to the pool, and is not leaked.*/
        /* Failsafe: Do every individual thing you want to do in the finally block in it's own try block-catch-ignore so everything is attempted.*/
        try
        {
            if (c != null) c.close();
        }
        catch (Exception ignore)
        {
        }
    }
}
```

## 从 JDeveloper 部署 Web 应用程序到 WebLogic Server

我们将开发一个 Web 应用程序，从“配置数据源”部分中配置的数据源检索 Oracle 数据库表中的数据。在 Oracle 数据库中创建一个示例数据库表。使用 SQL*Plus 等 SQL 客户端工具或命令行实用程序，使用以下 SQL 脚本创建表 Catalog：

```
CREATE TABLE Catalog (CatalogId INTEGER PRIMARY KEY, Journal VARCHAR(25), Publisher VARCHAR(25), Edition VARCHAR(25), Title Varchar(45), Author Varchar(25));
INSERT INTO Catalog VALUES ('1', 'Oracle Magazine', 'Oracle Publishing', 'Nov-Dec 2004', 'Database Resource Manager', 'Kimberly Floss');
INSERT INTO Catalog VALUES ('2', 'Oracle Magazine', 'Oracle Publishing', 'Nov-Dec 2004', 'From ADF UIX to JSF', 'Jonas Jacobi');
INSERT INTO Catalog VALUES ('3', 'Oracle Magazine', 'Oracle Publishing', 'March-April 2005', 'Starting with Oracle ADF ', 'Steve Muench');
```
