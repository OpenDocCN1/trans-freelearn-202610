> It is worth mentioning that neither JUnit nor Eclipse enforces this naming convention. The creation of separate folders/subpackages for tests is not mandatory either. However, following these conventions is a good programming practice. It is also worth mentioning that another widely used convention to organize test code is the maven convention. By following this convention, you need to create separate folders for regular Java code and test code, `src/main/java` and `src/test/java`. Also, the test classes are placed in the same package in which the classes they test are placed.

# Setting up JUnit

In order to use JUnit with Eclipse, you need to add `junit.jar` to your project's build path. To do this, right-click on your project, and navigate to **Build Path | Add Libraries...**, as shown in the following screenshot:

![](bbox=[0.293, 0.393, 0.707, 0.5])

The following screenshot shows the options listed by Eclipse. Select the second option **JUnit**. You can select either JUnit Version 3 or Version 4.

![](bbox=[0.257, 0.564, 0.743, 0.795])