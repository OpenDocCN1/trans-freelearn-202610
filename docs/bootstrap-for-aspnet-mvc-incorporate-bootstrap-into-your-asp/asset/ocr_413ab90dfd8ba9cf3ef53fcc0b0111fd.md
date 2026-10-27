# Creating T4 Templates to Scaffold Bootstrap Views

Visual Studio 2013 already includes code generators for ASP.NET MVC and Web API projects, and using these built-in scaffolding templates can drastically reduce the amount of time you spend on creating standard views and controller actions. When adding a standard controller to an ASP.NET MVC project in Visual Studio 2013, you're already using scaffolding, and this is indicated by the fact that the dialog window used to add a new controller's caption is **Add Scaffold**:

![](bbox=[0.179, 0.256, 0.824, 0.587])

# T4 templates

T4 stands for Text Template Transformation Toolkit, which is Microsoft's template-based text-generation framework. T4 is a combination of control logic and text blocks, which can generate any type of text file, including C# or VB source files.

The T4 code generator is built into Visual Studio and is used to generate the built-in ASP.NET MVC templates, including the standard MVC and Web API controller templates.