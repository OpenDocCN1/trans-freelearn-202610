```
<context:exclude-filter expression=
    "org.springframework.stereotype.Controller"
    type="annotation"/>
</context:component-scan>
</beans>
```

The important inferences that we can derive from the content of applicationContext.xml are:

+   The definition of the `<property-placeholder>` element of Spring's `context` namespace indicates that you must put your properties files that contain configuration information for Spring beans, in the META-INF/spring directory so that they can be picked up by Spring's application context. In the next chapter, we will see how this is used by Roo-generated applications to read database properties from an external properties file.

+   The `<spring-configured>` element of Spring's `context` namespace specifies that objects that are annotated with `@Configurable` annotation are configured using Spring, even if they are created outside of the Spring container. The objects created outside the Spring container include objects that are created programmatically using the `new` operator or by reflection. We will see example usage of `@Configurable` annotation in Chapter 2, Persisting Objects Using JPA.

+   The `<component-scan>` element of Spring's `context` namespace specifies that Spring components (that is, components annotated with `@Service`, `@Repository` and `@Component` Spring annotations) found inside the `sample.roo.flightapp` package or its sub-packages are automatically registered with Spring's application context. Later in this chapter, we will use this feature to create a service class in the flight-app project, which is auto-registered with Spring's application context.

The other important artifact that was generated during the project creation is the pom.xml file that is used by maven. The following XML code shows how the argument values specified in the project command are used in creating the pom.xml file of the flight-app project:

```
<project >
......
<groupId>sample.roo.flightapp</groupId>
<artifactId>flight-app</artifactId>
<packaging>jar</packaging>
<version>0.1.0.BUILD-SNAPSHOT</version>
<name>flight-app</name>
</project>
```

You may notice that the value of the `<packaging>` element is `jar` and not `war`. The reason for this lies with the fact that we haven't yet added a web layer to the flight-app application. We will see in Chapter 4, Web Application Development with Spring Web MVC and Chapter 5, Web Application Development with GWT, Flex, and Spring Web Flow how we go about creating a web application using Spring Roo.