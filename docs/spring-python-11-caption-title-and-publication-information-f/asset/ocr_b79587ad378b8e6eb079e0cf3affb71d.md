Let's utilize Spring Python's container to create our parser and also to serve it up with Pyro.

```
from springpython.config import PythonConfig
from springpython.config import Object
from springpython.remoting.pyro import PyroServiceExporter
from springpython.remoting.pyro import PyroProxyFactory

class WebServiceContainer(PythonConfig):
    def __init__(self):
        super(WebServiceContainer, self).__init__()

    @Object(lazy_init=True)
    def my_web_server(self):
        return PyroServiceExporter(service=ParamParser(),
                                    service_name="mywebservice",
                                    service_port=9000)

    @Object(lazy_init=True)
    def my_web_client(self):
        myService = PyroProxyFactory()
        myService.service_url="PYROLOC://localhost:9000/mywebservice"
        return myService
```

With this container definition, it is easy to write both a server application as well as a client application. To spin up one instance of our Pyro server, we use the following code:

```
from springpython.context import ApplicationContext
container = ApplicationContext(WebServiceContainer())
container.get_object("my_web_server")
```

The client application looks very similar.

```
from springpython.context import ApplicationContext
container = ApplicationContext(WebServiceContainer())
myService = container.get_object("my_web_client")
myService.parse_web_parms("pages=5&article=Spring_Python")
```

The Spring Python container works by containing all the definitions for creating key objects. We create an instance of the container, ask it for a specific object, and then use it.

This easily looks like just as much (if not more) code than using the Pyro API directly. So why is it considered less invasive?