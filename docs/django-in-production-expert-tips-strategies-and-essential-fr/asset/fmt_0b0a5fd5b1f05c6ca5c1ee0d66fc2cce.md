
```
some_costly_function.invalidate('abc')
# This would invalidate the caching for the input argument of
# 'abc' but would have the other functions cached
some_costly_function.invalidate()
# This would invalidate the caching for overall function.
```

* Implementing caching on auth tables is a double-edged sword; one should avoid it unless there is strong latency associated with DataBase query for auth tables.

* `Cacheops` provides the interface to cache all tables for Django apps using the configuration in the `settings.py` file. In my opinion, this is something one should avoid because it can cause a lot of confusion for developers unable to see expected results in production since there is a blanket caching layer on the model table. I always recommend using function-level caching, as this is more explicit and verbose for anyone to detect.

* If there is a strong need to implement caching in the DB layer—for example, if we have to cache all the get queries for auth user table—we can configure `cacheops` in the Django `settings.py` file:

```
CACHEOPS = {
    # Cache any User.objects.get() calls for 15 minutes
    # This also includes .first() and .last() calls,
    # as well as request.user or post.author access,
    # where Blog.author is a foreign key to auth.User
    'auth.user': {'ops': 'get', 'timeout': 60*15},
    # Cache all gets and QuerySet fetches
    # to other django.contrib.auth models for an hour
    'auth.*': {'ops': {'fetch', 'get'}, 'timeout': 60*60}
}
```

* Use `cached_as` decorator whenever you want to implement the auto invalidation of cached results on any data source update. For example, if we want to invalidate cache on any blog table update, we should use the `cached_as` decorator and pass the model name `Blog` as a parameter:

```
from cacheops import cached_as
@cached_as(models.Blog, timeout=15*60)
def get_all_blogs(author_id):
    blogs = models.Blog.objects.filter(author_id=author_id)
    return list(blogs)
```

Whenever there would be any update in the `Blog` model data, then the cache would be automatically invalidated in the `get_all_blogs` function.

Apart from the mentioned tips and caveats, it is highly recommended to read all the additional information provided in the official documentation. Now let us learn a few good practices to be followed while implementing caching in production.

## Best practices for caching in production

Let us see a few important points we should keep in mind while implementing caching in production:

* Always use high-availability Redis cluster on production. It is recommended to use managed services such as ElastiCache by AWS or Redis Lab with master–slave configuration for production. This becomes important because if you have a high read system and caching goes down, all the read queries hit the database, which can create a cascading effect and bring the whole system down.

* Avoid premature caching on every DB query. A lot of developers try to save an additional 10 ms of database query calls and make the system complicated.

* Always be verbose on the caching implementation and add caching on functions rather than overall views or DB tables.

* Cache invalidation is an important part of cache implementation. Always create interfaces to invalidate the cache in production. I have personally observed a couple of production issues caused by the lack of a cache invalidation mechanism; for example, a particular data was updated in the database, but users were seeing stale data. So, as a thumb rule, if you are caching a few important functions, create an invalidation interface early on to avoid any production incident.

* Never cache any API or function that deals with sensitive data. For example, never cache user authentication tokens. Caching authentication tokens should not be done because we want to perform authentication each time.

We have learned about caching and how we can implement caching in Django, DRF, and using `django-cacheops`. In the following section, we will learn about the concept of throttling and how DRF provides us with the interface to implement throttling in APIs built in Django. Throttling uses the same default caching infrastructure we mention in the Django setting configuration.

# Throttling with Django

Throttling is the process of limiting the number of API requests that can be made by a user in a given time frame. Django does not provide any throttling feature out of the box, but DRF does. In this section, we will learn about how we can incorporate throttling into using DRF.

Different systems have different use cases for throttling. For a SaaS product, throttling is needed to limit the number of API calls per customer per minute depending upon their paid plan. An example is Shopify backend APIs. A standard product company doesn’t want its APIs to be abused by any outside customer, so it would throttle any request beyond a particular threshold.

Important note

We should understand that in this section we are implementing throttling in the application layer. This is generally not a recommended approach to protecting your service from brute force attacks since application-level throttling might still increase the request queue for your application and degrade the overall system performance. We should always implement throttling using a `web application firewall` (`WAF`). This will protect your system from queuing up requests when there is a large brute-force attack. Do note that when you are building a new system and not expecting high traffic, you can postpone the incorporation of WAF in your system for a later point in time. Application-level throttling provided by DRF should be used to incorporate business tier implementations.

DRF uses Django’s default caching framework as a backend. Before setting up throttling, we should set up caching in Django; we do not have to do anything extra apart from the basic Django Redis setup we learned in the previous section. Once we have Redis set up with Django backend caching, we need to configure the throttling classes. It is recommended not to use blanket throttling via `DEFAULT_THROTTLE_CLASSES` configuration for all the APIs using `setting.py` because different APIs would have different throughputs. It is better to create an individual throttle limit after assessing traffic patterns for each endpoint independently. We need to define the default throttle rate in the `settings.py` file. The `'anon'` key represents how many *non-logged-in requests* are allowed and the `'user'` key represents the configuration for the *logged-in requests*:

```
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_RATES': {
        'anon': '100/day',
        'user': '1000/day'
        'scope': '10000/day'
    }
}
```

DRF provides three different out-of-the-box throttling classes that can help us implement throttling:

* `AnonRateThrottle`: Implement throttling for non-logged-in user requests. By default, the throttling rate would be taken as the value passed in the `'anon'` default throttle rate config. Internally, DRF would use the `X-Forwarded-For` header to identify the IP address and determine the different sources of the request. Hence, it is very important to configure the application to forward the IP address of the client in the given header.
