+   - `XFrameOptionsMiddleware` protects against X-Frame-Options header clickjacking attacks

The middleware modules are loaded in the order that they appear in the MIDDLEWARE list. This makes sense because we want to call the middleware that deals with initial security issues first so that dangerous requests are rejected before further processing occurs. Django also comes with several other middleware modules that perform important functions, such as using gzip file compression, redirect configuration, and web cache configuration.

This chapter is devoted to discussing two important aspects of stateful application development that are implemented as middleware components – `SessionMiddleware` and `AuthenticationMiddleware`.

The `process_request` method of `SessionMiddleware` adds a session object as an attribute of the `request` object. The `process_request` method of `AuthenticationMiddleware` adds a user object as an attribute of the `request` object.

It is possible to write a Django project without these layers of the middleware stack if a project does not require user authentication or a means of preserving the state of individual interactions. However, most of the default middleware plays an important role in application security. If you don’t have a good reason for changing the middleware components, it is best to maintain these initial settings. The Admin app requires `SessionMiddleware`, `AuthenticationMiddleware`, and `MessageMiddleware` to run, and the Django server will throw errors such as these if the Admin app is installed without them:

```
django.core.management.base.SystemCheckError: SystemCheckError: System check identified some issues:

ERRORS:

?: (admin.E408) 'django.contrib.auth.middleware.AuthenticationMiddleware' must be in MIDDLEWARE in order to use the admin application.
?: (admin.E409) 'django.contrib.messages.middleware.MessageMiddleware' must be in MIDDLEWARE in order to use the admin application.
?: (admin.E410) 'django.contrib.sessions.middleware.SessionMiddleware' must be in MIDDLEWARE in order to use the admin application.
```

Now that we know about the middleware modules, let’s look at one way we can enable authentication in our project using the authentication app’s views and templates.