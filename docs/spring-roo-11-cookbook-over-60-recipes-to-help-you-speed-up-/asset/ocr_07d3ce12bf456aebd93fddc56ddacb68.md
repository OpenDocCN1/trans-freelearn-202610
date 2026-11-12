+   - If the primary key of a database table consists of multiple columns, Roo creates a composite primary key class and a * _Roo_Identifier.aj AspectJ ITD. For instance, in the case of CUSTOMER_TBL, Roo generates a CustomerTblPK.java and CustomerTblPK_Roo_Identifier.aj AspectJ ITD file.

+   - If a database table or view doesn't define a primary key, Roo creates a composite primary key class consisting of all the fields in the database table or view. For instance, the FLIGHTS_VIEW doesn't define a primary key; therefore, Roo creates the FlightsViewPK.java composite primary key class.

The following code shows the FlightDescTbl_Roo_DbManaged.aj ITD, which corresponds to the FLIGHT_DESC_TBL table:

```
```java
privileged aspect FlightDescTbl_Roo_DbManaged
{
    @OneToMany(mappedBy = "flightDescId")
    private Set<FlightTbl> FlightDescTbl.flightTbls;
    @Column(name = "destination_city",
    columnDefinition = "VARCHAR" length = 255)
    @NotNull
    private String FlightDescTbl.destinationCity;
    @Column(name = "origin_city", length = 255)
    @NotNull
    private String FlightDescTbl.originCity;
    ...
}
```
```

The given code shows that the *_Roo_DbManaged.aj AspectJ ITD contains fields and relationship information derived from the database metadata. This is different from what we saw in earlier recipes where the JPA fields and relationships are contained in Java source files and not in AspectJ ITD files. Also, notice the presence of the @NotNull JSR 303 annotation, which is derived from the fact that the corresponding database column is defined as non-nullable.

As with other AspectJ ITDs, the creation of the *_Roo_DbManaged.aj is managed by a Roo annotation, @RooDbManaged, as shown here for the FlightDescTbl.java class:

```
```java
@RooJavaBean
@RooToString
@RooEntity(table = "flight_desc_tbl")
@RooDbManaged(automaticallyDelete = true)
public class FlightDescTbl {
}
```
```