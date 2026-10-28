

Another option is to use the `fieldsets` attribute of the `ModelAdmin` subclass to specify the form layout as a series of grouped fields. Each grouping in `fieldsets` consists of a title followed by a dictionary containing a `fields` key pointing to a list of field name strings:

```
fieldsets = (
    ("Linkage", {"fields": ("creator", "book")}),
    ("Review content", {"fields": ("content", "rating")}),
)
```

The review form should look as follows:

![](img/4840a43efd335abd10b93708225692d6_226_0.png)

Figure 4.59 – The review form with fieldsets

If we want to omit the title on a fieldset, we can do so by assigning the None value to it:

```
fieldsets = (
    (None, {'fields': ('creator', 'book')}),
    ('Review content', {'fields': ('content',
                                   'rating')})
)
```

Now, the review form should appear as shown in the following screenshot:

![](img/4840a43efd335abd10b93708225692d6_227_0.png)

Figure 4.60 – The review form with the first fieldset untitled

Now that we have learned about change lists and form customizations, we can put our knowledge into practice with a comprehensive activity.

### Activity 4.03 – customizing the Model admins

In our data model, the `Contributor` class is used to store data for book contributors; they can be authors, contributors, or editors. This activity focuses on modifying the `Contributor` class and adding a `ContributorAdmin` class to improve the user-friendliness of the admin app. At present, the `Contributor` change list defaults to a single column, `FirstNames`, based on the `__str__` method created in *Chapter 2, Models and Migrations*. We will investigate some alternative ways of representing this. These steps will help you complete the activity:

1. Edit `reviews/models.py` to add additional functionality to the `Contributor` model.
2. Add an `initialled_name` method to `Contributor` that takes no arguments (like the `Book.isbn13` method).
3. The `initialled_name` method will return a string containing `Contributor.last_names` followed by a comma and the initials of the given names. For example, for a `Contributor` object with `first_names` of `Jerome David` and `last_names` of `Salinger`, `initialled_name` will return `Salinger, JD`.
4. Replace the `__str__` method for `Contributor` with one that calls `initialled_name()`.

At this point, the **Contributors** display list will look like this:

![](img/4840a43efd335abd10b93708225692d6_229_0.png)

Figure 4.61 – The Contributors display list

5. Edit the `reviews/admin.py` file. We will modify `ContributorAdmin` that was subclassed from `admin.ModelAdmin` in the *The list display fields* section.

6. Modify it so that on the Contributors change list, records are displayed with two sortable columns: Last Names and First Names. We can remove the existing `list_display` attribute from `ContributorAdmin` along with the `initialled_name` function.

7. Add a search bar that searches on Last Names and First Names. Modify it so that it only matches the start of Last Names.

8. Add a filter on Last Names.

By completing the activity, you should see something like this:

![](img/4840a43efd335abd10b93708225692d6_230_0.png)

Figure 4.62 – Expected output

Changes such as these can be made to improve the functionality of the admin user interface. By implementing the First Names and Last Names columns as separate columns in the Contributors change list, we give the user an option to sort on either of the fields. By considering what columns are most useful in search retrieval and filter selections, we can improve the efficient retrieval of records.

> **Note**
> The solution for this activity can be found on https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/ActivitySolutions.

### Summary

In this chapter, we saw how to create superusers through the Django command line and how to use them to access the admin app. Then, after a brief tour of the admin app’s basic functionality, we examined how to register our models with it to produce a CRUD interface for our data.

Then we learned how to refine this interface by modifying site-wide features. We altered how the admin app presents model data to the user by registering custom model admin classes with the admin site. This allowed us to make fine-grained changes to the representation of our models’ interfaces. These modifications included customizing change list pages by adding additional columns, filters, date hierarchies, and search bars. We also modified the layout of the model admin pages by grouping and excluding fields.

