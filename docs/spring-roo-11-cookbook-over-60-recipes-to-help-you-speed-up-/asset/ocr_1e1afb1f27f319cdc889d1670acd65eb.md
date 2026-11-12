# # Creating a mapped superclass

In this recipe, we'll look at how to create an entity which inherits a *mapped superclass*. The fields of a mapped superclass are stored into the table to which the inheriting entity is mapped. The mapped superclass itself is *not* mapped to any table. In this recipe, we'll create the *Flight* entity, which extends the *AuditFields* class. The *AuditFields* class represents a *mapped superclass*; the fields of *AuditFields* are stored into the table to which the *Flight* entity is mapped.

The following figure shows the relationship between the *Flight* entity and the *AuditFields* mapped superclass:

![](bbox=[0.41, 0.33, 0.59, 0.535])

# ## Getting ready

Exit the Roo shell and delete the contents of the `C:\roo-cookbook\ch03-recipes` directory.

Start the Roo shell from the `C:\roo-cookbook\ch03-recipes` directory.

Execute the `ch03_jpa_setup.roo` script which creates the *flight-app* Roo project, sets up Hibernate as persistence provider, and configures MySQL as the database for the application. If you are using a different database than MySQL or your connection settings are different than what is specified in the script, then modify the script accordingly.

# ## How to do it...

To create an entity which inherits a *mapped superclass*, follow the given steps:

+   1. Create the *AuditFields* class using the `entity` command and add fields to it:

```
.. roo> entity --class ~.domain.AuditFields --mappedSuperclass
```