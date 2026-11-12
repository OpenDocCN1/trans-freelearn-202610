# There's more...

The ch02_flightapp_testing.zip file that accompanies this book contains the flight-app Eclipse project that we saw in recipes of the previous chapter and this chapter. You can import this project into your Eclipse IDE and view the following changes that I made to the flight-app project to demonstrate concepts that we've learned so far:

+   - Defined the setNumOfSeats method in FlightDataOnDemand.java to create Flight entities that meet the constraint specified by the JSR 303 @Size annotation for the numOfSeats field. Defining the setNumOfSeats method in FlightDataOnDemand.java results in the removal of the method with the same signature from the FlightDataOnDemand_Roo_DataOnDemand.aj file.
+   - Defined setOrigin and setDestination methods in FlightDataOnDemand.java to create Flight entities that meet the constraints specified by JSR 303 @DecimalMax and @DecimalMin annotations on origin and destination fields. Defining setOrigin and setDestination methods in FlightDataOnDemand.java results in removal of methods with the same signature from the FlightDataOnDemand_Roo_DataOnDemand.aj file.
+   - Changed the @RooIntegrationTest annotation in FlightIntegrationTest.java to instruct Roo not to generate findAll, findEntries, and count test methods.
+   - Autowired MyFlightDod reference (a custom 'data on demand' class) in FlightIntegrationTest.java and defined a custom testMyCustomDodTest method.
+   - Added a @Rollback(false) Spring annotation to the testPersist method in FlightIntegrationTest.java so that the data created by the testPersist method is not rolled back after it is executed. This feature could be particularly useful if you want to manually verify the data that was created by the testPersist method or if you want to keep the data for further testing.

You can execute the tests defined in the flight-app project using the perform tests command and check your database to view the seed data that was created by the 'data on demand' class and during the testing of testPersist method.

# See also

+   - Refer to the Creating integration tests for persistent entities recipe to see Spring Roo's support for auto-generation of integration tests for JPA entities
+   - Refer to the Creating mock tests for persistent entities recipe to mock static methods defined in entities