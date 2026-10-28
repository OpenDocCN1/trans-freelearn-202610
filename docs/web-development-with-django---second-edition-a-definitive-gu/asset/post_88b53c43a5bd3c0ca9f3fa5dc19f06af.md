
```
'django.template.context_processors.request',
'django.contrib.auth.context_processors.auth',
'django.contrib.messages.context_processors
    .messages',
],
},
},
]
```

+   3. Add the following lines of code into the `base.html` file:

```
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Home Page</title>
</head>
<body>
    <h1>Welcome to Bookr!</h1>
</body>
</html>
```

This is simple HTML that displays the Welcome to Bookr! message in the header.

+   4. Modify the code inside `bookr/reviews/views.py` so that it looks as follows:

```
from django.shortcuts import render

def welcome_view (request):
    return render(request, 'base.html')
```

Since we have already configured the 'templates' directory in the TEMPLATES configuration, `base.html` is available for use by the template engine. The code renders the `base.html` file using the imported render method from the `django.shortcuts` module.

+   5. Save the files, run `./manage.py runserver`, and open the http://0.0.0.8000/ or http://127.0.0.1:8000/ URL to check the newly added template loading in the browser:

![](img/4840a43efd335abd10b93708225692d6_156_0.png)

Figure 3.3 – Displaying "Welcome to Bookr!" on the home page

In this exercise, we created an HTML template and used Django templates and views to return the `Welcome to Bookr!` message.

Overall, we can say that Django templates help us to work with HTML templates and present information to the user in a desired format. Next, we will learn about the Django template language, which can be used to render the application's data along with HTML templates.

## Django’s template language

Django templates not only return static HTML templates but can also add dynamic application data while generating the templates. Along with data, we can also include some programmatic elements in the templates. All of these put together form the basics of Django’s template language. The following few sections look at some of the basic parts of the Django template language, such as template variables, template tags, comments, and filters.

### Template variables

A template variable is represented between two curly braces, as shown here:

```
{{ variable }}
```

When this is present in the template, the value carried by the variables will be replaced in the template. Template variables help add the application's data into the templates:

```
template_variable = "I am a template variable."

<body>
    {{ template_variable }}
</body>
```

### Template tags

A tag is similar to a programmatic control flow, such as an if condition or a for loop. A tag is represented between two curly braces and percentage signs, as shown. Here is an example of a for loop iterating over a list using template tags:

```
{% for element in element_list %}

{% endfor %}
```

Unlike Python programming, we also add the end of the control flow by adding the end tag, such as {% endfor %}. This can be used along with template variables to display the elements in the list, as shown here:

```
<ul>
    {% for element in element_list %}
        <li>{{ element.title }}</li>
    {% endfor %}
</ul>
```

### Comments

Comments in the Django template language can be written as shown here; anything in between {% comment %} and {% endcomment %} will be commented out:

```
{% comment %}
    <p>This text has been commented out</p>
{% endcomment %}
```

### Filters

Filters can be used to modify a variable to represent it in a different format. The syntax for a filter is a variable separated from the filter name using a pipe (|) symbol:

```
{{ variable|filter }}
```

Here are some examples of built-in filters:

- {{ variable|lower }}: This converts the variable string into lowercase
- {{ variable|title }}: This converts the first letter of every word into uppercase

Let’s use the concepts we have learned to develop the book review application.

The render function takes three parameters: the first one is the request object that was passed into the view, the second is the books_list.html HTML template, which will display the list of books, and the third is context, which we pass to the template.

Since we have passed book_list as a part of the context, the template will be using this to render the list of books using template tags and template variables.

3. Create the book_list.html file in the bookr/reviews/templates/reviews/books_list.html path and add the following HTML code to the file:

```
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Bookr</title>
</head>
    <body>
        <h1>Book Review application</h1>
        <hr>
```

You can find the complete code at https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/blob/main/Chapter03/Exercise3.03/bookr/reviews/templates/reviews/book_list.html.

This is a simple HTML template with template tags and variables iterating over book_list to display the list of books.

4. In bookr/reviews/urls.py, add the following URL pattern to invoke the books_list view:

```
from django.urls import path
from . import views

urlpatterns = [
    path('books/', views.book_list,
        name='book_list'),
]
```

This does the URL mapping for the books_list view function.

## Template inheritance

As we build the project, the number of templates will increase. It is highly probable that when we design the application, some of the pages will look similar and have common HTML code for certain features. Using template inheritance, other HTML files can inherit the common HTML code. This is similar to class inheritance in Python, where the parent class has all the common code, and the child class has those extras that are unique to the child's requirements.

For example, let's consider the following to be a parent template that is named base.html:

```
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Hello World</title>
</head>
<body>
<h1>Hello World using Django templates!</h1>
{% block content %}
{% endblock %}
</body>
</html>
```

The following is an example of a child template:

```
{% extends 'base.html' %}
{% block content %}
<h1>How are you doing?</h1>
{% endblock %}
```

In the preceding snippet, the {% extends 'base.html' %} line extends the template from base.html, which is the parent template. After extending from the parent template, any HTML code in between the block content will be displayed along with the parent template. Once the child template has been rendered, here is how it looks in the browser:

![](img/4840a43efd335abd10b93708225692d6_161_0.png)

Figure 3.5 – Greeting message after extending the base.html template

In the next section, we will do some template styling using Bootstrap.

## Template styling with Bootstrap

We have seen how to display all the books using views, templates, and URL mapping. Although we were able to display all the information in the browser, it would be even better if we could add some styling and make the web page look better. For this, we can add a few elements of Bootstrap. Bootstrap is an open source Cascading Style Sheets (CSS) framework that is particularly good for designing responsive pages that work across desktop and mobile browsers.

Using Bootstrap is simple. First, you need to add Bootstrap CSS to your HTML. You can experiment yourself by creating a new file called example.html. Populate it with the following code and open it in a browser:

```
<!doctype html>
<html lang="en">
<head>
<!-- Required meta tags -->
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

<!-- Bootstrap CSS -->
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9M uhOf23Q9Ifjh" crossorigin="anonymous">
</head>
<body>
Content goes here
</body>
</html>
```

The Bootstrap CSS link in the preceding code adds the Bootstrap CSS library to your page. This means that certain HTML element types and classes will inherit their styles from Bootstrap. For example, if you add the btn-primary class to the class of a button, the button will be rendered in blue with white text.
