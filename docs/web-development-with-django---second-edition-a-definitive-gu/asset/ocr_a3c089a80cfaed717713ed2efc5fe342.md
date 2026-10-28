```
```html
request.user.last_name }}<br>
Date Joined: {{ request.user.date_joined }} <br>
Email: {{ request.user.email }}<br>
Last Login: {{ request.user.last_login }}<br>

</p>
</div>
{% endblock %}
```
```

We also added a block containing the profile details of the user. More importantly, we made sure that `profile.html` extends `base.html`.

+   3. Finally, this path needs to be added to the top of the `urlpatterns` list in `bookr/urls.py`. First, import the new views and then add a path linking the `accounts/profile/` URL to `bookr.views.profile`:

```
```python
from bookr.views import profile

urlpatterns = [
    path('accounts/profile/', profile,
         name='profile'),
    path("", reviews.views.index),
    path("book-search/",
         reviews.views.book_search,
         name="book_search"),
    path('', include('reviews.urls'))]
```
```

This is a good start on a user profile page. When Alice logs in and visits `http://localhost:8000/accounts/profile/`, it is rendered as shown in Figure 9.5. Remember, if the server needs to be started, use the `python manage.py runserver` command: