The following listing shows the content of the flightapp_flex_scaffold-config.xml file:

```
```xml
<flex-config xmlns="http://www.adobe.com/2006/flex-config">
    <includes append="true">
    </includes>
</flex-config>
```
```

As this code shows, flightapp_flex_scaffold-config.xml doesn't do anything interesting. In the Scaffolding Flex application from JPA entities recipe, we'll discuss this file in detail once it's updated after the execution of the flex remoting all or flex remoting scaffold command.

# There's more...

We mentioned earlier that Roo configures the Flexmojos Maven plugin in the `pom.xml` file of the `flightapp_flex` project. Let's now look at the Flexmojos plugin configuration in detail.

# Flexmojos Maven plugin configuration

Flexmojos Maven plugin offers many features, but for brevity we'll focus only on the `flexmojos:compile-swf` goal, which is responsible for compiling the Flex project's sources (MXML and ActionScript files) and package it into an SWF file. The following listing shows the configuration of Flexmojos Maven plugin in the `pom.xml` file of the `flightapp_flex` project:

pom.xml

```
```xml
<plugin>
    <groupId>org.sonatype.flexmojos</groupId>
    <artifactId>flexmojos-maven-plugin</artifactId>
    <version>3.7.1</version>
    <executions>
        <execution>
            <id>compile-scaffold-swf</id>
            <phase>process-resources</phase>
            <goals>
                <goal>compile-swf</goal>
            </goals>
            <configuration>
                <incremental>true</incremental>
                <sourceFile>
                    ${basedir}/src/main/flex/${project.name}_scaffold.mxml
                </sourceFile>
                <sourcePaths>
                    <path>${basedir}/src/main/flex</path>
                </sourcePaths>
                <output>
```
```