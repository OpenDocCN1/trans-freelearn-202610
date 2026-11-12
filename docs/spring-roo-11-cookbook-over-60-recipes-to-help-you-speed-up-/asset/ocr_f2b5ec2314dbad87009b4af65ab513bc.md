When the `class` command is executed, notice that the Roo prompt changes to refer to the newly created Java class. In the next recipe, titled Adding fields to a Java class, we will see how the changed Roo prompt simplifies performing commands on the referred class. Also, notice that the service directory is automatically created by Spring Roo, if it doesn't exist.

> Some command arguments, like `rooAnnotations`, act as a flag for the command processor, and you don't need to specify their value.
> Simply specifying them as part of the command means that the value of the argument is true or yes.

# How it works...

The `class` command accepts the arguments listed in the following table:

| Argument | Purpose |
| :--- | :--- |
| class | It is a mandatory argument that identifies the fully-qualified name of
the Java class that you want to create. You can either specify the fully-
qualified class name using the tilde symbol ' ~ ' or you can use the
`TAB` (or `CTRL` + `SPACE`) multiple times to let Spring Roo complete the
package name for you.
The ' ~ ' symbol refers to the top-level package of the Roo project.
For instance, in `flight-app` project, it refers to `sample.roo.flightapp` package.
You can use this symbol to specify the package
(relative to top-level package) in which you want to create your Java class. |
| rooAnnotations | It is an optional argument that specifies the common Roo
annotations, such as @RooJavaBean, @RooToString,
and @RooSerializable, which are added to the generated Java class.
If unspecified, these annotations are not added to the generated Java
class. |
| path | It is an optional argument that specifies the path to the source directory
in which the class is created. By default, the path is SRC_MAIN_JAVA. |
| extends | It is an optional argument that specifies the fully-qualified name of
the class, which the Java class extends. You can use this argument name to
create a class which extends from a superclass. |
| abstract | It is an optional argument that indicates whether the class is an
abstract or concrete class. You can use this argument to create an
abstract class. |
| permitReserved
Words | It is an optional argument that indicates whether Roo should allow
creating a class whose name is a reserved word. By default, Roo
doesn't allow creating Java classes whose name uses reserved words.
For instance, by default you cannot create a class named New. |