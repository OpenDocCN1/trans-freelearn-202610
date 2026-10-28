2. To make sure the bookr_test application behaves the same way as any other application in the Django project, add this application to our INSTALLED_APPS section of the Bookr project. To do this, open the settings.py file in your Bookr project and append the following code to the INSTALLED_APPS list:

```
INSTALLED_APPS = [
    '...',
    '...',
    'bookr_test'
]
```

3. Now, with the application setup complete, create a new database model that you will use for testing purposes. For this exercise, we are going to create a new model named Publisher that will store the details about the book publisher in our database. To create the model, open the models.py file under the bookr_test directory and add the following code to it:

```
from django.db import models

class Publisher(models.Model):
    """A company that publishes books."""
    name = models.CharField(
        max_length=50, help_text="The name of the Publisher.")
    website = models.URLField(
        help_text="The Publisher's website.")
    email = models.EmailField(
        help_text="The Publisher's email address.")

    def __str__(self):
        return self.name
```

In the preceding code snippet, you created a new class named Publisher that inherits from the Model class of the Django's models module, defining the class as a Django model that will be used to store data about the publisher:

```
class Publisher(models.Model)
```