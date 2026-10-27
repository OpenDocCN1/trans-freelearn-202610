The alert boxes should look similar to the following screenshot in your browser:

![](bbox=[0.23, 0.17, 0.76, 0.46])

A dismissible alert is an alert that can be closed by the user by clicking on a small X icon in its top right-hand corner. In order to create a dismissible alert, you can use the alert-dismissible class name as follows:

```
<div class="alert alert-warning alert-Dismissible" role="alert">
<button type="button" class="close" data-dismiss="alert"><span aria-hidden="true">&times;</span><span class="sr-only">Close</span></button>
<strong>Alert!</strong> This is a dismissible alert.
</div>
```

> You can refer to this book's accompanying sample project for an indication of how to display specifically styled alerts directly from your ASP.NET MVC controllers. These alerts are adapted from Matt Honeycutt's example on GitHub: https://github.com/MattHoneycutt/Fail-Tracker.