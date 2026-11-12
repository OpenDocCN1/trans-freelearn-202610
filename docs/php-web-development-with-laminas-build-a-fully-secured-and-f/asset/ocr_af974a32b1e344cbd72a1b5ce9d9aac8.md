10. There must be warnings in the code editor because we have not created the class `SchoolClass` yet. As we can see in Figure 3.7, the code editor shows a warning on the left- and right-hand sides. On the left-hand side, the editor shows an icon of a lamp with a white X inside a red square. On the right-hand side, the editor shows red rectangles:

![](bbox=[0.12, 0.18, 0.85, 0.39])

Figure 3.7 – The error warnings on either side

11. The icons on the left-hand side allow us to try and fix the errors. When you click on any of them, Eclipse will show suggestions for fixing. The rectangles on the right-hand side highlight the errors in the line.

This concludes the basic structure for the use case of managing school classes. Next, we will create tests before implementing more business code.

# Creating our first automated test

Let us now create our first automated test for checking whether the `SchoolClass::getAll` static method returns what we expect. But... what do we expect? A collection of arrays, each with the `code` and `name` keys.

Before creating the first test case, let's install **PHPUnit** into our project. We will use **Composer** for this; however, this time, we will use Composer inside Eclipse.

To install PHPUnit, follow these steps:

+   1. Right-click on the `school11` project and access the **Configure | Add Composer Support** menu, as shown in Figure 3.8.

![](bbox=[0.12, 0.8, 0.85, 0.875])

Figure 3.8 – Adding Composer Support