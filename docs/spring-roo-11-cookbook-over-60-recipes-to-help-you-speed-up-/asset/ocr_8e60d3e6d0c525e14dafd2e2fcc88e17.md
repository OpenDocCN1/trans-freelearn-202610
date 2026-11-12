![](bbox=[0.213, 0.15, 0.783, 0.525])

This screenshot shows that the `toString` method defined in `Flight_To_String.aj` file will be added to `Flight.java` file. Also, notice that if we continue with this refactoring it will result in deletion of `Flight_Roo_ToString.aj` file. If you want to keep the empty `Flight_Roo_ToString.aj` file, then uncheck `Delete ...Flight_Roo_ToString.aj` option.

+   8. Click **OK** button to complete the refactoring. You’ll now see, that the `toString` method defined in `Flight_Roo_ToString.aj` AspectJ ITD has been moved to `Flight.java` file.

+   9. Optionally, if you want to revert back the refactoring, simply select the **Edit | Undo** option of Eclipse IDE.

# How it works...

Push-in refactoring is like any other refactoring mechanism provided by IDEs. Based on the AspectJ ITD declaration, IDE figures out the target of the declaration, and moves the code from the ITD file to the target Java source file.