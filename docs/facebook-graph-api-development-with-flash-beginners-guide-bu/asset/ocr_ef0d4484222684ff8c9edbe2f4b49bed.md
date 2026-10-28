# Time for action – authorizing through AIR with HTTP

Rather than placing our application inside an HTML page, we must place an HTML page inside our application. To do this, we can use an `HTMLLoader`.

The `flash.html.HTMLLoader` class, only accessible in AIR, is a type of `DisplayObject` that acts as a container for HTML content. We use it to load the Facebook login page for our application, much like we did with a pop-up window in Chapter 3. Back in that chapter, once the user had logged in, the pop-up window redirected to another page, passing the access token as part of the hash, and JavaScript in this page's source extracted the hash and passed it to the SWF. In AIR, our application will use an event listener to detect this redirection, and extract the hash directly.

Here's how that breaks down. These are general instructions, rather than being tied to the Visualizer, so implement them however best fits your project:

+   1. First, import HTMLLoader.
+   ```
+   import flash.html.HTMLLoader;
+   ```
+   2. Next, create an instance of this class, and add an event listener for when its contents are redirected.
+   ```
+   var htmlLoader:HTMLLoader = new HTMLLoader();
+   htmlLoader.addEventListener(Event.LOCATION_CHANGE, onRedirect);
+   ```
+   (Don't forget to `import flash.events.Event.`)
+   3. The HTMLLoader will have a width and height of zero, by default, which is not useful since we need the user to click a button inside it! So, resize it.
+   ```
+   var htmlLoader:HTMLLoader = new HTMLLoader();
+   htmlLoader.addEventListener(Event.LOCATION_CHANGE, onRedirect);
+   htmlLoader.width = 640;
+   htmlLoader.height = 480;
+   ```
+   4. Add it to the display list so that it can be seen.
+   ```
+   var htmlLoader:HTMLLoader = new HTMLLoader();
+   htmlLoader.addEventListener(Event.LOCATION_CHANGE, onRedirect);
+   htmlLoader.width = 640;
+   htmlLoader.height = 480;
+   addChild(htmlLoader);
+   ```