
> In the given code, the `FlightDescriptionDataOnDemand` object is autowired into the `FlightDataOnDemand` object. As the `Flight` entity has a many-to-one relationship with the `FlightDescription`, data on demand class of the `Flight` entity uses the data on demand class of the `FlightDescription` to obtain a reference to an existing `FlightDescription` instance and set it in the `Flight` instance. This ensures that the test data created during integration testing of the many-to-one relationship is as per the relationship that exists between related entities.

It is important to note that `@NotNull` and `@ManyToOne(..., optional=false)` mean the same thing. It is possible to specify that a relationship between entities must exist either by using `@NotNull` (JSR 303: Bean Validation annotation) or by specifying `false` as the value of the `optional` element of the `@ManyToOne` JPA annotation. In either case, the data on demand class of the entity will ensure that the data on demand class of the related entity is used to enforce a foreign key constraint on the test data created for integration testing.

### Dynamic finder method for a many-to-one relationship field

As with other persistent fields, you can use the `finder list` and `finder add` commands to view and add finder method(s) corresponding to the relationship field. For instance, if you execute the `finder list` command against the `Flight` entity, then one of the finders is for the `flightDescription` relationship field, as shown here:

```
~.domain.Flight roo> finder list
```

```
..
```

```
findFlightsByFlightDescription(FlightDescription flightDescription)
```

Also, you can use the `depth` argument of the `finder list` command to view dynamic finder methods based on the relationship field and other persistent fields of the entity.

### See also

- The Creating a one-to-many (or many-to-many) relationship between entities recipe that follows

## Creating a one-to-many (or many-to-many) relationship between entities

In the previous recipe, we saw how the many-to-one (or one-to-one) relationship is established between entities using the `field reference` command. In this recipe, we'll extend the same example to show how a one-to-many (or many-to-many) relationship can be created using the `field set` command. The following class diagram shows a one-to-many relationship between the `FlightDescription` and `Flight` entities:

![](img/b8c379a2ed921ead3aa2b01811dc29fa_126_0.png)

In the given figure, you'll notice that we have added a `flights` field of type `Set<Flight>` to the `FlightDescription` entity, reflecting the one-to-many relationship between the `FlightDescription` and the `Flight` entity.

### Getting ready

Exit the Roo shell and delete the contents of the `C:\roo-cookbook\ch03-recipes` directory.

Start the Roo shell from the `C:\roo-cookbook\ch03-recipes` directory.

Execute the `ch03_relationship_many_to_one.roo` script, which creates the `flight-app` Roo project, sets up Hibernate as a persistence provider, configures MySQL as the database for the application, creates the `Flight` and `FlightDescription` JPA entities, and defines a many-to-one relationship between the `Flight` and `FlightDescription` entities. If you are using a different database than MySQL or your connection settings are different than what is specified in the script, then modify the script accordingly.

### How to do it...

To create a one to many (or many to many) relationship between the `FlightDescription` and `Flight` entities, follow the steps given here:

- 1. Set the focus of the Roo commands on the `FlightDescription` entity:

```
.. roo> focus --class ~.domain.FlightDescription
```

- 2. Create a one-to-many relationship between the FlightDescription and Flight entities using the field set command:

```
~.domain.FlightDescription roo> field set --fieldName flights --type ~.domain.Flight --cardinality ONE_TO_MANY --mappedBy flightDescription
```

### How it works...

The field set command is used to create the many side of a one-to-many JPA relationship within an entity. If the relationship between two entities is many-to-many, then the field set command is used for both the entities in the relationship. The following table describes some of the important arguments that can be specified for the field set command:

