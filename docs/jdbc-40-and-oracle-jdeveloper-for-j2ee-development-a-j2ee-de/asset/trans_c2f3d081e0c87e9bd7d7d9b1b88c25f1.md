
3. XSQL 页面处理器使用 Oracle XML 解析器解析 XSQL 页面并缓存该页面。
4. XSQL 页面处理器使用 XSQL 页面中的 connection 属性连接到数据库。
5. XSQL 页面处理器使用 XML SQL 实用程序 (XSU) 从 SQL 查询生成 XML。
6. XSQL 页面处理器通过用其操作处理程序返回的 XML 结果替换每个 XSQL 操作元素（如 xsql:query）来生成 XML 数据报。
7. 如果指定了 XSLT 样式表，XSQL 页面处理器将解析样式表并缓存它。
8. XSQL 页面处理器使用 Oracle XSLT 处理器应用 XSLT 样式表来转换 XML 数据报。
9. XSQL 页面处理器将 XSLT 转换的 XML/HTML 输出返回给 XSQL Servlet 以在浏览器中显示。

本章中，我们将使用 XSQL 查询包含目录的示例数据库表。随后，我们将通过 XSLT 转换将 XML 输出显示为 HTML 表格。本章您将学习以下内容：

- 创建 XSQL 页面以运行 SQL 查询并生成 XML
- 在 SQL 查询中使用绑定参数
- 应用 XSQL 查询属性来自定义 XML 输出
- 对 XML 输出应用 XSLT 转换

## XSQL 标签概述

本节中，我们将讨论可在 XSQL 页面中指定的不同 XSQL 标签。XSQL 页面中的根元素是 <page>。以下表格中讨论了一些 XSQL 标签。XSQL 标签属性是可选的，除非在表格中指定为必需。

| XSQL 标签 | 描述 | 属性 |
|---|---|---|
| xsql:query | 运行 SQL 查询并将结果集作为 XML 包含在内。 | 参见第二张表 |
| xsql:dml | 运行 DML SQL 语句或 PL/SQL 匿名块。 | name (required): 页面级参数的名称。ignore-empty-value: 指定是否应忽略空值 ("")。默认为 'no'。value: 参数值。 |
| xsql:set-page-param | 设置页面级参数。 | name (required): 页面级参数的名称。ignore-empty-value: 指定是否应忽略空值 ("")。默认为 'no'。value: 参数值。 |
| xsql:set-session-param | 设置会话级参数。 | 同 xsql:set-page-param。 |
| xsql:set-stylesheet-param | 设置 XSLT 样式表参数。 | 同 xsql:set-page-param。 |
| xsql:include-param | 包含表示页面参数名称和值的 XML。例如，如果页面参数定义为：<xsql:set-page-param name="param1" value="paramValue"/> 定义为 <xsql:include-param name="param1"/> 的 include-param 在 XML 输出中包含以下 XML：<param1>paramValue</param1> | Name (required): 要包含的参数的名称。 |
| xsql:query | 运行 SQL 查询并将结果集作为 XML 包含在内 | 参见第二张表 |

| XSQL 标签 | 描述 | 属性 |
|---|---|---|
| xsql:include-request-params | 包含表示所有 HTTP 参数、Cookie 和会话变量的名称和值的 XML 片段。例如，如果指定了 `<xsql:include-request-params/>`，则会包含以下 XML 片段：`<request><parameters><param1>value1</param1></parameters><session><var1>value1</var1></session><cookies><cookiename1>cookieValue</cookiename1></cookies></request>` |  |
| xsql:include-xml | 包含来自绝对、相对或参数化 URL 的外部 XML 内容。 | href (required): 可能为静态文件或动态源的绝对、相对或参数化 URL。 |
| xsql:include-xsql | 包含另一个 XSQL 页面的 XML 输出。 | href (required): XSQL 资源的绝对、相对或参数化 URL。reparse: 指定是否应重新解析包含的 XSQL 页面。默认值是 'no'。 |

| XSQL 标签 | 描述 | 属性 |
|---|---|---|
| xsql:insert-request | 将请求中已发布的 XML 文档或 HTML 表单插入到数据库表或视图中。XML 文档可以用 XSLT 转换。如果发布了 HTML 表单，则包含请求参数、会话变量和 Cookie 的 XML 文档。示例 XML 文档：`<request><parameters><param1>value1</param1></parameters><session><var1>varValue</var1></session><cookies><cookiename1>cookieValue</cookiename1></cookies></request>` | table (required): 表、视图或同义词的名称。transform: 用于转换 XML 文档的 XSLT 的相对或绝对 URL。columns: 列的空格或逗号分隔列表 date-format: 数据字段值的日期格式。 |
| xsql:update-request | 更新已发布 XML 文档中表示的行。 | 除 xsql: insert-request 的属性外：key-columns: 用于标识表行的列值的空格或逗号分隔列表。 |
| xsql:delete-request | 删除已发布 XML 文档中表示的行。 | 同 xsql:update-request，只是 columns 不是属性。 |
| xsql:insert-param | 将参数的值插入到数据库表或视图中。 | name (required): 参数名称。table (required): 表名称。date-format: 数据字段值的日期格式。transform: XSLT 的相对、绝对或参数化 URL。 |

