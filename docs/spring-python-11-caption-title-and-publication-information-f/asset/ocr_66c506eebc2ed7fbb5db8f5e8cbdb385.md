First of all, we need to initialize the security context settings stored in SecurityContextHolder. Another thing required by CherryPy is turning on HTTP sessions. We also must activate the filterChainProxy. Spring Python's FilterChainProxy, setup in our application context, automatically registers itself as a CherryPy tool.

2. Let's add the security components to our IoC configuration.

```
```python
from springpython.config import PythonConfig, Object
from springpython.security.providers import *
from springpython.security.providers.dao import *
from springpython.security.userdetails import *
from springpython.security.vote import *
from springpython.security.web import *
from springpython.security.cherrypy3 import *

from app2 import *

class SpringBankAppContext(PythonConfig):
    def __init__(self):
        PythonConfig.__init__(self)

    @Object
    def view(self):
        view = SpringBankView()
        view.auth_provider = self.auth_provider()
        view.filter = self.auth_processing_filter()
        view.http_context_filter = self.
        httpSessionContextIntegrationFilter()
        return view

    @Object
    def filterChainProxy(self):
        return CP3FilterChainProxy(filterInvocationDefinitionSource =
            [
            ("/login.*",  ["httpSessionContextIntegrationFilter"]),
            ("/.*",
                ["httpSessionContextIntegrationFilter",
                "exception_translation_filter",
                "auth_processing_filter",
                "filter_security_interceptor"])
            ])
```
```