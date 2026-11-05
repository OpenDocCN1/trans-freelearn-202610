
This filter's job is to confirm the user is authenticated. If not, a security exception is thrown, and the exception_translation_filter handles the outcome. At this stage, it is possible to configure the system to re-authenticate on every web request, or to avoid consuming as many resources by caching the authentication status.

The following diagram shows how the filters are nested together. A call into this stack of filters gives each filter a chance to perform security functions on entry and/or exit. It also shows how the entire Spring Python Security stack is neatly staged between the caller and the application, without having to intertwine itself into the application itself through either class hierarchy or meddling with the application's API.

![](img/bcf886d8b665372ee8afbb620d4273bb_148_0.png)

Let's define the AuthenticationManager that manages the lookup of user credentials. For our situation, we will define a fixed list of users for test purposes.

```
@Object
def auth_manager(self):
    auth_manager = AuthenticationManager()
    auth_manager.auth_providers = [self.auth_provider()]
    return auth_manager
```

```
@Object
def auth_provider(self):
    provider = DaoAuthenticationProvider()
    provider.user_details_service = self.user_details_service()
    provider.password_encoder = PlaintextPasswordEncoder()
    return provider
```

```
def user_details_service(self):
    user_details_service = InMemoryUserDetailsService()
    user_details_service.user_dict = {
        "alice": ("alicespassword", ["ROLE_READ", "ROLE_EDIT"], True),
        "bob": ("bobspassword", ["ROLE_READ"], True)
    }
    return user_details_service
```

auth_manager references a list of authentication providers that are used to check credentials. When an authentication request is submitted, AuthenticationManager goes down this list, asking each one to attempt authentication until one of them succeeds. If no provider succeeds, AuthenticationManager throws a security exception, denying access.

Right now, we have only one provider defined, but it is possible to have more. This meets another one of our key requirements: multiple security providers must be allowed.

Our provider, DaoAuthenticationProvider, taps its user_details_service in order to lookup the user. It then runs the user-supplied password through its password_encoder and compares it to the stored password. In our situation, we are using an InMemoryUserDetailsService instead of an actual database, and the password is stored in the clear instead of hashed.

> It is highly recommended to not store passwords in the clear on any production system.

As you can see, we currently have two user accounts defined:

- alice has two defined roles: ROLE_READ and ROLE_EDIT. alice is an enabled account
- bob has one defined role: ROLE_READ. bob is an enabled account

Using the InMemoryUserDetailsService makes it easy to get our application up and running from a security perspective, since we don't have to worry about integrating with any external systems.

With all the components nicely separated, we can later swap InMemoryUserDetailsService with DatabaseUserDetailsService when we are ready to go online. We can also replace PlaintextPasswordEncoder with either ShaPasswordEncoder or Md5PasswordEncoder, to easily support different hashing algorithms.

Let's define the last filter needed in our chain: filter_security_interceptor.

```
@Object
def filter_security_interceptor(self):
    filter = FilterSecurityInterceptor()
    filter.auth_manager = self.auth_manager()
    filter.access_decision_mgr = self.access_decision_mgr()
    filter.sessionStrategy = self.session_strategy()
    filter.obj_def_source = [
        ("edit.*", ["ROLE_EDIT"]),
        ("/.*", ["ROLE_READ"])
    ]
    return filter
```

```
@Object
def access_decision_mgr(self):
    access_decision_mgr = AffirmativeBased()
    access_decision_mgr.allow_if_all_abstain = False
    access_decision_mgr.access_decision_voters = [RoleVoter()]
    return access_decision_mgr
```

filter_security_interceptor is the last security check in our app. Its job is to make sure the user is authorized to complete the request. It looks at the URL of the web request, and then goes down its list of URL patterns until it finds a match. When it does, it sees a list of roles. The roles along with the user's credentials are given to the access_decision_mgr, who submits a request for a vote from each of its access_decision_voters. In our configuration, we are using RoleVoter, which votes on whether or not the user has the role supplied in the list.

access_decision_mgr tallies up the votes, and decides if access is granted based on the policy. In our case, we have an AffirmativeBased policy, meaning that we need only one up vote, i.e. we only need to have one of the roles in the list. If we switched to a UnanimousBased policy, we would be required to have all the roles. A ConsensusBased policy would require that we have a majority of the roles.

This policy indicates that a user can only access /edit* URLs if he has ROLE_EDIT. For any other web path, the user must have ROLE_READ. Note: don't forget that /login* is excluded from this filter.

- Let's add a login page to our view layer
- Let's add the code to create and process a login page in our controller

```python
@cherrypy.expose
def login(self, fromPage="/", login="", password="",
errorMsg=""):
    return self.controller.getLoginPage(
        fromPage, login, password, errorMsg)
```

```python
def getLoginPage(self, fromPage="/", login="", password="", errorMsg=""):
    if login != "" and password != "":
        try:
            self.authenticate(login, password)
            return [self.redirector.redirect(fromPage)]
        except AuthenticationException, e:
            return [self.redirector.redirect(
                "?login=%s&errorMsg=Username/password failure" %
                login)]

    results = """
<html>
    <head>
        <title>Spring Python book demo</title>
    </head>
    <body>
        <form method="POST" action="">
            Login: <input type="text" name="login" value="%s"
size="10"/><br/>
            Password: <input type="password" name="password" size="10"/>
<br/>
            <input type="hidden" name="fromPage" value="%s"/><br/>
            <input type="submit"/>
        </form>
        <a href="http://springpythonbook.com">Spring Python book</a>
    </body>
</html>
    """ % (login, fromPage)
```

```
return [results]

def authenticate(self, username, password):
    token = UsernamePasswordAuthenticationToken(username, password)
    SecurityContextHolder.getContext().authentication = self.auth_manager.authenticate(token)
    self.httpContextFilter.saveContext()
```

- We also need to initialize the controller with some of the security components

```
def __init__(self):
    self.httpContextFilter = None
    self.auth_manager = None
    self.redirector = None
```

- These controller attributes must be injected from the IoC container

```
@Object
def controller(self):
    ctrl = controller.SpringWikiController()
    ctrl.httpContextFilter = self.httpSessionContextIntegrationFilter()
    ctrl.auth_manager = self.auth_manager()
    ctrl.redirector = self.redirectStrategy()
    return ctrl
```

With these parts injected into the controller, it now has the ability to process a login request, store it into the context, and have the user's authenticated credentials stored in their HTTP session data.

With all these modifications in place, Spring Python has clearly made it possible to wrap an existing application with a layer of security. The IoC definitions show fine grained control over what URLs require which filters, and also which authorization roles. This meets the important requirement: 'the security solution must be orthogonal to the class hierarchy'. At no time were we required to extend any security-based classes. Instead, we used a series of filters and security classes, all defined outside the hierarchy of our business model.

By keeping the policies in the centralized location of the application context, it is easy to adjust them as necessary. For example, if we needed a /admin.* pattern linked with ROLE_ADMIN, it is easy to adjust the access_decision_manager to easily support this. We can also put various URL patterns underneath different policies, all from within the application context. This supports the key requirement: 'security policies must be flexible and easy to fine tune'.
