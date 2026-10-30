
### `UserRateThrottle`
Implement throttling for logged-in user requests. By default, the throttling rate would be taken as the value passed in the `'user'` default throttle rate config. This puts a global rate limit on the total number of APIs called by the end user; hence, one should be cautious while attaching `UserRateThrottling`. It would use the user ID to generate the unique throttle key. If not found, it would fall back to the IP address. The default scope `'user'` key can be overridden by assigning the new scope value to the extended class.

### `ScopedRateThrottle`
Implement throttling to specific APIs clubbed together. For example, if we want to implement throttling for two APIs combined, we should use `ScopedRateThrottle` with a defined scope.

DRF APIViews has `throttle_classes` as a property that takes a list of all the throttle classes. If we have to attach an `AnonRateThrottle` to `BlogApiView`, we can do it by adding the throttle class in the following code snippet:

```
from rest_framework.views import APIView
from rest_framework.throttling import AnonRateThrottle
class BlogApiView(APIView):
    throttle_classes = [AnonRateThrottle]
    ...
```

Similarly, we can attach the `UserRateThrottle` class to `APIView`. For `ScopedRateThrottle`, we have to pass an additional `throttle_scope` property. For example, `BlogApiView` and `BlogDetailApiView` both have the same `throttle_scope` assigned, which means the total rate limit is going to be distributed among both the APIs while `Blog2ApiView` shall have an independent limit:

```
from rest_framework.views import APIView
from rest_framework.throttling import AnonRateThrottle
class BlogApiView(APIView):
    throttle_classes = [ScopedRateThrottle]
    throttle_scope = 'blog_limit'
    ...
class BlogDetailApiView(APIView):
    throttle_classes = [ScopedRateThrottle]
    throttle_scope = 'blog_limit'
    ...
class Blog2ApiView(APIView):
    throttle_classes = [ScopedRateThrottle]
    throttle_scope = 'blog_2_limit'
    ...
```

We can set the throttling limit in the `settings.py` file, as shown in the following snippet:

```
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_RATES': {
        'blog_limit': '1000/day',
        'blog_2_limit': '100/day'
    }
}
```

All the throttle classes are expandable and one can easily override them. It is also possible to pass multiple throttle classes to one APIView, so we can add multiple rules for a single API view. If there are multiple throttle classes attached to a single APIView, all the classes need to pass before executing the main body of the view or else `exceptions.Throttled` is raised.

For more details on how we can create custom throttle classes and extend existing classes, you can look into the official documentation: [https://www.django-rest-framework.org/api-guide/throttling/](https://www.django-rest-framework.org/api-guide/throttling/)

## Best practices for throttling in production

Now let us look into a few best practices one should follow while implementing throttling in production:

* Always prefer to implement throttling using WAFs or other components. Application-level throttling should be implemented only if the application is small and the added complexity of using other methods outweighs the advantage.
* Avoid adding blanket application throttling on DRF settings. It can backfire at any given time on production.
* Add monitoring and logging for all the throttled requests. This helps you monitor any unexpected throttling behavior or increase the throttling limit from time to time as per the usage pattern.
* Always remember to pass the correct IP address of the client to `X-Forwarded-For` so that the throttling backend performs the expected behavior. As we have multiple components between the client and the Django server, it is common to use the wrong IP address. Always double-check that the client IP is propagated properly to the Django server.
* The default `UserRateThrottle` always uses a global rate limit. It is recommended to use a custom scope by extending the `UserRateThrottle` class.

We shall now learn how we can implement logging in Django and how logging can help us function better in production.

# Logging with Django

Logging can be considered a record of every data in programming. Django uses Python’s built-in logging module ([https://docs.python.org/3/library/logging.html](https://docs.python.org/3/library/logging.html)) to capture logs. In Django, logging is configured as part of the general Django `django.setup()` function, so it’s always available unless explicitly disabled. Python’s logging module provides extensive options to set up and configure logging; hence, all those configurations are also available when we use logging in Django. In this section, we shall learn how we can set up logging in Django and the best practices to use logging in Django projects.

Important note

Logging in Python is an extensive topic, and the configuration of logging can be tricky at times. In this section, we will not go into too much detail about how one can use all the different options provided by Python. Rather, we shall check the standard configuration, which I have used in multiple projects and have found to be super helpful.

If you want to deep-dive into detailed configurations, please go through the Python logging module official documentation and Django logging documentation: [https://docs.djangoproject.com/en/stable/topics/logging/](https://docs.djangoproject.com/en/stable/topics/logging/)

## Setting up logging

By default, Django logs are sent to the console. Whenever we add a print statement to our Django code, the output is printed out in the console. While working on the local setup, this is the first approach taken by developers to log data and develop the application. However, this approach doesn’t work in production since developers don’t have access to the console. This is where logging output to files is helpful.

Let us see how we can configure Django to set up the logging infrastructure. Please note that we are going to work with just one standard configuration that mostly solves all the standard use cases of logging. To enable logging in Django, we need to add a snippet to the Django `settings.py` file, as seen in the following code snippet:

```
LOGGING = {
    "version": 1,
    "disable_existing_loggers": False,
    "formatters": {"verbose": {"format": "%(asctime)s %(process)d %(thread)d %(message)s"}},
    "loggers": {
        "django_default": {
            "handlers": ["django_file"],
            "level": "INFO",
        },
    },
    "handlers": {
        "django_file": {
            "class": "logging.handlers.RotatingFileHandler",
            "filename": "path/to/django_logs.log",
            "maxBytes": 1024 * 1024 * 10,  # 10MB
            "backupCount": 10,
            "formatter": "verbose"
        },
    },
}
```

Let me quickly explain what each line in the `LOGGING` object signifies:

* `version`: This means the configuration version used is `logging.config.dictConfig` *version `1`* format.
* `disable_existing_loggers`: Django provides a few default logging configurations. The `disable_existing_loggers` flag is used to enable/disable the default logging configuration setting. In general, keep this flag false. This means the default loggers configured by Django are still active.
* `formatters`: Formatters are key–value pairs, with each key representing a formatter we can use for logging messages. We have configured a `verbose` formatter to log additional information such as timestamps, process IDs, etc.
* `loggers`: Loggers are namespaces used when we create logs. One application can have multiple loggers. In our application, we are naming the custom logger `django_default`, and whenever we have to use this logging configuration, we would use this name:
