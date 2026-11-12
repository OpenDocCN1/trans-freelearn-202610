
如果要添加自定义查找器方法，且其名称或签名与 Roo 提供的动态查找器方法不同，则可以在持久实体 Java 类中定义该方法，也可以创建新的 AspectJ ITD 并在其中声明你的方法。这两种方法都可以，具体取决于你编写 AspectJ ITD 的熟悉程度。Roo 生成 AspectJ ITD，以便将横切关注点与 Java 类分离。因此，如果你觉得在持久实体 Java 类中编写查找器方法会污染该类，则应该考虑编写 AspectJ ITD。需要注意的是，你在项目中创建的自定义 AspectJ ITD 不由 Roo 管理。

以下代码展示了 Flight_MyCustom_Finder.aj AspectJ ITD 的示例，它将 searchFlights(SearchCriteria criteria) 查找器方法引入 Flight.java 中：

```
import javax.persistence.EntityManager;
import javax.persistence.TypedQuery;

privileged aspect Flight_MyCustom_Finder {

    public static TypedQuery<Flight> Flight.searchFlight (
            SearchCriteria criteria) {
        EntityManager em = Flight.entityManager();
        TypedQuery<Flight> q = em.createQuery("SELECT Flight ..",
                Flight.class);
        q.setParameter("destination", criteria.getDestination());
        q.setParameter("origin", criteria.getOrigin());
        return q;
    }

}
```

如给定代码所示，你可以创建自己的 AspectJ ITD 并向 JPA 实体添加自定义查找器方法。由于自定义 AspectJ ITD 不由 Roo 管理，如果在 Flight.java 类中创建 searchFlight 方法，Roo 不会从 Flight_MyCustom_Finder.aj 文件中将其移除。

## 动态查找器方法的集成测试

Roo 不会为自动生成的动态查找器方法创建集成测试。要测试查找器方法，请在持久实体对应的 *IntegrationTest.java 文件中编写测试方法。

## 通过 @RooEntity 注释添加动态查找器

@RooEntity 注释接受 finders 属性，该属性包含字符串值数组，用于标识必须为持久实体生成的动态查找器方法名称。finders add 命令将查找器方法名称添加到 finders 属性中，从而导致 Roo 生成动态查找器方法。由于将查找器方法名称添加到 finders 属性会触发动态查找器方法实现的生成，因此不使用 finder add 命令，你可以直接使用 IDE 将查找器方法名称添加到 finders 属性中，这将触发 Roo 生成查找器方法实现。

## 另请参阅

- 请参阅第 2 章（使用 JPA 持久化对象）中“控制持久实体自动生成的方法”配方，了解 @RooEntity 注释支持的更多元素
- 请参阅“查看候选动态查找器方法”配方，了解如何使用 finder list 命令显示候选动态查找器方法的名称

## 在实体之间创建多对一（或一对一）关系

在实际应用中，域实体之间存在关系。在本节中，我们将探讨 Roo 如何简化在 JPA 实体之间创建多对一（或一对一）关系。

以下图示展示了 FLIGHT_TBL 和 FLIGHT_DESC_TBL 表之间的关系，我们将以此作为建模多对一关系的参考：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_118_0.png)

在给定的图中，FLIGHT_TBL 表包含计划航班详细信息，FLIGHT_DESC_TBL 表包含航空公司提供的所有航班详细信息。FLIGHT_TBL 表中的每条记录都指向恰好一条 FLIGHT_DESC_TBL 记录。由于一个城市到另一个城市可能有多个航班，因此 FLIGHT_TBL 和 FLIGHT_DESC_TBL 之间的关系是多对一。FLIGHT_TBL 表通过 FLIGHT_DESC_ID 外键映射到 FLIGHT_DESC_TBL 表。预期如果删除 FLIGHT_TBL 记录，则删除仅限于 FLIGHT_TBL。

以下图示展示了与 FLIGHT_TBL 和 FLIGHT_DESC_TBL 表对应的 JPA 实体：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_119_0.png)

在给定的图中，flightId 和 flightDescId 属性分别表示 Flight 和 FlightDescription JPA 实体的主键。

## 准备工作

退出 Roo shell，删除 C:\roo-cookbook\ch03-recipes 目录的内容。

从 C:\roo-cookbook\ch03-recipes 目录启动 Roo shell。

执行 ch03_jpa_setup.roo 脚本，该脚本创建 flight-app Roo 项目，将 Hibernate 设置为持久化提供程序，并将 MySQL 配置为应用程序的数据库。如果你使用的数据库不是 MySQL，或者连接设置与脚本中指定的不同，则相应地修改脚本。

## 操作步骤

要创建具有多对一关系的 Flight 和 FlightDescription JPA 实体，请执行以下步骤：

- 1. 创建与 FLIGHT_TBL 表对应的 Flight JPA 实体（包括集成测试）：
  ```
  roo> entity --class ~.domain.Flight --identifierColumn FLIGHT_ID
  --identifierField flightId --identifierType java.lang.Long
  --table FLIGHT_TBL --testAutomatically
  ```

- 2. 向 Flight 实体添加属性：
  ```
  roo> field date --type java.util.Date --fieldName departureDate --column DEPARTURE_DATE
  roo> field date --type java.util.Date --fieldName arrivalDate --column ARRIVAL_DATE
  ```

- 3. 创建与 FLIGHT_DESC_TBL 表对应的 FlightDescription 实体（包括集成测试）：
  ```
  roo> entity --class ~.domain.FlightDescription
  --identifierColumn FLIGHT_DESC_ID --identifierField flightDescId
  --identifierType java.lang.Long --table FLIGHT_DESC_TBL
  --testAutomatically
  ```

- 4. 向 FlightDescription 实体添加属性：
  ```
  roo> field string --fieldName origin --column ORIGIN_CITY --notNull
  roo> field string --fieldName destination --column DESTINATION_CITY --notNull
  roo> field number --type java.lang.Float --fieldName price --column PRICE --notNull
  ```

- 5. 将 Roo 命令的焦点设置到 Flight 实体，并使用 field reference 命令在 Flight 和 FlightDescription 实体之间创建多对一关系，如下所示：
  ```
  roo> focus --class ~.domain.Flight
  roo> field reference --fieldName flightDescription --type ~.domain.FlightDescription --joinColumnName FLIGHT_DESC_ID --notNull
  ```

## 工作原理

由于我们未为实体使用复合主键，在使用用于创建 Flight 和 FlightDescription 实体的实体命令时，你会注意到以下几点：

- identifierField：使用了一个可选参数，为实体的标识符字段指定自定义名称。例如，Flight 实体的标识符字段名为 flightId，FlightDescription 实体的标识符字段名为 flightDescId。如果未使用 identifierField 参数，则默认情况下 Roo 会将 id 分配为实体的标识符字段名。

- identifierColumn：使用了一个可选参数，用于设置标识符字段映射到的表列名称。该参数指示 Roo 向标识符字段添加 @Column JPA 注释。如果未指定 identifierColumn，默认情况下，映射列的名称是通过拆分驼峰命名并添加下划线作为分隔符来得出的。例如，如果为名为 myOwnIdField 的标识符字段未指定 identifierColumn 参数，则表列名默认为 my_own_id_field。在本配方的上下文中，identifierColumn 不会产生任何影响，因为从标识符字段名得出的表列名与 identifierColumn 参数指定的名称相同。
