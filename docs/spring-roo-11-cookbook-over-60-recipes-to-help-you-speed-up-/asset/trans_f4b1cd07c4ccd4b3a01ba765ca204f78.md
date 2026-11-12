

## 控制持久实体自动生成的方法

当使用 Roo 创建持久实体时，会自动生成多个方法以简化实体的使用和测试。例如，在“创建持久实体”配方中创建了 Flight 实体后，相应的 Flight_Roo_Entity.aj AspectJ ITD 文件将被创建，其中包含 persist、remove、merge、flush、findFlight 等方法。

在本配方中，我们将了解如何通过以下方式控制实体方法的生成：

- 指定方法所使用的前缀
- 指示 Roo 不生成特定方法

为了本配方的目的，我们将指示 Roo 对 Flight 实体执行以下操作：

- 将自动生成的 persist 方法名称更改为 save
- 将自动生成的 findFlight 方法名称更改为 finderForFlight
- 不要生成 countFlights 和 findFlightEntries 方法

### 准备工作

退出 Roo shell，删除 C:\roo-cookbook\ch02-recipes 目录的内容。

执行 ch02_jsr303_fields.roo 脚本。该脚本会创建 flight-app Roo 项目，并使用 persistence setup 命令将 Hibernate 设置为持久化提供程序。该脚本还会创建 Flight 实体（其复合主键类为 FlightKey），并向 Flight 和 FlightKey 类添加字段。如果您使用的数据库不是 MySQL，或者连接设置与脚本中指定的不同，请相应地修改脚本。

从 C:\roo-cookbook\ch02-recipes 目录启动 Roo shell。

### 具体步骤...

以下步骤将向您展示如何控制自动生成的方法：

- 1. 在您喜欢的 IDE 中打开 Flight.java 文件。
- 2. 将 @RooEntity 注释更改为：@RooEntity(identifierType = FlightKey.class, persistMethod="save", countMethod="",
findMethod="finderFor", findEntriesMethod="")

- 3. 您将在 Roo shell 中看到以下输出：

```
Updated SRC_MAIN_JAVA\\sample\\roo\\flightapp\\domain\\Flight_Roo_Entity.aj
```

### 工作原理...

Roo 的 @RooEntity 注释负责管理相应的 *_Roo_Entity.aj AspectJ ITD 文件中定义的方法。@RooEntity 注释定义了允许您指定 Roo 生成的方法前缀的属性，并可控制是否生成特定方法。@RooEntity 注释还定义了可用于指定实体标识符和版本字段名称、类型及表列信息的属性。

在前面的代码中，@RooEntity 注释指定了以下信息：

- identifierType = FlightKey.class：指定 Flight 实体的标识符类型为 FlightKey 类。默认值为 Long。
- persistMethod = "save"：值 'save' 表示 Roo 不会生成 persist 方法，而是生成名为 save 的方法。
- countMethod = ""：由于值为 ""，表示 Roo 不得生成 countFlights 方法。
- findEntriesMethod = ""：由于值为 ""，表示 Roo 不得生成 findFlightEntries 方法。
- findMethod = "finderFor"：由于值为 finderFor，Roo 不会生成 findFlight 方法，而是生成名为 finderForFlight 的方法。

下表描述了 @RooEntity 注释定义的所有属性：

| @RooEntity 属性 | 描述 |
| --- | --- |
| countMethod, findAllMethod, findEntriesMethod, findMethod, flushMethod, mergeMethod, persistMethod, removeMethod | 用于指定生成方法的前缀的属性。值为 "" 表示 Roo 不会生成该方法。 |
| identifierColumn, identifierField, identifierType | 用于指定 JPA 实体标识符信息的属性，包括标识符字段映射到的表列名称、AspectJ ITD 文件中的标识符字段名称以及标识符字段的 Java 类型。 |
| versionField, versionType, versionColumn | 用于指定版本字段信息的属性，包括版本字段映射到的表列名称、AspectJ ITD 文件中的版本字段名称以及版本字段的 Java 类型。 |
| finders | 指定 Roo 为其生成动态查找方法的方法名称的属性。 |
| mappedSuperclass | 指示 Roo 生成 @MappedSuperclass 注释而不是 @Entity。我们将在第 3 章“Spring Roo 中的高级 JPA 支持”中了解映射超类的用法。 |
| inheritanceType | 要用于 JPA 实体的继承类型。 |
| persistenceUnit | 实体关联的持久化单元的名称，在 persistence.xml 中定义。 |
| transactionManager | [自 Spring Roo 1.1.5 起支持] 与实体关联的事务管理器的名称。 |

