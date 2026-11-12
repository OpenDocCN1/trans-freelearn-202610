The `Request->isXmlHttpRequest` method returns `true` if the request was made with XMLHttpRequest. This object makes asynchronous requests. As XMLHttpRequest is handled by JavaScript on the client side, and XML was used a lot to exchange data between the client and server sides, the use of XMLHttpRequest received a special name: **Asynchronous JavaScript and XML** (AJAX). Although XML use is increasingly rare nowadays because of the lightness of JSON, the acronym AJAX remains in use.

XMLHttpRequest specification
XMLHttpRequest is an open web standard documented at https://xhr.spec.whatwg.org.

If you click on the **Asynchronous request** button, you will see an alert window, as shown in Figure 13.12. It shows that the page has not been reloaded:

![](bbox=[0.33, 0.38, 0.63, 0.47])

Figure 13.12 – Response to asynchronous requests

If you click on the **Synchronous request** button, you will be taken to the following page:

![](bbox=[0.14, 0.55, 0.82, 0.75])

Figure 13.13 – Response to synchronous requests

You can see that the page has been reloaded. Observe that the same controller action can handle synchronous and asynchronous requests, generating different responses as per the situation.

In the next subsection, we will see a way to exchange messages between a controller and a view script without using the ViewModel class.