```
@Object
def data_access(self):
    return StubDataAccess()
```

2. This replaces MysqlDataAccess with StubDataAccess which runs quicker, avoids database contention with other developers, and has pre-formatted responses for each method. With Python's unit test framework taking the place of the view layer as the caller, we have isolated our code base for testing.

![](bbox=[0.24, 0.29, 0.75, 0.62])

3. Let's write a test that verifies the values of the caching service.

```
class CachedWikiTest(unittest.TestCase):
    def testCachingService(self):
        context = ApplicationContext(WikiTestAppConfig())
        caching_service = context.get_object("caching_service")
        self.assertEquals(len(caching_service.keys()), 0)
        caching_service.store("key", "value")
        self.assertEquals(len(caching_service.keys()), 1)
        self.assertEquals(caching_service.get("key"), "value")
        caching_service.del("key")
        self.assertEquals(len(caching_service.keys()), 0)
```