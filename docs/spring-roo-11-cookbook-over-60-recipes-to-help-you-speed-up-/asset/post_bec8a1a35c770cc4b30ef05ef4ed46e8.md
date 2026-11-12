
## Manually creating a Spring MVC controller for a JPA entity

If you want to create a custom controller, then Roo offers a `controller` class command that creates the skeleton structure of a controller and a JSPX view to let you quickly get started.

Let's consider that in our `flight-app` application we have the following entities:

- `Customer`: Represents a customer in the flight booking application
- `Address`: Represents the address of the customer

For the sake of simplicity, let's assume that there is a one-to-one bidirectional relationship between `Customer` and `Address` entities, the `Customer` being the owner of the relationship. The `Customer` entity has only one field: the `customerName` and the `Address` entity has two fields: `addressLine1` and `addressLine2`.

Let's say the `flight-app` application requires that the `Customer` and the corresponding `Address` entities are created from the same form. When the user enters the customer's name and clicks the **Add address** button (as shown in the next screenshot), then the form is expanded to show the table for entering address information for the customer, and for saving the customer's details. The following screenshot shows the form for entering information:

![](img/b8c379a2ed921ead3aa2b01811dc29fa_198_0.png)

In the given screenshot, clicking the **Save** button creates a `Customer` and also the corresponding `Address` entity instance.

As we have seen earlier, Roo generates views, which allow creating or updating only a single entity at a time. In this scenario, we need to create both `Customer` and `Address` entities simultaneously. This not only requires us to create a custom view but also a custom controller.

### Getting ready

Delete the contents of the `ch04-recipe` sub-directory inside the `C:\roo-cookbook` directory.

Copy the `ch04_manual_controller.roo` script into the `ch04-recipe` directory.

Execute the `ch04_manual_controller.roo` script which creates the flight-app Roo project, sets up Hibernate as the persistence provider, configures MySQL as the database for the application, creates Flight, FlightDescription, Customer, and Address JPA entities, defines the many-to-one relationship between Flight and FlightDescription entities and a one-to-one relationship between the Customer and Address entities. If you are using a different database than MySQL or your connection settings are different than what is specified in the script, then modify the script accordingly.

Start the Roo shell from the `C:\roo-cookbook\ch04-recipe` directory.

### How to do it...

To see how to use a manually created Spring Web MVC controller for the given application requirement, follow the given steps:

- 1. Execute the `web mvc setup` command to set up the Spring Web MVC artifacts, and to convert the Roo project into a web project:
  ```
  roo> web mvc setup
  ```

- 2. Execute the `controller class` command, as shown here:
  ```
  roo> controller class --class ~.web.CustomerController
  --preferredMapping /customer
  ```
  Created SRC_MAIN_JAVA\sample\roo\flightapp\web\CustomerController.java
  Created SRC_MAIN_WEBAPP\WEB-INF\views\customer
  Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\index.jspx
  Updated SRC_MAIN_WEBAPP\WEB-INF\i18n\application.properties
  Created SRC_MAIN_WEBAPP\WEB-INF\views\menu.jspx
  Created SRC_MAIN_WEBAPP\WEB-INF\tags\menu\menu.tagx
  Created SRC_MAIN_WEBAPP\WEB-INF\tags\menu\item.tagx
  Created SRC_MAIN_WEBAPP\WEB-INF\tags\menu\category.tagx
  Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\views.xml

- 3. As many dependencies were added to the `pom.xml` file of the `flight-app` project during processing of the `web mvc setup` command, execute the `perform eclipse` command of Roo to update the `.classpath` file of the Eclipse project:
  ```
  roo> perform eclipse
  ```
  Now, import the `flight-app` project in Eclipse IDE.

- 4. In the Customer.java file, make the association between the Customer and Address entities as mandatory by setting the optional attribute value of the @OneToOne JPA annotation to false. Also, set the cascade attribute value of @OneToOne to CascadeType.ALL. The following listing shows the modified @OneToOne annotation in Customer.java:

  ```
  import javax.persistence.CascadeType;
  ..
  @RooEntity(table = "CUSTOMER_TBL")
  public class Customer {
      @NotNull
      @Column(name = "CUST_NAME")
      private String customerName;
      @OneToOne(optional = false, cascade = CascadeType.ALL)
      @JoinColumn(name = "CUSTOMER_ID")
      private Address address;
  }
  ```

