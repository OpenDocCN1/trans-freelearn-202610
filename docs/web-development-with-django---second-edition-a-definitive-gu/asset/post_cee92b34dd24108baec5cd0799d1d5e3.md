
Aside from giving a Django website a modern look, we also need a way to give our site standards of authentication that are convenient and trusted by users.

## django-allauth

When browsing websites, you have probably seen buttons that allow you to log in using another website’s credentials – for example, using your GitHub login:

![](img/4840a43efd335abd10b93708225692d6_689_0.png)

Figure 15.34 – The sign-in form with options to log in with Google or GitHub

Before we explain the process, let us introduce the terminology we will be using:

- **Requesting site**: The site the user is trying to log in to.
- **Authentication provider**: The third-party provider that the user is authenticating to (for example, Google or GitHub).
- **Authentication application:** This is something the creators of the requesting site set up on the authentication provider. It determines what permissions the requesting site will have with the authentication provider. For example, the requesting application can get access to your GitHub username but won't have permission to write to your repositories. The user can stop the requesting site from accessing your information from the authentication provider by disabling access to the authentication application.

The process is generally the same regardless of which third-party sign-in option you choose. First, you will be redirected to the authentication provider site and asked to authorize the authentication application to access your account (Figure 15.35):

![](img/4840a43efd335abd10b93708225692d6_690_0.png)

Figure 15.35 – The authentication provider authorization screen

After you authorize the authentication application, the authentication provider will redirect you back to the requesting site. The URL that you are redirected to will contain a secret token that the requesting site can use to request your user information in the backend. This allows the requesting site to verify who you are by communicating directly with the authentication provider. After validating your identity using a token, the requesting site can redirect you to your content. This flow is illustrated in a sequence diagram in Figure 15.36:

![](img/4840a43efd335abd10b93708225692d6_691_0.png)

Figure 15.36 – Third-party authentication flow

Now that we have introduced authenticating using a third-party service, we can discuss `django-allauth`. This is an app that easily plugs your Django application into a third-party authentication service, including Google, GitHub, Facebook, and Twitter. In fact, at the time of writing, `django-allauth` supports over 75 authentication providers.

The first time a user authenticates to your site, `django-allauth` will create a standard Django User instance for you. It also knows how to parse the callback/redirect URL that the authentication provider loads after the end user authorizes the authentication application.

`django-allauth` adds three models to your application:

- `SocialApplication`: This stores the information used to identify your authentication application. The information you enter will depend on the provider, who will give you a *client ID*, *secret key*, and (optionally) a *key*. Note that these are the names that `django-allauth` uses for these values and they will differ based on the provider. We will give some examples of these values later in this section. `SocialApplication` is the only one of the `django-allauth` models that you will create yourself; `django-allauth` creates the others automatically when a user authenticates.
- `SocialApplicationToken`: This contains the values needed to identify a Django user to the authentication provider. It contains a *token* and (optionally) a *token secret*. It also contains a reference to the `SocialApplication` model that created it and the `SocialAccount` model to which it applies.
- `SocialAccount`: This links a Django user to the provider (for example, Google or GitHub) and stores extra information that the provider may have given.

Since there are so many authentication providers, we will not cover how to set them all up, but we will give a short instruction on setup and how to map the auth tokens from the providers to the right fields in `SocialApplication`. We will do this for the two auth providers we have been mentioning throughout the chapter – Google and GitHub.

## django-allauth installation and setup

Like the other apps in this chapter, `django-allauth` is installed with pip3:

```
```bash
pip3 install django-allauth
```
```

> **Note**
> 
> For Windows, you can use `pip` instead of `pip3` in the preceding command.

We then need a few settings changes. `django-allauth` requires the `django.contrib.sites` app to run, so it needs to be added to `INSTALLED_APPS`. Then, a new setting needs to be added to define a `SITE_ID` value for our site. We can just set this to 1 in our `settings.py` file:

```
```python
INSTALLED_APPS = [
    "bookr_admin.apps.BookrAdminConfig",
    "django.contrib.sites",  # this entry added
    "django.contrib.auth",
    # The rest of the values are truncated
]

SITE_ID = 1
```
```

> **Note**
> 
> It is possible to have a single Django project hosted on multiple hostnames and have it behave differently on each, but also have content shared across all the sites. We don’t need to use the `SITE_ID` anywhere else in our project, but one instance must be set here. You can read more about the `SITE_ID` settings at https://docs.djangoproject.com/en/3.0/ref/contrib/sites/.

We also need to add `allauth` and `allauth.socialaccount` to `INSTALLED_APPS`:

```
```python
INSTALLED_APPS = [
    # The rest of the values are truncated
    "allauth",
    "allauth.socialaccount",
]
```
```

Then, each provider we want to support must also be added to the list of `INSTALLED_APPS` – for example, consider the following snippet:

```
```python
INSTALLED_APPS = [
    # The rest of the values are truncated
    "allauth.socialaccount.providers.github",
    "allauth.socialaccount.providers.google",
]
```
```

After all of this is done, we need to burn the migrate management command to create the django-allauth models:

```
python3 manage.py migrate
```

Once this is done, new social applications can be added through the Django admin interface (Figure 15.37):

![](img/4840a43efd335abd10b93708225692d6_694_0.png)

Figure 15.37 – Adding a social application

To add a social application, select a provider (this list will only show those in the INSTALLED_APPS list), enter a name (it can just be the same as the provider), and enter the client ID from the provider’s website (we will go into detail on this soon). You may also need a secret key and a key. Select the site the secret key and key should apply to. (If you only have one Site instance, then its name does not matter – just select it. The site name can be updated in the Sites section of Django admin. You can also add more sites there.)

We will now look at the tokens used by our three example providers.

## GitHub auth setup

A new GitHub application can be set up under your GitHub profile. During development, your callback URL for the application should be set to http://127.0.0.1:8000/accounts/github/login/callback/ and updated with the real hostname when you deploy to production. After creating the app, it will provide a client ID and client secret. These are your client ID and secret key respectively in django-allauth.

## Google auth setup

The creation of a Google application is done through your Google Developers console. The authorized redirect URI should be set to http://127.0.0.1:8000/accounts/google/login/callback/ during development and updated after production deployment. The app's Client ID is also Client id in django-allauth, and the app's Client secret is Secret key.

## Initiating authentication with django-allauth

To initiate authentication through a third-party provider, you first need to add the django-allauth URLs in your URL maps. Somewhere inside your urlpatterns is one of your urls.py files, including allauth.urls:

```
urlpatterns = [path('allauth', include('allauth.urls')),]
```

You will then be able to initiate a login using URLs such as http://127.0.0.1:8000/allauth/github/login/?process=login, http://127.0.0.1:8000/allauth/google/login/?process=login, and so on. django-allauth will handle all the redirects for you and then create/authenticate the Django user when they return to the site. You can have buttons on your login page with text such as Login with GitHub or Login with Google that links to these URLs.

## Other django-allauth features

Other than authentication with third-party providers, django-allauth can also add some useful features that Django does not have built in. For example, you can configure it to require an email address for a user, and have the user verify their email address by clicking a confirmation link they receive before they log in. django-allauth can also handle generating a URL for a password reset that is emailed to the user. You can find the documentation for django-allauth that explains these features, and more, at https://django-allauth.readthedocs.io/en/stable/overview.html.

Now that we have covered the first four third-party apps in depth and given a brief overview of django-allauth, you can undertake the activity for this chapter. In this activity, we will refactor the ModelForm instances we are using to use the CrispyFormHelper class.
