The following diagram summarizes how a simple add-on works:

![](bbox=[0.228, 0.175, 0.77, 0.505])

The figure shows that CommandMarker is an interface provided by Roo, and it is implemented by the MysimpleCommands class. The MysimpleCommands class invokes methods of the MysimpleOperationsImpl class to process the commands exposed by the mysimple add-on. The MysimpleCommands and MysimpleOperationsImpl classes use services provided by Roo to perform the desired functionality.

# There’s more...

In this section we'll look at:

+   - How to locally deploy the mysimple add-on for testing Roo commands
+   - How tab-completion support is implemented for Roo commands using constants defined in an enum type and a Java class
+   - How the @CliAvailabilityIndicator annotation can be used for a method to define availability of multiple Roo commands exposed by the *Command class
+   - What templates are in add-ons and how they are typically used
+   - Plugins and dependency configuration in the pom.xml file of an add-on