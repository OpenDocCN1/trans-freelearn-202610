The Net panel allows us to see the URL that is being requested, the GET or POST parameters, the server's response, the size of the response in KB, and the time it took to complete the request. You may also have to enable the Net panel—it can be enabled in a similar way we enabled the Console panel, by clicking on the arrow next to the Net text on the top tab. Let's take a look at what the Net panel looks like for the example from Chapter 1:

![](bbox=[0.18, 0.238, 0.82, 0.408])

Before we talk about the requests being made, take a look at the toolbar above the lists of requests—the one that contains the links Clear, Persist, All, HTML, etc. Clicking on Clear will do what its name implies—clears out the list of requests. Clicking on Persist will cause the list of requests to persist, or not get deleted, on page reloads.

The next grouping of links allows us to filter the requests by type. Because we are getting back only images from the map server in this case, all the requests you see would belong to the Images option. If the CSS option was clicked, we would not see those requests, as they are not CSS files.

Now, let's break down the actual request list.

# Request list

The request list shows us all the requests the page makes. Each URL in the previous screenshot is a URL that OpenLayers is making a request to. By clicking on the + (plus) sign next to each request, we can get more information about the request, including the full request URL and the response. When we click on the plus sign, we get a box with Params, Headers, Response, and Cache tabs. The Params tab lists all the parameters, or key/value pairs the URL contains. The Response tab provides us with the server's response to our request. For the purpose of this book, we do not need to worry about the other two tabs.