
Developing an application is the first step of product development. Users will use a product only when there is enough trust in the product. The first step toward establishing trust in a product is making the product stable. To achieve stability in our application, we need to have a good monitoring system and reduce errors and downtime. We shall learn how to use different tools to monitor our Django application.

In this chapter, we shall cover the following topics:

* Integrating error monitoring tools into a Django application
* Integrating uptime monitoring tools into a Django application
* Integrating APM tools into a Django application
* Integrating messaging tools into the development process
* Handling production incidents better
* Using blameless RCA for incidents

# Technical requirements

In this chapter, we shall focus on integrating different third-party tools into our Django application. We expect you to be well-versed in the concepts discussed in the previous chapters. You should have basic knowledge of exception/error monitoring and `Application Performance Monitoring` `APM` tools and how they can be used to improve the stability and performance of applications. You are also expected to know about the basic concepts of uptime monitoring and should be using messaging tools such as `Slack`/`Teams`.

Here is the GitHub repository that has all the code and instructions for this chapter: [https://github.com/PacktPublishing/Django-in-Production/tree/main/Chapter14](https://github.com/PacktPublishing/Django-in-Production/tree/main/Chapter14)

# Integrating error monitoring tools

When we develop an application, developers will try to handle all the corner cases and write as much error-free code as possible. But, somehow, a few corner cases will be missed. A few errors will always slip by, and users will see occasional errors while using the service. These application errors are occasional, but it’s important to address them to have a stable application. While working on a local development setup, these errors can be easily detected in the terminal. But when we move to production, it becomes difficult to detect these errors. A lot of beginners still use logs to detect raised exceptions. Error monitoring tools are lifelines to detect any production exceptions raised.

Tools such as `Sentry`, `Rollbar`, `BugSnag`, and so on are error/exception monitoring tools that help us track and fix exceptions raised in production. In this chapter, we shall use `Rollbar` ([https://rollbar.com/](https://rollbar.com/)) and integrate it into our Django project.

## Integrating Rollbar into a Django project

To integrate `Rollbar` into our Django project, we will follow these steps:

1. Create an account in `Rollbar` ([https://rollbar.com/](https://rollbar.com/)) and use the wizard to create a new project. In *Figure 14**.1*, we are creating a new project by giving the project name `DjangoDemoApp`:

![Figure 14.1: Wizard to create a new Django integration for Rollbar](image/Figure_14.1_B18867.jpg)

Figure 14.1: Wizard to create a new Django integration for Rollbar

1. Once we click the `Create Project` button, we will then move on to the next step, where we need to choose the framework. In *Figure 14**.2*, we are selecting the `Django` framework:

![Figure 14.2: Select the Django framework in the wizard](image/Figure_14.2_B18867.jpg)

Figure 14.2: Select the Django framework in the wizard

1. Once we click the `Continue` button, `Rollbar` gives us a set of instructions to follow, as mentioned in the next steps.

2. Install the `rollbar` SDK:

```
pip install rollbar
```

3. Configure the `rollbar` SDK for the Django project. Go to the `settings.py` file and add the `Rollbar` middleware to `MIDDLEWARE`:

```
MIDDLEWARE = [
    # OTHER MIDDLEWARES 'rollbar.contrib.django.middleware.RollbarNotifierMiddleware',
]
```

4. Add the `Rollbar` access token in the `settings.py` file:

```
ROLLBAR = {
    'access_token': '<token>',
    'environment': 'dev' if DEBUG else 'prod',
    'root': BASE_DIR
}
```

We have integrated `Rollbar` into the Django application. To test the Django `Rollbar` integration, we need to intentionally create an exception error in our code. Add the following code in the Django `urls.py` file:

```
from django.http import HttpResponse
def rollbar_test_view(request):
    a = None
    a.hello() # This would raise an exception
    return HttpResponse("Hello World!")
urlpatterns = [
    # old url config
    path('rollbar-test/', rollbar_test_view),
]
```

Now, when we open the URL in our browser, `http://127.0.0.1:8000/rollbar-test/` is mapped to the `index` view where we have introduced the error. We will see an exception in our browser and also see our first exception captured on the `Rollbar` dashboard. *Figure 14**.3* shows the error capture in the `Rollbar` dashboard:

![Figure 14.3: First error captured in Rollbar](image/Figure_14.3_B18867.jpg)

Figure 14.3: First error captured in Rollbar

Next, we will integrate `Rollbar` with `Slack`.

## Integrating Rollbar with Slack

`Rollbar` has official documentation that mentions all the steps needed to integrate it into `Slack`. Follow the official guide at [https://docs.rollbar.com/docs/slack](https://docs.rollbar.com/docs/slack).

Important note

We are not adding the integration steps in the current chapter because third-party tools are generally updated frequently.

Once `Slack` is integrated into `Rollbar`, whenever a new error is captured, we will see the error on the configured `Slack` channel. *Figure 14**.4* shows an example error message posted by `Rollbar`:

![Figure 14.4: Slack notification for error captured in Rollbar](image/Figure_14.4_B18867.jpg)

Figure 14.4: Slack notification for error captured in Rollbar

Now, we have configured `Rollbar` to capture all the errors happening in production and get notified on our `Slack` channel whenever a new exception is raised in production. The advantage of using `Rollbar` and similar tools is that it can capture the input that caused the exception to happen. This way, developers have more data on what caused errors and can fix issues faster rather than doing guesswork about the reason for the exception in production.

## Best practices while working with error monitoring tools

As the application grows, the number of errors also might increase. Teams must follow best practices with error monitoring tools so that they don’t get overwhelmed. Let us look into a few good practices while working with error monitoring tools:

* `Never silence errors forever`: It might be possible that the team is so occupied with new feature development that a particular customer-facing exception is taking a backseat. The team should always come back to an error and fix it when the workload has reduced rather than marking the issue as muted, which will make the error silent forever unless someone visits it.

* `Dedicate time to triage the dashboard`: Have a dedicated person in the team who will spend time triaging and assigning issues from `Rollbar`. This role can be on a rotation basis every month.

* `Connect source code`: Connecting source code to error monitoring systems can help in getting better stack trace and deployment information.

* `Use proper notification rules`: Notification rules are important for any monitoring tool. We don’t want to lose out on important alerts, nor do we want to make too much noise. Each team/company should discuss internally creating a standard notification cadence.

Using `Sentry`
