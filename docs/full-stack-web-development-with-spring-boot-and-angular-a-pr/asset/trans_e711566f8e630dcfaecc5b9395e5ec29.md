

```
<scope>runtime</scope>
</dependency>

<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <scope>runtime</scope>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
</dependencies>
```

And for a Spring application using Gradle, we will add the dependencies as follows:

```
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    runtimeOnly 'com.h2database:h2'
    runtimeOnly 'org.postgresql:postgresql'
}
```

IntelliJ will automatically recognize the added dependencies and install them for the project, and we can successfully build and run the Spring Boot application on the fly.

We have learned how to add Spring Data JPA and PostgreSQL Driver to our Spring Boot application. In the next section, we will learn how to connect our Spring Boot application to our PostgreSQL database.

## Connecting to a database

We have already configured our PostgreSQL database and initialized our Spring Boot application with the needed dependencies. Now, we will learn how to connect our PostgreSQL to our application. There are two ways we can connect to our database – the first is through Spring JDBC, and the other is Spring Data JPA. Spring Data JPA is the most convenient way to connect to our database, but we will demonstrate both methods in this section.

## Configuring the database properties

The first thing we need to do is configure the database properties in our Spring Boot application. We need to specify the server URL of the database, the admin username, and the password by adding the following source code to the application.properties file:

```
spring.datasource.url=jdbc:postgresql://localhost:5432/springDB
spring.datasource.username=postgres
spring.datasource.password=password
```

In the preceding example, we can see that we have configured the basic connection settings for our PostgreSQL. springDB on the URL will be the name of the database in PostgreSQL, which should already exist on our server.

## Connecting using Spring JDBC

The first method to connect to the database is by using Spring JDBC. We will add an additional dependency to our application for us to use this method.

To add JDBC, we will add the following code to our pom.xml (Maven) or build.gradle (Gradle) files:

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

After successfully adding the JDBC dependency, we can now use JdbcTemplate to execute queries on our application:

```
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.jdbc.core.JdbcTemplate;

@SpringBootApplication
public class AwesomeJavaProject {
    @Autowired
    private JdbcTemplate jdbcTemplate;
    public static void main(String[] args) {
```

```
SpringApplication.run(AwesomeJavaProject.class,
    args);
}
@Override
public void run(String... args) throws Exception {
    String sql = "INSERT INTO blog (title, author,
    body) VALUES ("+ "'Awesome Java Project',
    'Seiji Villafranca', 'This is an
    awesome blog for java')";

    int rows = jdbcTemplate.update(sql);
}
}
```

In the preceding example, we can execute database statements such as INSERT in our application and call the update() method to modify data in the database.

## Connecting using Spring Data JPA

The second method is by using the Spring Data JPA plugin. The first step we need to take is to add additional details to the application.properties file:

```
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.properties.hibernate.format_sql=true
```

After adding the new settings, we can now create Entity and Repository for a specific table in our application – for example, we have a Blog table:

```
package net.codejava;
import javax.persistence.*;
@Entity
@Table(name = "blog")
public class Blog {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
```

```
private Integer id;

private String title;
private String body;
private String author;
}
```

In the preceding example, we have created a Blog class and annotated it with @Entity and @Table to indicate that this is an object connected to our database table:

```
package net.codejava;
import org.springframework.data.jpa.repository.JpaRepository;

public interface BlogRepository extends JpaRepository<Blog,
Integer> {
}
```

After creating our entity, we have made the repository for the blog, which can be extended by the repositories provided by JPA. BlogRepository can now be injected into our services or controllers to read, add, modify, or delete data on our database.

## Summary

That brings you to the end of this chapter. Let’s have a recap of the valuable things you have learned. You learned how to set up PostgreSQL on your local machine using the installer or Docker container.

You have also learned about the concepts and advantages of Spring Data JPA in Spring Boot and how to add it to your application, which is helpful in creating services with CRUD capabilities with less boilerplate code.

Last but not least, you learned how to connect your Spring Boot application with the PostgreSQL database using JDBC and Spring Data JPA.

In the next chapter, we will be learning how to start our server, how to add controllers, models, and services in our code, and about Redis for caching.

## Building APIs with Spring

In the previous chapter, you learned about the concepts and advantages of PostgreSQL and set it up on your local machine using the installer or the Docker container. You know how to configure Spring Data Java Persistence API (Spring Data JPA) on our project and use its provided repositories to perform Create, Read, Update, and Delete (CRUD) operations on our database with less boilerplate code. Lastly, you have also learned to connect your application with PostgreSQL using the Java Database Connectivity (JDBC) driver and Spring Data JPA.

This chapter will create your Spring Boot application programming interface (API) project; we will focus on coding, making our models, and adding controllers and services to develop our endpoints. We will also add Remote Dictionary Server (Redis) for caching to help improve our application performance.

In this chapter, we will cover the following topics:
- Starting the server
- Adding models
- Writing services
- Adding controllers
- Adding Redis for caching

## Technical requirements

There are no technical requirements for this chapter.

The link to the finished version of this chapter can be found here:

```
https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-05/superheroes
```

## Starting the server

In this section, we will now attempt to run our Spring Boot application on our server but first, let’s have a recap of the previous chapter: we learned how to configure Spring Data JPA and connect our application to our PostgreSQL database, and—most importantly—we have installed all of the needed dependencies in our application. These are all prerequisites before running the Spring Boot application.

In the following examples, we will be using an application named `spring-boot-superheroes`. We will generate a new Spring Boot application using Spring Initializr with the same dependencies. Throughout developing our application, we will also show you several ways to write the different parts of your API, such as how to write models, services, and controllers. These are the most common ways used now in the industry. Still, first things first: let’s proceed with running our Spring Boot application.

We assume that you have already generated your Spring Boot application with the needed dependencies. However, if you have missed this part or are unsure whether all dependencies are included on your generated project, let’s list again all the dependencies we installed in the previous chapter, as follows:
- **Spring Data JPA**: Dependency for adding Spring Data JPA used for built-in data store-related implementation.
- **PostgreSQL Driver**: A JDBC and **Reactive Relational Database Connectivity (R2DBC)** driver that will allow the connection of Java applications to the PostgreSQL database.
- **H2 Database**: An in-memory database that supports JDBC API and R2DBC access; this is commonly used for unit testing.

If you have successfully initialized your application with the listed dependencies, open your project in your preferred **integrated development environment (IDE)**; we will be using IntelliJ for Spring Boot in the following examples. Then, proceed as follows:
1. Expand the project folder; we will see several folders inside, as indicated in the following screenshot:

