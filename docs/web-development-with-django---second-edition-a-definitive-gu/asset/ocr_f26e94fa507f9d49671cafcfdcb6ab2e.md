If you see the image shown in Figure 5.7, you have set up static file serving correctly.

Now that you’ve seen that loading static images works correctly, we’ll learn more about how to prevent hardcoding these URLs. In the next section, you’ll see how Django can automatically generate correct static URLs for use in templates.

# Generating static URLs with the static template tag

In Exercise 5.01 – serving a file from an app directory, you set up an image file to be served by Django. We saw that the URL of the image was http://127.0.0.1:8000/static/landing/logo.png, which you could use inside an HTML template. For example, to display the image with an img tag, you could use this code in your template:

```
<img src="http://127.0.0.1:8000/static/landing/logo.png">
```

Or, since Django is also serving the media and has the same host as the dynamic template response, you can simplify this by just including the path, as follows:

```
<img src="/static/landing/logo.png">
```

Both addresses (URLs and paths) have been hardcoded into the template – that is, we include the full path to the static file and make assumptions about where the file is being hosted. This works fine with the Django dev server or if you host your static files and Django website on the same domain. For more performance as your site becomes more popular, you might consider serving static files from their own domain or Content Delivery Network (CDN).

> Note
> A CDN is a service that can host parts or all of your website for you. It provides several web servers and can seamlessly speed up the loading of your website. For example, it might serve files to a user from the server that is geographically closest to them. There are several CDN providers and depending on how they are set up, they might require you to specify a certain domain from which to serve your static files.

Take, for instance, a common separation approach: using a different domain for static file serving. You host your main website at https://www.example.com but want to serve static files from https://static.example.com. During development, we could use just the path to the logo file, as in the example we just saw. But when we deploy to the production server, our URLs would need to change so that they include the domain, like so:

```
<img src="https://static.example.com/landing/logo.png">
```