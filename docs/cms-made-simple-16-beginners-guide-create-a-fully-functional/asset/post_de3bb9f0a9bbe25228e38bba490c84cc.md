
Once the hierarchy is created, go through your already created products and assign them to the proper hierarchy item. The hierarchy can now be displayed in the sidebar navigation on the page. Open the main page template (Layout | Templates) and find the section with sidebar navigation. Add the Smarty tag for the product hierarchy shown as follows:

```
{Products action="hierarchy"}
```

## Customizing product templates

The display of the product hierarchy template is very technical. Let's customize all the templates for the module. There are three of them:

- Summary Templates
- Detail Templates
- Hierarchy Report Templates

Let's start with the Hierarchy Report Templates. This template defines how the hierarchy in the sidebar is displayed. In the admin area of the Product Manager, click on the Hierarchy Report Templates tab and find the list of existing templates for the hierarchy. The template Sample is the one that is used by default. You can customize this template or create your own by clicking on the New Template link. I choose the second way. It is generally advisable not to change sample templates, but create your own and set them as default. This way you can delete anything from the custom template and use the Sample template for reference if you need parts removed from the custom template. For the template name, I chose My Shop. However, you can use any name you wish.

In the Template Text field, the sample template code is already suggested. Leave this code as it is and submit the new template. Now you see two templates in the list. Make the template My Shop the default one by clicking on the red cross in the Default column. Let's see what we have in the template and what we actually need. Open the new template for editing:

| Smarty variable | Description |
|---|---|
| {$hierarchy_item.name} | The name of the hierarchy item. |
| {$hierarchy_item.id} | The ID of the hierarchy item. |
| {$upurl} | The URL to the parent hierarchy item. Only applicable if there are more than one hierarchy levels. |
| {$mod} | The object containing all the information about the module Products. In the template the object is used to get translations: {$mod->Lang('parent')} returns the translation for the key parent from the translation file. You can replace this variable with your custom text if your website is monolingual and the language of the website will be never changed. |

```
    {$node.name}
    {/if}
    </h3>
    {if !empty($node.description)}
    <div>{$node.description}
    </div>
    {/if}
    {/foreach}
{/if}

<!-- This tag will be automatically added to the page content.
    It uses the default summary template for products. -->
{Products hierarchyid=$hierarchy_item.id}
```

This template goes through each hierarchy and displays its name as a HTML heading <h3>. It will also show a description field of the hierarchy item below the heading. The last Smarty tag in the template shows the summary list of the products limited to the hierarchy item selected in the sidebar. The template of this summary list is the default summary template. The products summary list will automatically replace the page content.

Before you customize the summary template, open it (Content | Product Manager, tab Summary Templates, template Sample) and find out what the major parts of this template are. It consists of:

- Drop-down list of categories
- Page navigation
- List of products with the add-to-cart functionality for each product

At present, you can see only the list of products, no categories, no page navigation, and no cart functionality.

Displaying the drop-down list of the categories does not work in the actual version of the module the way it is added to the sample template:

```
    {if isset($catformstart)}
        {$catformstart}
        {$catdropdown}{$catbutton}
        {$catformend}
    {/if}
```

It is still possible that this functionality will be added in a later release of the module. At present, you can completely delete this part or replace it with the Smarty tag shown as follows:

```
{Products action="categorylist"}
```

```
```css
{
    float: left;
    padding: 15px;
    text-align: center;
}
```
```

This is just an example; you are free to add any styles that will fit into your own design.

The size of the thumbnails created while uploading product images can be changed in the tab **Preferences (Content | Product Manager)**. On this page, scroll down to the section **Image Handling Settings** and adjust the values as you like.

If you have used my templates so far, your summary template and hierarchy should look as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_255_0.png)

The last step will be customization of the detailed view of the product. Create a new template called **My Shop** under the **Detail Templates** tab in the admin area of the **Products** module. Then set the template as default.

In the detail template, you will find a section for the product attributes. We did not discuss it so far. The products can have different attributes, like size or color for the t-shirts. These attributes may or may not have an impact on the price. For each product, the attributes are added separately. It does not make sense to add size or color to the mouse pad, if we have only one model.

Let's create the attribute for the color of the long t-shirt. Assume that we have two colors available: white and grey. The color does not change the price of the product. In the admin area of the **Products** module, click on the **Products** tab. In the list of products, at the end of each line there is an icon for **Edit Attribute Sets**. Click on this icon in line with the product to which you would like to add attributes.

On the next screen, click on **Add Attribute**. Enter the name of the attribute. This name contains the generic term like Color. In the **Label** field enter the name of the attribute in the same way as it should appear on the website. As the color of the t-shirt does not change the price of the product, enter **0** in the field **Price Adjustment** and click on **Update**. Add another label for the color **Grey**. It should look as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_256_0.png)

Click on **Submit** once you have entered all possible values for this attribute. See the detailed view of the product with defined attributes. All possible values are listed in the template. When you install the **Cart** module later on, you will not only be able to display the attributes, but add them to the cart as well. The price will be automatically adjusted in the cart view.

You should now be able to customize the detail template yourself.

Follow these steps:

- 1. Create a new template and do not overwrite the sample one. The **Sample template** can be used for future reference if you delete any part from your custom template.
- 2. Analyze the template. Try to figure out its major parts and usage of other modules in it.
- 3. Write down all the Smarty variables used in the templates in a table as shown previously for the **Hierarchy Report Templates**.
- 4. Figure out, what values are saved in these variables. If the variable contains an array, use the Smarty modifier `print_r` to get all array values from it. For example, `{$entry|print_r}` will output all the values contained in this variable.
- 5. Use HTML and Smarty to markup the template.
- 6. Add stylesheet definitions to change the appearance.

## Creating detail view for product

Follow the list above and create your own template for the detailed view of the product. Delete everything you do not need in the template; for example, the Weight field. Assign custom fields (Product image and Product number) the same way you did in the summary template. Check the values of the variables with the modifier `print_r` if you are not sure what you have to use.

My detail template looks as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_257_0.png)

The template code I have used for the detailed template is shown in the following code snippet:

```
```html
<div class="products-detail">
  <img align="left" src="{$entry->file_location}/{$entry->fields.image->value}" alt="{$entry->product_name}"/>
  <h1>{$entry->product_name}</h1>
  <h2>{$currency_symbol}{$entry->price}</h2>
  <div>{$entry->details}</div>
```
```
