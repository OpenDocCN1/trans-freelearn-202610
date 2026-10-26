```
```csharp
placeholderDef.Description = "A placeholder created from the PAPI";
placeholderDef.AllowHyperlinks = true;
placeholderDef.AllowLineBreaks = true;
placeholderDef.AllowAttachments = true;
placeholderDef.AllowImages = true;
placeholderDef.MustUseResourceGallery = false;
placeholderDef.UseGeneratedIcon = false;
placeholderDef.Formatting =
    HtmlPlaceholderDefinition.SourceFormatting.FullFormatting;

// commit the changes
cmsContext.CommitAll();

- - - code continues - - -
```
```

The Create Template dialog is now complete. Save and build the solution. Let's test it to see if it works:

+   1. In CMS Explorer, click on the Templates button. Open the MyTemplateGallery template gallery created earlier.
+   2. Select New | New Template.
+   3. In the Create Template dialog, enter the following values:

| Property | Value |
|---|---|
| Name | MyTemplate |
| Description | This template was created using the PAPI |
| SourceFile | /tropicalgreen/plantcatalog/plant.aspx |

+   4. Click Create Template and close the dialog.
+   5. Refresh the CMS Explorer screen and verify that MyTemplate has been created in MyTemplateGallery.

# Creating Connected Templates

Connected templates are created in the same way as templates, with the exception that we now call the `TemplateGallery.CreateConnectedTemplate()` method.

Let's try it out:

+   1. Add a new web form to the project with the name `CreateConnectedTemplate.aspx`.
+   2. Drag and drop `Styles.css` onto the web form.
+   3. Toggle to HTML view and enter the following code (including the text markers):

```
```html
<table>
<tr>
  <td colspan="2">
    <h1>Create Connected Template</h1>
    <h2>
      Original Template:
      (Add literal for displaying the path of the original template here)
    <h2>
  </td>
</tr>
<tr>
  <td>Name:</td>
  <td>(Add the text box for the Name here)</td>
```
```