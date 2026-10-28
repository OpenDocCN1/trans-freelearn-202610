Next, we’ll see how Django locates template files.

# Finding HTML templates in app directories

There are many options that are available to tell Django how to find templates, which can be set in the TEMPLATES setting of settings.py, but the easiest one (for now) is to create a templates directory inside the reviews directory. Django will look in this (and in other apps’ templates directories) because of APP_DIRS being True in the settings.py file, as we saw in the previous section. However, for Django to know that the reviews directory is an app, we need to configure it in the settings. We’ll do that in the next exercise.

# Exercise 1.05 – creating a templates directory and a base template

In this exercise, you will create a templates directory for the reviews app. Then, you will add an HTML template file that Django will be able to render to an HTTP response.

We discussed settings.py and its INSTALLED_APPS setting in the Exploring Django settings section. We need to add the reviews app to INSTALLED_APPS for Django to be able to find templates. Follow these steps:

+   1. Open settings.py in PyCharm. Update the INSTALLED_APPS setting and add reviews to the end. It should look like this:

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'reviews'
]
```