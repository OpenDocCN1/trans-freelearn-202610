The integration test for the Flight entity will fail currently because:

+   - The auto-generated 'data on demand' class provides limited support for creating entity instances that comply with JSR 303 annotations. As explained in the Creating integration tests for persistent entities recipe, you can write custom setter methods for persistent fields in the FlightDataOnDemand.java entity to create Flight entities that comply with JSR 303 annotations specified on the persistent fields.

+   - The query fired by the countFlights method in the Flight_Roo_Entity.aj AspectJ ITD file doesn't work with MySQL 5.1.1. You need to change the countFlights method from:

```
public static long countFlights() {
    return entityManager().createQuery("SELECT COUNT(o)
    FROM Flight o", Long.class).getSingleResult();
}
```

To:

```
public static long countFlights() {
    return entityManager().createQuery("SELECT COUNT(*)
    FROM Flight o", Long.class).getSingleResult();
}
```

As AspectJ ITD files are managed by Spring Roo, you should not change the countFlights method in the Flight_Roo_Entity.aj file. Instead, either perform push-in refactoring or create a countFlights method in the Flight.java file.

In push-in refactoring, you use the IDE to push the declarations, methods, attributes, and constructors defined in the AspectJ ITD file to the target Java class. For more information on push-in refactoring, refer to the Removing Roo with push-in refactoring recipe of Chapter 7.

> When writing countFlights method in Flight.java, make sure that Roo is running in the background, so that Roo can remove the countFlights method from the Flight_Roo_Entity.aj file. Additionally, ensure that the signature of countFlights method is same as the one defined in the Flight_Roo_Entity.aj file.

# How it works...

The perform tests command is processed by the maven add-on of Roo and is responsible for executing all the tests that form part of the Roo project. If you want to directly execute the tests using maven, then exit the Roo prompt and use the mvn test command of maven. The results of test execution are saved in the <project_dir>/target/surefire-reports directory, where project_dir is your Roo project directory.