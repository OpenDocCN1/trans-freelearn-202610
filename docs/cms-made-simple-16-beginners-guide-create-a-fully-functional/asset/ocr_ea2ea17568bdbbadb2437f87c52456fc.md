To test the template, open a page that you would like to see within the new template for editing (**Content | Pages**). Switch to the **Options** tab, and select the newly created template in the field **Template**. The screen is automatically redirected to the **Main** tab. Click on **Apply** and then on the icon with the magnifying glass icon beside the button **Apply** to see the changes on the page. Compare this page to other pages that use different templates.

This wonderful principle of CMS Made Simple allows you to give each page its own look and feel. You can use different templates to separate some parts of your website. For example, you could use one template for the company website and a completely different template for the shop if you like. Designers often use different templates to enable one, two, or three columns layouts on different pages.

To see which page uses what template, open the list of the current pages in the admin console (**Content | Pages**) and see the column **Template**.

If you would like to use one template on all pages of your website, you can assign it to all pages at once. This function is especially useful if you already have a great number of pages. In the admin console, open the list of existing templates (**Layout | Templates**) and click the **Set All Pages** link beside the name of the template. This way, the template is assigned to all pages of your website with just one click.

In the list of templates, you can also see that one template is set as default. When you create a new page, CMS Made Simple will automatically take the default template for the new page. Click on the icon with the red cross in the column **Default** beside the template name that you would like to use as the standard template.

# Creating a new template

Surely, it is easier to use ready-made templates for the website. However, you often need your own style for the website. In this section, we will learn how to create a new template from scratch.

Templates in CMS Made Simple contain pure HTML. For dynamic parts of the layout, such as navigation or content, special placeholders are used. These placeholders are realized with the **Smarty** web template system. You use simple **Smarty tags** in your template to mark the places where dynamic parts in the pages are required. Smarty tags are placeholders that are substituted with simple text or even other HTML templates when the website is displayed in the browser. In contrast to HTML tags, Smarty tags are always enclosed in curly brackets.