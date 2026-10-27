
```html
<p>Hello! ${args.name}.</p>
</body>
</html>
```

- 5. Register the web script by hitting `http://localhost:8080/alfresco/service/index` and click on the **Refresh Web Scripts** button.
- 6. Now, hit the URL `http://localhost:8080/alfresco/service/helloworld?name=Ramesh` in order to execute the web script. On execution, you should be able to see **Hello! Ramesh.** as the output.

> It is important to understand here that even if you had not provided name={argumentName} in the third step and hit the URL to execute the web script, as mentioned in the sixth step, then you would also have got the same result. Specifying name={argumentName} in the description document helps you know about the arguments used by web script. This way, by just looking at the description document, you will know about the arguments for web script. As a good practice, you should always add the arguments used by your web script in its description document.

## Extending the first web script to use the controller

The web script framework in Alfresco makes it easy to have a clear separation of concerns by following a model-view-controller pattern in order to develop a web script. All the business logic resides in the controller and it is possible to have multiple views as required to return the response in different response formats supported by the web script framework in Alfresco. The model object is a data structure used to pass information from the controller to the view. The controller populates the model object with the required data and passes it to the view to generate the response.

In the first web script, we created a basic and simple web script, which just renders the output without interacting with the Alfresco repository. Ideally, any processing related to business logic such as querying the Alfresco repository, creation of content, updating content, deleting content from the repository, and executing actions should be done in a controller. A controller could be a server-side JavaScript or a Java-based controller.

Let's extend our first web script to add a controller to get an understanding about how the controller populates and passes data to FreeMarker template and how FreeMarker template uses the model data to generate the response:

- 1. Log in to Alfresco Share UI.
- 2. Click on the **Repository** link from the top header.
- 3. Go to **Data Dictionary | Web Scripts Extensions**.
- 4. Edit the descriptor file `helloworld.get.desc.xml` as follows:

    ```
    <webscript>
      <shortname>Hello World</shortname>
      <description>First webscript Hello world</description>
      <url>/helloworld?name={argumentName}</url>
      <authentication>user</authentication>
    </webscript>
    ```

- 5. Create a new file named `helloworld.get.js` with content as follows:

    ```
    model.email="Ramesh.chauhan@cignex.com";
    ```

- 6. Edit the FreeMarker template `helloworld.get.html.ftl` as follows:

    ```
    <html>
      <body>
        <p>Hello! ${args.name}.</p>
        <p>email address: ${email}</p>
      </body>
    </html>
    ```

- 7. Go to `http://localhost:8080/alfresco/service/index` and click on the **Refresh Web Scripts** button.
- 8. Now, hit the URL `http://localhost:8080/alfresco/service/helloworld?name=Ramesh` to execute the web script. It will ask for user credentials. Provide user credentials and you should see the response as:
    **Hello! Ramesh.**
    **email address: Ramesh.chauhan@cignex.com**

This is how we can have a controller added in a web script in a simple way. In the preceding example, we hard-coded the e-mail address to be returned in the web script response. We did not have any interaction with the Alfresco repository. We just populated the model object and passed it to the FreeMarker template from the server-side JavaScript controller.

> Model is basically a root object that Alfresco makes available for data sharing to happen between the server-side JavaScript controller and FreeMarker template view.

You can modify the controller to get the e-mail address dynamically retrieved from the Alfresco repository based on the name argument received. With the help of Alfresco JavaScript API, you may first retrieve the user from the repository based on the name argument and then fetch the e-mail address of the user. In a server-side JavaScript controller, you can leverage on available Alfresco JavaScript API to interact with the repository. This way, we can have a web script render a response based on the data retrieved from the Alfresco repository.

> In a server-side JavaScript controller, you can leverage on available Alfresco JavaScript API to access the Alfresco repository and perform business logic as required.

## Behind the scenes of web script execution

### A web script without a controller

To execute the first web script we created without controller, we just hit the URL http://localhost:8080/alfresco/service/helloworld?name=Ramesh on a web browser, which is basically an HTTP GET request call.

All the incoming requests matching the URL pattern /alfresco/service/* are mapped to the web script request dispatcher servlet in the Alfresco web application configuration.

The servlet then passed the request to the Web Script Runtime to process the web script.

The Web Script Runtime passed the control to the Web Script Container. The Web Script Container internally identified which web script to execute based on the invoked web script URL, HTTP method, and the web script descriptor document helloworld.get.desc.xml. We have not specified any response format explicitly in the descriptor document and have not explicitly mentioned one while executing the web script. Hence, the web script framework has rendered the HTML as the default format. Based on the naming conventions of a FreeMarker template, we recollect that we have created the FreeMarker template as `helloworld.get.html.ftl`. This generated the HTML response for the web script. The formatted response is then sent back to the web browser to display the HTML text in the web script response along with the value of the argument name.

### A web script with a controller

You must have observed that in the first web script, we created the web script without the controller and then extended the web script to have the controller. The URL to invoke this web script remains the same. There isn't any difference in the URL. It was only a backend implementation that was additionally implemented to have a controller added in the web script implementation.

Behind the scenes when a web script with a controller is invoked, the web script framework in Alfresco will follow the same process as mentioned for a web script with no controller. The only difference would be before rendering the response, the web script framework in Alfresco would execute the controller implementation first. The controller will first complete the required business logic processing interacting with the Alfresco repository and generating the required data to be returned as a web script response. The controller will populate the model object to pass to the FreeMarker template.

## Important things for any web script

While implementing the web scripts in Alfresco, it is essential to understand some of the very important things to be taken care of. There are three important things you should always keep in mind when developing Alfresco web scripts:

- Choosing the HTTP method for the web script
- Specifying the URL and arguments for the web script
- Response formats of the web script

### HTTP methods supported by a web script

While developing web scripts and interacting with the Alfresco repository, you should ensure you select the appropriate HTTP method for the web script. It is important to understand the various HTTP methods supported by the web script framework in Alfresco so that you can select the appropriate one for your web script.
