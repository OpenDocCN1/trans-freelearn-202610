
# Exercise 10.04 – adding custom views to the admin site

In this exercise, you will add a custom view to the admin site, which will render a user profile and will show the user the options to modify their email and add a new profile picture. To build this custom view, follow the steps described:

+   1. Open the admin.py file under the bookr_admin directory and add the following imports. These will be required to build our custom view inside the admin site application:

```
from django.template.response import TemplateResponse
from django.urls import path
```

+   2. Open the admin.py file under the bookr_admin directory and create a new method named profile_view, which takes in a request variable as its parameter, inside the BookrAdmin class:

```
def profile_view(self, request):
```

Next, inside the method, get the name of the current application and set that in the request context. For this, you can use the name property of the class, which is auto-populated by Django. To get this property and set it in your request context, you need to add the following line:

```
request.current_app = self.name
```

Once you have the application name populated to the request context, the next step is to fetch the template variables, which are required to render the contents, such as site_title and site_header, in the admin templates. For this, leverage the each_context() method of the AdminSite class, which provides the dictionary of the admin site template variables from the class:

```
context = self.each_context(request)
```

Once you have the data in place, the last step is to return a TemplateResponse object, which will render the custom profile template when someone visits the URL endpoint mapped to your custom view:

```
return TemplateResponse(request,
    "admin/admin_profile.html", context)
```

+   3. With the view function now created, the next step is to make AdminSite return the URLs mapping the view to a path inside AdminSite. To do this, you need to create a new method called get_urls(), which overrides the AdminSite.get_urls() method and returns the mapping of your new view. This can be done by first creating a new method named get_urls() inside the BookrAdmin class you have created for your custom admin site:

```
def get_urls(self):
```

Inside this method, the first thing you need to do is to get the list of the URLs that are already mapped to the admin endpoint. This is a required step; otherwise, your custom admin site will not be able to load any results associated with the model editing pages, logout page, and so on if this mapping is lost. To get this mapping, call the get_urls() method of the base class from which the BookrAdmin class is derived:

```
urls = super().get_urls()
```

Once the URLs from the base class have been captured, the next step is to create a list of URLs that map our custom view to a URL endpoint in the admin site. To do this, we will create a new list named url_patterns and map our profile_view method to the admin_profile endpoint. This is done by using the path utility function from Django, which allows us to map the view function with a string-based API endpoint path:

```
url_patterns = [
    path("admin_profile", self.profile_view)
]
return url_patterns + urls
```

Save the admin.py file. It should look like this: https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter10/Exercise10.04/bookr_admin/admin.py.

Note that the value returned by get_urls() is a non-modifiable list. In the preceding code snippet, if we try to switch the order between the merging of the url_patterns and urls objects, the method won’t work and the admin_profile URL won’t be created.

4. Now, with the BookrAdmin class configured for the new view, the next step is to create your template for the admin profile page. To do this, create a new file named admin_profile.html under the templates/admin directory of your project root. Inside this file, first, add an extends tag to make sure that you are extending from the default admin template:

```
{% extends "admin/index.html" %}
```

This step ensures that all of your admin template style sheets and HTML are available for use inside your custom view template. For example, without having this extends tag, your custom view will not show any specific content already mapped to the admin site, such as site_header, site_title, or any links to log out or go to another page.

Once the extends tag is added, add a block tag and provide it with the value of content. This makes sure that the code you add between the pair of {% block content %}...{% endblock %} segments overrides whatever value is present in the index.html template that comes prepackaged with the Django admin module:

```
{% block content %}
```

Inside the block tag, add the HTML required to render the profile view that was created in step 2 of this exercise:

```
<p>Welcome to your profile, {{ username }}</p>
<p>You can do the following operations</p>
<ul>
    <li><a href="#">Change E-Mail Address</a></li>
    <li><a href="#">Add Profile Picture</a></li>
</ul>
{% endblock %}
```

The file should now look like this: https://github.com//PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter10/Exercise10.04/templates/admin/admin_profile.html.

5. Now, with the preceding steps complete, reload your application server by running python manage.py runserver localhost:8000 and then visiting http://localhost:8000/admin/admin_profile.

When the page opens, you can expect to see something like the following screenshot:

![](img/4840a43efd335abd10b93708225692d6_512_0.png)

Figure 10.7 – The profile page view on the Bookr Administration site

Note

The view created so far will render just fine, irrespective of whether a user is logged into the admin application.

To make sure that this view is only accessible to logged-in admins, you need to make a small modification inside your `get_urls()` method, which you defined in step 2 of this exercise.

Inside the `get_urls()` method, modify the `url_patterns` list to look something like the one shown here:

```
```python
url_patterns = [
    path("admin_profile",
         self.admin_view(self.profile_view)),
]
```
```

In the preceding code, you wrapped your `profile_view` method inside the `admin_view()` method.

The `AdminSite.admin_view()` method causes the view to be restricted to users who are logged in. If a user who is currently not logged into the admin site tries to visit the URL directly, they will be redirected to the login page, and only in the event of a successful login will they be allowed to see the contents of our custom page.

During this exercise, we leveraged our existing understanding of writing views for Django applications, merging it with the context of the `AdminSite` class to build a custom view for our admin dashboard. With this knowledge, we can now move on and add useful functionalities to our Django admin to supercharge its usefulness.

## Passing additional keys to templates using template variables

Inside the admin site, the variable values passed to the templates are done so by using template variables. These template variables are prepared and returned by the `AdminSite.each_context()` method.

Now, if there is a value that you want to pass to all the templates of your admin site, you can override the `AdminSite.each_context()` method and add the required fields to the request context. Let’s look at an example to see how we can achieve this outcome.

Consider the username field, which we passed to our `admin_profile` template earlier. If we want to pass it to every template inside our custom admin site, we first need to override the `each_context()` method inside our BookrAdmin class, as shown here:

```
```python
def each_context(self, request):
    context = super().each_context(request)
    context['username'] = request.user.username
    return context
```
```

The `each_context()` method takes a single argument (we’re not considering `self` here) of the `HTTPRequest` type, which it uses to evaluate certain other values.
