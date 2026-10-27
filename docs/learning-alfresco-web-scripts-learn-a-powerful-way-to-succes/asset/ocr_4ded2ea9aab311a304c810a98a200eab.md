# # DeclarativeWebScript versus AbstractWebScript

The web script framework in Alfresco provides two different helper classes from which the Java-backed controller can be derived. It's important to understand the difference between them.

The first helper class is the one we used while developing the web script in this chapter, `org.springframework.extensions.webscripts.DeclarativeWebScript`. The second one is `org.springframework.extensions.webscripts.AbstractWebScript`.

`DeclarativeWebScript` in turn only extends the `AbstractWebScript` class.

If the Java-backed controller is derived from `DeclarativeWebScript`, then execution assistance is provided by the `DeclarativeWebScript` class. This helper class basically encapsulates the execution of the web script and checks if any controller written in JavaScript is associated with the web script or not. If any JavaScript controller is found for the web script, then this helper class will execute it. This class will locate the associated response template of the web script for the requested format and will pass the populated model object to the response template.

For the controller extending `DeclarativeWebScript`, the controller logic for a web script should be provided in the `Map<String, Object> executeImpl(WebScriptRequest req, Status status, Cache cache)` method. Most of the time while developing a Java-backed web script, the controller will extend `DeclarativeWebScript` only.

`AbstractWebScript` does not provide execution assistance in the way `DeclarativeWebScript` does. It gives full control over the entire execution process to the derived class and allows the extending class to decide how the output is to be rendered. One good example of this is the `DeclarativeWebScript` class itself. It extends the `AbstractWebScript` class and provides a mechanism to render the response using FTL templates. In a scenario like streaming the content, there won't be any need for a response template; instead, the content itself needs to be rendered directly. In this case, the Java-backed controller class can extend from `AbstractWebScript`.