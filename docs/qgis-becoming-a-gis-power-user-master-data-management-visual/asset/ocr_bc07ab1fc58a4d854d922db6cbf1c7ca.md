# Opening files using actions

To open files directly from within QGIS, we use the Open actions. If you added the default actions in the previous exercise, your layer will already have an Open file action. The action is as simple as [% "PATH" %] for opening the file path specified in the layer's path attribute. Since none of our sample datasets contain a path attribute, we'll add one now to test this feature. Check out Chapter 3, Data Creation and Editing, if you need to know the details of how to add a new attribute. For example, the paths added in the following screenshot will open the default image viewer and PDF viewer application, respectively:

![](bbox=[0.184, 0.313, 0.816, 0.511])

While the previous example uses absolute paths stored in the attributes, you can also use relative paths by changing the action code so that it completes the partial path stored in the attribute value; for example, you can use C:\temp\[% "TYPE" %].png to open .png files that are named according to the TYPE attribute values.

# Opening a web browser using actions

Another type of useful Open action is opening the web browser and accessing certain websites. For example, consider this action:

```
http://www.google.com/search?q=[% "TYPE" %]
```

It will open your default web browser and search for the TYPE value using Google, and this action:

```
https://en.wikipedia.org/w/index.php?search=[% "TYPE" %]
```

will search on Wikipedia.