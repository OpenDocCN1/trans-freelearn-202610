
- Refer to the Adding static views to Roo-generated web application recipe to find out how you can add your custom web pages to Roo-generated web applications
- Refer to the Creating a Spring MVC controller for a specific JPA entity recipe for details on how you can instruct Roo not to generate views for certain functionalities.

## Round-tripping support in Spring Roo for web controllers and views

In Roo-generated applications, you can change JPA entities and let Roo take care of making necessary changes to the controllers and views. In this recipe, we look at an example scenario, which demonstrates how changes to a JPA entity are propagated to corresponding controllers and views.

### Getting ready

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
identifierColumn = "FLIGHT_DESC_ID",
table = "FLIGHT_DESC_TBL",
finders = {"findFlightDescriptionsByDestinationAndOrigin" })
public class FlightDescription
{
    @NotNull
    @Column(name = "ORIGIN_CITY")
    private String origin;
    @NotNull
    @Column(name = "DESTINATION_CITY")
    private String destination;
    @NotNull
    @Column(name = "PRICE")
    private Float price;
}
```

### How to do it...

To see round-tripping support for web controllers and views, follow the steps given here:

- 1. Open the FlightDescription.java file in your editor.

- 2. Remove the finders attribute and its value from the @RooEntity annotation, and save the FlightDescription.java file. The Roo shell shows the actions taken by Roo in response to the deletion of the finders attribute, as shown here:

```
Updated SRC_MAIN_WEBAPP\WEB-INF\views\menu.jspx
Deleted SRC_MAIN_JAVA\...\web\FlightDescriptionController_Roo_Controller_Finder.aj
Deleted SRC_MAIN_JAVA\...\domain\FlightDescription_Roo_Finder.aj
```

- 3. Change the name of the origin field to originCity, destination field name to destinationCity and save the FlightDescription.java file. The Roo shell shows the following actions taken by Roo in response to our changes:

```
Updated ...\WEB-INF\views\flightdescriptions\list.jspx
Updated ...\WEB-INF\views\flightdescriptions\show.jspx
Updated ...\WEB-INF\views\flightdescriptions\create.jspx
Updated ...\WEB-INF\views\flightdescriptions\update.jspx
Updated ...\WEB-INF\i18n\application.properties
Updated ...\domain\FlightDescription_Roo_JavaBean.aj
Updated SRC_MAIN_JAVA\...\web\ApplicationConversionServiceFactoryBean_Roo_ConversionService.aj
Updated SRC_MAIN_JAVA\...\domain\FlightDescription_Roo_ToString.aj
```

- 4. Remove the @NotNull JSR 303 annotation from the price field and save the FlightDescription.java file. Now, Roo shell shows the following output in response to the change:

```
Updated... \WEB-INF\views\flightdescriptions\create.jspx
Updated... \WEB-INF\views\flightdescriptions\update.jspx
```

### How it works...

Let's now look at what Spring Roo did when we made modifications to FlightDescription.java. The following adjustments were made by Spring Roo in the flight-app project when the finders element was removed:

- Spring Roo removed the FlightDescription_Roo_Finder.aj ITD, which contained the implementation of the finder method.

> If a JPA entity defines multiple finder methods, then removing a single finder method from the finders attribute of the @RooEntity annotation will only remove the corresponding finder method from the ITD file.

- When the FlightDescriptionController_Roo_Controller_Finder.aj ITD was created initially, it contained a method for showing the form (refer to /WEB-INF/views/flightdescriptions/findFlightDescriptionsByDestinationAndOrigin.jspx), which allows searching for a FlightDescription based on origin and destination cities. It also contained a method to search the FlightDescription instances and display results (refer to /WEB-INF/views/flightdescriptions/list.jspx). Now, as the finder method has been removed, the controller methods to show the search form and search results are removed. It is important to note that in Spring Roo 1.1.3, the method responsible for searching entity instances is not created in FlightDescriptionController_Roo_Controller_Finder.aj. This bug is resolved in Spring Roo 1.1.4 and above.

> It is important to note that even though the findFlightDescriptionsByDestinationAndOrigin.jspx file is no longer required in the flight-app application, Spring Roo doesn't remove it. You will need to manually remove the JSPX file from your Roo project. This is because Spring Roo doesn't automatically delete JSPX files that are no longer required in the application.

- As the finder method has been removed, the menu option Find by Destination and City is also removed from the menu.jspx file.

When the origin and the destination field names are modified, then the following modifications are performed by Roo:

- In the `FlightDescription_Roo_JavaBean.aj` ITD, the getter and setter methods for the `origin` and the `destination` fields are replaced with the getter and setter methods for `originCity` and `destinationCity`, respectively.

- In the `FlightDescription_Roo_ToString.aj` ITD, the `toString` method is modified to include the value of the `destinationCity` and `originCity` fields, instead of the `origin` and the `destination` fields.

- In the Auto-generating Spring MVC controllers and JSPX views from JPA entities recipe we discussed how the Roo-generated `ApplicationConversionServiceFactoryBean` is configured by the `<mvc:annotation-driven>` element defined in the web application context XML. We saw earlier that the `*_Roo_ConversionService.aj` file of the flight-app project introduces static classes into the `ApplicationConversionServiceFactoryBean` class that represent converters for the `Flight` and the `FlightDescription` JPA entities.

- The following code shows the `FlightDescriptionConverter` static class defined in `ApplicationConversionServiceFactoryBean_Roo_ConversionService.aj`, which returns the converter for the `FlightDescription` entity:

```
import org.springframework.core.convert.converter.Converter;
privileged aspect ApplicationConversionServiceFactoryBean_Roo_ConversionService {
...
static class ApplicationConversionServiceFactoryBean.FlightDescriptionConverter implements
Converter<FlightDescription, java.lang.String> {
public String convert(FlightDescription
flightDescription){
return new StringBuilder().
append(flightDescription.getOrigin()).
append("").
append(flightDescription.getDestination()).
append(" ").
append(flightDescription.getPrice()).toString();
}
}
...
}
```

In the given code, the `FlightDescriptionConverter` static class implements Spring's `Converter`. It converts the `FlightDescription` JPA entity into a `String` representation. The `String` representation of `FlightDescription` is created by simply concatenating the values of each of its fields.

As we modified the names of the `origin` and the `destination` fields to `originCity` and `destinationCity`, Roo modifies the `FlightDescription` Converter class to use the modified getter methods for the fields. The changes that occurred due to these modifications can be outlined as follows:

- The `application.properties` file is modified to add new properties, which act as labels, for the `originCity` and the `destinationCity` fields, as shown here:

```
label_sample_roo_flightapp_domain_flightdescription_destinationcity=Destination City
label_sample_roo_flightapp_domain_flightdescription_origincity=Origin City
```

As you may have guessed, the property names are derived from the package in which the JPA entity resides, that is the JPA entity name and the name of the field. It is also important to note that once a property is added to the `application.properties` file, it is never removed or modified by Roo. Roo always creates new properties in the `application.properties` file. So, if you frequently modify your JPA entity fields, it will result in unwanted proliferation of properties in the `application.properties` file, which you will need to remove manually.

- JSPX views: the `create.jspx`, `update.jspx`, `show.jspx`, and the `list.jspx` files in `/WEB-INF/views/flightdescriptions/` are modified to reflect the change in name of the fields of the `FlightDescription` JPA entity. It is important to note that if a field defined in the JSPX view is not Roo-managed, that is, the value of the `z` attribute is 'user-managed' then Roo will not make any modification to the field in response to changes in JPA entities.

When the `@NotNull` JSR 303 annotation is removed from the `price` field of `FlightDescription`, then Roo cascades this change to the `/WEB-INF/views/flightdescriptions/create.jspx` and the `/WEB-INF/views/flightdescriptions/update.jspx` views. The only change that Roo makes to these views is to remove the `required` attribute from the `<input>` custom tag element that shows the `price` field on the web user interface.

### There's more...

The round-tripping support for JSPX views in Roo is quite sophisticated and takes care of the following changes in the Roo-managed JPA entity:

- Change in name of fields
- Change in type of fields
- Change in the JSR 303 annotation associated with fields
