```
url = "http://www.packtpub.com/books"
page = urllib2.urlopen(url)
soup_packtpage = BeautifulSoup(page)
```

In the previous Python script, we have used the urllib2 module, which is a native Python module, to open the http://www.packtpub.com/books page. The urllib2.urlopen() method returns a file-like object for the input URL. Then we create the BeautifulSoup object, soup_packtpage, by passing the file-like object.

> Creating a BeautifulSoup object using a URL file-like object is the efficient way to deal with online web pages.

We learned how to create a BeautifulSoup object by passing a file-like object for a URL in the previous example. Similarly, we can pass the file object for a local file to the BeautifulSoup constructor.

For this, create a local folder in your machine by executing the command mkdir Soup from a terminal. Create an HTML file, foo.html, in this folder using touch Soup/foo.html. From the same terminal, change to the directory just created using cd Soup.

Now let us see the creation of BeautifulSoup using the file foo.html.

```
with open("foo.html","r") as foo_file:
    soup_foo = BeautifulSoup(foo_file)
```

The previous lines of code create a BeautifulSoup object based on the contents of the local file, foo.html.

BeautifulSoup has a basic warning mechanism to notify whether we have passed a filename instead of the file object.

Let us look at the next code line:

```
soup_foo = BeautifulSoup("foo.html")
```

This will produce the following warning:

UserWarning: "foo.html" looks like a filename, not markup. You should probably open this file and pass the filehandle into Beautiful Soup.

But still a BeautifulSoup object is created assuming the string ("foo.html") that we passed as HTML.