
To use the plugins, you have to separate HTML, CSS, and JavaScript and put them at the appropriate place in CMS Made Simple. HTML markup is saved in templates, whereas CSS is saved in stylesheets and then assigned to the template. JavaScript can be placed globally (**Site Admin | Global Metadata**), so that it is available on the whole page. Alternatively, it can be placed in the page template. In this case, it will be available in all pages using this template. If you need JavaScript only in one page, then you can add it to the **Page Specific Metadata** field in the **Options** tab.

Let's see how you can integrate a jQuery drop-down menu on your website. Examples and instructions for this effect can be found at `http://www.dynamicdrive.com/style/csslibrary/item/jquery_multi_level_css_menu_horizontal_blue/`.

## Time for action – integrating jQuery in navigation

The instruction on the link above explains how you can use this jQuery plugin in the pure HTML website. Let's see how you can integrate the plugin in CMS Made Simple.

- 1. Save the files `jquerycssmenu.css` and `jquerycssmenu.js` from the address given above to your local disk.
- 2. Upload the file `jquerycssmenu.js` into the folder `uploads` on your web hosting.
- 3. In the admin console **Add a Stylesheet (Layout | Stylesheets)**, call it **JQueryCSSMenu**, and copy the whole content from the file `jquerycssmenu.css` into the **Content** field.
- 4. Click on **Submit**.
- 5. In the list of stylesheets, click the blue CSS button beside the new created stylesheet.
- 6. Select your main template from the drop-down field, and click on **Attach to this Template**.
- 7. Open the main template of your website, and add the following code snippet in the head section:

```
{literal}
<!--[if lte IE 7]>
<style type="text/css">
html .jquerycssmenu{height: 1%; /*Holly Hack for IE7 and below*/
</style>
<![endif]-->
{/literal}
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.2.6/jquery.min.js"></script>
<script type="text/javascript" src="uploads/jquerycssmenu.js"></script>
```

- 8. Create or replace the top navigation in the main template with:

```
<div id="myjquerymenu" class="jquerycssmenu">
{menu template="minimal_menu.tpl"}
<br style="clear: left" />
</div>
```

- 9. Click on Apply, and see the drop-down menu of your website.

## What just happened?

This was a very basic example of how to integrate jQuery into your website. First, you have to figure out what JavaScript files are needed for the plugin and upload them to your web hosting. Then create a new stylesheet, copy all styles into it, and attach the new stylesheet to the template. If images are used in the stylesheet, then upload them to the web hosting as well and adjust the image path in the stylesheet.

Then open your template and add JavaScript to the head section of the template. Any scripts should be enclosed in the Smarty tags {literal}{/literal}. This tag allows a block of data to be taken literally. This is typically used around JavaScript or stylesheet blocks where curly braces would interfere with the curly braces of Smarty tags. Anything within {literal}{/literal} tags is not interpreted, but displayed as it is.

In the last step, you add the required CSS classes and IDs to the HTML markup of the template. This method works for any jQuery plugin you would like to integrate. Another example for integrating jQuery into the Gallery module was explained in Chapter 7, Using Third-party Modules.

## Pop quiz – advanced use of CMS Made Simple

- 1. If you change the language of the admin console, it would have an impact on:
  - a. All user accounts in CMS Made Simple.
  - b. On the language displayed to the visitors of the website.
  - c. Only to this user account.
  - d. Nothing. You have to choose the language once during installation and cannot be changed later.

- 2. If you would like to participate in making translations for CMS Made Simple, you have to:
  - a. Join the official developer team of CMS Made Simple.
  - b. Join the translation project of your language in Forge.
  - c. Translate on your local installation and upload translations to CMS Made Simple forum.
  - d. Pay the community to get needed translations.

- 3. Meta tags are important for search engines. What meta tags are used by the major search engines like Google, Yahoo and Bing?
  - a. `descr<meta name="iption" content="" />`
  - b. `<meta name="refresh" content="" />`
  - c. `<meta name="robots" content="" />`
  - d. `<meta name="author" content="" />`

- 4. Assume you use the tag `{title}` in the tag `<title>` in your template. What will be affected if the title of the page is changed?
  - a. Alias of the page
  - b. Menu text of the page
  - c. Heading of the page
  - d. The name in the title bar of the browser

- 5. How can you prevent different URLs of the same page being treated as duplicate content?
  - a. Consequently use domain with or without www.
  - b. Avoid publishing print version of your websites.
  - c. Use canonical tag for canonical URLs.
  - d. Use `<meta name="robots" content="noindex" />` in all your pages.

- 6. Why should you use search engine friendly addresses?
  - a. The RAM memory of your computer gets overloaded by opening more than seven pages without friendly URLs (they are too long).
  - b. The URL is optimized for search engines.
  - c. You should not use them at all because you have to create for every search engine a special one and so create duplicated content.
  - d. The page is loaded much quicker

- 7. User-defined tags are written in:
  - a. Pure HTML
  - b. Pure HTML and CSS
  - c. JavaScript
  - d. PHP

- 8. Some jQuery plugins cannot be integrated in CMS Made Simple:
  - a. true
  - b. false

## Have a go hero – make search engine optimization

Make search engine optimizations for your website before you publish it. It is better to start with a clear and optimized website from the very beginning. Double-check page titles, menu texts and words represented as internal links in the content of the page. Control if one page can be reached with different URLs and use a canonical tag to help the search engines to understand which URL is the main one. Create an XML sitemap for search engines and monitor the statistics of your visitors to figure out what pages are the most popular, thus figuring out what contents of the page can be extended.

Consider that your website is made primarily for visitors and not for search engines. Search engines are just carriers that bring visitors to your website. And whether the visitor likes your website or not lies in your responsibility.

## Summary

In this chapter, we learned advanced subjects that are not necessarily a part of every website.

Specifically, we covered the following topics:

- **Localization and translation:** You need this section if the primary language of the website is not in English or if you are going to create a multilingual website.
- **Additional content and controls for editors:** This section helps to understand what you can do if you have a sophisticated layout or would give website 'editors' more control.
- **Search engine optimization:** You learned how you can optimize the website for search engines.
- **User-defined tags and Event Manager:** If you have some basic PHP knowledge, you can even implement your own PHP scripts into CMS Made Simple and still stay upgradable.
- **Implementing jQuery effects:** You learned how you can implement simple jQuery functionality into the top navigation of the website.

Now that we've learned how to realize these sophisticated features, read the next chapter to see what tasks you have as a website administrator during the lifetime of the website.

## Administration and Troubleshooting

Once the website you have created is ready and running, you have to take a closer look at the tasks you have as a webmaster during the lifetime of the website. Depending on the nature of the website, you must regularly make backups if you or your customer continuously add and edit the content or change the design. It is also recommended to watch the development of CMS Made Simple and make updates to keep the website secure and be able to use the newest features.

This chapter helps you administer your CMS Made Simple website once it is running. In this chapter, we will see how to:

- Take a backup of your website
- Move it to another web host
- Upgrade your installation or modules
- Optimize the performance
- Secure and protect your website in the most efficient way
- Get help from the community

You are not supposed to read the whole chapter. You can start reading from any section, as they do not depend on each other. *Upgrading CMS Made Simple* is the part that you will probably need when a new release of CMS Made Simple is published.

I strongly recommend that you read the *Troubleshooting* section and devote 10 minutes of your time to the question *How can I get a quick answer in the free support forum?* There are some useful tips on how to get the best answer in the shortest period of time.
