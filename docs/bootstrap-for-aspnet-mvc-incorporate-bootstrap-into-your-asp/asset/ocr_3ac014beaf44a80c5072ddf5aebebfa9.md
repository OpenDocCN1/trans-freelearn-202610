# Creating a custom scaffolder extension

If you need more control on how the code for your views or controllers is scaffolded, you need to create a new scaffolding extension for Visual Studio. This extensibility enables developers to add their own scaffolding items to the Add Scaffold dialog window, as illustrated in the following screenshot:

![](bbox=[0.178, 0.25, 0.822, 0.531])

To create such a custom scaffolder, we'll create a new basic scaffolder project. This project template is installed by the SideWaffle extension and adds all the files necessary to create a custom scaffolder.

Perform the following steps to create your own custom scaffolder project:

+   1. Create a new project inside Visual Studio.
+   2. Select the Basic Scaffolder project template by navigating to Extensibility | SideWaffle, as shown in the following screenshot: