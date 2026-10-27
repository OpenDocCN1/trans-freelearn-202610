
The implementation of this runtime (`WebScriptJSFRuntime`) can be found as an inner class implementation in `org.springframework.extensions.webscripts.jsf.UIWebScript` located in `spring-webscripts-*.jar`.

## Facebook Runtime

Facebook Runtime provides access to Facebook APIs in the Alfresco web scripts.

The implementation of this runtime is available in `org.alfresco.repo.web.scripts.facebook.FacebookServletRuntime` in `alfresco-remote-api-*.jar`.

## SURF Runtime

The SURF web framework runtime allows building up web-tier UI pages by embedding the Alfresco web script components.

The implementation of the SURF runtime can be found in `org.springframework.extensions.webscripts.LocalWebScriptRuntime` in `spring-surf-*.jar`.

## Web Script Authenticator

When interacting with any content repository, secure access to content is a must and is really essential. Alfresco also requires an authenticated access while accessing the content repository. The main task of the Web Script Authenticator is to authenticate web script requests.

There are different authenticators, as mentioned in the following list, available out-of-the-box in Alfresco, which can be configured in the relevant Alfresco Web Script Runtime in order to provide secure access to the Alfresco repository through Alfresco web scripts:

- HTTP basic authenticator
- Alfresco explorer authenticator
- JSR-168 Authenticator
- JSR-168 Authenticator with Alfresco explorer support
- Facebook Authenticator

The web script framework by default uses basic HTTP authentication. However, it can be configured to use other forms of authentication mechanisms.

### HTTP basic authenticator

A username and password or a ticket must be specified while accessing the web script through an HTTP/HTTPS request when web script needs to be invoked for an authenticated user.

The authorization header must contain a username and password or a ticket.

The ticket can be specified through a query string parameter: `alf_ticket`.

> A ticket basically represents an authenticated user who has already performed successful login to Alfresco. It can be easily obtained using the out-of-the-box Alfresco web script via the URL `http://localhost:8080/alfresco/service/api/login?u={username}&pw={password}`. You just need to provide the username and password in the placeholders here before invoking it. Generally, while accessing the Alfresco repository from your custom frontend application or third-party system, this would be the first thing you would do in order to proceed further and invoke other web scripts with authenticated access.

Servlet Runtime uses an HTTP basic authenticator to authenticate the web script requesting user.

This authenticator is configured as a spring bean having ID `webscripts.authenticator.basic` in `web-scripts-application-context.xml`.

> The `web-scripts-application-context.xml` file can be located at `tomcat\webapps\alfresco\WEB-INF\classes\alfresco` inside your Alfresco installed directory. If you are using Alfresco Community 5, it can be located under the `alfresco` package inside `alfresco-remote-api-*.jar`.

### Alfresco Explorer authenticator

This authenticator uses the authentication mechanism that is used while logging to Alfresco Web Client.

The user will be redirected to the Alfresco Web Client login page if a login is required.

This authenticator is configured as a spring bean having ID `webscripts.authenticator.webclient` in `web-client-application-context.xml`.

The `web-client-application-context.xml` file can be found at `tomcat\webapps\alfresco\WEB-INF\classes\alfresco` inside your Alfresco installed directory. The location remains the same for Alfresco Community 5 as well.

### JSR-168 Authenticator

JSR-168 Authenticator expects the user has been already authenticated by the portal. Authenticated portal user can be found from the portal session using the `alfportletusername` attribute.

This authenticator is configured as a spring bean having ID `webscripts.authenticator.jsr168` in `web-scripts-application-context.xml`.

### JSR-168 Authenticator with Alfresco Explorer support

Alfresco repository web scripts, depending on Alfresco Web Client functionalities, can be authenticated and used within the portal with the use of this authenticator.

This authenticator is configured as a spring bean having ID `webscripts.authenticator.jsr168.webclient` in `web-client-application-context.xml`.

### Facebook Authenticator

Users will be redirected to the Facebook login page if a login is required.

This authenticator is configured as a spring bean having ID `webscripts.authenticator.facebook` in `web-scripts-application-context.xml`.

## Web Script Container

Web Script Container is the core of the web script framework. It plays a major role in executing the web script by being a central point integrating all other required services in order to execute web script. It identifies the appropriate web script and provides the root objects to the web script.

Alfresco out-of-the-box provides two types of Web Script Containers:

- Repository Container
- Presentation Container

### Repository Container

This container plays a key role in executing all the web scripts that provide direct access to the content residing in the Alfresco repository using the various services provided by Alfresco.

It is a specialized type of container mainly to be embedded only in the Alfresco repository. It uses some of the core services provided by the Alfresco repository to support transactions and authentication for web scripts.

Another useful and important thing that the Repository Container does is that it provides the repository-specific root objects available for the JavaScript-based controller and response templates.

Repository Container configuration can be found in Alfresco's web app in `web-scripts-application-context.xml` as a bean entry having ID `webscripts.container`.

In the recent versions of Alfresco (Version 4 onwards), implementation class is `org.alfresco.repo.web.scripts.TenantRepositoryContainer`, found in `alfresco-remote-api-*.jar`.

### Presentation Container

Presentation Container is a lightweight implementation mainly for web scripts to render user interface components making remote calls to remote repositories. It provides the basic support to web scripts for making service call to remote data sources.

This is available as the vanilla container in the spring web scripts implementation that gets shipped with Alfresco. This container implementation makes it easy to host web scripts in any environment.

Presentation Container configuration can be found in `spring-webscripts-*.jar` as a spring bean entry having ID `webscripts.container` in `web-scripts-application-context.xml`.

## Other supporting components

The various other components that Web Script Container relies upon and which play a key role while the execution of web scripts are being executed are explored in the following sections.

### Configuration service

All relevant XML configuration files for the web script are read and parsed using the configuration service.

The implementation class is `org.springframework.extensions.config.xml.XMLConfigService`.

### Web Script registry

Indexes for all the web scripts that are registered to the specific Web Script Container are maintained using the web script registry.

The implementation class is `org.springframework.extensions.webscripts.DeclarativeRegistry`.

### Format registry

Format registry is the registry to get the relevant format for the specified MIME type and also to get the MIME type for the mentioned format.

The `org.springframework.extensions.webscripts.FormatRegistry` class contains the implementation details.

### Script processor registry

The script processor registry provides a script processor that executes the backend implementation of the web script controller implemented in JavaScript.

The implementation of the script processor registry can be found in `org.springframework.extensions.webscripts.ScriptProcessorRegistry`.

The script processor used to execute the JavaScript-based controller is `org.alfresco.repo.jscript.RhinoScriptProcessor`.

### Template processor registry

The template processor registry provides the template processor that executes the response templates for the web script.

The implementation of it can be located in `org.springframework.extensions.webscripts.TemplateProcessorRegistry`.

The Default FreeMarker template processor implementation is available in `org.springframework.extensions.webscripts.processor.FTLTemplateProcessor`.
