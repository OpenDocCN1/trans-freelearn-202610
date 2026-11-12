
- 如果数据库表的主键由多个列组成，Roo 会创建一个复合主键类和一个 *_Roo_Identifier.aj AspectJ ITD。例如，对于 CUSTOMER_TBL，Roo 会生成 CustomerTblPK.java 和 CustomerTblPK_Roo_Identifier.aj AspectJ ITD 文件。

- 如果数据库表或视图未定义主键，Roo 会创建一个由数据库表或视图中的所有字段组成的复合主键类。例如，FLIGHTS_VIEW 未定义主键；因此，Roo 会创建 FlightsViewPK.java 复合主键类。

以下代码显示了 FlightDescTbl_Roo_DbManaged.aj ITD，它对应 FLIGHT_DESC_TBL 表：

```java
privileged aspect FlightDescTbl_Roo_DbManaged
{
    @OneToMany(mappedBy = "flightDescId")
    private Set<FlightTbl> FlightDescTbl.flightTbls;
    @Column(name = "destination_city",
    columnDefinition = "VARCHAR" length = 255)
    @NotNull
    private String FlightDescTbl.destinationCity;
    @Column(name = "origin_city", length = 255)
    @NotNull
    private String FlightDescTbl.originCity;
    ...
}
```

给定的代码显示，*_Roo_DbManaged.aj AspectJ ITD 包含从数据库元数据派生的字段和关系信息。这与我们在先前示例中看到的情况不同，在那些示例中，JPA 字段和关系包含在 Java 源文件中，而不在 AspectJ ITD 文件中。另外，请注意 @NotNull JSR 303 注释的存在，它源于相应的数据库列定义为非空。

与其他 AspectJ ITD 一样，*_Roo_DbManaged.aj 的创建由 Roo 注释 @RooDbManaged 管理，如 FlightDescTbl.java 类所示：

```java
@RooJavaBean
@RooToString
@RooEntity(table = "flight_desc_tbl")
@RooDbManaged(automaticallyDelete = true)
public class FlightDescTbl {
}
```

在给定的代码中，@RooDbManaged 注释表示 FlightDescTbl 实体由 Roo 的数据库逆向工程过程管理。如本代码所示，FlightDescTbl 类为空且仅包含类级别注释。@RooDbManaged 注释控制 *_Roo_DbManaged.aj ITD 的创建。@RooDbManaged 注释的 automaticallyDelete 属性指定如果从数据库中删除表，是否应移除该实体。默认值为 true，这意味着如果你删除相应的数据库表然后执行数据库逆向工程命令，JPA 实体将从 Roo 项目中移除。

### 还有更多...

现在让我们看看如何为 Oracle 和 DB2 数据库安装 JDBC 驱动程序，覆盖由数据库逆向工程过程生成的 JPA 字段和关系，执行增量数据库逆向工程，以及 Roo 如何处理复合主键情况。

### 为 Oracle 和 DB2 安装 JDBC 驱动程序

Roo 的数据库逆向工程过程要求 JDBC 驱动程序作为 OSGi bundle 可用。Roo 提供了 addon create wrapper 命令，你可以使用它将 JDBC 驱动程序包装在 OSGi 合规的 bundle 中，以用于数据库逆向工程过程。Roo 为大多数 JDBC 驱动程序提供了 OSGi 合规的 bundle，可以通过 addon install 命令进行安装。由于 Oracle 和 DB2 没有开源的 JDBC 驱动程序，要对这些数据库使用 Roo 的数据库逆向工程过程，你首先需要将这些数据库的非 OSGi JDBC 驱动程序 JAR 文件转换为 OSGi 合规的 bundle。

在第 7 章中，我们将了解如何使用 addon create wrapper 命令将非 OSGi JDBC 驱动程序 JAR 或任何其他 Maven artifact 转换为 OSGi 合规的 bundle。

### 修改自动生成的 JPA 字段/关系

如果你想修改由数据库逆向工程过程生成的 JPA 字段和关系，请在 JPA 实体类本身中定义它们。你在 JPA 实体类中定义的字段和关系将由 Roo 从相应的 *_Roo_DbManaged.aj ITD 文件中自动移除。

