# Working with Django templates

In Exercise 3.01 – implementing a simple function-based view, we saw how to create a view, do the URL mapping, and display a message in the browser. But if you recall, we hardcoded the Welcome to Bookr! HTML message in the view function itself and returned an HttpResponse object, as follows:

```
message = f"<html><h1>Welcome to Bookr!</h1> "
"<p>{Book.objects.count()} books and counting!</p></html>"
return HttpResponse(message)
```

Hardcoding HTML inside Python modules is not a good practice, because as the amount of content to be rendered on a web page increases, so does the amount of HTML code we need to write for it. Having a lot of HTML code among Python code can make the code hard to read and maintain in the long run.

For this reason, Django templates provide us with a better way to write and manage HTML templates. Django's templates not only work with static HTML content but also with dynamic HTML templates.

Django's template configuration is done in the TEMPLATES variable present in the settings.py file. This is how the default configuration looks:

```
TEMPLATES = [
    {
        'BACKEND':
            'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```