Probably the most impressive thing about Flickr Services is the choices you have in request and response formats. For request, you can use any of the three most popular formats – REST, SOAP, and XML-RPC. For responses, you can choose Flickr's own XML schema, SOAP, XML-RPC, JSON, or even serialized PHP objects. Regardless of the format you choose for request and responses, Flickr Services has a consistent method of doing things. All requests take the same parameters and return the same data. You just need to format and parse differently for each one.

# Executing a Search

Because Flickr Services is so consistent, the best way to get an overview of it is to walk through an example. In our mashup, we will need to concentrate on the group of photo methods. In particular, we need one to search photos based on user tags. Let's try and execute a search like we will be doing for our mashup.

For our request and response, we'll look to keep things simple. We will send the service request using REST. Our web application is PHP driven, so a serialized PHP response would be intriguing. However, as JavaScript will be doing a lot of the work, we will use JSON. The straight XML response from a REST call would also be acceptable, but it would be nice to avoid the DOM parsing that would be required with it.

The method names are fairly self-explanatory and give us a lot of clues on what the method does. Looking at the documentation for the method flickr.photos.search at http://www.flickr.com/services/api/flickr.photos.search.html, we see it is exactly what we need to search photos.

The URL for all Flickr REST requests is http://api.flickr.com/services/rest/. Following this URL are the parameters of the method in a GET request format. There are two required parameters for all REST requests—method and api_key. The value of method is the name of the method that you wish to call. The value of api_key is your Flickr API Key. To call flickr.photos.search, our complete URL would be:

```
http://api.flickr.com/services/rest/?method=flickr.photos.search&api_key=YOUR_FLICKR_API_KEY
```

A methods documentation page lists all the parameters the method can take. flickr.photos.searchs' available parameters are quite extensive. This gives us a lot of ability to tweak our search. According to the documentation, the only required parameter is api_key. However, this is sort of misleading because we also need to supply a search term. We can search tags using the tags parameter, or a free text search using the text parameter. Even though both are optional parameters, we need to include one or the other. Otherwise, Flickr will return a message saying that empty searches are not supported.