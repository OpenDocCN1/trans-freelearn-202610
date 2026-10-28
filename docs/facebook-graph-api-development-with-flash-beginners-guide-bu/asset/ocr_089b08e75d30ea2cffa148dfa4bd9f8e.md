Publishing a wall post obviously causes a change, so we should use an "unsafe" method—in this case, POST. This is the same method used when submitting a web page form that changes or adds some data, like when ordering a book from Amazon.

> For security reasons, Flash Player 10 and above will allow only the POST method to be used if the user initiates the load. This means that your application cannot post to the Graph automatically (for instance, based on how long the user has been logged in); instead, you must always publish from, say, a button's MOUSE_CLICK event handler function.

# Time for action – using the POST method

+   1. To specify that we want to use the POST method, we change the method property of URLRequest (after all, POST is a request method). Another class, flash.net.URLRequestMethod, contains consts with all the permitted values, so import it in HTTPRequestor.as:
+   ```
+   import flash.net.URLRequestMethod;
+   ```
+   2. We set up the URLLoader, URLRequest, and URLVariables instances in the same way as we do in the request() and search() methods:
+   ```
+   public function publish(a_publishObject:PublishObject):void
+   {
+       var loader:URLLoader = new URLLoader();
+       var urlRequest:URLRequest = new URLRequest();
+       var variables:URLVariables = new URLVariables();
+   }
+   ```
+   3. Because we want to post to our own Wall, we use our profile's feed connection as the URL—there's no separate Post to Wall URL:
+   ```
+   public function publish(a_publishObject:PublishObject):void
+   {
+       var loader:URLLoader = new URLLoader();
+       var urlRequest:URLRequest = new URLRequest();
+       var variables:URLVariables = new URLVariables();
+   
+       urlRequest.url = "https://graph.facebook.com/me/feed";
+   }
+   ```
+   4. Now we use the POST const from the URLRequestMethod class to set the request method:
+   ```
+   public function publish(a_publishObject:PublishObject):void
+   {
+   ```