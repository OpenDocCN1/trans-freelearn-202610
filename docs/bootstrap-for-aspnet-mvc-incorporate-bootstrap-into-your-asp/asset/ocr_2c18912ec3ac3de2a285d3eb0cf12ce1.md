Each of these items generates boilerplate code, which you can customize afterwards. However, let's assume that all our projects' controller classes need a constructor that accepts a parameter. Using the standard items, we would have to manually add the constructor for each controller in our project.

Fortunately, we are able to override the generated output for the controllers, because in essence, they are nothing more than T4 templates.

To customize the output of the MVC 5 Controller – Empty item, perform the following steps:

+   1. Right-click on your ASP.NET MVC project's name inside Visual Studio, and navigate to Add | New Item.
+   2. Navigate to Visual C# | Web | SideWaffle and select the ASP.NET Scaffolding T4 files item template and click on Add:

![](bbox=[0.179, 0.387, 0.821, 0.668])