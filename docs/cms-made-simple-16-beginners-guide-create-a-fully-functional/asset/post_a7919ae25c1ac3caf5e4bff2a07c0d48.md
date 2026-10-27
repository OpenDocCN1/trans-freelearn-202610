
# Design and Layout

There are more qualifiers that can be used to build logical conditions in Smarty. Some of them are listed in the following table:

| Qualifier | Meaning | Example |
|---|---|---|
| `eq` | equals | `$page_alias eq "contact-us"` |
| `neq` | not equal to | `$page_alias neq "contact-us"` |
| `gt` | greater than | `$friendly_position gt 2` |
| `lt` | less than | `$friendly_position lt 3` |
| `gte` | greater than or equal | `$friendly_position gte $count` |
| `lte` | less than or equal | `$friendly_position lte 5` |
| `not` | negation | `not $lang`<br><br>(Negation can be used to check if a variable contains any value or if it is empty. If no value is given, then the result of the condition is true, or else the result of the condition is false.) |
| `mod` | modulo | `$count mod 2`<br><br>The modulo operation finds the remainder of division of one number by another. You can use the above example to figure out if the number of pages in the first hierarchy level is even or odd. |

The IF structure is a useful tool for handling slight changes in one template depending on the page name or the position in the hierarchy. In the preceding example, you saw that you can use every variable from the template to build a logical condition.

## Creating navigation template with Smarty loop

You can also change the HTML markup of the navigation. In this chapter, the great principle of a "template in template" giving you full control of your design is explained. Before you can learn this principle, you have to understand some Smarty basics.

When we added the top navigation to the website, we used a standard template for the navigation. It displays the navigation as an unordered HTML list. Imagine that you need a kind of footer navigation where all the links from the top navigation are shown. You do not need an unordered HTML list in this case. You just would like to show all links in one line separated by a pipe (|) shown as follows:

![](img/b21e3dc5c94405bc7b3f102758ea770c_109_0.png)

This means that you need a completely different HTML markup for this kind of navigation. The great advantage of CMS Made Simple is the ability to display a template in template. While you can use the main template to define the whole layout for the page, the HTML markup of the navigation is saved in its own template. This navigation template is just a piece of the HTML code that is added to the main template at the place where the tag `{menu}` is placed.

The menu template contains some HTML markup for the navigation. Let us see how you can create a fully new template for the menu and add your individual HTML markup.

### Time for action – creating a menu template

We need a simple footer navigation where all pages of the website are listed, separated by a pipe (|) sign, as shown earlier.

- 1. In the admin console of the page, click on **Layout | Menu Manager**.
- 2. Click on **Add Template**.
- 3. Fill in the fields for **New Template Name** and **Template Content**, as shown in the following screenshot :

![](img/b21e3dc5c94405bc7b3f102758ea770c_110_0.png)

- 4. Click on **Submit** to save the new menu template.
- 5. Open the **Business World** template for editing (**Layout | Templates**).
- 6. Add footer navigation with the `{menu}` tag, and provide the name of the menu template, as shown in the following code snippet:

```
<!DOCTYPE html>
<html>
    <head>
        <title>{title} - {sitename}</title>
        {stylesheet}
        {metadata}
        <meta name="description" content="" />
    </head>
    <body>
```

```
<div id="container">
<div id="header">
businessWorld
</div>
<div id="top-navi">
{menu number_of_levels="1" template="minimal_menu.tpl"}
</div>
<div id="content">
<h1>{title}</h1>
{content}
{if $page_alias neq "contact-us"}
<p>Last modified by {last_modified_by format="fullname"}</p>
{/if}
</div>
<div id="sidebar">
{menu start_level="2" template="minimal_menu.tpl"}
</div>
<div id="footer">
<p>{menu template="footer_menu"}</p>
2009 businessWorld
</div>
</div>
</body>
</html>
```

7. Click on Apply and see the footer navigation on your website. It should look as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_111_0.png)

## What just happened?

You have just created a new menu template. In the menu template, you have used some Smarty code that needs to be explained. The most important part in the code is the Smarty variable $nodelist. It is not an ordinary Smarty variable like the ones we have already learned. This variable is an array. It means that more than one value is saved in it. This variable contains all the information, such as menu texts or URLs of all active pages of our website. With the built-in Smarty function foreach, we can run through all the values of this variable and display the same piece of HTML code for each page. Let us see exactly how a loop works.

foreach goes through every single page saved in the variable {$nodelist} and prints the HTML code placed between the foreach tags as many times as there are pages found in the variable. For each run, the values for the single page are saved in the variable $node. We name and create this variable with parameter item=node in the foreach tag. The loop starts its run with the first menu item—the page Our Company. At this point, the menu text and URL are saved in the variable $node. We can reach the required information with {$node->menutext} and {$node->url} and use the tags in the HTML code. With the next (second) run, the next page is saved in the variable $node. So beginning with the second run, another menu text and URL is pasted into the same HTML code. The foreach loop runs through every page and stops automatically when no more pages are found.

Footer navigation displays all pages independent of their position in the hierarchy. You can limit the footer navigation only to the first level as well. Do it the same way you did it with the top navigation. Use parameter number_of_levels in the tag {menu} shown as follows:

```
{menu number_of_levels="1" template="footer_menu"}
```

foreach loops are a powerful way to construct any form of navigation you can imagine. This way, you can use images instead of text in your navigation.

One more example, how a graphical navigation can be created, is as shown:

```
{foreach from=$nodelist item=node}
<a href="{$node->url}">
    <img src="uploads/design/{$node->alias}.jpg"
         alt="{$node->menutext}" border="0" />
</a>
{/foreach}
```

To understand what HTML is produced by the preceding Smarty code, in your mind, replace the bold Smarty tags with the actual values of a page, for example, Contact Us:

```
<a href="contact-us">
    <img src="uploads/design/contact-us.jpg" alt="Contact Us" />
</a>
```

As the previous code is placed in the `foreach` loop, the markup will be repeated for every page. You can see that for the graphical example to work, you will need to create a JPG file for every page. The name of the files has to correspond to the page alias. In our example, for the top navigation, you will need five images:

- - `our-company.jpg`
- - `products.jpg`
- - `client-center.jpg`
- - `contact-us.jpg`
- - `sitemap.jpg`

Create the images (buttons) in the graphics editing program of your choice and upload them to the `uploads/design` directory on your web server. Create a new menu template with the name `graphical_menu`, and add the Smarty `foreach` code into it, as shown earlier. Then replace the top navigation in the main template (**Layout | Templates**) with the following line:

```
```smarty
{menu number_of_levels="1" template="graphical_menu"}
```
```

Now, the navigation is made of graphics instead of text.

There is even more information about each page available in menu template which is given in the following table:

| Variable | Explanation |
|---|---|
| `$node->pagetitle` | Title of the page |
| `$node->depth` | Number of levels in the page hierarchy |
| `$node->prevdepth` | Number of levels of the previous page in the page hierarchy |
| `$node->haschildren` | Prints 1 if the page has child pages |
| `$node->target` | The target of the page can be set by the editor from the drop-down menu. In your template, you use it in the following way, as shown in the following example:<br><br>`<a href="{$node->url}" target="{$node->target}">` |
| `$node->alias` | Prints the alias of the page |
| `$node->current` | Prints 1 if the page is currently displayed |
| `$node->type` | The type of the page. Possible values are:<br><br>`sectionheader` (Section header)<br>`separator` (Separator)<br>`content` (Content)<br>`pagelink` (Internal page link)<br>`link` (External link) |
