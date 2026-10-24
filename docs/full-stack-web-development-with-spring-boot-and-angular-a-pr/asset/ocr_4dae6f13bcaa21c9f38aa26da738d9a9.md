In the preceding example, we have defined the Student and Grades object as beans. The only difference is that the Student object has a constructor-arg that references grades; this indicates that we are injecting the Grades object into our Student object.

We have already achieved constructor-based dependency by using the Beans.xml configuration. We can also use annotations directly in our code to configure our beans and their dependencies.

Let's look at the following example of how to configure beans and dependencies with annotations:

```
@Configuration
public class AppConfig
{
    @Bean
    public Student student() {
        return new Student(grades());
    }

    @Bean
    public Grades grades() {
        return new Grades();
    }
}
```

We can see in the preceding example that instead of using XML, we have used annotations to identify our beans and configuration. For example, the @Configuration annotation indicates that the AppConfig class is the source of the bean definitions, and the @Bean annotation defines the bean in our application. We will discuss annotations and beans intensely as we go throughout this chapter.

We have successfully learned how to implement constructor-based dependency injection by using Bean.xml and annotations. Now, let's move on to the implementation of setter-based dependency injection.

# Setter-based dependency injection

The injection of dependencies can be achieved when the container calls the setter methods of our class. So, instead of creating a constructor for the class, we will create a function that will set the object's dependency.