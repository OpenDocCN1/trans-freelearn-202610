You can submit your sitemap to major search engines such as Google, Yahoo!, Bing, or Ask. For minor search engines, you can extend the robots.txt file placed in the root directory of your website. Add the following line at the very bottom of the file:

```
Sitemap: http://www.yourdomain.com/sitemap.xml
```

Adjust your domain name in the example.

# Visitor statistics

Now, when the website is published, you would probably like to know how many visitors you get per day, week, and month. In this way, you can control your efforts in search engine optimization and gain additional information about what the visitors of your website look for or which content is the most interesting to them. The module provides information on how the visitors find your website and where they come from. You can also see what browser and screen resolution the visitor used.

There are a lot of web analysis tools that you can use on the website. There is even a special third-party module that you can install to see the statistic of your website directly in the admin console. The module is called Statistics. This module not only makes the visitor statistics available in your admin console, but also shows a visible counter on the website.

You can also use external website analysis tools such as Google Analytics or even both (the module Statistics and Google Analytics) if you want to compare the results. Google Analytics provides you with special JavaScript code that you must add to the main template of your website (Layout | Templates). It is important to know that only pages that include this code are tracked in the statistics. By placing it in the main template, the code is automatically called with every page, and thus the entire website is covered.

Always include JavaScript code in the Smarty tag {literal} to avoid the code being interpreted as Smarty code. For example:

```
{literal}
    alien JS code
{/literal}
```

# User-defined tags

User-defined tags are a simple way to insert some PHP code into your site. With the default installation of CMS Made Simple there are two example tags already created, which are as follows:

+   - {custom_copyright}
+   - {user_agent}