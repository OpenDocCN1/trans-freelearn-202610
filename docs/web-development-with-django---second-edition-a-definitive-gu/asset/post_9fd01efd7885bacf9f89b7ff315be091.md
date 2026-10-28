
![](img/4840a43efd335abd10b93708225692d6_239_0.png)

*Figure 5.2 – Directory layout with namespaced directories*

To load a specific file, we must include the namespaced directory too. For the reviews logo, the URL path is `/static/reviews/logo.png`, which maps to `bookr/reviews/static/reviews/logo.png` on the disk. Similarly, for the store logo, the path is `/static/store/logo.png`, which maps to `bookr/store/static/store/logo.png`. You might have noticed that the example paths for the logo.png file were already namespaced in the *Static file finders – use during a request* section.

> **Note**
> 
> If you are considering writing a Django app that might be released as a standalone plugin, you could use an even more explicit sub-directory name. For example, you could choose one that contains the entire dotted project path: `bookr/reviews/static/bookr.reviews`. In most cases, though, it’s fine for the sub-directory name to be unique for just your project.

Now that we have introduced `AppDirectoriesFinder` and static file namespacing, we can use them to serve our first static file. In the first exercise of this chapter, we will create a new Django project for a basic business site. We will then serve a logo file from an app called `landing` that we will create in this project. The `AppDirectoriesFinder` class is used to find static files inside each app directory, in a directory called `static`. The application must be listed in the `INSTALLED_APPS` setting in your `settings.py` file. As we mentioned in *Chapter 1, An Introduction to Django*, it is good for apps to be self-contained. By letting each application have a `static` directory, we can continue the self-contained design by also storing app-specific static files inside the app directory.

The easiest way to serve a static file is from an app directory. This is because we don’t need to make any settings changes. Instead, we just need to create the files in the correct directory, and they will be served using the default Django configuration.

> **The business site project**
> For the exercises in this chapter, we’ll create a new Django project, and use it to demonstrate the static file concepts. The project will be a basic business site with a simple landing page that has a logo. The project will have one app, called *landing*.
> 
> You can refer to *Exercise 1.01 – creating a project and app, and starting the dev server from Chapter 1, An Introduction to Django*, to refresh your memory on creating a Django project.

## Exercise 5.01 – serving a file from an app directory

In this exercise, you will add a logo file for the *landing* app. This will be done by putting a `logo.png` file in a `static` directory inside the *landing* app directory. Once you’ve done this, you can test that the static file is being served correctly and confirm the URL that will serve it:

- 1. Start by creating the new Django project. You can reuse the Bookr virtual environment that already has Django installed.
- 2. Open a new terminal and activate the virtual environment (refer to the *Preface* for instructions on how to create and activate a virtual environment).
- 3. Run the `django-admin` command in the terminal (or Command Prompt) to start a Django project named `business_site`:
```
django-admin startproject business_site
```
There won’t be any output. This command will scaffold the Django project in a new directory named `business_site`.
- 4. Create a new Django app in this project by using the `startapp` management command. The app should be called *landing*. To do this, `cd` into the `business_site` directory, then run the following:
```
python3 manage.py startapp landing
```
Note that there won’t be any output again. The command will create the `landing` app directory inside the `business_site` directory.

> **Note**
> Remember that on Windows, the command is `python manage.py startapp landing`.

5. Launch PyCharm and open the business_site directory. If you already have a project open, you can do this by choosing File | Open; otherwise, just click Open in the Welcome to PyCharm window. Navigate to the business_site directory, select it, then click Open. If prompted, choose Trust Project. The business_site project window should look similar to what’s shown in Figure 5.3:

> Note
> For detailed instructions on how to set up and configure PyCharm to work with your Django project, refer to Exercise 1.02 – project setup in PyCharm, in Chapter 1, An Introduction to Django.

![](img/4840a43efd335abd10b93708225692d6_241_0.png)

Figure 5.3 – The business_site project

6. Create a new run configuration to execute manage.py runserver for the project. You can reuse the Bookr virtual environment again. The Run/Debug Configurations window should look similar to what’s shown in Figure 5.4 when you’re done:

## Note

> If you are unsure of how to configure these settings in PyCharm, refer to Exercise 1.02 – project setup in PyCharm, in Chapter 1, An Introduction to Django.

![](img/4840a43efd335abd10b93708225692d6_242_0.png)

Figure 5.4 – Run/Debug Configurations for Runserver

You can test that the configuration has been set up correctly by clicking the Run button, and then visiting http://127.0.0.1:8000/ in your browser. You should see the Django welcome screen. If the debug server fails to start or you see the Bookr main page, then you probably still have the Bookr project running. Try stopping the Bookr runserver process (press Ctrl + C in the terminal that’s running it) and then starting the new one you just set up.

- 7. Open settings.py in the business_site directory and add 'landing' to the INSTALLED_APPS setting. We learned how to do this in Step 1 of Exercise 1.05 – creating a templates directory and base template, in Chapter 1, An Introduction to Django.
- 8. In PyCharm, right-click on the landing directory in the project pane and select New | Directory.

9. Enter `static` and press the *Enter* key:

![](img/4840a43efd335abd10b93708225692d6_243_0.png)

Figure 5.5 – Naming the directory static

10. Right-click on the `static` directory you just created and select **New | Directory** again.

11. Enter `landing` and press *Enter*. This will implement namespacing of the static files directory, as we discussed earlier:

![](img/4840a43efd335abd10b93708225692d6_243_1.png)

Figure 5.6 – Naming the new directory reviews to implement namespacing

12. Download the `logo.png` file from `https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter05/Exercises.01/business_site/landing/static/landing/logo.png` and move it into the `landing/static/landing` directory.

13. Start the Django dev server. If it is not already running, then navigate to `http://127.0.0.1:8000/static/landing/logo.png`. You should see the image being served in your browser:

![](img/4840a43efd335abd10b93708225692d6_243_2.png)

Figure 5.7 – Image served by Django

If you see the image shown in Figure 5.7, you have set up static file serving correctly.

Now that you’ve seen that loading static images works correctly, we’ll learn more about how to prevent hardcoding these URLs. In the next section, you’ll see how Django can automatically generate correct static URLs for use in templates.

## Generating static URLs with the static template tag

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
