
## Creating applications that interact with multiple databases

As of Spring Roo 1.1.3, both entity and persistence setup commands support the persistenceUnit argument which lets you create enterprise applications which interact with multiple databases. In this recipe we'll create two persistent units:

- flight: the flight persistence unit consists of a single entity, Flight. It uses Hibernate as a JPA provider and maps to a MySQL database named "myFlightDB".
- payment: the payment persistence unit consists of a single entity, Payment. It uses Hibernate as the JPA provider and maps to a MySQL database named "myPaymentDB".

## Getting ready

Exit the Roo shell and delete the contents of the C:\roo-cookbook\ch02-recipes directory.

Start the Roo shell from the C:\roo-cookbook\ch02-recipes directory.

## How to do it...

The following steps will demonstrate how to create an application that interacts with multiple databases:

- 1. Create the flight-app Roo project:
  roo> project --topLevelPackage sample.roo.flightapp --java 6 --projectName flight-app

- 2. Set up the flight persistence unit:
  roo> persistence setup --provider HIBERNATE --database MYSQL --databaseName myFlightDB --persistenceUnit flight

- 3. Create Flight entity, which is associated with the flight persistence unit:
  roo> entity --class ~.domain.Flight --table FLIGHT_TBL --persistenceUnit flight

- 4. Add some fields to the Flight entity:
  ~.domain.Flight roo> field string --fieldName origin --column FLT_ORIGIN --notNull
  ~.domain.Flight roo> field string --fieldName destination --column FLT_DESTINATION --notNull

- 5. Create an integration test for the `Flight` entity:
  `~.domain.Flight roo> test integration`

- 6. Set up the `payment` persistence unit:
  roo> persistence setup --provider HIBERNATE --database MYSQL --databaseName myPaymentDB --persistenceUnit payment

- 7. Create the `Payment` entity, which is associated with the `payment` persistence unit:
  roo> entity --class ~.domain.Payment --table PAYMENT_TBL --persistenceUnit payment

- 8. Add fields to the `Payment` entity:
  `~.domain.Payment roo> field string --fieldName paymentType --column PYMT_TYPE --notNull`

- 9. Create an integration test for the `Payment` entity:
  `~.domain.Payment roo> test integration`

- 10. Execute the `perform eclipse` command to import the `flight-app` project into Eclipse IDE:
  `~.domain.Payment roo> perform eclipse`

- 11. Execute the integration tests:
  roo> perform tests

Executing the integration tests at this time will result in failure. We'll shortly see what we need to do to get the tests working when using multiple databases.

## How it works...

The concept of a persistence unit is not only useful when the enterprise application interacts with multiple databases but also when you want to logically group entities in your application. Using different persistence units can also be useful if you want to use different persistence providers for different logical groups of entities.

When setting up a persistence provider using the `persistence setup` command you can specify the persistence unit name by specifying the `persistenceUnit` argument. Also, when creating entities using the `entity` command you can use the `persistenceUnit` argument to specify the persistence unit to which the entity belongs. Spring Roo makes use of the `persistenceUnit` argument of the `persistence setup` command to define a different persistence unit in the `/META-INF/persistence.xml` file. The following listing shows the `persistence.xml` file of the `flight-app` project after the execution of the `persistence setup` commands:

```
<persistence ...>
<persistence-unit name="flight"
transaction-type="RESOURCE_LOCAL">
<provider>
org.hibernate.ejb.HibernatePersistence
</provider>
..
</persistence-unit>
<persistence-unit name="payment"
transaction-type="RESOURCE_LOCAL">
<provider>
org.hibernate.ejb.HibernatePersistence
</provider>
..
</persistence-unit>
</persistence>
```

In the code, the name attribute of the `<persistence-unit>` element reflects the persistenceUnit argument value that you specified in the persistence setup command. As we have specified Hibernate as the JPA provider for both flight and payment persistent units, the `<provider>` element contains org.hibernate.ejb.HibernatePersistence as the JPA provider. If you want to use different JPA providers for your persistence unit, then specify it using the provider argument of the persistence setup command.

As we are using different persistence units, the entities created using the entity command use the persistenceUnit attribute to identify the persistence unit with which the entity is associated. The following code listing shows that the persistenceUnit argument value is used in the @RooEntity annotation of the Flight class:

```
@RooJavaBean
@RooToString
@RooEntity(persistenceUnit = "flight", table = "FLIGHT_TBL")
public class Flight {...}
```

In the code, the value of the persistenceUnit attribute of @RooEntity is flight, which affects the way Roo generates the corresponding *_Roo_Entity.aj AspectJ ITD file. The following code listing shows the affect of persistenceUnit attribute of @RooEntity annotation on the Flight_Roo_Entity.aj ITD file:

```
privileged aspect Flight_Roo_Entity {
@PersistenceContext(unitName = "flight")
transient EntityManager Flight.entityManager;
...
@Transactional("flight")
public void Flight.persist() {...}
@Transactional("flight")
public void Flight.remove() {...}
...
}
```

In the code, the @PersistenceContext annotation makes use of the unitName attribute to specify the persistence unit with which the EntityManager persistence context is associated with. Also, notice that the @Transactional annotation now makes use of the flight qualifier to specify the transaction manager required for managing transactions.

As of Spring Roo 1.1.3, you'll have to ensure that the transaction manager, entity manager factory, and data source bean definitions for each persistence unit are configured in the /META-INF/spring/applicationContext.xml file, as shown here:

```
<bean class="org.apache.commons.dbcp.BasicDataSource"
 destroy-method="close" id="flightDataSource">
 ...
</bean>
<bean class="org.apache.commons.dbcp.BasicDataSource"
 destroy-method="close" id="paymentDataSource">
 ...
</bean>

<bean
 class="org.springframework.orm.jpa.JpaTransactionManager"
 id="flightTransactionManager">
 <qualifier value="flight"/>
 <property name="entityManagerFactory"
 ref="flightEntityManagerFactory"/>
</bean>

<bean
 class="org.springframework.orm.jpa.JpaTransactionManager"
 id="paymentTransactionManager">
 <qualifier value="payment"/>
 <property name="entityManagerFactory"
 ref="paymentEntityManagerFactory"/>
</bean>

<tx:annotation-driven mode="aspectj"
 transaction-manager="flightTransactionManager"/>
<tx:annotation-driven mode="aspectj"
 transaction-manager="paymentTransactionManager"/>

<bean
    class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean"
    id="flightEntityManagerFactory">
    <property name="persistenceUnitName" value="flight"/>
    <property name="dataSource" ref="dataSource"/>
</bean>
<bean class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean"
    id="paymentEntityManagerFactory">
    <property name="persistenceUnitName" value="payment"/>
    <property name="dataSource" ref="dataSource"/>
</bean>
```

The code shows that different transaction managers, entity manager factories, and data source beans are configured for each persistence unit. The transaction manager bean definitions make use of the <qualifier> element, so that @Transactional annotations can refer to the target transaction manager using a qualifier. Also, the LocalContainerEntityManagerFactoryBean is passed the persistence unit name with which it is associated, using the persistenceUnitName property.

To ensure that integration tests work, you'll also need to specify the transaction manager to use for the test methods annotated with the @Transactional annotation. To achieve this, all you need to do is specify the @Transactional annotation in your test class, as shown here for PaymentIntegrationTest class:

```
@RooIntegrationTest(entity = Payment.class)
@Transactional("payment")
public class PaymentIntegrationTest {

    @Test
    public void testMarkerMethod() {
    }

}
```

## See also

- Refer to the Creating persistent entities recipe to see how to create persistent entities which belong to a single persistence unit
