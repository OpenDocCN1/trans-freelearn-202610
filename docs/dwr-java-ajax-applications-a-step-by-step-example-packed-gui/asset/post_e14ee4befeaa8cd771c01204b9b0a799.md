
} catch (Exception e) {
    e.printStackTrace();
}
Collection<ScriptSession> sessions = serverContext
    .getAllScriptSessions();
for (Iterator<ScriptSession> iterator = sessions.iterator();
    iterator.hasNext();) {
    ScriptSession scriptSession = iterator.next();
    if (scriptSession.getId().equals(scriptId)) {
        ScriptBuffer script = new ScriptBuffer("orderProcessed('"
            + returnFromOrderSystem + "')");
        scriptSession.addScript(script);
    }
}
}

This class uses DWR reverse AJAX functionality. What is interesting about reverse AJAX here is that it is used from a non-web thread. In this case, when submitting the order, a new thread is started that sends a message via JMS to OrderSystem and waits for the response. When the response comes, or a timeout occurs, DWR methods are used to send a message to the web page.

The constructor of the OrderSystem class uses the DWR functionality to get web context for the calling web page. The code is provided by DWR and, as the comment in the source code indicates, it is little "hacky". At the end of the constructor, we get the script session ID of a calling page so we may send a response back to the correct web page and the orderProcessed() function can process the response in the index.jsp file. And finally, the constructor starts the worker thread and returns control to the calling method.

The run() method first initializes the JNDI initial context and then starts the JMS connection to the local ActiveMQ. Standard JMS methods are used to send order messages and then wait for responses to arrive in DWROrderConfirmationQueue.

## Setting up the Application for Messaging

In this section, we set up our example application for messaging by developing JavaScript functions and a Java class for submitting orders. We also set up reverse AJAX in this section.

## Backend Integration

The JavaScript function to receive order confirmation in the web page is as follows and we add it to index.jsp together with most of the other JavaScript functions.

```javascript
function orderProcessed(orderConfirmationMessage)
{
    var feedback=dwr.util.byId('formFeedback');
    var html="";
    html=orderConfirmationMessage;
    feedback.innerHTML=html;
}
```

In order to integrate the order system with the submission process, we change the code for the `submitOrder()` method in the FormHandler class. The new code for this method is as follows:

```java
public boolean submitOrder(String name, String address,
        String creditCardNumber, String expiryDate) {
    CreditCardValidatorSoapProxy ccValidatorProxy = new
    CreditCardValidatorSoapProxy();
    int rv = -1;
    try {
        rv = ccValidatorProxy.validCard(creditCardNumber,
                expiryDate.replace("/", ""));
        if (rv != 0) {
            System.out.println("Credit card check failed: " + rv);
        }
    } catch (RemoteException e) {
        e.printStackTrace();
    }
    if(rv==0)
    {
        //credit card valid, submit to order system
        new OrderSystem(name,address,creditCardNumber,expiryDate);
    }
    return rv == 0;
}
```

The new addition here is after the web service call to the `CreditCardValidator` (shown in bold in the previous code). If the credit card is valid, then the `OrderSystem` class is instantiated. Because it has a constructor that starts a new thread for processing, there is no need for any variables. When the thread completes, there will be no references to the `OrderSystem` class, and Java's garbage collector will clean up leftover objects.

5. We get the order confirmation back to the web application as is shown in the following screenshot:

![](img/5b721cfaa97eb697efc92e3914f62fca_174_0.png)

## Afterword

Using messaging in a DWR application is easy, just like using a database or web services. In this case, messaging gives a good channel for a platform to communicate directly with the user when using reverse AJAX.

## Summary

We had three examples for backend integration in this chapter: database, web services, and messaging. One or more of these are found in almost all applications, and as the examples presented, DWR is agnostic to backend integration. It is possible to use the most familiar technology to access databases and other backend services and let DWR handle only the frontend user interface.

We also saw how Comet technology is used in the DWR messaging example, where Comet is a natural fit, by providing event-style messaging between the browser and the server.

## Sample Applications

This chapter includes two sample applications that use some of the functionalities that were presented in the samples in the previous chapters. The first sample application is Collaborative Book Authoring, which enables multiple users to author the same book. The second sample application is a straightforward chat room application.

These samples concentrate on the DWR functionality, which means that much of the features of real production-grade applications are missing from the sample applications. However, the samples show how DWR is used, and it should be easy to enhance applications to production grade using the samples as a starting point for development and as a source of ideas.

Two sample applications in this chapter are as follows:

- Collaborative Book Authoring – shows how DWR is used to create a web-based, multi-user authoring environment
- Chatroom – a typical multi-user chat room application using DWR

### Collaborative Book Authoring

The Collaborative Book Authoring sample application enables multiple users to work on the same or different books simultaneously. The idea is that an author logs into a system and he/she sees all the books that are in process, and have been published. Anyone can start a new book and anyone can contribute to a book (so this is quite an open process). Also within a book, anyone can start a new chapter and again anyone can edit the chapter content. The system is built so that only one author can edit a chapter. If someone is editing a chapter content, and another user tries to edit it, he/she will see the most recent save of the chapter content.

When the book is finished, authors will vote for publishing. Authors can vote only once, and a vote cannot be taken back. When all the authors have voted for publishing, the book is moved to a published status, and it is formatted for reading.

## Starting the Project

We start the project by creating a new web project in Eclipse using the name **DWRBookAuthoring**. In this case, development starts with the HTML pages. We need a login page, login failed page, and main page that will hold all the JavaScript functions. We will also use the CSS from the Dynamic Drive as we did in the previous chapters.

The following screenshot shows what kind of user interface we are developing:

![](img/5b721cfaa97eb697efc92e3914f62fca_177_0.png)

## Developing the User Interface

We start the application development with the user interface. First, we get the style sheet. The source code of the style sheet is given next. Just like the style sheets in the previous chapters, this CSS is also from the Dynamic Drive CSS library, http://www.dynamicdrive.com/style/.

```
body{
margin:0;
padding:0;
line-height: 1.5em;
}
```

```
b{font-size: 110%;}
em{color: red;}

#topsection{
background: #EAEAEA;
height: 90px; /*Height of top section*/
}

#topsection h1{
margin: 0;
padding-top: 15px;
}

#contentwrapper{
float: left;
width: 100%;
}

#contentcolumn{
margin-left: 200px; /*Set left margin to LeftColumnWidth*/
}

#leftcolumn{
float: left;
width: 200px; /*Width of left column*/
margin-left: -100%;
background: #C8FC98;
}

#footer{
clear: left;
width: 100%;
background: black;
color: #FFF;
text-align: center;
padding: 4px 0;
}

#footer a{
color: #FFFF80;
}

.innertube{
margin: 10px; /*Margins for inner DIV inside each column (to provide padding)*/
margin-top: 0;
}
```
