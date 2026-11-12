+   - Refer to the Adding static views to Roo-generated web application recipe to find out how you can add your custom web pages to Roo-generated web applications
+   - Refer to the Creating a Spring MVC controller for a specific JPA entity recipe for details on how you can instruct Roo not to generate views for certain functionalities.

# # Round-tripping support in Spring Roo for web controllers and views

In Roo-generated applications, you can change JPA entities and let Roo take care of making necessary changes to the controllers and views. In this recipe, we look at an example scenario, which demonstrates how changes to a JPA entity are propagated to corresponding controllers and views.

# ## Getting ready

Delete the contents of the ch04-recipe sub-directory inside the C:\\roo-cookbook directory.

Copy the ch04_web-app.roo script into the ch04-recipe directory.

Execute the ch04_web-app.roo script that creates the flight-app Roo project, sets up Hibernate as the persistence provider, configures MySQL as the database for the application, creates Flight and FlightDescription JPA entities, and defines a many-to-one relationship between the Flight and FlightDescription entities. If you are using a different database than MySQL or your connection settings are different than what is specified in the script, then modify the script accordingly.

Start the Roo shell from the C:\\roo-cookbook\\ch04-recipe directory.

Execute the controller all command to create controllers and views corresponding to JPA entities in the flight-app project, as shown here:

```
.. roo> controller all --package ~.web
```

Execute the perform eclipse command to update the project's classpath settings, as shown here:

```
.. roo> perform eclipse
```

Now, import the project into your Eclipse IDE.

The following code from the FlightDescription.java file shows the FlightDescription entity, which we will modify in this recipe to get a feel of Roo's round-tripping capabilities:

```
@RooJavaBean
@RooToString
@RooEntity(identifierField = "flightDescId",
```