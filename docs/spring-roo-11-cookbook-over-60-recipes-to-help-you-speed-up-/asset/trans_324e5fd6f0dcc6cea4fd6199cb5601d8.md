

## 另请参阅

- 请参阅“创建持久实体”配方，了解如何创建持久实体
- 请参阅“创建与多个数据库交互的应用程序”配方，了解如何开发与多个数据库交互的应用程序的详细信息

## 查看数据库配置属性

在本配方中，我们将了解 `database properties list` 命令如何让我们查看数据库属性列表及其值，这些值在 `database.properties` 文件中指定。

### 准备工作

请参阅 *为您的项目设置 JPA 提供程序* 配方，以创建 *flight-app* Roo 项目，并使用 *persistence setup* 命令设置持久性提供程序。

> 如果您在 Roo 项目中使用的是 JNDI 绑定的数据源，则不需要本配方。

### 操作方法

按照以下步骤查看数据库属性：

- 1. 从 C:\roo-cookbook\ch02-recipes 目录启动 Roo shell。
- 2. 要查看定义在 SRC_MAIN_RESOURCES\META-INF\spring\ 目录中的 database.properties 文件中的数据库属性，可以使用 database properties list 命令，如下所示：
  ```
  roo> database properties list
  ```

  ```
  database.driverClassName = com.mysql.jdbc.Driver
  database.password =
  database.url = jdbc:mysql://localhost:3306/myFlightAppDB
  database.username =
  ```

### 工作原理

database properties list 命令由 JPA 加载项处理。除了使用 database properties list 命令，您还可以使用 properties list 命令（请参阅第 1 章中的“查看属性文件中定义的属性”配方），该命令显示属性文件中包含的属性。使用任一命令的最终结果相同，唯一的区别是您需要在 properties list 命令中指定名称和路径参数来引用 database.properties 文件。

### 更多内容

除了使用 Spring Roo，您还可以使用 Eclipse 或 STS 等 IDE 查看 database.properties 文件。使用 Spring Roo 的 database properties list 命令的整体思路是让开发人员可以在不切换到 IDE 的情况下查看数据库属性。

### 另请参阅

- 下一个配方“管理数据库配置属性”展示了您如何添加、删除和修改 Roo 项目中 database.properties 文件中定义的属性。

## 管理数据库配置属性

在上一个配方中，我们了解了如何使用 database properties list 命令查看 database.properties 文件中定义的数据库配置属性。在本配方中，我们将了解如何使用 database properties set 和 database properties remove 命令从 database.properties 文件中添加、修改或删除属性。

以下表格显示了我们将从 database.properties 文件中添加、修改和删除的属性：

| 属性 | 操作 |
| --- | --- |
| database.username | 修改为 database.username = root |
| database.password | 修改为 database.password = asarin |
| database.url = jdbc:mysql://localhost:3306/myFlightAppDB | 从 database.properties 中移除 |
| database.modified.url = jdbc:mysql://localhost:3406/myFlightAppDB | 添加到 database.properties |
| database.initialPoolSize=10 | 添加到 database.properties |

### 准备工作

请参阅“为您的项目设置 JPA 提供程序”配方，以创建 flight-app Roo 项目，并使用 persistence setup 命令设置持久性提供程序。

从 C:\roo-cookbook\ch02-recipes 目录启动 Roo shell。

> 如果您在 Roo 项目中使用的是 JNDI 绑定的数据源，则不需要本配方。

### 操作方法

以下步骤将向您展示如何添加、修改或删除属性：

- 1. 如果您想要修改 database.properties 文件中定义的属性，database properties set 命令非常有用，如下所示：
  ```
  roo> database properties set --key database.username --value root
  roo> database properties set --key database.password --value asarin
  ```

- 2. database properties set 命令也可以用于向 database.properties 文件中添加新属性，如下所示：
  ```
  roo> database properties set --key database.modified.url --value jdbc:mysql://localhost:3406/myFlightAppDB
  roo> database properties set --key database.initialPoolSize --value 10
  ```

- 3. database properties remove 命令用于从 database.properties 文件中删除现有属性，如下所示：
  ```
  roo> database properties remove --key database.url
  ```

### 工作原理

database properties set 和 database properties remove 命令由 JPA 加载项处理。除了使用这些命令，您还可以使用 properties set 和 properties remove 命令，如第 1 章“Spring Roo 入门”中的“管理属性文件中定义的属性”配方所示。

需要注意的是，如果您修改了 database.properties 文件中的属性名称，则这些修改必须反映在 Roo 项目的 applicationContext.xml 文件中定义的 dataSource bean 中。

### 更多内容

除了使用 Spring Roo，您还可以使用 Eclipse 或 STS 等 IDE 修改 `database.properties` 文件。使用 Roo 命令修改 `database.properties` 文件允许开发人员使用 `script` 命令重播从 Roo shell 执行的操作。

### 另请参阅

- 请参阅本章前面描述的“查看数据库配置属性”配方，了解如何使用 Roo 命令查看 `database.properties` 文件中定义的属性。

## 创建持久实体

在本配方中，我们将了解 Spring Roo 如何使用 `entity` 和 `field` 命令简化 JPA 实体的创建。在本配方中，我们将创建一个具有复合主键的 `Flight` JPA 实体。请参阅第 3 章“Spring Roo 中的高级 JPA 支持”中的“在实体之间创建多对一关系”配方，了解如何创建带有代理键的持久实体。

以下图表显示了 `Flight` 实体及其复合主键 (`FlightKey`) 的属性：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_69_0.png)

### 准备工作

退出 Roo shell 并删除 `C:\roo-cookbook\ch02-recipes` 目录的内容。

执行 `ch02_jpa_setup.roo` 脚本。它会创建一个 `flight-app` Roo 项目，并使用 `persistence setup` 命令将 Hibernate 设置为持久性提供程序。如果您使用的数据库不是 MySQL，或者连接设置与脚本中指定的不同，请相应地修改脚本。

从 C:\roo-cookbook\ch02-recipes 目录启动 Roo shell。

随本书附带的 ch02_persistent_entities.roo 脚本会创建 flight-app 项目，将 Hibernate 设置为持久性提供程序，修改 database.properties 文件中的 database.username 和 database.password 属性，并执行本配方中所示的命令。

### 操作方法

以下步骤将演示如何创建持久实体：

- 1. 使用 `entity` 命令在 `sample.roo.flightapp.domain` 包中创建 Flight 实体：
  ```
  roo> entity --class ~.domain.Flight --identifierType ~.domain.FlightKey --table FLIGHT_TBL
  ```

- 2. 使用 `field` 命令向 `Flight` 实体添加字段：
  ```
  roo> field number --type java.lang.Integer --fieldName numOfSeats
  roo> field string --fieldName origin
  roo> field string --fieldName destination
  roo> field date --type java.util.Date --fieldName createdDate
  roo> field date --type java.util.Date --fieldName modifiedDate
  roo> field string --fieldName createdBy
  roo> field string --fieldName modifiedBy
  ```

- 3. 将焦点切换到 `FlightKey` 类（在步骤 1 中创建 Flight 实体时，该类在 `sample.roo.flightapp.domain` 包中自动生成）：
  ```
  roo> focus --class ~.domain.FlightKey
  ```

- 4. 向 `FlightKey` 类添加 `flightId` 和 `departureDate` 字段：
  ```
  roo> field string --fieldName flightId
  roo> field date --fieldName departureDate --type java.util.Date
  ```

出于简洁性考虑，`entity` 命令的输出未在上方显示。我们将在 *操作方法...* 部分讨论与 JPA 实体对应生成的重要 ITD 文件。

