After authenticating and authorizing the user, the web request is allowed through to the web application. The following diagram shows what happens when we again submit our new wiki text, assuming we are authenticated and authorized to do so.

![](bbox=[0.17, 0.2, 0.82, 0.5])

It is important to point out that this requires no changes in either SpringWiki or SpringWikiController. We will quickly demonstrate that applying these changes involves only some configuration changes.

# Time to add security to our application

Now it's time to add these security features to our application.

First we need to define the Spring Python Security agent. To do this, we need to add the following code to our SpringWikiAppContext.

```
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
```