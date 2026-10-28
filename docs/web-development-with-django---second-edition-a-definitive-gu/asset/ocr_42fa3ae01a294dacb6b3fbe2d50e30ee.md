In this exercise, we put some CSS rules into a file and served them using Django’s `FileSystemFinder`. This was accomplished by creating a `static` directory inside the `business_site` project directory and specifying it in the Django settings (the `settings.py` file) using the `STATICFILES_DIRS` setting. We linked the `main.css` file to the `index.html` template using the static template tag. We loaded the main page in our browser and saw that the font and color changes applied.

We’ve now covered how static file finders are used during a request (to load a specific static file when given a URL). We’ll now look at their other use case: finding and copying static files for production deployment when running the `collectstatic` management command.

# ## Static file finders – use during collectstatic

Once we have finished working on our static files, they need to be moved into a specific directory that can be served by our production web server. We can then deploy our website by copying our Django code and static files to our production web server. In the case of `business_site`, we will want to move `logo.png` and `main.css` (along with other static files that Django itself includes) into a single directory that can be copied to the production web server. This is the role of the `collectstatic` management command.

We have already discussed how Django uses static file finders during request handling. Now, we will cover the other use case: collecting static files for deployment. Upon running the `collectstatic` management command, Django uses each finder to list static files on the disk. Every static file that is found is then copied into the `STATIC_ROOT` directory (also defined in `settings.py`). This is a little bit like the reverse of handling a request. Instead of getting a URL path and mapping it to a filesystem path, the filesystem path is copied to a location that can be predicted by the frontend web server. This allows the frontend web server to handle a request for a static file independently of Django.

> **Note**
> 
> A frontend web server is a web server that’s designed to route requests to applications (such as Django) or read static files from disk. They can handle requests faster but aren’t able to generate dynamic content in the same way as something such as Django. Frontend web servers are software such as Apache HTTPD, Nginx, and lighttpd.

For some specific examples of how `collectstatic` works, we’ll use the two files from *Exercise 5.01 – serving a file from an app directory* and *Exercise 5.03 – serving from a project status directory*, respectively: `landing/static/landing/logo.png` and `static/main.css`.

Let’s assume that `STATIC_ROOT` has been set to a directory being served by a normal web server – this would be something such as `/var/www/business_site/static`. The destination for these files would be `/var/www/business_site/static/reviews/logo.png` and `/var/www/business_site/static/main.css`, respectively.