| XSQL 标签 | 描述 | 属性 |
|---|---|---|
| xsql:set-cookie | 设置 HTTP Cookie 的值。 | name (required): HTTP Cookie 的名称。domain: Cookie 有效且可读的域。默认值是创建 Cookie 的文档的完整域。ignore-empty-value: 指定是否忽略空值。默认值是 'no'。max-age: Cookie 的最大生存时间（以秒为单位）。默认生存时间是用户当前浏览器会话。only-if-unset: 指定是否仅在 Cookie 当前未定义时设置 Cookie。默认值是 'no'。path: Cookie 有效且可读的域内的相对 URL。默认值是创建 Cookie 的文档的 URL 路径。value: Cookie 值。 |

## 设置环境

本节中，我们将讨论创建 XSQL 应用程序所需的前期设置，这并不多，因为 XSQLServlet 已在 web.xml 中指定，并且在创建 XSQL 页面时 XSQL 库已添加到项目中。Oracle 数据库 11g JDBC 驱动程序可与任何 Oracle 数据库 9i 及更高版本一起使用。我们将使用 Oracle 数据库 10g。我们需要安装 Oracle 数据库 10g 包括示例模式。我们需要创建并启动 Oracle 数据库实例。使用订单录入 (OE) 示例模式（用户名）连接到数据库。

```
CONNECT OE/<password>
```

运行以下 SQL 脚本来创建一个由示例目录组成的数据库表：

```
CREATE TABLE Catalog(Journal VARCHAR(25), Publisher
    Varchar(25),Edition VARCHAR(25), Title Varchar(45), Author
    Varchar(25));
INSERT INTO Catalog VALUES('Oracle Magazine', 'Oracle
    Publishing','July-August 2005', 'Tuning Undo Tablespace', 'Kimberly
    Floss');
INSERT INTO Catalog VALUES('Oracle Magazine',   'Oracle
    Publishing','September-October 2005', 'Creating Search Pages',
    'Steve Muench');
```

我们需要为 XSQL 应用程序创建一个 JDeveloper 项目：

![](img/553e44bc30e70d0d8e58278a7ece8da8_221_0.png)

## 配置连接

我们还需要创建一个数据库连接来运行 XSQL 查询。默认情况下，Oracle XSQL 页面使用在 J2EE Web 应用程序环境中配置的 Oracle JDBC 数据源。XSQLConfig.xml 中 connection-manager 元素中的 factory 元素设置 XSQL 页面使用 JDBC 数据源。

```
<connection-manager>
    ...
    <factory>
        oracle.xml.xsql.XSQLOracleDatasourceConnectionManager
    </factory>
</connection-manager>
```

XSQL 页面还可以通过在 XSQLConfig.xml 中按如下方式设置 connection-manager 工厂类来使用基于 XSQLConfig.xml 的命名连接：

```
<factory>oracle.xml.xsql.XSQLConnectionManagerFactoryImpl</factory>
```

## XSQL

如果要在 XSQL 页面中使用第三方 JDBC 数据源，请在 XSQLConfig.xml 中按如下方式设置 connection-manager 工厂类：

```
<factory> oracle.xml.xsql.XSQLDatasourceConnectionManager</factory>
```

JDeveloper 10.1.3.3 提供了三种不同的方法从 XSQL 页面创建数据库连接：

- **连接导航器** 中的 JDBC 连接。**连接导航器** 连接具有相应的数据源，并提供 JNDI 名称绑定。
- 在 **Embedded OC4J 首选项窗口** 或 `data-sources.xml` 中配置的数据源。
- 在 `XSQLConfig.xml` 文件中指定的连接。

要在 **连接导航器** 中创建数据库连接，请在 **连接导航器** 中选择 **数据库 | 新建数据库连接** 节点。Create Connection Wizard 将启动。单击 **下一步**。在 **类型** 框架中，指定 **连接名称** 并将 **连接类型** 选择为 Oracle (JDBC)。在 **身份验证** 框架中，指定 **用户名** 和 **密码** 并单击 **下一步**。在 **连接** 框架中指定 **JDBC 驱动程序**、**主机名**、**JDBC 端口** 和数据库 **SID** 并单击 **下一步**。

![](img/553e44bc30e70d0d8e58278a7ece8da8_222_0.png)
