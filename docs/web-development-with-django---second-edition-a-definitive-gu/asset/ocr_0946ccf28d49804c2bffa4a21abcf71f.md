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

![](bbox=[0.135, 0.4, 0.86, 0.66])

Figure 10.7 – The profile page view on the Bookr Administration site

Note

The view created so far will render just fine, irrespective of whether a user is logged into the admin application.