Authentication providers are expected to either succeed or fail with the following behavior:

+   - If authentication is *successful*, return an authentication object. It is recommended to create a new instance of `UsernamePasswordAuthenticationToken`, copy in the `username`, `password`, and `granted_auths`, and then return it. This is the same class used by all of Spring Python Security's providers. We do *not* have to call `setAuthenticated(true)` on the token. `AuthenticationManager` handles this for us
+   - If authentication *fails*, raise a `BadCredentialsException`

For this provider, we can use `DatabaseTemplate` to do most of our work. To do that, we need an injected SQL connection factory. If we set the username and password with the values supplied by our user, we can create a `DatabaseTemplate` and attempt a query to retrieve the user's roles. Since our instance of `DatabaseTemplate` will try to login to the database before doing the query, this should serve as our authentication check. If successful, we will receive the granted authorities in a result set. If it fails, we will get a SQL exception, which we can catch and replace with a `BadCredentialsException`.

```
from springpython.security.providers import *
from springpython.database.core import *

class MySqlLoginAuthenticationProvider(AuthenticationProvider):
    def __init__(self, factory=None):
        self.factory = factory

    def authenticate(self, authentication):
        self.factory.username = authentication.username
        self.factory.password = authentication.password
        dt = DatabaseTemplate(self.factory)

        try:
            authorities =
                dt.query("select authority from user where username = ?",
                        (authentication.username,),
                        rowhandler=DictionaryRowMapper())

            return UsernamePasswordAuthenticationToken(
                authentication.username,
                authentication.getCredentials(),
                [auth["authority"] for auth in authorities])
        except:
            raise BadCredentialsException("Invalid credentials")
```