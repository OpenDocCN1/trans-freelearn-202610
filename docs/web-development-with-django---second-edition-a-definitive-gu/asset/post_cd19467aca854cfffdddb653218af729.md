
Django provides the static URL mapping that can be added to your existing URL maps to serve media files. It is added to your urls.py file like this:

```
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    # your existing URL maps
]
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL,
                          document_root=settings.MEDIA_ROOT)
```

This will serve the MEDIA_ROOT setting defined in settings.py to the MEDIA_URL setting that is also defined there. The reason we check for settings.DEBUG before appending the map, so we don’t add this map in production.

For example, if your MEDIA_ROOT was set to /var/www/bookr/media, and your MEDIA_URL was set to /media/, then the /var/www/bookr/media/image.jpg file would be available at http://127.0.0.1:8000/media/image.jpg.

The static URL map does not work when the Django DEBUG setting is False, so it can’t be used in production. However, as has been mentioned, in production, your web server should be serving these requests so Django will not need to handle them.

In the first exercise, you will create a new add MEDIA_ROOT and MEDIA_URL to your settings.py. You will then add the static media serving URL map and add a test file to ensure media serving is configured correctly.

## Exercise 8.01 – configuring media storage and serving

In this exercise, you will set up a new Django project as an example project to use throughout this chapter. Then, you’ll configure it to be able to serve media files. You’ll do this by creating a media directory and adding the MEDIA_ROOT and MEDIA_URL settings. Then you’ll set up the URL mapping for MEDIA_URL.

Figure 8.1 shows the Runserver configuration of the project in PyCharm.

3. Create a new directory named media inside the media_project project directory. Then, create a new file in this directory named test.txt. The directory structure of this will look like Figure 8.2:

![](img/4840a43efd335abd10b93708225692d6_391_0.png)

Figure 8.2: The media directory and the test.txt layout

4. The test.txt file will also open automatically. Enter Hello, world! into it, then you can save and close the file.

5. Open settings.py inside the media_project package directory. At the end of the file, add a setting for MEDIA_ROOT using the path to the media directory you just created. Join the name of the directory (media) onto BASE_DIR:

```
MEDIA_ROOT = BASE_DIR / "media"
```

6. Directly below the line added in step 5, add another setting for MEDIA_URL; this should just be "/media/":

```
MEDIA_URL = "/media/"
```

With these changes made, settings.py should look like this: https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter08/Exercise8.01/media_project/media_project/settings.py.

7. Open the media_project package's urls.py file. After the urlpatterns definition, add the following code to add the media serving URL if running in the DEBUG mode. First, you will need to import the Django settings and static serving view by adding the highlighted import lines above the urlpatterns definition:

```
from django.contrib import admin
from django.urls import path
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
```

```
path("admin/", admin.site.urls),
]
```

8. Then, add the following code right after your urlpatterns definition (refer to the code block in the previous step) to conditionally add a mapping from the MEDIA_URL setting to the static view, which will serve from MEDIA_ROOT:

```
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL,
    document_root=settings.MEDIA_ROOT)
```

You can now save this file. It should look like this: https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter08/Exercise8.01/media_project/media_project/urls.py.

9. Start the Django dev server, if it is not already running, then visit http://127.0.0.1:8000/media/test.txt. If you did everything correctly, you should see the Hello, world! text in your browser:

![](img/4840a43efd335abd10b93708225692d6_392_0.png)

Figure 8.3: Serving a media file

If your browser looks like Figure 8.3, it means that the media files are being served from the MEDIA_ROOT directory.

The test.txt file we created was just for testing, but we will use it in Exercise 8.02 – template settings, and using MEDIA_URL in templates, so don’t delete it yet.

In this exercise, we configured Django to serve media files. We served a test file just to make sure everything worked as expected, and it did. We’ll now look at how we can automatically generate media URLs in templates.

## Context processors and using MEDIA_URL in templates

To use MEDIA_URL in a template, we could pass it in through the rendering context dictionary in our view. For example, as shown in the following code block:

```
```python
from django.conf import settings

def my_view(request):
    return render(request, "template.html", {"MEDIA_URL":
        settings.MEDIA_URL, "username": "jbloggs"})
```
```

This will work, but the problem is that MEDIA_URL is a common variable that we might want to use in many places, so we’d have to pass it through in practically every view.

Instead, we can use a **context processor**, which is a way of adding one or more variables automatically to the context dictionary on every render call.

A context processor is a function that accepts one argument, the current request. It returns a dictionary of context information that will be merged with the dictionary that was passed to the render call.

We can look at the source code of the media context processor, which illustrates how they work:

```
```python
def media(request):
    """
    Add media-related context variables to the context.
    """
    return {"MEDIA_URL": settings.MEDIA_URL}
```
```

With the media context processor activated, MEDIA_URL will be added to your context dictionaries. We could change our render call, seen previously, to this:

```
```python
return render(request, "template.html", {"username": "jbloggs"})
```
```

The same data would be sent to the template, as the context processor would add MEDIA_URL.

The full module path to the media context processor is django.template.context_processors.media. Some examples of other context processors that Django provides are the following:

- `django.template.context_processors.debug`: This returns the `{"DEBUG": settings.DEBUG}` dictionary.
- `django.template.context_processors.request`: This returns the `{"request": request}` dictionary; that is, it just adds the current HTTP request to the context.

To enable a context processor, its module path must be added to the context_processors option of your TEMPLATES setting. For example, to enable the media context processor, add django.template.context_processors.media. We will cover how to do this in detail in Exercise 8.02 – Template settings and using MEDIA_URL in templates.

Once the media context processor is enabled, the MEDIA_URL variable can be accessed inside a template just like a normal variable:

```
{{ MEDIA_URL }}
```

You could use it, for example, to source an image:

```
<img src="{{ MEDIA_URL }}uploads/image.jpg">
```

Note that, unlike with static files, there is no template tag for loading media files (i.e., there is no equivalent to the {% static %} template tag).

Custom context processors can also be written. For example, referring back to the Bookr application that we built, we might want to show a list of the five latest reviews in a sidebar that’s on every page. A context processor like this would perform the following code:

```
from reviews.models import Review

def latest_reviews(request):
    return {"latest_reviews": Review.objects.order_by("-date_created")[:5]}.
```

This would be saved in a file named context_processors.py in the Bookr project directory, then referred to in the context_processors setting by its module path, context_processors.latest_reviews. Or we could save it inside the reviews app and refer to it as reviews.context_processors.latest_reviews. It is up to you to decide whether a context processor should be considered project-wide or app specific. However, bear in mind that regardless of where it is stored, once activated, it applies to all the render calls for all apps.

A context processor can return a dictionary with multiple items or even zero items. It would do this if it had conditions to only add items if certain criteria were met. For example, show the latest reviews only if the user is logged in.

Let us explore this in detail in the next exercise.