> 如果你在实体类中定义所有 JPA 字段和关系，Roo 将自动删除相应的 *_Roo_DbManaged.aj ITD 文件。

### 增量数据库逆向工程

你可以在项目生命周期中使用数据库逆向工程过程。如果对数据库进行了修改，请执行 `database reverse engineer` 命令，而不使用 `schema` 和 `package` 参数。数据库逆向工程过程会更新 `dbre.xml` 文件，从而导致 JPA 实体及其字段和关系的添加、删除和修改。

`dbre.xml` 文件由 Roo 管理，但在修改数据库之前，你可以修改它，以查看对 Roo 生成的 JPA 实体的更改的影响。需要注意的是，当你重新执行 `database reverse engineer` 命令时，你对 `dbre.xml` 文件的自定义更改将被覆盖。

### 具有复合主键的数据库表

如果你的数据库表具有复合主键，则数据库逆向工程过程会负责创建一个主键类，该类通过 *_Roo_Identifier.aj ITD 被 JPA @Embeddable 注释标注。然后，主键类通过 *_Roo_Entity.aj ITD 引入 JPA 实体类，并被 JPA @EmbeddedId 注释标注。

### 另请参阅

- 参阅“生成数据库元数据”示例，了解如何查看数据库表的元数据信息

## 4 使用 Spring Web MVC 进行 Web 应用程序开发

在本章中，我们将介绍：

- 从 JPA 实体自动生成 Spring MVC 控制器和 JSPX 视图
- 打包、部署和使用 Roo 生成的 Spring MVC 应用程序
- 修改 Roo 生成的视图
- Spring Roo 对 Web 控制器和视图的往返支持
- 为特定 JPA 实体创建 Spring MVC 控制器
- 为 JPA 实体手动创建 Spring MVC 控制器
- 向 Roo 生成的 Web 应用程序添加静态视图
- 国际化 Roo 生成的 Web 应用程序
- 添加或修改 Roo 生成的主题
- 为域对象和控制器添加 JSON 支持
- 为 Web 控制器创建和执行 Selenium 测试

### 简介

在第 2 章“使用 JPA 持久化对象”和第 3 章“Spring Roo 中的高级 JPA 支持”中，我们探讨了如何使用 Spring Roo 的 JPA 插件快速开发应用程序的领域层。在本章中，我们将了解 Spring Roo 如何使用 Spring Web MVC 简化企业应用程序 Web 层的开发。我们还将了解 Spring Roo 如何让你在本地快速测试 Web 应用程序。

> > 即使你想使用的 Web 框架未被 Roo 支持，你也可以使用 Roo 开发企业应用程序的持久层。例如，如果你使用 Wicket 开发 Web 应用程序，仍可以使用 Roo 生成应用程序的持久层。同样，如果你正在开发 Swing 应用程序，可以使用 Roo 创建持久层。

### 从 JPA 实体自动生成 Spring MVC 控制器和 JSPX 视图

Spring Roo 自带 Web MVC 插件，支持从 Roo 管理的 JPA 实体创建 Spring MVC 控制器和 JSPX 视图。Spring Roo 提供由 Web MVC 插件处理的多个命令，以帮助自动生成 Spring MVC 控制器和 JSPX 视图。默认情况下，Roo 生成的控制器支持从数据存储中创建、读取、更新和删除 JPA 实体。

Roo 提供以下命令用于创建控制器：

- `controller all`: 用于为应用程序中尚不存在控制器的每个 JPA 实体脚手架一个 Spring Web MVC 控制器。`controller all` 命令不会让你对生成的控制器所支持的应用程序功能进行任何控制。
- `controller scaffold`: 用于为 JPA 实体脚手架一个 Spring Web MVC 控制器。与 `controller all` 命令不同，`controller scaffold` 命令提供可选参数，允许你控制生成的控制器所支持的某些应用程序功能。例如，你可以指定不为控制器生成用于创建、更新和删除 JPA 实体的方法。
- `controller class`: 用于手动创建控制器。`controller class` 命令生成控制器的骨架，将控制器的实现留给开发人员。
