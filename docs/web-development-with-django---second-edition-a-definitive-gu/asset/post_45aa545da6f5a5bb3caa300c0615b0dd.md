
If you submit a form in this way, the form will act as though you had clicked the first submit button on the form, so the `submit_input` input value will be included. The output you see should match that of the previous figure.

The other way to submit the form is by clicking the `button_element` submit input. We will try clicking this button to submit the form. You should see that `submit_button` is no longer in the list of posted values, while `button_element` is now present:

```
csrfmiddlewaretoken: ['HmMPTL0LXLRT\ntext_input: ['Text']\npassword_input: ['']\ncheckbox_on: ['Checkbox Checked']\nradio_input: ['Value Two']\nfavorite_book: ['1']\nbooks_you_own: ['1', '3']\ntext_area: ['Text Area']\nnumber_input: ['154']\nemail_input: ['email@example.com']\ndate_input: ['2022-03-22']\nbutton_element: ['Button Element']\nhidden_input: ['Hidden Value']
```

Figure 6.15: submit_button is now gone from the inputs, and button_element has been added

You can use this multiple-submit technique to alter how your view behaves, depending on which button was clicked. You can even have multiple submit buttons with the same *name* attribute to make the logic easier to write.

In this exercise, you added a CSRF token to your form element by using the `{% csrf_token %}` template tag. This means that your form can be submitted to Django successfully without it generating an **HTTP Permission Denied** response. We then added some code to output the values that our form contained when it was submitted. We tried submitting the form with various values to see how they are parsed into Python variables on the `request.POST` part of `QueryDict`. We will now discuss some more of the theory around the difference between GET and POST requests, then move on to the Django Forms library, which makes designing and validating forms easier.

## Choosing between GET or POST

Choosing when to use a GET or POST request requires you to consider several factors. The most important is deciding whether the request should be idempotent. A request can be said to be idempotent if it can be repeated and produce the same result each time. Let’s look at some examples.

If you type in any web address into your browser (such as any of the Bookr pages we have built so far), it will do a GET request to fetch the information. You can refresh the page, and no matter how many times you click to refresh, you will get the same data back. The request you make does not affect the content on the server. We call these requests idempotent.

Now, remember when you added data through the Django admin interface (in Chapter 4). You typed the information for the new book into a form, then clicked Save. Your browser made a POST request to create a new book on the server. If you repeated that POST request, the server would create another Book and would do so each time you repeated the request. Since the request is updating information, it is not idempotent. Your browser will warn you about this. If you’ve ever tried to refresh a page that you were sent to after submitting a form, you may have received a message asking if you want to "Repost form data?" (or something more verbose, as shown in the following figure). This is a warning that you’re sending the form data again, which might cause the action you just undertook to be repeated:

![](img/4840a43efd335abd10b93708225692d6_303_0.png)

Figure 6.16: Firefox confirming if information should be resent

This is not to suggest that all GET requests are idempotent and all POST requests are not – your backend application can be designed in any way you want. Although it is not best practice, a developer might have decided to make data get updated during a GET request in their web application. When you are building your applications, you should try to make sure GET requests are idempotent and leave data-altering to POST requests only. Stick to these principles unless you have a good reason not to.

Another point to consider is that Django only applies CSRF projection to POST requests. Any GET request, including one that alters data, can be accessed without a CSRF token.

Sometimes, it can be hard to decide if a request is idempotent or not – for example, a login form. Before you submitted your username and password, you were not logged in, and afterward, the server considered you to be logged in, so could we consider that non-idempotent as it changed your authentication status with the server? On the other hand, once logged in, if you were able to send your credentials again, you would remain logged in. This implies that the request is idempotent and repeatable. So, should the request be a GET or a POST? This brings us to the second point to consider when choosing what method to use: form data visibility.

If sending form data with a GET request, the form parameters will be visible in the URL. For example, if we made a login form using a GET request, the login URL might be https://www.example.com/login?username=user&password=password1. The username, and worse, the password, are visible in the web browser's address bar. It would also be stored in the browser history, so anyone who used the browser after the real user could log in to the site. The URL is often stored in web server log files as well, meaning the credentials would be visible there too. In short, regardless of the idempotency of a request, don't pass sensitive data through URL parameters.

Sometimes, knowing that the parameter will be visible in the URL might be something you desire. For example, when searching with a search engine, usually, the search parameter will be visible in the URL. To see this in action, try visiting https://www.google.com and searching for something. You'll notice that the page with the results has your search term as the q parameter in the URL. For example, a search for “Django” will take you to the URL https://www.google.com/search?q=Django. This allows you to share search results with someone else by sending them this URL. In Activity 1, you will add a search form that passes a parameter similarly.

Another consideration is that the maximum length of a URL allowed by a browser can be short compared to the size of a POST body – sometimes, only around 2,000 characters (or about 2 KB) compared to many megabytes or gigabytes that a POST body can be (assuming your server is set up to allow these size requests).

As we mentioned earlier, URL parameters are available in request.GET, regardless of the type of request being made (GET, POST, PUT, and so on). You might find it useful to send some data in URL parameters and others in the request body (available in request.POST). For example, you could specify a format argument in the URL that sets what format some output data will be transformed to, but the input data is provided in the POST body.

Setting parameters in the GET query string might seem redundant when values can be passed as part of the URL path and then parsed by Django to route the URL. Next, we'll look at some reasons why you might prefer using the query string.

## Why use GET when we can put parameters in the URL mappings?

Django allows us to easily define URL maps that contain variables. We could, for example, set up a URL mapping for a search view like this:

```
path("/search/<str:search>/", reviews.views.search)
```

This probably looks like a good approach at first, but when we start wanting to customize the results view with arguments, it can get complicated quickly. For example, we might want to be able to move from one results page to the next, so we'll add a page argument:

```
path("/search/<str:search>/<int:page>/", reviews.views.search)
```

And then, we might also want to order the search results by a specific category, such as author name or date of publishing, so we’ll add another argument for that:

```
path("/search/<str:search>/<int:page>/<str:order >", reviews.views.search)
```

You might be able to see the problem with this approach – we can’t order the results without providing a page. If we wanted to add a results_per_page argument too, we wouldn’t be able to use that without setting a page and order key.

Contrast this to using query parameters – all of them are optional, so you could search like this:

```
?search=search+term:
```

Or you could set a page like this:

```
?search=search+term&page=2
```

Or just set the results ordering like this:

```
?search=search+term&order=author
```

Or you could combine them all:

```
?search=search+term&page=2&order=author
```

Another reason to use URL query parameters is that when submitting a form, the browser always sends the input values in this manner – it cannot be changed so that parameters are submitted as path components in the URL. Therefore, when submitting a form using GET, the URL query parameters must be used as the input data.

Now that we’ve introduced the fundamentals of forms in HTTP requests and HTML, let’s look at the Django Forms library, which makes dealing with forms and requests much easier.

## The Django Forms library

We’ve looked at how to manually write forms in HTML and how to access the data on the request object using QueryDict. We saw that the browser provides some validation for us for certain field types (such as email or numbers), but we have not tried validating the data in the Python view. We should validate the form in the Python view for two reasons:

- It is not safe to rely solely on browser-based validation of input data. A browser may not implement certain validation features, meaning the user could post any type of data. For example, older browsers don’t validate number fields, so a user can type in a number outside the range we are expecting. Further, a malicious user could try to send harmful data without using a browser at all. The browser validation should be considered a nicety for the user and that’s all.
