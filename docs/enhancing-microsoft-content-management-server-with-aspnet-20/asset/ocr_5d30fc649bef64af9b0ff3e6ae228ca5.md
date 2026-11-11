After the conversion process, the <link> tag will be rendered as:

```
<link href="/tropicalgreen/App_Themes/Sunny/Sunny.css" type="text/css" rel="stylesheet"/>
```

# Creating the CorrectThemes HTTP Module

Let’s create the HTTP module that corrects the URL stored in the <link> tags.

+   1. Add a new Visual C# Class Library project to the solution. Name the project **TropicalGreenHttpModules**.
+   2. Delete the `Class1.cs` file created by the wizard. We won’t need it.
+   3. Add the following reference required by the project:
+   ```
+   Microsoft.ContentManagement.Publishing.dll
+   ```
+   (The library file is located in the `<install directory>\Microsoft Content Management Server\Server\bin\` directory).
+   As well as the following references from the `System` namespace:
+   ```
+   System.Web
+   ```
+   4. Add a class file to the **TropicalGreenHttpModules** project. Name the new class file **CorrectLinksInThemes.cs**.
+   5. Add the following using statements above the namespace declaration:
+   ```
+   using System;
+   . . . code continues . . .
+   
+   using System.Web;
+   using System.Web.UI;
+   using System.Web.UI.HtmlControls;
+   using Microsoft.ContentManagement.Publishing;
+   
+   namespace TropicalGreenHttpModules
+   {
+       . . . code continues . . .
+   }
+   ```
+   6. As we are creating an HTTP module, the class will implement the `IHttpModule` interface. Add a colon followed by the interface name as shown in the following code:
+   ```
+   public class CorrectLinksInThemes : IHttpModule
+   {
+   }
+   ```