

> 在给定的代码中，`FlightDescriptionDataOnDemand` 对象被自动注入到 `FlightDataOnDemand` 对象中。由于 `Flight` 实体与 `FlightDescription` 实体具有多对一关系，`Flight` 实体的按需数据类使用 `FlightDescription` 的按需数据类来获取现有 `FlightDescription` 实例的引用，并将其设置在 `Flight` 实例中。这确保了在多对一关系的集成测试期间创建的测试数据与相关实体之间存在的关系一致。

需要注意的是，`@NotNull` 和 `@ManyToOne(..., optional=false)` 具有相同的含义。可以通过使用 `@NotNull`（JSR 303：Bean Validation 注解）或将 `@ManyToOne` JPA 注解的 `optional` 元素的值指定为 `false`，来指定实体之间必须存在关系。在这两种情况下，实体的按需数据类都将确保使用相关实体的按需数据类，对集成测试中创建的测试数据强制执行外键约束。

### 多对一关系字段的动态查找器方法

与其他持久字段一样，你可以使用 `finder list` 和 `finder add` 命令来查看和添加与关系字段对应的查找器方法。例如，如果针对 `Flight` 实体执行 `finder list` 命令，则其中一个查找器是针对 `flightDescription` 关系字段的，如下所示：

```
~.domain.Flight roo> finder list
```

```
..
```

```
findFlightsByFlightDescription(FlightDescription flightDescription)
```

此外，你可以使用 `finder list` 命令的 `depth` 参数，查看基于实体的关系字段和其他持久字段的动态查找器方法。

### 另请参阅

- 后续的“在实体之间创建一对多（或多对多）关系”示例

## 在实体之间创建一对多（或多对多）关系

在上一个示例中，我们了解了如何使用 `field reference` 命令在实体之间建立多对一（或一对一）关系。在本示例中，我们将扩展相同的示例，展示如何使用 `field set` 命令创建一对多（或多对多）关系。以下类图显示了 `FlightDescription` 和 `Flight` 实体之间的一对多关系：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_126_0.png)

在给定的图中，你会注意到我们已将类型为 `Set<Flight>` 的 `flights` 字段添加到 `FlightDescription` 实体，反映了 `FlightDescription` 和 `Flight` 实体之间的一对多关系。

### 准备工作

退出 Roo shell 并删除 `C:\roo-cookbook\ch03-recipes` 目录的内容。

从 `C:\roo-cookbook\ch03-recipes` 目录启动 Roo shell。

执行 `ch03_relationship_many_to_one.roo` 脚本，该脚本将创建 `flight-app` Roo 项目，设置 Hibernate 作为持久化提供程序，将 MySQL 配置为应用程序的数据库，创建 `Flight` 和 `FlightDescription` JPA 实体，并定义 `Flight` 和 `FlightDescription` 实体之间的多对一关系。如果你使用的数据库不是 MySQL 或你的连接设置与脚本中指定的不同，则相应地修改脚本。

### 操作步骤

要在 `FlightDescription` 和 `Flight` 实体之间创建一对多（或多对多）关系，请按照此处给出的步骤操作：

- 1. 将 Roo 命令的焦点设置在 `FlightDescription` 实体上：

```
.. roo> focus --class ~.domain.FlightDescription
```

- 2. 使用 field set 命令在 FlightDescription 和 Flight 实体之间创建一对多关系：

```
~.domain.FlightDescription roo> field set --fieldName flights --type ~.domain.Flight --cardinality ONE_TO_MANY --mappedBy flightDescription
```

### 工作原理

field set 命令用于在实体内创建一对多 JPA 关系的多方。如果两个实体之间的关系是多对多，则 field set 命令用于关系中的两个实体。以下表格描述了可以为 field set 命令指定的一些重要参数：

