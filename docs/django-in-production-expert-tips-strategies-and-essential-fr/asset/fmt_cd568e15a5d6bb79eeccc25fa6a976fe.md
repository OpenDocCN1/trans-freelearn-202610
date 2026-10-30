
Django custom permissions enable us to utilize the existing Django permissions framework to have custom RBAC support for our Django application.

## Using Django permissions and groups for an API

Django groups and permissions can be used for an API by writing custom code. DRF also supports a custom `Permission` class that can use Django permissions and groups under the hood:

```
def check_permission(user, group_name):
    return user.groups.filter(name=group_name).exists()
@api_view(['POST'])
def blog_view(request)
    if not check_permission(request.user, 'can_view_blog')
        return Response(status=403)
    # perform operation
```

In the preceding code example, we are checking if the user making the request is eligible to comment or not. If the `can_view_blog` group is attached to the user, then they are eligible to comment; otherwise, they are not eligible to comment, and we will respond with a `403` error.

Similarly, we can write custom business logic in our code to support authorization in our DRF API. We can also write a custom DRF permission class to perform such operations.

Read more

For more details, check the official documentation on how to create a custom permission class in DRF: [https://www.django-rest-framework.org/api-guide/permissions/#custom-permissions](https://www.django-rest-framework.org/api-guide/permissions/#custom-permissions%20)

## Caveats of using permissions

Though Django groups and permissions are one of the most powerful RBAC systems I have come across, I have also seen a lot of projects becoming nightmares due to the overuse of the Django permissions framework. In this section, we will try to learn all the good practices that developers and teams can adapt to use Django groups and permissions to their advantage and in the right way:

*   **Avoid attaching permissions directly to auth user objects**: When we start using Django permissions, it might be tempting to directly attach permissions to django auth users, but this temptation soon leads to a nightmare. As we start using more Django permissions, it becomes tricky to keep track of which user has access to which part of the Django application. To solve this problem, always use Django groups to attach permissions to users.
*   **Avoid creating superusers**: We should avoid creating superusers for our project because they have unrestricted access to perform any operation using Django Admin.
*   **Avoid using too many Django custom permissions**: Django permissions are attached to a model, and a lot of times, developers tend to unnecessarily attach custom Django permissions to models when they don’t need any model relation. Try to use Django groups for this purpose.

In the following code snippet, we are checking if a user is attached to a particular group for authorization:

```
if user.groups.filter(name='custom_group').exists():
    # perform operation
```

`custom_group` might not have any model permission attached to it, but a basic Django group can be used to check if the user belongs to a particular group to operate.

So far, we learned about the Django permissions framework and how we can use it to create an RBAC system in our Django application. Now, we will look into how DRF supports token-based authentication and how we can integrate it into our Django project.

# Using DRF token-based authentication

Modern web applications are no longer limited to just browsers. In fact, *more than 55%* of internet traffic comes from mobile (as per this *Cloudflare* report: [https://radar.cloudflare.com/traffic?range=28d](https://radar.cloudflare.com/traffic?range=28d)). Django was first built more than 18 years ago when browsers were more prevalent, but in today’s world, there are more mobile devices and applications than browsers, which require a different type of authentication. DRF already provides multiple types of authentications out of the box:

*   Basic authentication
*   Session-based authentication
*   Token-based authentication
*   Remote user authentication

Token-based authentication is the most popular and widely used form of authentication in today’s world. In this section, we will learn how to enable and use token-based authentication in our Django project, the advantages of using token-based authentication, and another custom type of token-based authentication.

## Integrating token-based authentication into DRF

DRF token-based authentication uses a simple token-based HTTP authentication schema. DRF creates a new model that saves the issued token for a given user and has a one-to-one relationship with the `User` table. Let us check step by step how DRF implements the token-based authentication workflow:

1.  The user makes a login request that contains a username and password.
2.  Django verifies the username and password from the `Auth User` table.
3.  Once the user credentials are verified, DRF creates a new token for the given user and then sends the token to the end user as a response.

All subsequent requests made by the client now have the issued token attached to the HTTP header.
4.  When Django receives a request that has the auth token attached to the request, Django then validates the token against the saved token from the database and attaches the `Auth User` object to the `request.user` property. This hydrates the user details to the `request` object, and the token can be used to identify which user has made the request. If the token is mismatched, then DRF will send a `401` error to the client.

In this section, we learned on a high level how DRF token-based authentication works. Now, let us look into the integration of token-based authentication into Django using DRF.

## Adding DRF token-based authentication to a Django project

In DRF, authentication is checked as the first step before the execution of the view. It is important to understand that, unlike Django session-based authentication, DRF authentication is *not implemented via middleware*. Instead, DRF checks for authentication when the associated view execution starts. DRF takes this approach primarily to make sure DRF is a plug-and-play system that can easily co-exist with other authentication mechanisms used by other views. For example, Django Admin Views uses the native Django session-based authentication.

Just as with Django, DRF also attaches the `Auth User` object to the `request.user` attribute after successful authentication of the request. If the request does not have any authentication attribute, then it attaches an `Anonymous` user to `request.user`. Let us now learn the steps to integrate a DRF token into our Django project:

1.  The DRF package has an internal app that creates a `Token` table in our database. We need to add `rest_framework.authtoken` to the installed apps of our Django settings to attach the `authtoken` app to our project:

```
INSTALLED_APPS = [
    ...
    'rest_framework.authtoken'
]
```

2.  Run the `python manage.py migrate` Django migration command to create the `Token` table in our database.
3.  Add the default authentication class to the configuration settings.

DRF evaluates the authentication before the execution of the view starts, so we can define the authentication class for each view. But this could cause security gaps if the developer forgets to add authentication. Hence, it is advisable to attach the default authentication class. If we need to change the authentication for a particular view, then we can always override the authentication in the *view* definition.

Just attaching authentication to a view is not enough; we also need to tell DRF what the permission type for the given view is. By default, we should add `IsAuthenticated` to all our views so that all the views are always accessible by the authenticated users unless explicitly mentioned.
