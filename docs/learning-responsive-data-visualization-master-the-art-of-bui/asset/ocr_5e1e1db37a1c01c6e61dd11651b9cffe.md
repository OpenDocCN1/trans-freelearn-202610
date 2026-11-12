Let's take a look at some code. First, we need to include the Bootstrap CSS to the header of the web page:

```
<!DOCTYPE html>
<html>
<head>
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/
bootstrap/3.3.5/css/bootstrap.min.css">
<title>Simple Grid System</title>
...
</head>
<body>
...
</body>
</html>
```

Now, we can add the grid layout according to the previous example, but this time, we need to wrap it in a fluid container:

```
<div class="container-fluid">
<div class="row">
<div class="col-lg-1">.col-lg-1</div>
<div class="col-lg-1">.col-lg-1</div>
...
</div>
<div class="row">
...
</div>
</div>
```

If we open the web page and look at the output of the web browser, we see an example similar to the following figure:

![](bbox=[0.1, 0.65, 0.92, 0.78])

Browser output of grid layout with Bootstrap