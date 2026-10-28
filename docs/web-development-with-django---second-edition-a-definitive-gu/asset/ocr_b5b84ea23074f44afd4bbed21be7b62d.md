Note that we could access the method directly inside the template without passing it in a context dictionary, by using the request.method variable. We know from Chapter 3, URL Mapping, Views, and Templates, that by using the render shortcut function, the request is always available in the template. We just demonstrated how to access the method in the view here because later on, we will change the behavior of the page based on the method.

+   4. We also need to add the CSRF token to the form HTML. We do this by putting the {% csrf_token %} template tag after the opening <form> tag. The start of the form should look like this:

```
<form method="post">
    {% csrf_token %}
```

Now, save the file.

+   5. Start the Django Dev Server if it's not already running. Load the example page (http://127.0.0.1:8000/form-example/) in your browser; you should see it now displays the method at the top of the page (GET):

![](bbox=[0.12, 0.42, 0.85, 0.62])

Figure 6.10: Method at the top of the page

+   6. Enter some text or data in each of the inputs and submit the form by clicking the Submit Input button: