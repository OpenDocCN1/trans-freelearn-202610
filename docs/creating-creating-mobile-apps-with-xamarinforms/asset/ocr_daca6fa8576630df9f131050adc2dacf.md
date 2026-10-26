```
void AddChildrenToParent(ClassAndSubclasses parentClass,
    List<TypeInformation> classList)
{
    foreach (TypeInformation typeInformation in classList)
    {
        if (typeInformation.IsDerivedDirectlyFrom(parentClass.Type))
        {
            ClassAndSubclasses subclass =
                new ClassAndSubclasses(typeInformation.Type);
            parentClass.Subclasses.Add(subclass);
            AddChildrenToParent(subClass, classList);
        }
    }
}

void AddItemToStackLayout(ClassAndSubclasses parentClass, int level)
{
    // If assembly is not Xamarin.Forms, display full name.
    string name = parentClass.Type.Name;
    TypeInfo typeInfo = parentClass.Type.GetTypeInfo();

    if (typeInfo.Assembly != xamarinFormsAssembly)
    {
        name = parentClass.Type.FullName;
    }

    // If generic, display angle brackets and parameters.
    if (typeInfo.IsGenericType)
    {
        Type[] parameters = typeInfo.GenericTypeParameters;
        name = name.Substring(0, name.Length - 2);
        name += "<";

        for (int i = 0; i < parameters.Length; i++)
        {
            name += parameters[i].Name;
            if (i < parameters.Length - 1)
                name += ", ";
        }
        name += ">";
    }

    // Create Label and add to StackLayout
    Label label = new Label
    {
        Text = String.Format("{0}{1}", new string(' ', 4 * level), name),
        TextColor = parentClass.Type.GetTypeInfo().IsAbstract ?
            Color.Accent : Color.Default
    };

    stackLayout.Children.Add(label);

    // Now display nested types.
```