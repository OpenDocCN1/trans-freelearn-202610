```
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
<version>5.3.5</version>
</dependency>
```

Spring Boot only requires spring-boot-starter-web, which is a Spring Starter for our application to run. The necessary dependencies are added automatically at build time as the starter will be responsible for the configuration:

```
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
<version>2.4.4</version>
</dependency>
```

Another thing to consider in Spring is that we need to define some configurations, such as dispatcher servlets and mappings, for our application to run on the server:

```
public class SpringInitializer implements WebApplicationInitializer {
@ Override
public void onStartup(ServletContext container) {
AnnotationConfigWebApplicationContext context =
    new AnnotationConfigWebApplicationContext();
context.setConfigLocation("com.springexample");
container.
addListener(new          ContextLoaderListener(context));
ServletRegistration.Dynamic dispatcher =
    container. addServlet("dispatcher",
        new  DispatcherServlet(context));
dispatcher.setLoadOnStartup(1);
dispatcher.addMapping("/");
    }
}
```

After initializing the dispatcher servlets, we also need to use @EnableWebMvc and have a Configuration class with a @Configuration annotation where we will instantiate a view resolver for the applications.