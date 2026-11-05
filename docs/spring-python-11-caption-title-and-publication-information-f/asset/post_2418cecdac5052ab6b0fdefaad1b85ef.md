
Spring Python provides **dynamic weaving** through the use of **proxies**. Proxies are configured using the IoC container. Calls that are normally targeted at the object are instead routed through the proxy. The proxy evaluates possible pointcuts and applies `Advice`. This is also known as **intercepting.**

## Adding caching to Spring Python objects

In this example, we will enhance the wiki engine that we have been developing by writing a service that retrieves wiki text from the database and converts it to HTML. Then, to limit the load on our infrastructure, we will add caching support.

- 1. First, we need to code our service. The service will call a data access component to retrieve the wiki text stored in our database. Then we will convert it to HTML and hand it back to the caller.

```python
class WikiService(object):
    def __init__(self, data_access):
        self.data_access = data_access

    def get_article(self, article):
        return self.data_access.retrieve_wiki_text(article)

    def store_article(self, article):
        self.data_access.store_wiki_text(article)

    def html(self, text):
        pass # return wiki text converted to HTML

    def statistics(self, article):
        hits = self.data_access.hits(article)
        return (hits, hits /
            len(self.data_access.edits(article)))
```

Here we have a `WikiService` similar to the one we used in our IoC example. In addition to having a `statistics` method, this version is able to retrieve wiki text from the database and format it into HTML. It can also store edited wiki articles in the database. In this example we aren't particularly concerned with the code that transforms wiki text to HTML and so we've omitted it.

- 2. Let's put our WikiService in a Spring Python IoC container, so that it can be used as part of our wiki application.

```python
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

![](img/bcf886d8b665372ee8afbb620d4273bb_67_0.png)

- 3. Let's assume that our wiki engine is now being used for a very popular site with lots of articles and users. Multiple requests for the same article between edits are likely going to be common and will tax the database server with a database query every time an article is retrieved. To limit that performance hit, let's code a simple caching solution.

```python
class WikiServiceWithCaching(object):
    def __init__(self, data_access):
        self.data_access = data_access
        self.cache = {}

    def get_article(self, article):
        if article not in self.cache:
            self.cache[article] =
                self.data_access.retrieve_wiki_text(article)
        return self.cache[article]

    def store_article(self, article):
        del self.cache[article]
        self.data_access.store_wiki_text(article)

    def html(self, text):
        pass # return wiki text converted to HTML

    def statistics(self, article):
        hits = self.data_access.hits(article)
        return (hits, hits /
            len(self.data_access.edits(article)))
```

To handle multiple requests for the same article, WikiServiceWithCaching stores the wiki text in an internal dictionary called cache. This cache is then checked for the requested article before falling back to the database if the article is not in the cache. The cache is cleared whenever new edits are made. Finally the statistics don't have any caching at all.

To bring in our new caching wiki service, we just need to change the wiki_service declaration in our IoC container definition.

```python
class WikiProductionAppConfig(PythonConfig):
    def __init__(self):
        super(WikiProductionAppConfig, self).__init__()

    @Object
    def data_access(self):
        return MysqlDataAccess()

    @Object
    def wiki_service(self):
        return WikiServiceWithCaching(self.data_access())
```

This idea is pretty simple and should lessen the load on our database server. However, by mixing caching with wiki services we have violated the Single Responsibility Principle (SRP): a class should have one (and only one), reason to change. Changes to one of these functions could break the other. It is also harder to isolate these functions for testing and, ultimately, makes the code harder to read and understand.

- 4. Let's decouple these two concerns, maintaining the wiki and caching, by pulling our caching mechanism into a separate class and then delegating to our original WikiService.

```python
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

```python
class WikiProductionAppConfig(PythonConfig):
    def __init__(self):
        super(WikiProductionAppConfig, self).__init__()

    @Object
    def data_access(self):
        return MysqlDataAccess()

    @Object
    def wiki_service(self):
        return CachedService(WikiService(self.data_access()))
```

Our architecture has changed slightly, with the Wiki view layer calling into our CachedService.

![](img/bcf886d8b665372ee8afbb620d4273bb_70_0.png)

Now the SRP is obeyed as each class has only one responsibility. One class handles the caching by passing off all necessary WikiService calls to delegate. However, it isn't very practical. While CachedService (Code in Text) separates caching logic from WikiService, our usage of an adapter introduces some ugly constraints. The CachedService must have the same interface as the WikiService in order to handle its requests and we even had to code a passthrough for statistics. CachedService is also too specialized and wouldn't work for any generalized solution. Every time we add another method to WikiService, we have to change CachedService, giving us a tightly coupled pair of classes. Let's fix that now.

- 5. Let's use Spring Python to code an Interceptor that not only separates caching from wiki text management, but is also general enough to be reused in other places.

```python
from springpython.aop import *

class CachingInterceptor(MethodInterceptor):
    def __init__(self):
        self.cache = {}

    def invoke(self, invocation):
        if invocation.method_name.startswith("get"):
            if invocation.args not in self.cache:
                self.cache[invocation.args] =
                    invocation.proceed()
            return self.cache[invocation.args]

        elif invocation.method_name.startswith("store"):
            del self.cache[invocation.args]
            invocation.proceed()
```

![](img/bcf886d8b665372ee8afbb620d4273bb_71_0.png)

> CachingInterceptor defines a Spring Python aspect that has all the caching functionality and none of the wiki functionality. It contains both Advice (the caching functionality) and a pointcut (only applies to methods starting with get).

- 6. To use CachingInterceptor, we must create a Spring Python AOP proxy with an instance of our aspect as well as an instance of WikiService.

```python
class WikiProductionAppConfig(PythonConfig):
    def __init__(self):
        super(WikiProductionAppConfig, self).__init__()

    @Object
    def data_access(self):
        return MysqlDataAccess()

    @Object
    def wiki_service(self):
        return ProxyFactoryObject(
            target=WikiService(self.data_access()),
            interceptors=CachingInterceptor())
```
