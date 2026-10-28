Next, move on to other types of Facebook objects: personal profiles, events, groups. For personal profiles, the id may be alphanumeric (if the person has signed up for a custom Facebook Username at http://www.facebook.com/username/), but in general the id will be numeric, and auto-assigned by Facebook when the user signed up.

For certain types of objects (like photo albums), the value of id will not be obvious from the URL within the Facebook website; we'll look at how to find these later in the chapter.

In some cases, you'll get an error message, like:

```
{
  "error": {
    "type": "OAuthAccessTokenException",
    "message": "An access token is required to request this resource."
  }
}
```

Again, we'll look at what this means and how to get around it later in the book.

# Accessing the Graph API through AS3

Now that you've got an idea of how easy it is to access and read Facebook data in a browser, we'll see how to fetch it in AS3.

# Time for action – retrieving a Page's information in AS3

Set up the project from the Chapter 2 start files, as explained in Chapter 1. Check that the project compiles with no errors (there may be a few warnings, depending on your IDE). You should see a 640 x 480 px SWF, all white, with just three buttons in the top-left corner: Zoom In, Zoom Out, and Reset View:

![](bbox=[0.175, 0.64, 0.825, 0.75])

This project is the basis for a Rich Internet Application (RIA) that will be able to explore all of the information on Facebook using the Graph API. All the code for the UI is in place, just waiting for some Graph data to render. Our job is to write code to retrieve the data and pass it on to the renderers.