+   - If we refresh the page, we get almost the same result (unless somebody else has followed the project in the meantime). This gives us a clue that the REST call is *stateless* – the server takes our request, deals with it, then forgets about us. Our next request will be treated as a fresh new inquiry using the information we pass in at that time.

Our simple sample uses the browser to make web requests. As you might guess from our screenshot, REST doesn’t really care who or what is calling it, as long as it is a valid request. In this case, *valid* means that the client making the call knows how to “speak” HTTP, that the client is calling the right URL, and that it passes in the information expected to that URL.

# More on REST – GET and POST requests

In our previous example, we talked to the server using a GET request – all the information was in the URL. GET is the best option for simple requests – for example, Google uses it for its search queries where you can copy and paste a URL such as `https://www.google.com/search?q=packt+books` into different browsers and get the same response every time.

Our requests are likely to pass in more complex information, which is why will use the POST option. You’re likely to have used this option without realizing it, for example, when filling out a form on a web page and then hitting submit. If the web page URL doesn’t change or show your data, then the way the information gets transmitted behind the scenes is via a POST call. While they are easy to use (as we’ll see shortly), in POST calls, information is more hidden but more powerful.

You’ve probably guessed that there is a lot more to REST than the just-what-we-need-to-know introduction we’ve covered here. A good source of information if you want to read more about it is `https://restfulapi.net/`.

Our focus on learning REST is to communicate with our Kogito decision server. Let’s look at what that looks like in more detail, starting with Swagger – a tool that makes REST easier.

# Swagger – a more human-friendly link to Kogito

While REST is not unfriendly, Kogito helpfully uses a tool called Swagger to make REST pages even easier to understand.

We’ll pick up where we left off in *Chapter 5* where we deployed our decision model to the OpenShift cloud (dev sandbox) and shared the web page for our colleagues to admire. It’s worth going back to that web page now – remember that it looks similar to *Figure 6.3*.