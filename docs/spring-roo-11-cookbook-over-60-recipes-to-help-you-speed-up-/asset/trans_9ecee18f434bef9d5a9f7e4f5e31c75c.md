

| 参数 | 描述 |
| :--- | :--- |
| username | 标识连接数据源所需的用户名。仅当企业应用程序未使用JNDI绑定的数据源时，此参数才有用。 |
| password | 标识连接数据源所需的密码。仅当企业应用程序未使用JNDI绑定的数据源时，此参数才有用。 |
| applicationId | 如果要将应用程序部署到Google App Engine，则标识应用程序标识符。我们将在第6章的“在GAE上部署GWT应用程序”配方中详细讨论此参数。 |
| persistenceUnit | 指定Roo生成的persistence.xml文件中要使用的持久化单元名称。如果您的应用程序与多个数据库交互，则必须使用此参数。 |
| transactionManager | **[自Spring Roo 1.1.5起支持]** 与持久化单元对应的事务管理器名称。如果您的应用程序与多个数据库交互，则必须使用此参数。 |

如`persistence setup`命令的输出所示，我们的flight-app Roo项目中以下文件被创建或修改：

- `persistence.xml`: 这是一个新创建的文件，由JPA提供程序用于发现持久化提供程序信息，在flight-app项目中，该提供程序是Hibernate。
- `database.properties`: 这是一个新创建的文件，包含数据源信息，例如用户名、密码、数据源URL和驱动程序类。
- `applicationContext.xml`: 此文件在我们创建Roo项目时创建。此文件现已修改，以包含数据源、事务管理器和JPA EntityManagerFactory定义。本节稍后将详细介绍这些定义。
- `pom.xml`: 此文件在我们创建Roo项目时创建。它现在已修改，以包含MySQL连接器、Hibernate、Hibernate Validator等的项目依赖项。这表明Roo仅在您为企业应用程序添加额外功能时才向项目添加依赖项。例如，如果您未使用JPA，则除非执行`persistence setup`命令，否则JPA相关依赖项不会添加到您的项目中。

以下XML片段显示了由Spring Roo创建的persistence.xml文件：

```
<persistence ...>
<persistence-unit name="persistenceUnit"
transaction-type="RESOURCE_LOCAL">
<provider>org.hibernate.ejb.HibernatePersistence</provider>
<properties>
<property name="hibernate.dialect"
value="org.hibernate.dialect.MySQL5InnoDBDialect"/>
<property name="hibernate.hbm2ddl.auto" value="create"/>
...
</properties>
</persistence-unit>
</persistence>
```

前面的清单显示，Roo根据您提供给persistence setup命令的JPA提供程序和数据库信息创建persistence.xml文件。<persistence-unit>元素的name属性指定持久化单元名称。如果未指定persistence setup的persistenceUnit参数，则默认情况下，Roo将name属性值设置为persistenceUnit。hibernate.hbm2ddl.auto属性的create值表示每次创建Hibernate SessionFactory时，都会重新创建数据库。您可能希望根据JPA实体中提供的映射来更改hibernate.hbm2ddl.auto属性的值，将其从create更改为validate或update或create-drop，具体取决于您希望Hibernate如何管理数据库架构。

此外，如果您的企业应用程序更新多个数据源，则应将persistence-unit元素的transaction-type属性值设置为JTA（Java事务API），而不是RESOURCE_LOCAL。

以下代码显示了执行persistence setup命令时添加到applicationContext.xml文件的元素：

```
<bean class="org.apache.commons.dbcp.BasicDataSource"
destroy-method="close" id="dataSource">
<property name="driverClassName"
value="${database.driverClassName}"/>
<property name="url" value="${database.url}"/>
<property name="username" value="${database.username}"/>
<property name="password" value="${database.password}"/>
...
</bean>

<bean
class="org.springframework.orm.jpa.JpaTransactionManager"
id="transactionManager">
<property name="entityManagerFactory"
              ref="entityManagerFactory"/>
</bean>
```

```
<tx:annotation-driven mode="aspectj"
              transaction-manager="transactionManager"/>
```

```
<bean
    class="org.springframework.orm.jpa.
    LocalContainerEntityManagerFactoryBean"
    id="entityManagerFactory">
    <property name="dataSource" ref="dataSource"/>
</bean>
```

如前面的XML所示，persistence setup命令添加的其他bean如下：

- dataSource: 指javax.sql.DataSource对象，表示应用程序的数据源。数据源的属性包含在persistence setup创建的database.properties文件中。由于applicationContext.xml文件中存在<property-placeholder>元素，Spring的应用程序上下文会读取database.properties文件（请参阅第1章中的“创建Roo项目”配方）。
- entityManagerFactory: 指Spring的工厂bean，负责创建JPA EntityManagerFactory。您应该使用LocalContainerEntityManagerFactoryBean，因为它对EntityManagerFactory的配置提供了最大控制权。
- transactionManager: 指JpaTransactionManager bean，如果您的应用程序使用单个EntityManagerFactory（即仅使用单个数据库或事务资源），则此bean非常合适。

Spring事务架构（spring-tx-3.0.xsd）的<annotation-driven>元素建议您使用Spring的@Transactional注解将方法标记为事务性。mode属性的值aspectj表示AspectJ事务切面在加载时或编译时被织入类中。如果您想使用Spring的AOP框架为@Transactional注解的bean（通常是您的服务类）创建代理，则请将mode属性的值指定为proxy，或者根本不指定mode属性。

如果保持mode属性值为aspectj（建议这样做），则应注意以下要求：

- 为您的应用程序启用编译时（或加载时）织入。您无需担心此问题，因为如果将Roo项目打包为WAR或JAR（请参阅本章中的“打包您的Roo项目”配方），则将使用AspectJ编译器Maven插件（请参阅第1章中的“创建Roo项目”配方）来将AspectJ事务切面（定义在spring-aspects.jar文件中）织入带有@Transactional注解的Roo方法中。
- 在具体类上使用@Transactional注解，而不是在接口上使用。

## 更多内容...

现在，让我们看看如何在Spring Roo生成的应用程序中使用JNDI绑定的数据源。

## JNDI绑定的数据源

在大多数实际应用程序中，javax.sql.DataSource对象是从JNDI获取的，而不是从属性文件中定义的属性创建的。要访问JNDI绑定的数据源，请不要依赖Spring为应用程序创建DataSource，而是使用persistence setup的jndiDataSource参数来指定DataSource的JNDI名称，如下所示：

```
persistence setup --provider HIBERNATE --database MYSQL --jndiDataSource jdbc/accountDB
```

如果指定了jndiDataSource参数，则Spring Roo将Spring的jee架构的jndi-lookup元素添加到applicationContext.xml文件中，如下所示：

```
<beans ... xmlns:jee="http://www.springframework.org/schema/jee" ...
xsi:schemaLocation="http://www.springframework.org/schema/jee http://
www.springframework.org/schema/jee/spring-jee-3.0.xsd">
.....
<jee:jndi-lookup id="dataSource" jndi-name="jdbc/accountDB" />
.....
```

如上所示，jndi-lookup元素负责访问名为jdbc/accountDB（由jndi-name属性引用）的JNDI中配置的DataSource，并使其作为bean id为dataSource在Spring的应用程序上下文中可用。

如果将前面的applicationContext.xml文件与先前显示的文件进行比较，您可以发现唯一的区别是dataSource bean如何提供给Spring的应用程序上下文。

