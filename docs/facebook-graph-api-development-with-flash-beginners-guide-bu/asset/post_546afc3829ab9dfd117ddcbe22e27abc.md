
## Let Me In!

To see how much information your friends can access about you through applications, log in to the Facebook website, click on Account | Privacy Settings, select Edit your settings for using applications, games and websites, and view the settings for Information accessible through your friends:

![](img/4e826fa36038b86f2087ef8b818d0acf_102_0.png)

## That's cheating!

Okay, fine, generating an access token in the browser and then pasting it directly into the source code is not a great way of handling authentication. We need to obtain an access token in our application itself.

## Time for action – authenticating through the application

When authenticating in the browser, we visit `https://graph.facebook.com/oauth/authorize?client_id=<your_application_id>&redirect_uri=http://google.com&type=user_agent`, so let's start by loading that URL in our code.

Start by creating a new function, attemptToAuthenticate(), in HTTPRequestor.as:

```
public function attemptToAuthenticate():void
{
    var loader:URLLoader = new URLLoader();
    var urlRequest:URLRequest = new URLRequest();

    var variables:URLVariables = new URLVariables();

    urlRequest.url = "https://graph.facebook.com/oauth/authorize";
    variables.client_id = "<your_application_id>";
    variables.type = "user_agent";
    variables.redirect_uri = "http://google.com";
    urlRequest.data = variables;

    loader.addEventListener(Event.COMPLETE, onAuthenticationComplete);
    loader.addEventListener(IOErrorEvent.IO_ERROR, onIOError);
    loader.load(urlRequest);
}
```

Make sure that you replace <your_application_id> with your actual application ID.

We're using the same IO_ERROR handler function as when we load a Graph object, but we need a new function, onAuthenticationComplete(), for when the loading has finished; add it to HTTPRequestor.as:

```
private function onAuthenticationComplete(a_event:Event):void
{
    trace("Authentication complete");
    var loader:URLLoader = URLLoader(a_event.target);
    trace(loader.data);
}
```

Nothing complicated there; it will trace whatever gets sent back.

In CustomGraphContainerController.as, let's change the constructor function so that it kicks this authentication off instead of loading a user profile:

```
public function CustomGraphContainerController(a_graphControlContainer:GraphControlContainer)
{
    super(a_graphControlContainer);
    _requestor = new HTTPRequestor();
    addEventListenersToRequestor();
    _requestor.attemptToAuthenticate();
    //_requestor.request(new GraphRequest("<your_user_id>"));
}
```

Before this can work, we need to add the function to the IRequestor interface. Modify \src\graph\apis\base\IRequestor.as to:

```
package graph.apis.base
{
    import flash.events.IEventDispatcher;
    import graph.GraphRequest;

    public interface IRequestor extends IEventDispatcher
    {
        function request(a_request:GraphRequest):void;
        function attemptToAuthenticate():void;
    }
}
```

Test it. What gets traced? A lot of HTML, which (when pasted into a blank .html file and opened in a browser) looks like this:

![](img/4e826fa36038b86f2087ef8b818d0acf_104_0.png)

> If you get an error, or no data returned at all, load the URL in your browser to see what error message is returned. Make sure that all the properties of your URLVariables object are set correctly.

## What just happened?

Oh, right, we need to log in first. But here's a problem: since we're loading the URL inside Flash Player, rather than in a browser, we don't have access to the browser cookies, so Facebook can't remember who we are and directs us to the login page. Of course, since we're not in a browser, we can't actually see the page!

We could write code to create a Flash login form, and post the user's e-mail address and password to the real Facebook login form, but why would the user trust our application not to hijack their data?

## A different approach

There are a few ways around this, depending on whether we're using AIR or Flash Player. In this chapter, we'll use the simplest method: embedding the SWF in a web page and using JavaScript to open the login page and pass the access token back to Flash Player.

## Time for action – authenticating via JavaScript

Here's what is going to happen:

- 1. We'll embed our application's SWF in a web page, and load the page in a browser.
- 2. The application will call a JavaScript function inside the web page, telling it to load the "authorize" URL in a new browser window.
- 3. The new window may ask us to log in; once we're authenticated, it'll redirect to the Redirect URI page, just as when we used it earlier.
- 4. This time, however, we'll have changed the Redirect URI page to one that we control (rather than using the Google home page). It will obtain the access token and pass it back to a JavaScript function in the page containing our application's SWF.
- 5. That JavaScript function will pass the access token to an AS3 function in our application, which will then be able to use it everywhere.

For this to work, the web page that Facebook redirects us to (in Step 4, containing the SWF) needs to be online. That means we need to have a web host. See Chapter 1 for details on setting one up.

Publish your project for the web, so that you end up with a web page containing the Visualizer SWF – again, the method for doing this is outlined in Chapter 1 – and upload it to your host. Let's suppose that the web page is called `index.html`, and that you uploaded all the files to `http://host.com/visualizer/`.

Load `http://host.com/visualizer/index.html` in your browser. You'll see, well, nothing much, because the application is still set to try to authenticate rather than loading any data, and it's failing at that. But if you look in the top-left-hand corner, you should see the familiar UI controls: **Zoom In**, **Zoom Out**, and **Reset View**. (If you see "Movie Not Loaded", check that your HTML page is set to load the correct SWF, and that you uploaded all of the files to the web host, rather than just `index.html`.)

To open a new window containing the Facebook login page, we need to run this JavaScript code from inside index.html:

```
window.open('https://graph.facebook.com/oauth/authorize?client_id=<your_application_id>&redirect_uri=http://google.com&type=user_agent', '<window_name>', 'height=370,width=670');
```

You can probably guess what the code does, as JavaScript syntax is very similar to ActionScript; it loads a pop-up window, with the height, width, and URL specified. The `<window_name>` parameter is a value we can use to refer to the window later.

We could put this code inside the HTML of the page. However, rather than risk messing up the SWF embedding code (and to make our SWF a little more flexible), we'll use ExternalInterface to inject it into the page from our SWF instead.

![](img/4e826fa36038b86f2087ef8b818d0acf_106_0.png)

> The flash.external.ExternalInterface class allows ActionScript code within an SWF to communicate with JavaScript code within an HTML page containing that SWF. This means that the SWF can call JavaScript methods, and vice-versa.

Open HTTPRequestor.as and import the ExternalInterface class:

```
import flash.external.ExternalInterface;
```

Now, remove all the code we put in attemptToAuthenticate() earlier, and replace it with an ExternalInterface call:

```
public function attemptToAuthenticate():void
{
    ExternalInterface.call("window.open",
        "https://graph.facebook.com/oauth/authorize?client_id=<your_application_id>&type=user_agent&redirect_uri=http://google.com",
        "facebookLoginWindow",
        "height=370, width=600");
}
```

Compile your SWF (avoid testing it in the standalone Flash Player, as it won't be able to access ExternalInterface outside of a browser) and upload it to your web host, overwriting the previous one. Then, refresh index.html in your browser.
