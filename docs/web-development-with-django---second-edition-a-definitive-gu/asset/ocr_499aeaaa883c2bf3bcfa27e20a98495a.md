In this exercise, we created a templates directory for the reviews app and added an HTML template to it. The HTML template will be rendered once we implement the use of the render function on our view.

# Rendering a template with the render function

We now have a template to use, but we need to update our index view so that it renders the template, instead of returning the Hello (name)! text that it is currently displaying (refer to Figure 1.30 to see how it currently looks). We will do this by using the render function and providing the name of the template. render is a shortcut function that returns HttpResponse. There are other ways to render a template that provide more control over how it is rendered, but for now, this function is fine for our needs. render takes at least two arguments – the first is always the request that was passed to the view, and the second is the name/relative path of the template being rendered. We will also call it with a third argument, the render context that contains all the variables that will be available in the template – there’ll be more on this in Exercise 1.07 – using variables in templates.

# Exercise 1.06 – rendering a template in a view

In this exercise, you will update your index view function to render the HTML template you created in Exercise 1.05 – creating a templates directory and a base template. You will make use of the render function, which loads your template from disk, renders it, and sends it to the browser. This will replace the static text you are currently returning from the index view function:

+   1. In PyCharm, open views.py in the reviews directory.
+   2. We no longer manually create HttpResponse, so remove the HttpResponse import line:
+   3. Replace it with an import of the render function from django.shortcuts:
+   4. Update the index function so that, instead of returning HttpResponse, it’s returning a call to render, passing in request and the template name:

```
from django.http import HttpResponse
```

```
from django.shortcuts import render
```

```
def index(request):
    return render(request, "base.html")
```