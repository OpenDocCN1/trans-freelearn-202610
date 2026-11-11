```
{«id»:»411598583», «owner»:»75859527@N00»,
«secret»:»657eb806c8», «server»:»172», «farm»:1, «title»:»Hocus Pocus», «ispublic»:1, «isfriend»:0, «isfamily»:0},
...
]
}
})
```

Each method's documentation page documents the returned XML format of the call. From there, it is easy to take an educated guess at the JSON equivalent. Generally, element attributes in the XML document are object properties in the JSON document. Nested elements are translated into nested objects. The subject of search results, whether they are things like blog entries, users in a group, or like in this case, photos, are returned as JSON arrays. If you have trouble estimating the exact translation of a method, you can always manually make the request in your browser like we did here.

Note that the JSON results are encapsulated in a call to jsonFlickrApi. By default, the API assumes that you want to pass the JSON results to a JavaScript callback function. If you have a function named jsonFlickrApi in your application, the JavaScript engine will pass the JSON object to that function when it receives the response. The engine will then automatically execute the function. This can be a controller in your JavaScript for the service's return value. However, you do need to create a function named jsonFlickrApi, and it must be set-up to act on the returned JSON code. If you choose not to use this, you can turn this automatic callback off by sending a true (1) value to the nojsoncallback parameter in your call. This will give the exact same text string without the jsonFlickrApi().

```
http://api.flickr.com/services/rest/?method=flickr.photos.search&api_key=YOUR_FLICKR_API_KEY&text=fender%20stratocaster&format=json&nojsoncallback=1
```

# Retrieving a Photo or a Photo's Page

Now that we have the results, we can use the data to retrieve photos from Flickr. Image URLs in Flickr have the following format:

```
http://farm{FARM-ID}.static.flickr.com/{SERVER-ID}/{ID}_{SECRET}{SIZE}.jpg
```

With the exception of the size, all the other variables can be extracted directly from flickr.photos.search's web service call response.