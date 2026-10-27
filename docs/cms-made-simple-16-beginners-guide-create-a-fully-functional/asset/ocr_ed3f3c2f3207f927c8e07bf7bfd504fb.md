Let's create the attribute for the color of the long t-shirt. Assume that we have two colors available: white and grey. The color does not change the price of the product. In the admin area of the **Products** module, click on the **Products** tab. In the list of products, at the end of each line there is an icon for **Edit Attribute Sets**. Click on this icon in line with the product to which you would like to add attributes.

On the next screen, click on **Add Attribute**. Enter the name of the attribute. This name contains the generic term like Color. In the **Label** field enter the name of the attribute in the same way as it should appear on the website. As the color of the t-shirt does not change the price of the product, enter **0** in the field **Price Adjustment** and click on **Update**. Add another label for the color **Grey**. It should look as shown in the following screenshot:

![](bbox=[0.179, 0.355, 0.824, 0.606])

Click on **Submit** once you have entered all possible values for this attribute. See the detailed view of the product with defined attributes. All possible values are listed in the template. When you install the **Cart** module later on, you will not only be able to display the attributes, but add them to the cart as well. The price will be automatically adjusted in the cart view.

You should now be able to customize the detail template yourself.

Follow these steps:

+   1. Create a new template and do not overwrite the sample one. The **Sample template** can be used for future reference if you delete any part from your custom template.
+   2. Analyze the template. Try to figure out its major parts and usage of other modules in it.
+   3. Write down all the Smarty variables used in the templates in a table as shown previously for the **Hierarchy Report Templates**.