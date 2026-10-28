
## Exercise 9.01 – repurposing the Admin app login template

We started this chapter without a login page for our project. By adding the URL patterns for authentication and copying the templates from the Admin app to our project, we can implement the functionality of a login page. But this login page is not satisfactory as it is directly copied from the Admin app and is disconnected from the Bookr design. In this exercise, we will follow the steps needed to repurpose the Admin app’s login template for our project. The new login template will need to inherit its style and format directly from the bookr project’s templates/base.html:

1. Create a directory inside your project for templates/registration.

The Admin login template is located in the Django source directory at the django/contrib/admin/templates/admin/login.html path. It begins with an extends tag, a load tag, importing the i18n and static modules, and a series of block extensions that override the blocks defined in the parent template, django/contrib/admin/templates/admin/base.html. A truncated snippet of the login.html file is shown in the following code block:

```
{% extends "admin/base_site.html" %}
{% load i18n static %}

{% block extrastyle %}{{ block.super }}...
{% endblock %}

{% block bodyclass %}{{ block.super }} login{% endblock %}
{% block usertools %}{% endblock %}
{% block nav-global %}{% endblock %}
{% block content_title %}{% endblock %}
{% block breadcrumbs %}{% endblock %}
```

2. Copy this Admin login template, django/contrib/admin/templates/admin/login.html, into templates/registration and begin editing the file using PyCharm.

3. As the login template you are editing is located at templates/registration/login.html and extends the base template (templates/base.html), replace the argument of the extends tag at the top of templates/registration/login.html with the following:

```
{% extends "base.html" %}
```

4. We don’t need most of the contents of this file. Just retain the content block, which contains the login form. The remainder of the template will consist of loading the i18n and static tag libraries:

```
{% load i18n static %}

{% block content %}
...
{% endblock %}
```

5. Now, you must replace the paths and reverse URL patterns in templates/registration/login.html with ones that are appropriate to your project. As you don’t have an app_path variable defined in your template, it needs to be replaced with the reverse URL for the login, 'accounts:login'. So, consider the following line:

```
<form action="{{ app_path }}" method="post" id="login-form">
```

This line changes as follows:

```
<form action="{% url 'accounts:login' %}" method="post"
id="login-form">
```

No 'admin_password_reset' has been defined in your project paths, so it will be replaced with 'accounts:password_reset'.

Consider the following line:

```
{% url 'admin_password_reset' as password_reset_url %}
```

This changes to the following line:

```
{% url 'accounts:password_reset' as password_reset_url %}
```

Your login template will look as follows:

`templates/registration/login.html`

```
{% extends "base.html" %}
{% load i18n static %}

{% block content %}
{% if form.errors and not form.non_field_errors %}
<p class="errornote">
{% if form.errors.items|length == 1 %}{% translate "Please correct the error below." %}{% else %}{% translate "Please correct the errors below." %}{% endif %}
</p>
{% endif %}
```

You can find the complete code for this file at https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter09/Exercise9.01/bookr.

6. As mentioned previously, to use the standard Django authentication views, we must add URL mappings to them. Open the urls.py file in the bookr project directory, ensure that auth is imported from django.contrib, and add the three paths to the URL patterns:

```
from django.contrib import admin, auth
...
urlpatterns = [
    ...
    path("accounts/",
        include(("django.contrib.auth.urls", "auth"),
        namespace="accounts")),
    path("accounts/password_reset/done/",
        auth.views.PasswordResetDoneView.as_view(),
        name="password_reset_done"),
    path("accounts/reset/done/",
        auth.views.PasswordResetCompleteView.as_view(),
        name="password_reset_complete"),
    ...
]
```

7. Now, when you visit the login link at http://127.0.0.1:8000/accounts/login/, you will see this page:

![](img/4840a43efd335abd10b93708225692d6_462_0.png)

Figure 9.3 – The Bookr login page

By completing this exercise, you have created the template required for non-admin authentication in your project.

> **Note**
>
> Before you proceed, you'll need to make sure the rest of the templates in the registration directory follow the bookr project's style; that is, they do not inherit from the Admin app's admin/base_site.html template. You've already seen this done with password_change_done.html and the login.html templates. Go ahead and apply what you've learned in this exercise (and the section before it) to the rest of the files in the registration directory. Alternatively, you may download the modified files from this book's GitHub repository: https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter09/Activity9.01/bookr.

Now that we have modified our project to enable authentication, we will learn about password and session storage in Django.

## Password storage in Django

Django does not store passwords in plain text form in the database. Instead, passwords are digested with a hashing algorithm, such as PBKDF2/SHA256, BCrypt/SHA256, or Argon2. As hashing algorithms are a one-way transformation, this prevents a user's password from being decrypted from the hash stored in the database. This often comes as a surprise to users who expect a system administrator to retrieve their forgotten password, but it is best practice in security design. So, if we query the database for the password, we will see something like this:

```
sqlite> select password from auth_user;
pbkdf2_sha256$320000$auohDtokS8a6LAoaG79Tv1$Mxzssu6gfd5uZvI1ADFI4faAjP0g3puP2JxxFdWZI0=
pbkdf2_sha256$320000$SqoVF8nhgo4zgw4juRUNtAK5TATQEqXtbRAfc4Ccuuwr9nRHVuShZo5W6N90ygpvVbA=
pbkdf2_sha256$320000$Ss3eprgniu7e4q0ux9Xj5nh$/SAFKgXuYIWSm08PWEwBUsMfmPCYd60rAaz0JZnLKGs=
```

Figure 9.4 – Password hashes in a Django SQLite3 database

The components of this string are `<algorithm>$<iterations>$<salt>$<hash>`. As several hashing algorithms have been compromised over time and we sometimes need to work with mandated security requirements, Django is flexible enough to accommodate new algorithms and can maintain data encrypted in multiple algorithms.

## The profile page and the request.user object

When a login is successful, the login view redirects to /accounts/profile. However, this path is not included in the existing auth URLs, nor does the authentication app provide a template for it. To avoid a Page not found error, a view and an appropriate URL pattern are required.

Each Django request has a `request.user` object. If the request is made by an unauthenticated user, `request.user` will be an AnonymousUser object. If the request is made by an authenticated user, then `request.user` will be a User object. This makes it easy to retrieve personalized user information in a Django view and render it in a template.

In the next exercise, we will add a profile page to our bookr project.

## Exercise 9.02 – adding a profile page

In this exercise, we will add a profile page to our project. To do so, we need to include the path to it in our URL patterns and also include it in our views and templates. The profile page will simply display the following attributes from the `request.user` object:

- `username`
- `first_name` and `last_name`
- `date_joined`
- `email`
- `last_login`

Perform the following steps to complete this exercise:

1. Add `bookr/views.py` to the project. It needs a trivial profile function to define our view:

```
from django.shortcuts import render

def profile(request):
    return render(request, 'profile.html')
```

2. In the templates folder of your main bookr project, create a new file called `profile.html`. In this template, the attributes of the `request.user` object can easily be referenced by using a notation such as `{{ request.user.username }}`:

```
{% extends "base.html" %}

{% block title %}Bookr{% endblock %}

{% block content %}
<h2>Profile</h2>
<div>
    <p>
        Username: {{ request.user.username }} <br>
        Name: {{ request.user.first_name }} {{
```
