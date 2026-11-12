# How to do it...

The following steps show how to use the `controller scaffold` command to create controllers for JPA entities:

+   1. Execute the `controller scaffold` command to create `FlightController`, as shown here:
+   
+   ..roo> controller scaffold --class ~.web.FlightController --entity ~.domain.Flight --path /myflightpath --disallowedOperations update,delete

+   2. Execute the `perform eclipse` command to update the classpath settings, and import the project in your Eclipse IDE.

# How it works...

The following table describes the purpose of each of the arguments passed to the `controller scaffold` command:

| Argument | Description |
|---|---|
| class (mandatory) | Fully-qualified name of the controller class, which you want to create. |
| entity (optional) | Fully-qualified name of the Roo-managed JPA entity class (a class annotated with `@RooEntity` annotation), which is used as a form-backing object by the generated controller. The value of this argument translates into the value of the `formBackingObject` attribute of the `@RooWebScaffold` annotation. |
| path (optional) | Identifies the sub-directory inside `/WEB-INF/views/`, which contains the JSPX views corresponding to the controller. The value of this argument translates into the value of the `path` attribute of the `@RooWebScaffold` annotation.

The value of the `path` argument also translates into the value of the `@RequestMapping` class-level annotation in the generated controller. |
| disallowedOperations (optional) | Comma-separated list of operations, which is not supported by the generated controller. For instance, if the value is `update, delete`, then the generated controller doesn't contain methods to update and delete the JPA entity corresponding to which the controller was generated. The only valid values for this argument are `update`, `delete`, and `create`. |