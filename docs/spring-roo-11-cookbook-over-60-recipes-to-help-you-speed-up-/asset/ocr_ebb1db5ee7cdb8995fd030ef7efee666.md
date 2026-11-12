```
findFlightsByDestinationLike(String destination)
findFlightsByDestinationNotEquals(String destination)
```

# How it works...

The finder list command displays names of the candidate dynamic finder methods for an entity. By default, the dynamic finder methods suggested by the finder list command search for entity instances based on only one persistent field of the entity. For instance, in the output of the finder list command you will not find a dynamic finder method name which finds the Flight entity instances based on both the createdDate and modifiedDate fields. If an entity inherits from a mapped superclass, then the dynamic finder methods corresponding to the inherited fields are also displayed by Roo.

The dynamic finder methods suggested by the finder list command are dependent upon the type of the field. For instance, the createdDate is of type java.util.Date and can participate in greater than, less than, and between comparisons; therefore, findFlightsByCreatedDateBetween, findFlightsByCreatedDateGreaterThan, and so on, are shown as candidate dynamic finder methods, and these methods accept the createdDate field as the argument. On the other hand, the destination field is of type String, which doesn't participate in greater than, less than, and between comparisons; therefore, Roo doesn't suggest dynamic finder methods for finding the Flight instances based on greater than, less than, and between comparisons of destination field.

# There's more...

Let's now look at how to instruct Roo to:

+   - Provide a list of candidate dynamic finder methods, which fetch entities based on more than one persistent field
+   - Restrict suggested dynamic finder method names based on a filter criteria

# Listing dynamic finder methods for multiple persistent fields

If you want Roo to list finder method names that fetch entities based on multiple persistent fields, you should use the depth argument of the finder list command. The depth argument accepts a numeric value, which determines the number of persistent fields the dynamic finder method uses to search for entities in the database. For instance, if you want Flight instances to be searched based on both the origin and destination fields, then the value of the depth argument must be 2. The default value of the depth argument is 1, therefore; when we executed the finder list command, without specifying depth argument, it listed dynamic finder method names that fetch entities based on only one persistent field. The following finder list command shows the affect of the depth argument on the suggested list of dynamic finder method names:

```
~ .domain.Flight roo> finder list --depth 2
findFlightsByDestinationAndOrigin(String destination, String origin)
```