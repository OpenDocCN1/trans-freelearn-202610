From this list of dependencies, you can learn that it is based on the functionality of the module **FrontEndUsers**. This means that your website's customers must register before they can start the payment process on your online store. There is no possibility to turn off this registration process and it is required for the checkout step.

Install all modules listed in the preceding dependencies list, then install the **Orders** module.

# Time for action – adding the checkout step

To enable the checkout process provided by the **Orders** module follow these steps:

+   1. Create a new content page (**Content | Pages | Add New Content**) and name it **Checkout**.
+   2. Add the following Smarty tag to the **Content** field of the **Checkout** page:
+      `{Orders}`
+   3. Save and view the page in the browser. This page will display either the billing form if the user is logged in or the warning message if the user is not logged in or does not belong to special user group.
+   4. To connect the **Cart** module with the new **Checkout** page, in the admin console click on **Extensions | Cart**.
+   5. Select the tab **Viewcart Form Templates** and click on the **Sample** template (or your own template, if you have created one).
+   6. At the very end of the template in the **Template Text** field, add the following line of code:
+      `{cms_selflink page="checkout"}`
+   7. Open your shop and add one or more products to the cart, find a link to the page **Checkout** at the very bottom of the screen.

```
{Orders}
```

```
{cms_selflink page="checkout"}
```

> The parameter page should contain the page alias of the page where the Smarty tag {Orders} has been added to the content.

# What just happened?

You have connected the module **Cart** with the module **Orders** by placing a link from the cart view to the checkout process. To test the link, add some products to the cart and click the **Checkout** link at the bottom of the cart view. If you are not logged in, you will see the following message: