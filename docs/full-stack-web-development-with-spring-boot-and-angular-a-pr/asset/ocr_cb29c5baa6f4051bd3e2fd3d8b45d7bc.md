After successfully configuring Logback, we can run the application and see the logs in our console:

```
2022-03-29 12:04:06,487 INFO  [background-preinit] org.hibernate.validator.internal.util.Version: HV000001: Hibernate Validator 6.2.0.Final
2022-03-29 12:04:06,728 INFO  [restartedMain] org.springframework.boot.StartupInfoLogger: Starting SuperheroesApplication using Java 17 on LAPTOP-NEIIT559 with PID 9512 (C:\Users\Sei
2022-03-29 12:04:06,728 DEBUG [restartedMain] org.springframework.boot.StartupInfoLogger: Running with Spring Boot v2.6.2, Spring v5.3.14
2022-03-29 12:04:06,729 INFO  [restartedMain] org.springframework.boot.SpringApplication: No active profile set, falling back to default profiles: default
2022-03-29 12:04:06,805 INFO  [restartedMain] org.springframework.boot.logging.DeferredLog: Devtools property defaults active! Set 'spring.devtools.add-properties' to 'false' to disa
2022-03-29 12:04:06,805 INFO  [restartedMain] org.springframework.boot.logging.DeferredLog: For additional web related logging consider setting the 'logging.level.web' property to 'D
2022-03-29 12:04:06,376 INFO  [restartedMain] org.springframework.data.repository.config.RepositoryConfigurationDelegate: Multiple Spring Data modules found, entering strict repositio
2022-03-29 12:04:06,379 INFO  [restartedMain] org.springframework.data.repository.config.RepositoryConfigurationDelegate: Bootstrapping Spring Data JPA repositories in DEFAULT mode.
```

Figure 8.1 – Log events using Logback

We will now use the Log4j2 framework for our logs.

# Configuring Log4j2

We can also use a different framework for logging events in our application. In this example, we will use Log4j2 to handle our logs:

+   1. The first step is to add the Log4j2 dependency to our pom.xml file. To do this, we will add the following code:

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-Log4j2</artifactId>
</dependency>
```

+   2. After successfully adding the dependency, we must exclude the spring-boot-starter-logging dependency in our Spring Boot application, allowing us to override Logback and use the Log4j2 framework in logging events.

To do so, we must add the following XML code to the dependencies under the org.springframework.boot group:

```
<exclusions>
    <exclusion>
        <groupId>org.springframework.boot</groupId>
        <artifactId>
            spring-boot-starter-logging</artifactId>
    </exclusion>
</exclusions>
```

+   3. After including the Log4j2 dependency, we will add a file named log4j2-spring.xml to the resources folder and add the following XML configuration:

```
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
```