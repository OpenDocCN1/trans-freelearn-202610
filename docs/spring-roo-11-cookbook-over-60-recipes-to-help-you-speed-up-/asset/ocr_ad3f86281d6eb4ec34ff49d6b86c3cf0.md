Spring Roo 1.1.3 generates pom.xml, which makes project dependent on GAE SDK 1.4.0 and Maven GAE plugin 0.5.7, as shown in the listing we just saw. At the time of writing this book, the current version of GAE SDK is 1.5.1 and that of Maven GAE plugin is 0.8.4. To change the version of GAE SDK, modify the gae.home property. And, to change the version of Maven GAE plugin, simply modify the value of the <version> sub-element of the <plugin> element, which configures Maven GAE plugin. If you are using Spring Roo 1.1.5, then the project already uses GAE SDK 1.5.1 and Maven GAE plugin 0.8.4.

The following table specifies some of the goals defined by the Maven GAE plugin:

| Goal | Description |
|---|---|
| gae:run | Runs the project locally on the GAE development web server |
| gae:deploy | Uploads the application to the GAE server |
| gae:logs | Retrieves application logs from the GAE server |
| gae:version | Shows the plugin and GAE SDK versions |

Let's now look at the Maven DataNucleus plugin and the role it plays in the GAE application.

# # Maven DataNucleus plugin

To make a class persistent, DataNucleus expects that the class must implement the PersistenceCapable interface of JDO. Why are we talking about JDO now? Well, it's because DataNucleus support for JPA is built on top of JDO. This means that even if you have annotated your domain classes with the @Entity JPA annotation, DataNucleus can't persist them. To free developers from implementing the PersistenceCapable interface in their domain classes, DataNucleus provides an enhancer, which works on the compiled domain classes and implements PersistenceCapable interface via bytecode enhancement. The Maven DataNucleus plugin provides a datanucleus:enhance goal, which enhances JPA classes annotated with the @Entity annotation. The following code shows this:

```
<plugin>
<groupId>org.datanucleus</groupId>
<artifactId>maven-datanucleus-plugin</artifactId>
<version>1.1.4</version>
<configuration>
<mappingIncludes>**/*.class</mappingIncludes>
<enhancerName>ASM</enhancerName>
<api>JPA</api>
<mappingExcludes>**/GaeAuthFilter.class</mappingExcludes>
</configuration>
<execution>
<execution>
<phase>compile</phase>
<goals>
```