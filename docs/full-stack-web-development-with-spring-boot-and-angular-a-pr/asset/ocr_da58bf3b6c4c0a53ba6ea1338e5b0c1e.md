# Authentication and authorization in Spring Boot

We have already discussed the concepts of Spring Security in the previous section; now, we will learn how to integrate Spring Security into our Spring Boot application. As we move on to the examples, we will be using all the modules and features of Spring Boot Security.

Authentication and authorization are the most common concepts that we come across when we implement security in our applications. These are the two validations we apply for our application to be secure.

# Configuring Spring Boot and implementing authentication

We will first implement authentication in our application. We first need to add the Spring Boot Security dependency to our project. To add the dependency, we will add the following to pom.xml:

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

Reload the project to install the new dependency and run the server. Let's try to visit localhost:8080 to open the Spring Boot application project in the browser. As we can see, a login page is now applied to our project as we've installed Spring Boot Security:

![](bbox=[0.22, 0.54, 0.795, 0.72])

Figure 7.1 – Login page integrated from Spring Boot Security

To create credentials for the login, we can configure the username and password under the application.properties file by placing the following setting:

```
spring.security.user.name=admin
spring.security.user.password=test
```