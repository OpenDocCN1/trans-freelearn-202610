More about the exception can be found from the **Locals** panel by clicking on the magnify icon from the **Exceptions** section:

![](bbox=[0.13, 0.15, 0.87, 0.34])

Figure 13.52 - The Exceptions window

From the exception, we can make out that it is **InvalidCastException**, which occurs when the conversion of an instance of one data type to another is not supported. In the **Type Into** activity for entering the age, the **CInt ()** function is used for converting **String** to **Integer**. Since the typecasting couldn't happen, the exception was thrown:

+   1. Apply a conditional breakpoint at this **Type Into** activity with the condition as **Not age.IsNumeric** and restart the debugging process:

![](bbox=[0.15, 0.56, 0.85, 0.79])

Figure 13.53 - A conditional breakpoint on the Type Into activity

The execution will then go into a paused state when the age cannot be converted to an integer. As expected, the debugging goes into a paused state while entering the age of the third person without throwing an exception.