# Design and Layout

> The full name of the user is shown if it is given in the user's account. In the admin console, open **Users & Groups | Users** and click on the username that you would like to modify. Enter the **First Name** and the **Last Name** of the user.

# Using plugins in content

In the last section, you added Smarty plugins to the template. However, it is also possible to add them directly into the content of the page. You use the tags in the content in the same way that you used them in the template. Write the name of the plugin enclosed in curly brackets in the field **Content** of a specific page. Try it out now. Open any page for editing (**Content | Pages**) and add one or more tags in the field **Content**. You can mix tags and static text in the same way you did in the template.

A small example for the start page of your website can be:

```
Welcome! Today is {current_date}.
```

The preceding code will automatically display the current date on the start page of your website.

> You should add plugins that need to be displayed on every page of your website to the template. If you need a special plugin on only one page (for example, the start page), then you should add the plugin into the field **Content** of this page.

# Adding stylesheets to the template

Nowadays, designing websites requires not only pure HTML, but also CSS. The most professional websites use HTML to structure the website and CSS to style them. CSS gives you full control over your layout. With CSS you can easily change the look and feel of your website without changing any HTML. You can see some good examples of CSS layouts based on the same HTML at `http://www.csszengarden.com`.

CMS Made Simple takes pure CSS code for your page layout and saves it in the database. In this section, you will learn how you can create a new stylesheet, add your styles to it, and attach it to the template.