| 参数 | 说明 |
| :--- | :--- |
| fieldName (必填) | 要添加到实体的关系字段的名称。该字段的类型为 `java.util.Set`。在我们的示例中，fieldName 参数指定为 `flights`，这会在 `FlightDescription` 实体中添加一个名为 `flights` 的字段。 |
| type (必填) | 标识 fieldName 参数所定义的 Set 中包含的相关实体（即一对多关系中多方）的类型。在我们的示例中，type 值指定为 `~.domain.Flight`，这意味着 Set（由 fieldName 参数定义）包含类型为 `Flight` 的元素，如下所示：`private Set<Flight> flights = new HashSet<Flight>()`； |
| mappedBy | mappedBy 参数的值指的是关系的所有者。它转换为 JPA `@OneToMany` 或 `@ManyToMany` 注解的 mappedBy 元素的值。在我们的示例中，mappedBy 参数的值是 `flightDescription`；因此，field set 命令创建的 `flights` 字段带有 `@OneToMany` 注解，其 mappedBy 属性值为 `flightDescription`。 |
| cardinality | JPA 实体之间关系的基数。默认情况下，基数为 `MANY_TO_MANY`。由于我们希望在 `FlightDescription` 和 `Flight` 实体之间创建一对多关系，因此 cardinality 参数的值指定为 `ONE_TO_MANY`。 |
| fetch | 相关实体的 JPA 获取策略。可能的值为 `EAGER` 和 `LAZY`。fetch 参数的值转换为 `@ManyToMany` 或 `@OneToMany` 注解的 fetch 元素的值。 |

> 尽管 Roo shell 将 `MANY_TO_ONE`、`MANY_TO_MANY`、`ONE_TO_MANY` 和 `ONE_TO_ONE` 显示为 field set 命令的 cardinality 参数的可能值，但它仅接受 `MANY_TO_MANY` 和 `ONE_TO_MANY`。原因是：field set 仅在多对多关系的情况或实体位于一对多关系的一方时才有意义。

在 `FlightDescription` 实体上执行 field set 命令后，会向其添加以下字段：

```
@OneToMany(cascade = CascadeType.ALL,
       mappedBy="flightDescription")
private Set<Flight> flights = new HashSet<Flight>() ;
```

给定的代码显示，field set 命令只是向实体添加一个关系字段。

### 更多内容

field set 和 field reference 命令不提供指定应用于相关实体的级联效果的选项。命令的行为如下所述：

- 使用 field set 命令创建的带有 `@ManyToMany` 或 `@OneToMany` 注解的 JPA 关系字段，其 cascade 元素的值为 `CascadeType.ALL`，这意味着 refresh、persist、merge 等实体操作会被传播到相关实体。
- 使用 field reference 命令创建的带有 `@ManyToOne` 和 `@OneToOne` 注解的 JPA 关系字段未指定 cascade 元素，这意味着 refresh、persist、merge 等实体操作不会被传播到关联实体。

在需要指定级联效果的场景中，你可以修改 Java 源文件中的相应 JPA 注解。

### 另请参阅

- 请参阅“在实体之间创建多对一（或一对一）关系”示例，了解如何使用 field reference 命令来创建多对一（或一对一）关系

## 创建映射超类

在本示例中，我们将了解如何创建继承 *映射超类* 的实体。映射超类的字段存储到继承实体所映射的表中。映射超类本身 *不* 映射到任何表。在本示例中，我们将创建 *Flight* 实体，它扩展 *AuditFields* 类。*AuditFields* 类代表一个 *映射超类*；*AuditFields* 的字段存储到 *Flight* 实体所映射的表中。

以下图显示了 *Flight* 实体与 *AuditFields* 映射超类之间的关系：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_129_0.png)

### 准备工作

退出 Roo shell 并删除 `C:\roo-cookbook\ch03-recipes` 目录的内容。

从 `C:\roo-cookbook\ch03-recipes` 目录启动 Roo shell。

执行 `ch03_jpa_setup.roo` 脚本，该脚本将创建 *flight-app* Roo 项目，设置 Hibernate 作为持久化提供程序，并将 MySQL 配置为应用程序的数据库。如果你使用的数据库不是 MySQL 或你的连接设置与脚本中指定的不同，则相应地修改脚本。

### 操作步骤

要创建继承 *映射超类* 的实体，请按照给定步骤操作：

- 1. 使用 `entity` 命令创建 *AuditFields* 类并向其添加字段：

```
.. roo> entity --class ~.domain.AuditFields --mappedSuperclass
```

