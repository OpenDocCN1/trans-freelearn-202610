

## 工作原理...

实体命令用于创建 JPA 持久化实体。它提供了一组参数，这些参数应足以满足您在开发企业应用持久层时遇到的大多数场景。例如，mappedSuperclass 参数使用 @MappedSuperclass JPA 注解标记类（请参阅第 3 章《Spring Roo 高级 JPA 支持》中的“创建映射超类”示例），inheritanceType 参数添加 @Inheritance JPA 注解，以便让您指定持久化继承层次结构中类所遵循的继承策略，等等。

接下来的表格描述了您可以传递给 entity 命令的参数：

| 参数 | 描述 |
| :--- | :--- |
| class | 这是一个必填参数，用于指定持久化实体类的完全限定名。指定完全限定名时，可以使用 '~' 符号。 |
| mappedSuperclass | 指示 Roo 该类是“映射超类”。如果指定，生成的类将使用 @MappedSuperclass JPA 注解进行标注。 |
| extends | 标识实体类的父类。 |
| abstract | 指定生成的实体是抽象实体。 |
| inheritanceType | 指定用于持久化实体的继承策略。它接受以下值之一：JOINED、SINGLE_TABLE 和 TABLE_PER_CLASS。如果指定，它会在实体类中添加 @Inheritance JPA 注解。 |
| table | 指定实体映射到的表名。 |
| identifierField | 指定实体中标识符字段的名称。默认情况下，标识符字段的名称为 id。 |
| identifierType | 指定标识符字段的 Java 类型。此参数可以接受 Roo 预定义的值，例如 java.lang.Long、java.lang.Double 等，也可以接受自定义 Java 类型。 |
| identifierColumn | 指定标识符字段映射到的表列。如果您的实体使用复合主键，则不得使用此参数。 |
| versionField | 指定实体中版本字段的名称。默认情况下，版本字段的名称为 version。 |
| versionColumn | 版本字段映射到的表列。 |
| testAutomatically | 指示 Roo 为实体自动生成集成测试。我们将在“为持久化实体创建集成测试”示例中详细讨论持久化实体的集成测试。 |
| schema, catalog | 用于指定数据库表名限定符的参数。这些参数转换为 @Table JPA 注解的 schema 和 catalog 属性。 |
| persistenceUnit | 持久化单元的名称，在 persistence.xml 文件中定义，持久化实体与此关联。 |
| transactionManager | [自 Spring Roo 1.1.5 起支持] 持久化实体使用的事务管理器的名称。 |

如果您的实体使用复合主键，则需要使用自定义 Java 类型。例如，Flight 实体将 FlightKey 指定为 identifierType，因为它是 Flight 实体的复合主键类。还要注意，当您执行 entity 命令时，复合主键类会自动生成。

您可能已经注意到，我们用于向 Flight JPA 实体添加字段的 field 命令，与我们在第 1 章《Spring Roo 入门》中的“向 Java 类添加属性”示例中用于向 Java 类添加属性的 field 命令是同一个。

```
@RooJavaBean
@RooToString
@RooEntity(identifierType = FlightKey.class, table= "FLIGHT_TBL")
public class Flight {
    private Integer numOfSeats;
    private String origin;
    private String destination;
    ...
}
```

在 Flight.java 代码中，@RooJavaBean 和 @RooToString 注解是最常用的 Roo 注解。有关 @RooJavaBean 和 @RooToString 的更多信息，请参阅第 1 章《Spring Roo 入门》中的“创建 Java 类”和“向 Java 类添加属性”示例。@RooEntity 注解提供了有关持久化实体（即前面代码中的 Flight 实体）的详细信息。identifierType 属性指定 Flight 实体的标识符类型，即 FlightKey——Flight 实体的复合主键类。table 属性指定 Flight JPA 实体映射到的数据库表。

> 您会注意到，实体的字段未使用 JPA @Column 注解来提供字段与相应 FLIGHT_TBL 表列之间的映射。稍后在本示例中，我们将了解如何使用 field 命令为字段指定表列映射。

@RooEntity 注解使用 ITD 文件 Flight_Roo_Entity.aj 引入了一些持久化相关方法和属性，如下所示：

```
privileged aspect Flight_Roo_Entity {
	declare @type: Flight: @Entity;

	declare @type: Flight: @Table(name = "FLIGHT_TBL");

	@PersistenceContext
	transient EntityManager Flight.entityManager;

	@EmbeddedId
	private FlightKey Flight.id;

	@Version
	@Column(name = "version")
	private Integer Flight.version;

	public FlightKey Flight.getId() {
		return this.id;
	}

	public void Flight.setId(FlightKey id) {
		this.id = id;
	}

	public Integer Flight.getVersion() {
		return this.version;
	}

	public void Flight.setVersion(Integer version) {
		this.version = version;
	}

	...
}
```

为简洁起见，前面代码清单中省略了持久化相关方法（例如 persist、remove 等）。自动生成的持久化相关方法将在“控制持久化实体的自动生成方法”示例中讨论。如您所见，Spring Roo 生成了创建功能齐全的 JPA 实体所需的代码。

Flight_Roo_Entity.aj 中的以下代码在 Flight 类中添加了 @Table 和 @Entity JPA 注解：

```
declare @type: Flight: @Entity;
declare @type: Flight: @Table(name = "FLIGHT_TBL");
```

在 Flight_Roo_Entity.aj 中，FlightKey 字段使用了 @EmbeddedId 注解，因为它是 Flight 实体的复合主键类。Roo 还在 Flight_Roo_Entity.aj 中创建了一个 version 字段，该字段映射到 Flight 实体映射表的 version 列。如果我们创建一个不使用复合主键的持久化实体，那么 Spring Roo 将使用 @Id 注解而非 @EmbeddedId 来标注主键。

在生成实体时，Spring Roo 还会生成一个 <entity-name>_Roo_Configurable.aj ITD 文件，用于向实体添加 Spring 的 @Configurable 注解。在此，<entity-name> 是持久化实体的名称。

实体实例通常由 JPA 提供程序或通过 new 运算符在 Spring 应用程序上下文之外创建。在实体中使用 @Configurable 注解特别有用，因为它允许将 Spring 应用程序上下文中配置的 Bean 注入到实体实例中。正因有 @Configurable 注解，Spring 才能将 EntityManager 实例注入到持久化实体中。

以下代码清单显示了 flight-app 应用程序的 FlightKey 类：

```
@RooToString
@RooIdentifier
public final class FlightKey {
    private String flightId;
    private Date departureDate;
}
```

在代码中，Spring Roo 的 @RooIdentifier 注解负责为字段添加构造函数、getter 和 setter 方法，并还为 FlightKey 复合主键类提供 hashCode 和 equals 方法的实现。Spring Roo 会生成一个与复合主键类上的 @RooIdentifier 注解对应的 <entity-name>_Roo_Identifier.aj ITD 文件。在此，<entity-name> 是持久化实体的名称。

以下代码显示了在 FlightKey_Roo_Identifier.aj ITD 文件中定义的方法和属性：

```
privileged aspect FlightKey_Roo_Identifier {
    declare @type: FlightKey: @Embeddable;

    public FlightKey.new(String flightId, Date departureDate) {}
}
```

