In the following sections, we will explore the contents of the project directory, learn what Django app directories contain, and see how to load up your project in PyCharm.

# The myproject directory

As well as the manage.py file, the other file item created by startproject is the myproject directory. This is the actual Python package for your project. It contains settings for the project, some configuration files for your web server, and the global URL maps. Inside the myproject directory are five files:

+   - __init__.py
+   - asgi.py
+   - settings.py
+   - urls.py
+   - wsgi.py

They can also be seen in the following screenshot:

![](bbox=[0.21, 0.46, 0.79, 0.68])

Figure 1.9 – The myproject package (inside the myproject project directory)

Now let’s see what these files contain:

+   - __init__.py: This is an empty file that lets Python know that the myproject directory is a Python module. You’ll be familiar with these files if you’ve worked with Python before.
+   - settings.py: This contains all the Django settings for your application. We will explain the contents soon.
+   - urls.py: This has the global URL mappings that Django will initially use to locate views or other child URL mappings. You will add a URL map to this file soon.