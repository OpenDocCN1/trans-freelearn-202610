
![](img/48815d6f06dc8c7aeaa3963d289fb1e7_155_0.png)

Figure 6.2 – A simple REST call

This REST call gives you information about a project hosted on GitHub. From the URL, you can guess that it contains information about a repo (project), and that project is the Office JavaScript example from the Microsoft team. Looking at the information returned, at the very bottom, we can see the “subscribers count” – the number of people following the project.

While the REST format is mostly used by machines, it’s not unfriendly to humans. We have already learned the following:

- REST calls are made over the web using standard transport (the HTTP that you’re familiar with). In particular, this is a GET request, where all the information we want to pass in is contained in the URL.
- REST URLs are structured to give a clue about the information that will be returned. From the JSON returned, we get pointers to other URLs (or endpoints) that might be of interest.
- The call returns JSON, a format we first met in Chapter 4, which allows us to guess the meaning of a lot of information. In fact, it returns too much information, but we can ignore everything except the field that we were looking for (Subscriber count).

- If we refresh the page, we get almost the same result (unless somebody else has followed the project in the meantime). This gives us a clue that the REST call is *stateless* – the server takes our request, deals with it, then forgets about us. Our next request will be treated as a fresh new inquiry using the information we pass in at that time.

Our simple sample uses the browser to make web requests. As you might guess from our screenshot, REST doesn’t really care who or what is calling it, as long as it is a valid request. In this case, *valid* means that the client making the call knows how to “speak” HTTP, that the client is calling the right URL, and that it passes in the information expected to that URL.

## More on REST – GET and POST requests

In our previous example, we talked to the server using a GET request – all the information was in the URL. GET is the best option for simple requests – for example, Google uses it for its search queries where you can copy and paste a URL such as `https://www.google.com/search?q=packt+books` into different browsers and get the same response every time.

Our requests are likely to pass in more complex information, which is why will use the POST option. You’re likely to have used this option without realizing it, for example, when filling out a form on a web page and then hitting submit. If the web page URL doesn’t change or show your data, then the way the information gets transmitted behind the scenes is via a POST call. While they are easy to use (as we’ll see shortly), in POST calls, information is more hidden but more powerful.

You’ve probably guessed that there is a lot more to REST than the just-what-we-need-to-know introduction we’ve covered here. A good source of information if you want to read more about it is `https://restfulapi.net/`.

Our focus on learning REST is to communicate with our Kogito decision server. Let’s look at what that looks like in more detail, starting with Swagger – a tool that makes REST easier.

## Swagger – a more human-friendly link to Kogito

While REST is not unfriendly, Kogito helpfully uses a tool called Swagger to make REST pages even easier to understand.

We’ll pick up where we left off in *Chapter 5* where we deployed our decision model to the OpenShift cloud (dev sandbox) and shared the web page for our colleagues to admire. It’s worth going back to that web page now – remember that it looks similar to *Figure 6.3*.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_157_0.png)

Figure 6.3 – A reminder of what our decision model looks like in the cloud

## Remember – you may need to restart/redeploy to OpenShift

As a friendly reminder, the OpenShift dev sandbox times out after 24 hours – you may need to go through the instructions in Chapter 5 again, to link the KIE sandbox to OpenShift. You may be getting tired of this renew-every-24 hours limitation. We’ll fix that in Chapter 10 when we look at more places we can deploy our decision models.

Getting to the machine-readable version of this page with the Swagger API is pretty simple – click on the dots at the top right of the page (in Figure 6.3), then click on the Swagger UI that appears. You’ll see the Swagger page giving a summary of the REST interface that Kogito has generated on our behalf, as in Figure 6.4.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_158_0.png)

Figure 6.4 – The Swagger summary of the REST API generated for our decision service

Before we look at this screen in detail, think of it like taking the fire escape in your hotel or apartment block. You’re still in the same building, you’re still using the stairs, but you get to see a lot more of the behind-the-scenes details than you’re used to, for example, exposed piping, cables not hidden behind panels, bare concrete, and brick walls. Similarly, here we are seeing a lot of technical details – so we’re going to focus on the key points we need to link Excel to our decision service.

We know from the final model we created in Chapter 4 that Product Recommendation Service is the one we want to use – we wrapped our model in this decision model node specifically to highlight that it’s the one intended for us to call externally. Before we explore this service further, let’s take a quick look at the other endpoints being exposed:

- GET /{modelname}: Allows us to download the original decision model
- POST / Upload Upload a new model – only really used by the KIE tooling
- GET / Upload/status: Allows us to view any errors from uploading the model
- POST - {modelname} and the similar {modelname}/dmnresult: Allow us to view the results of executing all decision nodes in the full model

There is a second version of Product Recommendation Service with a /dmnResult tag. This is the same as the main service, but adds information on Info, Warn, and Error messages returned by the KIE when executing the decision model.

Designing a good REST API like this is an art – more information on the body generated by the decision service, as in *Figure 6.6*.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_160_0.png)

Figure 6.6 – Swagger showing our successful REST call

Congratulations! You’ve just called your first REST API! Not surprisingly, the results are the same as the web page. We’ll be spending some time connecting Excel to this REST API – so play around with it, passing in different customer numbers, countries, dates, and so on, and see the returned values change.

Before we prove that we can use multiple clients to call the decision service, we’ll take a look at some vital information. This will also help our understanding of what we need to do to make a successful REST call in the samples in this and subsequent chapters.

## Some vital information – service endpoint and contents

Just below the text in *Figure 6.6*, Swagger also helpfully lists the command you’ve just executed in a format that you can copy and paste into a command prompt – such as the following listing:

```
```bash
curl -X 'POST' \
  'https://dmn-dev-sandbox-yu88rl6qu490-crt-openshift-dev.apps.sandbox.x8i5.pl.openshiftapps.com/CustomerRecommendations/Product Recommendation Service' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "Customer": {
    "Number": 0,
    "Name": "string",
```
