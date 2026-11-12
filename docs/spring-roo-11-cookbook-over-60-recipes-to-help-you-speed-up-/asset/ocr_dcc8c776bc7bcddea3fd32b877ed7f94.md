Flex Addon doesn't provide any support for controlling the methods that form a part of the Spring-managed remoting destinations. For instance, you can't control the methods that are part of the `FlightDescriptionService_Roo_Service.aj` file using `@RooFlexScaffold` annotation.

If you want that a method in the Spring-managed remoting destination is not accessible to Flex clients, then all you need to do is to either perform push-in refactoring or define the method in the corresponding Java class and add the `@RemotingExclude` annotation of Spring to the method.

Spring Roo makes use of JSR 303 annotations specified in the JPA entity to add Flex validators in the MXML files. For instance, if a JPA entity field specifies `@NotNull` JSR 303 annotation then Roo adds a Flex `StringValidator` or `NumberValidator` that checks that the field on the form is not blank. Note that Flex addon support for JSR 303 annotations is limited.

# See also

+   - Refer to the Getting started with Flex application development to see how you can set up Flex for your Roo project

# Getting started with Spring Web Flow

In Chapter 4, *Web Application Development with Spring Web MVC* we saw that Roo simplifies building Spring Web MVC applications. In this recipe, we'll look at how Roo sets up your Roo project to use Spring Web Flow—a framework that is built on top of Spring Web MVC.

Spring Web Flow allows modelling a web application as a set of flows, where each flow represents a finite state machine.

> The support for Spring Web Flow is broken in Spring Roo 1.1.3; therefore, you must use Spring Roo 1.1.4 or 1.1.5 to execute this recipe. This recipe has been developed using Spring Roo 1.1.15.

# Getting ready

Create a new directory `C:\roo-cookbook\ch05-webfcflow` in your system and copy the `ch05_webfcflow_app.roo` script that accompanies this book to the `ch05-webfcflow` directory.