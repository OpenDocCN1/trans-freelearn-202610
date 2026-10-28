In the next section, we will do some template styling using Bootstrap.

# Template styling with Bootstrap

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