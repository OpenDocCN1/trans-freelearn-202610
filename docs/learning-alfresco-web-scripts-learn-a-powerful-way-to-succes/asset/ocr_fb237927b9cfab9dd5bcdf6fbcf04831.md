# MVC in web scripts

The model-view-controller pattern in web scripts is explained as follows:

+   - The controller is a server-side JavaScript or Java class or it could be both as well
+   - The model is a data structure object passed between the controller and view
+   - The view is a FreeMarker template that is responsible for generating the response in the required format

The mapping of the web script URI to the controller is done through a descriptor file. It is mainly an XML file that will have the required details for a web script such as URL, description, arguments, transaction, authentication, and response formats.

Response formats are mapped to FreeMarker templates through naming conventions. For example, a FreeMarker template that returns an HTML response will have the extension `html.ftl`.

Web scripts are registered and executed by the web script engine in Alfresco.

> A descriptor, optional controller, and one or more FreeMarker response templates collectively make a web script in general scenarios. All these components are tied together through a specific document naming convention.

# Making web scripts work

To make web scripts work, it is important to deploy the web script files at an appropriate location.

The descriptor file and the FreeMarker template as per the response format for a web script can be either placed on a filesystem or put into the Alfresco repository.

If a JavaScript-based controller is used for a web script, then it could be placed along with the descriptor and FreeMarker template on a filesystem or in the Alfresco repository.