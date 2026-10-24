
5. Click on the Install Drupal in English link to proceed with the installation.

6. Drupal will now validate that all requirements for Drupal have been met. If any errors are displayed, they will need to be corrected before proceeding. On a default installation, you will need to create a settings.php file based on the default.settings.php file, which can be modified during the install process, as shown in the following screenshot:

## Requirements problem

The following error must be resolved before you can continue the installation process:

> The Drupal installer requires that you create a settings file as part of the installation process.
> 
> 1. Copy the ./sites/default/default.settings.php file to ./sites/default/settings.php.
> 
> 2. Change file permissions so that it is writable by the web server. If you are unsure how to grant file permissions, please consult the on-line handbook.
> 
> More details about installing Drupal are available in INSTALL.txt.

Please check the error messages and try again.

After you fix any errors that are reported, you can click try again, and repeat this step until no errors are reported.

7. You will now need to create a database for your site. The exact method of creating a MySQL database will depend on the host that you have chosen for your site. If you are unsure of how to create a database on your host, contact your provider for additional help.

8. After your database has been created, you will need to enter the name of the database, as well as the username and password for the database.

## Database configuration

Basic options

To set up your Drupal database, enter the following information.

Database type: *
○ mysqli
○ pgsql
The type of database your Drupal data will be stored in.

Database name: *
yoursite_db
The name of the database your Drupal data will be stored in. It must exist on your server before Drupal can be installed.

Database username: *
youruser

Database password:
••••••••••••

These will either be provided for you when your database was created by your hosting company, or you would have specified them yourself if you built the database from scratch.

## Administrator account

The administrator account has complete access to the site; it will automatically be granted all permissions and can perform any administrative activity. This will be the only account that can perform certain activities, so keep its credentials safe.

Username: *

YourSiteAdmin

Spaces are allowed; punctuation is not allowed except for periods, hyphens, and underscores.

E-mail address: *

admin@yoursite.com

All e-mails from the system will be sent to this address. The e-mail address is not made public and will only be used if you wish to receive a new password or wish to receive certain news or notifications by e-mail.

Password: *

********  Password strength: Low

Confirm password: *

********  Passwords match: Yes

The password does not include enough variation to be secure. Try:

+   - Adding both upper and lowercase letters.
+   - Adding numbers.
+   - Adding punctuation.

If you enter a password that is not very difficult to guess, Drupal will warn you and provide suggestions for a more robust password.

+   11. Finally, you will need to define some basic server configuration options. In most cases, Drupal sets these items correctly for you, but you may need to review the time zone setting to ensure that this is correct.

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_35_0.png)

+   12. When you are satisfied with all of the information, click **Save** to complete the installation process.

+   13. Once the installation is complete, you can return to the main page, where you can login and begin the additional configuration of your web site. We will start building the Good Eatin' site from this stage, starting from Chapter 2.

If you want a more thorough introduction to Drupal, with explanations of some of the more theoretical aspects of Drupal, check out *Building Powerful and Robust Websites with Drupal 6* by David Mercer, also published by Packt Publishing.

## 2

## Creating Good Eatin's Online Presence

In this chapter, we will set up the fundamental parts of the Good Eatin' web site using the Drupal Content Management System. We will start by building a few pages, including the About page and the Home page. We will also discuss how to delete and edit pages. After we get comfortable managing content, we will add your pages to the navigation system so that customers can easily find all of the information that they need. After the navigation has been completed, we will make your site look more visually appealing by adding images and slideshows. Finally, we will add a theme to your site, based on one of the many themes available on the Drupal web site.

At each step in this chapter, we will point out ways to ensure that your site is secure so that hackers and other malicious visitors can't interfere with your business.

Before we start making any changes, let's have a look at the default site provided by Drupal.

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_38_0.png)

At the end of this chapter, you will understand the techniques used by the vast majority of web sites on the Internet, and your site will look as shown in the following screenshot:

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_39_0.png)

## About Good Eatin': Adding static pages

This section will describe how to set up an informational page that has static text. For our client, this will cover adding the About Good Eatin' page. We will also create a basic menu page which will be changed later.

## Adding a page

At the core of every site are pages that don't change or rarely change. Drupal makes it extremely easy to add these types of pages to a web site without having any programming knowledge. We will demonstrate these techniques on the Good Eatin' site by creating a simple home page and an about page.

To add a new static page, select the Create Content link from the menu on the left most side of the site. You will now be presented with a list of content types that Drupal knows how to create. By default, Drupal knows about the Page type and the Story type. Most static pages, including our About Good Eatin' page, will use the Page type. However, if you want visitors to be able to comment on your pages, or if you are creating a blog-style site, the Story type may be a better choice, because comments are turned on by default for this type. We will discuss comments in detail in the next chapter.

To create our About Good Eatin' page, click on the Page link. Drupal will now present you with a form on which you can enter information about the Page. You can enter a title that is displayed in the title of the web browser and above the content when it is displayed on the site. We used **About Good Eatin'** as the title of our page.

A typical about page will contain the name of the company, the address of the company, and a brief history of what the company does. We will use this as a guideline when we create our About Good Eatin' page.

You can enter information exactly as you do in the word processor of your choice, using the enter key to separate paragraphs. Drupal will automatically convert the text you enter into correct HTML for display in a web browser. You don't need to worry about learning HTML. However, if you do know HTML, you can use tags to format your pages. Refer to the HTML primer sidebar for information on the basic tags that Drupal allows the pages to include.

The text we used for the About Good Eatin page is shown below:

```
```html
<em>Good Eatin' Bistro</em> is a full service restaurant serving
breakfast, lunch, and dinner daily. Good Eatin' Bistro was founded in
2008 by <cite>Mark Noble</cite> to serve as an example website for the
<strong>Drupal 6 Site Builder Solutions</strong>.

<dl>
<dt>Address:</dt>
<dd>1500 Main Street<br>
Anytown, CO<br>
United States of America</dd>

<dt>Phone:</dt>
<dd>303-555-5555</dd>
</dl>
Hours of operation
<ul>
<li>M-F: 8AM - 10PM</li>
<li>Sat: 7AM - 12PM</li>
<li>Sun: 8AM - 8PM</li>
</ul>
```
```

## Spicin up your site with simple HTML

Drupal allows you to use several simple HTML tags to improve the formatting of a page. Although HTML can be intimidating at first, it is really quite easy to learn and use.
