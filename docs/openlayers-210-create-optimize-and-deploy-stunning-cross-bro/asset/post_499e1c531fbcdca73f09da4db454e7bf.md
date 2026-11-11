
By double clicking on pretty much anything in the HTML or CSS list you can quickly change values and names. Any change you make will immediately show up on the page, which makes it very easy to change style in real time and see how the page is affected without having to edit and save any files. Play around with it a bit—if you mess anything up, you can just reload the page.

> When editing pages with Firebug, any changes you make will disappear when you refresh the page. You are not editing the actual web server's files with Firebug—instead, you are editing a copy that is on your computer that only you can see when you make changes to it. So, if you make any changes and want them to be saved, you'll have to edit your actual source code.

## CSS panel

This panel provides similar functionality as the CSS sidebar in the HTML panel we just talked about. It also provides the option to edit any CSS document associated with the page, not just the style of a selected element. We need not talk much about this panel, and for the purposes of this book we won't spend much time here. But if you are a web designer, this is another powerful panel that can greatly speed up your development time.

## Script panel

The Script panel is very powerful. Not only does it allow you to view all the JavaScript code associated with the page, it is a great real-time code debugger. You can set watch expressions, view the stack, set breakpoints, etc. If those terms are foreign, don't worry, we won't be spending much time with this panel.

However, before we move on to the next panel I want to quickly talk about enabling breaking on errors. With this option enabled, Firebug will stop the web page whenever a JavaScript error is encountered. This makes it very easy to quickly pinpoint where your page is blowing up at. To enable it, simply click on the pause-button icon.

![](img/1b8cd151c9f278555f9cf255b20d244e_55_0.png)

Keep note of when you enable it—I’ve been frustrated more than once when developing because I forgot that it had been enabled. When it is enabled, the `Script` text will glow yellow, as demonstrated in the above screenshot. You can also enable/disable it through the Console panel.

Unfortunately (depending on your viewpoint), we can’t go much more in depth with these tools, as it is outside the scope of this book. But please feel free to play around with them, as Firebug’s powerful debugger is a great resource. More information on it can be found at http://getfirebug.com/errors.

## DOM panel

The next panel is the Document Object Model panel, or **DOM** panel. The DOM is, basically, a representation of HTML elements as objects. The DOM panel automatically sets up a tree structure to represent our HTML page, allowing us to view everything our HTML page contains. We can also see JavaScript variables and their values, as well as functions, objects, and more. When using the DOM panel, attributes and properties are colored black, and functions and methods are colored green.

We won’t go much more in depth here, but the DOM panel is a very valuable tool, especially when you want to take a peek at JavaScript components. By using the DOM panel (and assuming we are looking at a page that includes OpenLayers), we can quickly see all of OpenLayers’ classes, functions, etc. It is not a replacement for the API docs, but serves as a good, quick way to view such information.

## Net panel

Firebug’s **Net** panel is a tool we will often use throughout this book. Basically, it provides a way for us to monitor network activity by viewing all the requests and responses the web page is making. We can see any Asynchronous JavaScript (**AJAX**) request the page makes. Without AJAX, we would have to refresh our entire page anytime we wanted to do anything with our OpenLayers map. So, for example, every time you zoom in, OpenLayers makes a series of **requests** to the map server to get new map images, and the map server’s **response** is a new map image that OpenLayers then displays. This request/response method is handled via AJAX—without it, we would have to refresh the entire page after every request.

![](img/1b8cd151c9f278555f9cf255b20d244e_56_0.png)

> AJAX is a method by which, through JavaScript, you can send a request to a server and receive a response without actually refreshing the page. Traditionally, if you want your web page to get an update from the server, you would need to have the page itself send a request.

The Net panel allows us to see the URL that is being requested, the GET or POST parameters, the server's response, the size of the response in KB, and the time it took to complete the request. You may also have to enable the Net panel—it can be enabled in a similar way we enabled the Console panel, by clicking on the arrow next to the Net text on the top tab. Let's take a look at what the Net panel looks like for the example from Chapter 1:

![](img/1b8cd151c9f278555f9cf255b20d244e_57_0.png)

Before we talk about the requests being made, take a look at the toolbar above the lists of requests—the one that contains the links Clear, Persist, All, HTML, etc. Clicking on Clear will do what its name implies—clears out the list of requests. Clicking on Persist will cause the list of requests to persist, or not get deleted, on page reloads.

The next grouping of links allows us to filter the requests by type. Because we are getting back only images from the map server in this case, all the requests you see would belong to the Images option. If the CSS option was clicked, we would not see those requests, as they are not CSS files.

Now, let's break down the actual request list.

## Request list

The request list shows us all the requests the page makes. Each URL in the previous screenshot is a URL that OpenLayers is making a request to. By clicking on the + (plus) sign next to each request, we can get more information about the request, including the full request URL and the response. When we click on the plus sign, we get a box with Params, Headers, Response, and Cache tabs. The Params tab lists all the parameters, or key/value pairs the URL contains. The Response tab provides us with the server's response to our request. For the purpose of this book, we do not need to worry about the other two tabs.

## Parameters

Take a look again at the list—before we mouse over the text in the screenshot, the titles contain GET vmap0?LAYERS. The GET specifies that the request type is GET, which basically means we are embedding variables inside the URL itself with key=value pairs, separated by a & sign.

When we mouse over a URL, we can see more of it—as in the previous screenshot, we see a bunch of variables in the format key=value&key=value& . . . These values correspond to the values listed in the Params tab when you expand the URL.

If you take a look at the link we have our mouse over, you'll notice that the URL is the same as the URL from the example in Chapter 1, http://labs.metacarta.com/wms/vmap0. However, there is an additional text after vmap0; a question mark followed by key=value pairs. The question mark signifies the start of the key=value pairs in a GET request.

Let's take a look at some of the variables the URL contains.

```
?LAYERS=basic&SERVICE=wms . . .
```

The LAYERS key should look familiar, as we specified in Chapter 1. In that example, we used the code:

```
{layers: 'basic'},
```

When we defined the key:value pair, we were essentially telling OpenLayers what to pass into the actual URL it generates. From the generated URL, we can see that it did indeed pass over the right layer name. Now, as we discussed earlier, this layer name, basic, is the layer we want from the WMS service. The layer name we pass in here affects what image the WMS server responds with. In this case, we'll get back an image with the layer named basic.

Notice that there are many more parameters (separated by & signs) than what we passed in. OpenLayers automatically adds these parameters for you, so we get back the right image of a map without us having to manually build each URL.

To see the entire key:value pairs, or parameters, expand the URL and look at the Params tab.

![](img/1b8cd151c9f278555f9cf255b20d244e_58_0.png)

> Note that these GET variables, such as LAYERS and SERVICE are specific to WMS. Other layer types will use different values.
