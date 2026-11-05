# Dependency Injection a.k.a. the Hollywood principle

So far, we have managed to delegate creation of our WikiService object to Spring Python's Inversion of Control container. However, WikiService still has a hard-coded dependency to MySqlDataAccess.

The nature of IoC is to push object creation into a 3rd party location. Up to this point, we have been using the term Inversion of Control. The way that Spring Python implements IoC, is through the mechanism of Dependency Injection. Dependency Injection, or DI, is where dependencies are pushed into objects through either initialization code or by letting the container directly assign attributes. This is sometimes described by the Hollywood cliché of Don't call us, we'll call you. It means that the object which needs a certain dependency shouldn't make it directly, but instead wait on the external container to provide it when needed.

> Because Spring Python only using Dependency Injection, the terms, Inversion of Control, IoC, Dependency Injection, and DI may be used interchangeably throughout this book to communicate the same idea.

The following version of WikiService shows a complete removal of dependence on MySqlDataAccess.

```
class WikiService(object):
    def __init__(self, data_access):
        self.data_access = data_access

    def statistics(self, page_name):
        """Return tuple containing (num hits, hits per edit)"""
        hits = self.data_access.hits(page_name)
        return (hits, hits / len(self.data_access.edits(page_name)))
```