This was only a very shallow dive into the functionality of the admin app. We will revisit the rich functionality of AdminSite and ModelAdmin in Chapter 10, Advanced Django Admin and Customization. But first, we need to learn some more intermediate features of Django. In the next chapter, we will learn how to organize and serve static content, such as CSS, JavaScript, and images, from a Django app.

## Serving Static Files

A web application with just plain HyperText Markup Language (HTML) is quite limiting. We can enhance the look of web pages with Cascading Style Sheets (CSS) and images, and add interaction with JavaScript. We call all these kinds of files “static files.” They are developed and then deployed as part of the application. We can compare this to dynamic responses, which are generated in real time when a request is made. All the views you have written generate a dynamic response by rendering a template. Note that we will not consider templates to be static files as they are not sent verbatim to a client; instead, they are rendered first and sent as part of a dynamic response.

During development, the static files are created on the developer’s machine, and then they must be moved to the production web server. If you must move to production in a short timeframe (say, a few hours) then it can be time-consuming to collect all the static assets, move them to the correct directory, and upload them to the server. When developing web applications using other frameworks or languages, you might need to manually put all of your static files into a specific directory that your web server hosts. Making changes to the URL from which static files are served might mean updating values throughout your code.

Django can manage static assets for us to make this process easier. It provides tools for serving them with its development server during development. When your application goes to production, it can also collect all your assets and copy them to a folder for a dedicated web server to host. This allows you to keep your static files segregated in a meaningful way during development, and automatically bundle them for deployment.

This functionality is provided by Django’s built-in staticfiles app. It adds several useful features for working with and serving static files:

- The `static` template tag automatically builds the static URL for an asset and includes it in your HTML.
- A view (called `static`) serves static files in development.
- Static file finders can be used to customize where assets are found on your filesystem.
- The `collectstatic` management command finds all static files and moves them into a single directory for deployment.
- The `findstatic` management command shows which static file on the disk has been loaded for a particular request. This also helps us debug if a particular file is not being loaded.

In the exercises and activities of this chapter, we will be adding static files (images and CSS) to the Bookr application. Each file will be stored inside the Bookr project directory during development. We will need to generate a URL for each so that the templates can reference them, and the browser can download them. Once the URL has been generated, Django will need to serve these files. When we deploy the Bookr application to production, all the static files need to be found and moved to a directory where they can be served by the production web server. If there are static files that are not loading as expected, we need some method of determining what the cause is.

For the sake of simplicity, let’s take a single static file as an example: `logo.png`. We will briefly introduce the role of each feature we mentioned in the previous paragraph and explain it in depth throughout this chapter. There are five parts of the static files app that we will look at:

- The `static` template tag is used to convert a filename into a URL or path that can be used in a template – for example, from `logo.png` to `/static/logo.png`.
- The `static` view receives a request to load the static file at the `/static/logo.png` path. It reads the file and sends it to the browser.
- A static file finder (or just `finder`) is used by the `static` view to locate the static file on the disk. There are different finders, but in this example, one is just converting from the `/static/logo.png` URL path to the path on the disk, `bookr/static/logo.png`.
- When deploying to production, the `collectstatic` management command must be used. This will copy the `logo.png` file from the Bookr project directory to a web server directory, such as `/var/www/bookr/static/logo.png`.
- If a static file is not working (for example, the request for it returns a `404 Not Found` response, or the wrong file is being served), then we can use the `findstatic` management command to try to determine the reason. This command takes the filename as a parameter and will output which directories were looked through and where it was able to locate that requested file.

These are the most common features that are used day to day, but there are others that we will also discuss.

In this chapter, you will start by learning the difference between static and dynamic responses. You will then see how the Django `staticfiles` app helps manage static files. As you continue to work on the Bookr app, you will enhance it with images and CSS. You’ll see the different ways you can lay out your static files for your project and examine how Django consolidates them for production deployment. Django includes tools for referencing static files in templates; you’ll see how these tools help reduce the amount of work you must do when deploying the application to production. After this,

