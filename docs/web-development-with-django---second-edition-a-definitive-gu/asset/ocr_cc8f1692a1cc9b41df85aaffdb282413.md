Now, when a request for a static file comes in, the web server will be able to serve it easily because the paths are mapped consistently:

+   - `/static/main.css` is served from the `/var/www/business_site/static/main.css` file
+   - `/static/reviews/logo.png` is served from the `/var/www/business_site/static/reviews/logo.png` file

This means the web server root is `/var/www/business_site/` and static paths are loaded directly from disk in the usual manner that a web server would load files.

With that, we have demonstrated how Django locates static files during development and can serve them itself. In production, we need the frontend web server to be able to serve static files without involving Django, for both safety and speed.

Without having to run `collectstatic`, a web server would not be able to map a URL back to a path. For example, it would not know that `main.css` must be loaded from the project static directory while `logo.png` is to be loaded from the landing app directory—it has no concept of the Django directory layout.

You might be tempted to serve files directly from the Django project directory by setting your web server root to this directory—**don’t do this.** There is a **security risk** in sharing your entire Django project directory as it would make it possible to download `settings.py` or other sensitive files. Running `collectstatic` will copy the files to a directory that can be moved outside the Django project directory to the web server root for security.

So far, we have talked about using `collectstatic` to copy static files directly to the web server root. You could also have Django copy them to an intermediary directory and have your deployment process move to a CDN or another server afterward. We won’t go into detail on specific deployment processes; how you choose to copy static files to the web server will depend on your or your company’s existing setup (for example, a continuous delivery pipeline).

> **Note**
> 
> The collectstatic command does not take into consideration the use of static template tags. It will collect all the static files inside static directories, even those that your project does not include inside a template.

In the next exercise, we will see the `collectstatic` command in action. We’ll use it to copy all the `business_site` static files that we have so far into a temporary directory.