If your website structure looks like the preceding screenshot, then you will get the output shown in the second column by using the tag from the first column.

| Smarty tag | Output |
|---|---|
| {menu start_element="3"} | • Client Center
  ○ FAQ
  ○ Newsletter
  ○ Service Desk |
| {menu start_element="1.1" show_root_siblings="1"} | • Announcements
  ○ All announcements
  • History
  • Team
  • Photo gallery |
| {menu start_element="1.1" show_root_siblings="1" number_of_levels="1"} | • Announcements
  • History
  • Team
  • Photo gallery |

You can see how flexible the navigation layout can be on CMS Made Simple. On every page, you can precisely define your navigation structure and even create a kind of articles' catalog by adding article category first and then assigning articles to the categories and displaying them with Menu Manager on the website in any page or in the template.

# Printing pages

With CMS Made Simple, every page and news article can be automatically prepared for print preview with the built-in Printing module.

# Time for action – adding a print link

Let's see how you can add a link to the print version of every page or news article.

+   1. In the admin console, open the template Business World for edit (Layout | Templates).
+   2. Add the Smarty plugin {print} above the {content} tag in the template, shown as follows:
+   3. Click Apply, and see the changes in your website.

```
...
<div id="content">
<h1><span>{$friendly_position}</span> {title}</h1>
{print}
{content}
...
```