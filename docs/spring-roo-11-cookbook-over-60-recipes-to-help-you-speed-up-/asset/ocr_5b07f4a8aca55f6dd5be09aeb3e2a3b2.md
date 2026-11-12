# # Creating a one-to-many (or many-to-many) relationship between entities

In the previous recipe, we saw how the many-to-one (or one-to-one) relationship is established between entities using the `field reference` command. In this recipe, we'll extend the same example to show how a one-to-many (or many-to-many) relationship can be created using the `field set` command. The following class diagram shows a one-to-many relationship between the `FlightDescription` and `Flight` entities:

![](bbox=[0.213, 0.301, 0.792, 0.412])

In the given figure, you'll notice that we have added a `flights` field of type `Set<Flight>` to the `FlightDescription` entity, reflecting the one-to-many relationship between the `FlightDescription` and the `Flight` entity.

# ## Getting ready

Exit the Roo shell and delete the contents of the `C:\roo-cookbook\ch03-recipes` directory.

Start the Roo shell from the `C:\roo-cookbook\ch03-recipes` directory.

Execute the `ch03_relationship_many_to_one.roo` script, which creates the `flight-app` Roo project, sets up Hibernate as a persistence provider, configures MySQL as the database for the application, creates the `Flight` and `FlightDescription` JPA entities, and defines a many-to-one relationship between the `Flight` and `FlightDescription` entities. If you are using a different database than MySQL or your connection settings are different than what is specified in the script, then modify the script accordingly.

# ## How to do it...

To create a one to many (or many to many) relationship between the `FlightDescription` and `Flight` entities, follow the steps given here:

+   1. Set the focus of the Roo commands on the `FlightDescription` entity:

```
.. roo> focus --class ~.domain.FlightDescription
```