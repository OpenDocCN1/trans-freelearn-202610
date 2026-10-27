> For Alfresco repository web scripts, the `org.alfresco.repo.web.scripts.RepositoryContainer` implementation and for Alfresco Share web script, `org.springframework.extensions.webscripts.PresentationContainer`, are used by the web script engine as the web script container.

# # Custom authenticator implementation

In order to access the web script securely, authentication is essential. The web script authenticator is the component behind providing the authenticated access to the web script. Let's first understand how authenticator implementation is referenced by the web script framework. If you take a look at web.xml under `tomcat\webapps\alfresco\WEB-INF` location inside your Alfresco-installed directory, you will find the `<param-name>authenticator</param-name>` entry under the `<init-param>` entries of a servlet, which is basically a request dispatcher servlet for web script execution. A request dispatcher servlet will use the mentioned authenticator while creating the web script runtime instance to serve the web script request.

The Spring bean ID for authenticator can be found under the `<param-value>` tag in web.xml under a servlet's init-param entries. The Spring bean entry can be found in `web-scripts-application-context.xml` or `web-client-application-context.xml`. Bean instances of an authenticator are created using a factory design pattern implementation. Hence, you will find the class name of the factory class against the bean ID. For example, the following is the code snippet for a Spring bean entry for the bean ID, `webscripts.authenticator.basic`:

```
```xml
<bean id="webscripts.authenticator.basic" class="org.alfresco.repo.web.scripts.servlet.BasicHttpAuthenticatorFactory">
```
```

All the authenticator-related factory classes implement the `org.springframework.extensions.webscripts.servlet.ServletAuthenticatorFactory` interface and implement the `create(WebScriptServletRequest req, WebScriptServletResponse res)` method to return the appropriate authenticator object. You will find the inner class implementation inside factory classes to represent the authenticator object provided by that factory class.

Now, let's take a look at the service provider interface to create an authenticator object. In the web script framework, to create a new authenticator, the `org.springframework.extensions.webscripts.Authenticator` interface must be implemented and must provide the implementation for the `authenticate()` and `emptyCredentials()` methods of this interface.