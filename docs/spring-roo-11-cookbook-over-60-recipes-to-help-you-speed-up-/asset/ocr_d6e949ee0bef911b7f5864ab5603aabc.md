# There's more...

Instead of using Spring Roo, you can also modify the `database.properties` file using an IDE like Eclipse or STS. Using Roo commands to modify the `database.properties` file allows developers to replay the actions taken from the Roo shell using the `script` command.

# See also

+   - Refer to the Viewing database configuration properties recipe, described earlier in this chapter, to see how you can view properties defined in the `database.properties` file using the Roo command

# Creating persistent entities

In this recipe we look at how Spring Roo simplifies the creation of JPA entities using the `entity` and `field` commands. In this recipe we'll create a `Flight` JPA entity which has a composite primary key. Refer to the Creating a many-to-one relationship between entities recipe of Chapter 3, Advanced JPA Support in Spring Roo to see how to create persistent entities with surrogate keys.

The following figure shows the attributes of the `Flight` entity and its composite primary key (`FlightKey`):

![](bbox=[0.275, 0.51, 0.725, 0.672])

# Getting ready

Exit the Roo shell and delete the contents of the `C:\roo-cookbook\ch02-recipes` directory.

Execute the `ch02_jpa_setup.roo` script. It creates a `flight-app` Roo project and sets up Hibernate as the persistence provider using the `persistence setup` command. If you are using a different database than MySQL or your connection settings are different from what is specified in the script, then modify the script accordingly.