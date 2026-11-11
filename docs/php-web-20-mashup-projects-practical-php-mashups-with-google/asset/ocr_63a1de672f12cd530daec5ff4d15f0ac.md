![](bbox=[0.2, 0.17, 0.26, 0.21])

> **Be careful with eval ()**
> Be careful when you use eval(). It blindly executes any code passed to it, so make sure you fully trust the source of the input.

Finally, we get to our APIs. We only have two we need to look at—the Google Maps API and Flickr Web Services.

# Google Maps API

The Google Maps API allows third party developers to use the features of Google Maps on their own sites. Anything you can do as a user of Google Maps can be done using the Google Maps API. The Google Maps documentation home page is located at http://www.google.com/apis/maps/documentation/. The documentation is quite extensive. We will take a look at how the API basically works, and concentrate on the features we will use in our mashup. Just knowing how the API is organized is the key step in searching for information and using the Google Maps API in future projects.

The Google Maps API requires an API key. You can register for it for free at http://www.google.com/apis/maps/signup.html. This key is used when including the Google Maps API in your page. Before you do anything with Google Maps, you will need to get this API key and put this source tag and in top of your page's head tag.

```
<script src="http://maps.google.com/maps?file=api&amp;v=2&amp;key=Your Google API Key" type="text/javascript"></script>
```

The API is a JavaScript API based heavily on objects. The central object is the Google Map that you see. Everything that you see on Google Maps including map controls, icons, lines, and the white information window box, are just JavaScript objects added to the map. As we go through the examples in this section, we will build the same page that is in the examples named googleMapTest.php.

# Creating a Map

The Map is created by instantiating the GMap2 class. The only required parameter in the GMap2's constructor is an HTML container to place the map. Typically, this is an empty div tag. The Google Map will be displayed in the space occupied by this tag. This places a lot of importance on this container. You can use CSS to position the map on the page, and the size of the container determines the size of the map.