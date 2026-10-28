In this exercise, we used authentication decorators to secure views in a Django app. The authentication decorators that were applied provided a simple mechanism to restrict views from users lacking necessary permissions, non-staff users, and unauthenticated users. Django’s authentication decorators provide a robust mechanism that follows Django’s role and permission framework, while the user_passes_test decorator provides an option to develop custom authentication.

Now that we have used decorators to control the authentication flow of the application, we can customize templates with user authentication data.

# Enhancing templates with authentication data

In Exercise 9.02 – adding a profile page, we saw that we can pass the request.user object to the template to render the current user’s attributes in the HTML. We can also take the approach of giving different template renderings according to the user type or permissions held by a user. Consider that we want to add an edit link that only appears to staff users. We might apply an if condition to achieve this:

```
{% if user.is_staff %}
<p><a href="{% url 'review:edit' %}">Edit this Review</a>
</p>
{% endif %}
```

If we didn’t take the time to conditionally render links based on permissions, users would have a frustrating experience navigating the application as many of the links that they click on would lead to 403 Forbidden pages. The following exercise will show how we can use templates and authentication to present contextually appropriate links in our project.

# Exercise 9.04 – toggling login and logout links in the base template

In the bookr project’s base template, located in templates/base.html, we have a placeholder logout link in the header. It is coded in HTML as follows:

```
<li class="nav-item">
<a class="nav-link" href="#">Logout</a>
</li>
```

We don’t want the logout link to appear after a user has logged out. So, this exercise aims to apply conditional logic to the template so that the Login and Logout links are toggled, depending on whether the user is authenticated:

+   1. Edit the templates/base.html file. Copy the structure of the Logout list element and create a Login list element. Then, replace the placeholder links with the correct URLs for the Logout and Login pages – /accounts/logout and /accounts/login, respectively – as follows: