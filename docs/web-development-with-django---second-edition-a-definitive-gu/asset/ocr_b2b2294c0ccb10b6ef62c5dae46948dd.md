2. Using this method is fine and it’s one way to add permissions checking to a view. You can also use a slightly more complicated but more flexible method. Instead of using a permission decorator to enforce permission rights on the publisher_edit method, you can create a test function that requires a staff user and apply this test function to publisher_edit with the user_passes_test decorator. Writing a test function allows more customization on how you validate users’ access rights or permissions. If you made changes to your views.py file in step 1, feel free to comment the decorator out (or delete it) and write the following test function instead:

```
from django.contrib.auth.decorators import
user_passes_test
...

def is_staff_user(user):
    return user.is_staff

@user_passes_test(is_staff_user)
...
```

3. Ensure that login is required for the review_edit and book_media functions by adding the appropriate decorator:

```
...

from django.contrib.auth.decorators import
    (login_required, user_passes_test)
...
@login_required
def review_edit(request, book_pk, review_pk=None):

@login_required
def book_media(request, pk):
...
```

4. In the review_edit method, add logic to the view that requires that the user be either a staff user or the owner of the review. The review_edit view controls the behavior of both review creation and review updates. The constraint that we are developing only applies to the case where an existing review is being updated. So, the place to add code is after a Review object has been successfully retrieved. If the user is not a staff account or the review’s creator doesn’t match the current user, we need to raise a PermissionDenied error: