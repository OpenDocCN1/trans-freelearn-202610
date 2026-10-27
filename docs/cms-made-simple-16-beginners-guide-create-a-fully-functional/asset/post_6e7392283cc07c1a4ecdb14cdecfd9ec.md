
## Design and Layout

> The full name of the user is shown if it is given in the user's account. In the admin console, open **Users & Groups | Users** and click on the username that you would like to modify. Enter the **First Name** and the **Last Name** of the user.

### Using plugins in content

In the last section, you added Smarty plugins to the template. However, it is also possible to add them directly into the content of the page. You use the tags in the content in the same way that you used them in the template. Write the name of the plugin enclosed in curly brackets in the field **Content** of a specific page. Try it out now. Open any page for editing (**Content | Pages**) and add one or more tags in the field **Content**. You can mix tags and static text in the same way you did in the template.

A small example for the start page of your website can be:

```
Welcome! Today is {current_date}.
```

The preceding code will automatically display the current date on the start page of your website.

> You should add plugins that need to be displayed on every page of your website to the template. If you need a special plugin on only one page (for example, the start page), then you should add the plugin into the field **Content** of this page.

### Adding stylesheets to the template

Nowadays, designing websites requires not only pure HTML, but also CSS. The most professional websites use HTML to structure the website and CSS to style them. CSS gives you full control over your layout. With CSS you can easily change the look and feel of your website without changing any HTML. You can see some good examples of CSS layouts based on the same HTML at `http://www.csszengarden.com`.

CMS Made Simple takes pure CSS code for your page layout and saves it in the database. In this section, you will learn how you can create a new stylesheet, add your styles to it, and attach it to the template.

### Creating a new stylesheet

To create a new stylesheet, you have to accomplish three simple steps:

- 1. Create and save the new stylesheet containing your CSS code.
- 2. Attach the stylesheet to the template.
- 3. Add a special tag in the template that will be replaced by an HTML tag `<link>` pointing to the stylesheet.

#### Time for action — creating the stylesheet

Normally, in static HTML pages, you create a CSS file and attach it to your template using an HTML `<link>` tag. In CMS Made Simple, stylesheets are saved in the database, and therefore cannot be linked directly. Let us see how stylesheets can be created and attached to the templates.

- 1. In the admin console, click on **Layout | Stylesheets**.
- 2. Click the **Add a Stylesheet** link at the bottom of the list.
- 3. Type in the name of the new stylesheet and enter the CSS code in the **Content** field, as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_88_0.png)

- 4. Click on **Submit**. The new stylesheet is added to the list of stylesheets.
- 5. To attach the new stylesheet to your template, in the admin console, click on **Layout | Templates**.
- 10. Click on **Apply** and then click on the magnifying glass icon in the top-right corner of the admin console to see the result of your template changes. It should look as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_90_0.png)

#### What just happened?

You have created a new stylesheet that contains pure CSS code. Then, you have attached this stylesheet to your template. By attaching the stylesheet, you have stated which stylesheet is relevant for your template. The `{stylesheet}` tag that you added to your template is substituted by the HTML `<link>` tag when the page is displayed in the browser.

Open the page source in the browser to see what is set at the place where the `{stylesheet}` tag has been added to the template. In my example, it looks like:

```
<link rel="stylesheet" type="text/css" href="http://www.yourdomain.com/stylesheet.php?cssid=51" />
```

In your special installation, the name of the domain will be different, and the last number in the URL of the stylesheet is also likely to be different. The tag is formed automatically, and you do not need to care how it is made technically.

You can define and attach more than one stylesheet to the template. For example, you can define a stylesheet with general styles and then add another one containing print styles. We will create and add more stylesheets in the course of the book so that you can see how it works later on. For each stylesheet, a separate HTML link tag is added at the place where the `{stylesheet}` tag is added. There is no need to add the tag twice or more if you have attached more stylesheets, as this plugin will create as many links as there are stylesheets attached to the template.

### Media types for stylesheets

While creating a stylesheet, you have probably seen the area **Media Type** with some checkboxes at the bottom of the page where stylesheet code is added. You can see the area if you open your stylesheet for editing in the admin console (**Layout | Stylesheets**) and scroll down. It appears as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_91_0.png)

The media type corresponds to the HTML attribute `media` and is automatically added to the HTML `<link>` tag (if you select any type here):

```
<link rel="stylesheet" type="text/css" media="screen" href="http://www.yourdomain.com/stylesheet.php?cssid=51" />
<link rel="stylesheet" type="text/css" media="print" href="http://www.yourdomain.com/stylesheet.php?cssid=52" />
```

The styles from the first stylesheet are used to display your website in the browser. However, the browser will use the print stylesheet if your page is printed:

> If you use any CSS framework with more than one CSS file, then you can copy the content of each CSS file into a separate stylesheet in CMS Made Simple and then attach them all to your templates. You can also change the order of the stylesheets while assigning them to the template.

### Creating navigation for the website

The heart of each website is surely the navigation. The navigation consists of links to the pages and is often divided into main navigation and sidebar navigation. In CMS Made Simple, the navigation is automatically built from the list of active pages. You can create navigation of any complexity grade in CMS Made Simple due to the very powerful concept explained in the following sections.

#### Time for action – adding navigation to the template

Let us add navigation links to the website.

- 1. In the admin console, click on **Layout | Templates**, and select **My first template** for edit.
- 2. Add a special tag for navigation before the `{content}` tag, as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_92_0.png)

- 3. Click on **Apply** and then click on the magnifying glass icon in the top-right corner of the admin console to see the result of your template changes. It should look as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_92_1.png)

#### What just happened?

With the custom CMS tag `{menu}`, a complete structure of the website is displayed at the place where it is added in the template. Each active page is displayed according to its position in the hierarchy and order in the list of pages.

The `{menu}` tag is very powerful. You can use numerous parameters that help you to create navigation of any complexity grade. For example, you can limit the navigation to only one level, so that only pages from the first hierarchy level are shown:

```
{menu number_of_levels="1"}
```

Try the preceding code to create some navigation limited to the main pages. This is a common way to create the main navigation for the website; normally placed at the top of each page. The subpages are not shown with this parameter but we can display them in the sidebar navigation later on.

We will learn more parameters for the `{menu}` tag and see more examples on how it can be customized later on. In this chapter, our main subject is design and layout. That is why we will now learn to customize the look and feel of top navigation.

### Designing navigation — the pure CSS way

With the simple plugin `{menu}`, the navigation of the website is displayed on every page. CMS Made Simple creates a simple unordered HTML list for the navigation as you can see in the source code of the page. Let us see how you can easily style a top navigation bar with pure CSS.

#### Time for action – design navigation with pure CSS

We are now going to style the top navigation so that it looks like buttons displayed beside each other instead of below each other.

- 1. In the admin console, click on **Layout | Templates**, and open **My first template** for edit.
- 2. Add the HTML `<div>` tag around the `{menu number_of_levels="1"}` tag, as shown in the following screenshot:
