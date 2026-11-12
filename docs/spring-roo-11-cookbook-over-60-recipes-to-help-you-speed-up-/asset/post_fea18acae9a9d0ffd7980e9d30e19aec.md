
If you want to add a custom finder method whose name or signature is different from the dynamic finder methods offered by Roo, then you can either define the method in the persistent entity Java class or you can create a new AspectJ ITD and declare your method in it. Both these approaches are fine, and it depends upon how comfortable you are with writing AspectJ ITDs. Roo generates AspectJ ITDs so that cross-cutting concerns are separate from the Java classes. So, if you feel that writing finder methods in your persistent entity Java class pollutes it, you should consider writing AspectJ ITDs. It is important to note that the custom AspectJ ITDs that you create in your project are not managed by Roo.

The following code shows an example of the Flight_MyCustom_Finder.aj AspectJ ITD, which introduces the searchFlights(SearchCriteria criteria) finder method into Flight.java:

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

As the given code shows, you can create your own AspectJ ITD and add custom finder methods to JPA entities. As custom AspectJ ITDs are not managed by Roo, if you create the searchFlight method in the Flight.java class, Roo will not remove it from the Flight_MyCustom_Finder.aj file.

## Integration testing of dynamic finder methods

Roo doesn't create integration tests for the auto-generated dynamic finder methods. To test finder methods, write the test method in the *IntegrationTest.java file corresponding to the persistent entity.

## Adding dynamic finders through @RooEntity annotation

The @RooEntity annotation accepts a finders attribute, which contains an array of string values identifying names of dynamic finder methods that must be generated for the persistent entity. The finders add command adds the finder method name to the finders attribute, which in turn results in the generation of dynamic finder method by Roo. As the addition of finder method name to finders attribute triggers generation of dynamic finder method implementation, instead of using the finder add command you can directly add the name of the finder method to the finders attribute using IDE, which in turn will trigger Roo to generate the finder method implementation.

## See also

- Refer to the Controlling auto-generated methods of persistent entities recipe in Chapter 2, Persisting Objects Using JPA to know more about the elements supported by the @RooEntity annotation
- Refer to the Viewing candidate dynamic finder methods recipe to see how the finder list command is used to show the names of candidate dynamic finder methods

## Creating a many-to-one (or one-to-one) relationship between entities

In real-world applications, domain entities have relationships between them. In this section, we look at how Roo simplifies creating a many-to-one (or one-to-one) relationship between JPA entities.

The following figure shows the relationship between the FLIGHT_TBL and FLIGHT_DESC_TBL tables, which we will use as a reference to model our many-to-one relationship:

![](img/b8c379a2ed921ead3aa2b01811dc29fa_118_0.png)

In the given figure, the FLIGHT_TBL table contains scheduled flight details and the FLIGHT_DESC_TBL table contains details of all the flights that an airline offers. Each record in the FLIGHT_TBL table refers to exactly one FLIGHT_DESC_TBL record. As there can be multiple flights from one city to another, the relationship between FLIGHT_TBL and FLIGHT_DESC_TBL is many-to-one. The FLIGHT_TBL table is mapped to the FLIGHT_DESC_TBL table by the FLIGHT_DESC_ID foreign key. It is expected that if a FLIGHT_TBL record is deleted, then the deletion is limited to the FLIGHT_TBL only.

The following figure shows JPA entities corresponding to the FLIGHT_TBL and FLIGHT_DESC_TBL tables:

![](img/b8c379a2ed921ead3aa2b01811dc29fa_119_0.png)

In the given figure, the flightId and flightDescId attributes represent primary keys of the Flight and FlightDescription JPA entities, respectively.

## Getting ready

Exit the Roo shell and delete the contents of the C:\roo-cookbook\ch03-recipes directory.

Start the Roo shell from the C:\roo-cookbook\ch03-recipes directory.

Execute the ch03_jpa_setup.roo script which creates the flight-app Roo project, sets up Hibernate as the persistence provider, and configures MySQL as the database for the application. If you are using a different database than MySQL or your connection settings are different than what is specified in the script, then modify the script accordingly.

## How to do it...

To create the Flight and FlightDescription JPA entities with a many to one relationship, follow the steps given here:

- 1. Create the Flight JPA entity (including integration tests) corresponding to the FLIGHT_TBL table:
  ```
  roo> entity --class ~.domain.Flight --identifierColumn FLIGHT_ID
  --identifierField flightId --identifierType java.lang.Long
  --table FLIGHT_TBL --testAutomatically
  ```

- 2. Add attributes to the Flight entity:
  ```
  roo> field date --type java.util.Date --fieldName departureDate --column DEPARTURE_DATE
  roo> field date --type java.util.Date --fieldName arrivalDate --column ARRIVAL_DATE
  ```

- 3. Create the FlightDescription entity (including integration tests) corresponding to the FLIGHT_DESC_TBL table:
  ```
  roo> entity --class ~.domain.FlightDescription
  --identifierColumn FLIGHT_DESC_ID --identifierField flightDescId
  --identifierType java.lang.Long --table FLIGHT_DESC_TBL
  --testAutomatically
  ```

- 4. Add attributes to the FlightDescription entity:
  ```
  roo> field string --fieldName origin --column ORIGIN_CITY --notNull
  roo> field string --fieldName destination --column DESTINATION_CITY --notNull
  roo> field number --type java.lang.Float --fieldName price --column PRICE --notNull
  ```

- 5. Set the focus of the Roo commands on the Flight entity and use the field reference command to create a many-to-one relationship between the Flight and FlightDescription entities, as shown here:
  ```
  roo> focus --class ~.domain.Flight
  roo> field reference --fieldName flightDescription --type ~.domain.FlightDescription --joinColumnName FLIGHT_DESC_ID --notNull
  ```

## How it works...

As we are not using composite primary keys for the entities, the following things you'll notice about the entity commands that we have used for creating Flight and FlightDescription entities:

- identifierField: An optional argument has been used to give a custom name to the identifier field of the entities. For instance, the name of identifier field of the Flight entity is flightId and that of FlightDescription is flightDescId. If we had not used the identifierField argument, then by default Roo would have assigned id as the name of the entity's identifier field.

- identifierColumn: An optional argument has been used to set the name of the table column to which the identifier field maps. This argument instructs Roo to add the @Column JPA annotation to the identifier field. If you don't specify the identifierColumn, by default the name of the mapping column is derived by splitting the camel-case name and adding underscore as the separator. For instance, if you don't specify the identifierColumn argument for an identifier field named myOwnIdField field, the name of the table column is assumed to be my_own_id_field. In the context of this recipe, the identifierColumn will not have any effect because the name of the table column, derived from the identifier field name, is the same as the name assigned by the identifierColumn argument.
