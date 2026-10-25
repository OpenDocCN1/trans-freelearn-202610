# # Kendo UI Editor

Here, the name of the tool is `custom`, and it contains a function, `exec`. The `exec` function is executed when the user clicks on the custom tool, as shown in the following screenshot. The function first gets a reference of the Editor widget and then calls the `exec` function on the Editor instance. The function takes two parameters, namely the command name and an object that contains a set of parameters for the executed command. In the previous example, the `inserthtml` command is used to insert text in the Editor widget. The text that should be inserted is specified as a second parameter.

![](bbox=[0.133, 0.228, 0.907, 0.442])

The command name to be used should be one of the Editor commands.

# # Using the image browser tool to insert images into the Editor

One of the tools available in the Editor toolset is the image browser. The image browser tool allows you to insert an image into the Editor. It asks the user for the image URL and an optional alternate text by displaying the dialog box, as shown in the following screenshot:

![](bbox=[0.276, 0.705, 0.761, 0.899])