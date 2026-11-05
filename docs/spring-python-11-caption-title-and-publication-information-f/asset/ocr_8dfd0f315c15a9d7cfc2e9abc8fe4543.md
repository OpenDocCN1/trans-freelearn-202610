# Adding Inversion of Control to our test

With this adjustment to WikiService and WikiProductionAppConfig, we can now code an alternative way of setting up our test case.

+   1. First, we subclass the production container configuration to create a test version. Then we override the data_access object so that it returns our stub alternative.
+   
+   2. Now, we can alter our test suite to fetch WikiService just like the production main code, except using our alternative container.

```
class WikiTestAppConfig(WikiProductionAppConfig):
    def __init__(self):
        super(WikiTestAppConfig, self).__init__()

    @Object
    def data_access(self):
        return StubDataAccess()
```

Using this technique, we inherit all the production definitions. Then we simply override the parts needed for our test situation. In this case, we return a slightly modified version of WikiService that is suitable for our testing needs.

```
from springpython.context import ApplicationContext

class WikiServiceTestCase(unittest.TestCase):
    def testHittingWikiService(self):
        container = ApplicationContext(WikiTestAppConfig())
        service = container.get_object("wiki_service")

        results = service.statistics("stub page")
        self.assertEquals(10.0, results[0])
        self.assertEquals(5.0, results[1])
```

The dependencies (or lack of!) between WikiService and StubDataAccess are shown in following diagram:

![](bbox=[0.31, 0.68, 0.69, 0.82])