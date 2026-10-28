3. Once this view has been created, we need to map this view to a URL endpoint that can then be visited in a browser or a test client. To do this, open the urls.py file under the bookr_test directory and add the highlighted code to it:

```
from django.urls import path
from . import views

urlpatterns = [
    path('test/greet_user', views.greeting_view_user,
        name='greeting_view_user')
]
```

In the preceding code snippet, we mapped greeting_view to the 'test/greet_user' endpoint for the application by setting the path in the urlpatterns list:

```
path('test/greet_user', views.greeting_view_user,
    name='greeting_view_user')
```

If you have followed the previous exercises, this URL should already be set up for detection in the project and no further steps are required to configure the URL mapping.

4. Once the view has been set up, the next thing we need to do is validate whether it works correctly. To do this, run the following command:

```
python manage.py runserver localhost:8080
```

Then, visit http://localhost:8080/test/greet_user in your web browser.

If you are not logged in already, by visiting the preceding URL, you will be redirected to the login page for the project.

5. Now, write the test cases for greeting_view_user, which check whether, on visiting the /test/greet_user endpoint, you get a successful result or not. To implement this test case, open the tests.py file under the bookr_test directory and add the following code to it:

```
from django.contrib.auth.models import User

class TestLoggedInGreetingView(TestCase):
    """Test the greeting view for the authenticated users."""
    def setUp(self):
        test_user =
            User.objects.create_user(
                username='testuser',
                password='test@#628password')
        test_user.save()
        self.client = Client()

    def test_user_greeting_not_authenticated(self):
```