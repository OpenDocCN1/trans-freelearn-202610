Once the hierarchy is created, go through your already created products and assign them to the proper hierarchy item. The hierarchy can now be displayed in the sidebar navigation on the page. Open the main page template (Layout | Templates) and find the section with sidebar navigation. Add the Smarty tag for the product hierarchy shown as follows:

```
{Products action="hierarchy"}
```

# Customizing product templates

The display of the product hierarchy template is very technical. Let's customize all the templates for the module. There are three of them:

+   - Summary Templates
+   - Detail Templates
+   - Hierarchy Report Templates

Let's start with the Hierarchy Report Templates. This template defines how the hierarchy in the sidebar is displayed. In the admin area of the Product Manager, click on the Hierarchy Report Templates tab and find the list of existing templates for the hierarchy. The template Sample is the one that is used by default. You can customize this template or create your own by clicking on the New Template link. I choose the second way. It is generally advisable not to change sample templates, but create your own and set them as default. This way you can delete anything from the custom template and use the Sample template for reference if you need parts removed from the custom template. For the template name, I chose My Shop. However, you can use any name you wish.

In the Template Text field, the sample template code is already suggested. Leave this code as it is and submit the new template. Now you see two templates in the list. Make the template My Shop the default one by clicking on the red cross in the Default column. Let's see what we have in the template and what we actually need. Open the new template for editing:

| Smarty variable | Description |
|---|---|
| {$hierarchy_item.name} | The name of the hierarchy item. |
| {$hierarchy_item.id} | The ID of the hierarchy item. |
| {$upurl} | The URL to the parent hierarchy item. Only applicable if there are more than one hierarchy levels. |
| {$mod} | The object containing all the information about the module Products. In the template the object is used to get translations:
{$mod->Lang('parent')} returns the translation for the key parent from the translation file.
You can replace this variable with your custom text if your website is monolingual and the language of the website will be never changed. |