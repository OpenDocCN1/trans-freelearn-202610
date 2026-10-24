
Let's look at a basic code example:

```
package com.springexample;

/* Class for Student */
public class Student {
    private Grades grades;

    public void setGrades(Grades grades) {
        this.grades = grades;
    }
    public Grades getGrades() {
        return grades;
    }
    public void retrieveGrades() {
        grades.getGrades();
    }
}
```

In the preceding example, we can see that we have created a setter method named setGrades(), which accepts a Grades object, and its primary function is to set a value for the grades dependency.

Instead of using the constructor with arguments, we use setters to inject our dependencies into our object.

To use the Student object, let's see the following example:

```
package com.springexample;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Main {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");

        Student student = (Student) context.getBean("student");
```

```
student.retrieveGrades();
}
}
```

We can see in the preceding example that it’s the same as how we used setter-based objects and constructor-based objects. The difference here is how we configure our beans in Bean.xml.

Let’s see the Beans.xml example for setter-based dependency injection:

```
<?xml version = "1.0" encoding = "UTF-8"?>

<beans xmlns = "http://www.springframework.org/schema/beans"
    xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation = "http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/ spring-beans-3.0.xsd">

    <!-- Definition for student bean -->
    <bean id = "student" class = "com.springexample.Student">
        <property name="grades" ref = "grades"/>
    </bean>

    <!-- Definition for grades bean -->
    <bean id = "grades" class = "com.springexample.Grades"></bean>

</beans>
```

We configured the beans in our Beans.xml, Student, and the Grades object in the preceding example. The only difference here is when we declare dependencies. We use the property tag instead of constructor-arg to define our dependencies.

We have successfully created our object with setter-based dependency injection, and now, we will discuss field-based dependency injection.

## Field-based dependency injection

As the name suggests, **field-based dependency injection** is a concept where we inject the object’s dependencies directly into the fields. We will not create a constructor or a setter method to inject our dependencies, but we will use the @Autowired annotation for injection.

Let’s see the following example of injecting dependencies into a field:

```
package com.springexample;

/* Class for Student */
public class Student {
    @Autowired
    private Grades grades;
}
```

In the preceding example code, we can see that we didn’t create a constructor or a setter method to inject our dependency. Instead, we only used the @Autowired annotation to inject the Grades object.

The field-based injection may be clean at first glance, having only annotations in our code and fewer methods, but many things happen behind our @Autowired dependency. For example, it uses reflection to inject dependencies that are costlier than a constructor and setter-based injection; it also violates the **single responsibility principle**. We can add more dependencies directly in the fields without warning.

We have learned the basics of dependency injection and the different ways to implement it in our Java application. Now, we will discuss the concept and importance of annotations and beans in Spring.

## Annotation and beans

**Annotation and beans** are essential parts of developing your Spring applications. They are considered the building blocks of Spring and make our code less boilerplate and maintainable.

**Spring annotations** are used to define the different types of beans. They are simply a form of metadata that marks our code to provide information. Conversely, **beans** are objects that are instantiated and created and can be injected with other beans. We will discuss more as we go through this section.

## Types of annotations

Annotations in Spring are categorized into different types depending on their functionality. The following are annotations grouped into their respective categories.

## Core annotations

**Core annotations** are used to leverage the Spring DI engine in our applications. They can be found in the `org.springframework.beans.factory.annotation` and `org.springframework.context.annotation` packages. The following is a list of core annotations:

- `@Required`: This is applied in the setter methods of a bean and implies that the bean must be injected with the dependency at configuration. Otherwise, it will throw `BeanInitializationException`. Let’s look at the following example of how to use the `@Required` annotation:

```
public class Car
{
private String brand;
@Required
    public void setBrand(String brand)
    {
        this.brand = brand;
    }
    public Integer getBrand()
    {
        return brand;
    }
}
```

In the preceding example, we can see that the `setBrand()` method was annotated with `@Required`; this indicates that the brand must be populated on initialization.

- `@Autowired`: We have encountered the `@Autowired` annotation several times in DI, and this is mainly used to inject dependencies without the use of constructors and setter methods. Let’s look at the following example of how to use the `@Autowired` annotation:

```
package com.springexample;

public class Car {
    @Autowired
    private Brand brand;
}
```

We can see in the preceding example that `@Autowired` is applied directly in the field. This is because the annotations use reflection to inject dependencies, with more processes involved than constructors and setter methods.

- @ComponentScan: The annotation is a class-level annotation to indicate the packages we want to be scanned for beans. @ComponentScan can accept arguments about what specific packages are to be scanned, and not providing any will allow the current packages and all sub-packages. Let’s look at the following example of how to use @ComponentScan:

```
@Configuration
@ComponentScan
public class SpringApp
{
    private static ApplicationContext applicationContext;
    @Bean
    public SpringBean springBean()
    {
        return new SpringBean();
    }
    public static void main(String[] args) {
        applicationContext = new AnnotationConfigApplicationContext(SpringComponentScanApp.class);
    }
}
```

We can see in the preceding example that the @ComponentScan app is applied to the Spring App class, and it is usually implemented together with the @Configuration annotation. Let’s say that SpringApp is found under the com.example.spring.app package; this will scan the package and its sub-packages if there are existing beans.

- @Configuration: The annotation is also a class-level annotation to indicate that a class is the source of bean definitions that the Spring container will process at runtime. Let’s look at the following example of how to use the @ComponentScan annotation:

```
@Configuration
public class SpringApp {
    @Bean(name="demoBean")
    public DemoBean service()
    {
    }
}
```

We can see in the preceding example that the @Configuration annotation is applied to the SpringApp class, which indicates that SpringApp will be the source of beans.

- @Bean: The annotation is a method-level annotation, and it is used to tell a method to produce a bean. Let’s look at the following example of how to use the @Bean annotation:

```
@Configuration
public class AppConfig {
    @Bean
    public BeanExample beanExample() {
        return new BeanExampleImpl();
    }
}
```

In the preceding example, the @Bean annotation is applied to the beanExample method. Once JavaConfig encounters the method, it will be executed and register the return value as a bean in BeanFactory, and the name will be the same as the method name when none is specified.

The @Bean annotation can also be configured in Spring XML, and the equivalent configuration is the following:

```
<beans>
    <bean name="transferService" class="com.acme.TransferServiceImpl"/>
</beans>
```

## Stereotype annotations

Stereotype annotations are mainly used to create Spring beans on the fly in an application context.

The following is a list of stereotype annotations:

- @Component: This is the primary stereotype annotation. As with the @Bean annotation, the @Component annotation is used to define a bean or a Spring component. The difference between the two is that @Component is applied at the class level while @Bean is applied at the method level.

The other difference is that the @Component class cannot also be used to create a bean if the class is outside the Spring container, whereas we can create a bean using @Bean even if the class is found outside the Spring container. Let’s look at the following example of how to use the @Component annotation:

```
@Component
public class Car
```
