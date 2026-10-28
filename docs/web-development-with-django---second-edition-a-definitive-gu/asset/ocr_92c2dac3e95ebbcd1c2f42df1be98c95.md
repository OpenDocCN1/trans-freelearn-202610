The value of admin:index and site_url is provided by the AdminSite class automatically, based on the settings we define.

Using the value for admin:index and site_url, we will create our Login Again hyperlink, which, when clicked, will take the user back to the login form, and the Go to Home Page link, which will take the user back to the home page of the website. The file should look like this now: https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter10/Exercise10.03/templates/admin/logout.html.

+   3. Now, with the custom template defined, the next step is to make use of the custom template on our custom admin site. To do this, let’s open the admin.py file under the bookr_admin directory and add the following field as the final value in the BookrAdmin class:

```
logout_template = 'admin/logout.html'
```

Save the file. It should look like this: https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter10/Exercise10.03/bookr_admin/admin.py.

+   4. Once all the preceding steps are complete, let’s start our development server by running the following command:

```
python manage.py runserver localhost:8000
```

Then, we navigate to http://localhost:8000/admin.

Once you are there, try to log in and then click Logout. Once you are logged out, you will see the following page rendered:

![](bbox=[0.27, 0.575, 0.73, 0.775])

Figure 10.6 – The logout view rendered to users after clicking the Logout button

With this, we have successfully overridden our first template. Similarly, we can also override other templates inside Django’s admin panel, such as the templates for the index view and the login form.