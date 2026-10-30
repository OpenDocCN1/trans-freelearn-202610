was created to establish a standard for communication between the client and server. However, SOAP is (contrary to its name) more elaborate and complex to implement and expects a predefined structure. This and other factors have led to a tendency for new web services to be built based on the REST architecture. In PAD, of course, we have corresponding actions for both versions that can be used for this, plus another action:

+   - **Download from web:** Used if we want to download a specific text or a file from a web server.
+   - **Invoke SOAP web service:** This is the action to use if we need to communicate with a web service that is based on SOAP. This action also provides a little wizard that helps to determine the values for the different parameters.
+   - **Invoke web service:** This action calls a RESTful web service.

PAD manages to hide the possible complexity in web service communication. Sometimes, however, more advanced settings have to be made because, for example, certain settings, such as authentication, are expected. Therefore, the two actions for web service calls can also make advanced settings, which are stored in the **Advanced** area, such as **Connection timeout, User agent, Encoding**, and more.

# Working with web data formats

When applications communicate with each other via web services, this is also done using a language. REST-based web services use JSON as a language here, which is built-in to PAD. There is, for example, an action to convert a variable into JSON and back.

# Background information – custom object versus JSON object

Internally, PAD can handle JSON objects but prefers to use a so-called custom object. This is a more flattened structure that is easier to handle in loops and contains a list of attribute names and values. A JSON object, instead, can be of any complexity with multiple levels of nesting structures.