- 5. In the Address.java file, make the association between the Address and Customer entities as mandatory by setting the optional attribute value of the @OneToOne annotation to false. As the Customer entity is the owner of the relationship, set the mappedBy attribute value of the @OneToOne JPA annotation to address. The following listing shows the modified @OneToOne annotation in Address.java:

  ```
  ...
  @RooEntity(table = "ADDRESS_TBL")
  public class Address {
      @NotNull
      @Column(name = "ADDRESS_LINE1")
      private String addressLine1;
      ...
      @OneToOne(optional = false, mappedBy = "address")
      private Customer customer;
  }
  ```

  As in Spring Roo 1.1.5, the field reference command doesn't support any argument, which lets you specify the owner of the one-to-one bidirectional relationship; therefore, you need to edit your Java source for specifying the mappedBy attribute value of the @OneToOne annotation. On the other hand, the field set command does provide a mappedBy argument for specifying the relationship owner.

- 6. Replace the CustomerController.java file in the sample.roo.flightapp.web package of SRC_MAIN_JAVA folder with the one contained in the source code folder of this chapter.

Replace the `index.jspx` file in the `/WEB-INF/views/customer/` folder with the `index.jspx` file contained in the source code folder of this chapter.

Exit the Roo shell and use the following maven command to deploy the `flight-app` project as a dynamic web application in an embedded Tomcat instance:

```
.ch04-recipe> mvn tomcat:run
```

Now, you can access the `flight-app` application by accessing the following URL:
`http://localhost:8080/flight-app`

If you see the following web page, then it means you have successfully deployed the `flight-app` application on the embedded Tomcat instance:

![](img/b8c379a2ed921ead3aa2b01811dc29fa_201_0.png)

In the given screenshot, selecting the **Customer Controller View** menu option will take you to the form for creating the `Customer` and `Address` entities.

### How it works...

The `controller` class command is used for creating a manual controller. It accepts the following arguments:

- `class`: The fully qualified name of the controller class that you want to create.

- `preferredMapping`: The request path to which the controller maps. The value of the `preferredMapping` argument is used to derive the value of `@RequestMapping` class-level annotation of the generated controller. The value of the `preferredMapping` is also used to create a sub-folder inside the `/WEB-INF/views/` to contain the JSPX views and the tiles definitions XML file corresponding to the generated controller.

As the output suggests, the following actions are performed by Roo when the `controller` class command is executed:

- Creates a skeleton `CustomerController` web controller, leaving the controller implementation details to be provided by the developer.

- Creates the `customer` sub-folder inside `/WEB-INF/views/`.

- Creates a skeleton `index.jspx` view inside the `/WEB-INF/views/customer` folder, leaving view details to be provided by the developer.

- Creates a tiles definitions XML file, `views.xml`, inside the `/WEB-INF/views/customer` folder. It contains a single tile definition `customer/index`, which corresponds to the `index.jspx` view. As we will see shortly, the skeleton `CustomerController` implementation makes use of the `customer/index` tile definition to show the `index.jspx` view.

- Adds the `Customer Controller View` label to the `application.properties` file, which is used by the `menu.jspx` file to display a menu option for invoking the `CustomerController` web controller.

- Adds a menu option labelled `Customer Controller View` to the `menu.jspx` file for invoking the `CustomerController` web controller.

It is important to note that no AspectJ ITD file is created when the `controller class` command is executed.

The following code from the `CustomerController.java` file shows the `CustomerController` generated by the `controller class` command:

```
@RequestMapping("/customer/**")
@Controller
public class CustomerController {

    @RequestMapping
    public void get(ModelMap modelMap, ...) { }

    @RequestMapping(method = RequestMethod.POST, value = "{id}")
    public void post(@PathVariable Long id, ...) { }

    @RequestMapping
    public String index() {
        return "customer/index";
    }
}
```

As the given code shows, the generated controller leaves it up to the developer to write the implementation of the controller. The method `index()` is invoked when you click the **Customer Controller View** menu option. The `index()` method simply returns `customer/index`, which shows the `index.jspx` view.

The Roo-generated `CustomerController` is hardly of any use, so you need to write its functionality. The `CustomerController.java` file that accompanies the source code of this chapter contains the necessary functionality for creating the `Customer` and `Address` entities. Let’s now take a look at the methods defined in the supplied `CustomerController.java` file:
