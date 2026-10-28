A URL mapping to the static view is automatically set up when running the Django development server, provided that your settings.py file consists of the following elements:

+   - Has DEBUG set to True
+   - Contains 'django.contrib.staticfiles' in its INSTALLED_APPS

Both settings exist by default.

The URL mapping that is created is roughly equivalent to having the following map in your urlpatterns:

```
path(settings.STATIC_URL, django.conf.urls.static)
```

Any URL starting with settings.STATIC_URL (which is /static/ by default) gets mapped to the static view.

> Note
> You could use the static view without having staticfiles in INSTALLED_APPS, but you must set up an equivalent URL mapping manually.

Next, we’ll talk about how Django locates static files using Static Files Finder.

# Introduction to Static Files Finder

There are three times when Django needs to locate static files on disk, and for this, it uses static file finder. It can be thought of as a plugin. It is a class that implements methods for converting URL paths into disks and iterates through the project directory to find static files.

The first time Django does this is when the Django static view receives a request to load a particular static file; here, it needs to convert the path in the URL into a location on disk. For example, let’s say the URL’s path is /static/logo.png, and it is converted into the bookr/static/logo.png path on the disk. As we noted in the previous section, this only happens during development. On a production server, Django should not receive this request as it will be handled directly by the web server.

The second time is when using the collectstatic management command. This gathers all the static files in the project directory and copies them to a single directory to be served by the production web server. bookr/static/logo.png will get copied to the web server root – for example, /var/www/bookr/static/logo.png. Static File Finder contains code for locating all the static files inside your project directory.

The last time Static File Finder is used is while executing the findstatic management command. This is similar to the first usage, where it accepts a static filename (such as logo.png), but it outputs the full path (bookr/static/logo.png) to the terminal instead of loading the file’s content.