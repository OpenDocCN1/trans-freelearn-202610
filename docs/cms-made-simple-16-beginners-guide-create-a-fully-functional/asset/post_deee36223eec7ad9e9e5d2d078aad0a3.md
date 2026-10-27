
For more help, you may take a look at the FAQ. The link to the FAQ can be found in the module help above the Copyright and License section.

## Creating the product list template

For modules created with CTLModuleMaker, some sample templates are created automatically. However, for each level, you can create an unlimited number of templates by yourself.

### Time for action – creating a new list template

Let's customize the appearance of the product list and add products' images as thumbnails to this list.

1. In the admin console, click on Content | Products Catalogue, and choose the Templates tab.
2. Below the section Default Templates, you will find a list of sample templates. Click on the Add template link.
3. For the product list with thumbnails, enter the following code snippet in the Template field:

   ```
   {if $itemcount > 0}
       <ul>
           {foreach from=$itemlist item="item"}
               <li style="list-style-type:none; float:left; margin: 20px;"><a href="{$item->detailurl}">
               <img src="uploads/{$item->image->thumbnail}" alt="{$item->name}" /></a>
               <br/>
               {$item->detaillink}<br/>{$item->price} USD</li>
           {/foreach}
       </ul>
   {/if}
   ```

4. Click on Submit.
5. In the **Default templates** section, choose the template you just created to be the **Default list template for level "product"**. The list of products should look as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_223_0.png)

### What just happened?

You have created a new template for the list of the products. This template shows not only the products' name but also the small version of the products' image. The small version of the image is represented as Smarty variable `{$item->image->thumbnail}` in the template. All Smarty variables that you can use in the template can be found on the tab **Template variables (Content | Products Catalogue | Templates)** when the template is open for edit. Consider that the fields containing images have even more variables. They are explained in the FAQ for the module in the section titled *How can I retrieve images in the display templates?*

> The inline CSS added to the template is not the proper way to implement styles. It has been made to simplify the example. You should create a new CSS class and store the style definitions in the main stylesheet.

In the same way, you can modify or create a new template for the **Default detail template** that is used to display the detailed view of one product.

With the **CTLModuleMaker** module, you can create any kind of catalog such as products, services, real estate advertisements, courses, and more. Everything that you can categorize can be put in a custom catalog-like module. Remember that you create an ordinary module that can be removed from the list of modules, exported as XML, and imported into another installation.

## Implementing service desk functionality

There are over 300 third-party modules in CMS Made Simple, and this number grows weekly. Some modules do not work as expected, as there is no quality assurance for the third-party modules. However, sometimes you do not need to use a dedicated module to solve your requirements. With some creativity you can divert modules from its intended use to create your own functionality.

There is no sophisticated module for the service desk in CMS Made Simple. However, if you need it to handle support tickets, then you can use the module **CGFeedback**; which was originally created to handle comments for the pages or news articles. Although this module has not been developed to handle support tickets, it can be used to solve the required features with some advanced customization.

## Managing visitors' logins

Before you set up the service desk feature on your website, you have to install the module **FrontEndUsers**. The **FrontEndUsers** module is a kind of membership management facility for your website. In our case, members are the customers who can authorize with their access data on the website and the protected content or communicate with you through their personal member area.

Generally, **FrontEndUsers** can be integrated with many other modules such as:

- **News** (Registered visitors can add news to the website)
- **DownloadManager** (Registered visitors can download protected files from the website)
- **Forum** (Registered users can communicate on the boards)
- **NMS** (Mass mail for registered users)

> Website members are not the same as the admin users of your website, which were described in Chapter 6, *Users and Permissions*. They have nothing to do with your admin console (hopefully!).

The module **FrontEndUsers** has a dependency on the module **CMSMailer**. It means that you have to configure e-mail settings, as described in Chapter 2, *Getting Started*, before installing member management on the website. Otherwise, features like sending out forgotten passwords will not work.

To install the module in the admin console, click on **Extensions | Module Manager**, and choose the letter **F** from the **Available modules** tab above the alphabetical module list. Find the **FrontEndUsers** module, and click the **Download & Install** link. The module will be installed and can now be configured.

Open the page **Service Desk (Content | Pages)**, and add the login form into the **Content** field of the page, shown as follows:

```
{cms_module module="FrontendUsers"}
```

Now, you can see the login form on the page. Let's create a first customer account and log in to the page so that you can test the functionality of the module.

### Time for action – creating the first user account

We are going to add the first customer account to the website.

1. In the admin console, click on **Users & Groups | Frontend User Management**.
2. Click on the **User Properties** tab.
3. Click the **Add Property** link and fill in the form, as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_225_0.png)

4. Click on **Submit**.
5. Open the **Groups** tab, and click on the **Add Group** link.
6. Fill in the fields, and assign the e-mail field created above to the group, as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_226_0.png)

7. Click on **Submit**.
8. Click on the **Users** tab and then on the **Add User** link.
9. Fill in the fields, as shown in the following screenshot (provide a valid e-mail address!), and assign the user to the **Customers** group:

![](img/b21e3dc5c94405bc7b3f102758ea770c_226_1.png)

10. Click on **Next**, and enter the e-mail address of the customer again.
11. Click on **Submit**.
12. Log in with the customer data (**Username = Email Address** of the customer) on the page **Service Desk** to see how it works. If you are logged in, the page should look as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_227_0.png)

### What just happened?

You have created the first customer account. You have started by adding the user property (a field that contains a customer's e-mail address). This field will be used for sending e-mails such as if the customer forgets his password. By default, this field is also used as the username (you can change it later on). The **Type** of the field must be set to **Email Address**, so that anything entered in this field must be a valid e-mail. You do not need to define user properties such as **username** or **password**. These two properties are already provided for you and cannot be changed or deleted.

> Never use any special characters like dashes in the field **Name** while adding user properties!

You have already created a new user group (**Customers** in our example). The e-mail field has been assigned to the group **Customers** as a required field (**Field Status**), which means that each customer must have a valid e-mail address.

At this point, you have configured the module so that the first customer could be added. All customer accounts have their expiration date, so they are disabled automatically if the date is in the past. The new user has been assigned to the **Customer** group and is now shown in the list of users on the **Users** tab (**Users & Groups | Frontend User Management**).

### Useful settings for module FrontEndUsers

As the administrator of the website, you can adjust the settings of the **FrontEndUsers** module. In the admin console of your website, click on **Users & Groups | Frontend User Management**, and select the **Preferences** tab. The following settings should be made:
