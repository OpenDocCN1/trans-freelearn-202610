+   3. Add a URL mapping to your new view to `urls.py`. The URL can be something such as `/book-search`.

After completing this activity, you should be able to pass in a search value through the URL's parameters and see it rendered on the resulting page. It should look like this:

![](bbox=[0.21, 0.2, 0.79, 0.36])

Figure 1.53 – Searching Django workshop

You should also be able to pass in special HTML characters such as < and > to see how Django automatically escapes them in the template.

![](bbox=[0.21, 0.46, 0.79, 0.585])

Figure 1.54 – Note how HTML characters are escaped so that we are protected from tag injection

Note
The solution for this activity can be found at https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/ActivitySolutions.