```
public class Main {
    public static void main(String[] args) {
        ApplicationContext context =
            new ClassPathXmlApplicationContext("Beans.xml");

        Student student =
            (Student) context.getBean("student");
        student.retrieveGrades();
    }
}
```

We can see in the preceding example that we have instantiated a new student in our main class by getting the bean of our Beans.xml file. The Beans.xml file is our main configuration file for our construction-based injection, which is where we will define our beans together with their dependencies.

Let’s examine the following example of what Beans.xml looks like:

```
<?xml version = "1.0" encoding = "UTF-8"?>

<beans xmlns =
    "http://www.springframework.org/schema/beans"
    xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation =
    "http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/
    spring-beans-3.0.xsd">

    <!-- Definition for student bean -->
    <bean id = "student"
        class = "com.springexample.Student">
        <constructor-arg ref = "grades"/>
    </bean>

    <!-- Definition for grades bean -->
    <bean id = "grades"
        class ="com.springexample.Grades"></bean>

</beans>
```