# Regions

Regions are used by Visual Studio to organize code into logical groups. They are not required in your code, but they are recommended. In the recipes of this chapter you will see several regions created by the Starter Kit (such as Private Members or Public Methods) and you will find them helpful when you are trying to find a routine that needs updating.

# Displaying labels from the resource file

Data entry forms in DNN often have labels to identify the fields on the page. These are special controls that display text, but also have a "help" icon that displays an explanation when clicked.

But where does this text come from? We could hardcode the text for the labels in the code, but this would limit our ability to update the text easily and we would miss out on the powerful feature of Localization provided by DNN. Localization is a way for DNN to automatically translate labels and system messages based on the language selected by the user. You can learn more about localization from the recipes in Chapter 9.

For this recipe we will store the label text and Help explanation in the resource file and link them to the labels through a unique ID.

# Getting ready

For this recipe we will use the EditEmployee control from the sample Employee project. To follow along you must have completed the following recipe from Chapter 4:

+   Creating a new module with the Starter Kit

# How to do it...

+   1. Launch the Development Tool and load the Employee project.
+   2. Double-click to open the EditEmployee.ascx file.
+   3. The recipe Building an Edit control in Chapter 5 created five sets of controls for editing values. Here is an example of the controls to edit Salary. Note the label control lblSalary:

```
<tr valign="top">
<td class="SubHead" width="125">
<dnn:label id="lblSalary" runat="server" controlname="lblSalary" suffix=":">
</dnn:label>
</td>
```