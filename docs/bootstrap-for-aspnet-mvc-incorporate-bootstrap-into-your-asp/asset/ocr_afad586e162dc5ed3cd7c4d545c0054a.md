```
public static bool IsServiceAvailable<TService>(this IServiceProvider provider) where TService : class
{
    if (provider == null)
    {
        throw new ArgumentNullException("provider");
    }
    return GetService<TService>(provider) != null;
}
```

11. Save and close the file, and open the CustomViewModel.cs file. This file is located inside the UI folder and is used to pass data between UIs, where the user will choose the settings and our code.

12. Add a new property called ProjectDbContexts to it. This property will return all valid DbContext objects in the project. The code for this property will be as follows:

```
public IEnumerable<ModelType> ProjectDBContexts
{
    get
    {
        ICodeTypeService codeTypeService =
            (ICodeTypeService)Context
                .ServiceProvider.GetService(typeof(ICodeTypeService));

        return codeTypeService
            .GetAllCodeTypes(Context.ActiveProject)
            .Where(codeType =>
                codeType.IsValidDbContextType())
            .Select(codeType => new ModelType(codeType));
    }
}
```

13. Next, add three properties called Title, ControllerName, and ViewName to the class:

```
public string Title { get; set; }
public string ControllerName { get; set; }
public string ViewName { get; set; }
```

14. Lastly, add a ModelType property called SelectedDbContext:
public ModelType SelectedDbContext { get; set; }

15. Save and close the file.