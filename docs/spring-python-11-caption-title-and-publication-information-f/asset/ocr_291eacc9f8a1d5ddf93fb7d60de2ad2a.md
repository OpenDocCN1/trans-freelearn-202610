```
class TvShowContainer(PythonConfig):
    def __init__(self):
        super(TvShowContainer, self).__init__()

    @Object(lazy_init=True)
    def web_server(self):
        return PyroServiceExporter(service=TvShowService(),
                                    service_name="tvshows",
                                    service_port=9000)

    @Object(lazy_init=True)
    def web_client(self):
        myService = PyroProxyFactory()
        myService.service_url="PYROLOC://localhost:9000/tvshows"
        return myService

if __name__ == "__main__":
    container = ApplicationContext(TvShowContainer())
    container.get_object("web_server")
```

By querying the database for TV shows and serving it up through Pyro, this block of code demonstrates how easy it is to use these powerful modules without mixing them together. It is much easier to maintain software over time when things are kept simple and separated.

We just took a quick walk through SQL and Pyro and examined their low level APIs. Many low level APIs require a certain sequence of steps to properly utilize them. We just looked at a SQL query. The need for templates also exists with database transactions and LDAP calls. By capturing the flow of the API in a Spring Python template and allowing you to insert your custom code, you can get out of writing plumbing code and instead work on your application's business logic.

# Spring Python for Java developers

Java developers often seek ways to increase productivity. With the incredible growth of alternative languages on the Java Virtual Machine (JVM), it is no wonder developers are looking into Jython (http://www.jython.org) as an alternative way to write scripts, segments, and subsystems. While it's convenient to use the Java libraries you already know, it can sometimes be rough interacting with the Java idioms of the library.