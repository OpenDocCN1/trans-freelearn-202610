> In the given code, the `FlightDescriptionDataOnDemand` object is autowired into the `FlightDataOnDemand` object. As the `Flight` entity has a many-to-one relationship with the `FlightDescription`, data on demand class of the `Flight` entity uses the data on demand class of the `FlightDescription` to obtain a reference to an existing `FlightDescription` instance and set it in the `Flight` instance. This ensures that the test data created during integration testing of the many-to-one relationship is as per the relationship that exists between related entities.

It is important to note that `@NotNull` and `@ManyToOne(..., optional=false)` mean the same thing. It is possible to specify that a relationship between entities must exist either by using `@NotNull` (JSR 303: Bean Validation annotation) or by specifying `false` as the value of the `optional` element of the `@ManyToOne` JPA annotation. In either case, the data on demand class of the entity will ensure that the data on demand class of the related entity is used to enforce a foreign key constraint on the test data created for integration testing.

# Dynamic finder method for a many-to-one relationship field

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

# See also

+   - The Creating a one-to-many (or many-to-many) relationship between entities recipe that follows