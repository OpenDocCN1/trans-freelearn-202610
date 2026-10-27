# Repository Container

This container plays a key role in executing all the web scripts that provide direct access to the content residing in the Alfresco repository using the various services provided by Alfresco.

It is a specialized type of container mainly to be embedded only in the Alfresco repository. It uses some of the core services provided by the Alfresco repository to support transactions and authentication for web scripts.

Another useful and important thing that the Repository Container does is that it provides the repository-specific root objects available for the JavaScript based-controller and response templates.

Repository Container configuration can be found in Alfresco's web app in `web-scripts-application-context.xml` as a bean entry having ID `webscripts.container`.

In the recent versions of Alfresco (Version 4 onwards), implementation class is `org.alfresco.repo.web.scripts.TenantRepositoryContainer`, found in `alfresco-remote-api-*.jar`.

# Presentation Container

Presentation Container is a lightweight implementation mainly for web scripts to render user interface components making remote calls to remote repositories. It provides the basic support to web scripts for making service call to remote data sources.

This is available as the vanilla container in the spring web scripts implementation that gets shipped with Alfresco. This container implementation makes it easy to host web scripts in any environment.

Presentation Container configuration can be found in `spring-webscripts-*.jar` as a spring bean entry having ID `webscripts.container` in `web-scripts-application-context.xml`.

# Other supporting components

The various other components that Web Script Container relies upon and which play a key role while the execution of web scripts are being executed are explored in the following sections.