# Switching elements from user-managed to Roo-managed mode

We saw that Spring Roo does a lot of heavy lifting to create the user interface of the web application. This includes creating JSPX files, installing custom tags, creating Spring Web MVC controllers, creating web application context XML file, creating the web application deployment descriptor, creating tiles definitions, installing themes, and so on. We also saw that the value of the `required` attribute of the `<select>` tag for showing flight descriptions was set to `true` because the Flight JPA entity participates in a not null many-to-one relationship with the FlightDescription entity.

Let's assume that we want the text field of the `departureDate` field also to be mandatory to create the `Flight` entity. You have the option to annotate the `departureDate` field of the `Flight` JPA entity with `@NotNull` JSR-303 annotation and let Roo take care of setting the `required` attribute's value to `true` for the `departureDate` text field. Let's further assume that we don't want to make any changes to the `Flight` entity and a date must be entered in the `departureDate` field when creating the `Flight` entity from the user interface. We can do this by simply setting the `required` attribute's value to `true`, as shown here:

```
<field:datetime
dateTimePattern="${flight_departuredate_date_format}"
field="departureDate" required="true"
id="c_sample_roo_flightapp_domain_Flight_departureDate"
z="BtcAuQvStTt55J3J6zFybhfkSxA=" />
```

As we have changed the default value of the `required` attribute from `false` to `true`, we have effectively modified the element; therefore, Roo will go ahead and set the value of the `z` attribute to user-managed. A user-managed element in Spring Roo doesn't participate in round tripping; therefore, if you make any modifications to the `departureDate` field of the `Flight` entity, then Roo will _not_ make any corresponding modifications to the `departureDate` element in the `create.jspx` file. For instance, if you remove the `departureDate` field from the `Flight` entity, then Roo will not remove the `departureDate` element from `create.jspx`. If you want your modified form element to be Roo-managed again, all you need to do is set the value of the `z` attribute to `?`. The value of `?` instructs Roo to re-calculate the value of the `z` attribute based on the current attributes and their values, making it possible for Roo to figure out if changes happen to the form element in the future.

As we just discussed, you can take control of Roo-managed form elements in JSPX views by adding or modifying one or more attributes. It is recommended that after you make changes to a form element, revert back to the Roo-managed mode by setting the value of the `z` attribute to `?`. In Roo-managed mode any modification to the JPA entities is taken care of by Roo, saving the effort to make adjustments to the views.

# See also

+   * Refer to the Round-tripping support in Spring Roo for web controllers and views recipe to see examples of round tripping support in Roo