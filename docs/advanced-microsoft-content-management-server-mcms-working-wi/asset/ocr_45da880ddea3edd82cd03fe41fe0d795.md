```
lblErrorMessage.Text = "You do not have rights to submit" + " a template.";
}
}
else
{
    lblErrorMessage.Text = "You do not have rights to create a template.";
}
}
catch(Exception ex)
{
    // rollback all changes
    cmsContext.RollbackAll();
    // the CMS context needs to be disposed of after a rollback
    cmsContext.Dispose();
    // display error message
    lblErrorMessage.Text = ex.Message;
}
```

The code uses the Template.CreateCustomPropertyDefinition() method to add a custom property definition to the template. Once the custom property has been created, we assign its property values.

> When creating a template and adding custom property definitions to it with the PAPI, you must call CommitAll() somewhere between the TemplateGallery.CreateTemplate() and Template.Submit() calls. Otherwise, you will find that the template has an additional custom property definition named NewCustomPropertyDefinitionX (where X is an incremental number). This is a known bug with the PAPI.

In this example, we added a simple text custom property definition. For the more complex selection custom property definition, the technique is very similar. The code snippet below shows how to create a custom property definition named MenuLocation with four allowed values: Left, Right, Top, and Bottom. It specifies Top as the default value.

```
CustomPropertyDefinition customPropSelection;
customPropSelection = new Template.CreateCustomPropertyDefinition();
customPropSelection.Name = "MenuLocation";
customPropSelection.AddAllowedValue("Left");
customPropSelection.AddAllowedValue("Right");
customPropSelection.AddAllowedValue("Top");
customPropSelection.AddAllowedValue("Bottom");
customPropSelection.DefaultValue = "Top";
```

# Creating Placeholder Definitions

Placeholder definitions can be created programmatically too, thanks to the PAPI's Template.CreatePlaceholderDefinition() method. You can add any placeholder definition to a template. Before you do so, it is a good idea to import its namespace, which is why we imported Microsoft.ContentManagement.Publishing.Extensions.Placeholders earlier.