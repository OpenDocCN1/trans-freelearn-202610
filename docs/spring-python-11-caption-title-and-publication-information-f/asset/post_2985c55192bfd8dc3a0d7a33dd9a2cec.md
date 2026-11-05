
## Looking at our web application from 10,000 feet

The following diagram shows a high level view of our web application as a user looks up an article, clicks on the Edit button, makes changes, and then submits an update. By clicking on the Save button, the new wiki text is posted to Spring Wiki.

![](img/bcf886d8b665372ee8afbb620d4273bb_142_0.png)

In our example application, we have coded a simplistic database: a Python dictionary. However, that could easily be replaced by a relational database hosted on a separate server. The concept is the same: save the changes submitted by the user.

If we started out developing this application on the premise that anyone can read and edit any article, this solution works. It is simple, easy to understand, and probably the only security needed would be a firewall to protect our servers from external attack.

## Handling new security requirements

We have doubtlessly learned that new requirements are always coming, and we have to be ready to implement them quickly. We started our application by implementing the core functionality. Now we need to support multiple users and need to control who edits what pages.

## Authentication confirms "who you are"

The first step towards securing an application is authenticating the user. This identifies who the user is, and is usually accomplished by the user providing a username and a password. The following diagram shows an update to our application's design. It includes a new component not found in the previous diagram: a Spring Python Security agent that polices every web request combined with a 3rd party security resource. Before allowing the user to actually touch our wiki application or the database, our security agent checks if the user has been authenticated. If not, Spring Python redirects the user to a login page. In the diagram, we pick up at the point where the user submits new wiki text.

![](img/bcf886d8b665372ee8afbb620d4273bb_143_0.png)

The security agent meets one of our earlier requirements: application of security must be decoupled from the application. The agent is a separate piece of code charged with the responsibility of managing security between the user and our application. Its configuration is not part of our wiki application, but instead configured using the IoC container. We will see how to configure this agent shortly.

The following diagram shows the user submitting credentials. The security agent checks the credentials against the 3rd party security resource to confirm the user's identity. The most common implementation is a simple database table containing user data.

![](img/bcf886d8b665372ee8afbb620d4273bb_144_0.png)

## Authorization confirms "what you can do"

It is important to point out that our security agent is not done. After confirming the user's identity, Spring Python next looks up what authorities the user is granted. One user may have the authority to read articles, while another has the authority to read articles and also edit them. The last few steps of the previous diagram shows this as authorities are looked up and stored in Spring Python Security's context holder.

The standard convention is to allow a user to have zero or more roles. Having collected this information, our security agent stores the user's identity, granted authorities, and authenticated status in a globally accessible `SecurityContextHolder`. This is important, because it provides a way for our application to look up information about the user's security status without modifying the application's APIs. This meets another one of our requirements: credential data must be available non-intrusively. After securing our application, we will see how this is available.

After authenticating and authorizing the user, the web request is allowed through to the web application. The following diagram shows what happens when we again submit our new wiki text, assuming we are authenticated and authorized to do so.

![](img/bcf886d8b665372ee8afbb620d4273bb_145_0.png)

It is important to point out that this requires no changes in either SpringWiki or SpringWikiController. We will quickly demonstrate that applying these changes involves only some configuration changes.

## Time to add security to our application

Now it's time to add these security features to our application.

First we need to define the Spring Python Security agent. To do this, we need to add the following code to our SpringWikiAppContext.

```python
@Object
def filterChainProxy(self):
    return CP3FilterChainProxy(filterInvocationDefinitionSource =
        [
        ("/login.*",  ["httpSessionContextIntegrationFilter"]),
        ("/.*",       ["httpSessionContextIntegrationFilter",
                       "exception_translation_filter",
                       "auth_processing_filter",
                       "filter_security_interceptor"])
        ])
```

springpython.security.cherrypy3.CP3FilterChainProxy uses CherryPy APIs to insert itself into the chain of handlers that are called before our CherryPy application is called. This allows the filter chain proxy to apply the security policies we are configuring.

The filter chain proxy is configured with a list of URL patterns. On every web request, the proxy will iterate over this list until it finds a match.

When a match is found, it applies the defined chain of filters before allowing access to the web application itself. Each filter is configured as a string used to reflectively look up the actual filter in the application context.

If a match is not found, the entire security stack is bypassed. This is not the recommended solution. Instead, it is best to cover all the possible patterns by having a catch-all pattern (/.*) to clearly show what filters are applied where.

Let's add the first filter needed to the application context:

```python
httpSessionContextIntegrationFilter.
@Object
def httpSessionContextIntegrationFilter(self):
    filter = HttpSessionContextIntegrationFilter()
    filter.sessionStrategy = self.session_strategy()
    return filter

@Object
def session_strategy(self):
    return CP3SessionStrategy()
```

First, note that the method name matches the filter's string name found in filterChainProxy.

springpython.security.web.HttpSessionContextIntegrationFilter is used to transfer security credentials between the user's HTTP session and Spring Python's SecurityContextHolder. The rest of the Spring Python Security components use SecurityContextHolder to access the user's credential information.

In our application's configuration, this filter is used when accessing the login page (which we haven't coded yet), so that the login page can store credential info. The rest of the application has several more filters.

In order for Spring Python to support different web frameworks, the mechanism to interact with HTTP session data is encapsulated inside cherrypySessionStrategy(). In our case, we use CP3SessionStrategy.

Let's add the exception_translation_filter to manage any security exceptions thrown by other filters or the web application itself.

```python
@Object
def exception_translation_filter(self):
    filter = ExceptionTranslationFilter()
    filter.authenticationEntryPoint = self.auth_filter_entry_pt()
    filter.accessDeniedHandler = self.accessDeniedHandler()
    return filter
```

```python
@Object
def auth_filter_entry_pt(self):
    filter = AuthenticationProcessingFilterEntryPoint()
    filter.loginFormUrl = "/login"
    filter.redirectStrategy = self.redirectStrategy()
    return filter
```

```python
@Object
def accessDeniedHandler(self):
    handler = SimpleAccessDeniedHandler()
    handler.errorPage = "/accessDenied"
    handler.redirectStrategy = self.redirectStrategy()
    return handler
```

```python
@Object
def redirectStrategy(self):
    return CP3RedirectStrategy()
```

This filter is responsible for redirecting the user to the right page in the event of a security exception. If the user has not been authenticated yet, he is redirected to the login page. If he is trying to access a page without the appropriate authorization, he is redirected to the accessDenied page.

In order for Spring Python to support different web frameworks, the mechanism to issue a web redirect is encapsulated inside redirectStrategy(). In our case, we use CP3RedirectStrategy.

Let's add the authenticationProcessingFilter to confirm a user is authenticated before proceeding.

```python
@Object
def auth_processing_filter(self):
    filter = AuthenticationProcessingFilter()
    filter.auth_manager = self.auth_manager()
    filter.alwaysReauthenticate = False
    return filter
```
