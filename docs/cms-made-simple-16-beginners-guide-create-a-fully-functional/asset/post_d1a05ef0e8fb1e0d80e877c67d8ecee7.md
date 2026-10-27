
```
{* print out attributes *}
{if isset($entry->attributes)}
  {foreach from=$entry->attributes key='name' item='attribset'}
    <p><strong>{$name}:</strong>
      {foreach from=$attribset key='label' item='adjustment'}
        {$label} ({$currency_symbol}{$adjustment}),
      {/foreach}
    </p>
  {/foreach}
{/if}
  {* include the cart *}
  {if isset($cart_module_tag)}
  {eval var=$cart_module_tag}
  {/if}
</div>
```

You are now ready to add cart functionality to your solution.

## Module Cart

This module will add simple cart functionality to the **Products** module. Once installed, the **Cart** module will be automatically displayed in the detailed view of the product. The visitor of the website will then be able to put one or more products in the basket along with the desired quantity. There are other modules that you can use instead of the **Cart** module, like **GiftBaskets**. For our example **Shop**, it is enough to use the simple functionality of the **Cart** module.

Install the module **Cart** from the list of available modules in the **Module Manager**. The admin area of this module will be added to **Extensions | Cart** in the admin console of CMS Made Simple.

## Time for action – connecting products and cart

If you now look at your **Shop** page and open a detailed view of any product, you will discover that there is still no cart functionality in it. The reason for it is that the **Products** module does not know that the **Cart** module has been installed and is ready for use. Let's connect two modules.

- 1. Open the admin area of the **Products** module (**Content | Product Manager**).
- 2. Click on the **Preferences** tab.
- 3. Select the **Cart** module from the field **Cart Module**.
- 4. Click on Submit and open detailed view of any product. You should see the button Add to My Cart in the detailed view of the product. If the product has some attributes additionally to the quantity field, then the attributes are displayed in a drop-down field besides it.

![](img/b21e3dc5c94405bc7b3f102758ea770c_259_0.png)

## What just happened?

You have told the Products module what module has to be used for cart functionality. Test it and add some products to the cart right now. The products will be added, but there is no possibility to display the content of the cart for the visitor yet.

Create a new content page called Cart and add the following Smarty tag as the content of the page:

```
{Cart action="viewcart"}
```

Save the page, then open the admin area for the Cart module (Extensions | Cart) and switch to the Preferences tab. Select the newly created page in the list of pages, so that the visitor is automatically redirected to the page after he has added the product to it.

There are four templates that you can customize for the module:

| Template | Description |
|---|---|
| AddToCart | This template controls how the quantity field, the button Add To My Cart (shown in the previous screenshot) and optionally product attributes with price adjustment will look. |
| MyCart | This template is used to display the cart status. It is displayed at the place where the Smarty tag {Cart action="mycart"} is placed. It shows the number of items currently saved in the cart and a link to the cart page. |
| Viewcart | This template is used for extended display of all products saved in the cart. It shows all products with their quantities and prices and the total order price. Additionally it offers the possibility to start the checkout order process if the module Orders is installed. |
| Product Summary | This template is used to format the output used for each product summary in the Viewcart template. It allows you to customize the product label based on the attributes, product name, and price. |

From this list of dependencies, you can learn that it is based on the functionality of the module **FrontEndUsers**. This means that your website's customers must register before they can start the payment process on your online store. There is no possibility to turn off this registration process and it is required for the checkout step.

Install all modules listed in the preceding dependencies list, then install the **Orders** module.

## Time for action – adding the checkout step

To enable the checkout process provided by the **Orders** module, follow these steps:

- 1. Create a new content page (**Content | Pages | Add New Content**) and name it **Checkout**.
- 2. Add the following Smarty tag to the **Content** field of the **Checkout** page:

  ```
  {Orders}
  ```

- 3. Save and view the page in the browser. This page will display either the billing form if the user is logged in or the warning message if the user is not logged in or does not belong to a special user group.
- 4. To connect the **Cart** module with the new **Checkout** page, in the admin console click on **Extensions | Cart**.
- 5. Select the tab **Viewcart Form Templates** and click on the **Sample** template (or your own template, if you have created one).
- 6. At the very end of the template in the **Template Text** field, add the following line of code:

  ```
  {cms_selflink page="checkout"}
  ```

- 7. Open your shop and add one or more products to the cart, find a link to the page **Checkout** at the very bottom of the screen.

```
{Orders}
```

```
{cms_selflink page="checkout"}
```

> The parameter `page` should contain the page alias of the page where the Smarty tag `{Orders}` has been added to the content.

## What just happened?

You have connected the module **Cart** with the module **Orders** by placing a link from the cart view to the checkout process. To test the link, add some products to the cart and click the **Checkout** link at the bottom of the cart view. If you are not logged in, you will see the following message:

## The user is not logged in (or not a member of the required group)

This message can be found in the template of the **Orders** module. But first, let's see an overview of the templates delivered with this module. In the admin console, click on **Content | Order Manager** and find the **Templates** link above the list of orders:

| Template | Description |
|---|---|
| **Billing Form** | This template controls the first step in the order processing. It displays the form where the customer can enter billing address and one or more shipping addresses. |
| **Payment Form** | This template controls the second step in the order processing and is used to display the payment form for manual e-mail-based checkout process. You will need this template if you do not use any payment services such as PayPal, EMoney, eWay, or Authorize.Net. |
| **Confirm Order** | This template controls the third step in the order processing and is used to display any information the customer entered in the first and optionally second step, so that the customer can confirm the shipping and invoice addresses or go back and correct them. |
| **Invoice** | This template is used in the admin area of the **Orders** module to print out the invoice or to send it via e-mail to the customer. |
| **Gateway Complete** | This template is used to display the status message after the payment has been made with any third-party service such as PayPal, EMoney, EWay or Authorize.net. |
| **User Email** | This template is used to customize the e-mail with order confirmation sent to the customer when the new order has been submitted. |
| **Admin Email** | This template controls the layout and content of the e-mail that is sent to the authorized administrator of the store when a new order is submitted. |
| **Message** | There is no sample template for the messages. Here you can define your custom e-mail notifications that can be manually sent to the customers from the admin area of the module. An example of such message can be an e-mail notification that is sent when the order is shipped. |

Now, we know where to start and how to proceed with customizing of the templates according to our requirements and payment configurations. Let's give the customer the possibility to register with or login to the website to be able to complete the checkout process.
