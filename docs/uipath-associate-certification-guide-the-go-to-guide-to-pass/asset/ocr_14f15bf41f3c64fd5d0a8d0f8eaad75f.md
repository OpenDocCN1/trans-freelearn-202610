UiPath uses string manipulation methods in **Visual Basic .NET (VB.NET)** or C#, depending on the language you choose to use in UiPath Studio. In this chapter, we will use the methods in VB.NET. The following sections describe some of the commonly used string manipulation methods in UiPath.

# Concat function

The Concat function enables the user to concatenate two or more string representations into one string representation. The Concat function is capable of concatenating up to four string objects at a time. The following code snippet shows the expression we use to concatenate strings:

```
String.Concat(StrVariable1, StrVariable2)
```

Let’s take a look at a simple example of how to use this function. Proceed as follows:

+   1. Open UiPath Studio and create a new project.
+   2. Once the project is ready, double-click on the Main.xaml file on the **Project** panel to open it in the **Designer** panel.
+   3. Click on the **Activities** panel, search for a **Sequence** activity, and drag it to the **Designer** panel.
+   4. Select the **Sequence** activity you added, and click on the **Variables** panel.
+   5. Create a new variable named DisplayText of type String.
+   6. Drag and drop an **Assign** activity and assign the following value to the DisplayText variable: "Hello... My name is".
+   7. Create another String variable named FirstName.
+   8. Use another **Assign** activity below the previous and assign a FirstName variable with the value "John".
+   9. Create a third String variable named LastName.
+   10. Use a third **Assign** activity below the previous one and assign a LastName variable with the value "Doe".
+   11. Now, search for a **Message Box** activity in the **Activities** panel and drag and drop it after the last **Assign** activity.
+   12. Configure your **Message Box** activity with the following code:

```
String.Concat(DisplayText, FirstName, LastName)
```