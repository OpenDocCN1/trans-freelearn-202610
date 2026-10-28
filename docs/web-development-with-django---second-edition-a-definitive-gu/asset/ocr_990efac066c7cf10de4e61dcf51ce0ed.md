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