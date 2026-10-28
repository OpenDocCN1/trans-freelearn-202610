# Installation and configuration

To install djangorestframework in the virtual environment setup along with PyCharm, follow these steps:

+   1. Enter the following code in your Terminal app or Command Prompt to do this:
+   2. Next, open the settings.py file and add rest_framework to INSTALLED_APPS as shown in the following snippet:

```
pip install djangorestframework
```

```
INSTALLED_APPS = [
    "bookr_admin.apps.BookrAdminConfig",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "rest_framework",
    "reviews", ]
```

You are now ready to start using DRF to create your first simple API.

# Functional API views

In Chapter 3, URL Mapping, Views, and Templates, we learned about simple functional views that take a request and return a response. We can write similar functional views using DRF. However, note that class-based views are more commonly used and will be covered next. A functional view is created by simply adding the following decorator onto a normal view, as follows:

```
from rest_framework.decorators import api_view

@api_view
def my_view(request):
    ...
```

This decorator takes the functional view and turns it into a subclass of the APIView DRF. It's a quick way to include an existing view as part of your API. Using what we have learned so far, we will create a simple REST API using DRF in the next section.