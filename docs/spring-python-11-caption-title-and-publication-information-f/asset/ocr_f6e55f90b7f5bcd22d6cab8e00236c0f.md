8. Having migrated this much, we decide to start coding some tests. We want to use the style shown at the beginning of this chapter where we swap out MySqlDataAccess with StubDataAccess using an alternative configuration. To avoid changing production.xml, we create an alternate configuration file test.xml.

```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<objects xmlns="http://www.springframework.org/springpython/
schema/objects"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/
springpython/schema/objects
        http://springpython.webfactional.com/schema/
context/spring-python-context-1.0.xsd">

    <object id="data_access" class="Py.StubDataAccess"/>
</objects>
```
```

9. Now let's code a test case using these two files for a specialized container. To do that, we give ApplicationContext with a list of configurations.

```
```python
import unittest
from springpython.context import ApplicationContext
from springpython.config import XMLConfig

class WikiServiceTestCase(unittest.TestCase):
    def testHittingWikiService(self):
        ctx = ApplicationContext([XMLConfig("production.xml"),
                                XMLConfig("test.xml")])
        service = ctx.get_object("wiki_service")
        results = service.statistics("stub page")
        self.assertEquals(10.0, results[0])
        self.assertEquals(2.0, results[1])
```
```

The order of filenames is important. First, a list of definitions is read from production.xml. Next, more definitions are read from test.xml. Any new definitions with the same name as previous ones will overwrite the previous definitions. In our case, we overwrite data_access with a reference to our stub object.