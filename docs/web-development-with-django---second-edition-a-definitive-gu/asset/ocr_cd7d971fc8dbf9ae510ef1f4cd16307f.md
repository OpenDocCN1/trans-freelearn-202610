# Sessions and Authentication

> **Note**
> 
> We only need to copy the templates that are dependencies for the views and should avoid copying the `base.html` or `base_site.html` files.

This gives a promising result at first, but as they stand, the admin templates do not meet our precise needs, as we can see from the login page (*Figure 9.2*):

![](bbox=[0.241, 0.234, 0.758, 0.543])

Figure 9.2 – A first attempt at a user login screen

As these authentication pages inherit from the Admin app’s `admin/base_site.html` template, they follow the style of the Admin app. We would prefer for these pages to follow the style of the `bookr` project that we have developed. We can do this by following these three steps on each Django template that we have copied from the Admin app to our project:

+   1. The first change that needs to be made is to replace the `{% extends "admin/base_site.html" %}` tag with `{% extends "base.html" %}`.
+   
+   2. Given that `template/base.html` only contains the title, brand, and content block definitions, we should examine the additional block substitutions from our templates in the `bookr` folder. We are not using the content from the `userlinks` and `breadcrumbs` blocks in our app’s template layouts, so these blocks can be removed entirely.
+   
+      Some of these blocks, such as `content_title` and `reset_link`, contain HTML content that is relevant to our application, so they should be retained.