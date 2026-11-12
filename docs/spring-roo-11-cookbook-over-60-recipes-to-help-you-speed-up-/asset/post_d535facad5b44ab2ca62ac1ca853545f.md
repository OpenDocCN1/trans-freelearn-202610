
The following code snippet from the `Flight.java` file shows the `Flight` entity that was created in this recipe:

```
@RooJavaBean
@RooToString
@RooEntity(identifierField = "flightId", identifierColumn = "FLIGHT_ID", table = "FLIGHT_TBL")
public class Flight {
    ...
}
```

The given code shows that the `@RooEntity` annotation contains the `identifierField`, `identifierColumn`, and the `table` attributes with values that we specified for these arguments in the entity command. As mentioned in the *Creating persistent entities using JPA* recipe of *Chapter 2, Persisting Objects Using JPA* the elements of the `@RooEntity` annotation are used for generating the corresponding `*_Roo_Entity.aj` AspectJ ITD. The following code shows the identifier field as defined in the `Flight_Roo_Entity.aj` AspectJ ITD:

```
privileged aspect Flight_Roo_Entity
{
    .....
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "FLIGHT_ID")
    private Long Flight.flightId;
    .....
}
```

In this code, the identifier field is annotated with the `@Column` annotation because of the presence of the `identifierColumn` argument in the `@RooEntity` annotation. The name `flightId` of the identifier field is derived from the value of the `identifierField` argument of the `@RooEntity` annotation.

Now, coming to the `field reference` command that is used for adding an attribute referring to another object in a relationship. In our next example, the `field reference` command adds the reference to the `FlightDescription` object in the `Flight` class. As only a reference to a related object is defined using `field reference`, you can consider that it defines *many* sides of a *many-to-one* relationship.

The `field reference` command is similar to the `field other` (described in *Adding attributes to a Java class* recipe of *Chapter 1, Getting Started with Spring Roo*) command because both the commands are used for defining a reference to custom Java objects. The only difference is that the `field reference` command is specifically meant for defining JPA relationships between entities. The following code from `Flight.java` shows the code introduced by executing the `field reference` command:

```
@NotNull
@ManyToOne
@JoinColumn(name = "FLIGHT_DESC_ID")
private FlightDescription flightDescription;
```

As we can see from the given code, the field reference command has added a many-to-one JPA relationship between the Flight and FlightDescription entities.

The following table describes the arguments that can be passed to the field reference command:

| Argument | Description |
|---|---|
| fieldName (mandatory) | Name of the attribute, which refers to the related entity. In our example, flightDescription is the name of the attribute. |
| type (mandatory) | Type of the related entity. In our example, FlightDescription is the type of the related entity. |
| joinColumnName | The column which acts as the *foreign key* for the related entity. In our example, the FLIGHT_DESC_ID is the column in the FLIGHT_TBL (represented by the Flight JPA entity), which acts as the foreign key to the FLIGHT_DESCRIPTION_TBL table (represented by the FlightDescription JPA entity). |
| cardinality | Cardinality of the relationship between JPA entities. By default, cardinality is MANY_TO_ONE. If you are creating a one-to-one relationship, then specify ONE_TO_ONE as the value of the cardinality argument. |
| fetch | JPA fetch strategy for related entity. The possible values are EAGER and LAZY. The value of the *fetch* argument translates into the value of the *fetch* element of a @ManyToOne or a @OneToOne annotation. |
| referencedColumnName | Identifies the column in the table of the related entity to which this join column links. The value of this argument is used as a value of the *referencedColumnName* attribute of the @JoinColumn JPA annotation. |

> Even though the Roo shell displays MANY_TO_ONE, MANY_TO_MANY, ONE_TO_MANY, and ONE_TO_ONE as possible values of the cardinality argument of the field reference command, it only accepts MANY_TO_ONE and ONE_TO_ONE. The reason for this is that field reference only makes sense in case of a one-to-one relationship or when the entity is on the many side of a many-to-one relationship.

## There's more...

### Testing JPA entities that participate in relationships

Testing entities that participate in relationships can get a bit tricky sometimes because integration tests may not be able to check relationship constraints. For instance, the testRemove method of FlightDescriptionIntegrationTest (refer to the FlightDescriptionIntegrationTest_Roo_IntegrationTest.aj AspectJ ITD) doesn't test for the scenario in which one or more Flight entity instances are associated with the FlightDescription instance being removed.

To effectively test entities that participate in relationships, you may want to modify auto-generated test methods and data on demand classes. For instance, in our example scenario, the data on demand class of the FlightDescription (refer to the getNewTransientFlightDescription method defined in the FlightDescriptionDataOnDemand_Roo_DataOnDemand.aj AspectJ ITD) only creates FlightDescription instances, and doesn't create any associated Flight instances. As a result of this, the testRemove method will never be able to test a scenario in which one or more Flight instances are associated with the FlightDescription instance being removed. So, to perform effective testing of the FlightDescription entity, you'll need to do the following:

- Define the getNewTransientFlightDescription method in the FlightDescriptionDataOnDemand.java class, which creates the FlightDescription objects that are associated with one or more Flight instances
- Define the testRemove method in the FlightDescriptionIntegrationTesting class to first remove all the related Flight entities, before attempting to remove the FlightDescription entity

If entities are not related, data on demand classes are independent of each other. But, if the entities are related, then creating test data becomes a bit of an involved task. For instance, the Flight entity has a many-to-one relationship with FlightDescription; which means that the data on demand class for the Flight entity should create records in the FLIGHT_TBL table, which have a foreign key reference to the FLIGHT_DESC_TBL table records. It is mandatory to assign a foreign key reference to the Flight records in the FLIGHT_TBL table because the FlightDescription relationship is annotated with @NotNull.

Now, the question is how the data on demand class of the `Flight` entity can discover the `FlightDescription` instances (that were created in the `FLIGHT_DESC_TBL` table by the `getNewTransientFlightDescription` method of the `FlightDescriptionDataOnDemand_Roo_DataOnDemand.aj` AspectJ ITD) so that the `Flight` instances (created by the `getNewTransientFlight` method of the `FlightDataOnDemand_Roo_DataOnDemand.aj` AspectJ ITD) in the `FLIGHT_TBL` can specify the foreign key reference to the `FlightDescription` instances? It’s simple! It can be done by using the data on demand class of the `FlightDescription` entity because it maintains the list of records that it created in the `FLIGHT_DESC_TBL` table. This is exactly what Spring Roo does while generating data on demand classes—it creates dependency between data on demand classes of related entities to create test data.

The following code from the `FlightDataOnDemand_Roo_DataOnDemand.aj` file shows the `Flight` data on demand class:

```
privileged aspect FlightDataOnDemand_Roo_DataOnDemand
{
    ...

    @Autowired
    private FlightDescriptionDataOnDemand flightDescriptionDataOnDemand;

    public Flight getNewTransientFlight(int index)
    {
        sample.roo.flightapp.domain.Flight obj =
            new sample.roo.flightapp.domain.Flight();

        obj.setArrivalDate(obj, index);
        obj.setDepartureDate(obj, index);
        obj.setFlightDescription(obj, index);
        return obj;
    }

    private void setFlightDescription(Flight obj, int index) {
        sample.roo.flightapp.domain.FlightDescription flightDescription = flightDescriptionDataOnDemand.getRandomFlightDescription();
        obj.setFlightDescription(flightDescription);
    }

    ...
}
```
