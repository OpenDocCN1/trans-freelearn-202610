2. Let's put our WikiService in a Spring Python IoC container, so that it can be used as part of our wiki application.

```
from springpython.config import PythonConfig
from springpython.config import Object

class WikiProductionAppConfig(PythonConfig):
    def __init__(self):
        super(WikiProductionAppConfig, self).__init__()

    @Object
    def data_access(self):
        return MysqlDataAccess()

    @Object
    def wiki_service(self):
        return WikiService(self.data_access())
```

This should look familiar. It is the same IoC container definition we used in the previous chapter. The following block diagram shows the components of our application, with the Wiki's view layer calling into WikiService, which in turn calls data_access, finally reaching the database.

![](bbox=[0.37, 0.48, 0.63, 0.73])