| Argument | Description |
| :--- | :--- |
| fieldName (mandatory) | Name of the relationship field that you want to add to the entity. The type of the field is java.util.Set. In case of our example, the fieldName argument is specified as flights, which results in an additional field named flights to the FlightDescription entity. |
| type (mandatory) | Identifies the type of the related entity (which is on the many side of a one-to-many relationship) contained in the Set defined by the fieldName argument. In our example, the type value is specified as ~.domain.Flight, which means that the Set (defined by the fieldName argument) contains elements of type Flight, as shown here: private Set<Flight> flights = new HashSet<Flight>() ; |
| mappedBy | The value of the mappedBy argument refers to the owner of the relationship. It translates into the value of the mappedBy element of the JPA @OneToMany or @ManyToMany annotation. In our example, the value of the mappedBy argument is flightDescription; therefore, the flights field created by the field set command is annotated with a @OneToMany annotation whose mappedBy attribute value is flightDescription. |
| cardinality | Cardinality of the relationship between JPA entities. By default, cardinality is MANY_TO_MANY. As we want to create a one-to-many relationship between the FlightDescription and Flight entity, the value of the cardinality argument is specified as ONE_TO_MANY. |
| fetch | The JPA fetch strategy for related entity. The possible values are EAGER and LAZY. The value of the fetch argument translates into the value of the fetch element of the @ManyToMany or @OneToMany annotation. |

> Even though the Roo shell displays MANY_TO_ONE, MANY_TO_MANY, ONE_TO_MANY, and ONE_TO_ONE as possible values of the cardinality argument of the field set command, it only accepts MANY_TO_MANY and ONE_TO_MANY. The reason for this--field set only makes sense in the case of a many-to-many relationship or when the entity is on the one side of a one-to-many relationship.

The execution of the field set command against the FlightDescription entity results in the following field added to it:

```
@OneToMany(cascade = CascadeType.ALL,
       mappedBy="flightDescription")
private Set<Flight> flights = new HashSet<Flight>() ;
```

The given code shows that the field set command simply adds a relationship field to the entity.

### There's more...

field set and field reference commands don't provide the option to specify the cascade effect that applies to the related entity. The behavior of the commands is as described here:

- @ManyToMany or @OneToMany annotated JPA relationship field created using the field set command has the value of the cascade element as CascadeType.ALL, which means the entity operations such as refresh, persist, merge, and so on, are propagated to a related entity.
- @ManyToOne and @OneToOne annotated JPA relationship field created using the field reference command don't have the cascade element specified, which means entity operations such as refresh, persist, merge, and so on, are not propagated to an associated entity.

In scenarios where you want to specify the cascade effect, you can modify the corresponding JPA annotation in your Java source file.

### See also

- Refer to the Creating a many-to-one (or one-to-one) relationship between entities recipe to see how a field reference command is used for a creating many-to-one (or one-to-one) relationship

## Creating a mapped superclass

In this recipe, we'll look at how to create an entity which inherits a *mapped superclass*. The fields of a mapped superclass are stored into the table to which the inheriting entity is mapped. The mapped superclass itself is *not* mapped to any table. In this recipe, we'll create the *Flight* entity, which extends the *AuditFields* class. The *AuditFields* class represents a *mapped superclass*; the fields of *AuditFields* are stored into the table to which the *Flight* entity is mapped.

The following figure shows the relationship between the *Flight* entity and the *AuditFields* mapped superclass:

![](img/b8c379a2ed921ead3aa2b01811dc29fa_129_0.png)

### Getting ready

Exit the Roo shell and delete the contents of the `C:\roo-cookbook\ch03-recipes` directory.

Start the Roo shell from the `C:\roo-cookbook\ch03-recipes` directory.

Execute the `ch03_jpa_setup.roo` script which creates the *flight-app* Roo project, sets up Hibernate as persistence provider, and configures MySQL as the database for the application. If you are using a different database than MySQL or your connection settings are different than what is specified in the script, then modify the script accordingly.

### How to do it...

To create an entity which inherits a *mapped superclass*, follow the given steps:

- 1. Create the *AuditFields* class using the `entity` command and add fields to it:

```
.. roo> entity --class ~.domain.AuditFields --mappedSuperclass
```
