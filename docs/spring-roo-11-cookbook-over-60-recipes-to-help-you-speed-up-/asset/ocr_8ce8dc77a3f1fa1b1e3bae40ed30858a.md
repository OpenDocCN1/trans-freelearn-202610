The pom.xml additionally contains Maven plugins, which are available to Roo projects by default. The following table summarizes some of the important Maven plugins that are available to our newly created flight-app project:

| Maven Plugin | Usage |
|---|---|
| IDEA plugin | You may use this plugin to convert the flight-app project into an IntelliJ IDEA project. |
| Eclipse plugin | You may use this plugin to convert the flight-app project into an Eclipse project. |
| AspectJ compiler plugin | This plugin weaves AspectJ aspects into your project classes. This plugin is used internally by Spring Roo. We will see the AspectJ compiler in action in Chapter 2. |
| Tomcat and Jetty plugins | You can use these plugins during development to run Tomcat or Jetty in embedded mode to test your web application. |

# There's more...

You won't always be starting a project from scratch, and you may find Spring Roo compelling enough (which you will, as we go through its various features) to use in your existing Spring-based Java projects. In such scenarios, you need to do the following:

+   1. Convert your existing Spring-based project into a standard Maven project as created by Spring Roo's project command.
+   2. Add the AspectJ compiler plugin to the pom.xml file of your project.
+   3. Move bean definitions in your existing project to the applicationContext.xml file in META-INF/spring directory.
+   4. Move the properties file used for configuring Spring beans to META-INF/spring/ directory.

There are other things you will need to do to convert your existing projects into a Roo project, which we will discuss in relevant recipes.

# See also

+   - The Configuring logging recipe discusses how to configure logging in Roo projects
+   - Refer to the Creating a Java class and Create a Java interface recipes to find out how you can use Spring Roo to create classes/interfaces in your application