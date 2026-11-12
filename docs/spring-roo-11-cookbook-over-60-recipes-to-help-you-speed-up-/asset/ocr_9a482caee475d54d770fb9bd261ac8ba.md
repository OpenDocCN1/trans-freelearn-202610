Spring Roo distribution comes with a **Maven add-on**, which is responsible for processing the `project` command. There are more Roo commands that are processed by the Maven add-on, which you will see later in this book.

The output of `project` command shows that it creates directories with name `SRC_MAIN_JAVA`, `SRC_MAIN_WEBAPP`, and so on. These are logical names given to standard directories created by the Maven add-on. The following table shows the directories to which each of these names map in the case of the `flight-app` project:

| Path value | Project directory |
| :--- | :--- |
| SRC_MAIN_JAVA | Refers to the root of the Java source directory, which contains application's Java sources: ch01-recipe\src\main\java. |
| SRC_MAIN_RESOURCES | The root of the directory, which contains resources (Spring's application context XML, database properties file, log4j properties file, persistence.xml file, and so on) required by the Java enterprise application: ch01-recipe\src\main\resources. |
| SRC_TEST_JAVA | The root of the Java Source directory, which contains unit and integration tests: ch01-recipe\src\test\java. |
| SRC_TEST_RESOURCES | The root of the directory, which contains resources required during unit and integration testing: ch01-recipe\src\test\resources. |
| SRC_MAIN_WEBAPP | The web application directory, which contains web pages, images, style sheets, and web application configuration: ch01-recipe\src\main\webapp. |
| ROOT | Refers to the root directory of the project, which is ch01-recipe in case of flight-app project. |
| SPRING_CONFIG_ROOT | Refers to the directory, which contains Spring's application context XML file. In the context of flight-app project this refers to ch01-recipe\src\main\resources\META-INF\spring. |

As evident from the execution of the `project` command, not only did it create a maven-ized project, it also created Spring's `applicationContext.xml`, maven's `pom.xml` file, and a `log4j.properties` file. The following XML code shows the contents of the `applicationContext.xml` file, which contains some interesting details:

```
<beans ...>

<context:property-placeholder
     location="classpath*:META-INF/spring/*.properties"/>

<context:spring-configured/>

<context:component-scan base-package="sample.roo.flightapp">
    <context:exclude-filter
         expression=".*_Roo_.*" type="regex"/>
```