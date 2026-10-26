# Essential How-Tos, Tips, and Tricks

+   - Register the method that cleans up links to deleted resources on every template file with the `WebAuthorContext.SavePostingEvent()` event. However, doing so would mean repeating code on all template files in the solution.
+   - Add the logic required to clean up the dead links within the `CmsPosting_PlaceholderPropertyChanging()` event handler in the `global.asax` file.

While both options are workable, let's choose to place the clean-up code in the `CmsPosting_PlaceholderPropertyChanging()` event handler in the `global.asax` file. This will ensure that any time placeholder content is changed, the clean-up processes are triggered. You are guaranteed that there will be no links to deleted resources for pages that are saved after the resource has been removed.

To begin, let's register the `CmsPosting_PlaceholderPropertyChanging()` event handler.

+   1. Open the code-behind file of the `global.asax` file (`global.asax.cs`).
+   2. Add the following namespaces:
+   
+   ```csharp
+   using Microsoft.ContentManagement.Publishing;
+   using Microsoft.ContentManagement.Publishing.Events;
+   using Microsoft.ContentManagement.Publishing.Extensions.Placeholders;
+   ```
+   
+   3. Below the `Global()` constructor, add the `CmsPosting_PlaceholderPropertyChanging()` event handler:
+   
+   ```csharp
+   protected void CmsPosting_PlaceholderPropertyChanging( Object sender,
+       PropertyChangingEventArgs e )
+   {
+   }
+   ```

Resources are typically linked to in three possible locations. They could be used within:

+   - An `ImagePlaceholder` as an image. Links to resources are stored in the `Src` property value.
+   - An `AttachmentPlaceholder` as an attachment. Links to resources are stored in the `Url` property value.
+   - An `HtmlPlaceholder` as either images or attachments. They are mixed within the content retrieved from the `Html` property value.

> Links to resources can also be found within XmlPlaceholders. Based on your specific XML schema, do consider them in your solution as well.

The following code handles each of these cases in turn. Add the code within the `CmsPosting_PlaceholderPropertyChanging()` event handler in the `global.asax.cs` file.

```
```csharp
public void CmsPosting_PlaceholderPropertyChanging( Object sender,
    PropertyChangingEventArgs e )
{
    string propName = e.PropertyName;
    string propValue = e.PropertyValue as string;
    switch (propName)
    {
        case "Src": // handle ImagePlaceholder
        case "Url": // handle AttachmentPlaceholder
        {
```
```