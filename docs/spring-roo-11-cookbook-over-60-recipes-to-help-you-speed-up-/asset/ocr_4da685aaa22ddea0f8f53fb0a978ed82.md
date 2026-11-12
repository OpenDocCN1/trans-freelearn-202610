+   FlightDataOnDemand_Roo_DataOnDemand.aj: this AspectJ ITD file defines methods that are added to the FlightDataOnDemand class during compilation. The methods defined in this AspectJ ITD file are responsible for dynamically creating 10 instances of the Flight entity and storing them in the database—referred to as seed data. These Flight instances are used while performing integration testing. The entity instances created by AspectJ ITD comply with the JSR 303 constraints that apply on persistent fields of the entity. By default, a transaction associated with a test method is rolled-back after the test method completes—the reason why you won't see seed data in database tables after the execution of integration tests. Refer to the Executing persistent entities tests recipe to see an example usage of Spring's @Rollback annotation to specify that transactions associated with test methods must not be rolled-back.

+   FlightIntegrationTest.java: represents the JUnit integration test class for the Flight entity.

+   FlightIntegrationTest_Roo_IntegrationTest.aj: AspectJ ITD responsible for defining integration testing methods for the Flight entity.

+   FlightDataOnDemand_Roo_Configurable.aj and Flight_Roo_Configurable.aj: AspectJ ITDs that add the @Configurable annotation to FlightDataOnDemand and Flight classes, respectively.

Let's now look at each of these files in detail.

The following listing shows the FlightDataOnDemand.java class:

```
import org.springframework.roo.addon.dod.RooDataOnDemand;
import sample.roo.flightapp.domain.Flight;

@RooDataOnDemand(entity = Flight.class)
public class FlightDataOnDemand { }
```

The code listing shows the use of Roo's @RooDataOnDemand annotation, which identifies the persistent entity for which the FlightDataOnDemand class creates seed data for integration testing. The @RooDataOnDemand annotation is responsible for the creation of the corresponding *_Roo_DataOnDemand.aj AspectJ ITD file. The @RooDataOnDemand accepts two attributes to customize the behavior of seed data generation:

+   entity: identifies the persistent entity for which the seed data needs to be created.

+   quantity: the number of records to be created for the entity, default being 10. If you want to create more records for integration testing of the entity, then specify an appropriate value of this attribute.