For our current problem, we want Spring Python to manage the creation of `WikiService` in a way that allows us to make changes in one place, so we don't have to edit every test method. To do this, we will define an IoC container and let it handle creating the objects for us. The following code shows a simple container definition.

+   1. First of all, let's create an IoC container using Spring Python and have it create our instance of `WikiService`.

```
from springpython.config import PythonConfig
from springpython.config import Object

class WikiProductionAppConfig(PythonConfig):
    def __init__(self):
        super(WikiProductionAppConfig, self).__init__()

    @Object
    def wiki_service(self):
        return WikiService()
```

You can spot the objects defined in the container by noting Spring Python's `@Object` decorator.

+   2. Now we want to change our startup code so that it uses the container instead of creating `WikiService` object directly.

```
if __name__ == "__main__":
    from springpython.context import ApplicationContext
    container = ApplicationContext(WikiProductionAppConfig())
    service = container.get_object("wiki_service")
    WikiWebApp(service).run()
```

In this version of our wiki web application, the service object was obtained by asking the container for `wiki_service`. Spring Python dispatches this request to `WikiProductionAppConfig` where it invokes the `wiki_service()` method. It is now up to the container to create an instance of `WikiService` and return it back to us.