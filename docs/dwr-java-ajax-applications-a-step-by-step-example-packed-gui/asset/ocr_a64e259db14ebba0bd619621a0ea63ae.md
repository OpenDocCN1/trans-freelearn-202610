# User Interface: Basic Elements

The Servers tab shows that the server is started and all the code has been synchronized, that is, the code is the most recent (Synchronization happens whenever we save changes on some deployed file.) The Servers tab also has a list of deployed applications under the server. Just the one application that we are testing here is visible in the Servers tab.

![](bbox=[0.123, 0.206, 0.876, 0.31])

Now comes the interesting part – what are we going to test if we haven't really implemented anything? If we take a look at the web.xml file, we will find that we have defined one initialization parameter. The Debug parameter is true, which means that DWR generates test pages for our remoted Java classes. We just point the browser (Firefox in our case) to the URL http://127.0.0.1:8080/DWREasyAjax/dwr and the following page opens up:

![](bbox=[0.123, 0.458, 0.876, 0.593])

This page will show a list of all the classes that we allow to be remoted. When we click the class name, a test page opens as in the following screenshot: