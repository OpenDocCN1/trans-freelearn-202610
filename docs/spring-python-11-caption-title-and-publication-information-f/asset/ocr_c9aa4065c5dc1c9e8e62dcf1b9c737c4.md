```
return MysqlDataAccess()

@Object
def wiki_service(self):
    return WikiServiceWithCaching(self.data_access())
```

This idea is pretty simple and should lessen the load on our database server. However, by mixing caching with wiki services we have violated the Single Responsibility Principle (SRP): a class should have one (and only one), reason to change. Changes to one of these functions could break the other. It is also harder to isolate these functions for testing and, ultimately, makes the code harder to read and understand.

+   4. Let's decouple these two concerns, maintaining the wiki and caching, by pulling our caching mechanism into a separate class and then delegating to our original WikiService.

```
class CachedService(object):
    def __init__(self, delegate):
        self.cache = {}
        self.delegate = delegate

    def get_article(self, article):
        if article in self.cache:
            return self.cache[article]
        else:
            return self.delegate.get_article(article)

    def store_article(self, article):
        del self.cache[article]
        self.delegate.store_article(article)

    def html(self, text):
        return self.delegate.html(text)

    def statistics(self, article):
        return self.delegate.statistics(article)
```

Now we need to update our IoC configuration.

```
class WikiProductionAppConfig(PythonConfig):
    def __init__(self):
        super(WikiProductionAppConfig, self).__init__()
```