> **另请参阅**
> 请参阅“创建持久实体”配方，了解如何使用 Roo 创建 JPA 实体

## 为持久实体创建集成测试

Spring Roo 提供了一个 test integration 命令，用于简化持久实体的集成测试创建。在本配方中，我们将了解如何为实体创建集成测试。

### 准备工作

退出 Roo shell，删除 C:\roo-cookbook\ch02-recipes 目录的内容。

执行 ch02_jsr303_fields.roo 脚本。该脚本会创建 flight-app Roo 项目，并使用 persistence setup 命令将 Hibernate 设置为持久化提供程序。该脚本还会创建 Flight 实体（其复合主键类为 FlightKey），并向 Flight 和 FlightKey 类添加字段。如果您使用的数据库不是 MySQL，或者连接设置与脚本中指定的不同，请相应地修改脚本。

从 C:\roo-cookbook\ch02-recipes 目录启动 Roo shell。

### 具体步骤...

以下步骤将向您展示如何创建集成测试：

- 1. 将 Roo 命令的焦点更改为 Flight 实体：
  ```
  roo> focus --class ~.domain.Flight
  ```

- 2. 执行 `test integration` 命令：
  ```
  ~.domain.Flight> test integration
  Created ...FlightDataOnDemand.java
  Created ...FlightIntegrationTest.java
  Created ...FlightDataOnDemand_Roo_DataOnDemand.aj
  Created ...FlightIntegrationTest_Roo_IntegrationTest.aj
  Created ... FlightDataOnDemand_Roo_Configurable.aj
  Created ... Flight_Roo_Configurable.aj
  ```

### 工作原理...

`test integration` 命令会在 src\test\java 文件夹中生成文件。`test integration` 命令的输出显示生成了以下文件：

`test integration` 命令的输出显示生成了以下文件：

- `FlightDataOnDemand.java`：表示一个“按需数据”类，为 `Flight` 实体的自动化集成测试提供必要数据。

- FlightDataOnDemand_Roo_DataOnDemand.aj：此 AspectJ ITD 文件定义在编译期间添加到 FlightDataOnDemand 类中的方法。此 AspectJ ITD 文件中定义的方法负责动态创建 10 个 Flight 实体实例并将其存储在数据库中（即种子数据）。在执行集成测试时会使用这些 Flight 实例。AspectJ ITD 创建的实体实例符合适用于实体持久字段的 JSR 303 约束。默认情况下，与测试方法关联的事务在测试方法完成后会回滚——这就是执行集成测试后您不会在数据库表中看到种子数据的原因。请参阅“执行持久实体测试”配方，了解 Spring 的 @Rollback 注释的示例用法，用于指定与测试方法关联的事务不得回滚。

- FlightIntegrationTest.java：表示 Flight 实体的 JUnit 集成测试类。

- FlightIntegrationTest_Roo_IntegrationTest.aj：负责为 Flight 实体定义集成测试方法的 AspectJ ITD。

- FlightDataOnDemand_Roo_Configurable.aj 和 Flight_Roo_Configurable.aj：分别向 FlightDataOnDemand 和 Flight 类添加 @Configurable 注释的 AspectJ ITD。

让我们现在详细查看每个文件。

以下清单显示了 FlightDataOnDemand.java 类：

```
import org.springframework.roo.addon.dod.RooDataOnDemand;
import sample.roo.flightapp.domain.Flight;

@RooDataOnDemand(entity = Flight.class)
public class FlightDataOnDemand { }
```

代码清单展示了 Roo 的 @RooDataOnDemand 注释的用法，该注释标识 FlightDataOnDemand 类为其创建集成测试种子数据的持久实体。@RooDataOnDemand 注释负责创建相应的 *_Roo_DataOnDemand.aj AspectJ ITD 文件。@RooDataOnDemand 接受两个属性来自定义种子数据生成的行为：

- entity：标识需要为其创建种子数据的持久实体。

- quantity：要为实体创建的记录数，默认值为 10。如果您想为实体的集成测试创建更多记录，请为此属性指定适当的值。

