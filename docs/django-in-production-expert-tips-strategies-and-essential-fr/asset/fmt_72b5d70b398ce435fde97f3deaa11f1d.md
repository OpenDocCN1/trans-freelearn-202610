
Read more here on how we can use `factory_boy` to mute signals and have faster test execution: [https://factoryboy.readthedocs.io/en/stable/orms.html#factory.django.mute_signals](https://factoryboy.readthedocs.io/en/stable/orms.html#factory.django.mute_signals).

We have learned how to write test cases and how to test different features of Django. Now, we shall learn about one of the advanced features of testing, Django runners. Django runners are advanced features that can be used by developers to set up a test database or customize test.

## Using Django runners

When we execute the Django management command to run tests, `python manage.py test`, Django internally finds the configuration to run tests. This configuration is present in the Django test runner, the `DiscoverRunner` class. Just like any other Django interface, `DiscoverRunner` can also be extended as per requirements.

Let us extend the default `DiscoverRunner` in this section, which should help with any custom use case developers have. Let us imagine that we want to add data to a database before we start the execution of any of the test cases, and then we can use `DiscoverRunner` to load our data. We can create a new class – for example, a `FillData` class, which has a `setup_databases` method that populates the data needed for the test cases. Then, create a `CustomRunner` class that inherits both classes, `FillData` and Django’s default runner, `DiscoverRunner`:

```
from django.test.runner import DiscoverRunner
class FillData:
    def setup_databases(self, *args, **kwargs):
        temp = super(FillData, self).setup_databases(*args, **kwargs)
        print("### Populating Test Cases Database ###")
        # Create any data
        print("### Database populated ############")
        return temp
class CustomRunner(FillData, DiscoverRunner):
    pass
```

Once we create the `CustomRunner` class, we need to save it in a given file – for example, `myblog/backend/common/custom_runner.py`. Now, we need to tell Django to use our `CustomRunner` every time for test case configuration. To do this, we need to set the `TEST_RUNNER` flag in our settings file:

```
TEST_RUNNER = 'common.custom_runner.CustomRunner'
```

Now, every time we run the test management command, Django will pick the test configuration from the `CustomRunner` class we defined. To verify the integration, we can run `python manage.py test`, and we will see the output, which has the `print` statement we added to our `CustomRunner`.

Note

The `CustomRunner` example we shared is an advanced use case, and we expect you to have a good understanding of MRO and other advanced concepts of Python multiple inheritances.

Here are a few custom use cases that developers can configure for Django custom test runners:

*   Configuring `parallel` to Django can use to run tests in parallel
*   Configuring the test name pattern using `test_name_patterns` to run tests efficiently
*   Setting up `tags` and `exclude_tags` to group different tests together
*   `failfast` stopping the execution of the test cases immediately when any one of the test cases fails

Django provides a lot of flexibility to create custom runners; however, not all the customizations are generally used by developers. Creating a custom runner is beyond the scope of this book, and we leave it to you to explore the official documentation to get more understanding. Read more on the topic in the official Django documentation: [https://docs.djangoproject.com/en/stable/topics/testing/advanced/#defining-a-test-runner](https://docs.djangoproject.com/en/stable/topics/testing/advanced/#defining-a-test-runner).

Now that we have covered all the use cases of writing tests in Django, let us move on to the next section, where we will cover best practices to write test cases.

# Learning best practices to write tests

So far, we have explored all the advantages of writing test cases and also how we can write them for different Django components. In this section, we shall learn about best practices to write test cases.

## Using unit tests more often

Writing test cases is always tricky, and it is very difficult to write test cases for all scenarios. As applications become complex, developers may not be able to write test cases to cover all scenarios, so the best approach to tackle this problem is by writing maximum unit tests. Whenever we are writing new business logic, we generally add small functions/methods that will have new/updated business logic, so writing unit tests for such functions helps to add more coverage. If we have written modular code, then it is easy to write unit tests for our application, and this is something that developers need to understand – *writing test cases is difficult only when the application code is not modular and `structured badly`*.

Now the question comes, when and how much should we write *integration tests*? The answer is, *it depends*. With more complex business applications, it is better to cover 90% of success scenarios and 30% of failure scenarios by integration tests. For non-critical systems, having fewer integration tests also works, but we should have a couple of success-and-failure-scenario integration tests.

## Avoiding time bomb test failures

Time bomb tests are one of the most rookie mistakes developers make. When writing test cases, developers might hardcode the current datetime to the expected datetime or write some logic that would not consider the timezone. This would fail the test cases during midnight or specific times of the day/night.

There is a popular library, `Freezegun` ([https://github.com/spulec/freezegun](https://github.com/spulec/freezegun)), that helps us mock a datetime efficiently to avoid any timebound test case failure. Let us look at an example where we don’t want users to spam our blogging application, so you want to limit users to posting 10 blogs per day. We need to write a test case for such a scenario where we can mock any function trying to fetch the datetime. Freezegun can help us mock any datetime automatically within the decorator or context manager scope. In the following code, we mock the datetime called `today` and then change it to the next day. This helps us to write test cases with a cleaner approach:

```
from django.utils import timezone
from freezegun import freeze_time
from blog.factoryboy import BlogFactory, AuthorFactory
from blog import public
class BlogTests7(APITestCase):
    def test_blog_time_block(self):
        today = timezone.now().date()
        tomo = timezone.now().date() + timezone.timedelta(days=1)
        with freeze_time(today) as frozentime:
            # Post 10 blogs
            author = AuthorFactory()
            create_10_blogs = BlogFactory.create_batch(10, author=author)
            # Check if the user can post more blogs today
            user_can_post = public.check_if_allowed_to_publish_blog(author)
            # Validate that the user cannot post a blog today.
            self.assertFalse(user_can_post)
            # Move the date to the next day
            frozentime.move_to(tomo)
            # Validate that the user cannot post a blog tomorrow.
            result = public.check_if_allowed_to_publish_blog(author)
            self.assertTrue(result)
```

Mocking a datetime is one of the most tricky scenarios, but `freezetime` makes it much simpler.

## Avoiding brittle tests
