```
parameters,
skipIfExists: false);
}
}
```

+   21. In the preceding code, we added the values captured in the form by the user to a `parameters` collection. We also collected all the metadata about the selected object by using `IEntityFrameworkService`. This service expects a context type name as one of the parameters for its `AddRequiredEntity` method, so we'll pass in the name of the `DbContext` object the user selected on the form.
+   
+   22. We'll then check whether the user is adding the scaffolded item to a folder, and we'll make a small assumption that the parent folder of the selected folder will be the Views folder. We'll then invoke the `AddFileFromTemplate` method.
+   
+   23. With the UI and view model in place, open the `CustomTextTemplate.cs.t4` file.
+   
+   24. The top part of the template file specifies the template language, in this case, C# as well as the extension that the template needs to output. Since we're going to scaffold a view, this should be `.cshtml`.
+   
+   25. We'll also declare all the variables names that will be passed into the template at the top. The following should be added to the top of the template file:

```
<#@ template language="C#" #>
<#@ output extension=".cshtml" #>
<#@ assembly name="System.Core" #>
<#@ assembly name="EnvDTE" #>
<#@ include file="Imports.include.t4" #>
<#@ parameter name="ModelType" type="EnvDTE.CodeType" #>
<#@ parameter name="Title" type="System.String" #>
<#@ parameter name="ControllerName" type="System.String" #>
<#@ parameter name="ViewName" type="System.String" #>
<#@ parameter name="ModelMetadata" type="Microsoft.AspNet.Scaffolding.Core.Metadata.ModelMetadata" #>
```

+   26. The rest of the T4 markup for the template is as follows:

```
@model <#= ModelType.Namespace.FullName #>.<#= ModelType.Name #>

@{
    ViewBag.Title = "<#= Title #>";
}
```