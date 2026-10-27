+   4. In the next step, open the `config.php` file for editing, and find the section *URL Settings*.
+   5. Change the line for rewriting URLs, as shown:
+      `$config['url_rewriting'] = 'mod_rewrite';`
+   6. Open your website, and see how the links in the navigation look. They should be search engine friendly now.

# What just happened?

To enable search engine friendly URLs on your website, you had to complete two steps. In the first step, you moved the file `htaccess.txt` into the root directory of your website. You then renamed the moved file to `.htaccess` (period at the beginning belongs to the filename). `.htaccess` is the name of a configuration file that allows you to control the web server. The file is placed in the root directory and overrides some of the default settings of your server.

The default `htaccess.txt` is suitable for most servers. However, sometimes you may encounter difficulties. For example, not all web hosting providers accept the following setting:

```
Options +FollowSymLinks
```

In this case, you will get an error 500 (internal server error) after turning on search engine friendly URLs. To solve the issue, comment out the following option as shown:

```
# Options +FollowSymLinks
```

If your website is placed in a subfolder, like `http://www.yourdomain.com/folder`, then you have to adjust the `.htaccess` file. Find the line `RewriteBase /`, and change it to the name of the folder where your website is placed. In our example, it would be:

```
RewriteBase /folder
```

In the second step, you modified the file `config.php`. With these changes, you have told CMS Made Simple that `.htaccess` is placed in the root directory, and you are going to use these new settings to rewrite URLs.

With CMS Made Simple, you can even simulate a static address like `http://www.yourdomain.com/products.html`. In this case, the address of the pages will look like static HTML files. Open `config.php`, and find the line `$config['page_extension'] = ''`; . If you change it to `$config['page_extension'] = 'html';`, then all URLs of your website will look like static HTML pages.